# Windows Firewall & Network Monitoring Lab

## Overview

This project demonstrates basic firewall configuration and network connectivity testing in an isolated virtual lab environment.

I used Kali Linux to test connectivity to a Windows Server virtual machine, performed Nmap scans to identify open network ports and services, and created a temporary Windows Defender Firewall rule to block ICMP ping traffic.

After testing the rule, I removed it and verified that ping connectivity was restored.

## Objectives

- Establish baseline network connectivity between two virtual machines.
- Use Nmap to identify open TCP ports and associated services.
- Create and enable an inbound Windows Firewall rule.
- Test the effect of blocking ICMP ping traffic.
- Remove the temporary rule and verify restored connectivity.
- Document the procedure and results as a security lab project.

## Lab Environment

| Component | Details |
|---|---|
| Attacking/testing VM | Kali Linux |
| Target VM | Windows Server |
| Network scanning tool | Nmap |
| Firewall | Windows Defender Firewall with Advanced Security |
| Target IP address | `192.168.233.130` |
| Virtual environment | VMware |

This was a controlled home lab using virtual machines that I manage.

## Lab Network

Kali Linux was used to test network connectivity to the Windows Server VM.

- **Source:** Kali Linux
- **Target:** Windows Server (`192.168.233.130`)
- **Test traffic:** ICMP echo requests (ping)
- **Security control:** Windows Defender Firewall inbound rule

## Step 1: Establish Baseline Connectivity

Before changing the firewall configuration, I tested connectivity from Kali Linux to the Windows Server using ping.

Command:

```bash
ping 192.168.233.130
```

The baseline test established whether the target responded to ICMP echo requests before the firewall change.

<img width="595" height="389" alt="01-baseline-ping" src="https://github.com/user-attachments/assets/c532512d-350f-4002-b3bc-e0d1487790cf" />


## Step 2: Perform an Nmap Scan

I used Nmap to identify TCP ports exposed by the target from the Kali VM.

Command:

```bash
nmap 192.168.233.130
```

The scan identified open TCP ports and ports that did not respond to the scan. Nmap results describe what was observable from the testing VM at the time of the scan.

<img width="587" height="373" alt="02-nmap-baseline" src="https://github.com/user-attachments/assets/a2cfb402-6ef8-4200-9043-86f808765816" />


## Step 3: Identify Services with Nmap

I performed service detection to gather more information about the services associated with the open ports.

Command:

```bash
nmap -sV 192.168.233.130
```

The scan reported services associated with DNS, Kerberos, Microsoft RPC, NetBIOS, LDAP, SMB, Remote Desktop Protocol (RDP), and Windows Remote Management (WinRM).

Open ports should not automatically be considered vulnerabilities. Their exposure must be evaluated against the server's intended role, required access, and security configuration.

<img width="634" height="493" alt="03-nmap-service-detection" src="https://github.com/user-attachments/assets/25375dc5-8ff6-4953-aaf1-5326c533ed43" />


## Step 4: Configure a Windows Firewall Rule

I opened **Windows Defender Firewall with Advanced Security** on the Windows Server VM and created a new inbound rule.

Rule configuration:

- **Rule name:** `LAB - Block ICMP Ping`
- **Rule type:** Custom
- **Program:** All programs
- **Protocol:** ICMPv4
- **Remote IP addresses:** Any IP address
- **Action:** Block the connection
- **Profiles:** Domain, Private, and Public
- **Status:** Enabled

The rule was created specifically for this lab to demonstrate how a firewall can block inbound ping requests. It was not intended as a permanent production configuration.

<img width="443" height="333" alt="04-firewall-block-rule" src="https://github.com/user-attachments/assets/dadc6222-f4a7-4178-9195-497ce67ac1ca" />


## Step 5: Test the Firewall Rule

With the inbound rule enabled, I repeated the ping test from Kali Linux.

Command:

```bash
ping 192.168.233.130
```

The test showed 100% packet loss while the rule was active. This result was consistent with the firewall blocking ping traffic.

<img width="573" height="130" alt="05-ping-blocked" src="https://github.com/user-attachments/assets/cbac3328-3608-4de4-b8a0-57a9450eed34" />


## Step 6: Remove the Rule and Verify Connectivity

After documenting the blocked traffic, I deleted the temporary `LAB - Block ICMP Ping` rule through the Windows Firewall GUI.

I repeated the ping test from Kali Linux and confirmed that connectivity was restored.

This completed the test cycle: establish a baseline, apply a security control, test its effect, remove the temporary change, and verify the result.

<img width="547" height="223" alt="06-ping-restored" src="https://github.com/user-attachments/assets/16094ec2-cc50-4d02-857c-236202bae604" />


## Results

- Established baseline connectivity between the virtual machines.
- Identified open TCP ports and associated services using Nmap.
- Created an inbound Windows Firewall rule using the graphical interface.
- Observed 100% packet loss while the ICMP blocking rule was enabled.
- Restored ping connectivity after removing the rule.
- Documented the configuration and test results with screenshots.

## Security Takeaways

- Firewalls control network traffic according to configured rules.
- Nmap helps identify network services visible from a particular scanning location.
- An open port is not automatically a vulnerability; the service and its exposure must be assessed.
- Firewall changes should be tested and verified.
- Unnecessary network access should be restricted without disrupting required business services.
- Temporary lab changes should be cleaned up after testing.

## Limitations

This was a basic firewall and connectivity lab. It did not establish that every open service was vulnerable, perform a penetration test, or validate the security of a production environment.

Ping results alone do not prove that all network traffic is blocked or allowed. Service-specific testing is required to evaluate access to individual TCP or UDP ports.

## Skills Demonstrated

- Basic network troubleshooting
- Nmap scanning and service identification
- Windows Defender Firewall configuration
- ICMP connectivity testing
- Security control verification
- Technical documentation
- Screenshot-based evidence collection

## Conclusion

This lab provided hands-on practice with network scanning, firewall rule configuration, and connectivity verification in a controlled virtual environment. It reinforced the importance of understanding a server's role before changing firewall settings and verifying that security changes have the intended effect.
