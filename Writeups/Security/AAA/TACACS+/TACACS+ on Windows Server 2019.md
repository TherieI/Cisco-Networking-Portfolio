# Securing a Cisco router with TACACS+ on Windows Server 2019

*By Gabriel Rosas*



This guide will teach you how to configure AAA on a Cisco router, secured by TACACS+ running on Windows Server 2019. I used a VirtualBox VM of Windows Server 2019 in this guide, but it should work the same regardless of VM.



**Prerequisites**

* None



## Topology

<img src="guide\topology.PNG" alt="topology" style="zoom:67%;" />



## Setting up a VirtualBox VM

|                             Step                             |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Install and open VirtualBox. Navigate to `Machine>New` to create a new machine. | <img src="guide\1. Creating the VM\1.PNG" alt="1" style="zoom:80%;" /> |
|                 Name and select the version.                 | <img src="guide\1. Creating the VM\2.PNG" alt="2" style="zoom:80%;" /> |
| Create a Disk Drive and allocate a reasonable amount of space (50GB is more than enough). | <img src="guide\1. Creating the VM\3.PNG" alt="3" style="zoom:80%;" /> |
| After the disk has been created, launch the VM. <br />A prompt should appear. Enter the Windows Server ISO file location. | <img src="guide\1. Creating the VM\4.PNG" alt="4" style="zoom:80%;" /> |



## Installing Windows Server

|                             Step                             |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Set language preferences.<br />Navigate with `TAB` and `ENTER`. | <img src="guide\2. Installing Win Ser\1.PNG" alt="1" style="zoom:80%;" /> |
|                      Hit `Install Now`.                      | <img src="guide\2. Installing Win Ser\2.PNG" alt="2" style="zoom:80%;" /> |
| Choose either of the Standard versions. <br />I chose the Graphical (Desktop) version. | <img src="guide\2. Installing Win Ser\3.PNG" alt="3" style="zoom:80%;" /> |
|                     Do a custom install.                     | <img src="guide\2. Installing Win Ser\4.PNG" alt="4" style="zoom:80%;" /> |
| Select the partition available and continue.<br />The install should now complete. | <img src="guide\2. Installing Win Ser\5.PNG" alt="5" style="zoom:80%;" /> |



## Installing and running a basic TACACS+ Service

|                             Step                             |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Download the TACACS+ service from [TACACS.net][TACACS_DOWNLOAD]. | <img src="guide\3. TACACS configuration\1.PNG" alt="1" style="zoom:80%;" /> |
| To transfer a file from the host to the guest machine, I recommend using a USB drive.<br />For USB 3.0 support, install and run the VirtualBox Extension pack found [here][VB_EXTENSION].<br />Run the TACACS exe. You should be prompted to enter a key. Remember that key, we will need it later. | <img src="guide\3. TACACS configuration\2.PNG" alt="2" style="zoom:80%;" /> |
| TACACS.net services should now be installed.<br />Navigate to the TACACS configuration folder. | <img src="guide\3. TACACS configuration\3.PNG" alt="3" style="zoom: 67%;" /> |
| There will be a couple `xml` files in the `TACACS` folder.<br />We will edit `authentication`, `authorization`, `tacplus`. Start by opening `authentication.xml` and locate `UserGroup`. | <img src="guide\3. TACACS configuration\4.PNG" alt="4" style="zoom:80%;" /> |
| Much like HTML, information is embedded within editable UML tags.<br />Users are defined like `<User> ... </User>` with internal tags containing information about the user.<br />Feel free to change the `<Name>`, `<LoginPassword>`, and `<EnablePassword>` tags to fit the username and passwords you desire.<br />Note that comments are in the form: `<!-- comment here -->`. The users may be commented out, and you may have to uncomment them. | <img src="guide\3. TACACS configuration\5.PNG" alt="5" style="zoom:80%;" /> |
| Open `authorization.xml` and locate the `<UserGroups>` section. You should see a UserGroup called "Network Engineer", exactly like that in `authentication.xml`.<br />There are a lot of interesting configurable features here, though we will only focus on permitting all commands. In the `<Shell>` tag, and add `<Permit>.*</Permit>` to allow the user to enter any command. | <img src="guide\3. TACACS configuration\6.PNG" alt="6" style="zoom:80%;" /> |
| Finally, open up `tacplus.xml`. Change `<LocalIP>` to contain the IP of the TACACS+ Server (most likely the local machine).<br />Feel free to edit any of the prompts. We need to restart the TACACS+ service for this specific change to take effect. | <img src="guide\3. TACACS configuration\7.PNG" alt="7" style="zoom:80%;" /> |
| In a command prompt, enter `sc stop TACACS.net` to stop the service and `sc start TACACS.net` to start it. | <img src="guide\3. TACACS configuration\8.PNG" alt="8" style="zoom:80%;" /> |
| We can use `tacverify` to check our files for syntax errors and test our TACACS+ service using `tactest`.<br />I recommend running both these commands to make sure everything is working. | <img src="guide\3. TACACS configuration\9.PNG" alt="9" style="zoom:80%;" /> |
| Tactest example: `tactest -s <IP> -k <Server key> -u <Username> -p <Password>`. <br />A success should look something like this: | <img src="guide\3. TACACS configuration\10.PNG" alt="10" style="zoom:80%;" /> |

That concludes the TACACS+ server-side configuration. Now lets move on to the router.



## Basic TACACS+ configuration on the Cisco Router

Define a new AAA model and use tacacs+ for authentication.

```
aaa new-model
aaa authentication login default group tacacs+
aaa authentication enable default group tacacs+
```

Use `tacacs server <name>` to create a new tacacs server. Add an IP and key. You defined the key when you ran the tacacs exe, though you can find it in `clients.xml` if you forgot.

```
tacacs server <Arbitrary Name>
 address ipv4 <IP>
 key <Server Key>
```

And that's it! The router should now request authentication and authorization from the TACACS+ Server.

[VB_EXTENSION]: https://www.virtualbox.org/wiki/Downloads
[TACACS_DOWNLOAD]: https://tacacs.net/download/
