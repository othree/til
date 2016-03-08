External Resource for Homebrew Formula
======================================

Sometimes, it is necessary to grab necessary files from other location in order to build program.
Ex: Apache httpd server requires [APR and APR-Util][apr]. And Homebrew Formula don't have direct 
method for this kind of requirement. But it is still possible to achive by using `go_resource`.
A simple example:

```python
  go_resource "brotli" do
    url "https://github.com/google/brotli/archive/v0.3.0.tar.gz"
    sha256 "5d49eb1a6dd19304dd683c293abf66c8a419728f4c6d0f390fa7deb2a39eaae2"
  end

  def install

    resource("brotli").stage buildpath
```

First use `go_resource` to define an external resource before install block. And then inside install
block. We can use `resource("brotli").stage buildpath` to extract this resource file. `buildpath`
is the location of temp directory while building[[ref][bpath]]. And `go_resource` will append
the name of the resource. In this case `brotli`.

So in this example, home brew will fetch `v0.3.0.tar.gz` and extract to `$buildpath/brotli`.
We can write similar formula for Apache HTTP Server to grab APR before compile:

```python
  go_resource "APR" do
    url "http://ftp.yz.yamagata-u.ac.jp/pub/network/apache//apr/apr-1.5.2.tar.gz"
    sha256 "1af06e1720a58851d90694a984af18355b65bb0d047be03ec7d659c746d6dbdb"
  end

  def install

    resource("APR").stage buildpath
```


[apr]:https://httpd.apache.org/docs/2.4/install.html#requirements
[bpath]:https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Formula-Cookbook.md#variables-for-directory-locations
