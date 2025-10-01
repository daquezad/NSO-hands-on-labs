# Day 1

## Contents {#contents .TOC-Heading}
[Cisco Network Services Orchestrator
[3](#cisco-network-services-orchestrator)](#cisco-network-services-orchestrator)

[1 - Connect to the Workstation
[3](#connect-to-the-workstation)](#connect-to-the-workstation)

[1.1 - Install NSO and NEDs
[4](#install-nso-and-neds)](#install-nso-and-neds)

[1.2 -- Registering XRd routers
[9](#registering-xrd-routers)](#registering-xrd-routers)

[1.3 - Configure Devices using NSO
[11](#configure-devices-using-nso)](#configure-devices-using-nso)

[1.4 - Use Rollback's [13](#use-rollbacks)](#use-rollbacks)

[1.5 - Use NSO capabilities to detect Out-of-Band device configurations
[15](#use-nso-capabilities-to-detect-out-of-band-device-configurations)](#use-nso-capabilities-to-detect-out-of-band-device-configurations)

[1.6 - Create Device Groups and Device Templates
[18](#create-device-groups-and-device-templates)](#create-device-groups-and-device-templates)

[1.7 - Create a Simple NSO Service
[23](#create-a-simple-nso-service)](#create-a-simple-nso-service)

[1.8 - Create Role Based and Resource Based Access Control rules
[37](#create-role-based-and-resource-based-access-control-rules)](#create-role-based-and-resource-based-access-control-rules)

**\
**

## Cisco Network Services Orchestrator {#cisco-network-services-orchestrator .Style-Heading-1-+-(Latin)-CiscoSansTT-Custom-Color(RGB(080115))}

##  Connect to the Workstation

Please click on the \"Win11\" option, and then select \"VM Console\"
from the Remote Access.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image2.png){width="6.268055555555556in" height="3.4770833333333333in"} -->

After selecting \"VM Console,\" you will be redirected to the Windows
operating system interface. If you see "You're almost done setting up
your PC," then click "Remind me in 3 days."

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image3.png){width="6.268055555555556in" height="3.2875in"} -->

Alternatively, you can choose an Ubuntu Desktop. Please click on the
\"Ubuntu 24.04 Desktop\" and then select \"VM Console\" from the Remote
Access.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image4.png){width="6.268055555555556in" height="3.854861111111111in"} -->

# 1.1 - Install NSO and NEDs 

-   Click on Visual Studio Code shortcut and work on the Terminal
    window.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image5.png){width="6.268055555555556in" height="3.2888888888888888in"} -->

cisco@xrd-host:\~\$ cd NSO-6.5-free/

<!-- Deep research: 
This command navigates to the NSO installation directory and lists all files with the .bin extension. These .bin files are NSO and NED installers, which are signed binaries provided by Cisco for NSO and device support. 
-->
**Example: List .bin files in NSO directory**
```bash
cisco@xrd-host:~$ cd NSO-6.5-free/
cisco@xrd-host:~/NSO-6.5-free$ ls *.bin
ncs-6.5-cisco-asa-6.18.23-freetrial.signed.bin
ncs-6.5-cisco-ios-6.109.4-freetrial.signed.bin
ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin
ncs-6.5-cisco-nx-5.27.3-freetrial.signed.bin
nso-6.5-freetrial.container-image-build.linux.x86_64.signed.bin
nso-6.5-freetrial.container-image-prod.linux.x86_64.signed.bin
nso-6.5-freetrial.linux.x86_64.signed.bin
```

First, you need to extract installer ".bin" file. To avoid messing up
the directory, create a new directory named "work" and execute "bash
../nso-6.5-freetrial.linux.x86_64.signed.bin". Using
"\--skip-verification" skips certification check.

cisco@xrd-host:\~/NSO-6.5-free\$ mkdir work

cisco@xrd-host:\~/NSO-6.5-free\$ cd work

cisco@xrd-host:\~/NSO-6.5-free/work\$

bash ../nso-6.5-freetrial.linux.x86_64.signed.bin \--skip-verification

Unpacking\...

cisco@xrd-host:\~/NSO-6.5-free/work\$

<!-- Deep research:
This sequence creates a working directory to avoid cluttering the main install folder, then extracts the NSO installer using bash. The --skip-verification flag disables certificate checks, which is useful for lab or trial environments.
-->
**Example: Extract installer .bin file**
```bash
cisco@xrd-host:~/NSO-6.5-free$ mkdir work
cisco@xrd-host:~/NSO-6.5-free$ cd work
cisco@xrd-host:~/NSO-6.5-free/work$ bash ../nso-6.5-freetrial.linux.x86_64.signed.bin --skip-verification
Unpacking...
cisco@xrd-host:~/NSO-6.5-free/work$
```

Execute the **nso-6.5.linux.x86_64.installer.bin** with the
"---local-install" flag and choose the installation directory.

cisco@xrd-host:\~/NSO-6.5-free/work\$

bash nso-6.5.linux.x86_64.installer.bin \--local-install \~/NSO-INSTALL

INFO Using temporary directory /tmp/ncs_installer.780985 to stage NCS
installation bundle

INFO Unpacked ncs-6.5 in /home/cisco/NSO-INSTALL

INFO Found and unpacked corresponding DOCUMENTATION_PACKAGE

INFO Found and unpacked corresponding EXAMPLE_PACKAGE

INFO Found and unpacked corresponding JAVA_PACKAGE

INFO Generating default SSH hostkey (this may take some time)

INFO SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ecdsa_key hostkey
generated

INFO SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ed25519_key
hostkey generated

INFO Generating self-signed certificates for HTTPS

INFO Environment set-up generated in /home/cisco/NSO-INSTALL/ncsrc

INFO NSO installation script finished

INFO Found and unpacked corresponding NETSIM_PACKAGE

INFO NCS installation complete

<!-- Deep research:
This runs the extracted installer with --local-install, specifying the target directory for NSO. The installer unpacks NSO and its documentation, examples, Java support, and generates SSH keys and HTTPS certificates for secure management.
-->
**Example: Execute NSO installer**
```bash
cisco@xrd-host:~/NSO-6.5-free/work$ bash nso-6.5.linux.x86_64.installer.bin --local-install ~/NSO-INSTALL
INFO Using temporary directory /tmp/ncs_installer.780985 to stage NCS installation bundle
INFO Unpacked ncs-6.5 in /home/cisco/NSO-INSTALL
INFO Found and unpacked corresponding DOCUMENTATION_PACKAGE
INFO Found and unpacked corresponding EXAMPLE_PACKAGE
INFO Found and unpacked corresponding JAVA_PACKAGE
INFO Generating default SSH hostkey (this may take some time)
INFO SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ecdsa_key hostkey generated
INFO SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ed25519_key hostkey generated
INFO Generating self-signed certificates for HTTPS
INFO Environment set-up generated in /home/cisco/NSO-INSTALL/ncsrc
INFO NSO installation script finished
INFO Found and unpacked corresponding NETSIM_PACKAGE
INFO NCS installation complete
```

Go to the installation folder and source NSO

cisco@xrd-host:\~/NSO-6.5-free/work\$ cd \~/NSO-INSTALL/

cisco@xrd-host:\~/NSO-INSTALL\$ source ncsrc

<!-- Deep research:
The 'source ncsrc' command loads environment variables required for NSO CLI and tools. This script sets up paths and variables so subsequent NSO commands work correctly in the shell session.
-->
**Example: Source NSO environment**
```bash
cisco@xrd-host:~/NSO-INSTALL$ source ncsrc
```

Execute the NSO Setup script

cisco@xrd-host:\~/NSO-INSTALL\$ ncs-setup \--dest nso-instance

<!-- Deep research:
'ncs-setup --dest nso-instance' initializes a new NSO runtime instance in the specified directory. This creates the configuration, state, and package folders for the NSO server.
-->
**Example: NSO Setup script**
```bash
cisco@xrd-host:~/NSO-INSTALL$ ncs-setup --dest nso-instance
```

Change directory to the "nso-instance" folder and find "ncs.conf".

cisco@xrd-host:\~/NSO-INSTALL\$ cd nso-instance/

cisco@xrd-host:\~/NSO-INSTALL/nso-instance\$ ls

README.ncs logs ncs-cdb ncs.conf packages scripts state

<!-- Deep research:
This lists the contents of the NSO instance directory. You should see configuration files, logs, package folders, and state directories, which are essential for NSO operation and package management.
-->
**Example: List contents of nso-instance folder**
```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ls
README.ncs logs ncs-cdb ncs.conf packages scripts state
```

From NSO 6.5, WebUI and RESTCONF are allowed only to NSO server's
hostname (such as "localhost"). To allow an access via IP address,
following line has to be added to ncs.conf. Add
\<server-alias\>198.18.134.27\</server-alias\> to line 330 and save the
file.

<!-- Deep research:
This XML snippet shows how to enable the NSO WebUI for access via a specific IP address. By default, NSO restricts WebUI and RESTCONF to localhost for security. Adding <server-alias> allows remote access for labs or demos.
-->
**Example: Add server-alias to ncs.conf**
```xml
326 <allow-case-insensitive-enums>true</allow-case-insensitive-enums>
327 </cli>
328
329 <webui>
330 <server-alias>198.18.134.27</server-alias>
331 <enabled>true</enabled>
332 <transport>
333   <tcp>
334     <enabled>true</enabled>
335     <ip>0.0.0.0</ip>
```

Once added, start NSO by executing "ncs" command.

cisco@xrd-host:\~/NSO-INSTALL/nso-instance\$ ncs

<!-- Deep research:
This starts the NSO server. The 'ncs' command launches the NSO process, which loads configuration, packages, and starts the management interfaces (CLI, WebUI, RESTCONF).
-->
**Example: Start NSO**
```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ncs
```

Check if NSO is running

cisco@xrd-host:\~/NSO-INSTALL/nso-instance\$ ncs \--status \| grep
started

status: started

<!-- Deep research:
Checks the status of the NSO server. The output 'status: started' confirms that NSO is running and ready to accept management connections.
-->
**Example: Check NSO status**
```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ncs --status | grep started
status: started
```

To log in to NSO via the Web UI, open Firefox and access following URL:
<http://198.18.134.27:8080/login.html>

You can log in using the default username and password, which are
\"admin\" for both the username and password fields.

<!-- ![A screenshot of a computer server room AI-generated content may be incorrect.](media/image6.png){width="6.268055555555556in" height="2.8090277777777777in"} -->

After logging in, you will be directed to the main page, which displays
the available applications and components provided by NSO.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image7.png){width="6.268055555555556in" height="2.84375in"} -->

Please navigate to the \"Config Editor\" section.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image8.png){width="6.268055555555556in" height="1.6319444444444444in"} -->

Since this is a fresh installation and we do not have any packages yet,
let's proceed by adding the Network Element Drivers (NEDs).

In the \"\~/NSO-6.5-free \" folder, you should find some file starting
with "ncs-" and they are NED packages.

cisco@xrd-host:\~/NSO-INSTALL/nso-instance\$ cd \~/NSO-6.5-free/

cisco@xrd-host:\~/NSO-6.5-free\$ ls ncs\*

ncs-6.5-cisco-asa-6.18.23-freetrial.signed.bin
ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin

ncs-6.5-cisco-ios-6.109.4-freetrial.signed.bin
ncs-6.5-cisco-nx-5.27.3-freetrial.signed.bin

cisco@xrd-host:\~/NSO-6.5-free\$

You also need to extract NED files. To avoid messing up the directory,
go to the work folder and execute "bash ../\<ned.signed.bin\>" commands.
Using "\--skip-verification" skips certification check.

cisco@xrd-host:\~/NSO-6.5-free\$ cd work/

cisco@xrd-host:\~/NSO-6.5-free/work\$

bash ../ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin
\--skip-verification

Unpacking\...

cisco@xrd-host:\~/NSO-6.5-free/work\$

"ncs-6.5-cisco-iosxr-7.69.tar.gz" file is the NED package. Copy the file
to nso-instance/packages folder.

cisco@xrd-host:\~/NSO-6.5-free/work\$

cp ncs-6.5-cisco-iosxr-7.69.tar.gz \~/NSO-INSTALL/nso-instance/packages/

<!-- Deep research:
This command copies the extracted NED package (Network Element Driver) to the NSO packages directory. NEDs enable NSO to manage specific device types (e.g., Cisco IOS XR).
-->
**Example: Copy NED package to NSO packages folder**
```bash
cisco@xrd-host:~/NSO-6.5-free/work$ cp ncs-6.5-cisco-iosxr-7.69.tar.gz ~/NSO-INSTALL/nso-instance/packages/
```

Please return to the NSO Web UI, go to ncs:packages and select
"Actions", then click the \"Reload\" button for the packages.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image9.png){width="6.268055555555556in" height="3.084722222222222in"} -->

After executing the \"Run reload action\" button, you should now be able
to see the \"cisco-ios-xr\" package in the list.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image10.png){width="6.268055555555556in" height="1.7104166666666667in"} -->

Now that we have added our Network Element Driver (NED), let\'s navigate
to the \"Devices\" section.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image11.png){width="6.268055555555556in" height="2.047222222222222in"} -->

Currently, there are no devices listed. However, we have two options to
add them: manually by clicking on the \"+\" button, or programmatically
using scripts or APIs.

# 1.2 -- Registering XRd routers

There are two XRd routers running on the server which you have installed
NSO. Please confirm they are running by executing "docekr ps".

cisco@xrd-host:\~/NSO-6.5-free/work\$ docker ps

CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

8502937299d3 ios-xr/xrd-control-plane:25.2.1 \"/usr/local/sbin/init\" 5
weeks ago Up 4 days xr-2

2d77e0ef76f7 alpine:3.15 \"/bin/sh -c \'ip rout...\" 5 weeks ago Up 4
days source

4a68f412191b ios-xr/xrd-control-plane:25.2.1 \"/usr/local/sbin/init\" 5
weeks ago Up 4 days xr-1

593c076cbbe7 alpine:3.15 \"/bin/sh -c \'ip rout...\" 5 weeks ago Up 4
days dest

cisco@xrd-host:\~/NSO-6.5-free/work\$

A device template has been prepared to load these devices. Go to
NSO-6.5-free and load "devices.xml" by using the "ncs_load" command to
load them. -l ( load ) -m ( merge )

cisco@xrd-host:\~/NSO-6.5-free/work\$ cd \~/NSO-6.5-free/

cisco@xrd-host:\~/NSO-6.5-free\$ ncs_load -l -m devices.xml

Go back to NSO and click on Devices and you see two XRd routers there.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image12.png){width="6.268055555555556in" height="1.601388888888889in"} -->

The first thing you need to do after a device to NSO is execute the
"sync-from" so you update NSO CDB with the device configuration. Select
all devices and make a sync-from

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image13.png){width="6.268055555555556in" height="2.0597222222222222in"} -->
<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image14.png){width="6.268055555555556in" height="2.473611111111111in"} -->

If sync-from failed, please do "fetch ssh host keys" and try again.

# 1.3 - Configure Devices using NSO

You can enter in ios-xr-0 to verify interface configurations. To
navigate in the configurations, you can use the Top bar (where I typed
"confi" ) or scroll down and look for what you want to configure.

<!-- ![A screenshot of a computer Description automatically generated](media/image15.png){width="6.268055555555556in" height="2.435416666666667in"} -->

You can follow the path and verify the GigabitEthernet0/0/0/0
configuration

Full Path:
/ncs:devices/device{xr-1}/config/cisco-ios-xr:interface/GigabitEthernet{0/0/0/0}/ipv4/address/

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image16.png){width="6.268055555555556in" height="1.4534722222222223in"} -->

Let's make a change in the IP address. To do it, you just need to click
on Edit Config and then change the field IP. Please change address from
"10.1.1.3" to "10.1.1.30".

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image17.png){width="6.268055555555556in" height="2.640277777777778in"} -->

You can notice there is a green bar on the left of the tile that you
edited. This means there is a candidate configuration on-going, and this
configuration will only be pushed to the device after you commit.

Let's go the Tools \> Commit Manager and see "config" tab.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image18.png){width="6.268055555555556in" height="2.595138888888889in"} -->

In the commit manager we have a diff view like on github. If we agree
with the changes, we click on commit.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image19.png){width="6.268055555555556in" height="2.786111111111111in"} -->

After our commit is done, we can see a rollback is created. This is
because, every time we make a commit in NSO we create a rollback file
that allow us to revert what we did.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image20.png){width="6.268055555555556in" height="1.4243055555555555in"} -->

If we go back to the router, we can see that our device already has the
new IP on the Loopback Interface

<!-- Deep research:
This SSH command connects to a managed device and shows the running configuration for a specific interface. It's used to verify that NSO has successfully pushed configuration changes to the device.
-->
**Example: SSH into device and show interface config**
```bash
cisco@xrd-host:~$ ssh admin@172.30.0.2
(admin@172.30.0.2) Password: (password = cisco123)
Last login: Tue Sep 30 03:08:41 2025 from 172.30.0.1
RP/0/RP0/CPU0:xr-1#show run interface gigabitEthernet 0/0/0/0
Tue Sep 30 03:10:55.127 UTC
! Configure left data port
interface GigabitEthernet0/0/0/0
ipv4 address 10.1.1.30 255.255.255.0
!
RP/0/RP0/CPU0:xr-1#
```

# 1.4 - Use Rollback's

To Rollback this we just need to go back to Commit Manager, click on
"Load/Save" Button and choose the rollback file that we want.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image21.png){width="6.268055555555556in" height="3.3048611111111112in"} -->

We can see the user that made the commit and what northbound interface
was used. In this case was the user admin via webui. We click on load,
and we can see that the change will be reverted.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image22.png){width="6.268055555555556in" height="2.7305555555555556in"} -->

After the commit we can check on the device that the change was
reverted.

<!-- Deep research:
This command verifies that a rollback operation in NSO has reverted the device configuration to its previous state. Rollbacks are essential for operational safety and compliance.
-->
**Example: Show run after rollback**
```bash
RP/0/RP0/CPU0:xr-1#show run interface gigabitEthernet 0/0/0/0
Tue Sep 30 03:13:12.733 UTC
! Configure left data port
interface GigabitEthernet0/0/0/0
ipv4 address 10.1.1.3 255.255.255.0
!
RP/0/RP0/CPU0:xr-1#
```

# 1.5 - Use NSO capabilities to detect Out-of-Band device configurations

So, now let's see one of the main NSO capabilities (check-sync,
sync-from, sync-to)

Login into xr-1 device using SSH and configure the Loopback 100
interface.

<!-- Deep research:
This sequence shows how to make an out-of-band change directly on the device (not via NSO). NSO can detect such changes and help reconcile configuration drift using sync-from/sync-to.
-->
**Example: Configure Loopback 100 out-of-band**
```bash
RP/0/RP0/CPU0:xr-1#configure terminal
Tue Sep 30 03:15:28.503 UTC
RP/0/RP0/CPU0:xr-1(config)#interface Loopback 100
RP/0/RP0/CPU0:xr-1(config-if)#description out-of-band change
RP/0/RP0/CPU0:xr-1(config-if)#commit
Tue Sep 30 03:15:40.348 UTC
RP/0/RP0/CPU0:xr-1(config-if)#end
RP/0/RP0/CPU0:xr-1#show run interface Loopback 100
Tue Sep 30 03:15:44.240 UTC
interface Loopback100
description out-of-band change
!
RP/0/RP0/CPU0:xr-1#
```

Since this change was done out of NSO we can go to NSO WEB UI / CLI and
see if the device is in sync.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image24.png){width="6.268055555555556in" height="2.9868055555555557in"} -->

We got a red alert because this change was done out of NSO. If we click
on Compare-config we can see exactly what was done out of NSO.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image25.png){width="6.268055555555556in" height="3.1756944444444444in"} -->

And now we've 2 options

[Option 1]{.underline}: We "SYNC-FROM" the device

[Option 2]{.underline}: We "SYNC-TO" device

If we go with Option 1, we will grab the configuration that was done in
the device without using NSO and push to NSO. If we use the Option 2, we
will push to the device the configuration that NSO had previously about
the device.

In this case we want to delete that configuration that was done without
NSO so we will "sync-to"

<!-- Deep research:
This NSO CLI command pushes the NSO configuration to the device, overwriting any out-of-band changes. 'sync-to' is used to enforce NSO's intended state on the device.
-->
**Example: Sync-to configuration**
```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ncs cli
admin@ncs# devices device xr-1 sync-to
```

We can now click on Check-Sync and confirm that both device and NSO
device configuration are the same

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image27.png){width="6.268055555555556in" height="2.9972222222222222in"} -->

If we login to the device, we will confirm that the VTP configuration is
not there anymore

<!-- Deep research:
This command confirms that the out-of-band configuration has been removed from the device after a sync-to operation. NSO ensures device compliance with its managed state.
-->
**Example: Show run after sync-to**
```bash
RP/0/RP0/CPU0:xr-1#show run interface Loopback 100
Tue Sep 30 03:19:34.654 UTC
% No such configuration item(s)
RP/0/RP0/CPU0:xr-1#
```

# 1.6 - Create Device Groups and Device Templates

Ok, so what if we want to apply configurations to several devices at the
same time? Let's do it. Let's create a device Group.

Go into Devices and select the "Device groups" tab.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image28.png){width="6.268055555555556in" height="2.5527777777777776in"} -->

