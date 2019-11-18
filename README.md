# IPMI Fan Control for Dell PowerEdge Servers

A simple daemon script implementing a custom fan curve on Dell PowerEdge servers using IPMI.

I wrote this for my own purposes. If you want to use this, do it on your own risk. This hasn't been tester thoroughly or under heavy loads. Might cause damage due to overheating in the worst case.

# Deployment

1) Make sure your PowerEdge server's iDRAC has IPMI enabled.

2) Make sure the system running this script has atleast `bash`, `ipmitool` and `envsubst`.

3) Set the following values inside the script `ipmi`:

```
IPMI_HOST=<ipmi ip or hostname>
IPMI_USER=<ipmi user>
IPMI_PASSWORD=<ipmi password>
```

4) You should be able to run the daemon now `./ipmi`

5) In order to install the daemon as a systemd service, run `./install`. This, by default 1) copies the ipmi binary to /srv/ipmi-fancontrol/ 2) creates a system user to run the daemon as 3) Creates a systemd service `ipmi-fancontrol` 4) enables and starts the service.

6) Some of the default options are prompted when you run the install scripts. Others can be modified inside the script.

# Important Notes:

This has been tested only on Dell PowerEdge R620 running Ubuntu 19.10. This repository includes a daemon script, that adjusts fan speeds according to the maximum temperature reading from any sensor. The thresholds and fan speeds are defined in the script `ipmi`. Temperatures should be in C and fan speeds can range 0-100 (typed in hex 0x00-0x64).

The daemon enables static/manual fan control on start, and disables it upon termination (SIGTERM, SIGINT). So stopping the service or quitting the script with Ctrl-C should return the fan control to normal.
