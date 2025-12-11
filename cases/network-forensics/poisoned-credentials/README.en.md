# Poisoned Credentials: Anatomy of a Network Forensics Incident

Hello cybersecurity enthusiasts! In this article, we will solve step by step the "PoisonedCredentials" case featured on the CyberDefenders platform, which tests network forensics skills. Starting with a single piece of evidence, a `.pcap` file, we will uncover how an attacker exploited a simple typo in a local network to gain access to a critical server. Our tool will be **Wireshark**, the indispensable companion of network analysts.

Ready? Let's prepare our filters and begin tracking this cyber attack!

## Crime Scene: Initial Clues and Analysis Environment

Our only evidence is a network traffic recording: the `PoisonedCredentials.pcap` file. Our goal is to detect anomalous activities in this traffic and reconstruct all stages of the attack.

---

## Question 1: The Mistyped Query That Started the Incident

> **Question:** In the context of the incident described in the scenario, the attacker initiated their actions by taking advantage of benign network traffic from legitimate machines. Can you identify the specific mistyped query made by the machine with the IP address 192.168.232.162?

### Analytical Approach

The term "mistyped query" typically begins when a user misspells a server or website name. As analysts, our first hypothesis is that this could be a DNS query. We start by examining DNS traffic originating from the IP address `192.168.232.162` using Wireshark filters.

### Examining the Evidence

First, we search for standard DNS traffic:
`ip.src==192.168.232.162 and dns`

![DNS Filter - Empty Result](assets/q1_dns_filter.png)

This filter returning empty results indicates our hypothesis was incorrect. The attack did not occur through standard internet DNS. In this case, we turn to Windows' local network name resolution mechanisms: **NBNS (NetBIOS Name Service)**.

When we update our filter to `nbns`, the evidence we're looking for emerges:
`ip.src==192.168.232.162 and nbns`

![NBNS Query - Typo](assets/q1_nbns_query.png)

In the screenshot, packet 47's "Info" column shows `Name query NB FILESHAARE<20>`. It's clear that the legitimate file server name "fileshare" was misspelled by adding an extra "a" to become **"fileshaare"**. This is the starting point of the attack.

**✅ Answer:** `fileshaare`

---

## Question 2: The Rogue Machine's IP Address

> **Question:** We are investigating a network security incident. To conduct a thorough investigation, We need to determine the IP address of the rogue machine. What is the IP address of the machine acting as the rogue entity?

### Analytical Approach

In the previous step, we found that the victim accidentally asked for "fileshaare." Now we need to find the attacker who falsely responds "I am fileshaare!" To do this, we must filter all NBNS conversations containing "FILESHAARE."

### Examining the Evidence

Using the following filter, we can see both the question and answer simultaneously:
`nbns.name contains "FILESHAARE"`

![Fake Response - Rogue Machine](assets/q2_rogue_response.png)

This screenshot shows the heart of the incident:
- **Packet 47:** A **query** originating from `192.168.232.162` (victim).
- **Packet 51:** A **response** originating from `192.168.232.215`.

The source IP address of the packet responding to the query is the rogue machine that pretends to be a legitimate server.

**✅ Answer:** `192.168.232.215`

---

## Question 3: The Second Affected Machine's IP Address

> **Question:** As part of our investigation, identifying all affected machines is essential. What is the IP address of the second machine that received poisoned responses from the rogue machine?

### Analytical Approach

Now that we know the attacker's identity (`192.168.232.215`), we need to determine the scope of the incident by finding who else received fake responses from this attacker. We'll configure our filter to show all local network name resolution responses originating from the attacker.

### Examining the Evidence

Our filter: `ip.src == 192.168.232.215 and (nbns or llmnr or mdns)`

![Second Victim - Affected Machine](assets/q3_second_victim.png)

In this filter's output, we examine the "Destination" column. We can see that the attacker sent fake responses not only to the first victim `192.168.232.162` but also to **a different IP address: `192.168.232.176`**. This is the second machine affected by the attack.

**✅ Answer:** `192.168.232.176`

---

## Question 4: The Compromised User Account

> **Question:** We suspect that user accounts may have been compromised. To assess this, we must determine the username associated with the compromised account. What is the username of the account that the attacker compromised?

