# Day 1 instructions
## 1.0 Connect to the Workstation

Please click on the "**Win11**" option, and then select "**VM Console**" from the Remote Access.
<!-- Add picture -->
![](<media/Pasted%20image%2020251001204206.png>)


After selecting "**VM Console**", you will be redirected to the Windows operating system interface. If you see “You’re almost done setting up your PC,” then click “Remind me in 3 days.”

<!-- Add picture -->
![](<media/Pasted%20image%2020251001204214.png>)

Alternatively, you can choose an Ubuntu Desktop. Please click on the "`Ubuntu 24.04 Desktop`" and then select "**VM Console**" from the Remote Access.

![](<media/Pasted%20image%2020251001204223.png>)

## 1.1 Install NSO and NEDs
- Click on Visual Studio Code shortcut and work on the Terminal window.
<!-- Add picture -->
![](<media/Pasted%20image%2020251001204457.png>)
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

!!! Note
	To extract the installer, first **create a `work` directory**. Then, run `bash ../nso-6.5-freetrial.linux.x86_64.signed.bin --skip-verification`. The `--skip-verification` flag is used to bypass the certification check.

```bash
cisco@xrd-host:~/NSO-6.5-free$ mkdir work
cisco@xrd-host:~/NSO-6.5-free$ cd work
cisco@xrd-host:~/NSO-6.5-free/work$
bash ../nso-6.5-freetrial.linux.x86_64.signed.bin --skip-verification
Unpacking...
cisco@xrd-host:~/NSO-6.5-free/work$
```

Execute the nso-6.5.linux.x86_64.installer.bin with the “—local-install” flag and choose the installation directory.

Alternatively, you can choose an Ubuntu Desktop. Please click on the "Ubuntu 24.04 Desktop" and then select "VM Console" from the Remote Access.

## 1.1 - Install NSO and NEDs

- Click on Visual Studio Code shortcut and work on the Terminal window.
<!-- Add picture -->

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

!!! Note
	First, you need to extract installer `“.bin”` file. To avoid messing up the directory, create a new directory named “work” and execute `“bash ../nso-6.5-freetrial.linux.x86_64.signed.bin”`. Using `“--skip-verification”` skips certification check.

```bash
cisco@xrd-host:~/NSO-6.5-free$ mkdir work
cisco@xrd-host:~/NSO-6.5-free$ cd work
cisco@xrd-host:~/NSO-6.5-free/work$
bash ../nso-6.5-freetrial.linux.x86_64.signed.bin --skip-verification
Unpacking...
cisco@xrd-host:~/NSO-6.5-free/work$
```

Execute the **nso-6.5.linux.x86_64.installer.bin** with the “—local-install” flag and choose the installation directory.

```bash
cisco@xrd-host:~/NSO-6.5-free/work$
bash nso-6.5.linux.x86_64.installer.bin --local-install ~/NSO-INSTALL
INFO  Using temporary directory /tmp/ncs_installer.780985 to stage NCS installation bundle
INFO  Unpacked ncs-6.5 in /home/cisco/NSO-INSTALL
INFO  Found and unpacked corresponding DOCUMENTATION_PACKAGE
INFO  Found and unpacked corresponding EXAMPLE_PACKAGE
INFO  Found and unpacked corresponding JAVA_PACKAGE
INFO  Generating default SSH hostkey (this may take some time)
INFO  SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ecdsa_key hostkey generated
INFO  SSH /home/cisco/NSO-INSTALL/etc/ncs/ssh/ssh_host_ed25519_key hostkey generated
INFO  Generating self-signed certificates for HTTPS
INFO  Environment set-up generated in /home/cisco/NSO-INSTALL/ncsrc
INFO  NSO installation script finished
INFO  Found and unpacked corresponding NETSIM_PACKAGE
INFO  NCS installation complete
```

Go to the installation folder and source NSO
```bash
cisco@xrd-host:~/NSO-6.5-free/work$ cd ~/NSO-INSTALL/
cisco@xrd-host:~/NSO-INSTALL$ source ncsrc
```

  
Execute the NSO Setup script
```bash
cisco@xrd-host:~/NSO-INSTALL$ ncs-setup --dest nso-instance
```

