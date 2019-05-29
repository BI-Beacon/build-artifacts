.. describes the reasoning behind the current architecture

.. _ref_architecture:

Architecture
============

.. graphviz::

    digraph bibeacon_architecture {

        bibeacon_1 [ label="BI-Beacon\n1" ]
        bibeacon_2 [ label="BI-Beacon\n2" ]
        LAN_caller [ label="LAN\ncaller" ]
        Cloud_caller [ label="Cloud\ncaller" ]
        state_server [ label="State server" ]

        bibeacon_1 -> state_server
        bibeacon_2 -> state_server
        LAN_caller -> state_server
        Cloud_caller -> state_server
    }

BI-Beacon 1 and 2
    This is either physical or virtual BI-Beacon devices, showing some state
    of something interesting to your business.

State Server
    This is the source of state for BI-Beacons.

Caller systems
    This is the user of the Beacons - where API calls originate.


Background
----------

The BI-Beacon architecture is fairly straightforward, however, might need
some explanation anyway since it is not the simplest of possible
designs and this is intentional.

So let's begin with the simplest possible design and work our
way from there.



Idea 1: direct cable connections
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If we want to control devices in our offices, the simplest possible
idea would be to connect them directly to our computers.

Indeed, this  is possible  with a  BI-Beacon, using  a USB  data cable
connected  to a  wall-powered  PC, but  since  it requires  USB-serial
device drivers, as well as the right user/device permissions on the PC
in question, this  would actually be harder to do  than using the REST
API over WiFi!

This method also has the drawback of limiting the location of a Beacon
to the  vicinity of a  PC. Also,  the software controlling  the Beacon
would need to be on that PC.


Idea 2: dynamic IP addresses
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The next natural step after direct connection to a PC would be
to connect over the local network - be it over WiFi or an ethernet cable,
giving the device a local IP.

So why not use this method?

Well, the reason is corporate IT networks; they're a mess in general!

Getting a  dynamic IP  by connecting  a device to  the network  is one
thing; DHCP  is common enough  today that  it can be  generally relied
upon, however,  what then?  If you  want to  communicate to  a (local)
device connected to  the (local) network, you would need  to know its'
dynamic IP address.

At home, you could just login to your router (at least if you are tech
savvy enough!) and find the IP-address of the Beacon.

But  at work,  unless you're  working in  the IT  department, that  is
typically out  of the question,  not only for "security  reasons", but
also due to that the IT  department most likely already have enough on
their hands! And  getting them to configure a device  to have a static
IP is just ..  many weeks of delivery time - time none  of us has, nor
want to put into getting a BI-Beacon up and running.

A drawback of this method is also the 'local' part - we cannot
control a BI-Beacon unless we're on the same network. Forget about
controlling it from the cloud!


Idea 3: IoT to the rescue!
~~~~~~~~~~~~~~~~~~~~~~~~~~

So, as odd as it sounds, it is actually easier to make the device
an internet-global device instead of a local (direct cable, or
local network) device!

The trick is to have the  Beacons retrieve their respective state from
a known  server (corporate  internal or  otherwise), via  secure HTTPs
requests.

This  means the  devices  can get  their  dynamic, local  IP-addresses
inside of your  fine and dandy corporate network and  you address them
indirectly  by  communicating with  the  state  server, which  resides
either inside your corporate network  (at a well-known address) or via
the public internet!

As an integrator or user of Bi-Beacons, all you have to do is send off
HTTPs requests  to the state  server (or  "API server" if  you prefer)
which stores the  states and serves them to Beacons  asking what state
to switch to.

It Just Works™! :)
