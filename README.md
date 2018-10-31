# Deploy up to 6 VM-Series Firewalls with an Application Gateway and up to 6 web servers fronted by an internal Load Balancer.

[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjasonmeurer%2Fazure-appgw-2fw%2Fmaster%2Fazuredeploy.json)

<img src="https://raw.githubusercontent.com/jasonmeurer/azure-appgw-2fw/master/diagram.png"/>

**Can be deployed to a New or Existing Resource Group and New or Existing VNET**

When deploying into an existing VNET, the Subnet Names and Prefixes must match the existing VNET.

The Firewalls will be deployed with Standard SKU Public IP addresses and Managed Disks.  Standard SKU PIPs were chosen to support the use of a Standard SKU Load Balancer should the design warrant.

While bootstrapping is not required, sample Bootstrap File and Init-cfg.txt files have been included in this repository.  
If Bootstrapping is not utilized, set the following parameters to None or leave blank.

[Bootstrap the VM-Series Firewall in Azure](https://www.paloaltonetworks.com/documentation/81/virtualization/virtualization/bootstrap-the-vm-series-firewall/bootstrap-the-vm-series-firewall-in-azure)

* customStorageAccount
* customAccessKey
* customFileShare
* customShareDirectory (Not required even if bootstrapping)

1. The Bootstrap file contains a user account u:pandemo p:demopassword.  
2. The Bootstrap file contains two objects that should be udpdated post deployment.  
   - The Untrust IP Address and an inside Load Balancer placeholder.
3. The Bootstrap file contains a route to the inside Subnets.  If the default subnet is not used, please update accordingly.
	

**Documentation**
* Release Notes: Included in this repository.
* About the [VM-Series Firewall for Azure](https://azure.paloaltonetworks.com)
