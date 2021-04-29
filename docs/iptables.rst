iptables
========

mongo
-----

On tsuru,
the iptables service is configured to accept incoming connections to mongo
for only a few whitelisted internal IPs.
All other attempts to connect to mongo will be rejected.

To see the current firewall rules, run:

.. code-block:: bash

   less /etc/iptables/iptables.rules

.. danger::
    When modifying rules, be sure to activate root's cronjob
    for auto-flushing and resetting rules;
    otherwise, lockout may occur!

You can toggle on/off the safety root cronjob by running:

.. code-block:: bash

   sudo crontab -e

then removing/adding a '#' to the beginning of the line that contains the
:code:`/root/fwstop.sh` command.

You can list root cronjobs by running:

.. code-block:: bash

   sudo crontab -l

This will show you if it is enabled or not (if it is commented out or not).

.. warning::
    When you are finished modifying rules,
    it may be useful to save the current iptables rules to a backup file.
    Then, you will need to save the rules to :code:`/etc/iptables/iptables.rules`
    by running :code:`iptables-save -f /etc/iptables/iptables.rules`.

    **You will also need to disable the root cronjob for flushing the rules!**
