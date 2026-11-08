# Windows Firewall Hardening Lab

Student cybersecurity project documenting inbound and outbound traffic rules in Windows Defender Firewall with Advanced Security.

**Author:** Camilo Hernandez  
**Lab date:** November 8, 2026  
**Environment:** Windows virtual machine in a browser-based training lab  
**Exercise:** 5.12 LAB: Securing the network (Scenario)

## Project Overview

I worked through this exercise to practice controlling network access with a host firewall. The scenario described a fictional company, ABC Tech, reviewing misconfigured firewall settings. My task was to follow the lab's traffic requirements and document the resulting rules.

As a computer science student developing my cybersecurity skills, I used this lab to connect familiar protocol names with practical decisions about traffic direction, ports, and allow or block actions.

## Objectives

- Practice creating inbound and outbound firewall rules.
- Identify the protocols and service ports associated with the lab requirements.
- Allow required services and restrict services that the scenario did not permit.
- Review rule actions, enabled states, and network profiles.
- Document the configuration with screenshots and explain what still needs verification.

## Tools and Environment

| Component | Use |
| --- | --- |
| Windows virtual machine | Isolated training environment |
| Windows Defender Firewall with Advanced Security | Review and configure firewall rules |
| Inbound and Outbound Rule Wizards | Select rule criteria, actions, and profiles |
| Lab checking tool | Receive feedback while working through the exercise |
| GitHub and Markdown | Organize the report and screenshot evidence |

## Firewall Policy

The screenshots show the following named rules enabled with the actions listed below. These rules display **All** in the Profile column.

The port columns are **protocol reference notes**, not an export of the rule settings. The screenshots do not expose the individual rules' protocol and port fields, so those settings require a separate properties screenshot or configuration export to verify.

### Inbound Rules

| Service or traffic | Visible action | Common protocol / port reference | Purpose in this lab |
| --- | --- | --- | --- |
| ICMP | Block | ICMP; no TCP or UDP port | Follow the scenario's ICMP restriction |
| L2TP | Block | UDP 1701 | Restrict the VPN traffic specified by the lab |
| FTP | Allow | TCP 21 for control; data connections depend on FTP mode | Permit the required file transfer service |
| SSH | Allow | TCP 22 | Permit secure remote administration |
| Telnet | Block | TCP 23 | Restrict an unencrypted remote terminal protocol |
| HTTPS | Allow | TCP 443 | Permit encrypted web service traffic |
| SMB | Block | TCP 445; legacy NetBIOS uses additional ports | Restrict file sharing access |
| RDP | Block | TCP and UDP 3389 | Restrict remote desktop access |

### Outbound Rules

| Rule name in screenshot | Visible action | Common protocol / port reference | Purpose in this lab |
| --- | --- | --- | --- |
| SMTP traffic | Block | TCP 25 | Apply the scenario's restriction on standard SMTP traffic |
| SMTP secure | Allow | TCP 465 for implicit TLS, or TCP 587 with STARTTLS | Permit the required mail submission service |
| TFTP block | Block | UDP 69 for the initial request | Restrict the file transfer protocol specified by the lab |

The secure SMTP rule's configured port is not visible in the evidence. Ports 465 and 587 describe different TLS connection methods; they are not a claim that both were configured or required by the lab.

## Configuration Workflow

1. Reviewed the scenario and separated its inbound requirements from its outbound requirements.
2. Opened Windows Defender Firewall with Advanced Security and selected the appropriate rule category.
3. Used the New Rule wizard to configure the required traffic criteria.
4. Selected the allow or block action and the applicable network profiles.
5. Gave each rule a recognizable service name.
6. Reviewed the rule lists and used the lab's checking tool for feedback.
7. Captured the screenshots included in this repository.

## Screenshot Evidence

### Inbound Rule List

The list shows enabled allow rules for FTP, SSH, and HTTPS, and enabled block rules for ICMP, L2TP, Telnet, SMB, and RDP.

