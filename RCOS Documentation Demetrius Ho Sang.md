---
RCOS IBM-Ceph Starter Guide
---

In order to contribute to ceph from a computer that doesn't have the
necessary resources to compile the code you will need to use a VPN.

## **VPN SETUP**

In order to gain access to the VPN and ssh in you will need to generate
credentials and submit a support ticket with all relevant information.

### **Mac**

-   Step 1: Download [Tunnelblick](https://tunnelblick.net/downloads.html)

-   Step 2: Once Tunnelblick is downloaded and setup open terminal and run the following commands

> mkdir /etc/openvpn
>
> cd /etc/openvpn
>
> wget
> [https://filedump.ceph.com/sepia-vpn-client.tar.gz](https://filedump.ceph.com/sepia-vpn-client.tar.gz)
>
> sudo tar zxvf sepia-vpn-client.tar.gz
>
> \# Generate client credentials
>
> \# USER should be your desired username and HOST should describe your
> workstation
>
> \# e.g., dgalloway@thinkpad
>
> sudo ./sepia/new-client USER@HOST
>
> \# Submit the output of this command in your ticket

-   Step 3: Replace the line auth-user-pass sepia/secret with just auth-user-pass in client.conf, you may have to use sudo to edit it.

-   Step 4: Follow [Tunnelblick\'s instructions](https://tunnelblick.net/cConfigT.html) for adding the config

-   Step 5: When logging into the vpn you will be prompted for user/pass, enter username USER@HOST as above, and for password use the secret contents of the file /etc/openvpn/sepia/secret. , you won\'t be able to access the vpn yet as you still have to submit the ticket.

**Once credentials are generated**

Submit a ticket answering the following questions and wait for approval

1\) Desired Username (can be your rcsID id):

2\) E-mail address:

3\) If you don\'t already have an established history of code
 contributions to Ceph, is there an existing community or core
 developer you\'ve worked with who has reviewed your work and can vouch
 for your access request?

4\) Paste your SSH public key:

Generate one using these steps:

> 1\. Open Terminal
>
> 2\. Paste the text below, substituting in your GitHub email address.
>
> \`ssh-keygen -t rsa -C \"your_email@example.com\"\`
>
> 3\. When you\'re prompted to \"Enter a file in which to save the
> key,\" press Enter. This accepts the default file location.
>
> \> Enter a file in which to save the key (/home/you/.ssh/id_rsa):
> \[Press enter\]
>
> 4\. At the prompt, type a secure passphrase. you can press Enter if
> choose to not use passphrase.
>
> 5\. SSH key is now generated. Copy the contents of \"cat
> \~/.ssh/id_rsa.pub\" to this tracker.
>
> 5\) Paste your hashed VPN credentials (Format: user@hostname
> 22CharacterSalt 65CharacterHashedPassword)

**Once in the vpn**

-   Step 1: go to terminal and type

> ssh desiredUsername@smithi061.front.sepia.ceph.com
>
> OR
>
> ssh desiredUsername@smithi124.front.sepia.ceph.com

-   (Optional) Step 2: You can use the [remote-ssh extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) in visual
     studio code to code in there if you don't want to use VIM.



### **Ubuntu**

> sudo \[apt-get\|yum\] install openvpn
>
> sudo mkdir -p /run/openvpn
>
> \## Fedora 28 and later
>
> cd /etc/openvpn/client
>
> \## All others
>
> cd /etc/openvpn
>
> sudo wget https://filedump.ceph.com/sepia-vpn-client.tar.gz
>
> sudo tar zxvf sepia-vpn-client.tar.gz
>
> \# Generate client credentials
>
> \# USER should be your desired username and HOST should
>
> \# describe your workstation
>
> \# e.g., dgalloway@thinkpad
>
> sudo ./sepia/new-client USER@HOST
>
> \# Submit the command output in your ticket
>
> \# After you\'ve been notified in your ticket that access has been
> granted,
>
> sudo service openvpn restart
>
> OR
>
> sudo systemctl restart openvpn@sepia
>
> OR
>
> sudo systemctl restart openvpn-client@sepia
>
> \# Try all 3. One of them should work.
>
> \# Whichever works, enable the systemd service
>
> sudo systemctl enable openvpn@sepia
>
> OR
>
> sudo systemctl enable openvpn-client@sepia
>
Linux Gotchas
>
> You may need to edit user and group in /etc/openvpn/sepia/client.conf
> depending on what user the service runs as. This could be nobody,
> nogroup, or openvpn.
>
> sed -i \'s/nobody/openvpn/g\' /etc/openvpn/sepia/client.conf \|\| sed
> -i \'s/nobody/openvpn/g\' /etc/openvpn/client/sepia/client.conf
>
> sed -i \'s/nogroup/openvpn/g\' /etc/openvpn/sepia/client.conf \|\| sed
> -i \'s/nogroup/openvpn/g\' /etc/openvpn/client/sepia/client.conf
>
> If you\'re using OpenVPN for any other VPN connection (e.g., Red
> Hat\'s), you may need to change the dev name in
> /etc/openvpn/sepia/client.conf. See below.
>
> \# ERASE
>
> dev tun
>
> \# REPLACE WITH
>
> dev sepia0
>
> dev-type tun
>
> If the new-client script throws an error about /usr/bin/python not
> being found, run:
>
> sudo sed -i \'s\|/usr/bin/python\|/usr/bin/python3\|g\'
> sepia/new-client
>
Troubleshooting
>
> Please disable SELinux on RHEL clients
>
> To troubleshoot your VPN connection, try running the following command
> to determine where the connection is failing:
>
> openvpn \--config /etc/openvpn/sepia.conf \--cd /etc/openvpn \--verb 5
>
> OR
>
> openvpn \--config /etc/openvpn/client/sepia.conf \--cd
> /etc/openvpn/client \--verb 5
>

Fedora NetworkManager GUI

1.  Make sure you\'ve followed all the prerequisite steps
    > [here](https://wiki.sepia.ceph.com/doku.php?id=vpnaccess#linux)

2.  Right click the NetworkManager icon

3.  Edit Connections

4.  Click the + symbol

5.  Select Import a saved VPN configuration from the bottom

6.  Click Create

7.  Browse to /etc/openvpn/sepia/client.conf

8.  Enter your the first line in /etc/openvpn/sepia/secret (e.g., USER@HOST) under User name

9.  Enter the second line in your /etc/openvpn/sepia/secret file for Password

Fedora Network Manager GUI \-- Fedora 34

> This procedure was confirmed to work on Fedora 34 on 14 July 2021.

1.  Make sure you\'ve followed all the prerequisite steps above.

2.  Right click the NetworkManager icon

3.  Select Settings --\> Network

4.  Click the + symbol under VPN

5.  Select Import from file... from the bottom

6.  Browse to /etc/openvpn/client/sepia.conf

7.  Enter your the first line in /etc/openvpn/client/sepia/secret (e.g., USER@HOST) under User name

8.  Enter the second line in your /etc/openvpn/client/sepia/secret file for Password