Click "+ Add device group" button.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image29.png){width="6.268055555555556in" height="2.6680555555555556in"} -->

Add a name like "ios-xr-devices" and click Create.

Then select devices and press "+Add to device group".

<!-- ![Screens screenshot of a computer AI-generated content may be incorrect.](media/image30.png){width="6.268055555555556in" height="2.7597222222222224in"} -->

Click "Create device group" button. Following should be the result.

<!-- ![Screens screenshot of a computer AI-generated content may be incorrect.](media/image31.png){width="6.268055555555556in" height="2.763888888888889in"} -->

After creating a Device group, let's create a Device Template so we can
create a config that we will spread across all the devices in the group
with just few clicks.

Go to the "Configuration Editor" and choose the "ncs:devices" module and
click "Edit config" in the top menu.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image32.png){width="6.268055555555556in" height="2.14375in"} -->

In the template tile, click on the + Button

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image33.png){width="6.268055555555556in" height="0.81875in"} -->

Enter in the template and select the NED that we're using (cisco-iosxr)
and click Confirm.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image34.png){width="6.268055555555556in" height="2.4618055555555554in"} -->

Click on the NED to setup the configuration.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image35.png){width="6.268055555555556in" height="1.8895833333333334in"} -->

Now click on the config (inside the NED)

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image36.png){width="6.268055555555556in" height="1.1145833333333333in"} -->

