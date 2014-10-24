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

Network Connectivity. 

Out of the box we provide several options such as Active-Passive Connection or more advanced LACP (802.1ad) network connection. However, we still have separate connection for PXE network booting.

DataBase - MySQL

AMQP - Rabbit

Memcache

Storage

API Services

Neutron/Heat/Ceilometer
