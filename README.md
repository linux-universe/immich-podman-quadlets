# immich-podman-quadlets

The original [Immich Docker Compose](https://github.com/immich-app/immich/blob/main/docker/docker-compose.yml) file rewritten using Podman Quadlet.

## Steps

1. Clone to `/etc/containers/systemd`:

   ```shell
   git clone https://github.com/linux-universe/immich-podman-quadlets.git /etc/containers/systemd
   ```

> [!NOTE]
> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/` instead.

2. Go through every `.container` file and replace the `${}` variables with the needed values.

   For example:

   ```
   Image=ghcr.io/immich-app/immich-server:${IMMICH_VERSION}
   --->
   Image=ghcr.io/immich-app/immich-server:release
   ```

   See [https://immich.app/docs/install/environment-variables](https://immich.app/docs/install/environment-variables) for the default values.

3. Reload systemd:

   ```shell
   systemctl daemon-reload
   ```

4. Then start Immich:

   ```shell
   systemctl start immich-server
   ```
> [!NOTE]
> Run steps 3 and 4 with the `--user` argument for rootless setups.

## Machine Learning

You need to change the machine learning URL in the admin settings to `http://systemd-immich-machine-learning:3003` in order for it to work.

---
