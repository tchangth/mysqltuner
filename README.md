# mysqltuner
How to install mysqltuner on mariadb 10 

Use MySQLTuner to tune your vps when run low of memory (1 gbyte or less). Remember to do systemctl restart mariadb once you made a configuration on mysql:

**MySQLTuner**

1. If you install mariadb-server on Ubuntu change your drive to /etc/mysql/mariadb.conf.d

2. nano 50-server.cnf (just below bind-address, input the performance parameters)

​	bind-address      = 127.0.0.1

 

# Performance Schema

performance_schema=ON

performance-schema-instrument='stage/%=ON'

performance-schema-consumer-events-stages-current=ON

performance-schema-consumer-events-stages-history=ON

performance-schema-consumer-events-stages-history-long=ON

3. If you are in a rush with just 1 gbyte RAM, and you did not intend to do the analysis, in the same file input the following parameter (must be below [mysqld]):

\# this is only for the mysqld standalone daemon

[mysqld]

max_connections = 15 (this is the result of mysqltuner)

(shown below as to how 15 is to be. the default is 151 connections)

4. cd /root 

  git clone https://github.com/grooverdan/mariadb-sys 

mariadb-sys/views/p_s/metrics_56.sql seems to be the problem but Dan version is ok)

 



 <img width="784" alt="metrics56" src="https://user-images.githubusercontent.com/11190418/175762897-49d35997-28a1-4d0d-8237-9f6040f1a56b.png">

