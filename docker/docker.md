# 🐳 Docker Notes

> Docker is a platform for building, shipping, and running applications inside **containers** — lightweight, portable, and isolated environments.

---

## 🏛️ The Problem Docker Solves

### Traditional Approach: One App, One Server
- Wasteful — server resources are underutilized
- Hard to scale and manage

### Virtual Machines (e.g. VMware)
- Each VM has its **own full OS** — heavy and slow to start
- Resources are **dedicated and pre-allocated** (even if unused)
- Slow boot times, large disk footprint

### Containers
- Share the **host OS kernel** — much lighter than VMs
- Start in **milliseconds**
- Contain everything the app needs: code, runtime, libraries, config

---

## 🏗️ Docker Architecture

### The Big Picture

```
Docker CLI  ←──REST API──→  Server (Daemon / dockerd)
```

Docker follows a **client-server architecture**:
- **Docker CLI** — what you type commands into
- **REST API** — the communication layer between CLI and server
- **Daemon (dockerd)** — the server that does all the actual work

---

### Image → Container (Class → Object analogy)

```
Docker Image  ──────▶  Container
     │                     │
     │                     │
  Like a Class          Like an Object
(blueprint/template)  (running instance)
```

> 💡 You can create **many containers** from a single image, just like you can create many objects from one class.

---

### Runtime Stack (Bottom-Up)

```
┌─────────────────────────────┐
│        Docker CLI           │
├─────────────────────────────┤
│     Daemon (dockerd)        │  ← Brain of Docker
├─────────────────────────────┤
│       containerd            │  ← Container lifecycle manager
├──────────┬──────────────────┤
│  shim    │  shim  │  shim   │  ← One per container (babysitter)
├──────────┼─────────────────-┤
│  runc    │  runc  │  runc   │  ← Actual container creator (OCI)
└──────────┴─────────────────-┘
```

---

## 🧩 Docker Components Explained

### 1. 🧠 Daemon → `dockerd`
The **main Docker server process**.

- Listens to Docker CLI commands (`docker run`, `docker ps`, etc.)
- Manages: images, containers, networks, volumes
- **Think of it as:** The brain of Docker

> 💡 Even if the daemon goes down, **running containers keep running** — they don't depend on the daemon to stay alive.

---

### 2. 📦 `containerd`
A **container runtime manager**, extracted from Docker as a standalone component.

- Pulls images from registries
- Manages the full container lifecycle (create, start, stop, delete)
- Handles storage and networking
- Communicates with `runc` for low-level execution
- **Think of it as:** The manager that actually handles containers

---

### 3. 👶 Shim → `containerd-shim`
A **small intermediate process** — one exists per running container.

- Keeps the container running even if `containerd` crashes
- Handles: STDIN/STDOUT, exit status reporting
- **Think of it as:** A babysitter for each container

---

### 4. ⚙️ `runc`
The **low-level OCI-compliant runtime** that does the actual container creation.

- Uses Linux **namespaces** (isolation) and **cgroups** (resource limits) to create containers
- Once the container is started, `runc` exits — its job is done
- **Think of it as:** The worker that physically builds and starts the container

---

### Full Flow Summary

```
Docker CLI
   │
   ▼ (gRPC)
dockerd (Daemon)
   │
   ▼
containerd
   │
   ▼
containerd-shim  (one per container, stays alive)
   │
   ▼
runc  (creates container, then exits)
   │
   ▼
🟢 Container is now running
```

---

### 🌐 Remote API & Security

```
Client ──── HTTP (port 2375) ──▶ Docker Server
```

- **Port 2375** — default port Docker uses for remote API communication
- **TLS (Transport Layer Security)** — Docker only allows **secured (TLS) connections** for remote access to prevent unauthorized control of the daemon

---

## 📟 Essential Docker Commands

> 💡 For any container ID argument, you can use just the **first 4–6 characters** of the ID instead of the full string.

### Running Containers

```bash
# Run a container interactively
docker run -it <image-name>

# Classic hello world test
docker run hello-world
```

