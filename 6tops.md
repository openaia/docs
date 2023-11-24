# 6TOPS

List of 6TOPS NPU-based AI Accelerators supported by OpenAIA.

- **Edgeble AI**
  - Edgeble NCM6A (consumer grade, 0°C to +80°C)
  - Edgeble NCM6B (industrial grade, -40°C to +85°C)

## Quick Start

- **NCM6B Kit**
  - Power
  - Debug UART
  - 4G/5G Connections

- **Factory Boot**
  - MaskROM
  - Boot from eMMC
  - Boot from SDMMC

## OpenAIA

### OpenAIA v1.0
- **Compile OpenAIA**

  - [Debmodel](https://github.com/openaia/debmodel#readme) - For Debos based OpenAIA
  - [meta-openaia](https://github.com/openaia/meta-openaia#readme) - For Yocto/OE layer based OpenAIA

- **Compile Linux Kernel**
- **Compile U-Boot**

- **Flash Program**
  - Program eMMC
  - Program SDMMC
  - Program SATA
  - Program M.2 M-Key

## IO-NCM6B Ports

- PMIC (rk806)
- eMMC
- LPDDR4X
- OTP
###  WiFi6

- **Check wireless Interface Enabled**

        openaia@6tops:~$ sudo ifconfig

        wlP3p49s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            ether 36:be:f6:1c:24:34  txqueuelen 1000  (Ethernet)
            RX packets 0  bytes 0 (0.0 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

- **Enable Wifi**

        openaia@6tops:~$ nmcli radio wifi
        enabled
        openaia@6tops:~$ nmcli radio wifi o


- **List Wifi AccessPoints**

        openaia@6tops:~$ nmcli dev wifi list
        IN-USE  BSSID              SSID                 MODE   CHAN  RATE        SIGNAL>
        *   D4:6E:0E:E6:A7:5A  AP-1                   Infra  11    270 Mbit/s  89    >
            9C:53:22:8A:15:0E  AP-2                   Infra  6     130 Mbit/s  52    >


- **Connect**

        openaia@6tops:~$ sudo nmcli dev wifi connect "wifi_name" password "wifi_password"
        Device 'wlP3p49s0' successfully activated with 'd0d1f61e-6093-4830-9dea-fa2e0d21b2d5'.


- **Check wireless Interface for IP address**

        openaia@6tops:~$ sudo ifconfig

        wlP3p49s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.113  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::a577:9922:977e:e49b  prefixlen 64  scopeid 0x20<link>
        ether a4:34:d9:0b:85:ce  txqueuelen 1000  (Ethernet)
        RX packets 34  bytes 5022 (4.9 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 33  bytes 4427 (4.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


- **Ping to Network**

        openaia@6tops:~$ ping google.com
        Device 'wlP3p49s0' successfully activated with 'd0d1f61e-6093-4830-9dea-fa2e0d21b2d5'.
        PING google.com (142.250.195.238) 56(84) bytes of data.
        64 bytes from maa03s43-in-f14.1e100.net (142.250.195.238): icmp_seq=1 ttl=59 time=44.4 ms
        64 bytes from maa03s43-in-f14.1e100.net (142.250.195.238): icmp_seq=2 ttl=59 time=18.4 ms
        ^C
        --- google.com ping statistics ---
        2 packets transmitted, 2 received, 0% packet loss, time 1002ms
        rtt min/avg/max/mdev = 18.356/31.383/44.411/13.027 ms


- CAM0
- CAM1

## IO-NCM6B Ports

- SD Card
- PWM Fan
- RTC
- RS232
- RS485
- CAN
- Buttons
- LED
- POE
- 40PIN Header
- USB 2.0
- USB 3.0
- 2.5G Ethernet
- Audio Playback
- Audio Record
- SATA
- M.2 M-Key (WiFi)

### M.2 E-Key (4G)
- **Link peripheral**

  First, connect 4G Module to board .
  You can check if the device is connected by the following command:

      openaia@6tops:~$ lsusb | grep -i Quectel
      Bus 001 Device 003: ID 2c7c:0125 Quectel Wireless Solutions Co., Ltd. EC25 LTE modem

  The modem usually communicates with the host computer through a serial port.
  Check whether the system correctly enumerates the corresponding serial port devices:

      openaia@6tops:~$ ls /dev/ttyUSB*
      /dev/ttyUSB0  /dev/ttyUSB1  /dev/ttyUSB2  /dev/ttyUSB3

- **Test the modem**

  First, open the serial port using picocom :

      openaia@6tops:~$ sudo picocom -b 115200 /dev/ttyUSB3

  After the program starts, you can enter the following at command to check the status of the modem:

      Terminal ready
      at+cpin?
      +CPIN: READY
      OK  #Check whether the SIM card is in place

      at+csq
      +CSQ: 11,99

      OK  #Detection signal. 99 means no signal

      at+cops?
      +COPS: 0,0,"JIO 4G Jio",7

      OK #View Carrier

      at+creg?
      +CREG: 0,1

      OK  #Get the registration status of the phone (0,1: indicates normal registration)

      at+qeng="servingcell"
      +QENG: "servingcell","NOCONN","LTE","FDD",405,854,49E31,91,2450,5,3,3,74,-124,-16,-91,-5,3

      OK  #Signal strength and quality of the currently connected service cell

  If the modems return to normal, you can use the Ctrl+A Ctrl+X key combination to exit picocom .

      Terminating...
      Thanks for using picocom
      openaia@6tops:~$


- **Dial-up Internet via ppp**

  You can now try dialing using ppp :

        openaia@6tops:~$ sudo pppd call rasppp &    #Background dialing

  The complete dial-up process is shown as follows:

        openaia@6tops:~$ sudo pppd call rasppp &

        [1] 540
        openaia@6tops:~$
        pppd options in effect:
        debug           # (from /etc/ppp/peers/rasppp)
        nodetach                # (from /etc/ppp/peers/rasppp)
        dump            # (from /etc/ppp/peers/rasppp)
        noauth          # (from /etc/ppp/peers/rasppp)
        user ctnet@mycdma.cn            # (from /etc/ppp/peers/rasppp)
        password ??????         # (from /etc/ppp/peers/rasppp)
        remotename 3gppp                # (from /etc/ppp/peers/rasppp)
        /dev/ttyUSB3            # (from /etc/ppp/peers/rasppp)
        115200          # (from /etc/ppp/peers/rasppp)
        lock            # (from /etc/ppp/peers/rasppp)
        connect /usr/sbin/chat -s -v -f /etc/ppp/peers/rasppp-chat-connect              # (from /etc/ppp/peers/rasppp)
        disconnect /usr/sbin/chat -s -v -f /etc/ppp/peers/rasppp-chat-disconnect                # (from /etc/ppp/peers/rasppp)
        crtscts         # (from /etc/ppp/peers/rasppp)
        local           # (from /etc/ppp/peers/rasppp)
        asyncmap 0              # (from /etc/ppp/options)
        lcp-echo-failure 4              # (from /etc/ppp/options)
        lcp-echo-interval 30            # (from /etc/ppp/options)
        hide-password           # (from /etc/ppp/peers/rasppp)
        novj            # (from /etc/ppp/peers/rasppp)
        novjccomp               # (from /etc/ppp/peers/rasppp)
        ipcp-accept-local               # (from /etc/ppp/peers/rasppp)
        ipcp-accept-remote              # (from /etc/ppp/peers/rasppp)
        ipparam 3gppp           # (from /etc/ppp/peers/rasppp)
        noipdefault             # (from /etc/ppp/peers/rasppp)
        defaultroute            # (from /etc/ppp/peers/rasppp)
        usepeerdns              # (from /etc/ppp/peers/rasppp)
        noccp           # (from /etc/ppp/peers/rasppp)
        noipx           # (from /etc/ppp/options)
        timeout set to 15 seconds
        abort on (BUSY)
        abort on (ERROR)
        abort on (NO ANSWER)
        abort on (NO CARRTER)
        abort on (NO DIALTONE)
        send (AT^M)
        expect (OK)
        AT^M^M
        OK
        -- got it

        send (^MATZ^M)
        expect (OK)
        ^M
        ATZ^M^M
        OK
        -- got it

        send (^MAT+CGDCONT=1,"IP",""^M)
        expect (OK)
        ^M
        AT+CGDCONT=1,"IP",""^M^M
        OK
        -- got it

        send (ATDT#777^M)
        expect (CONNECT)
        ^M
        ATDT#777^M^M
        CONNECT
        -- got it

        send (\d)
        Script /usr/sbin/chat -s -v -f /etc/ppp/peers/rasppp-chat-connect finished (pid 555), status = 0x0
        Serial connection established.
        using channel 1
        Using interface ppp0
        Connect: ppp0 <--> /dev/ttyUSB3
        sent [LCP ConfReq id=0x1 <asyncmap 0x0> <magic 0xfb465ca3> <pcomp> <accomp>]
        rcvd [LCP ConfReq id=0x0 <asyncmap 0x0> <auth chap MD5> <magic 0x2bd81892> <pcomp> <accomp>]
        sent [LCP ConfAck id=0x0 <asyncmap 0x0> <auth chap MD5> <magic 0x2bd81892> <pcomp> <accomp>]
        rcvd [LCP ConfAck id=0x1 <asyncmap 0x0> <magic 0xfb465ca3> <pcomp> <accomp>]
        sent [LCP EchoReq id=0x0 magic=0xfb465ca3]
        rcvd [LCP DiscReq id=0x1 magic=0x2bd81892]
        rcvd [CHAP Challenge id=0x1 <7767d88a1a163c58bfa43a909cb088d7>, name = "UMTS_CHAP_SRVR"]
        sent [CHAP Response id=0x1 <780ce605b4caf11cb42905af945adf04>, name = "ctnet@mycdma.cn"]
        rcvd [LCP EchoRep id=0x0 magic=0x2bd81892 fb 46 5c a3]
        rcvd [CHAP Success id=0x1 ""]
        CHAP authentication succeeded
        CHAP authentication succeeded
        kernel does not support PPP filtering
        sent [IPCP ConfReq id=0x1 <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        rcvd [IPCP ConfReq id=0x0]
        sent [IPCP ConfNak id=0x0 <addr 0.0.0.0>]
        rcvd [IPCP ConfRej id=0x1 <ms-dns2 0.0.0.0>]
        sent [IPCP ConfReq id=0x2 <addr 0.0.0.0> <ms-dns1 0.0.0.0>]
        rcvd [IPCP ConfReq id=0x1]
        sent [IPCP ConfAck id=0x1]
        rcvd [IPCP ConfNak id=0x2 <addr 10.221.128.38> <ms-dns1 49.45.0.1>]
        sent [IPCP ConfReq id=0x3 <addr 10.221.128.38> <ms-dns1 49.45.0.1>]
        rcvd [IPCP ConfAck id=0x3 <addr 10.221.128.38> <ms-dns1 49.45.0.1>]
        Could not determine remote IP address: defaulting to 10.64.64.64
        Script /etc/ppp/ip-pre-up started (pid 567)
        Script /etc/ppp/ip-pre-up finished (pid 567), status = 0x0
        local  IP address 10.221.128.38
        remote IP address 10.64.64.64
        primary   DNS address 49.45.0.1
        Script /etc/ppp/ip-up started (pid 570)
        Script /etc/ppp/ip-up finished (pid 570), status = 0x0
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        sent [IPV6CP ConfReq id=0x1 <addr fe80::8c4f:5739:ce37:d292>]
        openaia@6tops:~$ IPV6CP: timeout sending Config-Requests

  From the output of the program we can get the following information:

        1.local IP address   : 10.221.128.38
        2.primary DNS server : 49.45.0.1

  We can now configure the network based on the above information:

        # configure the gateway
        openaia@6tops:~$ sudo ip route add default via 10.221.128.38

        # configure primary DNS
        openaia@6tops:~$ echo "nameserver 49.45.0.1" | sudo tee -a /etc/resolv.conf

  Check the updated config of ppp using ifconfig

        openaia@6tops:~$ sudo ifconfig
        enP2p33s0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
                ether f6:61:39:79:d5:0c  txqueuelen 1000  (Ethernet)
                RX packets 0  bytes 0 (0.0 B)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 0  bytes 0 (0.0 B)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
                device interrupt 132

        lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
                inet 127.0.0.1  netmask 255.0.0.0
                inet6 ::1  prefixlen 128  scopeid 0x10<host>
                loop  txqueuelen 1000  (Local Loopback)
                RX packets 0  bytes 0 (0.0 B)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 0  bytes 0 (0.0 B)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

        ppp0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
                inet 10.221.128.38   netmask 255.255.255.255  destination 10.64.64.64
                ppp  txqueuelen 3  (Point-to-Point Protocol)
                RX packets 5  bytes 50 (50.0 B)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 47  bytes 2032 (1.9 KiB)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

        wlP3p49s0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
                ether aa:c7:c0:2d:22:7e  txqueuelen 1000  (Ethernet)
                RX packets 0  bytes 0 (0.0 B)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 0  bytes 0 (0.0 B)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

        wlan1: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
                ether 5e:52:74:40:c6:ac  txqueuelen 1000  (Ethernet)
                RX packets 0  bytes 0 (0.0 B)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 0  bytes 0 (0.0 B)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


  You can now use the ping command to check if you are connected to the Internet:

        openaia@6tops:~$ ping google.com
        PING google.com (142.250.196.46) 56(84) bytes of data.
        64 bytes from maa03s45-in-f14.1e100.net (142.250.196.46): icmp_seq=2 ttl=113 time=308 ms
        64 bytes from maa03s45-in-f14.1e100.net (142.250.196.46): icmp_seq=3 ttl=113 time=1626 ms
        64 bytes from maa03s45-in-f14.1e100.net (142.250.196.46): icmp_seq=4 ttl=113 time=1570 ms


- M.2 B-Key (5G)
- HDMI Out
- Display Port 0
- Display Port 1
- MIPI DSI 0
- MIPI DSI 1
- eDP
- HDMI In
- CAM2
- CAM3
- CAM4
- CAM5
