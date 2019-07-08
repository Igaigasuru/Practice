# Ping NP Resolution Resource Practice

## Overview
This practice shows how to confirm Ping NP Resoution behaviour.

## Reference
[Reference Guide](https://www.nec.com/en/global/prod/expresscluster/en/support/manuals.html)  
	Chapter: "Details on network partition resolution resources"

## Points
NP Resolution resources (including Ping NP) check their status when all HeartBeats get down.  
If NP Resolution resources detect Network Partition (NP), they will execute actions for NP because NP is not normal situation.  
Actions for NP are different between started cluster and running cluster.

Assuming one Ping NP resource:
1. All HeartBeats do not work when cluster service is started:
	- Cluster service on servers checks whether Ping NP target is reachable or not.
	- If Ping NP target is reachable, cluster service gets started.
	- If Ping NP target is NOT reachable, cluster service stops.

1. All HeartBeats get down when cluster is already running:
	- Cluster service on servers checks whether Ping NP target is reachable or not.
	- If Ping NP target is reachable, cluster service keeps on running.
	- If Ping NP target is NOT reachable, cluster service executes Action at NP Occurrence (default: Emergency shutdown).

## Practices
### Practice 1
Go to [Practice1 page](https://github.com/Igaigasuru/Practice/blob/master/EC/PingNP/Practice_PingNP_Practice1.md).
