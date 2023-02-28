- Enables you to **configure connection between multiple datacenters** and **VPCs** with **VPN \(datacenters can also connect with each other\)**
- **All traffic encrypted**
- Following Hub and spoke model

## Architecture
- Connection is made by setting up VPN tunnels between on\-premise data centers and VGW \(Virtual Gateway\)
- [[BGP]] Peering is configured between customer router & VGW using unique BGP VGW at each location
- VGW is retrieving prefixes from each location and its re\-advertising it to other peers
- IP addresses in all locations cannot overlap
