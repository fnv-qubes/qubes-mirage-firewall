# qubes-mirage-firewall

An **experimental** unikernel that can run as a QubesOS ProxyVM, replacing `sys-firewall`.
It uses the [mirage-qubes][] library to implement the Qubes protocols.

Note: This firewall *ignores the rules set in the Qubes GUI*.

Currently it only prevents incoming connections from the outside world (which is really just a side-effect of doing NAT). So currently it's really just a router rather than a firewall.

Also, it doesn't yet proxy DNS requests.

To build:

    $ opam install mirage
    $ opam pin add mirage-clock-xen https://github.com/mirage/mirage-clock.git
    $ opam pin add mirage-net-xen 'https://github.com/talex5/mirage-net-xen.git#disconnect'
    $ opam pin add tcpip https://github.com/mirage/mirage-tcpip.git
    $ opam pin add mirage-xen 'https://github.com/talex5/mirage-platform.git#mm'
    $ opam pin add mirage-qubes https://github.com/talex5/mirage-qubes.git
    $ opam pin add mirage-nat 'https://github.com/talex5/mirage-nat.git#simplify-checksum'
    $ mirage configure --xen
    $ make

You can use this with the [test-mirage][] scripts to deploy the unikernel (`mir-qubes-firewall.xen`) from your development AppVM. e.g.

    $ test-mirage mir-firewall.xen mirage-firewall
    Waiting for 'Ready'... OK
    Uploading 'mir-qubes-firewall.xen' (4843304 bytes) to "mirage-firewall"
    Waiting for 'Booting'... OK
    --> Loading the VM (type = ProxyVM)...
    --> Starting Qubes DB...
    --> Setting Qubes DB info for the VM...
    --> Updating firewall rules...
    --> Starting the VM...
    --> Starting the qrexec daemon...
    Waiting for VM's qrexec agent.connected
    --> Starting Qubes GUId...
    Connecting to VM's GUI agent: .connected
    --> Sending monitor layout...
    --> Waiting for qubes-session...
    Connecting to mirage-firewall console...
    MirageOS booting...
    Initialising timer interface
    Initialising console ... done.
    Netif: add resume hook
    gnttab_stubs.c: initialised mini-os gntmap
    2015-12-30 10:04.42: INF [qubes.rexec] waiting for client...
    2015-12-30 10:04.42: INF [qubes.gui] waiting for client...
    2015-12-30 10:04.42: INF [qubes.db] connecting to server...
    2015-12-30 10:04.42: INF [qubes.db] connected
    2015-12-30 10:04.42: INF [qubes.rexec] client connected, using protocol version 2
    2015-12-30 10:04.42: INF [qubes.db] got update: "/qubes-keyboard" = "xkb_keymap {\n\txkb_keycodes  { include \"evdev+aliases(qwerty)\"\t};\n\txkb_types     { include \"complete\"\t};\n\txkb_compat    { include \"complete\"\t};\n\txkb_symbols   { include \"pc+gb+inet(evdev)\"\t};\n\txkb_geometry  { include \"pc(pc104)\"\t};\n};"
    2015-12-30 10:04.42: INF [qubes.gui] client connected (screen size: 6720x2160)
    2015-12-30 10:04.42: INF [unikernel] agents connected in 0.052 s (CPU time used since boot: 0.007 s)
    Netif.connect 0
    Netfront.create: id=0 domid=1
     sg:true gso_tcpv4:true rx_copy:true rx_flip:false smart_poll:false
    MAC: 00:16:3e:5e:6c:0b
    ARP: sending gratuitous from 10.137.1.13
    2015-12-30 10:04.42: INF [application] Client (internal) network is 10.137.3.0/24
    ARP: transmitting probe -> 10.137.1.1
    2015-12-30 10:04.42: INF [net] Watching backend/vif
    2015-12-30 10:04.42: INF [qubes.rexec] Execute "user:QUBESRPC qubes.SetMonitorLayout dom0\000"
    2015-12-30 10:04.42: WRN [command] << Unknown command "QUBESRPC qubes.SetMonitorLayout dom0"
    2015-12-30 10:04.42: INF [qubes.rexec] Execute "root:QUBESRPC qubes.WaitForSession none\000"
    2015-12-30 10:04.42: WRN [command] << Unknown command "QUBESRPC qubes.WaitForSession none"
    2015-12-30 10:04.42: INF [qubes.db] got update: "/qubes-netvm-domid" = "1"
    ARP: retrying 10.137.1.1 (n=1)
    ARP: transmitting probe -> 10.137.1.1
    ARP: updating 10.137.1.1 -> fe:ff:ff:ff:ff:ff



# LICENSE

Copyright (c) 2015, Thomas Leonard
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
gg

[test-mirage]: https://github.com/talex5/qubes-test-mirage
[mirage-qubes]: https://github.com/talex5/mirage-qubes