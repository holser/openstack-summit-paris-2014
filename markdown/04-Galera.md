# Database - MySQL/Galera

Note: 
Why Galera? Sometimes, I hear that Galera is too complex. DRBD is easy solution which is enough for reliability of Database. Firstly, DRBD block replication is not native mechanism for MySQL. Secondly, you cannot scale with DRBD. 

What about master-slave?

Master-Slave is well known and good technology. However, at the moment there is no big difference between galera and master-slave replication in terms of reliability.

OCF script:
Our previous implementation of OCF script was fragile and didn't reassemble the cluster in many conditions. The new version was rewritten from scratch **link** which allows to bring claster back without human interruptions

The general idea is to keep data from grastate to Pacemaker and use those data to find the most resent server for Primary Component. If no data or it's corrupted we try to restore data from grastate.dat

MySQL Server was upgraded to 5.6.11 with latest galera which resolved many stability issues.
mysqldump was replaced with xtrabackup from percona. Firstly, that

HAProxy was extended to perform simple checks agains database and do not perform any operations against Donor/Desynced servers.

Problems:
Haproxy still performs write operations to one server. OpenStack services are not ready. It's a distingushing how Galera works. When 2 servers have write operation simultaneously. One of the servers should revert own transaction, perform a transaction from neighbour. Services should be aware of such manipulations.

We work with OSLO developers on multiple write resolution, as it will allow to spread DB write load across all controllers in cluster.

We don't have garbd to have 2-node cluster.

Our OCF script is clone base. We have plans to make it as master-slave allowing OpenStack Cloud operations to see the status from **pcs** or **crm_mon**
