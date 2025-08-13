# eth0-for-lap-to-cm5

-----

### Netplan Configuration Documentation for Static IP

This document explains the configuration used to set up a static IP address and ensure a proper internet connection on a device using Netplan. This is particularly useful when automatic network configuration via DHCP fails to provide a default gateway, or when a consistent, unchanging IP address is required.

#### **Configuration Script**

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.137.10/24]
      routes:
        - to: default
          via: 192.168.137.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

#### **Explanation of Each Section**

  * **` network:  `**: This is the top-level key that indicates the beginning of the network configuration. All network settings are nested under this key.
  * **`version: 2`**: Specifies the version of the Netplan YAML format. Using version 2 is standard for modern Linux distributions.
  * **` ethernets:  `**: This section defines the settings for all Ethernet network interfaces. You can define multiple interfaces here.
  * **`eth0:`**: This is the specific name of the network interface being configured. In your case, this is the Ethernet port on your CM5. This name can vary (`eth1`, `eno1`, etc.) depending on the system's hardware and drivers.
  * **`dhcp4: false`**: This is a critical line that **disables the automatic acquisition of an IPv4 address** via DHCP. By setting this to `false`, you are telling the system that you will provide the network details manually.
  * **`addresses: [192.168.137.10/24]`**: This line assigns a **static IPv4 address** to the `eth0` interface.
      * **`192.168.137.10`**: This is the chosen static IP address for the device. You selected this IP within the subnet that your laptop's Internet Connection Sharing created.
      * **`/24`**: This is the **CIDR notation** for the subnet mask, which is equivalent to `255.255.255.0`. It tells the system that the first three octets of the IP address (`192.168.137`) define the local network.
  * **`routes:`**: This section defines the routing table entries for the interface.
      * **`- to: default`**: This specifies that this is the **default route** for all traffic that is not explicitly routed to a local network.
      * **`via: 192.168.137.1`**: This defines the **gateway IP address** for the default route. This is the IP address of your laptop's Ethernet adapter, which is acting as the router to the internet. All internet-bound traffic from the CM5 will be sent to this address first.
  * **`nameservers:`**: This section specifies the IP addresses of the DNS servers.
      * **`addresses: [8.8.8.8, 1.1.1.1]`**: These are the IP addresses of public DNS servers (Google and Cloudflare, respectively). They are used to **resolve domain names to IP addresses**, allowing you to access websites using names like `www.google.com`.
