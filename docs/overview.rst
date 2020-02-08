.. describes the scope of this documentation

BI-Beacon Overview
==================

.. image:: https://api.travis-ci.org/objarni/leanmodel.svg?branch=master

.. image:: https://sonarcloud.io/api/project_badges/measure?project=bibeacon-code-examples&metric=alert_status

This is the official documentation for Business Intelligence Beacon.
The primary focus of this documentation is the API to control Beacons,
however it also includes information on how to configure devices,
and some code examples for how to control Beacons from several different
languages.

.. tip::

    Read more about the ideas behind BI-Beacon, and order a Beacon for
    your company here:

    https://bi-beacon.se

In a nutshell: BI-Beacons use a RESTful API.

There is only one end-point: ``/<channelkey>``.

This endpoint serves as the controlling mechanism for *channel* with ID
`channelkey`.

Here is an example of a complete endpoint:

   ``https://api.bi-beacon.com/v1/our-beautiful-monitor``


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


.. _`Documentation repository`: https://github.com/BI-Beacon/docs
