<div align="center" width="100%">
    <img src="./frontend/public/icon.svg" width="128" alt="" />
</div>

# Dockge with GitHub Integration

> **Note:** This is a fork of [louislam/dockge](https://github.com/louislam/dockge) with enhanced Git/GitHub integration and additional features for managing unmanaged Docker Compose stacks.

A fancy, easy-to-use and reactive self-hosted docker compose.yaml stack-oriented manager.

[![Upstream Repo](https://img.shields.io/badge/upstream-louislam%2Fdockge-blue?logo=github)](https://github.com/louislam/dockge) [![GitHub Repo stars](https://img.shields.io/github/stars/louislam/dockge?logo=github&style=flat&label=upstream%20stars)](https://github.com/louislam/dockge) [![Docker Pulls](https://img.shields.io/docker/pulls/louislam/dockge?logo=docker)](https://hub.docker.com/r/louislam/dockge/tags)

<img src="https://github.com/louislam/dockge/assets/1336778/26a583e1-ecb1-4a8d-aedf-76157d714ad7" width="900" alt="" />

View Video: https://youtu.be/AWAlOQeNpgU?t=48

## ⭐ Features

### Enhanced Features in This Fork
- 🔀 **Git Integration** - Full Git integration for managing stacks with version control:
  - Check git status (branch, changed files, commits ahead/behind)
  - Add files to staging
  - Commit staged changes with custom messages
  - Push changes to remote repositories
  - Pull changes from remote repositories
  - Clone GitHub repositories to create new stacks
  - Support for both public and private repositories (with credential management)
- 📦 **Unmanaged Stack Support** - Manage Docker Compose stacks that weren't created by Dockge:
  - View and monitor external Docker Compose stacks
  - Stop and start unmanaged stacks
  - Separate grouping for managed vs. unmanaged stacks
  - Full control over existing Docker Compose deployments

### Core Features (from upstream)
- 🧑‍💼 Manage your `compose.yaml` files
  - Create/Edit/Start/Stop/Restart/Delete
  - Update Docker Images
- ⌨️ Interactive Editor for `compose.yaml`
- 🦦 Interactive Web Terminal
- 🕷️ (1.4.0 🆕) Multiple agents support - You can manage multiple stacks from different Docker hosts in one single interface
- 🏪 Convert `docker run ...` commands into `compose.yaml`
- 📙 File based structure - Dockge won't kidnap your compose files, they are stored on your drive as usual. You can interact with them using normal `docker compose` commands

<img src="https://github.com/louislam/dockge/assets/1336778/cc071864-592e-4909-b73a-343a57494002" width=300 />

### Additional Features in This Fork
- 🚄 Reactive - Everything is just responsive. Progress (Pull/Up/Down) and terminal output are in real-time
- 🐣 Easy-to-use & fancy UI - If you love Uptime Kuma's UI/UX, you will love this one too

![](https://github.com/louislam/dockge/assets/1336778/89fc1023-b069-42c0-a01c-918c495f1a6a)

## 🔄 Differences from Upstream

This fork extends the original [louislam/dockge](https://github.com/louislam/dockge) with the following enhancements:

### 1. Git/GitHub Integration
The original Dockge does not include any Git integration features. This fork adds comprehensive Git support:
- **Git Status Management**: View repository status, staged/unstaged files, and commit history
- **Clone Repositories**: Create new stacks by cloning Git repositories (HTTPS/SSH)
- **Version Control Operations**: Add, commit, push, and pull changes directly from the UI
- **Credential Management**: Securely store GitHub credentials for private repositories
- **Visual Git Interface**: User-friendly modal dialogs for all Git operations

### 2. Unmanaged Stack Support
The original Dockge only manages stacks it creates within its stacks directory. This fork extends support to:
- **Discover External Stacks**: Automatically detect Docker Compose stacks running on the system
- **Manage Unmanaged Stacks**: Stop, start, and monitor stacks not created by Dockge
- **Separate Grouping**: Visual distinction between managed and unmanaged stacks
- **Safe Operations**: Carefully handle external stacks without modifying their configuration files

These features make this fork ideal for environments where:
- You want to version control your Docker Compose configurations
- You work with Git/GitHub-based infrastructure workflows
- You need to manage existing Docker Compose deployments alongside new ones

## 🔧 How to Install

> **Note:** This fork currently uses the same installation method as upstream. If you want to use this fork with the enhanced features, you'll need to build it from source (see below) or wait for Docker images to be published for this fork.

### Installing Upstream Dockge (Standard Installation)

Requirements:
- [Docker](https://docs.docker.com/engine/install/) 20+ / Podman
- (Podman only) podman-docker (Debian: `apt install podman-docker`)
- OS:
  - Major Linux distros that can run Docker/Podman such as:
     - ✅ Ubuntu
     - ✅ Debian (Bullseye or newer)
     - ✅ Raspbian (Bullseye or newer)
     - ✅ CentOS
     - ✅ Fedora
     - ✅ ArchLinux
  - ❌ Debian/Raspbian Buster or lower is not supported
  - ❌ Windows (Will be supported later)
- Arch: armv7, arm64, amd64 (a.k.a x86_64)

### Basic

- Default Stacks Directory: `/opt/stacks`
- Default Port: 5001

```
# Create directories that store your stacks and stores Dockge's stack
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge

# Download the compose.yaml
curl https://raw.githubusercontent.com/louislam/dockge/master/compose.yaml --output compose.yaml

# Start the server
docker compose up -d

# If you are using docker-compose V1 or Podman
# docker-compose up -d
```

Dockge is now running on http://localhost:5001

### Advanced

If you want to store your stacks in another directory, you can generate your compose.yaml file by using the following URL with custom query strings.

```
# Download your compose.yaml
curl "https://dockge.kuma.pet/compose.yaml?port=5001&stacksPath=/opt/stacks" --output compose.yaml
```

- port=`5001`
- stacksPath=`/opt/stacks`

Interactive compose.yaml generator is available on: 
https://dockge.kuma.pet

## How to Update

```bash
cd /opt/dockge
docker compose pull && docker compose up -d
```

## 🔨 Building This Fork from Source

If you want to use this fork with the Git integration and unmanaged stack features, you can build it from source:

### Prerequisites
- Node.js >= 22.14.0
- npm
- Git
- Docker (for building the Docker image)

### Steps

1. **Clone the repository:**
```bash
git clone https://github.com/mhmgad/dockge-with-github.git
cd dockge-with-github
```

2. **Install dependencies:**
```bash
npm install
```

3. **Build the frontend:**
```bash
npm run build
```

4. **Run in development mode (optional):**
```bash
# Terminal 1 - Backend
npm run dev:backend

# Terminal 2 - Frontend
npm run dev:frontend
```

5. **Build Docker image (for production):**
```bash
docker build -t dockge-with-github:latest -f docker/Dockerfile .
```

6. **Run with Docker:**
```bash
# Create directories
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge

# Create a compose.yaml using your built image
# Edit the compose.yaml to use dockge-with-github:latest instead of louislam/dockge

# Start the server
docker compose up -d
```

For more development details, see [CONTRIBUTING.md](./CONTRIBUTING.md).

## Screenshots

![](https://github.com/louislam/dockge/assets/1336778/e7ff0222-af2e-405c-b533-4eab04791b40)


![](https://github.com/louislam/dockge/assets/1336778/7139e88c-77ed-4d45-96e3-00b66d36d871)

![](https://github.com/louislam/dockge/assets/1336778/f019944c-0e87-405b-a1b8-625b35de1eeb)

![](https://github.com/louislam/dockge/assets/1336778/a4478d23-b1c4-4991-8768-1a7cad3472e3)


## Motivations

### Original Dockge Motivations (by Louis Lam)
- I have been using Portainer for some time, but for the stack management, I am sometimes not satisfied with it. For example, sometimes when I try to deploy a stack, the loading icon keeps spinning for a few minutes without progress. And sometimes error messages are not clear.
- Try to develop with ES Module + TypeScript

### This Fork's Additional Motivations
- **Git Integration**: Many DevOps workflows rely on Git for infrastructure-as-code. Having Git operations directly in the Dockge UI streamlines the workflow for managing Docker Compose files in version control.
- **Unmanaged Stack Support**: In real-world scenarios, systems often have Docker Compose stacks that predate Dockge or were created through other means. Being able to manage these alongside Dockge-created stacks provides a complete view of the system.
- **GitHub-Centric Workflows**: Supporting repository cloning and credential management makes it easy to deploy stacks directly from GitHub repositories.

If you love this project, please consider giving it a ⭐.


## 🗣️ Community and Contribution

### For Issues Related to Enhanced Features (Git Integration, Unmanaged Stacks)
- **Bug Reports**: [mhmgad/dockge-with-github/issues](https://github.com/mhmgad/dockge-with-github/issues)
- **Discussions**: [mhmgad/dockge-with-github/discussions](https://github.com/mhmgad/dockge-with-github/discussions)

### For Core Dockge Issues
- **Bug Reports**: [louislam/dockge/issues](https://github.com/louislam/dockge/issues)
- **Discussions**: [louislam/dockge/discussions](https://github.com/louislam/dockge/discussions)

### Translation
If you want to translate Dockge into your language, please read [Translation Guide](https://github.com/louislam/dockge/blob/master/frontend/src/lang/README.md)

### Create a Pull Request

For contributions related to Git integration or unmanaged stack features, submit PRs to this fork. For core Dockge features, please submit to the [upstream repository](https://github.com/louislam/dockge) and read their [contribution guide](https://github.com/louislam/dockge/blob/master/CONTRIBUTING.md).

## FAQ

#### "Dockge"?

"Dockge" is a coinage word which is created by myself. I originally hoped it sounds like `Dodge`, but apparently many people called it `Dockage`, it is also acceptable.

The naming idea came from Twitch emotes like `sadge`, `bedge` or `wokege`. They all end in `-ge`.

#### Can I manage a single container without `compose.yaml`?

The main objective of Dockge is to try to use the docker `compose.yaml` for everything. If you want to manage a single container, you can just use Portainer or Docker CLI.

#### Can I manage existing stacks?

Yes, you can. However, you need to move your compose file into the stacks directory:

1. Stop your stack
2. Move your compose file into `/opt/stacks/<stackName>/compose.yaml`
3. In Dockge, click the " Scan Stacks Folder" button in the top-right corner's dropdown menu
4. Now you should see your stack in the list

#### Is Dockge a Portainer replacement?

Yes or no. Portainer provides a lot of Docker features. While Dockge is currently only focusing on docker-compose with a better user interface and better user experience.

If you want to manage your container with docker-compose only, the answer may be yes.

If you still need to manage something like docker networks, single containers, the answer may be no.

#### Can I install both Dockge and Portainer?

Yes, you can.

#### How does Git Integration work?

**Git Status Management:**

If your stack directory is a Git repository, you can use the "Git Status" button on the stack's page to:
- View the current git status (branch, changed files, commits ahead/behind)
- Add untracked files to staging
- Commit staged changes
- Push changes to the remote repository
- Pull changes from the remote repository

**Clone Repository:**

You can clone a Git repository to create a new stack:
1. Click the "Clone Repository" button on the Dashboard
2. Enter the repository URL (HTTPS or SSH)
3. Enter a name for the new stack
4. For private repositories, provide your GitHub credentials
5. The repository will be cloned and a new stack will be created

The first time you push, pull, or clone a private repository, you'll be asked for your GitHub credentials (username and password/token). These credentials are stored for future operations.

**Note**: For SSH remotes, please configure SSH keys separately as the credential injection only works with HTTPS URLs.

## 🙏 Credits and Attribution

This project is a fork of [Dockge](https://github.com/louislam/dockge) created by [Louis Lam](https://github.com/louislam), the creator of [Uptime Kuma](https://github.com/louislam/uptime-kuma).

**Upstream Project**: [louislam/dockge](https://github.com/louislam/dockge)

All core functionality and the beautiful, reactive UI are thanks to the original Dockge project. This fork adds Git/GitHub integration and enhanced support for managing unmanaged Docker Compose stacks.

If you love this project, please consider giving a ⭐ to both:
- This fork: [mhmgad/dockge-with-github](https://github.com/mhmgad/dockge-with-github)
- The original: [louislam/dockge](https://github.com/louislam/dockge)

## Others

Dockge is built on top of [Compose V2](https://docs.docker.com/compose/migrate/). `compose.yaml`  also known as `docker-compose.yml`.
