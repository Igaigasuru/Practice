## Overview
This practice shows how to confirm multi activation behaviour.

## Points
If cluster internal communication among cluster nodes works, failover group can start on only one server.  
However, if Network Partition occurs, there is a possibilty that the group start on multi servers.  
When cluster detects a group is active on multi servers, it execute emergency shutdown on all servers (\*) to avoid data corruption.  
 \* This action for multi activation is different from Action at NP Occurrence although both actions are "emergency shutdown".

## Practice
