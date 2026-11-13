Day 2: Incident Report, Infected Windows Host (2019-03-19 Traffic Analysis Exercise)

Source: malware-traffic-analysis.net exercise, 2019-03-19. Analysis in Wireshark.
Malware families were identified from the published answer key after I finished my own analysis.

Executive Summary
On 2019-03-19 at about 01:49 UTC, a Windows 7 computer used by bobby.tiger
(BOBBY-TIGER-PC, 10.0.90.215) was infected. It downloaded two executable files over
HTTP from servers identified only by IP address. For the next three hours it made
repeated connections to outside servers on unusual ports, using encrypted traffic
with no site name. This pattern is consistent with malware communicating with
command-and-control servers. The answer key identifies the malware as Remcos RAT
and Dridex.

Infected Host
- IP address: 10.0.90.215
- MAC address: 64:32:a8:57:2b:42
- Host name: BOBBY-TIGER-PC
- Windows user account: bobby.tiger
- Operating system: Windows 7 (from the HTTP user agent)

Timeline (UTC, 2019-03-19)
- 01:47:04: GET /test1.exe from 209.141.34.8 (200 OK)
- 01:49:44: connection to 103.1.184.108 on TCP 2404; traffic did not match the IEC 104 protocol that uses that port
- 01:49:46: GET /f4.exe from 217.23.14.81 (200 OK)
- 02:03:24: connections begin to 31.22.4.176 on TCP 3389 (TLS 1.0, no server name)
- 02:08:16: connections begin to 203.45.1.75 on TCP 443 (TLS 1.0, no server name)
- Repeated through about 04:55, ending with a burst to 115.112.43.81, 46.105.131.77 and 189.189.64.242

How I found it
- The alerts file repeatedly named 10.0.90.215.
- Host name and username came from NetBIOS and Kerberos traffic.
- HTTP request filters showed two .exe downloads. The alerts flagged both as likely
  hostile (executable from a bare IP, terse file name, binary under 1 MB).
- The user agent was an Internet Explorer 7 compatibility string, consistent with a
  script making the request rather than a person browsing.
- Outbound connections were summarized with Statistics > Conversations, then checked
  one by one. None of the TLS Client Hellos contained a server_name.

Indicators of Compromise
- http://209.141.34.8/test1.exe
- http://217.23.14.81/f4.exe
- 103.1.184.108, TCP 2404
- 31.22.4.176, TCP 3389
- 203.45.1.75, TCP 443
- 115.112.43.81, TCP 443
- 46.105.131.77, TCP 443
- 109.230.231.176 and 189.189.64.242, TCP 443 (connection attempts, no response)
File hashes: not collected.

MITRE ATT&CK mapping
- T1105 Ingress Tool Transfer: two executables downloaded from outside servers
- T1571 Non-Standard Port: encrypted traffic on 3389 and non-IEC-104 traffic on 2404
- T1573 Encrypted Channel: TLS connections with no server name

Recommendations
- Isolate BOBBY-TIGER-PC from the network and reimage it.
- Block the IP addresses above at the firewall.
- Reset bobby.tiger's password and check for other hosts connecting to these addresses.
- Check how the malware first ran. The delivery method was not visible in this capture.

What I could not determine
- How the malware first reached the computer.
- The contents of the encrypted traffic.