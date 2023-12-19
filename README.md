# Inspecting Windows Infection
Example of windows infection traffic from commodity malware distribution.

Note:
* Filter bar is red = incorrect filter expression

* Filter bar is yellow = correct but will not work

* Filter bar is green = correct filter expression

* Use `!(ip.addr eq 192.168.10.1)` instead of this `ip.addr != 192.168.10.1` because it won't work as expected.

* Use `http.request or ssl.handshake.type == 1` to filter web traffic.

* `http.request` - filters HTTP traffic

* `ssl.handshake.type == 1` - filters HTTPS traffic

* UDP Port 1900 is Simple Service Discovery Protocol (SSDP) used to discover plug & play devices also not associated with normal web traffic.

* `(http.request or ssl.handshake.type == 1) and !(ssdp)` - used to exclude ssdp traffic.

* `tcp.flags eq 0x0002` - used to reveal connections with servers that are offline or refusing TCP connections.

* `dns` - filter used to find servers that are not web-based and directly hosted on ip address or domain names (e.g., C2)
    * RAT (is not HTTP/HTTPS/SSL/TLS)

> `(http.request or ssl.handshake.type == 1 or tcp.flags eq 0x0002 or dns) and !(udp.port eq 1900)` is now entire filter expression

* `ftp` - filter used to find file transfer traffic of the computer with the internet.

* `smtp` - used to filter mail traffic (spambot malware can send hundreds of email every minute).

* `smtp contains "From:"` - used to find unecrypted SMTP traffic.
