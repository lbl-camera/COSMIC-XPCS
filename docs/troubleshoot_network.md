# Troubleshooting Network Issues

## Step 1: Check Network Addresses (on tsuru)

After ssh'ing to tsuru, run:

```
ip address
```

Interfaces 2, 4, and 5 should be UP.

* 2 - fabric switch
* 4 - local network
* 5 - lab network

If DOWN, run Ron's setup scripts to configure the ip addresses for the network interfaces:

```
bash ~rp/setup.sh
ip address
```

## Step 2: Initializing Camera

We want to make sure the camera is initialize when testing the network connections.

First, log in to tsuru (vnc is helpful here to run Xi-CAM).

If the Controls GUI does not show in Xi-CAM Acquire after selecting `fastccd` device,
check that you have `$PASSWD_MONGO` and `$USER_MONGO` set.

If initalizing the camera (via the `Initialize` button) fails, check the service logs:

```
journalctl -f -u fastccd_support_ioc
```

This may show something like `socket.timeout=timed out`.

Re-check the network interface addresses (Step 1).

Retry the initialization of the camera in Xi-CAM.

## Step 3: Check connection between tsuru and CIN

We will then want to test the connection between tsuru and the camera interface node.

CIN IP: `10.0.5.207`

On tsuru, run:

```
sudo ping -f 10.0.5.207
```

After a few seconds, CTRL-C and check packet loss (20% is usual).

Next, we will want to look at the connection from the fastccdATCA1 server to the fabric switch.

## Step 4: Check connection between fastccdATCA1 and CIN

ssh to fastccdatca1.dchp.lbl.gov and repeat the ping flood from step 3.

This may also show packet loss of around 20%.

## Step 5: Check fastccdATCA1 and tsuru network configuration
**is this relevant??? -- recording for note purposes, modify later**

On fastcameraatca1 and tsuru, check the `net.core.rmem_default` and `net.core.rmem_max` values:

```
systemctl net.core.rmem_default
systemctl net.core.rmem_max
```

Max should match default. If not, modify `net.core.rmem_default` to match the `net.core.rmem_max` in the following files:

* `/etc/sysctl.conf`
* `/etc/rc.d/rc.local`

Then, reboot:

```
sudo shutdown -r 0
```

After rebooting, reconnect and repeat steps 3 and 4.

