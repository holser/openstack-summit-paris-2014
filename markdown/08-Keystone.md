# Keystone

- Tokens in Memcache
- PKI tokens are very large

Note:

We strongly believe that temporary data should be kept in key value storages. Firstly, there is no need to keep temporary data in database. Secondly, What will happen if we loose this data? Right, on next operation the client or service will get a new token using standard authentication method.

We switched backend from MySQL to Memcached and failed miserably. Python-memcached doesn't support multiple backends well. If you lose one backend it could have sent data to that backend, making 5 second deadlock. There were no logic to drop that backend for a while and try it once again.

We tried to switch to Python-pylibmc, which is great. However, we realized that it's not eventlet safe ... So,


# New Keystone driver

Note: One of our developer (Yury Taraday) made a new backend for keystone, which is eventlet safe with backend "dead" detection. This allows to use keystone natively allowing to switch to another memcache server in case of problem with the first one.


