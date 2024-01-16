# Build Container for AOSP (Docker)

## About

This repository holds a Dockerfile to provide the needed toolchain for building AOSP14+

## Install Docker CE and Compose
1. Install Docker CE and Docker-Compose on your host machine (recommended: 8 Cores, 250GB+ HDD)
2. see https://docs.docker.com/engine/install/ and https://docs.docker.com/compose/install/

## Run
1. ```docker-compose up -d --build```
2. ```docker exec -it aosp_builder bash```

## Download AOSP source
This follows the normal AOSP approach, e.g.

1. ```cd /AOSP```
2. ```repo init --depth=1 -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r21```
3. ```repo sync -c --no-tags --no-clone-bundle -j$(nproc --all)```

See also [Android tags](https://source.android.com/docs/setup/about/build-numbers)

## Build AOSP
This follows the normal AOSP approach, e.g.

1. ```cd /AOSP```
2. ```source build/envsetup.sh```
3. ```lunch sdk_phone_x86_64```
4. ```rm -rf out/ #(cleans build target folder)```
5. ```m -j$(nproc --all)```

## Tips
Use screen on your docker host if you connect via ssh

1. ```screen -S aosp``` creates a screen session
2. ```screen -r aosp``` attaches to a screen session
3. ```screen -d``` or ```CTRL+a + CTRL+d``` detaches a screen 
session
4. ```exit``` closes a screen session
