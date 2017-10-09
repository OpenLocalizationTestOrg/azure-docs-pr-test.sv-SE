## <a name="virtual-network"></a>Virtual Network
Virtuella nätverk (VNET) och undernät resurser hjälpa dig att definiera en säkerhetsgräns för arbetsbelastningar som körs i Azure. Ett virtuellt nätverk kännetecknas av en samling adressutrymmen, definierad som CIDR-block. 

> [!NOTE]
> Nätverksadministratörer är bekanta med CIDR-notering. Om du inte är bekant med CIDR, [mer information om den](http://whatismyipaddress.com/cidr).
> 
> 

![Virtuella nätverk med flera undernät](./media/resource-groups-networking/Figure4.png)

Vnet innehålla hello följande egenskaper.

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **addressSpace** |Samling av adressprefix som utgör hello VNet i CIDR-notation |192.168.0.0/16 |
| **undernät** |Samling av undernät som utgör hello VNet |Se [undernät](#Subnets) nedan. |
| **IP-adress** |IP-adress som tilldelats tooobject. Det här är en skrivskyddad egenskap. |104.42.233.77 |

### <a name="subnets"></a>Undernät
Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix. Nätverkskort läggas toosubnets och anslutna tooVMs som man har anslutning för olika arbetsbelastningar.

Undernät innehålla hello följande egenskaper. 

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **addressPrefix** |Enkel adressprefixet som utgör hello undernät i CIDR-notation |192.168.1.0/24 |
| **networkSecurityGroup** |NSG tillämpas toohello undernät |Se [NSG: er](#Network-Security-Group) |
| **Migreringstillståndet** |Routningstabellen tillämpas toohello undernät |Se [UDR](#Route-table) |
| **ipConfigurations** |Samling av IP-configruation objekt som används av nätverkskort anslutna toohello undernät |Se [UDR](#Route-table) |

Exempel VNet i JSON-format:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Ytterligare resurser
* Hämta mer information [VNet](../articles/virtual-network/virtual-networks-overview.md).
* Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) för Vnet.
* Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) för undernät.

