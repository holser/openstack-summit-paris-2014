# Ceph

## Used for:

- ephemeral storage <=> live migration
- object and image storage shared
- share host param for volume service

Note: Speaker - Sergii Golovatiuk:

We are using pretty common Ceph architecture putting monitor nodes on the controller nodes and having separate roles for Ceph OSD nodes. User is able to set specific (e.g. SSD disks) for ceph journal for each OSD.
One of the things that we can use in Ceph is that it is shared software defined storage and can be used as a replacement for proprietary solutions if you want live migration and highly available object/image/volume stroge. We had to write a bunch of code to support live migration with ceph as it was not in the perfect state previously. For cinder HA we had to specify identical host parameter in cinder config for all volume nodes in order to make volumes really shared.
