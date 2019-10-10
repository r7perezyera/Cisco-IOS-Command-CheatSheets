# :construction: Work in progress! :construction:

# Switch command cheat-sheet
#### Useful switch commands for CCNA2 (Routing and Switching Essentials) Cisco Networking Academy course.

## Table of contents


## Before we start: Configuration modes
Three basic configuration modes we MUST be familiar with already (you will see them below, a lot).
Mode (prompt)|Explanation (sorta)|idk
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

#### Option 1 to display stuff
Command|Additional Notes
---|---
``S1#show running-config``|N/A
``S1#show interface [int-id]``|
``S1#show mac address-table``|
``S1#show vlan``|
``S1#show vlan brief``|
``S1#show interface vlan [id]``|
``S1#show port-security interface [int-id]``|

#### Option 2 to display stuff
```
S1#show running-config                          // some comment on how the command works
S1#show interface [int-id]
S1#show port-security interface [int-id]        // another comment
S1#show mac address-table
S1#show vlan
S1#show vlan brief
S1#show interface vlan [id]
```

#### Option 3 would be same as option 1 but with configuration mode in its own column
