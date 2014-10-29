# Galera Diagram 
![Galera](images/galera.png)


# Database - MySQL/Galera
- Clone-based
- Use latest GTID info for master election:
    - From Galera itselves
    - From grastate.dat
- Start master with empty gcomm://
- MySQL 5.6.11
- XtraBackup
- HaProxy + xinetd httpcheck
- [not yet done] real master-slave for operators convenience
- Optimistic locking and OSLO.db clash

Note: 
Why Galera? Sometimes, I hear that Galera is too complex. DRBD is an easy solution which is enough for reliability of Database. Firstly, DRBD block replication is not a native mechanism for MySQL. Secondly, you cannot scale with DRBD.

What about master-slave?

Master-Slave is well known and good technology. However, at the moment there is no big difference between Galera and master-slave replication in terms of reliability.

OCF script:
Our previous implementation of OCF script was fragile and didn't reassemble the cluster in many conditions. The new version was rewritten from scratch **link** which allows is to bring the cluster back without human interruptions.

The general idea is to keep data from grastate to Pacemaker and use this data to find the most up-to-date server for Primary Component. If no data or it's corrupted, we try to restore data from grastate.dat

MySQL Server was upgraded to 5.6.11 with the latest Galera which resolved many stability issues.
Mysqldump was replaced with xtrabackup from Percona. Firstly, that

HAProxy was extended to perform simple checks against the database and to avoid performing any DB operations against Donor/Desynced servers.

Problems:
HAproxy still performs write operations to one server. OpenStack services are not ready. It's a distingushing characteristic of how Galera works. When 2 servers have write operation simultaneously, one of the servers will revert its own transaction, perform its neighbor's transaction, then repeat its DB transaction again. OpenStack Services should be aware of such manipulations.

We work with Oslo developers on multiple write resolution, as it will enable spread DB write load across all controllers in the cluster.

We don't have garbd to have 2-node cluster.

Our OCF script is a simple clone base. We have plans to make it as master-slave, allowing OpenStack Cloud operators to see the status from **pcs** or **crm_mon**