And now you just need to make the configurations that you want to apply.
Let's create a DNS configuration. Look for Domain.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image37.png){width="6.268055555555556in" height="1.7770833333333333in"} -->

Add the Domain name and name-server.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image38.png){width="6.268055555555556in" height="1.9951388888888888in"} -->

Now, we go to the commit manager and let's see the differences.

From the config diff we can see that we've created a device-group and
the configuration template that we've created.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image39.png){width="6.268055555555556in" height="1.5763888888888888in"} -->

After commit, we can now go to the Device Group and Apply the Template.

Go to the Configuration Editor, "ncs:devices" module and click on the
recently created device-group "ios-xr-devices" the click "Actions" in
the top menu.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image40.png){width="6.268055555555556in" height="1.5916666666666666in"} -->
<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image41.png){width="6.268055555555556in" height="1.2673611111111112in"} -->

Then we click on "Apply-Template" button

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image42.png){width="6.268055555555556in" height="2.91875in"} -->

And now, select the "template-name" that we've created (you might need
to scroll up).

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image43.png){width="6.268055555555556in" height="2.047222222222222in"} -->

Scroll down and click on "Run apply-template action".

We can observe the apply-template result "ok" for each device present in
the device group.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image44.png){width="6.268055555555556in" height="1.24375in"} -->

