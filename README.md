# wlanpi-extcap
Wireshark extcap interface for the [WLAN Pi](www.wlanpi.com). It allows you to perform live captures on a specific channel using the WLAN Pi's Wi-Fi adapter.

This extcap interface is basically a wrapper for [sshdump](https://www.wireshark.org/docs/man-pages/sshdump.html) that includes an option to choose the channel we want to capture on. It also simplifies the configuration of the extcap interface so that the user doesn't deal with remote capture commands, etc.

## Requirements

### WLAN Pi
Requires WLAN Pi 1.8.1.

### Windows
The `wlanpidump` extcap interface requires the `sshdump` extcap interface, which is not installed by default on Windows. When installing Wireshark on Windows, select __SSHdump__ as one of the components to install:

<p align="center">
<img src="../master/images/wireshark-installer-sshdump.png" alt="Wireshark Installer SSHdumpr" height="400px">
</p>

## Setup

### WLAN Pi:
1. Create the file `/etc/sudoers.d/wlanpidump` with the following line:
```sh
wlanpi ALL = (root) NOPASSWD: /sbin/iwconfig, /usr/sbin/iw
```
> __Note__: This is required so that the extcap interface can put the Wi-Fi adapter into monitor mode and change the channel before starting the capture.

### If you're running Wireshark on macOS:
1. Copy `wlanpidump` to `/Applications/Wireshark.app/Contents/MacOS/extcap/`
2. Make sure it has execution permissions:

```sh
chmod +x /Applications/Wireshark.app/Contents/MacOS/extcap/wlanpidump
```

### If you're running Wireshark on Windows:

1. Copy `wlanpidump` to `C:\Program Files\Wireshark\extcap\`
2. Create a file called `wlanpidump.bat` in the same `C:\Program Files\Wireshark\extcap\` directory with the following content: 

```bat
@echo off
<PATH_TO_PYTHON_INTERPRETER> <PATH_TO_WLANPIDUMP> %*
```

Where _<PATH_TO_PYTHON_INTERPRETER>_ is the path to the Python executable and _<PATH_TO_WLANPIDUMP>_ is the path to the `wlanpidump` extcap interface script. For example:

```bat
@echo off
"C:\Program Files (x86)\Python37-32\python.exe" "C:\Program Files\Wireshark\extcap\wlanpidump" %*
```

Now launch Wireshark and verify that __WLAN Pi remote capture__ is listed as an extcap interface:

![WLAN Pi Extcap Interface](../master/images/wlanpidump-interface.png "WLAN Pi Extcap Interface")

> __Note__: You will have to repeat the setup of the `wlanpidump` extcap interface on your computer each time you update Wireshark. The Wireshark installer doesn't preserve 3rd-party extcap interfaces added to the _extcap_ folder.

## Usage

1. Click the _gear_ icon next to "WLAN Pi remote capture" to display the interface options, then choose the channel (and channel width) you want to capture on:

![WLAN Pi Extcap Interface Options](../master/images/wlanpidump-interface-options.png "WLAN Pi Extcap Interface Options")

> __Note:__ All 802.11 channels are listed, however, the Wi-Fi adapter on the WLAN Pi may support only a subset of them. If you choose a channel that is not supported by the Wi-Fi adapter or a channel width that doesn't apply to the selected channel, the capture will fail.
2. Go to the _Server_ tab and enter the WLAN Pi SSH address, e.g. 192.168.42.1.

<p align="center">
<img src="../master/images/wlanpidump-interface-options-server.png" alt="WLAN Pi Extcap Interface Options - Server" height="200px">
</p>

3. Go to the _Authentication_ tab and enter the username and password.

<p align="center">
<img src="../master/images/wlanpidump-interface-options-auth.png" alt="WLAN Pi Extcap Interface Options - Auth" height="200px">
</p>

> __Note:__ To avoid having to enter the password each time you start a capture, setup passwordless SSH authentication and skip this step.

4. Click the __Start__ button to start the capture.