### Analytical Approach

Victim machines, believing the attacker to be a legitimate file server, will attempt to send their credentials to connect to it. This authentication traffic occurs over the **SMB2** protocol in Windows networks. To find the username, we must examine this SMB2 conversation.

### Examining the Evidence

To isolate all SMB2 traffic between the attacker and victims, we use this filter:
`(ip.addr == 192.168.232.215) and smb2`

![Username - Compromised Account](assets/q4_username.png)

In the filtered traffic, **Packet 242**'s "Info" column contains `Session Setup Request, NTLMSSP_AUTH, User: cybercactus.local\janesmith`. This shows the moment when the user `janesmith` attempted to establish a session using credentials from the `cybercactus.local` domain. With this packet, the attacker has captured this user's credentials.

**✅ Answer:** `janesmith`

---

## Question 5: The Hostname of the Machine Accessed by the Attacker

> **Question:** As part of our investigation, we aim to understand the extent of the attacker's activities. What is the hostname of the machine that the attacker accessed via SMB?

### Analytical Approach

After stealing `janesmith`'s credentials, the attacker will use this information for lateral movement within the network. Our goal is to find which target machine the attacker connected to using these stolen credentials. The target machine identifies itself (hostname) in "Negotiate Protocol Response" or "Session Setup Response" packets at the beginning of the SMB conversation.

### Examining the Evidence

We continue examining the conversation using the filter from the previous question (`... and smb2`). We focus on **Packet 241**, which is the "Session Setup Response" packet sent by the target machine (`192.168.232.176`) to the attacker.

![Hostname - Target Machine](assets/q5_hostname.png)

When we examine the packet details, in the **Target Info** section within the `NTLMSSP_CHALLENGE` message, we can see the information the target machine provides about itself:
- `Attribute: NetBIOS computer name: ACCOUNTINGPC`
- `Attribute: NetBIOS domain name: CYBERCACTUS`

This definitively proves that the machine the attacker accessed using stolen credentials is named **ACCOUNTINGPC**.

**✅ Answer:** `ACCOUNTINGPC`

---

## Conclusion: The Complete Attack Chain

Starting from a single `.pcap` file, we successfully reconstructed all steps of a classic "Name Poisoning" attack:

1. **Initiation:** A user accidentally types `fileshaare` instead of `fileshare`.
2. **Poisoning:** The attacker (`192.168.232.215`) responds to this incorrect query, redirecting victims (`.162` and `.176`) to itself.
3. **Credential Theft:** Victim machines send `janesmith`'s credentials (password hash) to the attacker.
4. **Lateral Movement:** The attacker uses these stolen credentials to gain access to a critical target in the network: the `ACCOUNTINGPC` server.

This case demonstrates how powerful network traffic analysis is in illuminating a cyber incident and how even the most complex-looking traffic records can tell a story with the right filters.

---

## Technical Details and Lessons Learned

### Wireshark Filters Used
- `ip.src==192.168.232.162 and dns` - Search DNS traffic
- `ip.src==192.168.232.162 and nbns` - Search NBNS traffic
- `nbns.name contains "FILESHAARE"` - Analyze incorrect query
- `ip.src == 192.168.232.215 and (nbns or llmnr or mdns)` - Attacker traffic
- `(ip.addr == 192.168.232.215) and smb2` - SMB2 authentication traffic

### Protocol Details
- **NBNS (NetBIOS Name Service):** Windows local network name resolution
- **LLMNR (Link-Local Multicast Name Resolution):** Windows Vista+ name resolution
- **MDNS (Multicast DNS):** Local network DNS alternative
- **SMB2:** Windows file sharing protocol
- **NTLMSSP:** Windows authentication protocol

### Security Measures
1. **DNS Security:** Use DNSSEC
2. **Network Segmentation:** Isolate critical systems
3. **Monitoring and Alerting:** Detect anomalous NBNS/LLMNR traffic
4. **User Training:** Correct server name typing
5. **Endpoint Security:** Advanced threat protection

---

*This article is prepared for cybersecurity education purposes. Each analysis contains unique details and challenges. Similar techniques are used in real cases to detect and prevent cyber attacks.*