If we go inside the Device CLI we can see that the configurations were
applied.

<!-- Deep research:
This SSH command verifies that a device template applied via NSO has correctly configured DNS settings on all devices in the group. Device templates are used for bulk configuration.
-->
**Example: Show run domain after applying template**
```bash
cisco@xrd-host:~$ ssh admin@172.30.0.2
(admin@172.30.0.2) Password: (password=cisco123)
Last login: Tue Sep 30 09:31:58 2025 from 172.30.0.1
RP/0/RP0/CPU0:xr-1#show run domain
Tue Sep 30 09:36:10.562 UTC
domain name-server 2.2.2.2
RP/0/RP0/CPU0:xr-1#
```

# 

\#### NTP configuration is not supported on XRd. Need a different
scenario! \####

# 1.7 - Create a Simple NSO Service

Templates are very flexible and easy to create. But, they lack of
consistency. If someone goes thought NSO and change one configuration
that you've done via template, you don't have an easy way to see it and
rollback. For that, you have the NSO Services.

An NSO service will allow you to see if the configuration that you
applied is still present on the devices. And if someone changed, we
could compare-configs and re-deploy if necessary.

First step is to create a service.

NSO already brings a script that creates a service skeleton.

Use the command "ncs-make-package -h"

RP/0/RP0/CPU0:xr-1#exit

