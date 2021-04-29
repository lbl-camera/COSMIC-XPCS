Tsuru and BL701-IOC Services and Configuration Guide
====================================================

This guide will show:

* how to start up IOCs on tsuru and bl701-ioc1
* which unit files need to be active
* how the network is configured on tsuru
* which network service should be active
* some useful commands for troubleshooting

Starting up IOCs
----------------

IOCs:

* tsuru
    * epics
    * caproto
    * fastccd_support_ioc
* bl7011-ioc1
    * delay_generator_ioc
    * lakeshore_ioc

tsuru
^^^^^

1. Check IOC Units Are Enabled
""""""""""""""""""""""""""""""

ssh to tsuru, then run:

.. code-block:: bash
   
   systemctl list-unit-files --state=enabled

to see which services are enabled.
The ``caproto``, ``epics``, and ``fastccd_support_ioc`` units should be enabled.

*If they are disabled, they can be enabled as follows:*

.. code-block:: bash

   sudo systemctl enable epics caproto fast_support_ioc

2. Start/Restart the IOCs
"""""""""""""""""""""""""

.. code-block:: bash

   sudo systemctl restart epics
   sudo systemctl restart caproto
   sudo systemctl restart fastccd_support_ioc

3. Check the Services
"""""""""""""""""""""

.. code-block:: bash

   systemctl status epics caproto fastccd_support_ioc

4. Check the Network
""""""""""""""""""""

.. code-block:: bash
   
   ip address

This will show the ip addresses for the interfaces on tsuru.

* 2: enp25s0f0,  fabric connection
* 4: enp179s0f0, local network
* 5: enp179s0f1, lab network

Make sure these interfaces have an **UP** state
and have **inet** addresses set to valid IPv4 type addresses.

.. code-block:: bash

   netctl list

This will show all the configured profiles.
Active profiles will have a '\*' next to them::

    * enp179s0f0
    * enp179s0f1
    * enp25s0f0

*These three profiles (interfaces) should all be active.*

bl7011-ioc1
^^^^^^^^^^^

1. Check IOC Units Are Enabled
""""""""""""""""""""""""""""""

ssh to bl7011-ioc1, then run:f

.. code-block:: bash

   systemctl list-unit-files --state=enabled

to see which services are enabled.
The ``delay_generator_ioc`` and ``lakeshore_ioc`` units should be enabled.

.. note::
    If they are disabled, they can be enabled as follows:

    :code:`sudo systemctl enable delay_generator_ioc lakeshore_ioc`

2. Start/Restart the IOCs
"""""""""""""""""""""""""

.. code-block:: bash

   sudo systemctl restart delay_generator_ioc
   sudo systemctl restart lakeshore_ioc

3. Check the Services
"""""""""""""""""""""

.. code-block:: bash

   systemctl status epics caproto fastccd_support_ioc

Resources
---------

tsuru
^^^^^

===================================  ====================
Unit File                            Description 
-----------------------------------  --------------------
:code:`caproto.service`              caproto IOC
:code:`dhcpcd.service`               DHCP client 
:code:`epics.service`                EPICS IOC
:code:`fastccd_support_ioc.service`  FastCCD IOC
:code:`iptables.service`             firewall/routing
:code:`mongodb.service`              Mongo database
:code:`netctl.service`               net interface config
===================================  ====================

*The only networking service running should be :code:`netctl` (with :code:`dhcpcd`)!
If other networking services like :code:`NetworkManager` or :code:`systemd-networkd` are running,
connection errors may occur!*

bl7011-ioc1
^^^^^^^^^^^

===================================  ======================
Unit File                            Description 
-----------------------------------  ----------------------
:code:`delay_generator_ioc.service`  Delay Generator IOC
:code:`lakeshore_ioc.servie`         Temperature Sensor IOC
===================================  ======================

*There is no custom configured networking service here.
By default, something like the :code:`NetworkManager` services will be enabled.*

systemctl 
^^^^^^^^^

systemctl list-unit-files --state=enabled
    list all enabled installed units
ls /etc/systemd/system
    show system-level unit configuration files
ls /usr/lib/systemd/system
    show user-level unit configuration files
journalctl -u UNIT
    look at journal (logs) for the unit (service)
systemctl start UNIT
    start (activate) the unit(s)
systemctl stop UNIT
    stop (deactivate) the unit(s)
systemctl restart UNIT
    stop then start the unit(s)

netctl
^^^^^^

netctl list
    list all profiles, active ones marked with a '\*'
netctl status PROFILE
    check the status of the PROFILE
/etc/netctl
    directory where profiles (configurations) are located
netctl start PROFILE
    start the PROFILE (will make it active)
ip address
    show ip addresses on this machine

