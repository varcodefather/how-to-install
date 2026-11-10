# Install Docker on Arch Linux

For Arch and Arch-based systems. Other Linux distributions need their own package instructions.

Update your system first using your usual Arch update process, then install Docker and its Compose and Buildx plugins:

```sh
sudo pacman -S docker docker-buildx docker-compose
```

Start Docker and enable it for the next boot:

```sh
sudo systemctl enable --now docker
systemctl is-active docker
```

The second command should say `active`.

For a trusted local development account, add your user to the Docker group. That group can control Docker with broad privileges on this machine:

```sh
sudo usermod -aG docker "$USER"
```

**Sign out and back in** before the next commands. An existing terminal does not gain the new group automatically.

```sh
docker version
docker compose version
```

`docker version` should show both Client and Server. If it only shows Client followed by an error, check the service and your new login's group membership.

Run a small web server:

```sh
docker run -d --name install-web \
  -p 127.0.0.1:8080:80 nginx:alpine
docker ps
```

Open **http://localhost:8080** on this machine. The nginx welcome page confirms the container and port mapping work. Binding the published port to `127.0.0.1` keeps this test local.

When finished:

```sh
docker stop install-web
docker rm install-web
```

Docker stays installed; these commands remove only the test container.

References: [Arch Docker package](https://archlinux.org/packages/extra/x86_64/docker/), [ArchWiki Docker guide](https://wiki.archlinux.org/title/Docker).
