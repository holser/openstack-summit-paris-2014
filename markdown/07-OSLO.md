# AMQP - oslo.messaging

- AMQP - multiple rabbit connection included
- OSLO.messaging lacks [heartbeats](https://bugs.launchpad.net/mos/+bug/856764)

Note:
We started with HAproxy proxying to the only rabbitmq node, but it was not the best solution. When oslo.messaging was merged, we started using several RabbitMQ instances in config files along with shuffling of them to minimize effect of AMQP failover.

OSLO.messaging hearbeats part 