Connection to 172.30.0.2 closed.

cisco@xrd-host:\~\$ ncs-make-package -h

Usage: ncs-make-package \[options\] package-name

ncs-make-package \--netconf-ned DIR package-name

ncs-make-package \--lsa-netconf-ned DIR package-name

ncs-make-package \--generic-ned-skeleton package-name

ncs-make-package \--snmp-ned DIR package-name

ncs-make-package \--service-skeleton TYPE package-name

ncs-make-package \--data-provider-skeleton package-name

ncs-make-package \--erlang-skeleton package-name

ncs-make-package \--nano-service-skeleton TYPE package-name

where TYPE is one of:

java Java based service

java-and-template Java service with template

python Python based service

python-and-template Python service with template

template Template service (no code)

\<...output omitted for brevity...\>

You can see that we've many options here.

Python and Java options are when we want to apply templates/services,
based on logic. Imagine you want to apply QOS data to a device interface
but only based on the interface high utilization. You can push that data
via external APIs and only send the device configs according to what you
receive.

Let's choose the python-and-template option. (Use it inside the packages
folder, otherwise you will have to copy the folder there after)

cisco@xrd-host:\~\$ cd NSO-INSTALL/nso-instance/packages/

cisco@xrd-host:\~/NSO-INSTALL/nso-instance/packages\$\
ncs-make-package \--service-skeleton python-and-template NTP

Now, let's open the VS Code to build our script.

Choose the NSO folder and click Ok. Expand the explorer until the
packages folder under nso-instance.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image45.png){width="6.268055555555556in" height="3.6180555555555554in"} -->

<!-- ![](media/image46.png){width="1.7590277777777779in" height="2.6381944444444443in"} --> After you run the script, you will find
this structure

package-meta-data.xml is where you will find the service version and
other useful information

<!-- ![A screen shot of a computer program AI-generated content may be incorrect.](media/image47.png){width="4.121298118985127in" height="2.101862423447069in"} -->

Then you have the XML file and the YANG file.

This is what we will find in the XML file

<!-- ![A screenshot of a computer program AI-generated content may be incorrect.](media/image48.png){width="5.788156167979003in" height="3.6663987314085738in"} -->

And this is what we will find on the YANG file:

<!-- ![A screenshot of a computer program AI-generated content may be incorrect.](media/image49.png){width="4.735600393700787in" height="5.318213035870516in"} -->

Our next step will be:

Go to NSO and make the configurations that we want to automate via the
service.

