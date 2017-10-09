## <a name="network-security-group"></a><span data-ttu-id="34564-101">Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="34564-101">Network Security Group</span></span>
<span data-ttu-id="34564-102">NSG-resurs gör det möjligt för hello skapa säkerhetsgräns för arbetsbelastningar, genom att implementera Tillåt och neka regler.</span><span class="sxs-lookup"><span data-stu-id="34564-102">An NSG resource enables hello creation of security boundary for workloads, by implementing allow and deny rules.</span></span> <span data-ttu-id="34564-103">Dessa regler kan tillämpas tooa VM, ett nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="34564-103">Such rules can be applied tooa VM, a NIC, or a subnet.</span></span>

| <span data-ttu-id="34564-104">Egenskap</span><span class="sxs-lookup"><span data-stu-id="34564-104">Property</span></span> | <span data-ttu-id="34564-105">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="34564-105">Description</span></span> | <span data-ttu-id="34564-106">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="34564-106">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="34564-107">**undernät**</span><span class="sxs-lookup"><span data-stu-id="34564-107">**subnets**</span></span> |<span data-ttu-id="34564-108">Lista med undernät-ID: n hello NSG tillämpas.</span><span class="sxs-lookup"><span data-stu-id="34564-108">List of subnet ids hello NSG is applied to.</span></span> |<span data-ttu-id="34564-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-XXXXXXXXXXXX/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd</span><span class="sxs-lookup"><span data-stu-id="34564-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span></span> |
| <span data-ttu-id="34564-110">**securityRules**</span><span class="sxs-lookup"><span data-stu-id="34564-110">**securityRules**</span></span> |<span data-ttu-id="34564-111">Lista över säkerhetsregler som utgör hello NSG</span><span class="sxs-lookup"><span data-stu-id="34564-111">List of security rules that make up hello NSG</span></span> |<span data-ttu-id="34564-112">Se [säkerhetsregeln](#Security-rule) nedan</span><span class="sxs-lookup"><span data-stu-id="34564-112">See [Security rule](#Security-rule) below</span></span> |
| <span data-ttu-id="34564-113">**defaultSecurityRules**</span><span class="sxs-lookup"><span data-stu-id="34564-113">**defaultSecurityRules**</span></span> |<span data-ttu-id="34564-114">Lista över säkerhet standardregler finns i varje NSG</span><span class="sxs-lookup"><span data-stu-id="34564-114">List of default security rules present in every NSG</span></span> |<span data-ttu-id="34564-115">Se [standard säkerhetsregler](#Default-security-rules) nedan</span><span class="sxs-lookup"><span data-stu-id="34564-115">See [Default security rules](#Default-security-rules) below</span></span> |

* <span data-ttu-id="34564-116">**Säkerhetsregeln** -en NSG kan ha flera säkerhetsregler som definierats.</span><span class="sxs-lookup"><span data-stu-id="34564-116">**Security rule** - An NSG can have multiple security rules defined.</span></span> <span data-ttu-id="34564-117">Varje regel kan tillåta eller neka olika typer av trafik.</span><span class="sxs-lookup"><span data-stu-id="34564-117">Each rule can allow or deny different types of traffic.</span></span>

### <a name="security-rule"></a><span data-ttu-id="34564-118">Säkerhetsregeln</span><span class="sxs-lookup"><span data-stu-id="34564-118">Security rule</span></span>
<span data-ttu-id="34564-119">En säkerhetsregel är en underordnad resurs som tillhör en NSG som innehåller hello egenskaper nedan.</span><span class="sxs-lookup"><span data-stu-id="34564-119">A security rule is a child resource of an NSG containing hello properties below.</span></span>

| <span data-ttu-id="34564-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="34564-120">Property</span></span> | <span data-ttu-id="34564-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="34564-121">Description</span></span> | <span data-ttu-id="34564-122">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="34564-122">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="34564-123">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="34564-123">**description**</span></span> |<span data-ttu-id="34564-124">Beskrivning för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-124">Description for hello rule</span></span> |<span data-ttu-id="34564-125">Tillåt inkommande trafik för alla virtuella datorer i undernät X</span><span class="sxs-lookup"><span data-stu-id="34564-125">Allow inbound traffic for all VMs in subnet X</span></span> |
| <span data-ttu-id="34564-126">**protokollet**</span><span class="sxs-lookup"><span data-stu-id="34564-126">**protocol**</span></span> |<span data-ttu-id="34564-127">Protokollet toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-127">Protocol toomatch for hello rule</span></span> |<span data-ttu-id="34564-128">TCP, UDP eller *</span><span class="sxs-lookup"><span data-stu-id="34564-128">TCP, UDP, or *</span></span> |
| <span data-ttu-id="34564-129">**sourcePortRange**</span><span class="sxs-lookup"><span data-stu-id="34564-129">**sourcePortRange**</span></span> |<span data-ttu-id="34564-130">Källan port intervallet toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-130">Source port range toomatch for hello rule</span></span> |<span data-ttu-id="34564-131">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="34564-131">80, 100-200, *</span></span> |
| <span data-ttu-id="34564-132">**destinationPortRange**</span><span class="sxs-lookup"><span data-stu-id="34564-132">**destinationPortRange**</span></span> |<span data-ttu-id="34564-133">Mål-port intervallet toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-133">Destination port range toomatch for hello rule</span></span> |<span data-ttu-id="34564-134">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="34564-134">80, 100-200, *</span></span> |
| <span data-ttu-id="34564-135">**sourceAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="34564-135">**sourceAddressPrefix**</span></span> |<span data-ttu-id="34564-136">Källan adress prefixet toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-136">Source address prefix toomatch for hello rule</span></span> |<span data-ttu-id="34564-137">10.10.10.1 10.10.10.0/24 VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="34564-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="34564-138">**destinationAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="34564-138">**destinationAddressPrefix**</span></span> |<span data-ttu-id="34564-139">Mål-adress prefixet toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-139">Destination address prefix toomatch for hello rule</span></span> |<span data-ttu-id="34564-140">10.10.10.1 10.10.10.0/24 VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="34564-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="34564-141">**riktning**</span><span class="sxs-lookup"><span data-stu-id="34564-141">**direction**</span></span> |<span data-ttu-id="34564-142">Riktningen för trafik toomatch för hello regel</span><span class="sxs-lookup"><span data-stu-id="34564-142">Direction of traffic toomatch for hello rule</span></span> |<span data-ttu-id="34564-143">inkommande eller utgående</span><span class="sxs-lookup"><span data-stu-id="34564-143">inbound or outbound</span></span> |
| <span data-ttu-id="34564-144">**prioritet**</span><span class="sxs-lookup"><span data-stu-id="34564-144">**priority**</span></span> |<span data-ttu-id="34564-145">Prioritet för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="34564-145">Priority for hello rule.</span></span> <span data-ttu-id="34564-146">Reglerna kontrolleras i prioritsordning, när en regel testas inga fler regler för matchning.</span><span class="sxs-lookup"><span data-stu-id="34564-146">Rules are checked int he order of priority, once a rule applies, no more rules are tested for matching.</span></span> |<span data-ttu-id="34564-147">10, 100, 65000</span><span class="sxs-lookup"><span data-stu-id="34564-147">10, 100, 65000</span></span> |
| <span data-ttu-id="34564-148">**åtkomst**</span><span class="sxs-lookup"><span data-stu-id="34564-148">**access**</span></span> |<span data-ttu-id="34564-149">Typ av åtkomst tooapply om hello regeln matchar</span><span class="sxs-lookup"><span data-stu-id="34564-149">Type of access tooapply if hello rule matches</span></span> |<span data-ttu-id="34564-150">tillåt eller neka</span><span class="sxs-lookup"><span data-stu-id="34564-150">allow or deny</span></span> |

<span data-ttu-id="34564-151">Exempel NSG i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="34564-151">Sample NSG in JSON format:</span></span>

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a><span data-ttu-id="34564-152">Standardsäkerhetsregler</span><span class="sxs-lookup"><span data-stu-id="34564-152">Default security rules</span></span>

<span data-ttu-id="34564-153">Standardregler för säkerhet har hello samma egenskaper som är tillgängliga i säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="34564-153">Default security rules have hello same properties available in security rules.</span></span> <span data-ttu-id="34564-154">De finns tooprovide grundläggande anslutning mellan resurser som har toothem NSG: er som används.</span><span class="sxs-lookup"><span data-stu-id="34564-154">They exist tooprovide basic connectivity between resources that have NSGs applied toothem.</span></span> <span data-ttu-id="34564-155">Kontrollera att du vet vilken [standard säkerhetsregler](../articles/virtual-network/virtual-networks-nsg.md#default-rules) finns.</span><span class="sxs-lookup"><span data-stu-id="34564-155">Make sure you know which [default security rules](../articles/virtual-network/virtual-networks-nsg.md#default-rules) exist.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="34564-156">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="34564-156">Additional resources</span></span>
* <span data-ttu-id="34564-157">Hämta mer information [NSG: er](../articles/virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="34564-157">Get more information about [NSGs](../articles/virtual-network/virtual-networks-nsg.md).</span></span>
* <span data-ttu-id="34564-158">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="34564-158">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) for NSGs.</span></span>
* <span data-ttu-id="34564-159">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) för säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="34564-159">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) for security rules.</span></span>
