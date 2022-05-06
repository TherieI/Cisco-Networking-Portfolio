# Securing a Cisco router with FreeRADIUS

*By Gabriel Rosas, in cooperation with Harsha Bhat*



This guide will teach you how to configure AAA on a Cisco router, secured by FreeRADIUS on a raspberry pi. This should also work on any other Linux distro.



**Prerequisites**

* Have SSH working on the raspberry PI.



## Topology

<img src="guide\topology.PNG" alt="topology" style="zoom:80%;" />



## Installing FreeRADIUS

|                             Step                             |                          Reference                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Log in to your raspberry pi. (Or on Linux, open a terminal). | <img src="guide\1. Installing FreeRADIUS\1.PNG" alt="1" style="zoom:80%;" /> |
| Install freeRADIUS using the command<br />`sudo apt-get install freeradius freeradius-utils`. | <img src="guide\1. Installing FreeRADIUS\2.PNG" alt="2" style="zoom:80%;" /> |
| Navigate to `/etc/freeradius/3.0`. We will be editing the `clients.conf` and `users` files. | <img src="guide\1. Installing FreeRADIUS\3.PNG" alt="3" style="zoom:80%;" /> |
|    Edit `clients.conf` using your preferred text editor.     | <img src="guide\1. Installing FreeRADIUS\4.PNG" alt="4" style="zoom:80%;" /> |
| Scroll to the bottom of the file. Add the client (in my case, the IP of the Cisco router).<br />For example, `client <ip> {}`.<br />Within the braces, define a `secret`, `nastype`, and `shortname`. | <img src="guide\1. Installing FreeRADIUS\5.PNG" alt="5" style="zoom:80%;" /> |
|            Save `clients.conf` and open `users`.             | <img src="guide\1. Installing FreeRADIUS\6.PNG" alt="6" style="zoom:80%;" /> |
| Scroll to the bottom of the file. Add the line `<username> Cleartext-Password := "password"` for each unique user. <br />Add the line, `Service-Type = NAS-Prompt-User,` and another for permission `Cisco-AVPair = "shell:priv-lvl=<0-15>"` per user.<br />This will act as an account for an admin to log in with.<br />For enable mode, use `$enab15$` for the username. | <img src="guide\1. Installing FreeRADIUS\7.PNG" alt="7" style="zoom:80%;" /> |
| Ensure freeRADIUS is working, then launch debug mode: `sudo freeradius -X` | <img src="guide\1. Installing FreeRADIUS\8.PNG" alt="8" style="zoom:80%;" /> |

If everything went according to plan, we should have a FreeRADIUS server with two accounts and an `enable` password. It is time to add the complementary commands on the router.



## Configuring AAA on the router

First we will define a new AAA model, set a banner and fail message, and state we want radius to act as the default login server. We also set the enable mode to use our radius server.

```
aaa new-model
aaa authentication attempts login 5
aaa authentication banner `Stop mortal. Speak thine Words of Entry.`
aaa authentication fail-message `You have failed. Begone.`
aaa authentication login default group radius
aaa authentication enable default group radius
```

Finally, we define the radius server that the router should use. The IP should be the radius server and the key is the one we defined in `clients.conf`.

```
radius server PI_RADIUS
 address ipv4 <ip> auth-port 1812 acct-port 1813
 timeout 30
 retransmit 3
 key chad
```

And that's it! Hopefully everything is working as expected.
