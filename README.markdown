NAME
====

nginx-systemtap-toolkit - Real-time analyzing and diagnosing tools for Nginx based on [SystemTap](http://sourceware.org/systemtap/wiki)

Status
======

These scripts are considered experimental.

Prerequisites
=============

You need at least systemtap 2.1+ (with [my patch](http://sourceware.org/ml/systemtap/2013-q2/msg00213.html) applied) and perl 5.6.1+ to run these tools on your Linux system.

Also, you should ensure the (DWARF) debuginfo of your perl binary being analyzed is already enabled (or installed separately).

If you compile perl from source, then ensure you pass the `-Doptimize=-g` option to the `Configure` command line.

If you are on Linux kernels older than 3.5, then you may have to apply the [utrace patch](http://sourceware.org/systemtap/wiki/utrace) (if not yet) to your kernel to get
user-space tracing support for your systemtap installation. But if you are using Linux distributions in the RedHat family (like RHEL, CentOS, and Fedora), then your old kernel should already has the utrace patch applied.

The mainstream Linux kernel 3.5+ does have support for the uprobes API for userspace tracing.

Permissions
===========

Running systemtap-based tools requires special user permissions. To prevent running
these tools with the root account,
you can add your own (non-root) account name to the `stapusr` and `staprun` user groups.
But if the user account running the Nginx process is different from your current
user account, then you will still be required to run "sudo" or other means to run these tools
with root access.

Tools
=====

pl-sample-bt
------------

This script can be used to sample Perl-land backtraces in a running perl process specified by the `-p` option.

It outputs the aggregated backgraces (by count). For example,
to sample a running perl process (whose pid is 737) in user space only for total 5 seconds:

    $ ./pl-sample-bt -p 737 -t 5 > a.bt
    WARNING: Sampling 737 (/opt/perl514/bin/perl) for Perl-land backtraces...
    Please wait for 4 seconds.

The resulting output file `a.bt` can then be used to generate a Flame Graph by using Brendan Gregg's [FlameGraph tools](https://github.com/brendangregg/FlameGraph):

    stackcollapse-stap.pl a.bt > a.cbt
    flamegraph.pl a.cbt > a.svg

where both the `stackcollapse-stap.pl` and `flamegraph.pl` are from the FlameGraph toolkit.
If everything goes right, you can now use your web browser to open the `a.svg` file.

Below is a simple example Perl-land flamegraph

http://agentzh.org/misc/flamegraph/perl-silly.svg

This tool has been tested with perl 5.14.4 and 5.16.2.

Community
=========

English Mailing List
--------------------

The [openresty-en](https://groups.google.com/group/openresty-en) mailing list is for English speakers.

Chinese Mailing List
--------------------

The [openresty](https://groups.google.com/group/openresty) mailing list is for Chinese speakers.

Bugs and Patches
================

Please submit bug reports, wishlists, or patches by

1. creating a ticket on the [GitHub Issue Tracker](http://github.com/agentzh/nginx-systemtap-toolkit/issues),
1. or posting to the [OpenResty community](http://wiki.nginx.org/HttpLuaModule#Community).

TODO
====

Author
======

Yichun "agentzh" Zhang (章亦春), CloudFlare Inc.

Copyright & License
===================

This module is licenced under the BSD license.

Copyright (C) 2012-2013 by Yichun Zhang (agentzh) <agentzh@gmail.com>, CloudFlare Inc.

All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:

* Redistributions of source code must retain the above copyright
notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright
notice, this list of conditions and the following disclaimer in the
documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

See Also
========
* SystemTap Wiki Home: http://sourceware.org/systemtap/wiki
