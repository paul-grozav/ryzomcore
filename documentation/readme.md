# Build instructions for the server

## Prerequisites
```bash
ryzom_repo="/data/personal/ryzomcore" &&
# podman run -it --name rcbuilder -v ${ryzom_repo}/:${ryzom_repo}/:rw --network=host docker.io/ubuntu:24.04
# You will also have to install the packages from .devcontainer/Dockerfile

#podman-compose up &&
podman build -f ${ryzom_repo}/.devcontainer/Dockerfile -t ryzomcore/devcontainer ${ryzom_repo}/.devcontainer
podman run -it --name rcbuilder -v ${ryzom_repo}:${ryzom_repo}:rw --network=host localhost/ryzomcore/devcontainer
# Then, inside, run:
apt update && apt install -y \
    git cmake make g++ \
    libboost-all-dev \
    libmysqlclient-dev \
    lua5.1 liblua5.1-0-dev \
    python3 \
    zlib1g-dev \
    libxml2-dev \
    libpng-dev
```

## Steps to Build
1. Clone the repository:
    ```bash
    git clone https://github.com/your-repo/ryzomcore.git
    cd ryzomcore
    ```

2. Create a build directory:
    ```bash
    mkdir build
    cd build
    ```

3. Configure the build using CMake:
    ```bash
    cmake .. -DCMAKE_BUILD_TYPE=Release
    ```

4. Build the server:
    ```bash
    make -j$(nproc)
    ```

5. (Optional) Run tests to verify the build:
    ```bash
    make test
    ```

## Post-Build
- The server binary will be located in the `bin` directory within the build folder.
- Ensure you configure the server by editing the configuration files located in the `config` directory.

## Notes
- Refer to the project's documentation for additional configuration and deployment instructions.
- If you encounter issues, check the logs or consult the community for support.
