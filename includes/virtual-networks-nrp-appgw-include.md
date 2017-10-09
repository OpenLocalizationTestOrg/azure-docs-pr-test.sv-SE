## <a name="application-gateway"></a>Application Gateway
Programgateway tillhandahåller en lösning baserad på layer 7 belastningsutjämning för Azure-hanterad HTTP av belastningsutjämning. Belastningsutjämning för program kan hello användningen av routningsregler för nätverkstrafik baserat på HTTP. 
<BR>

| Egenskap | Beskrivning |
| --- | --- |
| **backendAddressPools** |hello lista över IP-adresser för hello backend-servrar. hello IP-adresser anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP eller privat IP |
| **backendHttpSettingsCollection** |Varje pool har inställningar som port och protokoll cookie baserat tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello pool |
| **frontendPorts** |Den här porten är hello offentliga öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar |
| **httpListeners** |Lyssnare har en klientdelsport, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning) |
| **requestRoutingRules** |hello regeln Binder hello-lyssnare och hello server serverdelspool och definierar vilken serverdel server pool hello trafik ska dirigeras. För närvarande fungerar bara som resursallokering |

Exempel på en programmall gateway Json:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "location": {
          "type": "string",
          "metadata": {
            "description": "Location toodeploy to"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address prefix for hello Virtual Network"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/28",
          "metadata": {
            "description": "Subnet prefix"
          }
        },
        "skuName": {
          "type": "string",
          "allowedValues": [
            "Standard_Small",
            "Standard_Medium",
            "Standard_Large"
          ],
          "defaultValue": "Standard_Medium",
          "metadata": {
            "description": "Sku Name"
          }
        },
        "capacity": {
          "type": "int",
          "defaultValue": 2,
          "metadata": {
            "description": "Number of instances"
          }
        },
        "backendIpAddress1": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 1"
          }
        },
        "backendIpAddress2": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 2"
          }
        }
      },
      "variables": {
        "applicationGatewayName": "applicationGateway1",
        "publicIPAddressName": "publicIp1",
        "virtualNetworkName": "virtualNetwork1",
        "subnetName": "appGatewaySubnet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPRef": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "applicationGatewayID": "[resourceId('Microsoft.Network/applicationGateways',variables('applicationGatewayName'))]",
        "apiVersion": "2015-05-01-preview"
      },
      "resources": [
        {
          "apiVersion": "[variables('apiVersion')]",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIPAddressName')]",
          "location": "[parameters('location')]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic"
          }
        },
        {
          "apiVersion": "[variables('apiVersion')]",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[variables('subnetName')]",
                "properties": {
                  "addressPrefix": "[parameters('subnetPrefix')]"
                }
              }
            ]
          }
        },
        {
          "apiVersion": "[variables('apiVersion')]",
          "name": "[variables('applicationGatewayName')]",
          "type": "Microsoft.Network/applicationGateways",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/virtualNetworks/', variables    ('virtualNetworkName'))]",
            "[concat('Microsoft.Network/publicIPAddresses/', variables    ('publicIPAddressName'))]"
          ],
          "properties": {
            "sku": {
              "name": "[parameters('skuName')]",
              "tier": "Standard",
              "capacity": "[parameters('capacity')]"
            },
            "gatewayIPConfigurations": [
              {
                "name": "appGatewayIpConfig",
                "properties": {
                  "subnet": {
                    "id": "[variables('subnetRef')]"
                  }
                }
              }
            ],
            "frontendIPConfigurations": [
              {
                "name": "appGatewayFrontendIP",
                "properties": {
                  "PublicIPAddress": {
                    "id": "[variables('publicIPRef')]"
                  }
                }
              }
            ],
            "frontendPorts": [
              {
                "name": "appGatewayFrontendPort",
                "properties": {
                  "Port": 80
                }
              }
            ],
            "backendAddressPools": [
              {
                "name": "appGatewayBackendPool",
                "properties": {
                  "BackendAddresses": [
                    {
                      "IpAddress": "[parameters('backendIpAddress1')]"
                    },
                    {
                      "IpAddress": "[parameters('backendIpAddress2')]"
                    }
                  ]
                }
              }
            ],
            "backendHttpSettingsCollection": [
              {
                "name": "appGatewayBackendHttpSettings",
                "properties": {
                  "Port": 80,
                  "Protocol": "Http",
                  "CookieBasedAffinity": "Disabled"
                }
              }
            ],
            "httpListeners": [
              {
                "name": "appGatewayHttpListener",
                "properties": {
                  "FrontendIPConfiguration": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendIPConfigurations/appGatewayFrontendIP')]"
                  },
                  "FrontendPort": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendPorts/appGatewayFrontendPort')]"
                  },
                  "Protocol": "Http",
                  "SslCertificate": null
                }
              }
            ],
            "requestRoutingRules": [
              {
                "Name": "rule1",
                "properties": {
                  "RuleType": "Basic",
                  "httpListener": {
                    "id": "[concat(variables('applicationGatewayID'), '/    httpListeners/appGatewayHttpListener')]"
                  },
                  "backendAddressPool": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendAddressPools/appGatewayBackendPool')]"
                  },
                  "backendHttpSettings": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendHttpSettingsCollection/    appGatewayBackendHttpSettings')]"
                  }
                }
              }
            ]
          }
        }
      ]    
    }


### <a name="additional-resources"></a>Ytterligare resurser
Läs [ Programgateway REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) för mer information.

