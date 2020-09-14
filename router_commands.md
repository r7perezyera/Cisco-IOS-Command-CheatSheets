# :construction: Work in progress! :construction:

# CCNA Router command cheat-sheet
#### Useful router commands for CCNA2 v6 (Routing and Switching Essentials) Cisco Networking Academy course.
---
### :warning: Please note that CCNA v6 is outdated! As of February 24 2020 the new exam version (200-301) and Netacad course material are available.  However, I'm doing my best to keep these cheat-sheets updated with the same content you can find in the new Netacad course versions.
---
## Table of contents

- [Important ``show`` commands](#important-show-commands)
- [Output filtering](#filtering-information-from-show)
- [**Static routing**](#static-routing)
- [**Dynamic routing: OSPF**](#dynamic-routing-ospf)
- [Routing between VLANS: ROAS](#configuring-Router-on-a-stick-inter-VLAN-routing)
- [Access Control Lists (ACL)](#access-control-lists-(acl))
- [DHCPv4](#DHCPv4)
- [DHCPv6](#DHCPv6)
- [NAT](#NAT)
- [Common administrative distance values](#appendix-common-administrative-distance-ad-values)
- [Previous exam version content](#legacy-section-CCNA-version-6-200-125-exam)
    - [RIP](#dynamic-routing-rip)
    - [EIGRP](#dynamic-routing-eigrp)
        - [Configuration](#configuration)
        - [Verification](#verification)
        - [Fine tuning](#fine-tuning)
    - [EIGRP for IPv6](#dynamic-routing-eigrp-for-ipv6)

---
## Before we start: Configuration modes
Three basic configuration modes we MUST be familiar with already (you will see them below, a lot).  

Mode (prompt)|Mode|Mode change (current -> next)
---|---|---
``R1>``|user EXEC mode  AKA _view-only mode_|type ``enable`` to pass to next mode
``R1#``|Privileged EXEC mode|type ``configure terminal`` to pass to next mode
``R1(config)#``|Global configuration mode|N/A

Common abbreviations to the commands above (separated by commas):
```
en, ena
conf t, config term
```

On a Cisco router, from **global configuration mode**, you can also access the two following configuration modes:

Mode (prompt)|Mode|Description
---|---|---
``R1(config-if)#``|Interface configuration mode|used to configure an individual interface
``R1(config-router)#``|Router configuration mode|used when configuring an IPv4 routing protocol

> :bulb: When configuring an IPv6 routing protocol, such as OSPFv3, the router configuration mode prompt will be: ``R1(config-rtr)#``

---
## Important ``show`` commands:
Note that these commands are executed on privileged EXEC mode (``R1#`` prompt).  
You can execute them from global configuration mode (``R1(config)#`` prompt) by adding the ``do`` keyword before the command.  
example:  
``R1(config)#do show ip interface brief``  

Command|Description
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
## Static routing

With an administrative distance (AD) of ``1``. These are, just after the **directly connected routes** the most trustworthy routes a router can have on its routing table.

Static routes are all configures in **global configuration mode**, with the ``ip route`` command, i.e.,
>``R1(config)#ip route ...``

We'll cover the whole command right away.

### There are essentially four types of static routes, with particular purposes.

### Standard Static Route
### Default Static Route
### Summary Static Route
### Floating Static Route

---
## Dynamic routing: OSPF

### OSPF configuration
Command|Description
---|---
``R1(config)#router ospf [PID]``|create OSPF process with Process ID [PID] (1 - 65535)
``R1(config-router)#router-id [a.b.c.d]``|manually assign the router an ID, in an IPv4 address format.
``R1(config-router)#network [network-address] [wildcard-mask] area [area id]``|for all directly connected networks, announce each network following this nomenclature. Area ID can go from 0 to 4294967295

:bulb: The ``area id`` can also be expressed in IP address format, hence the range of available ``area id``s.

:bulb: ProTip: How can I easily visualize all the directly connected networks a router has?  
Issue the ``R1(config)#do show ip route con`` command.  
It will display the routing table ONLY with the directly connected networks (routes).

>:trophy: **Best practice:** OSPF routers **within an area** use **and need unique IDs** to identify themselves. It is highly convenient to manually set a desired router ID with the ``router-id`` command.


### OSPF verification

The following are useful commands to verify and troubleshoot your OSPF configurations.

Command|Description
---|---
``R1#show ip protocols``|your go-to command to quickly verify key OSPF configuration information on a router, including PID, router ID, advertised networks, neighbors and administrative distance.
``R1#show ip route``|adfa. Additionally, issue ``shw ip route ospf`` to see only the OSPF-learned networks (entries) on the routing table.
``R1#show ip ospf neighbor``|list of this router's OSPF _neighbor_ routers
``R1#show ip ospf``|useful to identify the PID, router ID, area information, and when the SPF algorithm was run for the last time
``R1#show ip ospf interface brief``|summary of router's OSPF-enabled interfaces. :warning: This command does NOT seem to work on Packet Tracer 7.3
``R1#show ip ospf interface``|in-detail list of every OSPF-enabled interface
``R1#show ip ospf interface [int-id]``|in-detail information for a particular OSPF-enabled interface


:bulb: Recall: OSPF's default AD is ``110``  
You can find common AD values [here](#appendix-common-administrative-distance-ad-values).

---
## Configuring Router-on-a-stick inter-VLAN routing

To enable router-on-a-stick inter-VLAN routing, we must configure multiple **subinterfaces** (as many as the number of VLANs over which we want to route traffic).  
Through this process, only one physical interface, and thus, one trunk link, are required.  

In global configuration mode, i.e., ``R1(config)#`` prompt:

Command|Description
---|---
``R1(config)#interface g0/0.[vlan-id]``|create the ``.[vlan-id]`` **subinterface** on interface Gigabit Ethernet 0/0
``R1(config-subif)#encapsulation dot1q [vlan-id]``|configure subinterface to operate on a specified VLAN
``R1(config-subif)#encapsulation dot1q [vlan-id] native``|must be configured on the subinterfaace belonging to the **native** VLAN
``R1(config-subif)#ip address [ip-address] [subnet-mask]``|:exclamation: don't forget to assign the subinterface an IP address!
``R1(config-subif)#interface g0/0``|access the Gigabit0/0 interface (i.e., the actual physical interface) to enable it
``R1(config-if)#no shutdown``|enable the physical interface. This enables **all** configured subinterfaces **on that interface**

---
## Access Control Lists (ACL)
> :construction: Disclaimer: Do keep in mind that work is currently in progress on this section.  
Additional commands coming soon... :construction:

### Procedure for configuring Standard ACLs

Command|Description
---|---
``R1(config)#access-list [number] (permit/deny) [address] [wildcard mask]``|create entry in standard IPv4 ACL
``R1(config)#interface [int-id]``|select the interface to which the ACL will be applied
``R1(config-if)#ip access-group [number] (in/out)``|activate the ACL on the selected interface

---
## DHCPv4

### Configure a Cisco router as a DHCPv4 server

---
## DHCPv6

### Stateless DHCPv6

---
## NAT

### Configuring Static NAT
Command|Description
---|---
``R1(config)#ip nat inside source static [local-add] [global-add]``|configure static nat, specifying the local address that will be mapped to a global address

### Inside and outside interfaces
:bulb: Recall: After this, **always remember** to specify the **inside** and **outside** interfaces.  
>Though it might be easy for us to identify them with our topology diagram, it's not that easy for the router.

Command|Description
---|---
``R1(config)#interface [int-id]``|ID of the _inside_ interface
``R1(config-if)#ip nat inside``|
``R1(config)#interface [int-id]``|ID of the _outside_ interface
``R1(config-if)#ip nat outside``|


### Configuring Dynamic NAT
Command|Description
---|---
``R1(config)#ip nat pool [pool-name] [start-address] [end-address] {netmask [mask] \| prefix-length [prefix-length]}``|create the pool of routable addresses that hosts will use to exit the private network
``access-list [number] {permit \| deny} [source] [wildcard mask]``|create an access list to blah blah
``ip nat inside source list [acl-number] pool [pool name]``|we now bind the pool to the ACL

:bulb: Lastly, **DO NOT forget to [specify the inside and outside interfaces](#inside-and-outside-interfaces)**



---
# Appendix: Common administrative distance (AD) values:

Default values:

code|type|AD value
---|---|---
``C``|Directly connected network|``0``
``S``|Static route|``1``
``D``|Internal EIGRP|``90``
``O``|OSPF|``110``
``R``|RIP|``120``

[Back to the table of contents](#table-of-contents)  
[Back to OSPF verification](#ospf-verification)  
[Back to EIGRP verification](#eigrp-verification)

---
# Legacy section (CCNA version 6, 200-125 exam)
:warning: **Attention:** the following topics are no longer on the **current** version (7) of the 200-301 CCNA exam.

## Dynamic routing: RIP

## Dynamic routing: EIGRP

### EIGRP Configuration
Command|Description
---|---
``R1(config)#router eigrp [AS-number]``|[AS-number] value range: 1-65535.
``R1(config-router)#eigrp router-id [a.b.c.d]``|(optional) manually configure a **router ID**, in an IPv4 address format
``R1(config-router)#network [network-address]``|add the classful network address for each directly connected network
``R1(config-router)#network [network-address] ([wildcard-mask])``|add the network address with the wildcard mask, recommended when using classless addresing


:bulb: If no EIGRP router ID is configured, the router will use the highest IPv4 address of any active loopback interface. If the router has NO active loopback interfaces, the router ID will be the highest IPv4 address of any active physical interface.

:bulb: ProTip: How can I easily visualize all the directly connected networks a router has?  
Issue the ``R1(config)#do show ip route con`` command.  
It will display the routing table ONLY with the directly connected networks (routes).

:bulb: Recall: by default, EIGRP does NOT automatically summarize networks.

### EIGRP Verification
Command|Description
---|---
``R1#show ip protocols``|verifies the current configured values for EIGRP (and any additional enabled routing protocol on the device)
``R1#show ip eigrp neighbors``|displays the nighbor table. Use it to verify the router recognizes its neighbors.
``R1#show ip route eigrp``|display **only EIGRP** entries in the routing table
``R1#show ip eigrp interface``|verifies which interface are enabled for EIGRP, number of peers, and transmit queues

:bulb: Recall: (internal) EIGRP's default AD is ``90``  
You can find common AD values [here](#appendix-common-administrative-distance-ad-values).
- _The AD for an **EIGRP summary route** is 5._
- _The AD for an **EIGRP external route** is 170._

### EIGRP Fine tuning
Command|Description
---|---
``R1(config-router)#variance [variance]``|change variance to perform **unequal cost load balancing**. [variance] value range: 1-128.
``R1(config-router)#auto-summary``|enable **automatic summarization**
``R1(config-router)#redistribute static``|propagate a **default static route**. Works on both IPv4 and IPv6. :bulb: remember to check routing tables to ensure correct/desired configuration
``R1(config-if)#bandwidth [BW in kilobits]``|modify the EIGRP metric. :warning: this is done in **interface configuration mode**
``R1(config-if)#ip hello-interval eigrp [AS-number] [time in seconds]``|modify hello interval on IPv4. :warning: this is done in **interface configuration mode**
``R1(config-if)#ipv6 hello-interval eigrp [AS-number] [time in seconds]``|modify hello interval on IPv6. :warning: this is done in **interface configuration mode**
``R1(config-if)#ip hold-time eigrp [AS-number] [time in seconds]``|modify hold timer length on IPv4 :warning: this is done in **interface configuration mode**
``R1(config-if)#ipv6 hold-time eigrp [AS-number] [time in seconds]``|modify hold timer length on IPv6 :warning: this is done in **interface configuration mode**

:bulb: Recall: **automatic summarization** is **disabled** by default

:warning: When modifying EIGRP hello intervals and hold timers, ALWAYS MAKE SURE your Hello intervals are LESS THAN your hold timers. Otherwise, you could have a _flapping link_ (due to timer misconfiguration, constantly goes up, down, up, ...)


## Dynamic routing: EIGRP for IPv6

### Configuration
Command|Description
---|---
``R1(config)#ipv6 unicast-routing``|this **global configuration command** enables the router to forward IPv6 packets
``R1(config)#ipv6 router eigrp [AS-number]``|enter router configuration mode for EIGRP for IPv6, specifying the AS number
``R1(config-rtr)#eigrp router-id [a.b.c.d]``|:bulb: The EIGRP for IPv6 process will start running **after** you enter the router ID
``R1(config-rtr)#no shutdown``|:bulb: This is a best practice, since EIGRP for IPv6 has a **shutdown feature**
``R1(config-rtr)#interface [int-id]``|now, go to **each interface** where you want to enable EIGRP for IPv6
``R1(config-if)#ipv6 eigrp [AS-number]``|use this command with the AS-number you used when configuring EIGRP for IPv6 on **each desired interface**

:bulb: Note that the configuration mode prompt when configuring EIGRP for IPv6 is different. While EIGRP for IPv4 is configured under the configuration mode with prompt ``R1(config-router)#``, EIGRP for IPv6 is configured under the mode/prompt ``R1(config-rtr)#``

### Verification
The same process for verification or troubleshooting for IPv4 can be used on IPv6 implementations. Replace ``ip`` for ``ipv6`` in your commands

Command|Description
---|---
``R1#show ipv6 protocols``|verifies the current configured values for EIGRP (and any additional enabled routing protocol on the device)
``R1#show ipv6 eigrp neighbors``|displays the nighbor table. Use it to verify the router recognizes its neighbors.
``R1#show ipv6 route eigrp``|display **only EIGRP** entries in the routing table
``R1#show ipv6 eigrp interface``|verifies which interface are enabled for EIGRP, number of peers, and transmit queues
