# immich-podman-quadlets

The original [Immich Docker Compose](https://github.com/immich-app/immich/blob/main/docker/docker-compose.yml) file rewritten using Podman Quadlet.

## Steps

1. Clone to `/etc/containers/systemd/immich`:

   ```shell
   git clone https://github.com/linux-universe/immich-podman-quadlets.git /etc/containers/systemd/immich
   ```

> [!NOTE]
> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/immich` instead.

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

## Hardware Transcoding

### VAAPI (AMD / NVIDIA / Intel) & Quick Sync (Intel)

To enable VAAPI or Quick Sync hardware transcoding, uncomment the following line in `immich-server.container`:
```
AddDevice=/dev/dri
```

In the Admin page under **Video transcoding settings**, change the hardware acceleration setting to the appropriate option and save.

### NVENC (NVIDIA)

> [!NOTE]
> As I don't have the necessary hardware, I couldn't test the Nvidia instruction. Any confirmation would be appreciated.

Uncomment **and add** the following lines in `immich-server.container`:
```
AddDevice=/dev/dri
AddDevice=nvidia.com/gpu=0 # Make sure this matches your GPU ID
Unmask=/dev/dri:/dev/dri
```

In the Admin page under **Video transcoding settings**, change the hardware acceleration setting to the appropriate option and save.

## Hardware-Accelerated Machine Learning

> [!NOTE]
> As I don't have the necessary hardware, I couldn't test the both the CUDA and ROCM instructions. Any confirmation would be appreciated.

### ROCM (AMD)

1. Add `-rocm` to the end of the Image= line in `immich-machine-learning.container`.
2. Add the following lines in `immich-machine-learning.container` under `[Container]`.

   ```
   GroupAdd=video
   AddDevice=/dev/dri
   AddDevice=/dev/kfd
   ```
   
   You may need to add `Environment=HSA_OVERRIDE_GFX_VERSION="x.x.x"` depending on your GPU.

### CUDA (NVIDIA)

1. Add `-cuda` to the end of the Image= line in `immich-machine-learning.container`.
2. Add the following lines in `immich-machine-learning.container` under `[Container]`.

   ```
   AddDevice=/dev/dri
   AddDevice=nvidia.com/gpu=0 # Make sure this matched your GPU ID
   Unmask=/dev/dri:/dev/dri
   ```

---
thanks to [tbelways repo](https://github.com/tbelway/immich-podman-quadlets) for inspiration
