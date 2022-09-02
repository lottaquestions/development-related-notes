# General notes on the development workflow of MariaDB

## Dependencies when installing MariaDB in Ubuntu
 sudo apt install gnutls-dev
 sudo apt install gzip
 sudo apt install bison
 sudo apt install bison-dev
 sudo apt install libevent-dev
 sudo apt install cmake 
 sudo apt install openssl
 sudo apt install libjemalloc-dev libjemalloc2 -y
 sudo  apt install zlib1g
 sudo apt install zlib1g-dev
 sudo apt install valgrind
 sudo apt install kcachegrind
 sudo apt install massif-visualizer
 sudo apt install curl
 sudo apt -y install libncurses5
 sudo apt -y install libncurses5-dev
 sudo apt install libboost-dev libboost -y
 sudo apt install libboost-dev -y
 sudo apt install -y  libboost-doc libboost1.71-doc libboost-atomic1.71-dev libboost-chrono1.71-dev libboost-container1.71-dev libboost-context1.71-dev libboost-contract1.71-dev libboost-coroutine1.71-dev
 sudo apt install libboost-all-dev
 sudo apt install pmem
 sudo apt install bzip2
 sudo apt install lz4
 sudo apt install LibLZMA
 sudo apt install liblzma-dev -y
 sudo apt install lzo
 sudo apt install liblzo2-2 liblzo2-dev -y
 sudo apt install libpmem-dev libpmem1 libpmem1-debug -y
 sudo apt install libxml2 libxml2-dev -y
 sudo apt install libsnappy1v5 ibsnappy-dev -y
 sudo apt install libsnappy1v5 libsnappy-dev -y
 sudo apt install liblzma-dev lzma lzma-dev liblzma5 -y
 sudo apt install liblz4-dev liblz4-1 lz4 -y
 sudo apt install libbz2-dev libbz2-1.0 -y
 sudo apt install libzstd-dev zstd -y
 sudo apt install flex

## Compiling
mkdir build-mariadb
cd build-mariadb
cmake ../server -DCMAKE_BUILD_TYPE=Debug
cmake --build .
## Installing
sudo cmake --install .

## Resources
Good video tutorial:
https://www.youtube.com/watch?v=7WGcOTQWAco
