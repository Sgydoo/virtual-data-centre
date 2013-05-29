
Virtual Data Centre
===================

These scripts allow you to launch a virtual network container for
QEMU/kvm virtual machines on a Linux host system. The container
consists of:

* A tap interface on the host machine.
* A Virtual Distributed Ethernet (VDE) switch connected to the tap interface.
* iptables rules on the host to configure NAT to the outside world.
* A DHCP server and DNS forwarder. We use dnsmasq for this.

Tested on Ubuntu 11.10 (Oneiric Ocelot) and Debian 7.0 (wheezy).


Why?
----

I've found this to be the quickest way to fire up some virtual
machines for testing purposes in their own self-contained VLAN,
especially when using meaningful shell aliases to run the scripts. No
more faffing about with graphical interfaces!

I'm aware there may be better tools out there that do the same thing,
but I was interested in the learning process.


Prerequisites
-------------

You will need at least the following:

1) A QEMU vitual machine disk image.

How to create one of these is beyond the scope of this document, but
you can find the documentation for QEMU [here](http://en.wikibooks.org/wiki/QEMU).

2) Some Debian/Ubuntu packages:

* Ubuntu 11.10 (Oneiric Ocelot)

    iptables
    qemu-common
    qemu-kvm
    kvm-pxe (optional, but stops a warning message every time you start a VM)
    uml-utilities
    vde2
    dnsmasq

* Debian 7.0 (wheezy)

    iptables
    qemu
    qemu-kvm
    uml-utilities
    vde2
    dnsmasq

General Note: The uml-utilities package also installs a uml switch,
which starts up automatically at boot time. Since we won't be using
that, it's a good idea to disable it.


Configuring dnsmasq
-------------------

After installation, dnsmasq needs to be configured. For example, my
/etc/dnsmasq.d/local.conf looks like this:

    except-interface=eth0
    bind-interfaces
    dhcp-range=192.168.10.50,192.168.10.100,12h

To prevent contention with existing DHCP servers, the 'except-
interface' directive should be whichever interface is connected
to your LAN.

Remember to set the dhcp-range to suit your local situation. Just
ensure that you use a subnet which doesn't conflict with your LAN
address range.


Running the Scripts
-------------------

There are two main scripts:

* vdcnet   - Starts the network container (does nothing if the container
             is already running).

* launchvm - Starts a virtual machine and plugs it in to the VDE switch.
             Takes a qemu VM disk image and amount of RAM as arguments.

So to launch a VM in a new or existing network container:

    $ vdcnet start; launchvm <disk_image_filename> <RAM_in_MB>


More Information
----------------

For more info, see the [Wiki](https://github.com/sgygit/virtual-data-centre/wiki) :)

Simon.
