# Cisco Unified Communications Manager V12.0.1.21900

By Gabriel Rosas

*A basic guide for getting IP Phones connected with the Cisco Unified Communications Manager. This guide assumes you have an ESXi Server*.



## Topology

<img src="CUCM Installation images\topology.PNG" alt="topology" style="zoom: 67%;" />



## Installing the CUCM on an ESXi Server



### 1. Downloading the CUCM ISO to local storage

|                Instruction                |                          Reference                           |
| :---------------------------------------: | :----------------------------------------------------------: |
| To upload an image, navigate to `Storage` | <img src="CUCM Installation images\1. Adding ISO to ESXi server files\1.PNG" alt="1" style="zoom:67%;" /> |
|         Click `Datastore browser`         | <img src="CUCM Installation images\1. Adding ISO to ESXi server files\2.PNG" alt="2" style="zoom: 50%;" /> |
| In the desired directory, click `Upload`  | <img src="CUCM Installation images\1. Adding ISO to ESXi server files\3.PNG" alt="3" style="zoom:100%;" /> |



### 2. Creating a Virtual Machine for the CUCM

|                         Instruction                          |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|           Navigate to `Virtual Machines/Create VM`           | <img src="CUCM Installation images\2. Creating CUCM VM\1.PNG" alt="1" style="zoom: 67%;" /> |
|                `Create` a new virtual machine                | <img src="CUCM Installation images\2. Creating CUCM VM\2.PNG" alt="2" style="zoom:50%;" /> |
| `Name` the VM and use *Red Hat Enterprise Linux 5* as the `Guest OS` | <img src="CUCM Installation images\2. Creating CUCM VM\3.PNG" alt="3" style="zoom:50%;" /> |
| Allocate reasonable amounts of `Memory` and `Storage`[^Error?]<br />Make sure the `CD/DVD Drive` boots the CUCM ISO | <img src="CUCM Installation images\2. Creating CUCM VM\4.PNG" alt="4" style="zoom:80%;" /><img src="CUCM Installation images\2. Creating CUCM VM\5.PNG" alt="5" style="zoom: 75%;" /> |

[^Error?]: The base allocations are insufficient for the CUCM (An error will occur during the boot). Personally, I've found **8 GB** of Memory and **100 GB** of Storage to work well, though I haven't tested other values rigorously.



## First time Installation 

|               Instruction                |                          Reference                           |
| :--------------------------------------: | :----------------------------------------------------------: |
|   Launch the new VM,<br />Select `OK`    | <img src="CUCM Installation images\3. Initializing the CUCM\1.PNG" alt="1" style="zoom:50%;" /> |
|               Select `OK`                | <img src="CUCM Installation images\3. Initializing the CUCM\2.PNG" alt="2" style="zoom:50%;" /> |
|             Select `Proceed`             | <img src="CUCM Installation images\3. Initializing the CUCM\3.PNG" alt="3" style="zoom:50%;" /> |
|               Select `No`                | <img src="CUCM Installation images\3. Initializing the CUCM\4.PNG" alt="4" style="zoom:50%;" /> |
|          Select your `Timezone`          | <img src="CUCM Installation images\3. Initializing the CUCM\5.PNG" alt="5" style="zoom:50%;" /> |
|               Select `No`                | <img src="CUCM Installation images\3. Initializing the CUCM\6.PNG" alt="6" style="zoom:50%;" /> |
|               Select `No`                | <img src="CUCM Installation images\3. Initializing the CUCM\7.PNG" alt="7" style="zoom:50%;" /> |
|     Enter `Static DHCP` information      | <img src="CUCM Installation images\3. Initializing the CUCM\8.PNG" alt="8" style="zoom:50%;" /> |
|               Select `No`                | <img src="CUCM Installation images\3. Initializing the CUCM\9.PNG" alt="9" style="zoom:50%;" /> |
| Enter `Administrative Login` credentials | <img src="CUCM Installation images\3. Initializing the CUCM\10.PNG" alt="10" style="zoom:50%;" /> |
|     Enter `Certificate` credentials      | <img src="CUCM Installation images\3. Initializing the CUCM\11.PNG" alt="11" style="zoom:50%;" /> |
|               Select `Yes`               | <img src="CUCM Installation images\3. Initializing the CUCM\12.PNG" alt="12" style="zoom:50%;" /> |
|          Enter `NTP Server(s)`           | <img src="CUCM Installation images\3. Initializing the CUCM\13.PNG" alt="13" style="zoom:50%;" /> |
|        Enter `Security Password`         | <img src="CUCM Installation images\3. Initializing the CUCM\14.PNG" alt="14" style="zoom:50%;" /> |
|               Select `No`                | <img src="CUCM Installation images\3. Initializing the CUCM\15.PNG" alt="15" style="zoom:50%;" /> |
|           Select `Disable All`           | <img src="CUCM Installation images\3. Initializing the CUCM\16.PNG" alt="16" style="zoom:50%;" /> |
|         Enter `User` Credentials         | <img src="CUCM Installation images\3. Initializing the CUCM\17.PNG" alt="17" style="zoom:50%;" /> |
|               Select `OK`                | <img src="CUCM Installation images\3. Initializing the CUCM\18.PNG" alt="18" style="zoom:50%;" /> |
|          Select `Re-initialize`          | <img src="CUCM Installation images\3. Initializing the CUCM\19.PNG" alt="19" style="zoom:50%;" /> |



## Registering Cisco IP Phones



### 1. Activating Services

|                         Instruction                          |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Log in to the CUCM using the Static IP in a web browser<br />Navigate to `Cisco Unified Serviceability` | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\1.PNG" alt="1" style="zoom:100%;" /> |
|             `Activate` all Services except DHCP              | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\2.PNG" alt="2" style="zoom:50%;" /> |



### 2. Enabling Auto-Registration

|                         Instruction                         |                          Reference                           |
| :---------------------------------------------------------: | :----------------------------------------------------------: |
|        Navigate to `Cisco Unified CM Administration`        | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\3.PNG" alt="3" style="zoom:100%;" /> |
|            Navigate to `System/Cisco Unified CM`            | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\4.PNG" alt="4" style="zoom:67%;" /> |
| Enable `Auto-Registration` and fill out the required fields | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\5.PNG" alt="5" style="zoom:50%;" /> |



### 3. Creating Phone Profiles

|                         Instruction                          |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                  Navigate to `Device/Phone`                  | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\6.PNG" alt="6" style="zoom:100%;" /> |
|                      `Add` a New Phone                       | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\7.PNG" alt="7" style="zoom:100%;" /> |
|       Enter your *specific* model phone and use `SCCP`       | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\8.PNG" alt="8" style="zoom:50%;" /> |
| Enter your phone's `mac address` and other required fields<br />Set the Owner to `Anonymous` | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\9.PNG" alt="9" style="zoom:50%;" /> |
| Click `Save`, then add a dial number if necessary<br />Restart previous steps for other phone | <img src="CUCM Installation images\4. Connecting Two IP phones (basic sadge way)\reload.PNG" alt="9" style="zoom:100%;" /> |





