# Project Report: How I Built a Cloud-Hosted SOC Lab Using Wazuh

> **Note:** This project is potentially a continuous project, and I may keep updating it as I work on new stuff related to it.

## Overview

Building such a project would require some resources that I didn't have, and trying to manage the one I had would impair its success, so I had to move smart by choosing Virtual Machines, but aside from that, I also did something else, which I'll explain below.

## Prerequisites

- A cloud provider (in my case, **Google Cloud Platform (GCP)**)
- Three Linux virtual machines running on the cloud

## Why Cloud?

I chose a cloud-based environment for three key reasons:

1. **Professional Development**: As a security analyst, I need to familiarize myself with how security works with cloud computing. Organizations are rapidly moving to the cloud, and this will undoubtedly lead to new forms of cyberattacks targeting their cloud assets. Already, a significant portion of cyberattacks occurring in cloud-based environments are due to misconfigurations that are left unchecked and unaudited.

2. **Resource Optimization**: Running such a project successfully will require that I set up a minimum of 3 different computers, all with varying degrees of specifications. This is quite impossible for my situation. However, with a cloud-based instance, I can create the necessary number of computers I want as virtual machines and configure them to have just the right amount of specifications needed. That's a dynamic approach to working—something that organizations currently employ.

3. **Consistent Uptime**: I am aware that the project I am working on will teach me in practical ways how a SOC works, and in such realistic situations, there is often 24/7 monitoring. This means I had to ensure that my event logs come in 24/7 so that I can access and analyze logs from anywhere and anytime, as in an actual organization cybersecurity team.

## Infrastructure Setup

To begin, I set up my instances with the following specifications:
- **Storage**: Minimum of 50GB of balanced persistent storage
- **Compute**: 2 VCPUs, 1 core each

> **Note:** I gave them different names to ensure that I can easily recognize them from the shell.

## Networking Configuration

Networking is a really big deal in cybersecurity, and in a project like this, I had to ensure that I got my networking right.

### Port Configuration

I ensured that I opened the following ports:

**Ingress/Inflow (Server End):**
- **TCP Port 22**: SSH access
- **TCP Port 1515**: Wazuh Manager
- **TCP Port 1514**: Wazuh Agent communication
- **TCP Port 55000**: Wazuh API

**Egress/Outflow:**
For outflow, I ensured that my agents could also communicate on those ports, including port 22 for SSH.

### IP Addressing Strategy

- **Server**: I ensured that my server was using a **static/elastic IP address**. This was to ensure that the IP address doesn't change, thereby causing connectivity issues if, for any reason, I stop the instance while troubleshooting. Ideally, the IP address of a server should not change.

- **Agents**: For my agents, their IP address was permitted to change, and I wanted to access my server from anywhere, so I configured my server's firewall to permit IP addresses on `0.0.0.0/0`, and left my agents' IP addresses as dynamic/ephemeral.

> **Security Note:** Google Cloud uses SSH keys by default, preventing password brute force attacks from getting their instances since their public IP address range is well known. Suppose you aren't using Google Cloud Platform or any cloud platform with such protection. In that case, I'll advise you to use a stronger password and protect your public IP from attacks (not always feasible given how public IP addresses are structured, so use a robust password).

## Wazuh Installation

### Server Installation

Moving forward, I installed the assisted installation configuration on my server using the appropriate command.

Then, I installed the Wazuh server, manager, dashboard, and Filebeat in one command.

Once my installation was completed, I copied my password and kept it somewhere safe for later use.

### Dashboard Access

I accessed my dashboard via my server's IP address on HTTPS:

https://server-ip

Using the login credentials I copied after my installation, I accessed my Wazuh dashboard.

## Agent Configuration

### Dashboard Configuration Steps

1. On my dashboard, I navigated to: **Menu > Agents Management > Summary > Deploy New Agent**

2. This opens up a new page where you can choose any of the three supported OS types:
   - Linux
   - Windows  
   - macOS

3. I added my server IP and the name I want to identify my agent with, and chose the group I want my agent to be in.

4. These inputs are used to autogenerate a one-line command. Copy this command and paste it into your terminal shell.

5. After running the command on your agent machine, the agent will be configured and connected to the Wazuh manager.

## Additional Resources

- **Comprehensive Setup Guide**: [Check Here](https://chisom.hashnode.dev/setting-up-a-wazuh-project-a-siem-and-xdr-platform-from-scratch)
- **Official Documentation**: [Wazuh Documentation](https://documentation.wazuh.com/)

---

*This project demonstrates a practical approach to building a cloud-hosted Security Operations Center (SOC) lab environment using Wazuh SIEM, providing hands-on experience with modern cybersecurity monitoring and analysis techniques.*
