## <a name="application-gateway"></a><span data-ttu-id="3f99f-101">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="3f99f-101">Application Gateway</span></span>
<span data-ttu-id="3f99f-102">Programgateway tillhandahåller en lösning baserad på layer 7 belastningsutjämning för Azure-hanterad HTTP av belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="3f99f-102">Application Gateway provides an Azure-managed HTTP load balancing solution based on layer 7 load balancing.</span></span> <span data-ttu-id="3f99f-103">Belastningsutjämning för program kan hello användningen av routningsregler för nätverkstrafik baserat på HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f99f-103">Application load balancing allows hello use of routing rules for network traffic based on HTTP.</span></span> 
<BR>

| <span data-ttu-id="3f99f-104">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3f99f-104">Property</span></span> | <span data-ttu-id="3f99f-105">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f99f-105">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3f99f-106">**backendAddressPools**</span><span class="sxs-lookup"><span data-stu-id="3f99f-106">**backendAddressPools**</span></span> |<span data-ttu-id="3f99f-107">hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="3f99f-107">hello list of IP addresses of hello back end servers.</span></span> <span data-ttu-id="3f99f-108">hello IP-adresser anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP eller privat IP</span><span class="sxs-lookup"><span data-stu-id="3f99f-108">hello IP addresses listed should either belong toohello virtual network subnet, or should be a public IP/VIP or private IP</span></span> |
| <span data-ttu-id="3f99f-109">**backendHttpSettingsCollection**</span><span class="sxs-lookup"><span data-stu-id="3f99f-109">**backendHttpSettingsCollection**</span></span> |<span data-ttu-id="3f99f-110">Varje pool har inställningar som port och protokoll cookie baserat tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="3f99f-110">Every pool has settings like port, protocol, and cookie based affinity.</span></span> <span data-ttu-id="3f99f-111">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello pool</span><span class="sxs-lookup"><span data-stu-id="3f99f-111">These settings are tied tooa pool and are applied tooall servers within hello pool</span></span> |
| <span data-ttu-id="3f99f-112">**frontendPorts**</span><span class="sxs-lookup"><span data-stu-id="3f99f-112">**frontendPorts**</span></span> |<span data-ttu-id="3f99f-113">Den här porten är hello offentliga öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="3f99f-113">This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="3f99f-114">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar</span><span class="sxs-lookup"><span data-stu-id="3f99f-114">Traffic hits this port, and then gets redirected tooone of hello back end servers</span></span> |
| <span data-ttu-id="3f99f-115">**httpListeners**</span><span class="sxs-lookup"><span data-stu-id="3f99f-115">**httpListeners**</span></span> |<span data-ttu-id="3f99f-116">Lyssnare har en klientdelsport, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning)</span><span class="sxs-lookup"><span data-stu-id="3f99f-116">Listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload)</span></span> |
| <span data-ttu-id="3f99f-117">**requestRoutingRules**</span><span class="sxs-lookup"><span data-stu-id="3f99f-117">**requestRoutingRules**</span></span> |<span data-ttu-id="3f99f-118">hello regeln Binder hello-lyssnare och hello server serverdelspool och definierar vilken serverdel server pool hello trafik ska dirigeras.</span><span class="sxs-lookup"><span data-stu-id="3f99f-118">hello rule binds hello listener and hello back end server pool and defines which back end server pool hello traffic should be directed.</span></span> <span data-ttu-id="3f99f-119">För närvarande fungerar bara som resursallokering</span><span class="sxs-lookup"><span data-stu-id="3f99f-119">Currently works only as Round-robin</span></span> |

<span data-ttu-id="3f99f-120">Exempel på en programmall gateway Json:</span><span class="sxs-lookup"><span data-stu-id="3f99f-120">Example of an application gateway Json template:</span></span>

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


### <a name="additional-resources"></a><span data-ttu-id="3f99f-121">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f99f-121">Additional resources</span></span>
<span data-ttu-id="3f99f-122">Läs [ Programgateway REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3f99f-122">Read [ application gateway REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) for more information.</span></span>

