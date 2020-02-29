# A recommended basic initial device configuration 

You might find convenient to execute the following sequence of commands before doing any other configuration or troubleshooting tasks on your Cisco device, in order to improve your workflow on the terminal.

You will find a command-and-explanation table right below, and a text box with the same command sequence **you can copy and paste on your device's terminal** after that.

Note: These commands work on both Cisco routers and switches.

Command|Additional Notes
---|---
``CDevice>enable``|
``CDevice#configure terminal``|
``CDevice(config)#no ip domain-lookup``|disable DNS lookup
``CDevice(config)#cdp run``|ensure CDP is running :bulb:(although it is running on Cisco devices by default)
``CDevice(config)#line console 0``|
``CDevice(config-line)#logging synchronous``|
``CDevice(config-line)#history size [lines]``|specify the size (number of lines) for the history of executed commands (you can view your history with ``R1#show history``)
``CDevice(config-line)#exec-timeout [minutes] [seconds]``|
``CDevice(config-line)#end``|exit to EXEC privileged mode, where the next command will be executed
``CDevice#copy running-config startup-config``|Saves the running configuration to the NVRAM

You can copy the same sequence, contained in the text box below, and paste it in your Cisco device, whether it is a real-world physical Cisco device, or a simulated device in an environment like Packet Tracer.  
:bulb: Note that the commands below are abbreviated. Still. They should work just fine.  
:bulb: Be sure to select and copy up to the blank line below the last command. That way, such last command will be executed without you having to manually press enter. 

```
ena
conf t
no ip domain-lookup
cdp run
line con 0
logging syn
his size 50
exec-timeout 25 0
end
copy run start

<< select and copy up to the line above this "marker" with your cursor
```

## Other tips and hints for your initial configurations
:bulb:If there are non-Cisco devices on your network, you might also want to enable LLDP (Link-Layer Discovery Protocol), use:  
````CDevice(config)#lldp run````
