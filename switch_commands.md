# :construction: Work in progress! :construction:

# Switch command cheat-sheet
#### Useful switch commands for CCNA2 (Routing and Switching Essentials) Cisco Networking Academy course.

## Table of contents

(coming soon)

## Before we start: Configuration modes
Three basic configuration modes we MUST be familiar with already (you will see them below, a lot).  

Mode (prompt)|Explanation (sorta)|Mode change
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
``S1#show interface [int-id]``|_f0/5_, for example
``S1#show mac address-table``|
``S1#show port-security interface [int-id]``|
``S1#show vlan``|
``S1#show vlan brief``|**only** displays VLANs, statuses, names, and assigned ports
``S1#show interface vlan [id]``|
``S1#show interface trunk``|
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
|
``S1(config)#crypto key zeroise rsa``|use to delete RSA key pair

## Managing more than one interface at the same time
When we want to execute a sequence on commands on more than one port, selecting an interface range makes the job a lot easier.  
Use: ``S1(config)#interface range [typeModule/firstNumber]-[lastNumber]``
|*typeModule*s|abbreviation
|---|---|
|``FastEthernet``|``f``
|``GigabitEthernet``|``g``

Here's an example:
``S1(config)#interface range f0/1-12``  

Note that you can select multiple ranges on a single command.  
Here's an example:
``S1(config)#interface range f0/1-12, 15-24, g0/1-2``

You might need to use it frequently on scenarios where the following blocks of commands are used.

## Configuring VLANs
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
``S1(config)#delete flash:vlan.dat``|erases the whole VLAN database

### Removing interface(s) a VLAN
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
