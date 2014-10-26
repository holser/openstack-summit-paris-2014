# AMQP - oslo.messaging

- AMQP - multiple rabbit connection included
- OSLO.messaging lacks of [heartbeats](https://bugs.launchpad.net/mos/+bug/856764)

Note:
We started with haproxy proxying to the only rabbitmq node, but it was not the best solution. When oslo.messaging was merged we started using several rabbitmq instances in config files along with shuffling of them to minimize effect of AMQP failover.

OSLO.messaging hearbeats part 
