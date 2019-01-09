.. all documentation in one page for now

BI-Beacon Overview
==================

.. image:: https://api.travis-ci.org/objarni/leanmodel.svg?branch=master

.. image:: https://sonarcloud.io/api/project_badges/measure?project=bibeacon-code-examples&metric=alert_status

This is the official documentation for Business Intelligence Beacon.
The primary focus of this documentation is the API to control Beacons,
however it also includes information on how to configure devices,
and control code examples in different programming languages, as well
as tooling around the project.

.. tip::

    Read more about the ideas behind BI-beacon on the concept page:

    https://bi-beacon.com

    Want to get one for your company? Visit the web shop, available here:

    https://bi-beacon.se

In a nutshell: BI-Beacons use a RESTful API.

There is only one end-point: ``/<channelkey>``.

This endpoint serves as the controlling mechanism for *channel* with ID
`channelkey`.

Here is an example of a complete endpoint:

   ``https://api.cilamp.se/v1/our-beautiful-monitor``


.. note::

    `channel key` used to be called `systemid`, so if you see the old name
    somewhere - please give us a shout and we'll fix it! Or even better,
    fork the docs repo, fix the mistake and make a pull request! :)

    Repo found on GitHub here `Documentation repository`_.

Multiple beacons can be configured to copy the state of the same channel -
this is intentional and means you can deploy several Beacons that indicate
the same thing.

This functionality may, for  instance, be used if you have multiple
offices, or if you want a Beacon both in the conference room and at
the coffee machine.


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


.. _ref_api:

API
===

There is only one endpoint! Hurray!

And it looks something like this:

   ``https://api.cilamp.se/v1/our-beautiful-monitor``

BI-Beacons are controlled  via a RESTful web API,  but only indirectly
via a state  server (see Architecture_). This means it  is possible to
control Beacons from any programming language that can make HTTPs POST
requests.

You configure a Beacon to continuously copy the state of a certain
channel, identified by a string called *channel key*.
A channel key is made up of at
least eight characters. Allowed characters classes are small and big
english letters, digits, underscore and dash, or put in regex form:

        ``[a-zA-Z0-9_-]{8,255}``

A channel key can be 8 to 255 characters long.

.. note::
    `channel key` used to be called `systemid`.

Several Beacons may use the same channel; they will then
show the same state, which is great if you have a large office,
or work at an international company.

The channel key may be viewed as the access key of the Beacon,
as it is all that is needed to control a device. So make sure you
only share the channel key with people and systems that should
be able to control the device. Do not store it publicly (unless you
want anyone to be able to change the state of your Beacon, which
could be fun but probably not your most common use case!)

If you want to add some security, randomize a string of at least 20
letters and numbers and use that as channel key.

A channel can be in one of two states:

+---------+--------------------------------------------------------+
| State   |  Meaning                                               |
+=========+========================================================+
| static  | Connected Beacons will show a constant color           |
+---------+--------------------------------------------------------+
| pulsing | Connected Beacons will pulse with                      |
|         | given speed and color                                  |
+---------+--------------------------------------------------------+

*Static colors* give the impression of the state of a system or process,
e.g. on or off, ready or failed.

*Pulsing colors* give the impression of something happening, e.g.
something is building or being processed.

The meaning of individual colors and pulses is up to your imagination.

Change state
------------

:URL:       ``https://:beacon-server/v1/:channelkey/``

:Method:    POST

:Parameters:

    {
      **color:** color specification (format "#RRGGBB")

      **period:** length of the pulse in milliseconds (optional, format integer)
    }

Make sure the parameters are be transmitted as URL encoded Form Data,
i.e. the request header Content-Type should be
``application/x-www-form-urlencoded``.

:beacon-server
    This is the hostname of the state server.

:channelkey
    This string identifies the channel you want to change the state of.

.. note:: At the moment, there is only one official beacon state server.
          It is available at this URL:

             ``https://api.cilamp.se/v1``

          The state server will be available as open source software
          in June 2019, see `Announcement: Open source server in June`_.



.. _`Announcement: Open source server in June`: https://bi-beacon.se/announcement-open-source-api-server-coming-in-june/

Parameter examples
~~~~~~~~~~~~~~~~~~

:Purpose:   Set beacon to green
:Parameters:

::

    {
        color: "#00FF00"
    }

:Purpose:   Set beacon to red and pulse once per second
:Parameters:

::

    {
        color: "#FF0000"
        period: 1000
    }


Expected response
~~~~~~~~~~~~~~~~~

On success

:Code:              200
:Body:

::

    {"message": "':channelkey' updated"}

On error

:Code:              400
:Body:

::

    {"message": "<error message>"}


Sample Curl Call
~~~~~~~~~~~~~~~~

The following will make a POST request to the BI-Beacon state server
``api.cilamp.se`` to change the state of the channel named
`testchannel` to green:

::

    curl -X POST -F "color=#00FF00" "https://api.cilamp.se/v1/testchannel"


.. _`Documentation repository`: https://github.com/BI-Beacon/docs
