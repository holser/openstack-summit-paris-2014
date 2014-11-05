# AMQP - oslo.messaging

- AMQP - multiple rabbit connection included
- OSLO.messaging lacks [heartbeats](https://bugs.launchpad.net/mos/+bug/856764):
    - 1st community heartbeat implementation was broken
    - even kernel could not help

 <br /> <center><!-- .element: class="fragment" -->Good news: fix is on review <br /> https://review.openstack.org/126330 </center>

Note: Speaker - Vladimir Kuklin

 We started with HAproxy proxying to the only rabbitmq node. That solution was not the
 best. We wanted to achieve affordable results allowing our controllers to scale up
 easily. When oslo.messaging was merged, we started using internal OSLO.messaging
 healing mechanism making connections to all RabbitMQ instances specified in config
 files along with shuffling of them to minimize effect of AMQP failover.

 But we had to rewrite part of oslo.messaging code to support AMQP heartbeats and
 handle connection failure scenarios. The first community implementation was broken.
 Even kernel killing connections could not make oslo fail over. 

 Good news is that we fixed it and pushed it to openstack gerrit. 
