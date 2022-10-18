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
`sudo apt install libpthread-stubs0-dev`<br />

## Compiling

`mkdir build-mariadb` <br />
`cd build-mariadb` <br />
`cmake ../server -DCMAKE_BUILD_TYPE=Debug` <br />
`cmake --build .` <br />

## Installing

`sudo cmake --install .`

## Resources
### Initial compilation

Good video tutorial: <br />
https://www.youtube.com/watch?v=7WGcOTQWAco

### Running mariadb from the build directory

To run mariadb from the build directory, use the instructions at this link (I have added my own notes here of changes I made to make things work):

https://mariadb.com/kb/en/running-mariadb-from-the-build-directory/

For my own case, in the file `~/.my.cnf` I made the following changes:

```
# Where you want to have your database
datadir=/home/lottaquestions/work/MariaDBServer/data

# Where you have your mysql/MariaDB source + sql/share/english
language=/home/lottaquestions/work/MariaDBServer/build-mariadb/sql/share/english
```

With the `~/.my.cnf` file in place, go to your MariaDB source directory and execute:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb$ ./scripts/mysql_install_db --srcdir=/home/lottaquestions/work/MariaDBServer/server --datadir=/home/lottaquestions/work/MariaDBServer/data --user=$LOGNAME`

You should see the below output:

```
Installing MariaDB/MySQL system tables in '/home/lottaquestions/work/MariaDBServer/data' ...
OK
```


Even though I was able to launch the server in ddd using the below command (see the database being launched is the `test` database), it did not seem to work right, it went into some kind of continuation loop and ddd became unresponsive:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb/sql$ ddd ./mariadbd`

However, I was able to run it in gdb successfully:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb/sql$ gdb ./mariadbd`

Once the server is started in gdb using the `run` command, a client can now connect to the server using the below command:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb$ /home/lottaquestions/work/MariaDBServer/build-mariadb/client/mysql test`

See sample output below of sample commands that were run against the test database:

```
lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb$ /home/lottaquestions/work/MariaDBServer/build-mariadb/client/mysql test
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.11.0-MariaDB-debug Source distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [test]> create table t1(n int);
Query OK, 0 rows affected (4.829 sec)

MariaDB [test]> insert into t1 values(345);
Query OK, 1 row affected (32.936 sec)

```
### Running mariadb from the build directory with rocksdb enabled

Source https://mariadb.com/kb/en/building-myrocks-in-mariadb/
Additional source https://mysqlnlinux.blogspot.com/2017/05/how-to-install-myrocks-into-mariadb-as.html

In the directory `~/work/MariaDBServer`, I created the file `my_rocksdb.cnf`, which had the following contents:

```
[mysqld]

datadir=../mysql-test/var/install.db
plugin-dir=../storage/rocksdb
language=./share/english
socket=/tmp/mysql.sock
port=3307

plugin-load=ha_rocksdb
default-storage-engine=rocksdb

```

Once the `my_rocksdb.cnf` files was created, I ran the following commands to start up the server with rocksdb:

```
~/work/MariaDBServer/build-mariadb
(cd mysql-test; ./mtr alias)
cp -r mysql-test/var/install.db ../data4rocksdb/ # Lottaquestions: I do not think this step is necessary
cd ../sql
./mysqld --defaults-file=~/work/MariaDBServer/my_rocksdb.cnf
```

I then had to connect to the database as root as follows:

```
sudo /home/lottaquestions/work/MariaDBServer/build-mariadb/client/mysql test
```

I was able to run the mariadb with rocksdb setup in valgrind's callgrind, and even had callgrind trace child processes as follows:

```
valgrind --trace-children=yes --tool=callgrind /home/lottaquestions/work/MariaDBServer/build-mariadb/sql/mysqld --defaults-file=~/work/MariaDBServer/my_rocksdb.cnf
```

## Testing Code Discovery and Debugging

To add a test, got to the mysql-test subdirectory in the source tree and in the main subdirectory, add your test in a file with a ".test" extension.
For example, `example.test` can be created that has the following contents:

`select 1;`

Execute the following command to create the master result file for you test file. Note that the ".test" extension is ommitted:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb/mysql-test$ ./mariadb-test-run --record example`

Execute the following command to load MySQL server into gdb in a separate xterm window for the "example" test case:

`lottaquestions@dawn-Inspiron-3668:~/work/MariaDBServer/build-mariadb/mysql-test$ ./mariadb-test-run --gdb example`

An xterm window will open with a gdb prompt inside. Set the below breakpoint:

```
(gdb) b mysql_parse
Breakpoint 1 at 0xa01477: file /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc, line 7962.
```

Next start the mariadb process as shown below:

```
(gdb) run
Starting program: /home/lottaquestions/work/MariaDBServer/build-mariadb/sql/mariadbd --defaults-group-suffix=.1 --defaults-file=/home/lottaquestions/work/MariaDBServer/build-mariadb/mysql-test/var/my.cnf --log-output=file --core-file --loose-debug-sync-timeout=300 --loose-debug-gdb --loose-skip-stack-trace < /dev/null
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
2022-09-12 14:26:57 0 [Note] /home/lottaquestions/work/MariaDBServer/build-mariadb/sql/mariadbd (server 10.11.0-MariaDB-debug-log) starting as process 25795 ...

