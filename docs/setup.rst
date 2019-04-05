.. documents how to configure a beacon

.. raw:: html

    <style>
      .blue {color:blue}
      .purple {color:purple}
      .green {color:green}
    </style>

.. role:: blue

.. role:: purple

.. role:: green


How to setup a BI-Beacon
========================

Setup BI-Beacon via Wifi
------------------------

.. |image1| image:: _static/wifisetup_1.png
   :width: 100%

.. |image2| image:: _static/wifisetup_2.png
   :width: 100%

.. |image3| image:: _static/wifisetup_3.png
   :width: 100%

.. |image4| image:: _static/wifisetup_4.png
   :width: 100%

You can setup a BI-Beacon using a mobile or desktop device that connects to the Beacons own setup web page.

+------------+--------------------------------------------------------+
|           Instructions                                              |
+============+========================================================+
|            |                                                        |
|            | 0. Find your WiFi credentials (password and WiFi name) |
|  |image1|  | 1. Plugin your BI-Beacon (it turns blue)               |
|            | 2. Disable cellular network (if on mobile device)      |
|            | 3. Wait up to 30 seconds until BI-Beacon starts        |
|            |    shifting slowly between blue and green              |
|            |    (this means setup mode is active)                   |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image2|  |                                                        |
|            | 4. Connect to WiFi “``BI-Beacon-A1B2C3``”              |
|            |    (where ``A1B2C3`` is unique for your Beacon)        |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image3|  |                                                        |
|            | 5. Browse to BI-Beacon’s Setup Page:                   |
|            |    http://192.168.4.1                                  |
|            |                                                        |
|            | 6. Enter details                                       |
|            |                                                        |
|            |    a. WiFi name: the WiFi lamp should connect          |
|            |    to (also known as SSID)                             |
|            |                                                        |
|            |    b. WiFi password: password of WiFi to connect       |
|            |    to (leave empty if no password)                     |
|            |                                                        |
|            |    c. Channel key: the string identifier controlling   |
|            |    the beacon state                                    |
|            |    (read more in the  :ref:`ref_api` section)          |
|            |                                                        |
|            |    *Optional settings available from firmware version  |
|            |    0.84 and upwards:*                                  |
|            |                                                        |
|            |    d. State server: The channel state server to connect|
|            |    to.                                                 |
|            |    Default: api.cilamp.se                              |
|            |                                                        |
|            |    e. Port: The TCP port to connect to on              |
|            |    the channel state server. Default: 4040.            |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image4|  |                                                        |
|            |  7. This should then happen:                           |
|            |                                                        |
|            |     a. The Beacon will turn :blue:`blue` meaning it    |
|            |        is trying to connect to the router              |
|            |                                                        |
|            |     b. It will turn :purple:`purple` which means it is |
|            |        connecting to state server                      |
|            |                                                        |
|            |     c. Then it will turn :green:`green`                |
|            |        if all goes well!                               |
|            |                                                        |
+------------+--------------------------------------------------------+


.. hint::

    If you have any issues, please refer to troubleshooting_.

.. _troubleshooting: https://cilamp.se/setup-guide/#1498746921926-4127dd4e-44a5



.. note:: To reset or reconfigure a BI-Beacon, reboot it whilst it cannot
          connect to the router. E.g by doing any of the following:

          1. Change SSID or password on the router
          2. Turn off the router temporarily
          3. Move the BI-Beacon far away from the router

          When you plug it in again, the Beacon will try to connect and
          fail. It will then go into setup mode again.


Setup BI-Beacon via USB
-----------------------

You can also setup a BI-Beacon using a USB data cable. (This, however, requires
that your user is on a Linux machine and has the proper permissions (often this
means being part of the ``dialout`` user group.)

Clone the cli repo (see :ref:`ref_repositories`).

Then, in a terminal of your choice,

   $ python config_via_usb.py <ssid> <password> <channelkey> [stateserver] [port]

