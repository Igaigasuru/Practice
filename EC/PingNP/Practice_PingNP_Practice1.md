# Practice 1
Let's check the behaviour in the case of "All HeartBeats do not work when cluster service is started".

## Configuration
```bat
            [Ping NP Target]
                    |
+-------+           |           +-------+
|       |-----------+-----------|       |
|  SV1  |                       |  SV2  |
|       |-----------------------|       |
+-------+     Interconnects     +-------+
```

## Practice
1. Shutdown both SV1 and SV2 by shutting down cluster.
	- WebManager: Right click cluster name and select "shutdown".
	- Command: Execute "clpstdn".
1. Disconnect Interconnects.
	- Phsycal servers: Disconnect both Interconnects LAN from SV2.
	- Virtual machines: Disable both NICs on SV2.  
    ```bat
                [Ping NP Target]
                        |
    +-------+           |           +-------+
    |       |-----------+---------  *       |
    |  SV1  |                       |  SV2  |
    |       |---------------------  *       |
    +-------+                       +-------+
    ```
1. Boot both SV1 and SV2.
1. Wait 5 minutes (default Server Sync Wait Time) or more.  
1. Logon SV1 and check cluster status on SV1 (\*).
1. Logon SV2 nd check cluster status on SV2 (\*).  
	\* WebManager or clpstat command will take some time to show cluster status because of timeout to get other server's status.

## Advanced Practice
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
