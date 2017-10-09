# Översikt
## [Virtuella nätverk](virtual-networks-overview.md)
## [Användardefinierade vägar och IP-vidarebefordran](virtual-networks-udr-overview.md)
## [Virtuell nätverkspeering](virtual-network-peering-overview.md)
## [Verksamhetskontinuitet](virtual-network-disaster-recovery-guidance.md)
## [Vanliga frågor och svar](virtual-networks-faq.md)
## [IP-adress](virtual-network-ip-addresses-overview-arm.md)
## Klassisk
### [IP-adress](virtual-network-ip-addresses-overview-classic.md)
### [Listor för åtkomstkontroll](virtual-networks-acl.md)

# Kom igång
## [Skapa ditt första virtuella nätverk](virtual-network-get-started-vnet-subnet.md)

# Gör så här för att
## Planera och designa
### [Virtuella nätverk](virtual-network-vnet-plan-design-arm.md)
### [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md)

## Distribuera
### [Virtuella nätverk](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [CLI](virtual-networks-create-vnet-arm-cli.md)
#### [Mall](virtual-networks-create-vnet-arm-template-click.md)

### Nätverkssäkerhetsgrupper
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [CLI](virtual-networks-create-nsg-arm-cli.md)
#### [Mall](virtual-networks-create-nsg-arm-template.md)
#### Klassisk
##### [PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [CLI](virtual-networks-create-nsg-classic-cli.md)

### Användardefinierade vägar
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [CLI](virtual-network-create-udr-arm-cli.md)
#### [Mall](virtual-network-create-udr-arm-template.md)
#### Klassisk
##### [PowerShell](virtual-network-create-udr-classic-ps.md)
##### [CLI](virtual-network-create-udr-classic-cli.md)

### Virtuell nätverkspeering
#### [Samma distributionsmodell – samma prenumeration](virtual-network-create-peering.md)
#### [Samma distributionsmodell – olika prenumerationer](create-peering-different-subscriptions.md)
#### [Olika distributionsmodeller – samma prenumeration](create-peering-different-deployment-models.md)
#### [Olika distributionsmodeller – olika prenumerationer](create-peering-different-deployment-models-subscriptions.md)

### Virtuella datorer
#### Skapa en virtuell dator med en statisk offentlig IP-adress
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [CLI](virtual-network-deploy-static-pip-arm-cli.md)
##### [Mall](virtual-network-deploy-static-pip-arm-template.md)
##### Klassisk
###### [PowerShell](virtual-networks-reserved-public-ip.md)

#### Skapa en virtuell dator med en statisk privat IP-adress
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [CLI](virtual-networks-static-private-ip-arm-cli.md)
##### Klassisk
###### [Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [CLI](virtual-networks-static-private-ip-classic-cli.md)

#### Skapa en virtuell dator med flera nätverksgränssnitt
##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### Klassisk
###### [PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [CLI](virtual-network-deploy-multinic-classic-cli.md)

#### Skapa en virtuell dator med flera IP-adresser
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [CLI](virtual-network-multiple-ip-addresses-cli.md)
##### [Mall](virtual-network-multiple-ip-addresses-template.md)

#### [Skapa en virtuell dator med accelererat nätverk](virtual-network-create-vm-accelerated-networking.md)

### Scenarier för anslutning
#### [Virtuella nätverk (VNet) tooVNet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Virtuella nätverk (Resource Manager) tooa VNet (klassisk)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNet tooon lokala nätverk (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNet tooon lokala nätverk (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Hybridnätverksarkitektur med hög tillgänglighet](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Säkerhetsscenarier
#### [Säkra nätverk med virtuella installationer](virtual-network-scenario-udr-gw-nva.md)
#### [DMZ mellan Azure och hello Internet](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Säkerhet i molntjänst och nätverk](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Enkel DMZ med NSG:er](virtual-networks-dmz-nsg-asm.md)
##### [DMZ med brandvägg och NSG:er](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ med brandvägg, UDR och NSG:er](virtual-networks-dmz-nsg-fw-udr-asm.md)
##### [Exempelprogram](virtual-networks-sample-app.md)

### Klassisk
#### [Virtuellt nätverk](create-virtual-network-classic.md)
##### [Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [CLI](virtual-networks-create-vnet-classic-cli.md)

## Konfigurera
### Virtuella datorer
#### [Lägg till eller ta bort nätverksgränssnitt](virtual-network-network-interface-vm.md)
#### [Namnmatchning för virtuella datorer och molntjänster](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Optimera nätverkets dataflöde](virtual-network-optimize-network-bandwidth.md)
#### [Visa och ändra värdnamn](virtual-networks-viewing-and-modifying-hostnames.md)
### Klassisk
#### Listor för åtkomstkontroll
##### [Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [PowerShell](virtual-networks-acl-powershell.md)

## Hantera
### [Virtuella nätverk](virtual-network-manage-network.md)
#### [Undernät](virtual-network-manage-subnet.md)
#### [Peering-sessioner](virtual-network-manage-peering.md)
#### Klassisk
##### [Nätverkskonfigurationsfil](virtual-networks-using-network-configuration-file.md)
##### [Migrera från en tillhörighetsgrupp tooa region](virtual-networks-migrate-to-regional-vnet.md)
### Nätverkssäkerhetsgrupper
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [CLI](virtual-network-manage-nsg-arm-cli.md)
#### [Loggar](virtual-network-nsg-manage-log.md)
### Nätverksgränssnitt (nätverkskort)
#### [Skapa, ändra eller ta bort nätverkskort](virtual-network-network-interface.md)
#### [Lägg till, ändra eller ta bort IP-adresser](virtual-network-network-interface-addresses.md)
### Virtuella datorer
#### [Flytta ett annat undernät för VM-tooa](virtual-networks-move-vm-role-to-subnet.md)
### [Offentliga IP-adresser](virtual-network-public-ip-address.md)

## Felsöka
### Nätverkssäkerhetsgrupper
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Vägar
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Testning av dataflöde](virtual-network-bandwidth-testing.md)
### [Det går inte att ta bort virtuella nätverk](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [VM tooVM-anslutningsproblem](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Referens
## [Kodexempel](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (klassisk)](/powershell/module/azure/)
## [Azure CLI](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (klassisk)](https://msdn.microsoft.com/library/jj157182.aspx)


# Relaterat
## [Virtual Machines](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Belastningsutjämnare](/azure/load-balancer/)
## [VPN-gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Resurser
## [Azure-översikt](https://azure.microsoft.com/roadmap/?category=networking)
## [Nätverksblogg](http://azure.microsoft.com/blog/topics/networking)
## [Nätverksforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Prissättning](https://azure.microsoft.com/pricing/details/virtual-network)
## [Priskalkylator](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
