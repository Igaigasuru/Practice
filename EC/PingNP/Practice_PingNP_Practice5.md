# Practice 5
We assume that you came this Practice from [TOP](https://github.com/Igaigasuru/Practice/edit/master/EC/PingNP/Practice_PingNP_top.md).  
Let's check the behaviour in the case of "All HeartBeats get down when cluster is already running".

## Configuration
```bat
             [PingNP Target]
                    |
+-------+           |           +-------+
|       |-----------+-----------|       |
|  SV1  |     Interconnect1     |  SV2  |
|       |                       |       |
|       |-----------------------|       |
+-------+     Interconnect2     +-------+
```
## Practice
1. Start failovergroup on SV1.
1. Disconnect Interconnects1 among SV1, Ping NP Target and SV2 and Interconnect2 between SV1 and SV2.
	```bat
	             [PingNP Target]
	                    |
	+-------+                       +-------+
	|       |---------- * ----------|       |
	|  SV1  |     Interconnect1     |  SV2  |
	| (Act) |                       | (Std) |
	|       |---------- * ----------|       |
	+-------+     Interconnect2     +-------+
	```  
	You can reproduce this situation by the below.
	- Physical servers: Disconnect Network Switch for Interconnect1 among SV1, SV2 and PingNP Target, and disconnect Interconnect2 LAN from SV2.
	- Virtual machines: Disable Interconnect1 NIC on PingNP Target and SV2, and disable Interconnect2 NIC on SV2.  
1. Wait HeartBeat timeout (default: 30sec) or more.
1. Logon SV1 and check status by yourself.
1. Logon SV2 and check status by yourself.

## Advanced Practice
After rebooting SV1 and SV2, check the behaviour from log "C:\Program Files\EXPRESSCLUSTER\log\userlog.log".

- SV1 userlog.log  
	Check the log by yourself.
- SV2 userlog.log  
	Check the log by yourself.
