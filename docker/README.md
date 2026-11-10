# Install Docker

Install Docker, check the engine, run nginx, then clean up. These steps target local development.

## Linux

- [Arch Linux walkthrough](arch-linux.md): actual installation shown in video.
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/), [Debian](https://docs.docker.com/engine/install/debian/), or [other supported distributions](https://docs.docker.com/engine/install/): use Docker's matching repository instructions. Arch package commands do not apply to apt-based systems.

After installation, `docker version` should show **Client** and **Server**. If Server is missing, check Docker's service and your account's access before continuing.

## macOS notes

Use the [official installer](https://docs.docker.com/desktop/setup/install/mac-install/) matching Apple silicon or Intel. Homebrew users can use:

```sh
brew install --cask docker-desktop
```

Open Docker Desktop, review its agreement, finish startup, and wait for the engine. Installing its files alone does not start Docker. Check:

```sh
docker version
```

This section contains documentation-based macOS notes. Video records Linux and Windows; it does not claim a local Mac test.

## Windows

1. Check [current Docker Desktop requirements](https://docs.docker.com/desktop/setup/install/windows-install/), including supported Windows version, virtualisation, and WSL. Use Linux containers for these demos.
2. In PowerShell, check `wsl --version`. If WSL is absent, follow [Microsoft's installation instructions](https://learn.microsoft.com/en-us/windows/wsl/install), run `wsl --install` as administrator, and restart when asked.
3. Download Docker Desktop from its official page for your processor. Run installer. Video uses recommended **per-user** installation and **WSL 2** backend.
4. Open Docker Desktop, review agreement and finish startup. Optional account sign-in can be skipped for this local demo. Wait for **Engine running**.
5. Open **Images**, search `nginx`, and pull official image. Video's Windows capture uses tag `stable-trixie-perl`; Linux demo uses `alpine`. Both serve nginx welcome page.
6. Choose **Run**, expand optional settings, name container `install-web`, map host port `8080` to container port `80`, then run it.
7. Open published port from container details. Browser at `http://localhost:8080` should show **Welcome to nginx!**
8. Inspect **Logs**, stop `install-web`, then remove that tutorial container. Do not remove unrelated containers or volumes.

Docker Desktop's licensing depends on use and organisation. Review current terms on Docker's site.

## Terminal verification

Linux/macOS users, or Windows users who prefer PowerShell, can run:

```sh
docker run -d --name install-web -p 127.0.0.1:8080:80 nginx:alpine
docker ps
```

Open `http://localhost:8080` in browser. When finished:

```sh
docker stop install-web
docker rm install-web
```

`127.0.0.1` keeps this terminal demo's published port on local loopback. Docker Desktop's GUI port mapping can bind more broadly; use it only in your isolated local lab or specify a loopback bind with terminal command above.
