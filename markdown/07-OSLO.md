# AMQP - oslo.messaging

- AMQP - multiple rabbit connection included
- OSLO.messaging lacks [heartbeats](https://bugs.launchpad.net/mos/+bug/856764):
    - 1st heartbeat implementation was broken
    - messaging code was still referencing dead objects
    - even kernel could not help
    - connections containing ZERO channels

#### Good news: fix is on review

Note: Speaker - Vladimir Kuklin

 We started with HAproxy proxying to the only rabbitmq node. That solution was not the
 best. We wanted to archieve affordable results allowing our controllers to scale up
 easily. When oslo.messaging was merged, we started using internal OSLO.messaging
 healing mechanism making connections to all RabbitMQ instances specified in config
 files along with shuffling of them to minimize effect of AMQP failover.

 But we had to rewrite part of oslo.messaging code to support AMQP heartbeats and
 handle connection failure scenarios. The first implementation was broken. Oslo code
 was not handling low-level exceptions and top-layer code was still referencing dead
 objects. Also, we noticed that there were some RPC consumers not consuming messages
 from the queues because some of the connections contained zero channels.

 Good news is that we fixed it and pushed it to openstack gerrit. 
