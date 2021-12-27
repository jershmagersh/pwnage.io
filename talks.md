---
title: "Talks"
permalink: "/talks/"
layout: page
---

## VB2021 Localhost - How CARBON SPIDER embraced ransomware

In April 2020, CARBON SPIDER (a.k.a. FIN7 and Carbanak) abruptly shifted away from targeting POS systems to broad, opportunistic ransomware operations. These campaigns initially delivered REvil, but in August 2020 the adversary introduced their own ransomware, Darkside. In November 2020, the Darkside Ransomware-as-a-Service (RaaS) program was opened, but CARBON SPIDER continued to conduct ransomware campaigns directly. These campaigns included encryption of VMware ESXi servers using a variant of Darkside specifically designed for ESXi. In May 2021, a Darkside ransomware affiliate was involved in an attack against Colonial Pipeline, which led to the shutdown of the Darkside RaaS. In July 2021, CARBON SPIDER re-emerged with the creation of the BlackMatter RaaS.

This case study will provide an overview of the transition of CARBON SPIDER (a.k.a. FIN7 and Carbanak) from the targeting of POS systems for credit card data to indiscriminate ransomware operations. This will include details of largely unknown downloaders and backdoors, a relationship with a Zloader operator for initial access not previously discussed in public reporting, and TTPs observed throughout these operations for credential access, lateral movement and ransomware deployment. Notable post-initial access TTPs include a particularly wide range of credential access techniques to achieve privileged context, enabling lateral movement and eventually Darkside ransomware deployment. While CARBON SPIDER makes heavy use of Cobalt Strike for lateral movement, the adversary has also used legitimate tools such as Plink, GoToAssist and TightVNC. In addition to deploying ransomware, CARBON SPIDER exfiltrates files from victims, primarily using the MEGASync client for hosting provider MEGA.

We will also provide details of the group’s deployments of REvil and how we attributed the creation of the Darkside ransomware to CARBON SPIDER. The presentation will conclude with a discussion of CARBON SPIDER’s operations in the wake of the Colonial Pipeline attack and how the adversary regrouped to ultimately continue ransomware campaigns using the new BlackMatter ransomware.

[VB2021 Localhost - How CARBON SPIDER embraced ransomware](https://www.youtube.com/watch?v=PAG3M7mWT2c&t=8192s)

## BSidesLV 2019 - From EK to DEK: An Analysis of Modern Document Exploit Kits

Exploit Kits haven’t disappeared, they’ve simply moved to Microsoft Office. Traditional Exploit Kits (EKs) have the ability to fingerprint and compromise web browser environments, but with the advent of sandboxing and advanced security measures, there has been a shift toward using the Microsoft Office environment as a primary attack surface. Document Exploit Kits (DEKs) leverage DCOM, ActiveX controls, and logic bugs to compromise machines by packing multiple exploits into a single file.

This talk will provide an in-depth overview of the vulnerabilities and exploitation techniques used by the ThreadKit and VenomKit documents to spread well known malware families, and how they are being used in targeted attacks.

{% include youtube.html id="elondmwn7PM" width="100%" %}

## BSides Edmonton 2018 - Mo' Monero Mo' Problems: An Analysis of Cryptomining Malware

As cryptocurrency popularity continues to grow and alt-coins continue to proliferate, malware authors are increasingly using Cryptomining as a means of generating income. This talk will provide an in-depth overview of methods being used to distribute and infect systems with Cryptomining malware. Strategies for collecting intelligence and combatting malicious mining activities will also be discussed.

{% include youtube.html id="LV4kBhPVUqc" width="100%" %}

## BSides Calgary 2017 - Attack of the Document Clones

Microsoft Office documents sent by email remain an excellent infection vector. Despite security awareness training and Microsoft's best efforts, users still open malicious documents and enable the macros within. 
Ross Gibb and Josh Reynolds' presentation will examine in-the-wild malicious documents sent to a variety of targets over time, and will introduce techniques to programmatically group and cluster malicious documents. In addition to improving detection, these techniques can also be used to track the actors creating and disseminating document clones.

{% include youtube.html id="n6MoYW_rCjg" width="100%" %}

