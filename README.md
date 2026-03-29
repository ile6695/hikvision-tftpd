Unbrick or upgrade a Hikvision device. Use as follows:

Download the firmware for your device. Usually there's a zip file containing digicap.dav

https://www.hikvision.com/en/support/download/firmware/

Run the script:

    $ sudo python3 hikvision_tftpd.py

**Hit Ctrl-C when done.**

The Hikvision TFTP handshake (for both cameras and NVRs) is stupid but easy
enough. The client sends a particular packet to the server's port 9978 from
the client port 9979 and expects the server to echo it back.  Once that
happens, it proceeds to send a tftp request (on the standard tftp port, 69)
for a specific file, which it then installs. The tftp server must reply
from port 69.

This script handles both the handshake and the actual TFTP transfer.
The TFTP server is very simple but appears to be good enough.

Note the expected IP addresses and file name appear to differ by model. So far
there are some known configurations:

| client IP    | server IP     | filename      |
| ------------ | ------------- | ------------- |
| 192.168.1.18 | 192.168.1.128 | `digicap.dav` |
| 192.0.0.64   | 192.0.0.128   | `digicap.dav` |
| 172.9.18.100 | 172.9.18.80   | `digicap.mav` |

This program defaults to 192.168.1.128. Others require commandline overrides:

    $ sudo python3 hikvision_tftp.py --server-ip=172.9.18.80 --filename=digicap.mav

If nothing happens when your device restarts, your device may be expecting
another IP address. Wireshark or tcpdump may be used to see the ip the camera searches in its ARP messages.

See [discussion thread](https://www.ipcamtalk.com/showthread.php/3647-Hikvision-DS-2032-I-Console-Recovery).
