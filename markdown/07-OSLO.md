# AMQP - oslo.messaging

- AMQP - multiple rabbit connection included
- OSLO.messaging lacks [heartbeats](https://bugs.launchpad.net/mos/+bug/856764)

Note: Speaker - Vladimir Kuklin

We started with HAproxy proxying to the only rabbitmq node. That solution was not the best. We wanted to archieve affordable results allowing our controllers to scale up easily. When oslo.messaging was merged, we started using internal OSLO.messaging healing mechanism making connections to all RabbitMQ instances specified in config files along with shuffling of them to minimize effect of AMQP failover.

OSLO.messaging hearbeat TODO
