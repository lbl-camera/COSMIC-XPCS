FastCCD Startup Process
=======================

Cautions
--------

Before running the FastCCD, check the state of the power supplies and cooling. The FCRIC and timing configuration can be modified live, however bias values should not be modified without turning off camera bias and clocks; otherwise damage to the sensors may result.


Setup the FastCCD
-----------------

The FastCCD can be initialized and checked from fastcameraATCA1.dhcp.lbl.gov when coming online.

- Connect to fastcameraATCA1.dhcp.lbl.gov via SSH and start the VNC server using the following commands

        vncserver -list
            shows last vnc process; the process shown may actually have ended, but you can restart it with below commands

        vncserver -kill :11
            kills the vnc server on window 11

        vncserver :11 -geometry 2400x1400
            starts a vnc server on window 11 with a screen size of 2400x1400


- Start the VNC client and connect to fastcameraATCA1.dhcp.lbl.gov
- Run the auto-start script

    ~/CameraControl/python/fccd_auto_start.py
        Sets up the FCCD. When running this script the Camera Interface Node led panel's bottom-right light will cycle in order red>yellow>green as it configures the device.

- Check the fiber optics status

    ~/CameraControl/python/get_FOPS_Status.py
        This script prints the monitor values of the fiber optics hardware. It should show approx. 4.49 V and 2.71 A.
- Launch the FCCD gui

    ~/CameraControl/fccd_gui/CINController.py
        Launches the main
- Click [Connect]
- Under [Expose], select Exposure Mode -> Continuous

    When all modules are masked in the fcric, the standard test pattern is sent. The fiber optic test pattern can also be sent; this is toggled with:
        - FOtestpatter_on.py
        - FOtestpattern_off.py

Starting Tsuru
--------------

Tsuru is fully configured to start automatically. The FCCD must be closed for Tsuru to receive data.

Notes
-----

- Configure Tsuru networking
    - 2: ens420 = 10G -> 10.0.6.42
    - 4: enp179sf0 = Base -> 192.168.1.42