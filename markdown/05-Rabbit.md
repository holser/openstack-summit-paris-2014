# RabbitMQ

Hard to reassemble RabbitMQ cluster

Each Rabbit node tries to connect to previous queue Master

![rabbit_ocf](images/rabbit_ocf.png)

Note: Speaker - Vladimir Kuklin:

While working on RabbitMQ resilience, we noticed that RabbitMQ server behaviour is not always obvious and clear. E.g. in case of 3-node cluster if you do hard reset of the cluster RabbitMQ along with erlang kernel it usually becomes very vulnerable to race conditions and there is not much you can do about it. 
In order to automate RabbitMQ reassemblance we created OCF script for RabbitMQ that leverages Pacemaker master/slave resources and notification mechanism for *cloned* resources.

-	Fire up beam processes
-	Let Pacemaker elect the master
-	Create 'master' attribute in CIB
-	Start 'master' app - attach *slave* nodes to the *master*
-       Each running rabbit is checked if it is connected to the master node
-       If rabbit app cannot start or join or whatever - reset it
