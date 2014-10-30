# Current challenges and plans for future

- Multi writes for [Galera](http://lists.openstack.org/pipermail/openstack-dev/2014-May/035344.html)
- OSLO.db resilience
- VRRP and DVR implementations and testing
- Fencing Support
- Event Driven failover and evacuations
- Sophisticated load balancing of services
- monit for compute nodes
- python-memcached fixes
- zeroMQ researches

Note: Speaker - Sergii Golovatiuk:

Multi writes:

With Galera Cluster, HAproxy still performs read/write operations to one server. OpenStack has SELECT ... FOR UPDATE queries. When 2 servers have write operation simultaneously, one of the servers will revert its own transaction, perform its neighbor's transaction, then repeat its DB transaction again. OpenStack Services should be aware of such manipulations. We work with OSLO.db developers to resolve these issues to have better balancing. Our OCF script is a simple clone base. We have plans to make it as master-slave, allowing OpenStack Cloud operators to see the status from **pcs** or **crm_mon**Our OCF script is a simple clone base. We have plans to make it as master-slave, allowing OpenStack Cloud operators to see the status from **pcs** or **crm_mon**. On deploytment side we want to introduce garbd to have 2-node cluster installation.

OSLO.db resilience:
OSLO.db still doesn't have good mechanism for transaction rollback.

Speaker - Vladimir Kuklin:
VRRP and DVR:
We want to handle the reschedule triggered by pacemaker and OCF along with utilizing VRRP mechanism for L3 agents. As you know VRRP has been merged.

Node Fencing:
Also we are working now on the mechanism of RabbitMQ cluster member fencing because in case of controller failure for sometime Rabbit cluster becomes unavailable as it issues RPC multicall with rather big timeout which hangs for a while waiting for the dead node to answer.
