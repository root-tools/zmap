ZMap: The Internet Scanner
==========================

[![Build Status](https://travis-ci.org/zmap/zmap.svg?branch=travis-configuration)](https://travis-ci.org/zmap/zmap)

ZMap is a fast single packet network scanner designed for Internet-wide network
surveys. On a typical desktop computer with a gigabit Ethernet connection, ZMap
is capable scanning the entire public IPv4 address space in under 45 minutes. With
a 10gigE connection and [PF_RING](http://www.ntop.org/products/packet-capture/pf_ring/),
ZMap can scan the IPv4 address space in under 5 minutes.

ZMap operates on GNU/Linux, Mac OS, and BSD. ZMap currently has fully implemented
probe modules for TCP SYN scans, ICMP, DNS queries, UPnP, BACNET, and can send a
large number of [UDP probes](https://github.com/zmap/zmap/blob/master/examples/udp-probes/README).
If you are looking to do more involved scans, e.g.,
banner grab or TLS handshake, take a look at [ZGrab](https://github.com/zmap/zgrab),
ZMap's sister project that performs stateful application-layer handshakes.

Installation
------------

The latest stable release of ZMap is version 2.1.1 and supports Linux, macOS, and
BSD. We recommend installing ZMap from HEAD rather than using a distro package manager.

**Instructions on building ZMap from source** can be found in [INSTALL](INSTALL.md).

Usage
-----

A guide to using ZMap is found in our [GitHub Wiki](https://github.com/zmap/zmap/wiki).

License and Copyright
---------------------

ZMap Copyright 2017 Regents of the University of Michigan

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See LICENSE for the specific
language governing permissions and limitations under the License.




Home
Zakir Durumeric edited this page on 27 Nov 2017 · 15 revisions
ZMap is a network scanner designed to perform comprehensive scans of the IPv4 address space or large portions of it.

By default, ZMap will perform a TCP SYN scan on the specified port at the maximum rate possible. A more conservative configuration that will scan 10,000 random addresses on port 80 at a maximum 10 Mbps can be run as follows:

$ zmap --bandwidth=10M --target-port=80 --max-targets=10000 --output-file=results.csv
Or more concisely:

$ zmap -B 10M -p 80 -n 10000 -o results.csv
ZMap can also be used to scan specific subnets or CIDR blocks. For example, to scan only 10.0.0.0/8 and 192.168.0.0/16 on TCP/80, run:

$ zmap -p 80 10.0.0.0/8 192.168.0.0/16
If the scan started successfully, ZMap will output real-time status updates:

0% (1h51m left); send: 28777 562 Kp/s (560 Kp/s avg); recv: 1192 248 p/s (231 p/s avg); hits: 0.04%
0% (1h51m left); send: 34320 554 Kp/s (559 Kp/s avg); recv: 1442 249 p/s (234 p/s avg); hits: 0.04%
0% (1h50m left); send: 39676 535 Kp/s (555 Kp/s avg); recv: 1663 220 p/s (232 p/s avg); hits: 0.04%
0% (1h50m left); send: 45372 570 Kp/s (557 Kp/s avg); recv: 1890 226 p/s (232 p/s avg); hits: 0.04%
These updates provide information about the current state of the scan and are of the following form: %-complete (est time remaining); packets-sent curr-send-rate (avg-send-rate); recv: packets-recv recv-rate (avg-recv-rate); hits: hit-rate.

If you do not know the scan rate that your network can support, you may want to experiment with different scan rates or bandwidth limits to find the fastest rate that your network can support before you see decreased results.

By default, ZMap will output the list of distinct IP addresses that responded successfully (e.g. with a SYN ACK packet) similar to the following. There are several additional formats (e.g. JSON and Redis) for outputting results. Additional output fields can be specified and the results can be filtered using an output filter. [more information]

115.237.116.119
23.9.117.80
207.118.204.141
217.120.143.111
50.195.22.82
We strongly encourage you to use a blacklist file, to exclude both reserved/unallocated IP space (e.g. multicast, RFC 1918), as well as networks that request to be excluded from your scans. By default, ZMap will utilize a simple blacklist file containing reserved and unallocated addresses located at /etc/zmap/blacklist.conf. [more information]

If you find yourself specifying certain settings, such as your maximum bandwidth or blacklist file every time you run ZMap, you can specify these in /etc/zmap/zmap.conf or use a custom configuration file.

If you are attempting to troubleshoot scan related issues, there are several options to help debug. First, it is possible can perform a dry run scan in order to see the packets that would be sent over the network by adding the --dryrun flag. As well, it is possible to change the logging verbosity by setting the --verbosity=n flag.
