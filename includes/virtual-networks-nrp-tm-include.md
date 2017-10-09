## <a name="traffic-manager-profile"></a>Traffic Manager-profilen
Aktivera tooendpoints för DNS i Azure och utanför Azure Traffic manager och dess underordnade endpoint-resursen. Sådana trafikfördelning styrs av routningsmetoder för principen. Traffic manager kan också endpoint hälsa toobe övervakas och trafik som distribueras på rätt sätt baserat på hello hälsotillståndet för en slutpunkt. 

| Egenskap | Beskrivning |
| --- | --- |
| **trafficRoutingMethod** |möjliga värden är *prestanda*, *viktat*, och *prioritet* |
| **Det går** |FQDN för hello-profilen |
| **Protokoll** |övervakning, möjliga värden är *HTTP* och *HTTPS* |
| **Port** |övervakning av port |
| **Sökväg** |övervakningssökvägen |
| **Slutpunkter** |behållare för slutpunkt-resurser |

### <a name="endpoint"></a>Slutpunkt
En slutpunkt är en underordnad resurs för en Traffic Manager-profil. Representerar en tjänst eller användaren webbtrafik endpoint toowhich distribueras baserat på hello konfigurerade principen i hello Trafikhanterarprofil resurs. 

| Egenskap | Beskrivning |
| --- | --- |
| **Typ** |hello typ för hello slutpunkten möjliga värden är *Azure slutpunkt*, *extern slutpunkt*, och *kapslade slutpunkt* |
| **targetResourceId** |offentliga IP-adressen för en tjänst eller web slutpunkt. Detta kan vara en Azure eller extern slutpunkt. |
| **Vikt** |slutpunkten vikten som används vid hantering av nätverkstrafik. |
| **Prioritet** |prioriteten för hello slutpunkt, används toodefine Redundansåtgärden |

Exempel på Traffic Manager i Json-format: 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a>Ytterligare resurser
Läs [REST API-dokumentation för Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) för mer information.

