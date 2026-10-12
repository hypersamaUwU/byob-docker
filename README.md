![Banner](https://github.com/malwaredllc/byob/blob/master/byob/static/byob_logo_black.svg)

[![license](https://img.shields.io/badge/license-GPL-brightgreen.svg)](https://github.com/malwaredllc/byob/blob/master/LICENSE)
[![version](https://img.shields.io/badge/version-2.0-blue.svg)](https://github.com/malwaredllc/byob)
[![Coverage Status](https://coveralls.io/repos/github/malwaredllc/byob/badge.svg)](https://coveralls.io/github/malwaredllc/byob)
<img alt="Discord" src="https://img.shields.io/discord/709150520446550097"/>
[![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=BYOB%20(Post-Exploitation%20Framework)&url=https://github.com/malwaredllc/byob&via=malwaredllc&hashtags=byob,python,security,github)


**Questions?** Check out the [docs](https://github.com/malwaredllc/byob/wiki) or join our [Discord support server](https://discord.gg/8FsSrw7)

__Disclaimer__: This project should be used for authorized testing or educational purposes only.

BYOB is an open-source post-exploitation framework for students, researchers and developers. It includes features such as:
- Command & control server with intuitive user-interface
- Custom payload generator for multiple platforms
- 12 post-exploitation modules

It is designed to allow students and developers to easily implement their own code and add cool new
features *without* having to write a C2 server or Remote Administration Tool from scratch.

This project has 2 main parts: the **original console-based application** (`/byob`) and the **web GUI** (`/web-gui`).

# Web GUI

## Dashboard
A control panel for your C2 server with a point-and-click interface for executing post-exploitation modules. The control panel includes an interactive map of client machines and a dashboard which allows efficient, intuitive administration of client machines.

![dashboard_preview](https://github.com/malwaredllc/byob/blob/master/web-gui/buildyourownbotnet/assets/images/previews/preview-dashboard.png)

## Payload Generator
The payload generator uses black magic involving Docker containers & Wine servers to compile executable payloads for any platform/architecture you select. These payloads spawn reverse TCP shells with communication over the network encrypted via AES-256 after generating a secure symmetric key using the [Diffie-Hellman IKE](https://tools.ietf.org/html/rfc2409).

![payloads_preview](https://github.com/malwaredllc/byob/blob/master/web-gui/buildyourownbotnet/assets/images/previews/preview-payloads2.png)

## Terminal Emulator
The web app includes an in-browser terminal emulator so you can still have direct shell access even when using the web GUI.

![terminal_preview](https://github.com/malwaredllc/byob/blob/master/web-gui/buildyourownbotnet/assets/images/previews/preview-shell.png)

# Console Application

## Client
[![client](https://img.shields.io/badge/byob-client-blue.svg)](https://github.com/malwaredllc/byob/blob/master/byob/payloads.py)

*Generate fully-undetectable clients with staged payloads, remote imports, and unlimited post-exploitation modules*

1) __Remote Imports__: remotely import third-party packages from the server without writing them 
to the disk or downloading/installing them
2) __Nothing Written To The Disk__: clients never write anything to the disk - not even temporary files (zero IO
system calls are made) because remote imports allow arbitrary code to be 
dynamically loaded into memory and directly imported into the currently running 
process
3) __Zero Dependencies (Not Even Python Itself)__: client runs with just the python standard library, remotely imports any non-standard
packages/modules from the server, and can be compiled with a standalone python 
interpreter into a portable binary executable formatted for any platform/architecture,
allowing it to run on anything, even when Python itself is missing on the target host
4) __Add New Features With Just 1 Click__: any python script, module, or package you copy to the `./byob/modules/` directory
automatically becomes remotely importable & directly usable by every client while 
your command & control server is running
5) __Write Your Own Modules__: a basic module template is provided in `./byob/modules/` directory to make writing
your own modules a straight-forward, hassle-free process
6) __Run Unlimited Modules Without Bloating File Size__: use remote imports to add unlimited features without adding a single byte to the
client's file size 
7) __Fully Updatable__: each client will periodically check the server for new content available for
remote import, and will dynamically update its in-memory resources
if anything has been added/removed
8) __Platform Independent__: everything is written in Python (a platform-agnostic language) and the clients
generated can optionally be compiled into a portable executable (*Windows*) or
bundled into a standalone application (*macOS*)
9) __Bypass Firewalls__: clients connect to the command & control server via reverse TCP connections, which
will bypass most firewalls because the default filter configurations primarily
block incoming connections
10) __Counter-Measure Against Antivirus__: avoids being analyzed by antivirus by blocking processes with names of known antivirus
products from spawning
11) __Encrypt Payloads To Prevent Analysis__: the main client payload is encrypted with a random 256-bit key which exists solely
in the payload stager which is generated along with it
12) __Prevent Reverse-Engineering__: by default, clients will abort execution if a virtual machine or sandbox is detected

## Modules
[![modules](https://img.shields.io/badge/byob-modules-blue.svg)](https://github.com/malwaredllc/byob/blob/master/byob/modules)

*Post-exploitation modules that are remotely importable by clients*

1) __Persistence__ (`byob.modules.persistence`): establish persistence on the host machine using 5 different methods
2) __Packet Sniffer__ (`byob.modules.packetsniffer`): run a packet sniffer on the host network & upload .pcap file
3) __Escalate Privileges__ (`byob.modules.escalate`): attempt UAC bypass to gain unauthorized administrator privileges
4) __Port Scanner__ (`byob.modules.portscanner`): scan the local network for other online devices & open ports
5) __Keylogger__ (`byob.modules.keylogger`): logs the user’s keystrokes & the window name entered
6) __Screenshot__ (`byob.modules.screenshot`): take a screenshot of current user’s desktop
7) __Outlook__ (`byob.modules.outlook`): read/search/upload emails from the local Outlook client
8) __Process Control__ (`byob.modules.process`): list/search/kill/monitor currently running processes on the host
9) __iCloud__ (`byob.modules.icloud`): check for logged in iCloud account on macOS

