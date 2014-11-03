# Testing HA

- fuel-devops - tiny piece of libvirt-based orchestrator
- destructive tests:
    -  running virtual environments
    -  performing destructive actions
-  testing that cluster failed over successfully:
    - OSTF
    - Tempest
    - Rally

Note: Speaker - Sergii Golovatiuk:

Testing HA does not come at easy cost. We are doing this primarily using our own test suite. We spawn environments using our tiny orchestrator which uses python libvirt and other libraries. Then we perform actual destructive actions and check if cluster can failover succesfully.
