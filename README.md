# wlanpi-extcap
Wireshark extcap interface for the [WLAN Pi](www.wlanpi.com). It allows you to perform live captures on a specific channel using the WLAN Pi's Wi-Fi adapter.

This extcap interface is basically a wrapper for [sshdump](https://www.wireshark.org/docs/man-pages/sshdump.html) that includes an option to choose the channel we want to capture on. It also simplifies the configuration of the extcap interface so that the user doesn't deal with remote capture commands, etc.

As it is, the extcap interface is only compatible with Wireshark on macOS (see [To-Do](#to-do)).

## Setup

### In your Mac:
1. Copy `wlanpidump` to `/Applications/Wireshark.app/Contents/MacOS/extcap/`
1. Make sure it has execution permissions:

```sh
chmod +x /Applications/Wireshark.app/Contents/MacOS/extcap/wlanpidump
```

1. Launch Wireshark and verify that __WLAN Pi remote capture__ is listed as an extcap interface:

![WLAN Pi Extcap Interface](../master/images/wlanpidump-interface.png "WLAN Pi Extcap Interface")

> __Note__: You will have to repeat this process each time you update Wireshark. The Wireshark installer doesn't preserve 3rd-party extcap interfaces added to the _extcap_ folder.

### In your WLAN Pi:
1. Create the file `/etc/sudoers.d/wlanpidump` with the following line:
```sh
wlanpi ALL = (root) NOPASSWD: /sbin/iwconfig, /usr/sbin/iw
```
> __Note__: This is required so that the extcap interface can put the Wi-Fi adapter into monitor mode and change the channel before starting the capture.

## Usage

1. Click the _gear_ icon next to "WLAN Pi remote capture" to display the interface options:

![WLAN Pi Extcap Interface Options](../master/images/wlanpidump-interface-options.png "WLAN Pi Extcap Interface Options")

2. Enter the WLAN Pi SSH address, e.g. 192.168.42.1.
3. Enter the username and password.
> __Note:__ To avoid having to enter the password each time you start a capture, setup passwordless SSH authentication and skip this step.
4. Choose the channel you want to capture on.
> __Note:__ All 802.11 channels are listed, however, the Wi-Fi adapter on the WLAN Pi may support only a subset of them. If you choose a channel that is not supported by the Wi-Fi adapter, nothing will be captured.
5. Click the _Start_ button to start the capture.

## To-Do

- Make it so only channels supported by the WLAN Pi's Wi-Fi adapter are listed in the "Channel" option.
- Add support for Windows and Linux.
- Improve error handling.
