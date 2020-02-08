.. describes the application programming interface

.. _ref_api:

API
===

There is only one endpoint! Hurray!

And it looks something like this:

   ``https://api.bi-beacon.com/v1/our-beautiful-monitor``

BI-Beacons are controlled  via a RESTful web API,  but only indirectly
via a state  server. This means it  is possible to
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

             ``https://api.bi-beacon.com/v1``

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
``api.bi-beacon.com`` to change the state of the channel named
`testchannel` to green:

::

    curl -X POST -F "color=#00FF00" "https://api.bi-beacon.com/v1/testchannel"


Turn off Beacon
---------------

Simply send a POST request with color equal to black - "#000000" - to turn off
a BI-Beacon.

