API Services and services engines
=================================
- All API requests go through active/active HAproxy
- Service engines are managed by pacemaker and ocf scripts
- HAproxy with VIP is in separate namespace using veth pairs and arp_proxy.

Note:

There is not much interesting to say about how we are balancing API requests. We are using standard HAproxy with active-active backends. Some of the parameters are tuned to digest production workloads. Regarding service engines, such as heat and ceilometer, we are managing them using dedicated ocf scripts. HAproxy itself is running in a separate network namespace to avoid hanging connection problems when migrating service endpoint IPs between controllers. To achieve this, we used the common veth+arp_proxy approach, along with the addition of NAT rules to retain connectivity with all networking services.
