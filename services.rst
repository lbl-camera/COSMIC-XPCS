Services on Tsuru
=================

Two services exist on Tsuru to automate startup of resources: a Caproto IOC service and an EPICS IOC service. Their unit files are at :code:`/etc/systemd/system/`.

The log of these services can be read by :code:`journalctl -u ...`. The services can be configured with :code:`systemctl`.


Caproto IOC
-----------

.. code-block::

    [Unit]
    Description=Caproto IOC Service
    Requires=network.target
    After=network.target

    [Service]
    ExecStart=/home/rp/PycharmProjects/alsdac/venv/bin/python /home/rp/PycharmProjects/alsdac/alsdac/caproto/__init__.py

    [Install]
    WantedBy=multi-user.target

EPICS IOC
---------

.. code-block::

    [Unit]
    Description=EPICS IOC Service
    Requires=network.target
    After=network.target

    [Service]
    ExecStart=/usr/local/epics/R7.0.1.1/support/areadetector/3-2/ADFastCCD/iocs/FastCCDIOC/iocBoot/iocFastCCD/st.cmd

    [Install]
    WantedBy=multi-user.target

Note
----
For more information, see `Arch's systemd <https://wiki.archlinux.org/index.php/systemd>`_.

https://serverfault.com/questions/821575/systemd-run-a-python-script-at-startup-virtualenv