- `docker` — the CLI
- `run` — pull the image (if needed) and create + start a container
- `-it` — interactive terminal (keeps STDIN open with a pseudo-TTY)

```bash
# Run a container in the background (detached mode)
docker run -d <image-name> ping <url>

# Run with port mapping: host-port:container-port
docker run -d -p 8080:<port> <image-name>

# Run a specific version (tag)
docker run ubuntu:16.04
```

---

### Listing & Inspecting Containers

```bash
docker ps                   # Show running containers
docker ps -a                # Show all containers (including stopped)
docker container ls         # Same as docker ps

docker inspect <container-id>   # Detailed JSON info about a container
docker inspect <image-name>     # Detailed JSON info about an image
docker logs <container-id>      # Show logs of a container
docker logs --since <t>s <id>   # Show logs from the last t seconds
```

---

### Starting, Stopping & Removing

```bash
docker start <container-id>     # Start a stopped container
docker stop <container-id>      # Gracefully stop a running container
docker rm <container-id>        # Remove a stopped container
docker container prune -f       # Remove ALL stopped containers (force, no prompt)
```

---

### Exec & Commit (Advanced)

```bash
# Open a shell inside a running container
docker exec -it <container-name> bash

# Save the current state of a container as a new image
docker commit -m "<message>" <container-id> <image-name>:<version>

# Run your newly committed image
docker run -it <image-name>:<version>
```

> 💡 `docker commit` is like taking a snapshot of a container's filesystem and turning it into a reusable image. Useful during development, but `Dockerfile` is preferred for reproducibility.

---

### Image Management

```bash
# Remove all images forcefully
docker rmi $(docker images -q) -f
```

- `docker images -q` — lists only image IDs
- `docker rmi` — removes images
- `-f` — force (even if containers use them)

---

## 📝 Dockerfile — Building Custom Images

A `Dockerfile` is a script of instructions that tells Docker how to build your image.

```dockerfile
# 1. Base image — the starting point (Node.js 18 on lightweight Alpine Linux)
FROM node:18-alpine

# 2. Set working directory inside the container
WORKDIR /usr/src/app

# 3. Copy dependency files FIRST (enables Docker layer caching)
#    If package.json hasn't changed, Docker skips re-running npm install
COPY package*.json ./

# 4. Install dependencies at BUILD time
RUN npm install

# 5. Copy the rest of the source code
COPY . .

# 6. Tell Docker which port the app listens on (documentation only)
EXPOSE 3000

# 7. Command that runs when the container STARTS (not at build time)
CMD ["node", "app.js"]
```

### Key Distinctions

| Instruction | When it runs | Purpose |
|-------------|-------------|---------|
| `RUN` | **Build time** | Install packages, set up the image |
| `CMD` | **Container start** | The default command to run the app |
| `EXPOSE` | Neither | Documents the port (doesn't actually publish it) |
| `WORKDIR` | Build time | Sets current directory for subsequent instructions |

> 💡 **Why copy `package*.json` before the rest of the code?** Docker builds images in layers. If your source code changes but `package.json` doesn't, Docker uses the cached `npm install` layer — saving significant build time.

### Building the Image

```bash
docker build .                        # Build from current directory
docker build -t <image-name> .        # Build and tag with a name
```

- `.` — the **build context** (path where Docker looks for files)
- `-t` — tag/name for the resulting image

---

## 🧠 Quick Reference Cheatsheet

```
# Lifecycle
docker run -it <image>              Run interactively
docker run -d -p 8080:3000 <image>  Run detached with port mapping
docker start / stop <id>            Start or stop a container
docker rm <id>                      Remove a container
docker container prune -f           Remove all stopped containers

# Inspection
docker ps / docker ps -a            List running / all containers
docker inspect <id or name>         Full details (JSON)
docker logs <id>                    View container logs

# Images
docker build -t <name> .            Build image from Dockerfile
docker commit -m "msg" <id> <name>  Save container state as image
docker rmi $(docker images -q) -f   Remove all images

# Inside containers
docker exec -it <name> bash         Open shell in running container

# Architecture (bottom-up)
dockerd → containerd → shim → runc → 🟢 Container
```
