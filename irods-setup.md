## Windows

1. Install Windows Subsystem for Linux (WSL). 
    
    - start PowerShell as administrator
    - ```wsl –install -d Ubuntu-18.04```
    
    Instructions are available here as well: https://learn.microsoft.com/en-us/windows/wsl/install

OR 

    - start PowerShell as administrator
    - ```wsl –install``` (this will install Ubuntu 20.something though)
    - Open the Microsoft Store (search for "store" from the start menu)
    - Search the store for "Ubuntu 18.04"
    - Install Ubuntu 18.04 LTS (it is not necessary to sign in to the store)

2. Quick setup of Ubuntu environment

    - Start Ubuntu 18.04 LTS
    - If prompted: 
        - Enter a username. This will create a local user account and you will be automatically logged in to Ubuntu 18.04 as this user.
        - Enter a password for the user and enter a second time to confirm.
    - Update all Ubuntu 18.04 software packages with sudo apt update && sudo apt upgrade -y

3. Run the following commands in Ubuntu:

```
sudo apt-get update
sudo apt-get install -y --only-upgrade ca-certificates
wget -qO - https://packages.irods.org/irods-signing-key.asc | sudo apt-key add -
echo "deb [arch=amd64] https://packages.irods.org/apt/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/renci-irods.list
sudo apt-get update
sudo apt -y install irods-runtime=4.2.11-1~bionic irods-icommands=4.2.11-1~bionic

# Prevent automatic upgrades of these two packages
sudo apt-mark hold irods-runtime irods-icommands

# Network tuning, ad hoc and permanent
echo "120" | sudo tee /proc/sys/net/ipv4/tcp_keepalive_time
echo "30" | sudo tee /proc/sys/net/ipv4/tcp_keepalive_intvl
echo "net.ipv4.tcp_keepalive_time=120" | sudo tee -a /etc/sysctl.d/99-sysctl.conf
echo "net.ipv4.tcp_keepalive_intvl=30" | sudo tee -a /etc/sysctl.d/99-sysctl.conf
```
The renci-irods.list that is created should contain: ```deb [arch=amd64] https://packages.irods.org/apt bionic main```

4. Create the iRODS directory and configuration file, within your home directory:

   - ```mkdir ~/.irods```
   - ```nano ~/.irods/irods_environment.json``` 

5. Update the iRODS configuration file to match the Yoda research environment you want to work with

    - See the following for all information on each environment: https://www.uu.nl/en/research/yoda/guide-to-yoda/i-am-using-yoda/using-icommands-for-large-datasets

6. Log into iRODS

    - Type in ```iinit``` to log into Yoda via iRODS
