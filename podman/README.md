# Install Podman and Podman Desktop

Podman runs containers. Podman Desktop provides a graphical interface. Recording uses Podman **6.1.4** and Podman Desktop **1.30.5**.

## Linux — Arch package route

Start with an updated Arch system:

```sh
sudo pacman -S podman flatpak
podman version
podman ps
systemctl --user enable --now podman.socket
```

Run Podman as your normal account for rootless containers. Empty container list is expected. If user mapping fails, follow [Podman rootless setup](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md), including subordinate UID/GID ranges, before continuing.

Ubuntu, Debian and Fedora users: follow [Podman installation instructions](https://podman.io/docs/installation) for your distribution. Arch package commands above are specific to Arch.

Install Desktop from Flathub:

```sh
flatpak remote-add --if-not-exists --user flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install --user -y flathub io.podman_desktop.PodmanDesktop
flatpak run io.podman_desktop.PodmanDesktop
```

If app launcher entry is missing after installing Flatpak, sign out and back in or use launch command above. In onboarding keep Podman selected; optional kubectl and Compose can wait. Telemetry can stay off. Linux connects to local Podman engine without creating a Podman machine.

## macOS notes

Project recommends [official DMG installer](https://podman-desktop.io/docs/installation/macos-install). Choose build for your processor, move app into Applications, then follow onboarding to install Podman and create its Linux machine. Check **Settings → Resources** for running engine.

Homebrew alternative remains available:

```sh
brew install --cask podman-desktop
```

These are documentation-based macOS notes. Actual recordings use Linux and Windows.

## Windows — GUI first

1. Check [current requirements and WSL instructions](https://podman-desktop.io/docs/installation/windows-install). Enable virtualization in firmware if needed.
2. Check existing WSL in PowerShell: `wsl --version`.
3. If WSL is missing, run `wsl --install --no-distribution` in administrator PowerShell and restart when requested.
4. Download [official Podman Desktop installer](https://podman-desktop.io/downloads). Current-user install is shown in video.
5. Start onboarding with Podman selected; skip optional kubectl and Compose. Optional telemetry can remain off.
6. Install Podman CLI when prompted. Choose **WSL2** provider. Hyper-V administrator warning refers to other provider.
7. Create default Podman machine with **Start the machine now** enabled. Wait for engine to connect.

Recording keeps Windows machine defaults, including rootful engine. Linux example uses rootless containers. Docker and Podman maintain separate container stores; verify engine selected in Desktop.

## Run a website — Linux and Windows

1. Open **Images → Pull**.
2. Enter `docker.io/library/nginx:alpine`, choose **Podman** engine, then **Pull**.
3. Choose **Run**. Name container `podman-web`.
4. Map local port **8080** to container port **80**. Choose **Create and start**.
5. Open **Summary**. Check engine **Podman**, state **running**, port **8080**.
6. Open published port in browser: <http://localhost:8080>. Expect nginx welcome page.
7. Return to Desktop, stop `podman-web`, then delete only that disposable test container.

CLI equivalent on Linux, binding test website to loopback:

```sh
podman run -d --name podman-web -p 127.0.0.1:8080:80 docker.io/library/nginx:alpine
podman ps
podman stop podman-web
podman rm podman-web
```

Desktop port form shown in video uses default host bind. In isolated recording VMs this remains behind VM NAT. Choose loopback binding for a host-only local test when available; no public-network firewall rule is needed for localhost check.

## Videos

- [Install Podman on Arch Linux — vertical Short](https://youtube.com/shorts/sOcd-od0e8k)

## Official references

- [Podman installation](https://podman.io/docs/installation)
- [Podman Desktop on Linux](https://podman-desktop.io/docs/installation/linux-install)
- [Podman Desktop on macOS](https://podman-desktop.io/docs/installation/macos-install)
- [Podman Desktop on Windows](https://podman-desktop.io/docs/installation/windows-install)
