A very old way to run system service
====================================

Want to run a service on system boot. Tranditional linux init ([System V](https://en.wikipedia.org/wiki/UNIX_System_V) like)
supports `rc.local` file.

Just edit `/etc/rc.d/rc.local` (CentOS6, real location depends on distribution) and add commands to run the service.

Recent solution to init system are [upstart](http://upstart.ubuntu.com/) and [systemd](https://freedesktop.org/wiki/Software/systemd/).
Both will be faster(synchronous vs asynchronous), but `rc.local` is much simpler.
