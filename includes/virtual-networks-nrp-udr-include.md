## <a name="route-tables"></a>Vägtabeller
Väg tabellen resurser innehåller vägar toodefine hur trafiken flödar i Azure-infrastrukturen. Du kan använda användardefinierade vägar (UDR) toosend all trafik från ett visst undernät tooa virtuell installation, till exempel en brandvägg eller intrångsidentifiering identifiering-system (ID). Du kan associera en väg tabellen toosubnets. 

Routningstabeller innehålla hello följande egenskaper.

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **vägar** |Samling användardefinierade vägar i routningstabellen för hello |Se [användardefinierade vägar](#User-defined-routes) |
| **undernät** |Samling av routningstabellen för undernät hello används för|Se [undernät](#Subnets) |

### <a name="user-defined-routes"></a>Användardefinierade vägar
Du kan skapa udr: er toospecify där trafik ska skickas till, baserat på dess mål-adress. Du kan se en väg som hello gateway standarddefinition baserat på hello måladress för ett nätverkspaket.

Udr: er innehåller hello följande egenskaper. 

| Egenskap | Beskrivning | Exempelvärden |
| --- | --- | --- |
| **addressPrefix** |Adressprefixet eller fullständig IP-adress för hello mål |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Typ av enhet hello trafik skickas för|VirtualAppliance, VPN-Gateway, Internet |
| **nextHopIpAddress** |IP-adress för hello nästa hopp |192.168.1.4 |

Exempel routningstabellen i JSON-format:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Ytterligare resurser
* Hämta mer information [udr: er](../articles/virtual-network/virtual-networks-udr-overview.md).
* Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) för routningstabeller.
* Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) definierats vägar (udr: er) för användaren.

