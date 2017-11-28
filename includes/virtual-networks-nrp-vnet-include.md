## <a name="virtual-network"></a><span data-ttu-id="e393a-101">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="e393a-101">Virtual Network</span></span>
<span data-ttu-id="e393a-102">Virtuella nätverk (VNET) och undernät resurser hjälpa dig att definiera en säkerhetsgräns för arbetsbelastningar som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="e393a-102">Virtual Networks (VNET) and subnets resources help define a security boundary for workloads running in Azure.</span></span> <span data-ttu-id="e393a-103">Ett virtuellt nätverk kännetecknas av en samling adressutrymmen, definierad som CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="e393a-103">A VNet is characterized by a collection of address spaces, defined as CIDR blocks.</span></span> 

> [!NOTE]
> <span data-ttu-id="e393a-104">Nätverksadministratörer är bekanta med CIDR-notering.</span><span class="sxs-lookup"><span data-stu-id="e393a-104">Network administrators are familiar with CIDR notation.</span></span> <span data-ttu-id="e393a-105">Om du inte är bekant med CIDR, [mer information om den](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="e393a-105">If you are not familiar with CIDR, [learn more about it](http://whatismyipaddress.com/cidr).</span></span>
> 
> 

![Virtuella nätverk med flera undernät](./media/resource-groups-networking/Figure4.png)

<span data-ttu-id="e393a-107">Vnet innehåller följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="e393a-107">VNets contain the following properties.</span></span>

| <span data-ttu-id="e393a-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e393a-108">Property</span></span> | <span data-ttu-id="e393a-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e393a-109">Description</span></span> | <span data-ttu-id="e393a-110">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="e393a-110">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e393a-111">**addressSpace**</span><span class="sxs-lookup"><span data-stu-id="e393a-111">**addressSpace**</span></span> |<span data-ttu-id="e393a-112">Samling av adressprefix som utgör VNet i CIDR-notation</span><span class="sxs-lookup"><span data-stu-id="e393a-112">Collection of address prefixes that make up the VNet in CIDR notation</span></span> |<span data-ttu-id="e393a-113">192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e393a-113">192.168.0.0/16</span></span> |
| <span data-ttu-id="e393a-114">**undernät**</span><span class="sxs-lookup"><span data-stu-id="e393a-114">**subnets**</span></span> |<span data-ttu-id="e393a-115">Samling av undernät som utgör VNet</span><span class="sxs-lookup"><span data-stu-id="e393a-115">Collection of subnets that make up the VNet</span></span> |<span data-ttu-id="e393a-116">Se [undernät](#Subnets) nedan.</span><span class="sxs-lookup"><span data-stu-id="e393a-116">see [subnets](#Subnets) below.</span></span> |
| <span data-ttu-id="e393a-117">**IP-adress**</span><span class="sxs-lookup"><span data-stu-id="e393a-117">**ipAddress**</span></span> |<span data-ttu-id="e393a-118">IP-adress som tilldelats objekt.</span><span class="sxs-lookup"><span data-stu-id="e393a-118">IP address assigned to object.</span></span> <span data-ttu-id="e393a-119">Det här är en skrivskyddad egenskap.</span><span class="sxs-lookup"><span data-stu-id="e393a-119">This is a read-only property.</span></span> |<span data-ttu-id="e393a-120">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="e393a-120">104.42.233.77</span></span> |

### <a name="subnets"></a><span data-ttu-id="e393a-121">Undernät</span><span class="sxs-lookup"><span data-stu-id="e393a-121">Subnets</span></span>
<span data-ttu-id="e393a-122">Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix.</span><span class="sxs-lookup"><span data-stu-id="e393a-122">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="e393a-123">Nätverkskorten kan läggas till i undernät och anslutna till virtuella datorer kan man har anslutning för olika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="e393a-123">NICs can be added to subnets, and connected to VMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="e393a-124">Undernät innehåller följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="e393a-124">Subnets contain the following properties.</span></span> 

| <span data-ttu-id="e393a-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e393a-125">Property</span></span> | <span data-ttu-id="e393a-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e393a-126">Description</span></span> | <span data-ttu-id="e393a-127">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="e393a-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e393a-128">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="e393a-128">**addressPrefix**</span></span> |<span data-ttu-id="e393a-129">Enkel adressprefixet som utgör undernät i CIDR-notation</span><span class="sxs-lookup"><span data-stu-id="e393a-129">Single address prefix that make up the subnet in CIDR notation</span></span> |<span data-ttu-id="e393a-130">192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="e393a-130">192.168.1.0/24</span></span> |
| <span data-ttu-id="e393a-131">**networkSecurityGroup**</span><span class="sxs-lookup"><span data-stu-id="e393a-131">**networkSecurityGroup**</span></span> |<span data-ttu-id="e393a-132">NSG tillämpas för undernätet</span><span class="sxs-lookup"><span data-stu-id="e393a-132">NSG applied to the subnet</span></span> |<span data-ttu-id="e393a-133">Se [NSG: er](#Network-Security-Group)</span><span class="sxs-lookup"><span data-stu-id="e393a-133">see [NSGs](#Network-Security-Group)</span></span> |
| <span data-ttu-id="e393a-134">**Migreringstillståndet**</span><span class="sxs-lookup"><span data-stu-id="e393a-134">**routeTable**</span></span> |<span data-ttu-id="e393a-135">Routningstabellen tillämpad på undernätet</span><span class="sxs-lookup"><span data-stu-id="e393a-135">Route table applied to the subnet</span></span> |<span data-ttu-id="e393a-136">Se [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="e393a-136">see [UDR](#Route-table)</span></span> |
| <span data-ttu-id="e393a-137">**ipConfigurations**</span><span class="sxs-lookup"><span data-stu-id="e393a-137">**ipConfigurations**</span></span> |<span data-ttu-id="e393a-138">Samling av IP-configruation objekt som används av nätverkskort som är anslutna till undernätet</span><span class="sxs-lookup"><span data-stu-id="e393a-138">Collection of IP configruation objects used by NICs connected to the subnet</span></span> |<span data-ttu-id="e393a-139">Se [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="e393a-139">see [UDR](#Route-table)</span></span> |

<span data-ttu-id="e393a-140">Exempel VNet i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="e393a-140">Sample VNet in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="e393a-141">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e393a-141">Additional resources</span></span>
* <span data-ttu-id="e393a-142">Hämta mer information [VNet](../articles/virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e393a-142">Get more information about [VNet](../articles/virtual-network/virtual-networks-overview.md).</span></span>
* <span data-ttu-id="e393a-143">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) för Vnet.</span><span class="sxs-lookup"><span data-stu-id="e393a-143">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) for VNets.</span></span>
* <span data-ttu-id="e393a-144">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) för undernät.</span><span class="sxs-lookup"><span data-stu-id="e393a-144">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) for Subnets.</span></span>

