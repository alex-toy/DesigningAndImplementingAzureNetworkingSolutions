# Designing and Implementing Azure Networking Solutions

The various objectives of this project are :

- Design, Implement, and Manage Hybrid Networking : How to connect your on-premises devices to Azure. We will see aspects such as developing Point-to-Site and Site-to-Site VPN connections. We will also see the various authentication aspects when it comes to VPN connections. We will understand the various aspects when it comes to Azure ExpressRoute.

- Design and Implement Core Networking Infrastructure : basics of networking. Create **Azure virtual networks** and **Azure virtual machines**. Working with DNS zones. Work with Azure virtual WAN.

- Design and Implement Routing : create User Defined routes for your subnets. Work with the various routing services - **Azure Load Balancer** , **Azure Application Gateway**, **Azure Front Door**, **Azure Traffic Manager**. **Azure Firewall service**. Working with basic networking filtering with the use of Network Security Groups. Monitor networks.

- Design and Implement Private Access to Azure Services : connect to Azure services. Use of **Service Endpoints** and Private endpoints. Secure connections against services such as Azure Web Apps and Azure Kubernetes.


## Designing and Implementing Core Networking Infrastructure

When you put a VM inside a VN, you need to make sure they belong to the same Azure Region

### Communication between VM in different networks

- create *demovm* inside *demo-network/default*. Install IIS on it. Add firewall rule for HTTP port 80
<img src="/pictures/demovm.png" title="create VM"  width="1000">

- create *secondaryvm* inside *demo-network/subnetA*
<img src="/pictures/secondaryvm.png" title="create VM"  width="1000">

Direct communication can occur between the machines because they are part of the same VN *demo-network*, through the private as well as the public IPs.

- create *stagingvm* inside *demo-network/default*. Install IIS on it. Add firewall rule for HTTP port 80
<img src="/pictures/demovm.png" title="create VM"  width="1000">

### Virtual Network Peering

The purpose is to be able to browse IIS in *stagingvm* from *testvm* via its private IP address.

- create *stagingvm* inside *staging-network/subnetA*. Add HTTP 80 as inbound port. Install IIS.
<img src="/pictures/vnp.png" title="virtual network peering"  width="1000">

- create *testvm* inside *test-network/subnetB*. 
<img src="/pictures/vnp1.png" title="virtual network peering"  width="1000">

- remove the public IP address of *stagingvm*. Check that you are not able to reach IIS from the internet anymore.
<img src="/pictures/vnp2.png" title="virtual network peering"  width="1000">
<img src="/pictures/vnp3.png" title="virtual network peering"  width="1000">

- add a virtual network peering
<img src="/pictures/vnp4.png" title="virtual network peering"  width="400">

- Check that you are able to reach IIS from *testvm* using *stagingvm*'s private IP
<img src="/pictures/vnp5.png" title="virtual network peering"  width="1000">

### Routing a Domain to a VM

- create *stagingvm*. Add HTTP 80 as inbound port. Install IIS.
<img src="/pictures/domain.png" title="routing a domain"  width="1000">

- inside *C:\inetpub\wwwroot* create a Default.html file containing web content.
<img src="/pictures/domain1.png" title="routing a domain"  width="1000">

### Local DNS

#### Setting up the domain

- create *dns-server*. Add HTTP 80 as inbound port. Install IIS.
<img src="/pictures/localdns.png" title="local dns"  width="1000">
