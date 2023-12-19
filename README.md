# Inspecting Windows Infection
Example of Windows infection traffic from commodity malware distribution.

## Tutorial 1
### Edit Wireshark columns
[Demonstration](https://www.youtube.com/watch?v=XO5Q77qdEKk)

### Filter unencrypted HTTP traffic
[Demonstration](https://youtu.be/gW4YWRUhSKc?si=pyHEKk-nFOaWa0dk)

### Filter encrypted HTTPS traffic
[Demonstration](https://youtu.be/4EB1seHYwf4)

### Filter encrypted and unencrypted HTTP traffic
[Demonstration](https://youtu.be/ycxWAK7yrtE?si=ITD_OK5-Rfrop9LB)

## Tutorial 2
### Filter traffic that tries to connect to a server that is offline or refusing TCP connection
[Demonstration](https://youtu.be/gF9K-psNVBM)

### Filter FTP traffic - Files
[Demonstration](https://youtu.be/0RGrrRrHTKU)

### Filter SMTP traffic - Emails
[Demonstration](https://youtu.be/0RGrrRrHTKU)

### Save custom filters in Wireshark
[Demonstration](https://youtu.be/2FuYGOTY0PQ)

Note:
* Filter bar is white/black(dark mode) = normal
  ![normal](https://github.com/Damian-crypto/chl059/assets/58256720/6e6047fe-ea1b-4af4-ae68-238392f4a90f)

* Filter bar is red = incorrect filter expression
  ![incorrect](https://github.com/Damian-crypto/chl059/assets/58256720/0a473a10-5522-45e9-a04e-ffad80f350e5)

* Filter bar is yellow = correct but will not work
  ![correct_not_work](https://github.com/Damian-crypto/chl059/assets/58256720/a0a7b120-c20a-45e7-b06b-467e938691cf)

* Filter bar is green = correct filter expression
  ![correct](https://github.com/Damian-crypto/chl059/assets/58256720/f4d5a34b-3c4d-4765-ab21-06ced59c8dd9)

* Use `!(ip.addr eq 192.168.10.1)` instead of this `ip.addr != 192.168.10.1` because it won't work as expected.

* Use `http.request or ssl.handshake.type == 1` to filter web traffic.

* `http.request` - filters HTTP traffic

* `ssl.handshake.type == 1` - filters HTTPS traffic

* UDP Port 1900 is Simple Service Discovery Protocol (SSDP) used to discover plug-and-play devices also not associated with normal web traffic.

* `(http.request or ssl.handshake.type == 1) and !(ssdp)` - used to exclude ssdp traffic.

* `tcp.flags eq 0x0002` - used to reveal connections with servers that are offline or refusing TCP connections.

* `dns` - filter used to find servers that are not web-based and directly hosted on IP address or domain names (e.g., C2)
    * RAT (is not HTTP/HTTPS/SSL/TLS)

> `(http.request or ssl.handshake.type == 1 or tcp.flags eq 0x0002 or dns) and !(udp.port eq 1900)` is now entire filter expression

* `ftp` - filter used to find file transfer traffic of the computer with the internet.

* `smtp` - used to filter mail traffic (spambot malware can send hundreds of emails every minute).

* `smtp contains "From:"` - used to find unencrypted SMTP traffic.
