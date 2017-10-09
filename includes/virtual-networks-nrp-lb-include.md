## <a name="load-balancer"></a><span data-ttu-id="637d4-101">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="637d4-101">Load Balancer</span></span>
<span data-ttu-id="637d4-102">En belastningsutjämnare används när du vill tooscale dina program.</span><span class="sxs-lookup"><span data-stu-id="637d4-102">A load balancer is used when you want tooscale your applications.</span></span> <span data-ttu-id="637d4-103">Vanliga distributionsscenarier omfattar program som körs på flera VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="637d4-103">Typical deployment scenarios involve applications running on multiple VM instances.</span></span> <span data-ttu-id="637d4-104">hello VM-instanser fronted av en belastningsutjämnare som hjälper toodistribute nätverk trafik toohello olika instanser.</span><span class="sxs-lookup"><span data-stu-id="637d4-104">hello VM instances are fronted by a load balancer that helps toodistribute network traffic toohello various instances.</span></span> 

![NIC på en enda virtuell dator](./media/resource-groups-networking/figure8.png)

| <span data-ttu-id="637d4-106">Egenskap</span><span class="sxs-lookup"><span data-stu-id="637d4-106">Property</span></span> | <span data-ttu-id="637d4-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="637d4-107">Description</span></span> |
| --- | --- |
| <span data-ttu-id="637d4-108">*frontendIPConfigurations*</span><span class="sxs-lookup"><span data-stu-id="637d4-108">*frontendIPConfigurations*</span></span> |<span data-ttu-id="637d4-109">en belastningsutjämnare kan innehålla en eller flera klientdelen IP-adresser, kallas även för en virtuell IP-adresser (VIP).</span><span class="sxs-lookup"><span data-stu-id="637d4-109">a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="637d4-110">IP-adresserna som fungerar som ingång för hello trafik och kan vara offentlig IP-adress eller privat IP</span><span class="sxs-lookup"><span data-stu-id="637d4-110">These IP addresses serve as ingress for hello traffic and can be public IP or private IP</span></span> |
| <span data-ttu-id="637d4-111">*backendAddressPools*</span><span class="sxs-lookup"><span data-stu-id="637d4-111">*backendAddressPools*</span></span> |<span data-ttu-id="637d4-112">Dessa är IP-adresser som är associerade med hello VM-nätverkskort toowhich belastningen ska distribueras</span><span class="sxs-lookup"><span data-stu-id="637d4-112">these are IP addresses associated with hello VM NICs toowhich load will be distributed</span></span> |
| <span data-ttu-id="637d4-113">*loadBalancingRules*</span><span class="sxs-lookup"><span data-stu-id="637d4-113">*loadBalancingRules*</span></span> |<span data-ttu-id="637d4-114">en regel för egenskapen mappar en given klientdelen IP-adress och port kombination tooa uppsättning serverdel IP-adresser och port-kombination.</span><span class="sxs-lookup"><span data-stu-id="637d4-114">a rule property maps a given front end IP and port combination tooa set of back end IP addresses and port combination.</span></span> <span data-ttu-id="637d4-115">Med en definition av en belastningsutjämningsresurs, kan du definiera flera regler för belastningsutjämning, varje regel reflektion av en kombination av en framför avslutas IP och port och tillbaka slut-IP och port som är kopplade till virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="637d4-115">With a single definition of a load balancer resource, you can define multiple load balancing rules, each rule reflecting a combination of a front end IP and port and back end IP and port associated with virtual machines.</span></span> <span data-ttu-id="637d4-116">hello regeln är en port i hello klientdelen poolen toomany virtuella datorer i hello serverdelspool</span><span class="sxs-lookup"><span data-stu-id="637d4-116">hello rule is one port in hello front end pool toomany virtual machines in hello back end pool</span></span> |
| <span data-ttu-id="637d4-117">*Avsökningar*</span><span class="sxs-lookup"><span data-stu-id="637d4-117">*Probes*</span></span> |<span data-ttu-id="637d4-118">avsökningar aktivera tookeep spåra hello hälsotillståndet hos VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="637d4-118">probes enable you tookeep track of hello health of VM instances.</span></span> <span data-ttu-id="637d4-119">Om en hälsoavsökningen misslyckas kommer hello virtuell datorinstans att tas bort från rotation automatiskt</span><span class="sxs-lookup"><span data-stu-id="637d4-119">If a health probe fails, hello virtual machine instance will be taken out of rotation automatically</span></span> |
| <span data-ttu-id="637d4-120">*inboundNatRules*</span><span class="sxs-lookup"><span data-stu-id="637d4-120">*inboundNatRules*</span></span> |<span data-ttu-id="637d4-121">NAT-regler som definierar hello inkommande trafik som passerar genom hello klientdelen IP och distribuerad toohello serverdel IP tooa specifik virtuell dator-instansen.</span><span class="sxs-lookup"><span data-stu-id="637d4-121">NAT rules defining hello inbound traffic flowing through hello front end IP and distributed toohello back end IP tooa specific virtual machine instance.</span></span> <span data-ttu-id="637d4-122">NAT-regel är en port på hello klientdelen poolen tooone virtuell dator i hello serverdelspool</span><span class="sxs-lookup"><span data-stu-id="637d4-122">NAT rule is one port in hello front end pool tooone virtual machine in hello back end pool</span></span> |

<span data-ttu-id="637d4-123">Exempel på mall för belastningsutjämnare i Json-format:</span><span class="sxs-lookup"><span data-stu-id="637d4-123">Example of load balancer template in Json format:</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location toodeploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
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
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a><span data-ttu-id="637d4-124">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="637d4-124">Additional resources</span></span>
<span data-ttu-id="637d4-125">Läs [belastningsutjämnare REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="637d4-125">Read [load balancer REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx) for more information.</span></span>