```

The breakpoint that was previously set will be hit, and you can now do fun things like looking at the stack trace and setting other breakpoints:

```
Thread 6 "mariadbd" hit Breakpoint 1, mysql_parse (thd=0x7fffe0000dc8, rawbuf=0x7fffe0013cb8 "SHOW SLAVE STATUS", length=17, parser_state=0x7ffff1d022e0) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:7962
7962      DBUG_ENTER("mysql_parse");
(gdb) bt
#0  mysql_parse (thd=0x7fffe0000dc8, rawbuf=0x7fffe0013cb8 "SHOW SLAVE STATUS", length=17, parser_state=0x7ffff1d022e0) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:7962
#1  0x0000555555f41317 in dispatch_command (command=COM_QUERY, thd=0x7fffe0000dc8, packet=0x7fffe000bb19 "SHOW SLAVE STATUS", packet_length=17, blocking=true) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:1894
#2  0x0000555555f3fc6b in do_command (thd=0x7fffe0000dc8, blocking=true) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:1407
#3  0x000055555612a209 in do_handle_one_connection (connect=0x555558ae0368, put_in_cache=true) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_connect.cc:1418
#4  0x0000555556129e96 in handle_one_connection (arg=0x555558ae0368) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_connect.cc:1312
#5  0x00005555566a52ce in pfs_spawn_thread (arg=0x555558abe5a8) at /home/lottaquestions/work/MariaDBServer/server/storage/perfschema/pfs.cc:2201
#6  0x00007ffff753db43 in start_thread (arg=<optimized out>) at ./nptl/pthread_create.c:442
#7  0x00007ffff75cfa00 in clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:81
(gdb) 

```

### Running mariadb in valgrind using the callgrind tool

Running mariadb in valgrind using the callgrind tool, especially when targeting execution of certain paths:
1. Launch valgrind with the callgrind tool, but with stats collection set to be off:

```
cd ~/work/MariaDBServer/build-mariadb/sql
valgrind --tool=callgrind --instr-atstart=no ./mariadbd
```

2. Turn on stats collection when ready to profile a specific section of the server:

```
callgrind_control -i on
```

3. Run the operation that you need to profile.

4. After running the operation, stop the profiling:

```
callgrind_control -i off
```

5. Dump the collected profiling statistics:

```
callgrind_control -d
```

6. Copy out the generated statistics files to a different directory.

7. Zero out the collected statistics, and then go back to step 2 to profile the next operation:

```
callgrind_control -z
```


And below is the gdb stack trace during an insert:

```
(gdb) b mysql_parse
Breakpoint 1 at 0xa01477: file /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc, line 7962.
(gdb) run
Starting program: /home/lottaquestions/work/MariaDBServer/build-mariadb/sql/mariadbd 
....
(gdb) b mysql_insert
Breakpoint 2 at 0x555555eefe10: file /home/lottaquestions/work/MariaDBServer/server/sql/sql_insert.cc, line 696.

Thread 14 "mariadbd" hit Breakpoint 2, mysql_insert (thd=0x7fffb4000dc8, table_list=0x7fffb4014300, fields=..., values_list=..., update_fields=..., update_values=..., duplic=DUP_ERROR, ignore=false, result=0x0) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_insert.cc:696
696	{
(gdb) bt
#0  mysql_insert (thd=0x7fffb4000dc8, table_list=0x7fffb4014300, fields=..., values_list=..., update_fields=..., update_values=..., duplic=DUP_ERROR, ignore=false, result=0x0)
    at /home/lottaquestions/work/MariaDBServer/server/sql/sql_insert.cc:696
#1  0x0000555555f49726 in mysql_execute_command (thd=0x7fffb4000dc8, is_called_from_prepared_stmt=false) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:4563
#2  0x0000555555f55710 in mysql_parse (thd=0x7fffb4000dc8, rawbuf=0x7fffb4014220 "insert into t1 values(345)", length=26, parser_state=0x7ffff41512e0)
    at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:8037
#3  0x0000555555f41317 in dispatch_command (command=COM_QUERY, thd=0x7fffb4000dc8, packet=0x7fffb400ba09 "insert into t1 values(345)", packet_length=26, blocking=true)
    at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:1894
#4  0x0000555555f3fc6b in do_command (thd=0x7fffb4000dc8, blocking=true) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_parse.cc:1407
#5  0x000055555612a209 in do_handle_one_connection (connect=0x5555588aced8, put_in_cache=true) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_connect.cc:1418
#6  0x0000555556129e96 in handle_one_connection (arg=0x555558b697e8) at /home/lottaquestions/work/MariaDBServer/server/sql/sql_connect.cc:1312
#7  0x00005555566a52ce in pfs_spawn_thread (arg=0x555558a82958) at /home/lottaquestions/work/MariaDBServer/server/storage/perfschema/pfs.cc:2201
#8  0x00007ffff753db43 in start_thread (arg=<optimized out>) at ./nptl/pthread_create.c:442
#9  0x00007ffff75cfa00 in clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:81

```

