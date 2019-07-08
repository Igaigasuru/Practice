# Practice 2
Let's check the behaviour in the case of "All HeartBeats get down when cluster is already running".

## Assumption
You have already checked the below:
- [Ping NP Resolution Resource Practice](https://github.com/Igaigasuru/Practice/blob/master/EC/PingNP/Practice_PingNP_top.md)
- [Practice 1 for Ping NP Resolution Resource](https://github.com/Igaigasuru/Practice/blob/master/EC/PingNP/Practice_PingNP_Practice1.md)

## Configuration
```bat
            [Ping NP Target]
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
1. Disconnect Interconnects1 between Ping NP Target and SV2 and Interconnects2 between SV1 and SV2.  
	(All HeartBeats get down and SV1 is reachable to Ping NP target.)
	```bat
	            [Ping NP Target]
	                    |
	+-------+           |           +-------+
	|       |-----------+---- * ----|       |
	|  SV1  |    Interconnects 1    |  SV2  |
	| (Act) |                       | (Std) |
	|       |---------- * ----------|       |
	+-------+    Interconnects 2    +-------+
	```  
	You can reproduce this situation by the below.
	- Physical servers: Disconnect both Interconnect LANs from SV2.
	- Virtual machines: Disable both NICs on SV2.  
1. Wait HeartBeat timeout (default: 30sec) or more.
1. Logon SV1 and check cluster status (*).  
	\* WebManager or clpstat command will take some time to show cluster status because of timeout to get other server's status.
1. Logon SV2 and check status.

## Advanced Practice
After rebooting SV2, check the behaviour from log, "C:\Program Files\EXPRESSCLUSTER\log\userlog.log".
- SV1 userlog.log  
	```bat
	  ERROR [nm   ] The resource lankhb1 of the server ws12r2-iga2 has an error.
	  ERROR [nm   ] The resource lankhb2 of the server ws12r2-iga2 has an error.
	   : (Keeps on running)
	```
- SV2 userlog.log  
	```bat
	  ERROR [nm   ] The resource lankhb1 of the server ws12r2-iga1 has an error.
	  ERROR [nm   ] The resource lankhb2 of the server ws12r2-iga1 has an error.
	* INFO  [pm   ] There was a request of emergency shutdown from the internal.
	   : (Shutting down)
	```
