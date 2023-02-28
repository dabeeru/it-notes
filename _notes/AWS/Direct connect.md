![[Untitled 2.png]]
- **Direct private connection between your datacenter and AWS**
- \(Not using VPN by default\)
- Better bandwidth, lower network costs
- Setting up Direct Connect \(Memorize it\!\)
    - Create virtual interface in the Direct Connect console
    - VPC console \-\> VPN connection \-\> Customer Gateway
    - Create Virtual private gateway
    - Attach Virtual Private Gateway to desired VPC
    - Select VPN Connections and create new VPN Connection
    - Select VPG and Customer Gateway
    - Once the VPN is available, set up the VPN on the customer gateway or firewall

