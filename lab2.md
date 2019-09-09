# Lab 2 

Register the following service at ```/etc/consul.d/counting.json``` and issue 
consul reload.

```json
{
  "service": {
    "name": "counting",
    "port": 9003,
    "check": {
      "id": "counting-check",
      "http": "http://localhost:9003/health",
      "method": "GET",
      "interval": "1s",
      "timeout": "1s"
    }
  }
}
```

Run ```consul catalog services``` to list available services. 

Testing the consul DNS interface

``` dig @127.0.0.1 -p 8600 counting.service.consul SRV ```

```
consul-101@consul-101-labs-dinosaur:~ > dig @127.0.0.1 -p 8600 counting.service.consul SRV

; <<>> DiG 9.10.3-P4-Ubuntu <<>> @127.0.0.1 -p 8600 counting.service.consul SRV
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63982
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 3
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;counting.service.consul.       IN      SRV

;; ANSWER SECTION:
counting.service.consul. 0      IN      SRV     1 1 9003 consul-101-labs-dinosaur.node.dc1.consul.

;; ADDITIONAL SECTION:
consul-101-labs-dinosaur.node.dc1.consul. 0 IN A 10.1.1.204
consul-101-labs-dinosaur.node.dc1.consul. 0 IN TXT "consul-network-segment="

;; Query time: 0 msec
;; SERVER: 127.0.0.1#8600(127.0.0.1)
;; WHEN: Mon Sep 09 18:03:05 UTC 2019
;; MSG SIZE  rcvd: 164
```

## Service Mesh

Sidecar proxies used to manage communication across services. Automatically 
encrypts with mTLS and enables open tracing. Reduces metrics required for each 
service by surfacing connection details. 

Everything can speak to everything, unless an explicit deny all is added.

### Intentions 

A way to allow or deny service to service communications.

### Certificate Authority (CA) 

Certs are SPIFFE compliant. Established using mTLS 

- Managed CA via consul.
- External CA also possible, i.e. Vault.


## Tips 

Consul stores all KV's in memory. Leaders have all keys in memory and if it runs 
out of memory the node will choke. MONITOR MEMORY USAGE.


## Consul template

Different types from consul can be used in template, but the data structs
are not documented. Looking at the consul-template codebase you can determine
what properties are exposed to Consul-Template. 
https://github.com/hashicorp/consul-template/blob/master/dependency/health_service.go







