# :construction: Work in progress! :construction:

# Router command cheat-sheet
#### Useful router commands for CCNA2 (Routing and Switching Essentials) Cisco Networking Academy course.

## Table of contents

- [Router-on-a-stick](#configuring-Router-on-a-stick-inter-VLAN-routing)
- [Access Control Lists (ACL)](#access-control-lists-(acl))

---
## Before we start: Configuration modes
Three basic configuration modes we MUST be familiar with already (you will see them below, a lot).  

Mode (prompt)|Explanation (sorta)|Mode change (current -> next)
---|---|---
``R1>``|EXEC mode|type ``enable`` to pass to next mode
``R1#``|Privileged EXEC mode|type ``configure terminal`` to pass to next mode
``R1(config)#``|Global congiguration mode|N/A

Common abbreviations to the commands above (separated by commas):
```
en, ena
conf t, config term
```

## Important ``show`` commands:
Note that these commands are executed on privileged EXEC mode (``R1#`` prompt).  
You can execute them from global configuration mode (``R1(config)#`` prompt) by adding the ``do`` keyword before the command.  
example:  
``R1(config)#do show ip interface brief``  

Command|Additional Notes
---|---
``R1#show running-config``|display config saved in RAM (and the currently running configuration in the device)
``R1#show startup-config``|display config saved in NVRAM
``R1#show history``|


## A _basic_ initial device configuration (recommended)
Command|Additional Notes
---|---
``R1(config)#no ip domain-lookup``|
``R1(config)#cdp run``|
``R1(config)#line console 0``|
``R1(config-line)#logging synchronous``|
``R1(config-line)#history size [lines]``|
``R1(config-line)#exec-timeout [minutes] [seconds]``|
``R1(config-line)#end``|exit to EXEC privileged mode, where the next command will be executed
``R1#copy running-config startup-config``|Saves the running configuration to the NVRAM

---

### Filtering information from ``show``

(you guessed, coming soon :blush: :sweat_smile:)

## Configuring Router-on-a-stick inter-VLAN routing

To enable router-on-a-stick inter-VLAN routing, we must configure multiple **subinterfaces** (as many as the number of VLANs over which we want to route traffic).  
Through this process, only one physical interface, and thus, one trunk link, are required.  

In global configuration mode, i.e., ``R1(config)#`` prompt:

Command|Additional Notes
---|---
``R1(config)#interface g0/0.[vlan-id]``|create the ``.[vlan-id]`` **subinterface** on interface Gigabit Ethernet 0/0
``R1(config-subif)#encapsulation dot1q [vlan-id]``|configure subinterface to operate on a specified VLAN
``R1(config-subif)#encapsulation dot1q [vlan-id] native``|must be configured on the subinterfaace belonging to the **native** VLAN
``R1(config-subif)#ip address [ip-address] [subnet-mask]``|:exclamation: don't forget to assign the subinterface an IP address!
``R1(config-subif)#interface g0/0``|access the Gigabit0/0 interface (i.e., the actual physical interface) to enable it
``R1(config-if)#no shutdown``|enable the physical interface. This enables **all** configured subinterfaces **on that interface**

## Access Control Lists (ACL)
> :construction: Disclaimer: Do keep in mind that work is currently in progress on this section.  
Additional commands coming soon... :construction:

### Procedure for configuring Standard ACLs

Command|Additional Notes
---|---
``R1(config)#access-list [number] (permit/deny) [address] [wildcard mask]``|create entry in standard IPv4 ACL
``R1(config)#interface [int-id]``|select the interface to which the ACL will be applied
``R1(config-if)#ip access-group [number] (in/out)``|activate the ACL on the selected interface