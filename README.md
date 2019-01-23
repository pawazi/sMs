Simple-Mac-Script with Ansible deployment.

This playbook contains the service automated some daily tasks on your Mac. It was created to help the IT classroom administrators or superusers to handle routine tasks jobs (cleaning Desktop, Document folder, adjust screen saver, etc.). Ansible doesn't calll the service, it only sends it to the hosts.

The service itself does the following tasks:

    - Cleaning Desktop and Documents folder.
    - Changing the Desktop Background.
    - Setting the screensaver options.
    - Changing power management options (sleeping after 15 mins, hibernating after 60).
    - Setting the mouse sensitivity to the default value.
    - Adding apps to the Dock.
    - Restoring keyboard keys to the default values.
    - Adjusting display's resolution for the connected monitor.

Prerequisites:

Edit the group_vars/all/vars file to properly assign the variables:

---
remote_user: 'remote_user' ---account with admin privileges
localusr: 'localusr' ---name of the account the service will be running from
localgrp: 'staff'  ---name of the group the user is assigned to
thumbApps:	       ---list of the applications you want to be picked to the Dock
 - '/Applications/iBooks.app'
lan: 'en'	       ---system language

Add the IP to the inventory file (./hosts).

Next, from the main folder, run the command:

	ansible-playbook -i hosts push_mac_script.yml --ask-become-pass

How to run the service?

Before running, it is recommended to close all aplications and exit the system preferences.

Log on your Mac as 'localusr' and click anywhere on the desktop. In the top left corner you'll see the 'Finder' button. Click them, next, go to Services and select the sMs.service. A rotating gear wheel will appear in the menu bar and, if you have the notifications turned on, it will inform you about every action.
Next, you will be asked for the images for the desktop background and for the screen server.

The service calls the pmset command that requires root privileges. Ansible will add the special file to sudoers.d folder, to avoid typing the admin/user password every time it is used.

Tested only on Sierra/High Sierra systems.

After running Ansible, the changes on Mac are as follows:

-	Adding sudoers file to /etc/sudoers.d folder.
-	Creating hidden folder in home directory to store the images for screen saver.
-	Adding sMs.service to ~/Library/Services folder.
