# Install kind and kubectl

kind runs local Kubernetes nodes as containers. This walkthrough uses **Docker** as its provider. Have Docker running first; `docker version` must show **Client** and **Server**.

## Arch Linux: quick package route

Start with an updated Arch system:

```sh
sudo pacman -S kind kubectl
kind version
kubectl version --client
```

This package route is shown in Arch Short. Full video also demonstrates release-binary route below.

## Linux: release-binary route

Full video uses Linux AMD64. Check `uname -m`: use `amd64` for `x86_64`; use matching `arm64` release for `aarch64`. Check [official releases](https://github.com/kubernetes-sigs/kind/releases) for current version. Recording uses kind **v0.33.0**.

```sh
curl -fLO https://kind.sigs.k8s.io/dl/v0.33.0/kind-linux-amd64
curl -fLO https://github.com/kubernetes-sigs/kind/releases/download/v0.33.0/kind-linux-amd64.sha256sum
sha256sum --check kind-linux-amd64.sha256sum
mkdir -p ~/.local/bin
install -m 755 kind-linux-amd64 ~/.local/bin/kind
kind version
```

`~/.local/bin` must be on your PATH. If needed, add `export PATH="$HOME/.local/bin:$PATH"` to your shell's startup file, then open a new terminal.

Install kubectl with your distribution's supported package route or [Kubernetes binary instructions](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/). Arch example:

```sh
sudo pacman -S kubectl
kubectl version --client
```

## macOS notes

With [Homebrew](https://brew.sh/) installed:

```sh
brew install kind kubectl
docker version
kind version
kubectl version --client
```

Docker Desktop must be open with engine running. These are documentation-based macOS notes; recorded demonstrations use Linux and Windows.

## Windows

Open Docker Desktop, use Linux containers, and wait for engine. Open PowerShell:

```powershell
docker version
New-Item -ItemType Directory C:\DevTools
curl.exe -fLo C:\DevTools\kind.exe https://kind.sigs.k8s.io/dl/v0.33.0/kind-windows-amd64
cd C:\DevTools
curl.exe -fLo kubectl.exe https://dl.k8s.io/release/v1.37.1/bin/windows/amd64/kubectl.exe
```

Create directory only if it does not already exist. Commands above use Windows AMD64, matching recorded machine. Consult [kind quick start](https://kind.sigs.k8s.io/docs/user/quick-start/) and [kubectl Windows instructions](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/) for supported downloads and current versions.

1. Open Start and search **Edit environment variables for your account**.
2. Under **User variables**, select **Path**, then **Edit**.
3. Choose **New** and add `C:\DevTools`. Keep existing entries.
4. Move it above Docker Desktop's entry so downloaded kubectl is found first.
5. Save both dialogs. Close old terminal, then open a fresh PowerShell window.

```powershell
kind version
kubectl version --client
```

Recording verifies kind **0.33.0**, kubectl **1.37.1**, and Kubernetes node **1.37.0** on Windows. If command is not recognised, check executable filename, user PATH, and fresh terminal.

## Create and verify cluster — all three systems

Use unique lab name. First startup can take longer while node image downloads.

```sh
kind create cluster --name install-demo --wait 120s
kubectl --context kind-install-demo get nodes
```

Expect control-plane node with **Ready** status. Explicit context keeps command pointed at this local lab.

When finished:

```sh
kind delete cluster --name install-demo
kind get clusters
```

Delete only your tutorial cluster. Docker, kind, and kubectl remain installed.

## Video

[Watch full kind install — Linux, macOS notes and Windows](https://youtu.be/OkVDcEbMUOM).

[Watch Arch Linux install — vertical Short](https://youtube.com/shorts/Cj4lSiVrenc).