![Inbound firewall rule names, enabled states, profiles, and actions](screenshots/inbound-rules.png)

### Outbound Rule List

The list shows the enabled SMTP block rule, secure SMTP allow rule, and TFTP block rule.

![Outbound SMTP and TFTP rule names, enabled states, profiles, and actions](screenshots/outbound-rules.png)

<details>
<summary>View the rule wizard screenshots</summary>

The inbound wizard shows the available rule types before a port rule is selected.

![Inbound wizard showing program, port, predefined, and custom rule types](screenshots/inbound-rule-wizard.png)

The outbound wizard shows TCP selected and the **Specific remote ports** field. The field is empty in this capture, so this image illustrates the configuration interface rather than a completed port setting.

![Outbound wizard showing protocol selection and the remote port field](screenshots/outbound-port-wizard.png)

</details>

## Troubleshooting and What I Learned

### Rule Direction Matters

One point I worked through was the difference between incoming and outgoing traffic. An inbound rule controls traffic entering the computer; an outbound rule controls traffic leaving it. A rule in the wrong category does not satisfy the intended requirement, even if the service name and port look correct.

For typical service rules, the inbound rule matches the local service port and the outbound rule matches the remote service port. Reviewing the SMTP instructions helped make this distinction clearer. [1]

### Allowing a Port Does Not Enforce Encryption

I learned to separate firewall access from the application's security settings. Opening a port allows matching traffic, but it does not configure certificates or require a successful TLS connection.

SMTP submission commonly uses implicit TLS on port 465 or STARTTLS on port 587. Secure use depends on the client and server requiring TLS. SMTP on port 25 can also support STARTTLS, so blocking port 25 is a lab policy choice rather than a test for unencrypted traffic. [2][3]

### Lab Requirements Need Context

The exercise required allowing FTP. Plain FTP does not provide transport encryption; I would investigate SFTP or properly configured FTPS for a real deployment. FTP also uses separate control and data connections, so allowing port 21 alone does not establish that file transfers work.

I also learned that restricting a service should be tied to the environment's needs. SMB, RDP, VPN traffic, and ICMP have legitimate uses. The decisions in this report reflect this training scenario and would need review before being applied elsewhere.

### Documentation Should Match the Evidence

The screenshots support specific observations about rule names, actions, profiles, and enabled states. They do not establish successful connections, encryption, or complete lab acceptance. Recording that distinction makes the report easier to review and reproduce.

## Verification Status

| Check | Evidence available |
| --- | --- |
| Named inbound rules are present and enabled | Inbound rule list screenshot |
| Named outbound rules are present and enabled | Outbound rule list screenshot |
| Displayed allow/block actions and profiles | Both rule list screenshots |
| Exact protocols, port values, and address scope | Not captured in the provided screenshots |
| Final passing result for every lab requirement | Final completion screenshot not included |
| Actual allowed and blocked network connections | Traffic test results not recorded |

The current evidence documents the visible configuration. Further verification would include rule properties or an export, a final lab result, and connection tests from an authorized lab host.

## Next Steps

- Capture each rule's protocol, port, direction, and address scope.
- Add the final lab completion screenshot when available.
- Test allowed and blocked connections and record the results.
- Practice using PowerShell to inspect and export firewall rules.
- Explore how narrower profiles and address scopes change the policy.

## Skills Practiced

Windows Firewall configuration, inbound and outbound filtering, protocol and port identification, troubleshooting, screenshot documentation, and Markdown reporting.

## References

1. [Microsoft Learn: Configure Windows Firewall rules](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure)
2. [RFC 8314: TLS for Email Submission and Access](https://www.rfc-editor.org/rfc/rfc8314.html)
3. [RFC 3207: SMTP Service Extension for Secure SMTP over TLS](https://www.rfc-editor.org/rfc/rfc3207.html)
4. [IANA: Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)

This repository documents my student lab work and learning process. The screenshots were captured in the training virtual machine.
