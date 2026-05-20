\# Lab: Active Directory Domain Controller Configuration



\## Objective

This lab project implements a functional Windows Server 2022 environment to replicate a standard small-to-medium enterprise (SME) network. The objective is to demonstrate proficiency in deploying Active Directory Domain Services (AD DS), managing domain-wide security policies, and establishing core infrastructure services.



\## Technology Used



\* \*\*OS:\*\* Windows Server 2022, Windows version 11 2h22

\* \*\*Hypervisor:\*\* Oracle VirtualBox

\* \*\*Core Services:\*\* Active Directory Domain Services (AD DS), DNS

\* \*\*Network Configuration:\*\* Static IPv4 Addressing (Isolated Internal network)

\* \*\*Management Tools:\*\* Windows PowerShell, Windows Command Line, Active Directory Users and Computers (ADUC)



\## Project Architecture

The lab environment consists of three virtual machines communicating over an isolated internal network named "kintnet".



\## Server Configuration



\* \*\*Kays Enterprise Server (Domain Controller):\*\* Windows Server 2022, AD DS, DNS. Static IP: 192.168.0.1/24

\* \*\*WS01-Client (Workstation):\*\* Windows 11 Pro. Static IP: 192.168.0.5/24

\* \*\*WS02-Client (Workstation):\*\* Windows 11 Pro. Static IP: 192.168.0.20/24



