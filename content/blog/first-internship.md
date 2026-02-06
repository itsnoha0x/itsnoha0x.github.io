---
title: "My First Internship In Offensive Security, Recap"
slug: "my-first-internship-in-offensive-security"
date: 2026-02-06
draft: false
summary: "I completed my first internship in offensive security, working on an Active Directory lab and project. I gracefully took my time to understand AD principles, secure them, and document the journey."
---

The most exciting part of cybersecurity is practice. It’s the moment you finally hack a real system, turn theory into action, and collaborate with active peers in the field.

A few weeks ago, I completed my project report, closed the deal, and ticked the box in my journal. My first internship in offensive security was officially complete, and it's time to document it. 

Within the [CyberV](https://cyberv.fr/) association, I joined the team as an intern, and my project? Testing and Securing an Active Directory... building the domain, attacking it, and then proposing the fixes to harden it.

## Project Context: Why an Active Directory?

Active Directory (AD) is the backbone of most companies’ IT infrastructure. It’s the server system that centralizes resources, users, and different departments into a single place. Making it easier to manage, but also dangerous if compromised.

 Its critical position makes it a primary vector of attack and a considerable access point to the bigger system. That’s why it remains crucial to understand the fundamentals of [Active Directory and its security risks](https://www.tanium.com/blog/what-is-active-directory-security/).

In fact, learning about the tactics and behaviors attackers use equips defenders to anticipate threats and spot anomalous activity. On the other hand, it’s also valuable for red teams to understand defensive techniques. So they simulate realistic adversaries and reveal gaps defenders may have missed. 

My internship project was built around this reality: to learn more about AD security and why audits and penetration tests are important for staying secure. 

## The Offensive Security Lab

To keep things simple, I built a lab with just three machines:

- **Windows Server** as the Domain Controller (Active Directory).
- **Windows client** joined to the domain and used as the victim.
- **Kali Linux** as the attacker machine for scanning and exploitation.

That’s all. The lab is self‑contained: the Windows Server acts as the network’s “brain” (AD + DHCP), hands out an IP address to the client, and serves as the central management point for the simulated organization.

## **Tools & Techniques I Worked With**

The main methodology I used was that of a penetration tester. So we’re talking recon, enumeration, exploitation, post-exploitation, and auditing.

Here’s a breakdown of the main tools I used:

- **nslookup**
    
    A tool for simple DNS reconnaissance to confirm domain names and IPs. It’s a useful first step to map what’s in the lab around you.
    
- **nmap**
    
    The classic open-source tool for port and service scanning. It helps identify open services, prioritise possible targets, and also check for specific vulnerabilities. For example, the 445 TCP port (SMB), found in my lab, is an interesting attack vector and often a vulnerable service.
    
- **enum4linux**
    
    A great enumeration tool to automate info gathering and return useful pieces. From what I noticed, its output can be limited by Windows Defender, but provides valuable info when protections are disabled.
    
- **Metasploit**
    
    A modular exploitation framework used for controlled exploitation attempts. It validates whether identified vulnerabilities are actually exploitable. And yes, SMB protocol was.

<figure style="margin: 40px 0; text-align: center;">
  <img src="/images/photos/metasploit.png" style="display: block; margin: 0 auto;" />

  <figcaption style="margin-top: 10px; font-size: 0.9em; color: #666;">
    Confirmed remote code execution via SMBv1 EternalBlue exploit.
  </figcaption>
</figure>

    
- **Kiwi (Mimikatz module)**
    
    Once we’re in with Meterpreter, the Kiwi module was used to illustrate credential harvesting from memory, especially those of a connected admin. It proved that an exposed account can easily escalate privileges.
    
- **CrackMapExec (CME)**
    
    An AD enumeration tool that’s also great for credential checking, spotting exposed SMB shares, and basic misconfigurations across the domain.
    
- **Pass‑the‑Hash (PtH)**
    
    An authentication abuse technique that uses stolen NTLM hashes to move across the domain. A real critical AD risk that leads to privilege escalation when admins connect to multiple devices in a network.
    
- **PingCastle**
    
    An automated AD audit tool producing a concise health report. This step was used after total compromise of the server, to end the project with a list of recommendations to harden the system. Deactivating the SMBv1 protocol, adding sensitive accounts to the Protected Users group, and activating the LSA protections were all in the list.
    

## Real-World Parallels: how they impacted the world

Working on a little lab can be your window to understanding cybersecurity trends. It makes you work with things you only read in headlines. To make it clear, here is a list of what I worked with and how it caused real breaches before:

- **EternalBlue (ms17_010)**
    
    An SMB vulnerability that allows full access to a target machine. Used in [WannaCry ransomware](https://www.fortinet.com/fr/resources/cyberglossary/wannacry-ransomeware-attack) (2017) to infect 200,000+ computers in 150 countries, crippling the NHS, FedEx, and Renault factories. Also exploited in NotPetya, causing around $10 billion in damage.
    
- **Pass-the-Hash Attack**
    
    Stolen NTLM hashes are not an easy game; they let attackers authenticate across a domain without passwords. In 2013, attackers compromised a Target vendor's credentials, used Pass-the-Hash techniques to move laterally through the network, and stole 40 million credit card numbers. And in 2014, Sony Pictures had a similar experience with lateral movement and was left with a destroyed system.
    
- **The Audit Gap**
    
    The way I used PingCastle to check for my AD health isn’t something all companies do. Sadly, most organizations only audit AD when required for compliance, and they don't even realize their AD configurations have vulnerabilities until they're breached.

<figure style="margin: 40px 0; text-align: center;">
  <img src="/images/photos/PingCastle.png" style="display: block; margin: 0 auto;" />

  <figcaption style="margin-top: 10px; font-size: 0.9em; color: #666;">
    PingCastle assessment of Active Directory security posture.
  </figcaption>
</figure>
    

See how simple it can be to cause global damage? Even today, these issues persist. Publicly known vulnerabilities continue to drive breaches and problems, showing why we still need to learn about them. And say it louder for those in the back!

## **Challenges & Problem-Solving Moments**

During my internship, I discovered the daily struggles of a pentester. And how technology needs patience, knowledge, and critical thinking.

The main issue you’d face when making a lab is the weird virtual machine problems, the limitations of a simple environment, and the technical issues you’ll spend hours fixing. 

For example, privilege escalation attempts with tools like [BloodHound](https://www.sans.org/blog/bloodhound-sniffing-out-path-through-windows-domains), Kerberoasting, and credential dumping sometimes failed because there were no active admin connections. So I had to simulate scenarios and plan accordingly.

Otherwise, the process was full of learning, problem-solving, and growth. 

## **Key Takeaways as a first-time intern**

I’m grateful to say that my first internship was both a technical and professional success; it made me improve my skills and learn how to lead future projects. Here are some takeaways:

- **Technical:** I’ve learned the AD pentesting workflow, privilege escalation mindset, implemented a variety of tools for the first time, and learned technical documentation of projects.
- **Professional:** Communication with my mentor and teammate, reporting vulnerabilities clearly, and asking the right questions when needed. Most importantly, the skill to learn, search, and experiment to understand.
- **Mindset:** I realized that security isn’t just a list of tasks; it’s a mindset and almost a lifestyle. You become the expert, the hacker, and use your expertise responsibly to help secure systems.

## In Little Words

My first internship was not only a one-month project! It helped me see my potential in offensive security,  gave me the boost I needed to start new projects, and connected me with some good friends along the way.

Sharing the story with you, dear readers, is my way of passing along knowledge and encouraging you to take a new step toward building that lit [cybersecurity career](https://nohawrites.com/blog/recruiters-advice-to-get-you-hired-in-cybersecurity/). It only takes some courage to start, and lots of passion to keep going.

Stay tuned for more related [blog posts](https://nohawrites.com/blog/)!