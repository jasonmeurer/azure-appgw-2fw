# Using Deploy up to 6 VM-Series Firewalls with an Application Gateway

[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjasonmeurer%2Fazure-appgw-2fw%2Fmaster%2Fazuredeploy.json)

**Can be deployed to a New or Existing Resource Group and New or Existing VNET**

When deploying into an existing VNET, the Subnet Names and Prefixes must match the existing VNET.

The Firewalls will be deployed with Standard SKU Public IP addresses and Managed Disks.  Standard SKU PIPs were chosen to support the use of a Standard SKU Load Balancer should the design warrant.

While bootstrapping is not required, sample Bootstrap File and Init-cfg.txt files have been included in this repository.  If Bootstrapping is not utilized, see the following parameters to None or leave blank.
[Bootstrap the VM-Series Firewall in Azure](https://www.paloaltonetworks.com/documentation/81/virtualization/virtualization/bootstrap-the-vm-series-firewall/bootstrap-the-vm-series-firewall-in-azure)
	* customStorageAccount
	* customAccessKey
	* customFileShare
	* customShareDirectory (Not required even if bootstrapping)

The Bootstrap file contains a user account u:pandemo p:demopassword.  
The Bootstrap file contains two objects that should be udpdate post deployment.  The Untrust IP Address and an inside Load Balancer placeholder.

**Documentation**
* Release Notes: Included in this repository.
* About the [VM-Series Firewall for Azure](https://azure.paloaltonetworks.com)



Instructions

0. Deploy the template, link below.
1. Make note of Internal LB - LoadBalancerFrontend IP
2. Make note of Firewall Unrust IP - Eth1
3. If SSH password was used, ssh to the firewall Public IP to set an admin password.
	* configure
	* set mgt-config users "insert admin name" password
	* commit
4. HTTPS to the Firewall's Public IP and Import/Load - fwconfig.xml
	* Adapted from Step 3 here. [Azure Template Deployment Proceedure](https://www.paloaltonetworks.com/documentation/71/virtualization/virtualization/set-up-the-vm-series-firewall-in-azure/start-using-the-vm-series-azure-application-gateway-template#_37860)
5. Update firewall-untrust-IP and internal-load-balancer-IP objects to match recorded addresses in steps 1 and 2 and commit.
6. If default subnets where not used, update the corresponding objects and Virtual Routers accordingly and commit.
7. Following the commits, credentials may be reset, use pandemo:demopassword to access the FW.
8. Relavent links
	* http://appGWip/wordpress
	* http://appGWip/sql-attack.html
		* Used for initiating traffic from Web to DB for visibility in the FW Monitor.
		* Once flows are verified, enable Threat prevention on the "eastwestpol" and relaunce the SQL attack.  Check the Monitor to validate the block.
	* http://appGWip/showheaders.php
		* Useful for troubleshooting XFF received by the web server.
9. Following along demo outlined here substituting the Application Gateway IP for the web server reference and keep in mind that you have more than one firewall to Monitor.
[Demo Guide](https://github.com/PaloAltoNetworks/azure/blob/master/two-tier-sample/Azure_ARM_template_deployment_guide.pdf)

This ARM template deploys two VM-Series firewalls between a pair of Azure load balancers. The external load balancer is an Azure Application Gateway (a web load balancer) that also serves as the Internet facing gateway, which  receives traffic and distributes it to the VM-Series firewalls. The firewalls enforce security policies to protect your workloads, and send the allowed traffic to the internal load balancer which is an Azure Load Balancer (Layer 4) that load balances across a pair of sample Apache web servers.  The ILB is utilizes a Standard Preview which can be used with HA Ports for Outbound and East/West traffic.

**Documentation**
* Release Notes: Included in this repository.
* About the [VM-Series Firewall for Azure](https://azure.paloaltonetworks.com)