After all the configs are done instead of doing the commit, we will ask
for the output of the configurations to be sent in XML.

<!-- Deep research:
This XML output is generated by NSO's CLI dry-run feature. It shows the configuration that NSO intends to push to the device, in XML format, which is useful for service development and debugging.
-->
**Example: NCS CLI dry-run output**
```xml
<devices xmlns="http://tail-f.com/ns/ncs">
  <device>
    <name>xr-1</name>
    <config>
      <ntp xmlns="http://tail-f.com/ned/cisco-ios-xr">
        <server>
          <server-list>
            <name>172.16.22.44</name>
            <minpoll>8</minpoll>
            <maxpoll>12</maxpoll>
          </server-list>
        </server>
      </ntp>
    </config>
  </device>
</devices>
```

Now, we grab the XML that we got from the NSO cli config and we paste
into the skeleton XML file that was generated.

We only grab what is inside the \<config\>\</config\> flags.

\<ntp xmlns=\"http://tail-f.com/ned/cisco-ios-xr\"\>

\<server\>

\<server-list\>

\<name\>172.16.22.44\</name\>

\<minpoll\>8\</minpoll\>

\<maxpoll\>12\</maxpoll\>

\</server-list\>

\</server\>

\</ntp\>

This should be the result.

<!-- ![A screenshot of a computer program AI-generated content may be incorrect.](media/image50.png){width="4.611111111111111in" height="4.125in"} -->

If we want to apply this static config. We just need to compile the
service and we're good to go.

But, let's see how we define variables.

In this list I see 3 inputs:

Server IP-address

Minpool

Maxpool

After defining the variables our XML should look like this

<!-- ![A screenshot of a computer program AI-generated content may be incorrect.](media/image51.png){width="4.611111111111111in" height="4.138888888888889in"} -->

Going to the YANG after setting up the definition for each variable they
should look like this.

<!-- ![A screenshot of a computer program AI-generated content may be incorrect.](media/image52.png){width="4.569444444444445in" height="5.138888888888889in"} -->

After all of this is done, we go to the packages folder. Change
Directory to NTP/src

And apply the "make" command.

admin@ncs(config-ntp)# end

Uncommitted changes found, commit them? \[yes/no/CANCEL\] no

admin@ncs# exit

cisco@xrd-host:\~/NSO-INSTALL/nso-instance/packages\$ cd NTP/src/

cisco@xrd-host:\~/NSO-INSTALL/nso-instance/packages/NTP/src\$ make

mkdir -p ../load-dir

