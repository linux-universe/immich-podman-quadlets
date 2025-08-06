# immich-podman-quadlets

The original Immich Docker Compose file rewritten in Podman Quadlet.

## Steps

Clone to /etc/containers/systemd

`git clone https://github.com/linux-universe/immich-podman-quadlets.git /etc/containers/systemd`

Clone to `~/.config/containers/systemd/ for rootless

Go trough every .container file and replace the ${} variables with the needen value.

For example:

```
Image=ghcr.io/immich-app/immich-server:${IMMICH_VERSION}
--->
Image=ghcr.io/immich-app/immich-server:release
```

See https://immich.app/docs/install/environment-variables for the default values

Reload with

`systemctl daemon-reload`

or rootless:

`systemctl --user daemon-reload`

Then start with

`sytemctl start immich-server`

or rootless:

`systemctl --user start immich-server`
