# Moonitor

## Installation

1. Run Ubuntu 22.04 LTS (Jammy Jellyfish)
   <details><summary>Ubuntu VM installation guide</summary>

   <br/>**Downloads:**
   - [VirtualBox](https://download.virtualbox.org/virtualbox/6.1.34/VirtualBox-6.1.34-150636-Win.exe)
   - [Ubuntu image](https://ubuntu.com/download/desktop/thank-you?version=22.04&architecture=amd64)
   
   <br/>**Add virutal machine:**
   - Open VirtualBox
   - Click 'New' to create a new machine
   - Enter your VM name, change the 'Type' to Linux and the 'Version' to Ubuntu (64-bit), and then proceed
   - Set the machine RAM, the default memory size should be sufficient for Moonitor. Then proceed
   - Select 'Create a virtual hard disk now' and proceed
   - Select 'VDI (VirtualBox Disk Image)' as hard disk file type, and proceed
   - Select 'Dynamically allocated' for storage on physical hard disk, and proceed
   - Set the hard disk location and size, it is recommended to allocate at least 20GB. Then proceed
   
   <br/>**Configure network:**
   - Click 'Settings' to open VM settings window
   - Click 'Network' and set 'Attached to' to 'Bridged Adapter'
   - Set 'Name' to a host interface from the list
   - Click 'OK' to save the settings
   
   <br/>**Boot the machine:**
   - Click 'Start' to start the machine
   - A start-up disk selection popup will appear, click the folder icon to select an Ubuntu image
   - Click 'Add' and select the Ubuntu image file (.iso) you have already downloaded
   - Click 'Choose' and then 'Start' in order to start-up the machine using the selected image
   - A black screen with boot options will appear, navigate with arrow keys to 'Try or install Ubuntu' and press 'Enter' to select
  
   <br/>**Install the image**
   - After a while, a white screen will appear. Click 'Install Ubuntu'
   - Select your keyboard layout and proceed
   - In 'Updates and other software', select 'Minimal installation' and procees
   - Select 'Erase disk and install Ubuntu' and click 'Install Now' to proceed
   - A popup regarding disk changes will appear, click 'Continue'
   - Select your time zone and proceed
   - Fill in your name, computer name, username and password. Then proceed
   - After a few minutes, the installation will finish. Click 'Restart Now' when the popup appears
   - When the message 'Please remove the installation medium, then press ENTER:' appears, press enter
   - When the restart finishes, an 'Online Accounts' page will appear, click 'Skip' to proceed
   - Select 'No, don't send info to Canonical'. Then click 'Next' to proceed
   - Keep the default privacy settings and click 'Next' to proceed
   - Click 'Done' to start using your new machine
   - Congratulations! You have an Ubuntu VM
   </details>

2. Start a bash on the VM using Terminal app, or an ssh connection
   
3. Switch to root user
```bash
sudo su
```

4. Install git
```bash
apt-get update
apt-get install -y git
```

5. Clone Moonitor Setup files from GitHub
```bash
cd /root
git clone https://github.com/MoonfaceDev/moonitor-setup.git "Moonitor Setup"
cd "Moonitor Setup"
```

## Setup Wizard
1. Install net-tools
```bash
apt-get install -y net-tools
```

2. Check VM's IP address and subnet (keep for later use)
```bash
ip a
```
Example:
![ip-a-guide](https://user-images.githubusercontent.com/36325466/166083106-83c2c111-fc58-4ecf-853f-a3be3cbe15cb.png)
In this example, the VM's IP address is 10.100.102.143 and the subnet is 10.100.102.0/24.

3. Check gateway IP:
```bash
route -n
```
![route-n-guide](https://user-images.githubusercontent.com/36325466/166083652-3dcf73c9-d01f-4445-9a78-3259720eae72.png)
Example:
In this example, the gateway IP address is 10.100.102.1

4. Check gateway MAC:
```bash
# Replace <gateway_ip> with gateway's IP address
arp <gateway_ip>
```
![arp-guide](https://user-images.githubusercontent.com/36325466/166083908-9a5053eb-ea51-4cd9-b8bd-e2590e4a46dd.png)
Example:
In this example, the MAC addresses ends with '...:c4:87'

5. Run Moonitor setup wizard
```bash
./moonitor setup
```
The wizard will ask you for the following parameters:
- Server hostname: hostname (IP address, NetBIOS or FQDN) for external users to connect to the server
<br/>_Examples: 10.100.102.143, elaipc_
- Gateway IP: IP address of the default gateway (router)
<br/>_Example: 10.100.102.1_
- Gateway MAC: MAC address of the default gateway (router)
- Network subnet: IP addresses to ping in each scan
<br/>_Example: 10.100.102.0/24_

<br/>Enter all parameters, and moonitor will do the rest :)

## Other Options
* Start services:
```bash
./moonitor start
```

* Stop services:
```bash
./moonitor stop
```

* Restart services:
```bash
./moonitor restart
```

* Enable user:
```bash
# Replace <user_email> with user_email
./moonitor enable-user <user_email>
```

* Disable user:
```bash
# Replace <user_email> with user_email
./moonitor disable-user <user_email>
```

## Configuration

### moonlan (API) configuration
path: /etc/moonitor/api/config.json

| key                         | meaning                                             |
|-----------------------------|-----------------------------------------------------|
| secret_key                  | Private key for JWT signing                         |
| algorithm                   | Algorithm for JWT signing                           |
| access_token_expire_minutes | JWT expire duration                                 |
| database_name               | MongoDB database name                               |
| gateway_ip                  | IP of default gateway in LAN (usually router)       |
| gateway_mac                 | MAC of default gateway in LAN (usually router)      |
| ip_forward_file_path        | Path to file determining packet forwarding behavior |
| allowed_origins             | List of allowed origins for CORS                    |


### moonlan (API) devices
path: /etc/moonitor/api/devices.json

Includes a list of devices, with each device having the following format:
{
   "name": ...,
   "mac": ...,
   "type": ...,
}

Supported types:
PC, Phone, Game console, Router, TV Adapter, Music, TV, Tablet, Printer, Security


### moonscan configuration
path: /etc/moonitor/scan/config.json

| key            | meaning                               |
|----------------|---------------------------------------|
| network_subnet | Subnet of IP addresses to scan        |
| scan_interval  | Time between scans                    |
| ports_to_scan  | Number of most frequent ports to scan |


### Nginx (HTTP server) configuration
path: /etc/nginx/sites-available/moonlan.conf

Example HTTP configuration:
```
server {
	server_name <server_name>;
	listen 80;
	location / {
		proxy_pass http://127.0.0.1:3000;
		proxy_set_header X-Real-IP $remote_addr;
      		proxy_set_header Host $host;
      		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}
}
```

Example HTTPS configuration:
```
server {
	server_name <server_name>;
	location / {
		proxy_pass http://127.0.0.1:3000;
	}
	listen 443 ssl; # managed by Certbot
	ssl_certificate /etc/letsencrypt/live/<server_name>/fullchain.pem; # managed by Certbot
	ssl_certificate_key /etc/letsencrypt/live/<server_name>/privkey.pem; # managed by Certbot
	include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
	ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
} server {
	if ($host = <server_name>) {
		return 301 https://$host$request_uri;
    	} # managed by Certbot
	listen 80;
	server_name <server_name>;
	return 404; # managed by Certbot
}
```
