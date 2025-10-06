# Wazuh Manager + Mac + Windows Setup

## Overview
This repository documents how I installed Wazuh Manager on my Lenovo (Ubuntu VM) and connected both a Windows 11 and macOS agent.  
It’s meant as a beginner-friendly, step-by-step record of what worked for me.

---

## Manager (Ubuntu VM)

1. Installed Wazuh Manager and Dashboard using official packages.  
2. Verified services were running:

    sudo systemctl status wazuh-manager  
    sudo systemctl status wazuh-dashboard  

3. Confirmed the dashboard loaded at:  
   http://<your-vm-ip>:5601  

---

## Windows Agent

1. Downloaded and installed wazuh-agent.msi on my Windows system.  
2. Edited the agent config file to set the Manager’s IP:

    <server>  
      <address>192.168.1.81</address>  
      <port>1514</port>  
      <protocol>tcp</protocol>  
    </server>  

3. Imported the authentication key from the Manager.  
4. Restarted the agent and confirmed it showed as **Active** in the Wazuh Dashboard.  

---

## macOS Agent

1. Downloaded the ARM64 package:

    curl -L -o ~/Downloads/wazuh-agent.pkg https://packages.wazuh.com/4.x/macos/wazuh-agent-4.12.0-1.arm64.pkg  
    sudo installer -pkg ~/Downloads/wazuh-agent.pkg -target /  

2. Edited the configuration:

    sudo nano /Library/Ossec/etc/ossec.conf  

   Updated the server block:

    <server>  
      <address>192.168.1.81</address>  
      <port>1514</port>  
      <protocol>tcp</protocol>  
    </server>  

3. Imported the authentication key:

    sudo /Library/Ossec/bin/manage_agents  

4. Restarted the agent:

    sudo /Library/Ossec/bin/wazuh-control restart  

5. Confirmed the Mac agent appeared as **Active** in the dashboard.  

---

## Notes

- Both Windows and Mac agents now show “Active” in the dashboard.  
- Removed old ghost agent ID 003 using:

    sudo /var/ossec/bin/manage_agents -r 003  

- This setup uses the default port 1514/TCP for Manager ↔ Agent communication.  

---

## Credits

Project by **0wnleahshad0w**, documenting a multi-platform Wazuh deployment and connection process.
