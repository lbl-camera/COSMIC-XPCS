Services on Tsuru
=================

Three services exist on Tsuru to automate startup of resources: a Caproto IOC service, an EPICS IOC service, and a
Jupyter Hub service. Their unit files are at :code:`/etc/systemd/system/`.

The log of these services can be read by :code:`journalctl -u ...`. The services can be configured with
:code:`systemctl`.


Caproto IOC
-----------

.. code-block:: jproperties

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

.. code-block:: jproperties

    [Unit]
    Description=EPICS IOC Service
    Requires=network.target
    After=network.target

    [Service]
    ExecStart=/usr/local/epics/R7.0.1.1/support/areadetector/3-2/ADFastCCD/iocs/FastCCDIOC/iocBoot/iocFastCCD/st.cmd

    [Install]
    WantedBy=multi-user.target

Jupyter Hub
-----------

.. code-block:: jproperties

    [Unit]
    Description=Jupyterhub
    After=syslog.target network.target

    [Service]
    User=root
    ExecStart=/usr/bin/jupyterhub -f /etc/jupyter/jupyterhub_config.py

    [Install]
    WantedBy=multi-user.target

The above unit file is at :code:`/lib/systemd/system/`.

Note
----
For more information, see `Arch's systemd <https://wiki.archlinux.org/index.php/systemd>`_.

https://serverfault.com/questions/821575/systemd-run-a-python-script-at-startup-virtualenv