# Main deployment <br /> components

-	Corosync/Pacemaker manifests
-	Haproxy with conf.d patch + manifests
-	NS contained resources

Note:

In order to get all of the aforementioned components set up and deployed we added or modified several deployment pieces and modules along with other software.

These were:

1.	Puppet-corosync module originally created by puppetlabs and modified by us
2.	We also needed Haproxy version that supports configuration directory includes in order to inject changes to haproxy in granular manner
3.	Modified VIP and Haproxy OCF resources to contain haproxy and IP addresses inside specific namespaces


# Corosync/pacemaker manifests: part 1

-	Not all resource types supported:
    -	Constraints (e.g. location) support
    -	Master/slave resources not supported
-	Puppet service type provider for pacemaker:
    - Parses LRM of alive nodes
    - Waits for status change with respect to timeouts
    - Handles timeouts depending on defaults or user-specified values
-	[5.1 release] Shadow approach broken => Moved to xml patches instead

Note:

First of all, we needed to polish some of the corosync module code that we had at the time we forked it. We needed additional support for other pacemaker resources/entities such as location constraints and master/slave resources. This code was not in place so we wrote it. Then, in order to deploy pacemaker resources and to maintain almost the same puppet code we needed to implement pacemaker service provider for puppet. It parses output of Pacemaker Local Resource Managers in CIB respecting timeout values and monitor commands. If it cannot derive user-specified values for timeouts, it parses OCF script metadata to get this information. Also in order to support complex OCF scripts such as scripts for Galera and RabbitMQ we had to abandon default upstream 'shadow'-like approach as it was sometimes overwriting cluster attributes during deployment changes and this desynchronization led to deployment failures. So we leveraged pacemaker support for XML diff CIB modification and rewrote all the providers to support it. Additionally, maintaining shadows was a real headache as we needed a strict sequential order of shadow+commit resource pairs in order not to overwrite CIB populated with other resources data.


# Corosync/pacemaker manifests: part 2 - stability

-	[5.1 release] asymmetric cluster:
	-	services stopped everywhere by default
		-	enabled by 0 location constraint ("unbanning")
		-	clones started locally
		-	primitives started only once

Note:

Our initial implementation of this provider was not perfect as it was triggering restarts not only for the services on the particular node being deployed but globally, e.g. for clone resources each resource was restarted. So we switched to asymmetric pacemaker cluster which does not start services by default. Then we refactored service provider to perform actions locally using pacemaker location constraints for start and stop actions. E.g. to enable a service somewhere we set zero location score for the particular node which we call "unbanning" of the node. In order to make service actions local-wise we altered status method behaviour depending on the type of resource. If resource is a primitive, then we check if it is already started in the cluster and if it is true, we "unban" it and silently exit. If it is false, we wait for status change. If it is a clone or master/slave resource then we check local status of the resource do ban/unban and wait for status to change


# Corosync/pacemaker manifests: part 3 - HA scalability

- Multicast problems =>
  switched to unicast by default 
- Need to alter and restart corosync => pacemaker maintenance mode
- Galera transitional sync => Limit on parallel controllers

Note:

So as soon as we won a fight for deployment stability we moved to scalability and here is what we faced: we received initial feedback from our services guys that most of our customers do not have multicast enabled or correctly configured. This made us switch deployment to unicast one by default. And this was what really saddened us because we had to alter corosync configuration and restart it each time we wanted to add a new controller. In order to make it work we modified init scripts for corosync to check for pacemaker and put it into maintenence mode. Also, we needed to limit amount of parallel controllers being deployed in order not to exhaust donor nodes and affect working environment.
