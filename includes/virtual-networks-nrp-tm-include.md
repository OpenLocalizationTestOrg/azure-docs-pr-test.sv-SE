## <a name="traffic-manager-profile"></a><span data-ttu-id="d2001-101">Traffic Manager-profilen</span><span class="sxs-lookup"><span data-stu-id="d2001-101">Traffic Manager Profile</span></span>
<span data-ttu-id="d2001-102">Aktivera tooendpoints för DNS i Azure och utanför Azure Traffic manager och dess underordnade endpoint-resursen.</span><span class="sxs-lookup"><span data-stu-id="d2001-102">Traffic manager and its child endpoint resource enable DNS routing tooendpoints in Azure and outside of Azure.</span></span> <span data-ttu-id="d2001-103">Sådana trafikfördelning styrs av routningsmetoder för principen.</span><span class="sxs-lookup"><span data-stu-id="d2001-103">Such traffic distribution is governed by routing  policy methods.</span></span> <span data-ttu-id="d2001-104">Traffic manager kan också endpoint hälsa toobe övervakas och trafik som distribueras på rätt sätt baserat på hello hälsotillståndet för en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2001-104">Traffic manager also allows endpoint health toobe monitored, and traffic diverted appropriately based on hello health of an endpoint.</span></span> 

| <span data-ttu-id="d2001-105">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d2001-105">Property</span></span> | <span data-ttu-id="d2001-106">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d2001-106">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d2001-107">**trafficRoutingMethod**</span><span class="sxs-lookup"><span data-stu-id="d2001-107">**trafficRoutingMethod**</span></span> |<span data-ttu-id="d2001-108">möjliga värden är *prestanda*, *viktat*, och *prioritet*</span><span class="sxs-lookup"><span data-stu-id="d2001-108">possible values are *Performance*, *Weighted*, and *Priority*</span></span> |
| <span data-ttu-id="d2001-109">**Det går**</span><span class="sxs-lookup"><span data-stu-id="d2001-109">**dnsConfig**</span></span> |<span data-ttu-id="d2001-110">FQDN för hello-profilen</span><span class="sxs-lookup"><span data-stu-id="d2001-110">FQDN for hello profile</span></span> |
| <span data-ttu-id="d2001-111">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="d2001-111">**Protocol**</span></span> |<span data-ttu-id="d2001-112">övervakning, möjliga värden är *HTTP* och *HTTPS*</span><span class="sxs-lookup"><span data-stu-id="d2001-112">monitoring protocol, possible values are *HTTP* and *HTTPS*</span></span> |
| <span data-ttu-id="d2001-113">**Port**</span><span class="sxs-lookup"><span data-stu-id="d2001-113">**Port**</span></span> |<span data-ttu-id="d2001-114">övervakning av port</span><span class="sxs-lookup"><span data-stu-id="d2001-114">monitoring port</span></span> |
| <span data-ttu-id="d2001-115">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="d2001-115">**Path**</span></span> |<span data-ttu-id="d2001-116">övervakningssökvägen</span><span class="sxs-lookup"><span data-stu-id="d2001-116">monitoring path</span></span> |
| <span data-ttu-id="d2001-117">**Slutpunkter**</span><span class="sxs-lookup"><span data-stu-id="d2001-117">**Endpoints**</span></span> |<span data-ttu-id="d2001-118">behållare för slutpunkt-resurser</span><span class="sxs-lookup"><span data-stu-id="d2001-118">container for endpoint resources</span></span> |

### <a name="endpoint"></a><span data-ttu-id="d2001-119">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="d2001-119">Endpoint</span></span>
<span data-ttu-id="d2001-120">En slutpunkt är en underordnad resurs för en Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="d2001-120">An endpoint is a child resource of a Traffic Manager Profile.</span></span> <span data-ttu-id="d2001-121">Representerar en tjänst eller användaren webbtrafik endpoint toowhich distribueras baserat på hello konfigurerade principen i hello Trafikhanterarprofil resurs.</span><span class="sxs-lookup"><span data-stu-id="d2001-121">It represents a service or web endpoint toowhich user traffic is distributed based on hello configured policy in hello Traffic Manager Profile resource.</span></span> 

| <span data-ttu-id="d2001-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d2001-122">Property</span></span> | <span data-ttu-id="d2001-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d2001-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d2001-124">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d2001-124">**Type**</span></span> |<span data-ttu-id="d2001-125">hello typ för hello slutpunkten möjliga värden är *Azure slutpunkt*, *extern slutpunkt*, och *kapslade slutpunkt*</span><span class="sxs-lookup"><span data-stu-id="d2001-125">hello type of hello endpoint, possible values are *Azure End point*, *External Endpoint*, and  *Nested Endpoint*</span></span> |
| <span data-ttu-id="d2001-126">**targetResourceId**</span><span class="sxs-lookup"><span data-stu-id="d2001-126">**targetResourceId**</span></span> |<span data-ttu-id="d2001-127">offentliga IP-adressen för en tjänst eller web slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2001-127">public IP address of a service or web endpoint.</span></span> <span data-ttu-id="d2001-128">Detta kan vara en Azure eller extern slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2001-128">This can be an Azure or external endpoint.</span></span> |
| <span data-ttu-id="d2001-129">**Vikt**</span><span class="sxs-lookup"><span data-stu-id="d2001-129">**Weight**</span></span> |<span data-ttu-id="d2001-130">slutpunkten vikten som används vid hantering av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="d2001-130">endpoint weight used in traffic management.</span></span> |
| <span data-ttu-id="d2001-131">**Prioritet**</span><span class="sxs-lookup"><span data-stu-id="d2001-131">**Priority**</span></span> |<span data-ttu-id="d2001-132">prioriteten för hello slutpunkt, används toodefine Redundansåtgärden</span><span class="sxs-lookup"><span data-stu-id="d2001-132">priority of hello endpoint, used toodefine a failover action</span></span> |

<span data-ttu-id="d2001-133">Exempel på Traffic Manager i Json-format:</span><span class="sxs-lookup"><span data-stu-id="d2001-133">Sample of Traffic Manager in Json format:</span></span> 

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


## <a name="additional-resources"></a><span data-ttu-id="d2001-134">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2001-134">Additional resources</span></span>
<span data-ttu-id="d2001-135">Läs [REST API-dokumentation för Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d2001-135">Read [REST API documentation for Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) for more information.</span></span>

