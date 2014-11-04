# Current challenges and plans for future

- multiple writes for [Galera](http://lists.openstack.org/pipermail/openstack-dev/2014-May/035344.html)
- oslo.db resilience
- VRRP and DVR implementations and testing
- fencing Support
- event-driven failover and evacuations
- memcached-related fixes
- zeroMQ research
- gate for HA tests

Note: Speaker - Sergii Golovatiuk:

Multiple writes:
With Galera Cluster, HAproxy still performs read/write operations to one server. OpenStack components has SELECT ... FOR UPDATE queries. When 2 servers have write operation simultaneously, one of the servers will revert its own transaction, perform its neighbor's transaction, then repeat its DB transaction again. OpenStack Services should be aware of such manipulations. We work with OSLO.db developers to resolve these issues. We have plans to make OCF script master-slave based, allowing OpenStack Cloud operators to see status from **pcs** or **crm_mon**. 

Speaker - Vladimir Kuklin:
We want to handle the reschedule triggered by pacemaker and OCF along with utilizing VRRP and DVR mechanism for L3 agents. 

Node Fencing:
We are working on configuration of fencing for pacemaker cluster nodes along with RabbitMQ cluster member fencing.

Event driven failover:

We also want to have centralized view of the whole cluster to start failover before actual failure happens. One of the common examples are triggers in conventional monitoring systems

Memcached-related fixes:

We need to fix python-memcached library along with memcached driver implementation for horizon.

ZeroMQ research:

Despite some big cloud installations show that zeromq may be a useful replacement for messaging, current oslo.messaging driver for zeromq is in a poor state. So we are going to research whether it maybe applicable for HA-enabled production ready installations.

HA tests:

As we already have a well working testing framework for highly available deployment and we are really close to provide ability for community FUEL ISO to deploy vanilla openstack from particular commits, we are going to add HA tests gating to OpenStack jenkins to indicate whether each particular commit is not affecting HA.
