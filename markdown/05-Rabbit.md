# RabbitMQ

- Hard to reassemble RabbitMQ cluster
- Each Rabbit node tries to connect to previous queue Master
<br /> <br />
![rabbit_ocf](images/rabbit_ocf.png)

Note: Speaker - Vladimir Kuklin:

While working on RabbitMQ resilience, we noticed that RabbitMQ server behaviour is not always obvious and clear. E.g. in case of 3-node cluster if you do hard reset of RabbitMQ cluster it usually becomes very vulnerable to race conditions and there is not much you can do about it. 
In order to automate RabbitMQ reassemblance we created OCF script that leverages Pacemaker master/slave resources and notification mechanism for *cloned* resources.

So you can see the diagram showing what you need to do to assemble rabbitmq cluster under control of Pacemaker:

-	Fire up beam processes for the first node
-	Let Pacemaker elect the master (this will be the first node)
-	Create 'master' attribute in CIB
-	Start 'master' application on the first node and attach *slave* nodes to the *master*
-       Periodic status command should check each running rabbitmq server if it is connected to the master node
-       If rabbit app cannot start or join or whatever - reset it
