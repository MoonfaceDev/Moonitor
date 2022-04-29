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
   - Set the hard disk location and size, the default disk size should be sufficient for Moonitor. Then proceed
   
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
   - In 'Updates and other software', keep the default options and proceed
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


```bash
sudo su
cd /root
apt-get update
apt-get install -y git
git clone https://github.com/MoonfaceDev/moonitor-setup.git "Moonitor Setup"
cd "Moonitor Setup"
./moonitor setup
```
