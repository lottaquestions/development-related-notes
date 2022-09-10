# General notes on the development workflow of MariaDB

## Dependencies when installing MariaDB in Ubuntu
`sudo apt install gnutls-dev`<br />
`sudo apt install gzip		`<br /> 
`sudo apt install bison	`<br /> 
`sudo apt install bison-dev`<br />
`sudo apt install libevent-dev`<br />
`sudo apt install cmake 	   `<br />
`sudo apt install openssl	   `<br />
`sudo apt install libjemalloc-dev libjemalloc2 -y`<br />
`sudo  apt install zlib1g						  `<br />
`sudo apt install zlib1g-dev					  `<br />
`sudo apt install valgrind						  `<br />
`sudo apt install kcachegrind					  `<br />
`sudo apt install massif-visualizer			  `<br />
`sudo apt install curl							  `<br />
`sudo apt -y install libncurses5				  `<br />
`sudo apt -y install libncurses5-dev			  `<br />
`sudo apt install libboost-dev libboost -y		  `<br />
`sudo apt install libboost-dev -y				  `<br />
`sudo apt install -y  libboost-doc libboost1.71-doc libboost-atomic1.71-dev libboost-chrono1.71-dev libboost-container1.71-dev libboost-context1.71-dev libboost-contract1.71-dev libboost-coroutine1.71-dev`<br />
`sudo apt install libboost-all-dev`<br />
`sudo apt install pmem			   `
`sudo apt install bzip2		   `<br />
`sudo apt install lz4			   `<br />
`sudo apt install LibLZMA		   `<br />
`sudo apt install liblzma-dev -y  `<br />
`sudo apt install lzo			   `<br />
`sudo apt install liblzo2-2 liblzo2-dev -y`<br />
`sudo apt install libpmem-dev libpmem1 libpmem1-debug -y`<br />
`sudo apt install libxml2 libxml2-dev -y				 `<br />
`sudo apt install libsnappy1v5 ibsnappy-dev -y			 `<br />
`sudo apt install libsnappy1v5 libsnappy-dev -y		 `<br />
`sudo apt install liblzma-dev lzma lzma-dev liblzma5 -y`<br />
`sudo apt install liblz4-dev liblz4-1 lz4 -y			`<br />
`sudo apt install libbz2-dev libbz2-1.0 -y				`<br />
`sudo apt install libzstd-dev zstd -y					`<br />
`sudo apt install flex									`<br />
`sudo apt-get install -y libjudy-dev`<br />

## Compiling

`mkdir build-mariadb` <br />
`cd build-mariadb` <br />
`cmake ../server -DCMAKE_BUILD_TYPE=Debug` <br />
`cmake --build .` <br />

## Installing

`sudo cmake --install .`

## Resources
Good video tutorial: <br />
https://www.youtube.com/watch?v=7WGcOTQWAco


