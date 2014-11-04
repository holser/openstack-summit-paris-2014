# Keystone

- tokens in memcached
- dogpile driver with python-memcached broken
- pylibmc is non eventlet safe
- Y. Taraday wrote newer driver with connection pool
- there are still bugs in python-memcached

Note: Speaker - Sergii Golovatiuk

We strongly believe that temporary data should be kept in key value stores.

What will happen if we lose this data? Correct, on the next operation, the client or service will
get a new token using standard authentication method.

In order to maintain keystone resilience, we added memcached support for tokens,
but then we figured out that a dead controller with memcached instance may lead to 6 seconds lag in
operations and makes the cluster unusable. 

We started looking for another solution, we tried pylibmc which is a good one but it was not eventlet-safe.
Y. Taraday wrote a driver that supports pool of connections for memcached.

Nevertheless, there are still some problems with python-memcached as it has some broken logic for keys sharding,
which we are working to merge a fix for.
