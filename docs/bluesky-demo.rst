Demonstration of Bluesky on Tsuru
=================================

This tutorial will guide a user through accessing Tsuru, initializing a `RunEngine` in an IPython console, and executing simple plans using simulated devices

Connecting to Tsuru
-------------------

You can access Tsuru by connecting over ssh to :code:`tsuru.dhcp.lbl.gov`. A generic user account is provided, named 'alsuser'. Upon connecting, a virtual python environment will automatically be activated which includes Bluesky, Xi-cam, and all related components. The :code:`(venv)` prefix on the bash prompt will indicate this.

Getting Started
---------------

To begin, start an interactive Python console by running `ipython` at the command line. A profile specialized to the 7011 beamline is automatically loaded, which includes pre-defined ophyd objects specific to this beamline, as well as a Bluesky RunEngine pre-initialized with a databroker instance such that all documents generated from plans are archived into the databroker.

A few simulated devices are pre-defined by ophyd for convenience. Bluesky has more extensive documentation on this topic, and a `lengthier tutorial <https://nsls-ii.github.io/bluesky/tutorial.html>`_ you can follow.

Try a simple plan! This plan scans simulated detector over a simulated motor range.

.. code-block:: python

    from ophyd.sim import det, motor
    from bluesky.plans import scan
    dets = [det]   # just one in this case, but it could be more than one

    RE(scan(dets, motor, -1, 1, 10))

Other cool things
-----------------

Besides the IPython console, you can repeat the same exercise using JupyterHub. This is a web-based front-end to Python, providing similar access to an interactive python interpreter, but with the added benefit of a notebooke-style display. To access this, navigate a web browser to `<http://tsuru.dhcp.lbl.gov>`_ and login. Create a new notebook to get started. You can run the same test as above.

