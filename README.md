# choreonoid_ros2_dev_env



https://github.com/user-attachments/assets/d91177bc-d841-4c2a-a683-3e6c2f171529



## Prerequisites

- Docker

> [!NOTE]
> Docker installed from snap may not be compatible with GPU.
> We recommend using docker from apt.

- nvidia-driver

> [!NOTE]
> The `nvidia-driver` version recommended by `ubuntu-drivers devices` is a good choice.

reference: https://ssr-yuki.hatenablog.com/entry/2024/05/18/073337

## Clone (Host)

```bash
git clone git@github.com:HansRobo/choreonoid_ros2_dev_env.git
```

```bash
sudo pip install vcstool
cd choreonoid_ros2_dev_env
mkdir -p ros2_ws/src
vcs import ros2_ws/src < choreonoid.repos
```

## Install docker compose v2 (Host)

For docker compose v2.27.0:

```bash
sudo mkdir -p /usr/local/libexec/docker/cli-plugins
cd /usr/local/libexec/docker/cli-plugins
sudo curl -L https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o docker-compose
sudo chmod +x docker-compose
```

See [the official documents](https://docs.docker.com/compose/install/linux/) to install the other version.

## With GPU: Install NVIDIA Container Toolkit (Host)

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker.service
```

See [NVIDIA's official documents](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) for detailed explanation.

## Start Container (Host)

> [!NOTE]
> For the first time, it will take some time to build the container.

#### Without GPU

```bash
docker compose up -d choreonoid_humble
```

#### With GPU

```bash
docker compose up -d choreonoid_humble_gpu
```

#### Both

```bash
docker compose up -d
```

## Development

### Enter Container (Host)

#### Without GPU

```bash
xhost +local:
docker exec -it choreonoid_humble bash
```

#### With GPU

```bash
xhost +local:
docker exec -it choreonoid_humble_gpu bash
```

### Install dependencies via rosdep (Container)

```bash
rosdep install -i -y --from-path ~/ros2_ws/src
```

### Build (Container)

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### Run (Container)

```bash
source install/setup.bash
ros2 launch choreonoid_ros choreonoid.launch.xml
```