curl "https://codeload.github.com/FromDual/mariadb-sys/zip/master" > master.zip (this will break the view at the column 78 which is CREATE or REPLACE

 

5. create a mysqltuner directory

  	wget http://mysqltuner.pl/ -O mysqltuner.pl

​	chmod +x mysqltuner.pl

​	./mysqltuner.pl > result.txt

 The result.txt is shown here:

\>> MySQLTuner 2.0.4

​        \* Jean-Marie Renouard <jmrenouard@gmail.com>

​        \* Major Hayden <major@mhtx.net>

 \>> Bug reports, feature requests, and downloads at http://mysqltuner.pl/

 \>> Run with '--help' for additional options and output filtering

 [--] Skipped version check for MySQLTuner script

[OK] Logged in using credentials from Debian maintenance account.

[OK] Currently running supported MySQL version 10.5.15-MariaDB-0+deb11u1

[OK] Operating on 64-bit architecture

 -------- Log file Recommendations ------------------------------------------------------------------

[!!] Log file doesn't exist

-------- Storage Engine Statistics -----------------------------------------------------------------

[--] Status: +Aria +CSV +InnoDB +MEMORY +MRG_MyISAM +MyISAM +PERFORMANCE_SCHEMA +SEQUENCE 

[--] Data in InnoDB tables: 4.4M (Tables: 68)

[OK] Total fragmented tables: 0

 -------- Analysis Performance Metrics --------------------------------------------------------------

[--] innodb_stats_on_metadata: OFF

[OK] No stat updates during querying INFORMATION_SCHEMA.

 -------- Views Metrics -----------------------------------------------------------------------------

 -------- Triggers Metrics --------------------------------------------------------------------------

 -------- Routines Metrics --------------------------------------------------------------------------

 -------- Security Recommendations ------------------------------------------------------------------

[OK] There are no anonymous accounts for any database users

[OK] All database users have passwords assigned

[!!] There is no basic password file list!

-------- CVE Security Recommendations --------------------------------------------------------------

[--] Skipped due to --cvefile option undefined

-------- Performance Metrics -----------------------------------------------------------------------

[--] Up for: 15h 40m 57s (101K q [1.805 qps], 2K conn, TX: 326M, RX: 15M)

[--] Reads / Writes: 99% / 1%

[--] Binary logging is disabled

[--] Physical Memory   : 976.4M

[--] Max MySQL memory  : 700.7M

[--] Other process memory: 0B

**[--] Total buffers: 417.0M global + 18.9M per thread (15 max threads)**

[--] P_S Max memory usage: 72B

[--] Galera GCache Max memory usage: 0B

[OK] Maximum reached memory usage: 662.8M (67.89% of installed RAM)

[OK] Maximum possible memory usage: 700.7M (71.76% of installed RAM)

[OK] Overall possible memory usage with other process is compatible with memory available

[OK] Slow queries: 0% (0/101K)

[!!] Highest connection usage: 86% (13/15)

[OK] Aborted connections: 0.00% (0/2918)

[!!] Name resolution is active: a reverse name resolution is made for each new connection and can reduce performance

[OK] Query cache is disabled by default due to mutex contention on multiprocessor machines.

[OK] Sorts requiring temporary tables: 0% (0 temp sorts / 6 sorts)

[OK] No joins without indexes

[OK] Temporary tables created on disk: 0% (94 on disk / 84K total)

[OK] Thread cache hit rate: 99% (13 created / 2K connections)

[OK] Table cache hit rate: 98% (111K hits / 112K requests)

[OK] table_definition_cache (400) is greater than number of tables (348)

[OK] Open file limit used: 0% (56/32K)

[OK] Table locks acquired immediately: 100% (173 immediate / 173 locks)

-------- Performance schema ------------------------------------------------------------------------

[--] Performance_schema is activated.

[--] Memory used by P_S: 72B

[--] Sys schema is installed.

 -------- ThreadPool Metrics ------------------------------------------------------------------------

[--] ThreadPool stat is disabled.

 -------- MyISAM Metrics ----------------------------------------------------------------------------

[--] No MyISAM table(s) detected ....

-------- InnoDB Metrics ----------------------------------------------------------------------------

[--] InnoDB is enabled.

[--] InnoDB Thread Concurrency: 0

[OK] InnoDB File per table is activated

[OK] InnoDB buffer pool / data size: 128.0M/4.4M

[!!] Ratio InnoDB log file size / InnoDB Buffer pool size (75%): 96.0M * 1/128.0M should be equal to 25%

[--] Number of InnoDB Buffer Pool Chunk: 1 for 1 Buffer Pool Instance(s)

[OK] Innodb_buffer_pool_size aligned with Innodb_buffer_pool_chunk_size & Innodb_buffer_pool_instances

[OK] InnoDB Read buffer efficiency: 98.72% (62369 hits/ 63176 total)

[!!] InnoDB Write Log efficiency: 363.64% (1160 hits/ 319 total)

[OK] InnoDB log waits: 0.00% (0 waits / 1479 writes)

-------- Aria Metrics ------------------------------------------------------------------------------

[--] Aria Storage Engine is enabled.

[OK] Aria pagecache size / total Aria indexes: 128.0M/336.0K

[OK] Aria pagecache hit rate: 95.1% (612 cached / 30 reads)

-------- TokuDB Metrics ----------------------------------------------------------------------------

[--] TokuDB is disabled.

 -------- XtraDB Metrics ----------------------------------------------------------------------------

[--] XtraDB is disabled.

-------- Galera Metrics ----------------------------------------------------------------------------

[--] Galera is disabled.

 -------- Replication Metrics -----------------------------------------------------------------------

[--] Galera Synchronous replication: NO

[--] No replication slave(s) for this server.

[--] Binlog format: MIXED

[--] XA support enabled: ON

[--] Semi synchronous replication Master: OFF

[--] Semi synchronous replication Slave: OFF

[--] This is a standalone server

 -------- Recommendations ---------------------------------------------------------------------------

General recommendations:

  MySQL was started within the last 24 hours - recommendations may be inaccurate

  Reduce or eliminate persistent connections to reduce connection usage

  Configure your accounts with ip or subnets only, then update your configuration with skip-name-resolve=1

  Before changing innodb_log_file_size and/or innodb_log_files_in_group read this: https://bit.ly/2TcGgtU

Variables to adjust:

  max_connections (> 15)

  wait_timeout (< 28800)

  interactive_timeout (< 28800)

  skip-name-resolve=1

  innodb_log_file_size should be (=32M) if possible, so InnoDB total log files size equals 25% of buffer pool size.
