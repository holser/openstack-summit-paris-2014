Neutron
=======

-	API under HAproxy
-	Managing Neutron agents
	-	Clean everything on stop start actions:
		-	Interfaces
			-	Child Processes (dnsmasq, metadata proxy)
	-	Entities rescheduling after migration
	-	WIP: API handler and rescheduling using internal Neutron mechanism

<Picture here>

Note:

Managing neutron services is pretty easy when we are talking about API. Devil is in details when you want to safely manage agents. First of all, when you start and stop agents, you need to ensure that they do not leave any artifacts that can affect connectivity, such as orphaned interfaces along with IP addresses. In order to achieve this, we do special cleanup actions that destroy previously created interfaces on the node. The next thing you need to do is rescheduling of entities, such as routers for l3 agent and networks for dhcp agent after you do migration of the agent to another node, for example, in case of failover. Current version utilizies Neutron API calls for L3 and dhcp agents. Our Neutron community team is working on moving this functionality to Neutron core. Currently, there is some code for automatic rescheduling, but it showed some issues with AMQP failover, so our guys are working on eliminating this issue. In future, we want to make this rescheduling triggered by pacemaker and OCF along with utilizing VRRP mechanism for L3 agents.
