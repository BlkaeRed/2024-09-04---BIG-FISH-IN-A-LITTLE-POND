# 2024-09-04---BIG-FISH-IN-A-LITTLE-POND

SCENARIO

LAN segment details:

LAN segment range:  172.17.0[.]0/24 (172.17.0[.]0 through 172.17.0[.]255)

Domain:  bepositive[.]com

Active Directory (AD) domain controller:  172.17.0[.]17 - WIN-CTL9XBQ9Y19

AD environment name:  BEPOSITIVE

LAN segment gateway:  172.17.0[.]1

LAN segment broadcast address:  172.17.0[.]255


TASK
Write an incident report based on malicious network activity from the pcap and from the alerts.

The incident report should contains 3 sections:

Executive Summary: State in simple, direct terms what happened (when, who, what).

Victim Details: Details of the victim (hostname, IP address, MAC address, Windows user account name).

Indicators of Compromise (IOCs): IP addresses, domains and URLs associated with the activity.  SHA256 hashes if any malware binaries can be extracted from the pcap.


Initially, I analyzed the alert section of this exercise and noticed two important aspects. First, there were alerts related to one IP address within the LAN segment and another outside the LAN. Secondly, one of the alerts mentioned a specific type of malware called “Koi Stealer.” Before diving deeper into the analysis, I decided to research this malware and stealer-type malware in general.

An important characteristic of "Koi Stealer" is that it downloads an index.php file from a specified URL, which is later used to decrypt a large byte buffer contained in a PowerShell loader. Additionally, I found out that "Koi Stealer" collects data by encrypting it and then sending it via POST requests to a server. This meant I needed to look for potential index.php downloads and numerous POST methods sent from a suspicious domain.

Next, I examined the .pcap file. As expected, I observed that the index.php file was downloaded from the server at IP address 79.124.78.197 (discovered under "File" -> "Export Objects" -> "HTTP" and within one of the TCP streams). This convinced me that 79.124.78.197 was an IOC (Indicator of Compromise) related to the "Koi Stealer" infection.

I then sought to determine how the malware initially infiltrated the host. Unfortunately, the packet captures provided in the .pcap file made it impossible to pinpoint the exact origin of the malware. However, I found one candidate: the IP address 46.254.34.201 and the domain www.bellantonicioccolato.it could potentially be related to the infection. Nevertheless, because the first instance of traffic to/from 79.124.78.197 occurred before any traffic to/from 46.254.34.201, and given that the domain appears legitimate, there's no definitive way to confirm if that domain was responsible for infecting the DESKTOP-RNVO9AT host.

TASKS

Executive Summary:

As early as 2024-09-04, at 17:35 UTC (exact time and date is unknown), a Windows host DESKTOP-RNVO9AT utilized by user Andrew Fletcher exhibited indications of infection by Koi Stealer malware.
_________________________________________________________________________________________________________________________________________________________________________________________________

Victim Details:

Hostname: DESKTOP-RNVO9AT

Ip address: 172.17.0.99

Mac address: 18:3d:a2:b6:8d:c4

Windows user account name: afletcher
_________________________________________________________________________________________________________________________________________________________________________________________________

Indicators of Compromise (IOCs):

Suspicous Ip address/URL: 79.124.78.197

Ip address and domain potentially related to infection: 46.254.34.201/ www.bellantonicioccolato.it