Change directory to the “nso-instance” folder and find “ncs.conf”.

```bash
cisco@xrd-host:~/NSO-INSTALL$ cd nso-instance/
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ls
README.ncs  logs  ncs-cdb  ncs.conf  packages  scripts  state
```

!!! Note
	From NSO 6.5, WebUI and RESTCONF are allowed only to NSO server’s hostname (such as “*localhost*”). To allow an access via IP address, following line has to be added to ncs.conf. Add `<server-alias>198.18.134.27</server-alias>` to line 330 and save the file.


Once added, start NSO by executing `ncs` command.
```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ncs
```

Check if NSO is running

```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ ncs --status | grep started
```

status: **started**

  
To log in to NSO via the Web UI, open Firefox and access following URL: [http://198.18.134.27:8080/login.html](http://198.18.134.27:8080/login.html)

You can log in using the default username and password, which are "admin" for both the username and password fields.
![](<media/Pasted%20image%2020251001204550.png>)

After logging in, you will be directed to the main page, which displays the available applications and components provided by NSO.

![](<media/Pasted%20image%2020251001204557.png>)

Please navigate to the "Config Editor" section.

![](<media/Pasted%20image%2020251001204604.png>)

Since this is a fresh installation and we do not have any packages yet, let's proceed by adding the Network Element Drivers (NEDs).

In the *"~/NSO-6.5-free"* folder, you should find some file starting with `ncs-` and they are NED packages.

```bash
cisco@xrd-host:~/NSO-INSTALL/nso-instance$ cd ~/NSO-6.5-free/
cisco@xrd-host:~/NSO-6.5-free$ ls ncs*
ncs-6.5-cisco-asa-6.18.23-freetrial.signed.bin  ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin
ncs-6.5-cisco-ios-6.109.4-freetrial.signed.bin  ncs-6.5-cisco-nx-5.27.3-freetrial.signed.bin
cisco@xrd-host:~/NSO-6.5-free$
```

!!! Note
	You also need to extract NED files. To avoid messing up the directory, go to the work folder and execute `bash ../<ned.signed.bin>` commands. Using `--skip-verification` skips certification check.

```bash
cisco@xrd-host:~/NSO-6.5-free$ cd work/
cisco@xrd-host:~/NSO-6.5-free/work$
bash ../ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin --skip-verification
Unpacking...
cisco@xrd-host:~/NSO-6.5-free/work$
```

!!! Note
	“`ncs-6.5-cisco-iosxr-7.69.tar.gz`” file is the NED package. Copy the file to *nso-instance/packages* folder.

```bash
cisco@xrd-host:~/NSO-6.5-free/work$
cp ncs-6.5-cisco-iosxr-7.69.tar.gz ~/NSO-INSTALL/nso-instance/packages/
```

Please return to the NSO Web UI, go to `ncs:packages` and select “Actions”, then click the "Reload" button for the packages.
![](<media/Pasted%20image%2020251001204649.png>)
After executing the "Run reload action" button, you should now be able to see the "`cisco-ios-xr`" package in the list.
![](<media/Pasted%20image%2020251001204700.png>)

Now that we have added our Network Element Driver (NED), let's navigate to the "**Devices**" section.
![](<media/Pasted%20image%2020251001204721.png>)

Currently, there are no devices listed. However, we have two options to add them: manually by clicking on the **"+"** button, or programmatically using scripts or APIs.

## 1.2 – Registering XRd routers

There are two XRd routers running on the server which you have installed NSO. Please confirm they are running by executing `docekr ps`.

```bash
cisco@xrd-host:~/NSO-6.5-free/work$ docker ps
CONTAINER ID   IMAGE                             COMMAND                  CREATED       STATUS      PORTS     NAMES
8502937299d3   ios-xr/xrd-control-plane:25.2.1   "/usr/local/sbin/init"   5 weeks ago   Up 4 days             xr-2
2d77e0ef76f7   alpine:3.15                       "/bin/sh -c 'ip rout…"   5 weeks ago   Up 4 days             source
4a68f412191b   ios-xr/xrd-control-plane:25.2.1   "/usr/local/sbin/init"   5 weeks ago   Up 4 days             xr-1
593c076cbbe7   alpine:3.15                       "/bin/sh -c 'ip rout…"   5 weeks ago   Up 4 days             dest
cisco@xrd-host:~/NSO-6.5-free/work$
```

A device template has been prepared to load these devices. Go to NSO-6.5-free and load “devices.xml” by using the “ncs_load” command to load them. -l ( load ) -m ( merge )

```bash
cisco@xrd-host:~/NSO-6.5-free/work$ cd ~/NSO-6.5-free/
cisco@xrd-host:~/NSO-6.5-free$ ncs_load -l -m devices.xml
```

Go back to NSO and click on Devices and you see two XRd routers there.
![](<media/Pasted%20image%2020251001204735.png>)
The first thing you need to do after a device to NSO is execute the “`sync-from`” so you update NSO CDB with the device configuration. Select all devices and make a `sync-from`
![](<media/Pasted%20image%2020251001204745.png>)

![](<media/Pasted%20image%2020251001204755.png>)

!!! Note
	If `sync-from` failed, please do “`fetch ssh host keys`” and try again.

## 1.3 - Configure Devices using NSO

You can enter in ios-xr-0 to verify interface configurations. To navigate in the configurations, you can use the Top bar (where I typed “confi” ) or scroll down and look for what you want to configure.

<!-- Add picture -->
![](<media/Pasted%20image%2020251001204816.png>)

You can follow the path and verify the `GigabitEthernet0/0/0/0` configuration

**Full Path:** `/ncs:devices/device{xr-1}/config/cisco-ios-xr:interface/GigabitEthernet{0/0/0/0}/ipv4/address/`

<!-- Add picture -->

Let’s make a change in the IP address. To do it, you just need to click on Edit Config and then change the field IP. Please change address from “`10.1.1.3`” to “`10.1.1.30`”.

<!-- Add picture -->

You can notice there is a green bar on the left of the tile that you edited. This means there is a candidate configuration on-going, and this configuration will only be pushed to the device after you commit.

Let’s go the `Tools > Commit Manager` and see “`config`” tab.
<!-- Add picture -->
In the commit manager we have a diff view like on github. If we agree with the changes, we click on commit.
<!-- Add picture -->
After our commit is done, we can see a rollback is created. This is because, every time we make a commit in NSO we create a rollback file that allow us to revert what we did.
<!-- Add picture -->
If we go back to the router, we can see that our device already has the new IP on the Loopback Interface

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

## 1.4 - Use Rollback’s

To Rollback this we just need to go back to Commit Manager, click on “`Load/Save`” Button and choose the rollback file that we want.
<!-- Add picture -->
We can see the user that made the commit and what northbound interface was used. In this case was the user admin via webui. We click on load, and we can see that the change will be reverted.
<!-- Add picture -->
After the commit we can check on the device that the change was reverted.

```bash
RP/0/RP0/CPU0:xr-1#show run interface gigabitEthernet 0/0/0/0
Tue Sep 30 03:13:12.733 UTC
! Configure left data port
interface GigabitEthernet0/0/0/0
 ipv4 address 10.1.1.3 255.255.255.0
!
RP/0/RP0/CPU0:xr-1#
```

## 1.5 - Use NSO capabilities to detect Out-of-Band device configurations

So, now let’s see one of the main NSO capabilities (check-sync, sync-from, sync-to)

Login into **xr-1** device using SSH and configure the Loopback 100 interface.
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

Since this change was done out of NSO we can go to NSO WEB UI / CLI and see if the device is in sync.
<!-- Add picture -->
We got a red alert because this change was done out of NSO. If we click on Compare-config we can see exactly what was done out of NSO.
<!-- Add picture -->
And now we’ve 2 options

**Option 1:** We “`SYNC-FROM`” the device

**Option 2:** We “`SYNC-TO`” device

If we go with Option 1, we will grab the configuration that was done in the device without using NSO and push to NSO. If we use the Option 2, we will push to the device the configuration that NSO had previously about the device.

In this case we want to delete that configuration that was done without NSO so we will “`sync-to`”
<!-- Add picture -->
We can now click on Check-Sync and confirm that both device and NSO device configuration are the same
<!-- Add picture -->
If we login to the device, we will confirm that the VTP configuration is not there anymore

```bash
RP/0/RP0/CPU0:xr-1#show run interface Loopback 100
Tue Sep 30 03:19:34.654 UTC
% No such configuration item(s)

RP/0/RP0/CPU0:xr-1#
```

## 1.6 - Create Device Groups and Device Templates

Ok, so what if we want to apply configurations to several devices at the same time? Let’s do it. Let’s create a device Group.

Go into Devices and select the “Device groups” tab.
<!-- Add picture -->
Click “**+ Add device group**” button.
<!-- Add picture -->
Add a name like “*ios-xr-devices*” and click Create.
Then select devices and press “**+Add to device group**”.
<!-- Add picture -->
Click “**Create device group**” button. Following should be the result.
<!-- Add picture -->
After creating a Device group, let’s create a Device Template so we can create a config that we will spread across all the devices in the group with just few clicks.

Go to the “**Configuration Editor**” and choose the “`ncs:devices`” module and click “**Edit config**” in the top menu.
<!-- Add picture -->
In the template tile, click on the **+** Button
<!-- Add picture -->
Enter in the template and select the NED that we’re using (`cisco-iosxr`) and click **Confirm**.
<!-- Add picture -->
Click on the NED to setup the configuration.
<!-- Add picture -->
Now click on the config (inside the NED)
<!-- Add picture -->
And now you just need to make the configurations that you want to apply. Let’s create a DNS configuration. Look for Domain.
<!-- Add picture -->
Add the Domain name and name-server.
<!-- Add picture -->
Now, we go to the commit manager and let’s see the differences.

From the config diff we can see that we’ve created a device-group and the configuration template that we’ve created.
<!-- Add picture -->
After commit, we can now go to the Device Group and Apply the Template.

Go to the Configuration Editor, “`ncs:devices`” module and click on the recently created device-group “`ios-xr-devices`” the click “**Actions**” in the top menu.
<!-- Add picture -->
<!-- Add picture -->

Then we click on “Apply-Template” button
<!-- Add picture -->
And now, select the “template-name” that we’ve created (you might need to scroll up).
<!-- Add picture -->

Scroll down and click on “Run apply-template action”.

We can observe the apply-template result “ok” for each device present in the device group.
<!-- Add picture -->
If we go inside the Device CLI we can see that the configurations were applied.

```bash
cisco@xrd-host:~$ ssh admin@172.30.0.2
(admin@172.30.0.2) Password: (password=cisco123)
Last login: Tue Sep 30 09:31:58 2025 from 172.30.0.1

RP/0/RP0/CPU0:xr-1#show run domain
Tue Sep 30 09:36:10.562 UTC
domain name-server 2.2.2.2

RP/0/RP0/CPU0:xr-1#
```

!!! Note
	NTP configuration is not supported on XRd. Need a different scenario! 

## 1.7 - Create a Simple NSO Service

Templates are very flexible and easy to create. But, they lack of consistency. If someone goes thought NSO and change one configuration that you’ve done via template, you don’t have an easy way to see it and rollback. For that, you have the NSO Services.

An NSO service will allow you to see if the configuration that you applied is still present on the devices. And if someone changed, we could compare-configs and re-deploy if necessary.

First step is to create a service.

NSO already brings a script that creates a service skeleton.

Use the command “`ncs-make-package -h`”

```bash
RP/0/RP0/CPU0:xr-1#exit
Connection to 172.30.0.2 closed.
cisco@xrd-host:~$ ncs-make-package -h

Usage: ncs-make-package [options] package-name
  ncs-make-package --netconf-ned DIR package-name
  ncs-make-package --lsa-netconf-ned DIR package-name
  ncs-make-package --generic-ned-skeleton package-name
  ncs-make-package --snmp-ned DIR package-name
  ncs-make-package --service-skeleton TYPE package-name
  ncs-make-package --data-provider-skeleton package-name
  ncs-make-package --erlang-skeleton package-name
  ncs-make-package --nano-service-skeleton TYPE package-name

      where TYPE is one of:
          java                  Java based service
          java-and-template     Java service with template
          python                Python based service
          python-and-template   Python service with template
          template              Template service (no code)

  <…output omitted for brevity…>
```

You can see that we’ve many options here.

Python and Java options are when we want to apply templates/services, based on logic. Imagine you want to apply QOS data to a device interface but only based on the interface high utilization. You can push that data via external APIs and only send the device configs according to what you receive.

Let’s choose the **python-and-template** option. (Use it inside the packages folder, otherwise you will have to copy the folder there after)

```
cisco@xrd-host:~$ cd NSO-INSTALL/nso-instance/packages/

cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages$  
ncs-make-package --service-skeleton python-and-template NTP
```

  
Now, let’s open the VS Code to build our script.

  
Choose the NSO folder and click Ok. Expand the explorer until the packages folder under nso-instance.
<!-- Add picture -->
After you run the script, you will find this structure
<!-- Add picture -->

package-meta-data.xml is where you will find the service version and other useful information
<!-- Add picture -->
Then you have the XML file and the YANG file.

This is what we will find in the XML file
<!-- Add picture -->

And this is what we will find on the YANG file:

Our next step will be:

Go to NSO and make the configurations that we want to automate via the service.

After all the configs are done instead of doing the commit, we will ask for the output of the configurations to be sent in XML.

```
cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages$ ncs_cli -Cu admin

User admin last logged in 2025-09-30T09:49:14.561558+00:00, to xrd-host, from 10.16.68.225 using cli-ssh
admin connected from 10.16.68.225 using ssh on xrd-host
admin@ncs# config
Entering configuration mode terminal

Current configuration users:
admin http (webui from 198.18.133.100) on since 2025-09-30 09:07:42 terminal mode
admin@ncs(config)# devices device xr-1
admin@ncs(config-device-xr-1)# config
admin@ncs(config-config)# ntp
admin@ncs(config-ntp)# server 172.16.22.44 minpoll 8 maxpoll 12

admin@ncs(config-ntp)# commit dry-run outformat xml

result-xml {

    local-node {
        data <devices xmlns="http://tail-f.com/ns/ncs">
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
    }

}
```

Now, we grab the XML that we got from the NSO cli config and we paste into the skeleton XML file that was generated.

We only grab what is inside the <config></config> flags.

```xml
<ntp xmlns="http://tail-f.com/ned/cisco-ios-xr">
     <server>
         <server-list>
              <name>172.16.22.44</name>
              <minpoll>8</minpoll>
              <maxpoll>12</maxpoll>
        </server-list>
     </server>
</ntp>
```

This should be the result.
<!-- Add picture -->
If we want to apply this static config. We just need to compile the service and we’re good to go.

But, let’s see how we define variables.

In this list I see 3 inputs:
- Server IP-address
- Minpool
- Maxpool

After defining the variables our XML should look like this
<!-- Add picture -->
Going to the YANG after setting up the definition for each variable they should look like this.

So we will setup as mandatory the server-address and then the minpoll and maxpoll we setup as non-mandatory and with default values.
<!-- Add picture -->
After all of this is done, we go to the packages folder. Change Directory to NTP/src

And apply the “**make**” command.

```
admin@ncs(config-ntp)# end
Uncommitted changes found, commit them? [yes/no/CANCEL] no
admin@ncs# exit
cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages$ cd NTP/src/
cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages/NTP/src$ make
mkdir -p ../load-dir
/home/cisco/NSO-INSTALL/bin/ncsc  `ls NTP-ann.yang  > /dev/null 2>&1 && echo "-a NTP-ann.yang"` \
     --fail-on-warnings \
      \
     -c -o ../load-dir/NTP.fxs yang/NTP.yang
```
cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages/NTP/src$

Then, we should go to NSO, and ask for a package reload. Make sure the result is true.

```
cisco@xrd-host:~/NSO-INSTALL/nso-instance/packages/NTP/src$  
ncs_cli -Cu admin
User admin last logged in 2025-09-30T09:58:35.496282+00:00, to xrd-host, from 10.16.68.225 using cli-ssh
admin connected from 10.16.68.225 using ssh on xrd-host
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

After the reload you will see the NTP service package that we’ve just created.
<!-- Add picture -->
Going back to the Homepage we click on the “Services”
<!-- Add picture -->
We select the NTP service that we’ve created. Click on “+ Add service” then again click + button.
<!-- Add picture -->
We should give a service name (help us identify the configs applied) and setup the mandatory arguments, in our case, the server-address
<!-- Add picture -->
Service Created! Let’s add the devices that we want to affect with the service.
<!-- Add picture -->
Add the devices you want by clicking on Edit Config and on section device select it
<!-- Add picture -->
We can keep the minpoll and maxpoll with the default values. And, now, if we go to the commit manager we will see the configs applied to all the devices that we’ve selected.
<!-- Add picture -->
If we click on native config, we can even see the native commands that NSO will send to the devices.
<!-- Add picture -->
After commit, we can now see if the service is in Sync
<!-- Add picture -->
Now, to see the how powerful the services are let’s go inside 2 devices, ios-xr-2 and ios-xr-3 and delete the NTP config.

Go to the Device Manager, click on IOS-XR-2 and delete the ntp server configs by clicking on Edit Config, selecting them and clicking on the “-“ button. Then repeat for ios-xr-3.

<!-- Add picture -->
<!-- Add picture -->
Go to the commit manager and press “**Commit**”
<!-- Add picture -->
Now, if we go back to the “Service Manager” and we click on “Check-Sync” we will get a RED signal.
<!-- Add picture -->
<!-- Add picture -->
By clicking on Re-deploy Dry-Run

We will see exactly what needs to be added to the devices for them to be compliant with the service. And to configure the devices again we just need to click on re-deploy.

And, the service was re-deployed. If we connect to ios-xr-2 and make a show-run we can see that the configs are there again.

```
cisco@nso-613:~/NSO-TEST/nso-instance/packages/NTP/src$
ncs-netsim cli-c ios-xr-2
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

## 1.8 - Create Role Based and Resource Based Access Control rules

In this section the idea is to show you how we can create Role Based and Resource Based Access Control rules in NSO.

The first step is to add a new user.

In the Configuration Editor we navigate to `aaa:aaa` module
<!-- Add picture -->
Then we go to the authentication/users then we click on Edit Config and the “**+**” button
<!-- Add picture -->
Let’s create the user “**read_user**”
<!-- Add picture -->
After this we can “**Commit**”
<!-- Add picture -->
And login to the new recently created user
<!-- Add picture -->
We can now go to the Device Manager and check if this user is able to do all the normal operations
<!-- Add picture -->
And, as we can see Ping is not possible because the new user does not belong to any authgroup.

The Authgroup is the feature that allows mapping the NSO local user to the device local user. Each Device much belong to an authgroup, and each user much be present in one authgroup as well ( or, have a default-mapping )

To create a new Authgoup or associate the user with an existing authgroup we go to the Configuration Editor, to the module ncs:devices
<!-- Add picture -->
Then we select **authgroup** “`default`”
<!-- Add picture -->
At this moment we can make these operations on the “**read_user**” since we still don’t have not made any change to permissions.
<!-- Add picture -->
Now, inside the “default” authgroup we’ve the umap and default-map.

The umap, is a mapping for a local user that belongs to this Authgroup.

And, we’ve a default-map, this one is a default mapping for each user that belongs to this group.

Let’s add our read_user here
<!-- Add picture -->
Now, you can put the remote authentication for the devices that will belong have this authgroup
<!-- Add picture -->
Now, you can choose what will be the credentials that this NSO user will use when connecting to the devices. ( we can use admin-admin for the demo proposes ).

Go to the commit manager and commit.

Going now back to the “Device Manager” we can now check the Connect
<!-- Add picture -->
For now, this user can make all the normal operations

Let’s start making some restrictions.

Go back to the admin user.

Then, go to the configuration Editor.

And then to the module nacm:nacm
<!-- Add picture -->
In the NACM module you will see the “**rule-list**” and the “**groups**”

So, to set permissions we need to associate a user to a group, and then associate that group to a rule-list.
<!-- Add picture -->
Let’s create the “**read_group**”
<!-- Add picture -->
Then we setup one group-id ( 1 for example ) and we add the user “**read_user**” to that group
<!-- Add picture -->
After this, we can go back and create the rule-list “**read_rule**”
<!-- Add picture -->
On this rule, we will add the read_group in groups. This means that all the rules that we will setup here, will affect only the users that belong to this group.
<!-- Add picture -->
So, now we’ve 2 options, the cmdrule where list entries are examined for command authorization, and the rule where entries are examined for rpc, notification, and data authorization.

Let’s add a rule to deny the sync-from
<!-- Add picture -->
In the rule we’ve to specify a path
<!-- Add picture -->
This path is nothing more than an XPATH. To see how we can get the xpaths, we go back to CLI and we execute the command “`show devices device | display xpath`”
```bash
ios-xr-2# exit
cisco@nso-613:~/NSO-TEST/nso-instance/packages/NTP/src$ ncs_cli -Cu admin
User admin last logged in 2022-10-26T10:32:10.740451+00:00, to nso-613, from 198.18.133.252 using webui-http
admin connected from 198.18.133.252 using ssh on nso-613
admin@ncs# show devices device | display xpath
/devices/device[name='ios-cli-0']/commit-queue/queue-length 0
/devices/device[name='ios-cli-0']/active-settings/connect-timeout 20
/devices/device[name='ios-cli-0']/active-settings/read-timeout 20
/devices/device[name='ios-cli-0']/active-settings/write-timeout 20
/devices/device[name='ios-cli-0']/active-settings/connect-timeout 20
/devices/device[name='ios-cli-0']/active-settings/read-timeout 20
/devices/device[name='ios-cli-0']/active-settings/write-timeout 20
<…output omitted for brevity…>
```

This will show all devices path, if we want to see the paths for each device
```bash
admin@ncs# config
admin@ncs(config)# show full-configuration devices device ios-xr-1 | display xpath
/devices/device[name='ios-xr-1']/address 127.0.0.1
/devices/device[name='ios-xr-1']/port 10023  


<…output omitted for brevity…>
/devices/device[name='ios-xr-1']/authgroup default
/devices/device[name='ios-xr-1']/device-type/cli/ned-id cisco-iosxr-cli-7.38
/devices/device[name='ios-xr-1']/state/admin-state unlocked
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:domain/name cisco.com
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:domain/name-server[address='2.2.2.2']
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:ntp/peer[address='192.168.22.33']
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:ntp/server/server-list[name='172.16.22.44']/minpoll 8
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:ntp/server/server-list[name='172.16.22.44']/maxpoll 12
/devices/device[name='ios-xr-1']/config/cisco-ios-xr:vtp/mode off
```

  
For example, we can grab the path “`/devices/device[name='ios-xr-1']/config/cisco-ios-xr:domain`” and block all the operations with the exception of reading.

We create the rule “**deny-config-domain**”
<!-- Add picture -->
And inside the rule on the data-node we use the path that we just got, we setup the action to deny and access operations we will deny “`create,update,delete,exec`”
<!-- Add picture -->
To see all the access-operations you can toggle the button near “**view options**” on the top right and then the info button will appear in each tile.
<!-- Add picture -->
After this, we commit.

We logout from the admin user and we login into the **read_user**.

Now we go to the Device Manager and we test our rules. We can check the “`ping`”, “`connect`”, “`check-sync`” and “`sync-from`” and we will see that the “`sync-from`” got denied.
<!-- Add picture -->
Now, if we go to the device **ios-xr-1** and we go into *config/domain* we will see that we’re only able to read the info
<!-- Add picture -->
And, if we try to add anything by clicking on the **+** button we will get the “*access denied*” notification.
<!-- Add picture -->
Since we’ve made the rule just for the **ios-xr-1** device, if we go to the other devices, for example **ios-xr-0** we see that we can still make changes in the domain configuration.
<!-- Add picture -->
To correct this, we need to login into the admin user, and change the **PATH** of this rule.

So, instead of “`/devices/device[name='ios-xr-1']/config/cisco-ios-xr:domain/`”

We will change to “`/devices/device/config/cisco-ios-xr:domain/`”

Commit and go back to the **read_user**.

We can see that after this change, we’re not able to make any change in domain configuration of all the managed devices with this user.
<!-- Add picture -->