## Server
[![server](https://img.shields.io/badge/byob-server-blue.svg)](https://github.com/malwaredllc/byob/blob/master/byob/server.py)

*Command & control server with persistent database and console*

1) __Console-Based User-Interface__: streamlined console interface for controlling client host machines remotely via
reverse TCP shells which provide direct terminal access to the client host machines
2) __Persistent SQLite Database__: lightweight database that stores identifying information about client host machines,
allowing reverse TCP shell sessions to persist through disconnections of arbitrary
duration and enabling long-term reconnaissance
3) __Client-Server Architecture__: all python packages/modules installed locally are automatically made available for clients 
to remotely import without writing them to the disk of the target machines, allowing clients to use modules which require
packages not installed on the target machines

## Core
[![core](https://img.shields.io/badge/byob-core-blue.svg)](https://github.com/malwaredllc/byob/blob/master/byob/core)

*Core framework modules used by the generator and the server*

1) __Utilities__ (`byob.core.util`): miscellaneous utility functions that are used by many modules
2) __Security__ (`byob.core.security`): Diffie-Hellman IKE & 3 encryption modes (AES-256-OCB, AES-256-CBC, XOR-128)
3) __Loaders__ (`byob.core.loaders`): remotely import any package/module/scripts from the server
4) __Payloads__ (`byob.core.payloads`): reverse TCP shell designed to remotely import dependencies, packages & modules
5) __Stagers__ (`byob.core.stagers`): generate unique payload stagers to prevent analysis & detection   
6) __Generators__ (`byob.core.generators`): functions which all dynamically generate code for the client generator
7) __DAO__ (`byob.core.dao`): handles interaction between command & control server and the SQLite database
8) __Handler__ (`byob.core.handler`): HTTP POST request handler for remote file uploads to the server

________________________________________________________________________________________________

### To Do

*Contributors welcome! Feel free to issue pull-requests with any new features or improvements you have come up with!*

