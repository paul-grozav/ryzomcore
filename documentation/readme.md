```bash
ryzom_repo="/data/personal/ryzomcore" &&
# podman run -it --name rcbuilder -v ${ryzom_repo}/:${ryzom_repo}/:rw --network=host docker.io/ubuntu:24.04
# You will also have to install the packages from .devcontainer/Dockerfile

#podman-compose up &&
podman build \
  -f ${ryzom_repo}/.devcontainer/Dockerfile \
  -t ryzomcore/devcontainer \
  ${ryzom_repo}/.devcontainer \
  &&
podman run \
  -it \
  --name rcbuilder \
  -v ${ryzom_repo}:${ryzom_repo}:rw \
  --network=host \
  localhost/ryzomcore/devcontainer \
  &&
# Then, inside, run:
apt update &&
apt install -y \
  git \
  cmake \
  make \
  g++ \
  libboost-all-dev \
  libmysqlclient-dev \
  lua5.1 \
  liblua5.1-0-dev \
  python3 \
  zlib1g-dev \
  libxml2-dev \
  libpng-dev \
  &&

git clone https://github.com/your-repo/ryzomcore.git &&
cd ryzomcore  &&
mkdir build &&
cd build &&
cmake .. -DCMAKE_BUILD_TYPE=Debug &&
#cmake .. -DCMAKE_BUILD_TYPE=Release &&
make -j$(( $(nproc) - 2)) &&
make test &&
# For the client you need
apt-get install liblua5.2 libluabind-dev &&
( cd ryzom/client/ && ../../build/bin/ryzom_client ) &&
true
```


### Terms

#### Shard
​In Ryzom Core, a shard refers to an independent instance of the game world. Each
shard operates as a self-contained server environment, maintaining its own set
of services, databases, and configurations. This architecture allows for
multiple, parallel versions of the game universe, which can be used for
different purposes such as development, testing, or catering to specific player
communities.​

###### Composition of a Shard
A typical shard comprises several modular services, each handling specific
aspects of the game:​

These services are essential for operating a Ryzom shard:​
Ryzom Core Community | Nevrax Library

ryzom_entities_game_service (EGS): Manages gameplay elements such as player states, missions, items, and guilds. ​
Ryzom Core Community | Nevrax Library
+1
Ryzom Core Community | Nevrax Library
+1

ryzom_ai_service (AIS): Handles AI behaviors for non-player characters, often with multiple instances per shard to cover different geographic areas. ​
Ryzom Core Community | Nevrax Library
+1
Ryzom Core Community | Nevrax Library
+1

ryzom_frontend_service (FES): Manages client connections and propagates player positions. 
Ryzom Core Community | Nevrax Library
​

ryzom_general_utilities_service: Provides general utility functions across the shard.​

ryzom_ios_service (IOS): Manages input/output operations, including chat and localization. 
Ryzom Core Community | Nevrax Library
​

ryzom_tick_service (TS): Synchronizes time across all services within the shard. 
Ryzom Core Community | Nevrax Library
​

ryzom_naming_service (NS): Coordinates service discovery within the shard, functioning similarly to a real-time DNS. ​
U.S. Air Force
+2
Ryzom Core Community | Nevrax Library
+2
U.S. Air Force
+2

ryzom_admin_service (AS): Acts as an interface between web administration tools and shard services. ​
Ryzom Core Community | Nevrax Library
+1
Ryzom Core Community | Nevrax Library
+1

ryzom_backup_service (BMS): Handles saving and loading of persistent data like player characters and guilds. ​
Ryzom Core Community | Nevrax Library
+1
Ryzom Core Community | Nevrax Library
+1

ryzom_shard_unifier_service: Integrates multiple shards and administrative services into a cohesive system. ​
Ryzom Core Community | Nevrax Library
+4
Ryzom Core Community | Nevrax Library
+4
Ryzom Core Community | Nevrax Library
+4

ryzom_welcome_service (WS): Manages the login process and directs players to the appropriate frontend service. ​
Ryzom Core Community | Nevrax Library
+2
Ryzom Core Community | Nevrax Library
+2
Ryzom Core Community | Nevrax Library
+2

🛠️ Additional Utilities
Your directory also includes various tools for asset processing and development support, such as:​

build_rbank, build_soundbank, build_clod_bank: Used for compiling different types of game assets.​
Ryzom Core Community | Nevrax Library

patch_gen, patch_gen_service: Facilitate the creation and management of game patches.​

zone_check_bind, zone_lighter: Assist in map and zone development tasks.​
Ryzom Core Community | Nevrax Library
+1
Ryzom Core Community | Nevrax Library
+1



These services can be distributed across multiple physical or virtual servers,
or consolidated onto a single machine, depending on the scale and performance
requirements of the shard. 