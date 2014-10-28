# Keystone

- tokens in Memcache
- dogpile driver with python-memcached broken - dead server => 6 secs lag for each operation
- pylibmc non-eventlet-safe
- Y. Taraday wrote newer driver with connection pool
- Low-probability (1.6%) bugs in python-memcached

Note:

We strongly believe that temporary data should be kept in key value storages.
Firstly, there is no need to keep temporary data in database. Secondly, what will
happen if we lose this data? Correct, on next operation the client or service will
get a new token using standard authentication method.
In order to maintain keystone resilience we added memcached support for tokens,
but then we figured out that dead controller may lead to 6 seconds lag in
operations and makes cluster unusable. We started searching for another solution,
but pylibmc was not eventet-safe. So we wrote a driver that supported
connection pooling for memcached. Nevertheless, there are still some
problems with python-memcached as it has some broken logic for keys sharding,
which we are working to merge fix for.
