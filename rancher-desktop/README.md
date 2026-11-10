# Install Rancher Desktop

Rancher Desktop 1.25.0. Choose **dockerd (moby)** for the Docker commands below. **containerd** uses `nerdctl`; its containers and images are separate. Kubernetes stays disabled for this container-only install.

## Linux: official AppImage

Rancher Desktop starts a Linux VM. Hardware virtualisation and read/write access to `/dev/kvm` are required:

```sh
test -r /dev/kvm && test -w /dev/kvm && echo 'KVM ready'
```

If this does not say `KVM ready`, resolve access using the [official Linux requirements](https://docs.rancherdesktop.io/getting-started/installation/#linux). A missing `/dev/kvm` also needs virtualisation support; group membership alone cannot create it.

On an updated Arch system, install AppImage dependencies:

```sh
sudo pacman -S fuse2 pass gnupg
```

Download the official x86_64 AppImage, keep it in a stable location, and launch it:

```sh
mkdir -p ~/Applications
curl -fL -o ~/Applications/RancherDesktop.AppImage \
  https://download.opensuse.org/repositories/isv:/Rancher:/stable/AppImage/rancher-desktop-latest-x86_64.AppImage
chmod +x ~/Applications/RancherDesktop.AppImage
~/Applications/RancherDesktop.AppImage
```

For Ubuntu/Debian or Fedora, the [installation guide](https://docs.rancherdesktop.io/getting-started/installation/) also provides distribution package instructions. Arch commands above are not interchangeable with those package managers.

In first-run setup:

1. Turn **Enable Kubernetes** off.
2. Keep **dockerd (moby)** selected.
3. Use automatic PATH configuration, or add `~/.rd/bin` yourself if choosing Manual.
4. Confirm settings and wait for the engine to finish starting.
5. Open a new terminal so PATH changes take effect.

Linux uses `pass` for registry credentials. Public nginx needs no registry login. Before using `docker login` or `nerdctl login`, follow the guide's GPG and `pass` setup section.

## macOS notes

Use the official DMG for your processor, drag Rancher Desktop into Applications, then open it. The project recommends the DMG; the Homebrew cask is an alternative maintained outside the Rancher Desktop team:

```sh
brew install --cask rancher
```

Choose Moby, disable Kubernetes for this example, and wait for startup. These are documentation-based Mac notes; the demonstrations use Linux and Windows.

## Windows: current-user install

Check the [current Windows requirements](https://docs.rancherdesktop.io/getting-started/installation/#windows), including virtualisation and WSL. Check WSL in PowerShell:

```powershell
wsl --version
```

If WSL is missing, install it from administrator PowerShell and restart when requested:

```powershell
wsl --install --no-distribution
```

Download the MSI from the [official release](https://github.com/rancher-sandbox/rancher-desktop/releases), review its licence and choose **Install just for you** for this localhost example. This avoids installing the privileged service for access from other machines. Open Rancher Desktop, select Moby, disable Kubernetes and wait for its WSL environment and engine to start.

## Verify, run nginx, then clean up

Open a new terminal or PowerShell window:

```sh
docker context use rancher-desktop
docker context show
docker version
docker run -d --name rancher-web -p 127.0.0.1:8080:80 nginx:alpine
docker ps
```

The context should be `rancher-desktop`, Docker version should include **Server**, and `rancher-web` should be running. Context selection matters when another Docker engine is installed.

Open [localhost:8080](http://localhost:8080/) in your browser. Expect **Welcome to nginx!** In Rancher Desktop's **Containers** tab, open `rancher-web` and inspect **Logs**. Return to the list, select only that container, choose **Stop**, then **Delete** and confirm its name.

CLI cleanup equivalent:

```sh
docker stop rancher-web
docker rm rancher-web
```

Rancher Desktop stays installed. Only the disposable website container is removed. To return to another Docker engine, choose its context explicitly; `docker context ls` lists available contexts.

## References

- [Installation and prerequisites](https://docs.rancherdesktop.io/getting-started/installation/)
- [Moby versus containerd](https://docs.rancherdesktop.io/ui/preferences/container-engine/general/)
- [Container controls](https://docs.rancherdesktop.io/ui/containers/)
