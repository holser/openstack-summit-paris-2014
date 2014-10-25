# OpenStack HA Stack
Fault Tolerance of every component

- Network Connectivity <!-- .element: class="fragment" -->
- Database - MySQL <!-- .element: class="fragment" -->
- AMQP - RabbitMQ <!-- .element: class="fragment" -->
- Memcache <!-- .element: class="fragment" -->
- Storage - Ceph <!-- .element: class="fragment" -->
- API Services <!-- .element: class="fragment" -->
- Neutron/Heat/Ceilometer <!-- .element: class="fragment" -->

Note: To Provide High Availability to a system. We should provide connectivity to all components. No brainer. We will not cover High Availability for DataCenter components such as Power, Cooling. All these requirements are well described in Uptime Insitute Documents. 

Our primary focus is OpenStack Controllers and Compute nodes. So ...

Network Connectivity: 

Out of the box we provide several options such as Active-Passive Connection or more advanced LACP (802.1ad) network connection. However, we still have separate connection for PXE network booting.

DataBase - MySQL:

We started working on DB HA with galera and achieved good results by optimizing management of Galera cluster with pacemaker.

AMQP - Rabbit:

AMQP is a crucial part of OpenStack architecture. Death of the particular controller should not affect availability of service. We had to modify both deployment mechanism of rabbitmq and OpenStack messaging code to make it work.

Memcache:

Memcache is We need also be sure that all the services using memcached retain ability to serve requests in case of memcached server death.

Storage

For storage availability one can choose enterprise solutions or fault-tolerant software-defined storages. We concentrated our efforts on Ceph.

API Services

API services should be load-balanced and requests should be redirected to the node that can actually serve requests.

Neutron/Heat/Ceilometer

For Neutron agents we need also to implement HA solution that migrates assigned entities and retains networking connectivity. Also, we need to ensure that heat and ceilometer agents migrate to alive controllers.
