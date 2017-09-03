# Cassandra cluster on AWS #
A CloudFormation template that describes how to materialize a multi-node Cassandra cluster that is backed by Elastic 
Block Store (EBS) volumes and connect to each other using Elastic Network Interfaces (ENIs) in order to provide private
static IPs. 

## Architecture ##
This template is intended to be resilient to failures to nodes so if you take down any of the nodes, the downed nodes 
will come back up and re-attach the EBS volumes to re-gain their previous state and re-attach the ENI to re-gain the 
same IP and reconnect back into the cluster. We create an Auto-Scaling-Group per Cassandra node to allow this stateful
behavior and leverage and EBS-ENI pair per Cassandra node. This is one of the approaches to building stateful services
in the cloud. Another approach which involves leveraging Route 53's DNS Type A Records can be found 
[here](https://github.com/ukayani/cassandra-aws).

### Cluster details ###
This is a single AWS region (single C* datacenter) multi-AZ (multiple C* racks) deployment so we make use of the 
`Ec2Snitch`. The cluster consists of 3 nodes where each node resides in a single AWS Availability Zone.

### To-do ###
* OS optimizations
* Cassandra optimizations 
* EBS volume selection (commit log on one volume, data on another volume)
* Node-to-node encryption 
* Client-to-node encryption
* Monitoring (pull JMX metrics into CloudWatch Metrics visualized by Grafana)
* Repair (via [Cassandra Reaper](https://github.com/thelastpickle/cassandra-reaper))

**Note:** This is a work-in-progress