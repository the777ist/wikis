# SYSTEM DESIGN

## Horizontal vs. Vertical Scaling:

Vertical Scaling - getting more powerful servers (increasing server resources)
Horizontal Scaling - getting more number of servers

### Pros and Cons:

Horizontal scaling:
- load balancing required
- resilient
- network calls (RPC) - slow
- data inconsistency
- scales well as more users are added

Vertical Scaling:
- no load balancing required
- single point of failure
- inter-process calls - fast
- data consistent
- hardware limit

First vertically scale, later add horizontal scaling

## Approaching System Design:

- Vertical scaling to optimize processes and increase throughput using same resource
- Preprocessing using cron jobs run during non-peak hours
- Backup servers to avoid single points of failure
- Horizontal scaling
- Microservices
- Distributed Systems
- Load Balancing between the distributed systems
- Decoupling i.e. separation of responsibilities
- Logging and monitoring
- Extensibility - decoupling allows scaling
- Low level system design

## Load Balancing and Consistent Hashing:

Load Balancing distributes requests across servers.  
### Classical Approach:
- use `hash(request_id) % no_of_servers` = the `server_id` that will satisfy that request
- drawback: if you add an extra server `n_servers` changes and `r_id` will end up on a different server. 
- this is bad because often we want to map requests with the same ids consistently to the same servers (there could e.g. be cached data there that we want to reuse).

### Consistent hashing:
- where `M` is a constant, 
- use `hash(request_id) % M` = `request_hash`
- use `hash(server_id) % M` = `server_hash`
- the `server_id` whose `server_hash` is closest to `request_hash` will satisfy that request

- to avoid any unbalanced hashing, each `server_id` can have multiple (`K` no. of) aliases: `server_id + 1`, `server_id + 2` ... `server_id + K`

