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

- inside *C:\inetpub\wwwroot* create a Default.html file containing web content.
<img src="/pictures/domain1.png" title="routing a domain"  width="1000">

### Local DNS

#### Setting up the Domain

- create *dns-server* inside *new-network/subnetA*. Install *Active Directory Domain Services*.
<img src="/pictures/localdns.png" title="local dns"  width="1000">

- promote server to a domain controller. Leave defaults
<img src="/pictures/localdns1.png" title="local dns"  width="1000">
<img src="/pictures/localdns10.png" title="local dns"  width="1000">
<img src="/pictures/localdns11.png" title="local dns"  width="1000">
<img src="/pictures/localdns12.png" title="local dns"  width="1000">

#### Setting up the Web Server

- create *web-server* inside *new-network/subnetB*. Install *IIS*.
<img src="/pictures/webserver.png" title="web server"  width="1000">

- from *dns-server*, reach the site hosted in *web-server* using its private IP address
<img src="/pictures/webserver1.png" title="web server"  width="1000">

#### Using the DNS Server

- in *new-network*, use custom DNS Server. User *dns-server* private IP
<img src="/pictures/dnsserver.png" title="dns server"  width="1000">

- in *dns-server*, go to tools / DNS
<img src="/pictures/dnsserver1.png" title="dns server"  width="1000">

- in *dns-server*, create a new host. Use private IP of *web-server*
<img src="/pictures/dnsserver4.png" title="dns server"  width="1000">

- in *dns-server*, use the FQDN *web-server.alexei.privatedomain* to connect to the site
<img src="/pictures/dnsserver5.png" title="dns server"  width="1000">

- you can also create another hostname and still be able to query the web server
<img src="/pictures/dnsserver6.png" title="dns server"  width="1000">
<img src="/pictures/dnsserver7.png" title="dns server"  width="1000">

#### Azure Private DNS 

- in *new-network*, set custom again
<img src="/pictures/privatedns0.png" title="private dns"  width="1000">

- in *dns-server*, install IIS and create a default.html file in *wwwroot*
<img src="/pictures/privatedns.png" title="private dns"  width="1000">

- the same url http://dns-server.alexei.privatedomain/default.html cannot be reached from the *web-server*
<img src="/pictures/privatedns1.png" title="private dns"  width="1000">
