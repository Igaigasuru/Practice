# Practice 3
We assume that you came this Practice from [TOP](https://github.com/Igaigasuru/Practice/edit/master/EC/PingNP/Practice_PingNP_top.md).  
Let's check the behaviour in the case of "All HeartBeats get down when cluster is already running".

## Configuration
```bat
             [PingNP Target]
                    |
+-------+           |           +-------+
|       |-----------+-----------|       |
|  SV1  |    Interconnects 1    |  SV2  |
|       |                       |       |
|       |-----------------------|       |
+-------+    Interconnects 2    +-------+
```
## Practice
1. Start failovergroup on SV1.
1. Disconnect Interconnects1 among SV1, Ping NP Target and SV2.
	```bat
	             [PingNP Target]
	                    |
	+-------+           |           +-------+
	|       |-----------+-----------|       |
	|  SV1  |    Interconnect 1     |  SV2  |
	| (Act) |                       | (Std) |
	|       |---------- * ----------|       |
	+-------+    Interconnect 2     +-------+
	```  
	You can reproduce this situation by the below.
	- Physical servers: Disconnect Network Switch among SV1, SV2 and PingNP target.
	- Virtual machines: Disable Interconnect1 NIC on both PingNP target and SV2.  
1. Wait HeartBeat timeout (default: 30sec) or more.
1. Logon SV1 and check cluster status by yourself.
1. Logon SV2 and check cluster status by yourself.

## Advanced Practice
Check the behaviour from log, "C:\Program Files\EXPRESSCLUSTER\log\userlog.log".

- SV1 userlog.log  
	Check the log by yourself.
- SV2 userlog.log  
	Check the log by yourself.