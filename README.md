# consul-101

## Notes

Client mode - exposes RPC and HTTP api's, forwards requests to consul services.
Server mode - maintains state and responds to queries. 
Read throughput increased by RO mode servers


### Servers

Server (Follower) / Server Leader
Maintain state  (Replicated / Forwards to federated datacenters) 

Follower servers always forward requests to the leader, what means the leader
can get very busy. Enterprise features enable read-only servers that can 
increase read throughput.

Reccomended deployments, advise up to no more than 6 consul servers and
performance starts to drop off. Quorum increases with more nodes but fault 
tolerance also increases.

Always reccomended to run an odd number of servers, as it increases fault 
tolerance. Even numbers does not increase performance. Run 3 or 5 serveres.

Quorum needs to be met before writes are commited to the cluster. These are 
distributed with log files via RPC.

Port 8300

### Datacenter 

A set of agents / servers is a data center. 
Reqs: 
- Low Latency
- High Bandwidth
- Unrestricted communication.

Primary / Secondary data centers for replication protocols.


## LAN Gossip

peer to peer communication for membershio info, failure detection and 
event broadcasting. 

Based on a protocol called Swim (Based on Serf). Port 8031

## WAN Gossip

Communication for datacenters. Same as LAN, but for all servers in the 
datacenter. Servers only speak on this. Followers perform the WAN gossip 
protocol.

Based on Swim. Port 8302

## Service communication

### Registration

A process or person can register services, typically this is automated. 

Registration is  a HCL file that is placed in the consul configuraton dir 
by default, /etc/consul.d/services. Consul needs to be reloaded once its 
updated ```consul reload``` this causes no downtime, just re-reads config.

If registered with the CLI, a reload does not need to be issued. But dont 
automate against the CLI.

Ensure only one service instance per node...

### DNS Interface

Automatic round robin load balancing. Standard arecord query returns ip, but 
SRV can be queried to return an ip + port pair. 

### HTTP

Can query http api to obtain information

### Health checks 
 
Health checks can be used to monitor status of services. Checks run on nodes
where the service runs and can execute various different types. 

Health checks are configured in a check stanza in the service registration 
configuration






