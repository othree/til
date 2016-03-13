Add `/usr/local/lib/` to Static Library Lookup Path
===================================================

I used to try to build Apache HTTP2 server to support HTTP/2 on Ubuntu 14.04.
And it requires me to build OpenSSL 1.0.2 for ALPN. So I I have to run configure script like this:

    env PKG_CONFIG_PATH=/usr/local/ssl/lib/pkgconfig ./configure --enable-http2

But after compile complete. When I try to start HTTP server. I will got an error message.
Can't find OpenSSL lib. This is because `/usr/local/ssl/lib` is not in static library lookup path.
Also, `/usr/local/lib` is not in the paths by default. Which is the default install location of other
libs, if you compile and install it by yourself.

So when I want to start the HTTP server, I have to execute the command like:

    env LD_LIBRARY_PATH=/usr/local/ssl/lib /usr/local/apache2/bin/httpd

This is not easy to remember, and hard to maintain as a service. So I go find how to update the lookup path.
And I found the setting is at `/etc/ld.so.conf` in most Linux distribution. Its very easy to edit.
And after editing complete, just run `ldconfig` to update the run time setting.
