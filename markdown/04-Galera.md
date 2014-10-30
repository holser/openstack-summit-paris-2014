# Galera/MySQL 
![Galera](images/galera.png)

Note: Speaker - Golovatiuk Sergii:

Why Galera? Sometimes, I hear that Galera is too complex. DRBD is an easy solution which is enough for reliability of Database. Firstly, DRBD block replication is not a native mechanism for MySQL. Secondly, you cannot scale with DRBD.

What about master-slave?

Master-Slave is well known good technology. However, at the moment there is no big difference between Galera and master-slave replication in terms of reliability.
TODO: Explain Diagram


# MySQL/Galera improvements
- MySQL 5.6.11
- XtraBackup
- HaProxy + xinetd httpcheck

Note: Speaker - Sergii Golovatiuk:

MySQL Server was upgraded to 5.6.11 with the latest Galera which resolved many stability issues.
Mysqldump State Snapshot TRansfer (SST) was replaced with xtrabackup from Percona. Xtrabackup doesn't lock database during SST. Also it has really good allowing to synchronize big databases really quick.

HAProxy was extended to perform simple checks against the database and to avoid performing any DB operations against Donor/Desynced servers.


# MySQL/Galera - OCF script
- Start PC with empty gcomm://
- Use latest GTID info for master election:
    - From CIB itselves
    - From grastate.dat
- Clone-based

Note: Speaker - Sergii Golovatiuk:

Our previous implementation of OCF script was fragile and didn't reassemble the cluster in many conditions. The new version was rewritten from scratch **link** which allows to bring the Galera cluster back online without human interruptions.

The general idea is to select Primary Component with the most recent data. OCF script gets latest GTID and keep values in CIB. In case of problems OCF script gets the data from grastate file allowing to bootstrap cluster. Pacemaker uses this data to find the most up-to-date server for Primary Component election. In case of scenario where all Controllers are down, Pacemaker waits for neighbours for 5 minutes. If neighbours are stuck on fcsk or grub prompt Pacemaker will start with all available nodes.

On monitor function, OCF script finds the cases when node went out of sync. Though, it correctly finds Donor/Desync state allowing nodes to perform State Snapshot Transfer (SST)

OCF script is currently clonned based so we don't need to create primitive for every node.
