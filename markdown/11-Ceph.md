# Ceph

- Live migration <links here?>
- Cinder HA (host paramater and <link to the bug>)
- Misc stuff - links from Dmitry

Note: Speaker - Sergii Golovatiuk:

Mention Dmitry, Andrew and Ryan

We are using pretty common Ceph architecture putting monitor nodes on the controller nodes and having separate roles for Ceph OSD nodes. ...
One of the things that we can use in Ceph is that it is shared software defined storage and can be used as a replacement for proprietary solutions if you want live migration. Nevertheless, we had to write a bunch of code to support live migration with ceph as it was not in the perfect state previously. Cinder HA required tricky altering of the configuration. As we know, cinder sticks volume to the cinder-volume node it was allocated with, which is specified as host parameter in cinder config. There is a bug about to but it still has wishlist status.
