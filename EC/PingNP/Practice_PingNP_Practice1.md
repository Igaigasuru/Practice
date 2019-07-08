# Practice 1
Let's check the behaviour in the case of "All HeartBeats do not work when cluster service is started".

## Assumption
You have already checked the below:
- [Ping NP Resolution Resource Practice](https://github.com/Igaigasuru/Practice/blob/master/EC/PingNP/Practice_PingNP_top.md)

## Configuration
```bat
            [Ping NP Target]
                    |
+-------+           |           +-------+
|       |-----------+-----------|       |
|  SV1  |     Interconnect1     |  SV2  |
|       |-----------------------|       |
+-------+     Interconnect2     +-------+
```

## Practice
1. Shutdown both SV1 and SV2 by shutting down cluster.
	- WebManager: Right click cluster name and select "shutdown".
	- Command: Execute "clpstdn".
1. Disconnect Interconnects1 between Ping NP Target and SV2 and Interconnects2 between SV1 and SV2.  
	(All HeartBeats are disconnected and only SV1 is reachable to Ping NP target.)
	```bat
	            [Ping NP Target]
	                    |
	+-------+           |           +-------+
	|       |-----------+---- * ----|       |
	|  SV1  |     Interconnect1     |  SV2  |
	|       |                       |       |
	|       |---------- * ----------|       |
	+-------+     Interconnect2     +-------+
	```  
	You can reproduce this situation by the below.
	- Physical servers: Disconnect both Interconnect LANs from SV2.
	- Virtual machines: Disable both NICs on SV2.  
1. Boot both SV1 and SV2.
1. Wait 5 minutes (default Server Sync Wait Time) or more.  
1. Logon SV1 and check cluster status on SV1 (\*).
1. Logon SV2 nd check cluster status on SV2 (\*).  
	\* WebManager or clpstat command will take some time to show cluster status because of timeout to get other server's status.

## Advanced Practice
### Advanced Practice1
Check the behaviour from log, "C:\Program Files\EXPRESSCLUSTER\log\userlog.log".
- SV1 userlog.log  
  ```bat
    INFO  [pm   ] Cluster service has been started properly.
    INFO  [rc   ] Server SV1 started to return to the cluster.
    INFO  [rc   ] Server SV1 is returned to the cluster.
  * INFO  [nm   ] The resource pingnp1 of the server SV1 has been started.
    INFO  [nm   ] The resource lankhb1 of the server SV1 has been started.
    INFO  [nm   ] The resource lankhb2 of the server SV1 has been started.
     : (After 5 minutes)
  * INFO  [rc   ] The group failover is starting.
     :
  ```
- SV2 userlog.log  
	```bat
	  INFO  [pm   ] Cluster service has been started properly.
	  INFO  [rc   ] Server SV2 started to return to the cluster.
	  INFO  [rc   ] Server SV2 is returned to the cluster.
	  INFO  [nm   ] The resource lankhb1 of the server SV2 has been started.
	  INFO  [nm   ] The resource lankhb2 of the server SV2 has been started.
	   : (After 5 minutes)
	* INFO  [pm   ] There was a request to stop cluster service from the internal.
	   :
	* INFO  [pm   ] Cluster service is shutting down.
	```
### Advanced Practice2
Reconnect both Interconnects, make all cluster status GREEN, remove PingNP Target resource from the cluster and do the same practice and compare the behavior.
