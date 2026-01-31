---
title: "Harvard CS50’s Introduction to Cybersecurity Made My Classes Easy"
slug: "harvard-cs50-cybersecurity-course-review"
date: 2026-01-30
draft: false
summary: "Sometimes, one online course can be all it takes to break into a field. And in my story, it was the Harvard CS50’s introduction to cybersecurity! Here is a detailed course review"
---

Sometimes, one online course can be all it takes to break into a field. And in my story, it was the Harvard CS50’s introduction to cybersecurity!

After getting accepted into the cybersecurity major at my university, with little to no experience, I was eager to find something fun and easy to study over the summer. That’s when I came across CS50! With all those positive reviews over the internet, I didn’t hesitate to join immediately!

In this post, I’ll take you on a structured course review to help you decide, or convince you, to join the massive world of CS50’s. As well as a backstory on how it made my classes a lot easier.

## About the Course:

- Made by Harvard University
- Offered by the [edX platform](https://www.edx.org/) or directly on the [CS50 site](https://cs50.harvard.edu/x/2024/)
- Free to audit, with an optional paid certificate
- Completely self-paced (it took me about two months)
- Content of six weeks, diverse topics on the basics of cybersecurity
- Lectures by [David Malan](https://www.linkedin.com/in/malan/), the famous computer science professor at Harvard
- Format of a video lecture, notes, and an assignment per week

## Who is it for?

As I briefly mentioned, I took the class with very basic knowledge of cybersecurity and tech. So the course is beginner-oriented, no prior experience required. 

The material is not so technical; it is more introductory and fundamental building. It helps you grasp the main roots and build a roadmap for your next learning steps.

## A dive into the content of the course

Without further ado, let’s unpack the course, week by week:

### Week 0: Securing Accounts

Security of digital accounts is mandatory knowledge for everyone who uses the internet. It opens your eyes to how your daily apps work and the risks you may not realize you’re exposed to.

The class started with concepts like authentication (proving who the user is based on credentials), Single Sign-On (SSO), password managers, and passkeys.

We were also introduced to common attacks that target accounts:

- **Dictionary attacks:** When an attacker tries different words and combinations to guess your password, based on common possibilities.
- **Brute-Force attacks:** Using software to systematically try all possible password combinations.
- **SIM Swapping**: Trickery played on the company (phone provider) to swap the sim card, obtain the number, and receive the one-time password.
- **Keylogging:** Malicious software installed on a computer recording everything being typed and sends it to the adversary.
- **Credential Stuffing**: Finding a list of leaked username-password pairs and trying (stuffing) them in different websites, as in hoping users use the same info all around.
- **Social Engineering**: Manipulating people into revealing sensitive information by playing on their emotions.
- **Phishing:** A form of social engineering, usually via email, by sending a familiar, trustworthy (yet fake) link.
- **Man-In-The-Middle (MITM) Attacks:** When a third party joins in between the communication of two systems and intercepts the exchanged data.

<div style="margin: 40px 0;">
  <img src="/images/illustrations/authentication.png" width="320" style="display: block; margin: 0 auto;" />
</div>

### Week 1: Securing Data

Once we got familiar with the basics, it was time to move to an advanced area, DATA.

Data is at the core of everything we do online, from things you save in your machines to communications with servers, computers, and friends out there. And securing it stands heavily on encryption.

Which explains how this week was mainly about cryptography concepts:

- **Hashing**: Using one-way mathematical functions to transform passwords into hashes. A value that can’t be found without guessing or comparing hashes one at a time.
- **Salting:** Adding a random value called salt to the word before hashing it, which makes the hash unique each time and harder to find.
- **Symmetric key encryption:** Algorithms in which messages are encrypted and decrypted using a single secret key known only to the sender and receiver.
- **Asymmetric key encryption:** Algorithms that rely on a public key (used to encrypt the text but not decrypt it), and a private secret key (that actually decrypts the message).
- **Signature:** A method used to verify integrity, which consists of hashing a message and then encrypting the hash. Decrypting it by the public key and comparing the found hash to the original (assuming we know the message) is how a signature is verified.
- **End-to-End encryption:** A guarantee that even intermediaries in the communication path cannot see the real text. The text is fully ciphered from the sender to the receiver, no matter what route it takes.

### Week 2: Securing Systems

The web is known for being a common attack surface today. Many systems find their way to the world trough the web, but also to potential vulnerabilities and risks. It was crucial that we take a step to grasp the basics of web and network security, and that's what Week 2 came for.  

Lots of concepts were discussed, including:

- **HTTP / HTTPS:** Hyper Text Transfer Protocol, a language that computers use to find the needed URL. The S stands for Secure, and it guarantees the full encryption of your traffic by SSL or TLS.
- **Packet Sniffing:** Sniffing is essentially about closely inspecting and monitoring the behavior of network traffic. If not encrypted, the information can be fully visible and even changed through attacks like MITM.
- **Session Hijacking**: Cookies store the value of the currently active session. An unencrypted connection puts the cookie at risk of being stolen, the session being used, and the hacker logging in to the account.
- **SSL stripping:** Tricking the user into thinking they have an encrypted connection, only to send them to a third-party page. It looks pretty secure, but it’s not where you wish to be.
- **DDOS (Distributed Denial Of Service):** An attack in which a silent virus is sent to so many computers around the world until the attacker decides and uses them all (botnets) to flood a target with traffic and deny service.

The lecture contained multiple other definitions, such as VPN, SSH, ports, firewalls, proxies, etc…

### Week 3: Securing Software

Another week that went even deeper with web security, especially the attacks and those of code injection such as:

- **XSS (Cross-Site Scripting):** Injecting JavaScript pieces into web pages to run in the user's browser, whether it’s reflected or stored. Character escape is an implemented method that changes potentially dangerous characters to prevent code execution.
- **SQL Injection:** Injecting commands that question the database uncontrollably through input places. Prepared statements are the equivalent of character escapes for SQL commands.

We also dived into CVE, the standardized system that identifies existing vulnerabilities and makes tracking them easy, as well as CVSS, the system used to put them in order of priority, and other related explanations.

### Week 4: Preserving Privacy

The basics of cybersecurity couldn’t end without a moment of sensitization about privacy. Week 4 was all about revealing the secrets of browsers and sites to know how your data is treated. As sometimes, it may be exposing your privacy to a certain level.

We discussed logs in servers and their role of diagnosis and analytics. And since some consider these information as sensitive, client-side software exists to remove them after you, protecting the unwanted tracking.

The most interesting part, in my opinion, was the topic of fingerprinting: A group of parameters that a browser can track, such as your IP address, OS version, your screen resolution, fonts, time zone, extensions, and even your surfing behavior. This simply creates a unique identity for you and your device and you may become known and trackable.

The Professor also took a good time to explain cookies, not only but he also went in detail about all types of cookies and their usage:

- **Session Cookies:** Temporary, they help you stay logged then get deleted when the browser is closed.
- **Tracking Cookies:** Set by sites different than the one you're on. How Google Ads may have cookies while you visit pages, to track and use the info for analytics and ad improvement (like third-party cookies).
- **Super Cookies:** More persistent, more invasive, and not very wanted. It's a tracking mechanism that is stored somewhere unexpected, which makes it harder to detect and remove.

Adding to that, more other concepts discussed…

<div style="margin: 40px 0;">
  <img src="/images/illustrations/cybersecurity.png" width="320" style="display: block; margin: 0 auto;" />
</div>

## What I loved most about the course:

There are many things that made me excited about the course. No literally, happily studying it every time!

### The Professor’s teaching skills

Apparently Prof Malan is known for his teaching skills, but since day one, I was drawn to his explanation. He makes the lecture fun, easy to follow, beginner-friendly but still very loaded with info.

The way he would just move from section to section smoothly and draw your attention with analogies, live examples on his screen and even the question breaks he does, it all piles up to make it fun.

### The course’s structure

The weeks are structured on a scale, it becomes more detailed and more into the technical sides, from simple accounts gestion in your own laptop to how web attacks work. 

Simply, as you get more familiar with the concepts, you get to learn more about the details, and its easier because you’ve built the foundations.

## How did it affect my school year?

In my first semester in cybersecurity, I took an introductory and cryptography / PKI course. It was full of the basics, encryption algorithms, hashing, signatures, securing the web… etc Most of which were all included in the CS50’s introduction to cybersecurity. 

So instead of scrambling to learn everything from scratch, I was just deepening what I’d already seen. It felt light, easier to grasp, and refreshing to hear familiar things. And it applied to other courses, such as ethical hacking, web applications, and so on.

## Conclusion

CS50s are famous for a reason, it is the cocktail of every introductory information you need put together in a fun environment and structure.

The weekly assignments were pretty manageable, mostly direct questions about the lectures or something specific to research and understand (like a known vulnerability). The final project required making a YouTube video about a cybersecurity topic. While creative, it wasn't necessarily my favorite idea and I wish I had something more practical, like a real-life project.

In little words, I highly recommend you take the course, if:

- You’re new to cybersecurity and want to discover the basics
- You prefer a structured course and lectures style (engaging non-boring ones)
- You want to have a roadmap or a general idea of what to learn in the field

If you relate, get in and enroll today! And come back for [more articles on cybersecurity](https://nohawrites.com/blog/).