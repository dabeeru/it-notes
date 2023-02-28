
## Application load balancer
- based on **HTTP** and **HTTPs traffic \(Layer 7\)**
- You need to create **target group** that can be associated with **load balancer**
- You can set up **different routes** based on **http headers, paths etc.**
- [[Sticky session]]s supported **on target group level**

## Network load balancer
- **TCP traffic**
- Layer 4 
- high performance
- low latency
- **Can terminate TLS connection reducing the load on EC2 instance**
- **You can configure security policy for specifing cypher and protocols for TLS handshake**

## Classic load balancer
- Support both layer 7 and layer 4 load balancing
- Supports **X\-Forwarded**  original IP of the client can be saved in this header on load balancer
- Supports [[Sticky session]]s request will be send to specific target during the sessions
- If application is not responding LB will respond with **504** http error
- Cross zone load balancing traffic will be load balanced across zones evenly
- Connection draining period time when traffic can still flow before removing vm from load balancing targets
- **Health checks**
	- **Response timeout**
	- **HealthCheck interval**
	- **Unhealthy threshold**
	- **Healthy threshold**
- Load balancers are always associated with DNS name, you are never provided with IP address
- There are two statuses of instance in context of load balancing  InService and OutOfService
- You need to have **at least two subnets** in order to create load balancer \(?\)
- ALB needs to be deployed at least to two different AZs
