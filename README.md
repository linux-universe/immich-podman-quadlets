# immich podman quadlets

The original [Immich Docker Compose](https://github.com/immich-app/immich/blob/main/docker/docker-compose.yml) file rewritten using Podman Quadlet.

Check out the [wiki](https://github.com/linux-universe/immich-podman-quadlets/wiki) for additional guides, such as Hardware Transcoding & Acceleration.

>This guide serves as a stock base setup for Immich using Podman Quadlets. It closely follows the structure and intent of the official Docker Compose file provided by Immich. The goal is to provide a shared starting point the community can build upon. Instead of writing isolated full guides, consider contributing your enhancements or customizations to the [Wiki](https://github.com/linux-universe/immich-podman-quadlets/wiki).

## Steps

1. Either clone or manually write the quadlet files to `/etc/containers/systemd/immich`:

   ```shell
   git clone https://github.com/linux-universe/immich-podman-quadlets.git /etc/containers/systemd/immich
   ```

> [!NOTE]
> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/immich` instead.

2. Go through every `.container` file and replace the `${}` variables with the needed values.

   For example:

   ```diff
   - Image=ghcr.io/immich-app/immich-server:${IMMICH_VERSION}
   
   + Image=ghcr.io/immich-app/immich-server:release
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
