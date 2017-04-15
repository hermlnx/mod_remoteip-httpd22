## Apache HTTP Server 2.2.x mod_remoteip backport
This is a backport of apache 2.4.16 mod_remoteip to apache 2.2.x.

## Requirements

gcc, make, httpd-devel 

## Compile and Install

```
apxs -i -c -n mod_remoteip.so mod_remoteip.c
```
 
or simply try:


```
make; make install
```

## Build RHEL installation package

### Install Required Packages

```
yum install -y rpm-build rpmdevtools redhat-rpm-config
```

For more information see: https://wiki.centos.org/HowTos/SetupRpmBuildEnvironment

### Build RPM

First Build Source RPM
```
 make srpm
```

Then rebuild it into binary

```
rpmbuild --rebuild mod_remoteip-httpd22/dist/mod_remoteip-11-1.el6.src.rpm
```


## Configuration Example
```
LoadModule remoteip_module modules/mod_remoteip.so
# name of the header from your load balancer
# that contains the originating client IP address
RemoteIPHeader X-Forwarded-For
# IP address or block of your load balancer
# (from the perspective of your back-end servers)
RemoteIPInternalProxy 10.0.0.1/24

# standard log file formats rewritten with %a in place of %h
LogFormat "%v:%p %a %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" vhost_combined
LogFormat "%a %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%a %l %u %t \"%r\" %>s %O" common

```
For more information, see: http://httpd.apache.org/docs/2.4/mod/mod_remoteip.html


## Author

* Original Backport by Takashi Takizawa <taki@cyber.email.ne.jp>
* Replaced by Schlomo <schlomo.schapiro@immobilienscout24.de> with code 
  found in http://people.apache.org/~wrowe/httpd-2.2-ports/mod_remoteip.c because
  of segfaults. Added various bugfixes found via web search.

## Distribution

Latest version available from https://github.com/ImmobilienScout24/mod_remoteip-httpd22

