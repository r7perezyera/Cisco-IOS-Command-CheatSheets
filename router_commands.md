# :construction: Work in progress! :construction:

# CCNA Router command cheat-sheet
#### Useful router commands for CCNA2 v6 (Routing and Switching Essentials) Cisco Networking Academy course.
---
### :warning: Please note that CCNA v6 is outdated! As of February 24 2020 the new exam version (200-301) and Netacad course material are available.  However, I'm doing my best to keep these cheat-sheets updated with the same content you can find in the new Netacad course versions.
---
## Table of contents

- [Important ``show`` commands](#important-show-commands)
- [Output filtering](#filtering-information-from-show)
- [Recommended initial configuration sequence](#a-basic-initial-device-configuration-recommended)
- [Netacad recommended initial configuration]()
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

## Filtering information from ``show`` commands:
Some commands, such as ``show running-config``, generate multiple lines of output.  

To filter output, you can use the *pipe* (``|``) character along with a **filtering parameter** and a **filtering expression**.
Filtering parameters|Effect
---|---
``section [filtering-expression]``|shows the section of the _filtering expression_
``include [filtering-expression]``|includes all lines of output that match the _filtering expression_ **ONLY**
``exclude [filtering-expression]``|excludes all lines of output that match the _filtering expression_
``begin [filtering-expression]``|shows all the lines of output **beginning from** the line that matches the _filtering expression_

### Usage:
Here's an example of the usage of filtering with a ``show`` command:  
``R1#show running-config | include line con``

:bulb: ProTip: By default, the screen of output consists of 24 lines. Should you want to change the number of output lines displayed on the terminal screen, you can use the command: ``R1# terminal length [number-of-lines]``  
:warning: Unfortunately, this command is NOT supported in Cisco Packet Tracer (tested on version 7.2.2).

---


## :construction: TODO Router initial configuration best practices :construction:
According to Cisco Networking Academy course material, the following command sequence constitutes the basic configuration a router should have.

---
## Static routing

---
## Dynamic routing: RIP

## Dynamic routing: EIGRP

### Configuration

### Verification

### Fine tuning

## Dynamic routing: OSPF

---
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

---
## NAT

### Configuring Static NAT
Command|Additional Notes
---|---
``R1(config)#ip nat inside source static [local-add] [global-add]``|configure static nat, specifying the local address that will be mapped to a global address

:bulb: Recall: After this, **always remember** to specify the **inside** and **outside** interfaces.  
>Though it might be easy for us to identify them with our topology diagram, it's not that easy for the router.

Command|Additional notes
---|---
``R1(config)#interface [int-id]``|ID of the _inside_ interface
``R1(config-if)#ip nat inside``|
``R1(config)#interface [int-id]``|ID of the _outside_ interface
``R1(config-if)#ip nat outside``|

