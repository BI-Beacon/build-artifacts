.. documents how to configura a beacon

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
|  |image1|  | 1. Plugin your BI-Beacon (it turns blue)               |
|            | 2. Disable mobile network (if on mobile device)        |
|            | 3. Wait up to 30 seconds until BI-Beacon starts        |
|            |    shifting in slowly between blue and green           |
|            |    (it means setup mode is active)                     |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image2|  |                                                        |
|            | 4. Connect to WiFi “BI-Beacon-A1B2C3” (A1B2C3          |
|            |    is unique for your lamp)                            |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image3|  |                                                        |
|            | 5. Browse to BI-Beacon’s Setup Page:                   |
|            |    http://192.168.4.1                                  |
|            |                                                        |
|            | 6. Enter details                                       |
|            |                                                        |
|            |    a. WiFi name: the WiFi lamp should connect          |
|            |    to (SSID)                                           |
|            |    b. WiFi password: password of WiFi to connect       |
|            |    to (empty if no password)                           |
|            |    c. Systemid: the string identifier controlling      |
|            |    the beacon state (see :ref:`ref_api`)               |
|            |                                                        |
+------------+--------------------------------------------------------+
|  |image4|  |                                                        |
|            |  7. Wait for green                                     |
|            |                                                        |
|            |     a. It will first turn blue which means it is       |
|            |        connecting to router                            |
|            |     b. Then it will turn purple which means it is      |
|            |        connecting to state server                      |
|            |     c. Then it will turn green when connected          |
|            |                                                        |
+------------+--------------------------------------------------------+

If you have any issues, please refer to troubleshooting_.

.. _troubleshooting: https://cilamp.se/setup-guide/#1498746921926-4127dd4e-44a5



.. note:: To reset or reconfigure a BI-Beacon, reboot it whilst it cannot
          connect to the router. E.g by doing any of the following:

          1. Change SSID or password on the router
          2. Turn off the router temporarily
          3. Move the BI-Beacon far away from the router

          When you plug it in again, BI-Beacon will try to connect and
          fail. It will then go into setup mode again.


Setup BI-Beacon via USB
-----------------------

You can also setup a BI-Beacon using a USB data cable, however this requires
that you are on a Linux machine with a user in the `dialout` usergroup.

Clone the cli repo (see :ref:`ref_repositories`).

Then, in a terminal of your choice,

   $ python config_via_usb.py <ssid> <password> <systemid>