1) __Remote Import Encryption__ - encryption for data streams of packages/modules being remotely imported (to maintain confidentiality/authenticity/integrity and prevent any remote code execution vulnerabilities arising from deserialization)
2) __Transport Types__ - add support for more transport types (HTTP/S, DNS, etc.)
3) __Bug Fixes__ - fix any bugs/issues

________________________________________________________________________________________________

## 🐳 Docker Deployment

The BYOB Web GUI is published as a Docker image to [GitHub Container Registry](https://ghcr.io/malwaredllc/byob) and rebuilt automatically on every push to `master`.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) 20.10+
- [Docker Compose](https://docs.docker.com/compose/install/) v2+ *(for the Compose method)*

---

### Method 1 — `docker run` (single command)

```bash
docker run -d \
  --name byob-web-gui \
  --restart unless-stopped \
  -p 5000:5000 \
  -p 1337:1337 \
  -p 1338:1338 \
  -p 1339:1339 \
  -v byob-data:/data \
  -e BYOB_APP_SECRET_KEY="$(python3 -c 'import secrets; print(secrets.token_hex(32))')" \
  -e SQLALCHEMY_DATABASE_URI="sqlite:////data/database.db" \
  ghcr.io/malwaredllc/byob:latest
```

Then navigate to **http://localhost:5000** and create your first user account.

> **Tip:** Replace `$(python3 -c '...')` with a fixed strong secret in production so the key persists across container restarts.

---

### Method 2 — `docker compose` (recommended, no Git clone required)

1. **Download the compose file** (no need to clone the whole repo):

```bash
curl -O https://raw.githubusercontent.com/malwaredllc/byob/master/docker-compose.yml
```

2. **(Optional but recommended)** Create a `.env` file in the same directory to configure secrets:

```bash
# .env
BYOB_SECRET_KEY=your_very_long_random_secret_key_here
BYOB_WEB_PORT=5000
BYOB_C2_PORT=1337
```

Generate a secure key with:
```bash
python3 -c "import secrets; print(secrets.token_hex(32))"
```

3. **Start the application:**

```bash
docker compose up -d
```

4. **Check that it's running:**

```bash
docker compose ps
docker compose logs -f byob
```

5. **Open the web GUI:** [http://localhost:5000](http://localhost:5000)

6. **Stop / remove:**

```bash
docker compose down          # stop containers (data is preserved)
docker compose down -v       # stop + delete all data (irreversible)
```

---

### Environment Variables

| Variable | Default | Description |
|---|---|---|
| `BYOB_APP_SECRET_KEY` | *(empty)* | Flask secret key — **set this in production!** |
| `SQLALCHEMY_DATABASE_URI` | `sqlite:////data/database.db` | Database connection string |
| `BYOB_WEB_PORT` | `5000` | Host port for the web GUI *(compose only)* |
| `BYOB_C2_PORT` | `1337` | Host port for the C2 TCP listener *(compose only)* |

---

### Port Reference

| Port | Protocol | Purpose |
|---|---|---|
| `5000` | TCP/HTTP | Flask web GUI dashboard |
| `1337` | TCP | C2 reverse shell listener (clients connect here) |
| `1338` | TCP/HTTP | BYOB modules server (auto-started by C2) |
| `1339` | TCP/HTTP | Python packages server (auto-started by C2) |

> **Firewall note:** For clients to reach your C2 server, port `1337` must be reachable from the internet. Ports `1338` and `1339` are used internally by C2 clients; expose them only if needed.

---

### Data Persistence

The SQLite database is stored in a Docker **named volume** (`byob-data`) mounted at `/data` inside the container. This means your sessions, payloads, and user accounts survive container restarts and image upgrades.

To back up your database:
```bash
docker run --rm \
  -v byob-data:/data \
  -v $(pwd):/backup \
  alpine tar czf /backup/byob-backup.tar.gz /data
```

---

### Building the Image Locally

If you want to build from source instead of pulling from the registry:

```bash
git clone https://github.com/malwaredllc/byob.git
cd byob/web-gui
docker build -t byob-web-gui:local .
docker run -d -p 5000:5000 -p 1337:1337 -v byob-data:/data byob-web-gui:local
```

