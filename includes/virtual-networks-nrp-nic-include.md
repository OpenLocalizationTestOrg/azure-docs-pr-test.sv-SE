## <a name="nic"></a><span data-ttu-id="f24ae-101">NÄTVERKSKORT</span><span class="sxs-lookup"><span data-stu-id="f24ae-101">NIC</span></span>
<span data-ttu-id="f24ae-102">En nätverksresurs interface card (NIC) tillhandahåller anslutningen tooan befintliga undernät i ett VNet-resursen.</span><span class="sxs-lookup"><span data-stu-id="f24ae-102">A network interface card (NIC) resource provides network connectivity tooan existing subnet in a VNet resource.</span></span> <span data-ttu-id="f24ae-103">Även om du kan skapa ett nätverkskort som ett fristående objekt, måste du tooassociate den tooanother objektet tooactually anslutningsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f24ae-103">Although you can create a NIC as a stand alone object, you need tooassociate it tooanother object tooactually provide connectivity.</span></span> <span data-ttu-id="f24ae-104">Ett nätverkskort kan vara används tooconnect ett tooa VM-undernät, en offentlig IP-adress eller en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f24ae-104">A NIC can be used tooconnect a VM tooa subnet, a public IP address, or a load balancer.</span></span>  

| <span data-ttu-id="f24ae-105">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f24ae-105">Property</span></span> | <span data-ttu-id="f24ae-106">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f24ae-106">Description</span></span> | <span data-ttu-id="f24ae-107">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="f24ae-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f24ae-108">**virtualMachine**</span><span class="sxs-lookup"><span data-stu-id="f24ae-108">**virtualMachine**</span></span> |<span data-ttu-id="f24ae-109">VM hello NIC är associerad med.</span><span class="sxs-lookup"><span data-stu-id="f24ae-109">VM hello NIC is associated with.</span></span> |<span data-ttu-id="f24ae-110">/subscriptions/{GUID}/../microsoft.Compute/virtualMachines/vm1</span><span class="sxs-lookup"><span data-stu-id="f24ae-110">/subscriptions/{guid}/../Microsoft.Compute/virtualMachines/vm1</span></span> |
| <span data-ttu-id="f24ae-111">**MAC-adress**</span><span class="sxs-lookup"><span data-stu-id="f24ae-111">**macAddress**</span></span> |<span data-ttu-id="f24ae-112">MAC-adress för hello NIC</span><span class="sxs-lookup"><span data-stu-id="f24ae-112">MAC address for hello NIC</span></span> |<span data-ttu-id="f24ae-113">ett värde mellan 4 och 30</span><span class="sxs-lookup"><span data-stu-id="f24ae-113">any value between 4 and 30</span></span> |
| <span data-ttu-id="f24ae-114">**networkSecurityGroup**</span><span class="sxs-lookup"><span data-stu-id="f24ae-114">**networkSecurityGroup**</span></span> |<span data-ttu-id="f24ae-115">NSG som kopplats toohello NIC</span><span class="sxs-lookup"><span data-stu-id="f24ae-115">NSG associated toohello NIC</span></span> |<span data-ttu-id="f24ae-116">/subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1</span><span class="sxs-lookup"><span data-stu-id="f24ae-116">/subscriptions/{guid}/../Microsoft.Network/networkSecurityGroups/myNSG1</span></span> |
| <span data-ttu-id="f24ae-117">**dnsSettings**</span><span class="sxs-lookup"><span data-stu-id="f24ae-117">**dnsSettings**</span></span> |<span data-ttu-id="f24ae-118">DNS-inställningarna för hello NIC</span><span class="sxs-lookup"><span data-stu-id="f24ae-118">DNS settings for hello NIC</span></span> |<span data-ttu-id="f24ae-119">Se [PIP](#Public-IP-address)</span><span class="sxs-lookup"><span data-stu-id="f24ae-119">see [PIP](#Public-IP-address)</span></span> |

<span data-ttu-id="f24ae-120">Ett nätverkskort eller NIC, representerar ett nätverksgränssnitt som kan vara associerade tooa virtuell dator (VM).</span><span class="sxs-lookup"><span data-stu-id="f24ae-120">A Network Interface Card, or NIC, represents a network interface that can be associated tooa virtual machine (VM).</span></span> <span data-ttu-id="f24ae-121">En virtuell dator kan ha en eller flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="f24ae-121">A VM can have one or more NICs.</span></span>

![NIC på en enda virtuell dator](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a><span data-ttu-id="f24ae-123">IP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="f24ae-123">IP configurations</span></span>
<span data-ttu-id="f24ae-124">Är ett underordnat objekt med namnet **ipConfigurations** som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f24ae-124">NICs have a child object named **ipConfigurations** containing hello following properties:</span></span>

| <span data-ttu-id="f24ae-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f24ae-125">Property</span></span> | <span data-ttu-id="f24ae-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f24ae-126">Description</span></span> | <span data-ttu-id="f24ae-127">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="f24ae-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f24ae-128">**undernät**</span><span class="sxs-lookup"><span data-stu-id="f24ae-128">**subnet**</span></span> |<span data-ttu-id="f24ae-129">Undernät hello NIC är onnected till.</span><span class="sxs-lookup"><span data-stu-id="f24ae-129">Subnet hello NIC is onnected to.</span></span> |<span data-ttu-id="f24ae-130">/subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1</span><span class="sxs-lookup"><span data-stu-id="f24ae-130">/subscriptions/{guid}/../Microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1</span></span> |
| <span data-ttu-id="f24ae-131">**privateIPAddress**</span><span class="sxs-lookup"><span data-stu-id="f24ae-131">**privateIPAddress**</span></span> |<span data-ttu-id="f24ae-132">IP-adress för hello NIC i hello undernät</span><span class="sxs-lookup"><span data-stu-id="f24ae-132">IP address for hello NIC in hello subnet</span></span> |<span data-ttu-id="f24ae-133">10.0.0.8</span><span class="sxs-lookup"><span data-stu-id="f24ae-133">10.0.0.8</span></span> |
| <span data-ttu-id="f24ae-134">**privateIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="f24ae-134">**privateIPAllocationMethod**</span></span> |<span data-ttu-id="f24ae-135">IP-allokeringsmetod</span><span class="sxs-lookup"><span data-stu-id="f24ae-135">IP allocation method</span></span> |<span data-ttu-id="f24ae-136">Dynamiska eller statiska</span><span class="sxs-lookup"><span data-stu-id="f24ae-136">Dynamic or Static</span></span> |
| <span data-ttu-id="f24ae-137">**enableIPForwarding**</span><span class="sxs-lookup"><span data-stu-id="f24ae-137">**enableIPForwarding**</span></span> |<span data-ttu-id="f24ae-138">Om hello NIC kan användas för Routning</span><span class="sxs-lookup"><span data-stu-id="f24ae-138">Whether hello NIC can be used for routing</span></span> |<span data-ttu-id="f24ae-139">True eller false</span><span class="sxs-lookup"><span data-stu-id="f24ae-139">true or false</span></span> |
| <span data-ttu-id="f24ae-140">**primär**</span><span class="sxs-lookup"><span data-stu-id="f24ae-140">**primary**</span></span> |<span data-ttu-id="f24ae-141">Om hello NIC är hello primära nätverkskortet för hello VM</span><span class="sxs-lookup"><span data-stu-id="f24ae-141">Whether hello NIC is hello primary NIC for hello VM</span></span> |<span data-ttu-id="f24ae-142">True eller false</span><span class="sxs-lookup"><span data-stu-id="f24ae-142">true or false</span></span> |
| <span data-ttu-id="f24ae-143">**publicIPAddress**</span><span class="sxs-lookup"><span data-stu-id="f24ae-143">**publicIPAddress**</span></span> |<span data-ttu-id="f24ae-144">PIP som är associerade med hello NIC</span><span class="sxs-lookup"><span data-stu-id="f24ae-144">PIP associated with hello NIC</span></span> |<span data-ttu-id="f24ae-145">Se [DNS-inställningar](#DNS-settings)</span><span class="sxs-lookup"><span data-stu-id="f24ae-145">see [DNS Settings](#DNS-settings)</span></span> |
| <span data-ttu-id="f24ae-146">**loadBalancerBackendAddressPools**</span><span class="sxs-lookup"><span data-stu-id="f24ae-146">**loadBalancerBackendAddressPools**</span></span> |<span data-ttu-id="f24ae-147">Tillbaka slutet adresspooler hello NIC som är associerad med</span><span class="sxs-lookup"><span data-stu-id="f24ae-147">Back end address pools hello NIC is associated with</span></span> | |
| <span data-ttu-id="f24ae-148">**loadBalancerInboundNatRules**</span><span class="sxs-lookup"><span data-stu-id="f24ae-148">**loadBalancerInboundNatRules**</span></span> |<span data-ttu-id="f24ae-149">Inkommande belastningen belastningsutjämnaren NAT-regler hello NIC som är associerad med</span><span class="sxs-lookup"><span data-stu-id="f24ae-149">Inbound load balancer NAT rules hello NIC is associated with</span></span> | |

<span data-ttu-id="f24ae-150">Exempel offentliga IP-adressen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="f24ae-150">Sample public IP address in JSON format:</span></span>

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="f24ae-151">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f24ae-151">Additional resources</span></span>
* <span data-ttu-id="f24ae-152">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="f24ae-152">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) for NICs.</span></span>

