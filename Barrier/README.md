From: https://github.com/debauchee/barrier/wiki/Building-on-Linux

# [ Barrier Software Linux Ubuntu 20.04 Compile ]

# Building on Linux
Dom Rodriguez edited this page on Sep 22, 2020 · 18 revisions
Binaries Built

  *  barrier the GUI front-end and "main" program
  *  barriers the CLI server component
  *  barrierc the CLI client component

note: these aren't the only things build at compile time, but merely what the end-user is expected to use.
Preparing your system
Ubuntu/Debian/Raspbian


## Prerequirements

```bash
sudo apt update && sudo apt upgrade
sudo add-apt-repository ppa:rock-core/qt4
sudo apt update
sudo apt install git cmake make xorg-dev g++ libcurl4-openssl-dev \
                 libavahi-compat-libdnssd-dev libssl-dev libx11-dev \
                 libqt4-dev qtbase5-dev
```


## Building from source

```bash
git clone https://github.com/debauchee/barrier.git
# this builds from master,
# you can get release tarballs instead
# if you want to build from a specific tag/release
cd barrier
git submodule update --init --recursive
./clean_build.sh
cd build
sudo make install # install to /usr/local/
----
[sudo] password for roger: 
[  1%] Built target gmock
[  1%] Built target gtest
[  8%] Built target arch
[ 11%] Built target common
[ 17%] Built target base
[ 19%] Built target mt
[ 21%] Built target io
[ 26%] Built target net
[ 41%] Built target synlib
[ 49%] Built target server
[ 55%] Built target platform
[ 59%] Built target ipc
[ 60%] Built target client
[ 61%] Built target barrierc
[ 63%] Built target barriers
[ 67%] Built target integtests
[ 74%] Built target unittests
[ 75%] Automatic MOC and UIC for target guiunittests
[ 75%] Built target guiunittests_autogen
[ 79%] Built target guiunittests
[ 80%] Automatic MOC and UIC for target barrier
[ 80%] Built target barrier_autogen
[100%] Built target barrier
Install the project...
-- Install configuration: "Debug"
-- Installing: /usr/local/share/icons/hicolor/scalable/apps/barrier.svg
-- Installing: /usr/local/share/applications/barrier.desktop
-- Installing: /usr/local/bin/barrierc
-- Installing: /usr/local/bin/barriers
-- Installing: /usr/local/bin/barrier

----
```
