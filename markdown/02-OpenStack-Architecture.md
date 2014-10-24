# OpenStack Architecture

![OpenStack Diagram](images/openstack-arch.svg) <!-- class="fragment" -->

Note:
Here is a classical diagram of OpenStack and its loosely coupled components.

What is the main problems with all these componts? 

You should organize High Availability for all of these components. So, in reality you should have at least 2 copies of these elements. However, I would suggest to have at least 3 copies to eleminate split brain scenarios you may have.

All components should be ready for High Availability. Some of them might be under arbitrary control, Some of them may have own healing mechanisms.
