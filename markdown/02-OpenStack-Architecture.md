# OpenStack Architecture

<img src="images/openstack-arch.svg" alt="OpenStack Architecture" style="background: rgba(255, 255, 255, 1);">

Note: Speaker - Golovatiuk Sergii:

Here is a classical diagram of OpenStack and its loosely coupled components.

What are the main problems with each of these components? 

You should organize High Availability for all of them. So, in reality, you should have at least 2 copies of each component. However, I would suggest to have at least 3 copies to eliminate split brain scenarios that may arise.

All components should be ready for High Availability. Some of them might be under arbitrary control such as Pacemaker or Zookeper, some of them may have own healing mechanisms.
