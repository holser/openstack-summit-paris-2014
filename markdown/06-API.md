API Services and services engines
=================================
All API requests go through active/active haproxy
Service engines are managed by pacemaker and ocf scripts
HaProxy with VIP is in separate namespace

Note:

There is not a lot of interesting stuff how we are balancing API requests. We are using regular haproxy with active-active backends. Some of the parameters are tuned to digest production workloads. Regarding service engines, such as heat and ceilometer, we are managing them using regular ocf scripts. HaProxy itself is running in separate namespace to avoid hanging connections problems when you migrate IPs between controllers.
