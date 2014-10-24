# RabbitMQ OCF script

Hard to reassemble RabbitMQ cluster

Each Rabbit node tries to connect to previous queue Master

<Picture here>
==============

OCF script:

-	pseudo-Master/Slave
-	Start action == beam start
-	overall control is made by pacemaker notifies
-	check if we can start rabbit server app
-	reset rabbit if it stucks and try again
-	check if node is clustered with the 'master'
-	[not yet implemented] rabbit node fencing

Note: While working on rabbitmq resilience we noticed that rabbitmq server behaviour is not always obvious and clear. E.g. in case of 3-node cluster if you do hard reset of the cluster rabbit along with erlang kernel it uses becomes very vulnerable to race conditions and there is not much you can do about it. In order to automate rabbitmq reassemblance we created rabbitmq ocf script that leverages pacemaker master/slave resources and notification mechanism for 'cloned' resources.<There should go a big picture with the flow> What we really do:

-	Fire up beam processes
-	Let Pacemaker elect the master
-	Create 'master' attribute in CIB
-	Start 'master' app - attach 'slave' nodes to the 'master'

Also we are working now on the mechanism of RabbitMQ cluster member fencing because in case of controller failure for sometime Rabbit cluster becomes unavailable as it issues RPC multicall with rather big timeout which hangs for a while waiting for the dead node to answer.
