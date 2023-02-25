# Route53
- You can register domains directly in Route63  **up to 50 domains**, you can extend it after contacting support
- ELB do not have static IP, you have to always use its DNS name
- **You can set up notifications for health checks**

# Routing polices

## **Simple Routing**
- only one record, with one or more addresses
- Different addresses are returned in random order \(client can load balance or failover\)

## Weighted Routing
- You can split traffic to different regions \(IPs\)
- You can set weight
- All weights are added so weight / sum\(Weights\) = percentage of traffic

## Latency\-based routing
- Answer to request is based on latency from DNS server to target servver

## Failover routing
- You can define active and passive target
- When active fails, switch to passive is made

## Geolocation routing
- DNS resolution is made based on geolocation of ip of requester

## Geoproximity routing
- Routing is based on geographic location of you users and requested resources
- You can set **Bias** which **expands or shrinks geographic region from which traffic is routed**
- **You have to use Route 53 traffic flow**
- You can set up multiple different rules based on geo locations

## Multivalued answer routing
- **Like simple routing but enables you to set up health checks for each record set**
