# Galera/MySQL 
![Galera](images/galera.png)

Note: Speaker - Golovatiuk Sergii:

Why Galera? Sometimes, I hear that Galera is too complex. DRBD is an easy solution which is enough for reliability of Database. Firstly, DRBD block replication is not a native mechanism for MySQL. Secondly, you cannot scale with DRBD.

What about master-slave?

Master-Slave is well known good technology. However, at the moment there is no big difference between Galera and master-slave replication in terms of OpenStack reliability.

Here is a classical diagram of Galera Implementation in Fuel. All services communicates with MySQL/Galera cluster via HAProxy. High Availability of HAProxy is based on Virtual IP, which is controlled by Pacemaker. As you can see that only one HAProxy instance serves read/write operations for MySQL. The problem was described by Peter Boros @ Percona as many OpenStack Services use SELECT ... FOR UPDATE SQL queries or don't have special functioninality to perform retry on failed SQL transactions.


# MySQL/Galera improvements
- MySQL 5.6.11
- Latest XtraBackup
- HaProxy + xinetd httpcheck

Note: Speaker - Sergii Golovatiuk:

MySQL Server was upgraded to 5.6.11 with the latest Galera. That resolved many stability issues.
Mysqldump State Snapshot Transfer (SST) was replaced with xtrabackup from Percona. Xtrabackup doesn't lock database during SST. Also it has good performance allowing to synchronize large databases.

HAProxy was extended to perform simple checks against the database and to avoid performing any DB operations against Donor/Desynced servers.


# MySQL/Galera - OCF script
- Use latest GTID info for master election:
    - From CIB
    - From grastate.dat
- Start PC with empty gcomm://
- Clone-based

Note: Speaker - Sergii Golovatiuk:

Our previous implementation of OCF script was fragile and didn't reassemble the cluster in many conditions. The new version was rewritten from scratch **link** which allows to bring the Galera cluster back online without any interruptions.

The general idea is to select Primary Component with the most recent data. OCF script gets the most recent GTID and keeps values in CIB. In case of problems OCF script gets the data from grastate file allowing to bootstrap cluster. Pacemaker uses this data to find the most up-to-date server for Primary Component election. In case of scenario where all Controllers are down, Pacemaker waits for neighbours for 5 minutes. If neighbours are stuck on fsck or grub prompt, Pacemaker will start with all available nodes.

On monitor function, OCF script finds the cases when node went out of sync. Though, it correctly finds Donor/Desync state allowing nodes to perform State Snapshot Transfer (SST)

OCF script is currently clonned based so we don't need to create primitive for every node.
