# choreonoid_ros2_dev_env

## Clone(Host)

This repository uses submodules. To clone it, use the following command:
```bash
git clone git@github.com:HansRobo/choreonoid_ros2_dev_env.git --recursive
```

## Install docker compose v2(Host)

```bash
sudo mkdir -p /usr/local/libexec/docker/cli-plugins
cd /usr/local/libexec/docker/cli-plugins
sudo curl -L https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o docker-compose
sudo chmod +x docker-compose
```
## Build Container(Host)

```bash
docker compose build
```

## Start Container(Host)

```bash
docker compose up -d
```

## Development

### Enter Container(Host)

```bash
xhost +local:
docker exec -it choreonoid_workspace bash
```

### Build(Container)

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### Run(Container)

```bash
source install/setup.bash
ros2 launch choreonoid_ros choreonoid.launch.xml
```