/home/cisco/NSO-INSTALL/bin/ncsc \`ls NTP-ann.yang \> /dev/null 2\>&1 &&
echo \"-a NTP-ann.yang\"\` \\

\--fail-on-warnings \\

\\

-c -o ../load-dir/NTP.fxs yang/NTP.yang

cisco@xrd-host:\~/NSO-INSTALL/nso-instance/packages/NTP/src\$

Then, we should go to NSO, and ask for a package reload. Make sure the
result is true.

<!-- Deep research:
This sequence builds the NSO service package using 'make', which compiles the YANG model and service logic into a loadable NSO package. This step is required before deploying new services.
-->
**Example: Reload packages**
```bash
admin@ncs# packages reload
>>> System upgrade is starting.
>>> Sessions in configure mode must exit to operational mode.
>>> No configuration changes can be performed until upgrade has completed.
>>> System upgrade has completed successfully.
reload-result {
  package NTP
  result true
}
reload-result {
  package cisco-iosxr-cli-7.69
  result true
}
admin@ncs#
```

After the reload you will see the NTP service package that we've just created.

<!-- ![A white rectangular object with a white background Description automatically generated](media/image54.png){width="6.268055555555556in" height="1.417361111111111in"} -->

Going back to the Homepage we click on the "Services"

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image55.png){width="6.268055555555556in" height="1.8486111111111112in"} -->

We select the NTP service that we've created. Click on "+ Add service"
then again click + button.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image56.png){width="6.268055555555556in" height="0.9715277777777778in"} -->

We should give a service name (help us identify the configs applied) and
setup the mandatory arguments, in our case, the server-address

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image57.png){width="6.268055555555556in" height="2.329861111111111in"} -->

Service Created! Let's add the devices that we want to affect with the
service.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image58.png){width="6.268055555555556in" height="1.5819444444444444in"} -->

Add the devices you want by clicking on Edit Config and on section
device select it

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image59.png){width="6.268055555555556in" height="2.3208333333333333in"} -->

We can keep the minpoll and maxpoll with the default values. And, now,
if we go to the commit manager we will see the configs applied to all
the devices that we've selected.

<!-- ![A screenshot of a computer AI-generated content may be incorrect.](media/image60.png){width="6.268055555555556in" height="2.984027777777778in"} -->

If we click on native config, we can even see the native commands that
NSO will send to the devices.

<!-- ![A close-up of a computer screen AI-generated content may be incorrect.](media/image61.png){width="6.268055555555556in" height="1.604861111111111in"} -->

After commit, we can now see if the service is in Sync! <!-- ![A screenshot of a computer Description automatically generated](media/image62.png){width="6.268055555555556in" height="1.132638888888889in"} -->

Now, to see the how powerful the services are let's go inside 2 devices,
ios-xr-2 and ios-xr-3 and delete the NTP config.

Go to the Device Manager, click on IOS-XR-2 and delete the ntp server
configs by clicking on Edit Config, selecting them and clicking on the
"-" button. Then repeat for ios-xr-3.

<!-- ![](media/image63.png){width="6.268055555555556in" height="1.5486111111111112in"} -->
<!-- ![A screenshot of a computer Description automatically generated](media/image64.png){width="6.268055555555556in" height="1.28125in"} -->

Go to the commit manager and press "Commit" <!-- ![A screenshot of a computer Description automatically generated](media/image65.png){width="6.268055555555556in" height="2.428472222222222in"} -->

Now, if we go back to the "Service Manager" and we click on "Check-Sync"
we will get a RED signal. <!-- ![A screenshot of a computer Description automatically generated](media/image66.png){width="6.268055555555556in" height="1.1236111111111111in"} -->

<!-- ![A screenshot of a computer error Description automatically generated](media/image67.png){width="6.268055555555556in" height="1.6340277777777779in"} -->

By clicking on Re-deploy Dry-Run <!-- ![A screenshot of a computer Description automatically generated](media/image68.png){width="6.268055555555556in" height="2.425in"} -->

We will see exactly what needs to be added to the devices for them to be
compliant with the service. And to configure the devices again we just
need to click on re-deploy.

<!-- ![Graphical user interface, text, application Description automatically generated](media/image69.png){width="6.268055555555556in" height="1.538888888888889in"} -->

And, the service was re-deployed. If we connect to ios-xr-2 and make a
show-run we can see that the configs are there again.

<!-- Deep research:
This command verifies that the NSO service has re-deployed the intended configuration to the device after a compliance check. NSO services provide automated, consistent configuration management.
-->
**Example: Show run after service re-deploy**
```bash
ios-xr-2# show run
admin
exit-admin-config
!
domain name cisco.com
domain name-server 2.2.2.2
ntp
peer 192.168.22.33
server 172.16.22.44 minpoll 8 maxpoll 12
exit
vtp mode off
```

# 1.8 - Create Role Based and Resource Based Access Control rules

In this section the idea is to show you how we can create Role Based and
Resource Based Access Control rules in NSO.

The first step is to add a new user.

In the Configuration Editor we navigate to aaa:aaa module <!-- ![A close-up of a computer screen Description automatically generated](media/image70.png){width="6.268055555555556in" height="2.2930555555555556in"} -->

Then we go to the authentication/users then we click on Edit Config and
the "+" button

<!-- ![Graphical user interface, application Description automatically generated](media/image71.png){width="6.268055555555556in" height="1.742361111111111in"} -->

Let's create the user "read_user"

<!-- ![Graphical user interface, application Description automatically generated](media/image72.png){width="6.268055555555556in" height="3.0680555555555555in"} -->

After this we can "Commit" <!-- ![A screenshot of a computer Description automatically generated](media/image73.png){width="6.268055555555556in" height="1.8118055555555554in"} -->

And login to the new recently created user

<!-- ![Graphical user interface, website Description automatically generated](media/image74.png){width="5.566741032370953in" height="3.3285717410323707in"} -->

We can now go to the Device Manager and check if this user is able to do
all the normal operations <!-- ![A screenshot of a computer Description automatically generated](media/image75.png){width="6.268055555555556in" height="2.178472222222222in"} -->

And, as we can see Ping is not possible because the new user does not
belong to any authgroup.

The Authgroup is the feature that allows mapping the NSO local user to
the device local user. Each Device much belong to an authgroup, and each
user much be present in one authgroup as well ( or, have a
default-mapping )

To create a new Authgoup or associate the user with an existing
authgroup we go to the Configuration Editor, to the module ncs:devices

<!-- ![A screenshot of a computer Description automatically generated](media/image76.png){width="6.268055555555556in" height="2.464583333333333in"} -->

Then we select authgroup "default" <!-- ![A screenshot of a computer Description automatically generated](media/image77.png){width="6.268055555555556in" height="2.254861111111111in"} -->

At this moment we can make these operations on the "read_user" since we
still don't have not made any change to permissions.

<!-- ![A screenshot of a computer Description automatically generated](media/image78.png){width="6.268055555555556in" height="2.8881944444444443in"} -->

Now, inside the "default" authgroup we've the umap and default-map.

The umap, is a mapping for a local user that belongs to this Authgroup.

And, we've a default-map, this one is a default mapping for each user
that belongs to this group.

Let's add our read_user here <!-- ![A screenshot of a computer Description automatically generated](media/image79.png){width="6.268055555555556in" height="3.933333333333333in"} -->

Now, you can put the remote authentication for the devices that will
belong have this authgroup

<!-- ![A screenshot of a computer Description automatically generated](media/image80.png){width="6.268055555555556in" height="3.6020833333333333in"} -->

Now, you can choose what will be the credentials that this NSO user will
use when connecting to the devices. ( we can use admin-admin for the
demo proposes ).

Go to the commit manager and commit.

Going now back to the "Device Manager" we can now check the Connect

<!-- ![Graphical user interface, application Description automatically generated](media/image81.png){width="6.268055555555556in" height="1.3263888888888888in"} -->

For now, this user can make all the normal operations

Let's start making some restrictions.

Go back to the admin user.

Then, go to the configuration Editor.

And then to the module nacm:nacm <!-- ![A screenshot of a computer Description automatically generated](media/image82.png){width="6.268055555555556in" height="3.5395833333333333in"} -->

In the NACM module you will see the "rule-list" and the "groups"

So, to set permissions we need to associate a user to a group, and then
associate that group to a rule-list.

<!-- ![A screenshot of a computer Description automatically generated](media/image83.png){width="6.581221566054243in" height="2.901978346456693in"} -->

Let's create the "read_group" <!-- ![A screenshot of a computer Description automatically generated](media/image84.png){width="6.268055555555556in" height="2.8020833333333335in"} -->

Then we setup one group-id ( 1 for example ) and we add the user
"read_user" to that group

<!-- ![A screenshot of a computer Description automatically generated](media/image85.png){width="5.7467027559055115in" height="2.4735148731408576in"} -->

After this, we can go back and create the rule-list "read_rule" <!-- ![A screenshot of a computer Description automatically generated](media/image86.png){width="5.631265310586176in" height="2.9429068241469816in"} -->

On this rule, we will add the read_group in groups. This means that all
the rules that we will setup here, will affect only the users that
belong to this group.

<!-- ![A screenshot of a computer Description automatically generated](media/image87.png){width="6.268055555555556in" height="3.4166666666666665in"} -->

So, now we've 2 options, the cmdrule where list entries are examined for
command authorization, and the rule where entries are examined for rpc,
notification, and data authorization.

Let's add a rule to deny the sync-from <!-- ![A screenshot of a computer Description automatically generated](media/image88.png){width="6.268055555555556in" height="2.9375in"} -->

In the rule we've to specify a path <!-- ![A screenshot of a computer Description automatically generated](media/image89.png){width="6.268055555555556in" height="3.6104166666666666in"} -->

This path is nothing more than an XPATH. To see how we can get the
xpaths, we go back to CLI and we execute the command "show devices
device | display xpath"

ios-xr-2# exit

cisco@nso-613:\~/NSO-TEST/nso-instance/packages/NTP/src\$ ncs_cli -Cu
admin

User admin last logged in 2022-10-26T10:32:10.740451+00:00, to nso-613,
from 198.18.133.252 using webui-http

admin connected from 198.18.133.252 using ssh on nso-613

admin@ncs# show devices device | display xpath

/devices/device\[name=\'ios-cli-0\'\]/commit-queue/queue-length 0

/devices/device\[name=\'ios-cli-0\'\]/active-settings/connect-timeout 20

/devices/device\[name=\'ios-cli-0\'\]/active-settings/read-timeout 20

/devices/device\[name=\'ios-cli-0\'\]/active-settings/write-timeout 20

/devices/device\[name=\'ios-cli-0\'\]/active-settings/connect-timeout 20

/devices/device\[name=\'ios-cli-0\'\]/active-settings/read-timeout 20

/devices/device\[name=\'ios-cli-0\'\]/active-settings/write-timeout 20

\<...output omitted for brevity...\>

This will show all devices path, if we want to see the paths for each
device

admin@ncs# config

admin@ncs(config)# show full-configuration devices device ios-xr-1 \|
display xpath

/devices/device\[name=\'ios-xr-1\'\]/address 127.0.0.1

/devices/device\[name=\'ios-xr-1\'\]/port 10023

\<...output omitted for brevity...\>

/devices/device\[name=\'ios-xr-1\'\]/authgroup default

/devices/device\[name=\'ios-xr-1\'\]/device-type/cli/ned-id
cisco-iosxr-cli-7.38

/devices/device\[name=\'ios-xr-1\'\]/state/admin-state unlocked

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:domain/name
cisco.com

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:domain/name-server\[address=\'2.2.2.2\'\]

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:ntp/peer\[address=\'192.168.22.33\'\]

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:ntp/server/server-list\[name=\'172.16.22.44\'\]/minpoll
8

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:ntp/server/server-list\[name=\'172.16.22.44\'\]/maxpoll
12

/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:vtp/mode off

For example, we can grab the path
"/devices/device\[name=\'ios-xr-1\'\]/config/cisco-ios-xr:domain" and
block all the operations with the exception of reading.

We create the rule "deny-config-domain" <!-- ![A screenshot of a computer Description automatically generated](media/image90.png){width="6.268055555555556in" height="2.204861111111111in"} -->

And inside the rule on the data-node we use the path that we just got,
we setup the action to deny and access operations we will deny
"create,update,delete,exec"

<!-- ![A screenshot of a computer Description automatically generated](media/image91.png){width="6.065057961504812in" height="3.472658573928259in"} -->

To see all the access-operations you can toggle the button near "view
options" on the top right and then the info button will appear in each
tile.

<!-- ![Graphical user interface, text, application, email Description automatically generated](media/image92.png){width="6.268055555555556in" height="2.6881944444444446in"} -->

After this, we commit.

We logout from the admin user and we login into the read_user.

Now we go to the Device Manager and we test our rules. We can check the
"ping", "connect", "check-sync" and "sync-from" and we will see that the
"sync-from" got denied. <!-- ![A screenshot of a computer Description automatically generated](media/image93.png){width="6.268055555555556in" height="2.4229166666666666in"} -->

Now, if we go to the device ios-xr-1 and we go into config/domain we
will see that we're only able to read the info

<!-- ![A screenshot of a computer Description automatically generated](media/image94.png){width="6.268055555555556in" height="2.6534722222222222in"} -->

And, if we try to add anything by clicking on the + button we will get
the "access denied" notification.

<!-- ![Graphical user interface Description automatically generated](media/image95.png){width="6.107549212598425in" height="2.368993875765529in"} -->

Since we've made the rule just for the ios-xr-1 device, if we go to the
other devices, for example ios-xr-0 we see that we can still make
changes in the domain configuration. <!-- ![A screenshot of a computer Description automatically generated](media/image96.png){width="5.946339676290464in" height="2.532432195975503in"} -->

To correct this, we need to login into the admin user, and change the
PATH of this rule.

So, instead of
"/devices/device[name='ios-xr-1']/config/cisco-ios-xr:domain/"

We will change to "/devices/device/config/cisco-ios-xr:domain/"

Commit and go back to the read_user.

We can see that after this change, we're not able to make any change in
domain configuration of all the managed devices with this user. <!-- ![A screenshot of a computer Description automatically generated](media/image97.png){width="6.268055555555556in" height="2.80625in"} -->
