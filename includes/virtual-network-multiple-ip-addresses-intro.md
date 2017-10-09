> [!div class="op_single_selector"]
> * [Portalen](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [CLI 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Mall](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Azure virtuell dator (VM) innehåller ett eller flera nätverksgränssnitt (NIC) kopplade tooit. Alla nätverkskort kan ha en eller flera statiska eller dynamiska offentliga och privata IP-adresser tilldelade tooit. Tilldela flera IP-kan adresser tooa VM hello följande funktioner:

* Du kan hantera flera webbplatser eller tjänster med olika IP-adresser och SSL-certifikat på en enda server.
* Du kan konfigurera den som en virtuell nätverksenhet, t.ex. en brandvägg eller belastningsutjämnare.
* Hej möjlighet tooadd någon hello privata IP-adresser för någon av hello nätverkskort tooan Azure belastningsutjämnare backend-adresspool. I hello tidigare endast hello primära IP-adress för hello primära nätverkskortet kunde läggas till tooa backend-adresspool. toolearn mer om hur tooload Utjämna flera IP-konfigurationer, läsa hello [flera IP-konfigurationer för belastningsutjämning](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

Varje nätverkskort anslutet tooa VM innehåller ett eller flera IP-konfigurationer som är associerade med tooit. Varje konfiguration tilldelas en statisk eller dynamisk privat IP-adress. Varje konfiguration kan också ha en offentlig IP-adress resurs som är associerad tooit. En offentlig IP-adressresurs har antingen en dynamiska eller statiska offentliga IP-adress som tilldelats tooit. toolearn mer om IP-adresser i Azure kan läsa hello [IP-adresser i Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) artikel. 

Det finns en gräns toohow många privata IP-adresser tilldelas tooa NIC. Det finns också en gräns toohow många offentliga IP-adresser som kan användas i en Azure-prenumeration. Se hello [Azure begränsar](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikeln för information.
