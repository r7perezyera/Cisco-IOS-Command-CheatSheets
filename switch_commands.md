# :construction: Work in progress! :construction:

# CCNA2 Switch command cheat-sheet
#### Useful switch commands for CCNA2 (Routing and Switching Essentials) Cisco Networking Academy course.

## Table of contents

(coming soon)

## Before we start: Configuration modes
Three basic configuration modes we MUST be familiar with already (you will see them below, a lot).  

Mode (prompt)|Explanation (sorta)|Mode change (current -> next)
---|---|---
``S1>``|EXEC mode|type ``enable`` to pass to next mode
``S1#``|Privileged EXEC mode|type ``configure terminal`` to pass to next mode
``S1(config)#``|Global congiguration mode|N/A

Common abbreviations to the commands above (separated by commas):
```
en, ena
conf t, config term
```


## Important ``show`` commands:
Note that these commands are executed on privileged EXEC mode (``S1#`` prompt).  
You can execute them from global configuration mode (``S1(config)#`` prompt) by adding the ``do`` keyword before the command.  
example:  
``S1(config)#do show ip interface brief``  

Command|Additional Notes
---|---
``S1#show running-config``|N/A
``S1#show history``|
``S1#show interface [int-id]``|useful to detect errors or verify packets are being sent and received
``S1#show mac address-table``|
``S1#show port-security``|displays Port Security configuration for all interfaces
``S1#show port-security interface [int-id]``|display Port Security configuration of an interface
``S1#show vlan``|
``S1#show vlan brief``|**only** displays VLANs, statuses, names, and assigned ports
``S1#show interface vlan [id]``|
``S1#show interfaces trunk``|
### TODO shows de troncales

## Configuring SSH
Command|Additional Notes
---|---
``S1#show ip ssh``|Use it to verify that the switch supports SSH
``S1(config)#ip domain-name [domain-name]``|
``S1(config)#crypto key generate rsa``|
``S1(config)#username [admin] secret [ccna]``|
``S1(config)#line vty 0 15``|
``S1(config-line)#transport input ssh``|
``S1(config-line)#login local ``|
``S1(config-line)#exit``|
``S1(config)#ip ssh version 2``|enable SSH version 2
``S1(config)#crypto key zeroise rsa``|:warning: use to **delete** RSA key pair

### Modifying SSH configuration
Command|Additional Notes
---|---
``S1(config)#ip ssh time-out [time]``|Change timeout setting (time in seconds)
``S1(config)#ip ssh authentication-retries [retries]``|Change number of allowed authentication attempts

Verify your newly configured settings with ``S1#show ip ssh``

## Managing more than one interface at the same time
When we want to execute a sequence on commands on more than one port, selecting an interface range makes the job a lot easier.  
Use: ``S1(config)#interface range [typeModule/firstNumber]-[lastNumber]``

*typeModule*s|abbreviation
---|---
``FastEthernet``|``f``
``GigabitEthernet``|``g``

Here's an example:
``S1(config)#interface range f0/1-12``  

Note that you can select multiple ranges on a single command.  
Here's an example:
``S1(config)#interface range f0/1-12, 15-24, g0/1-2``

You might need to use it frequently on scenarios where the following blocks of commands are used.

## :closed_lock_with_key: Configuring Dynamic Port Security
Command|Additional Notes
---|---
``S1(config)#interface [int-id]``|
``S1(config-if)#switchport mode access``|Set interface mode to *access*.
``S1(config-if)#switchport port-security``|Enable port security on the interface
``S1(config-if)#switchport port-security violation [violation-mode]``|set violation mode (``protect``, ``restrict``, ``shutdown``)

>:trophy: **Best practice:** It is a best security and general practice to "hard-type" the ``switchport mode access`` command. This also applies to Trunk ports (``switchport mode trunk``).

## :closed_lock_with_key: Configuring Sticky Port Security
Command|Additional Notes
---|---
``S1(config)#interface [int-id]``|
``S1(config-if)#switchport mode access``|Set interface mode to *access*.
``S1(config-if)#switchport port-security``|Enable port security on the interface
``S1(config-if)#switchport port-security maximum [max-addresses]``|Set maximum number of secure MAC addresses allowed on port
``S1(config-if)#switchport port-security mac-address sticky``|Enable sticky learning
``S1(config-if)#switchport port-security violation [violation-mode]``|set violation mode (``protect``, ``restrict``, ``shutdown``)

## :closed_lock_with_key: :white_check_mark: Verifying Port Security & secure MAC addresses
No that we have configured Port Security, the following commands will be handy to verify and troubleshoot.

Command|Additional Notes
---|---
``S1#show port-security interface [int-id]``|displays interface's Port Security configuration. If violations occured, they can be checked here.
``S1#show port-security address``|displays secure MAC addresses configured on **all switch interfaces**
``S1#show interface [int-id] status``|displays port status. Useful to verify if an interface is in ``err-disabled`` status.

## Bringing an ``err-disabled`` interface back up

:bulb: Recall: After a violation, a port in **Shutdown violation mode** changes its status to *error disabled*, and is effectively **shut down**. To resume operation (sending and receiving traffic), we must bring it back up. Here's how:

* Access the interface configuration mode with ``S1(config)#interface [int-id]``.
* Shut the interface down using ``S1(config-if)#shutdown``.
* Bring the interface back up using ``S1(config-if)#no shutdown``.

## VLANs

### Configuring VLANs
Command|Additional Notes
---|---
``S1(config)#vlan 20``|create a VLAN
``S1(config-vlan)#name [someName]``| assign a name to the VLAN

Now it is time to assign ports to the newly created VLAN  

Command|Additional Notes
---|---
``S1(config)#interface [int-id]``|remember, ``interface range`` might be useful
``S1(config-if)#switchport mode access``|
``S1(config-if)#switchport access vlan [vlan-id]``|assign/change port VLAN

### Deleting a VLAN
Command|Additional Notes
---|---
``S1(config)#no vlan [vlan-id]``|
``S1(config)#delete flash:vlan.dat``|:warning: erases **the whole VLAN database**

### Removing interface(s) from a VLAN
Command|Additional Notes
---|---
``S1(config)#interface [int-id]``|
``S1(config-if)#no switchport access vlan [vlan-id]``|remove the VLAN from the port

### Configuring IEEE 802.1q trunk links
Command|Additional Notes
---|---
``S1(config)#interface [int-id]``|
``S1(config-if)#switchport mode trunk``|
``S1(config-if)#switchport trunk native vlan [vlan-id]``|
``S1(config-if)#switchport trunk allowed vlan [vlan-list]``|**All** allowed VLAN IDs.

### Troubleshooting VLANs
Command|Additional Notes
---|---
``S1#show vlan``|check whether a port belongs to the expected VLAN
``S1#show mac address-table``|check which addresses were learned on a particular port of the switch, and to which VLAN that port is assigned
``S1#show interfaces [int-id] switchport``|helpful in verifying an inactive VLAN is assigned to a port

### Troubleshooting Trunks
Command|Additional Notes
---|---
``S1#show interfaces trunk``|- check native VLAN id matches on both ends of link  - check whether a trunk link has been established between switches
