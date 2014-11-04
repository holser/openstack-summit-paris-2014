# OpenStack HA Stack
##### Fault Tolerance of every component

- Network Connectivity <!-- .element: class="fragment" -->
- Database - MySQL <!-- .element: class="fragment" -->
- AMQP - RabbitMQ <!-- .element: class="fragment" -->
- Memcached <!-- .element: class="fragment" -->
- Storage - Ceph <!-- .element: class="fragment" -->
- API Services <!-- .element: class="fragment" -->
- Neutron/Heat/Ceilometer <!-- .element: class="fragment" -->

Note: Speaker - Vladimir Kuklin:

To Provide High Availability to a system, we should provide connectivity to all components. No brainer. We will not cover High Availability for DataCenter components such as Power, Cooling, Chilling or Network Connectivity.

Our primary focus is OpenStack Controllers and Compute nodes. So ...

Network Connectivity: 

Out of the box we provide several options, such as Active-Passive Connection or more advanced LACP (802.1ad) network connection. However, we still have a separate connection for PXE network booting.

DataBase - MySQL:

We started working on DB HA with Galera Support. So we achieved very good results by optimizing management of Galera cluster with Pacemaker.

AMQP - Rabbit:

AMQP is a crucial part of OpenStack Architecture. Death of the particular Controller should not affect availability of service. We had to modify both deployment mechanisms of rabbitmq and OpenStack Messaging code to make it work.

Memcached:

Main goal with memcache is to retain the ability to serve requests when a one of memcached servers is dead.

Storage:

For storage availability, one can choose enterprise solutions such as NetUpp or EMC or fault-tolerant Software-Defined Storages. We concentrated our efforts on Ceph.

API Services:

API services should be load-balanced and requests should be redirected to the node that can actually serve API requests.

Neutron/Heat/Ceilometer:

For Neutron agents, we also need to implement High Avalability solution that migrates assigned entities and retains networking connectivity. Also, we need to ensure that Heat and Ceilometer Agents can be migrated to working controllers.
