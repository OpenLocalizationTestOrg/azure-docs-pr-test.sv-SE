## <a name="network-security-group"></a><span data-ttu-id="027f4-101">Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="027f4-101">Network Security Group</span></span>
<span data-ttu-id="027f4-102">NSG-resurs kan skapa säkerhetsgräns för arbetsbelastningar, genom att implementera Tillåt och neka regler.</span><span class="sxs-lookup"><span data-stu-id="027f4-102">An NSG resource enables the creation of security boundary for workloads, by implementing allow and deny rules.</span></span> <span data-ttu-id="027f4-103">Dessa regler kan tillämpas på en virtuell dator, ett nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="027f4-103">Such rules can be applied to a VM, a NIC, or a subnet.</span></span>

| <span data-ttu-id="027f4-104">Egenskap</span><span class="sxs-lookup"><span data-stu-id="027f4-104">Property</span></span> | <span data-ttu-id="027f4-105">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="027f4-105">Description</span></span> | <span data-ttu-id="027f4-106">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="027f4-106">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="027f4-107">**undernät**</span><span class="sxs-lookup"><span data-stu-id="027f4-107">**subnets**</span></span> |<span data-ttu-id="027f4-108">Lista över undernät-ID NSG: N används.</span><span class="sxs-lookup"><span data-stu-id="027f4-108">List of subnet ids the NSG is applied to.</span></span> |<span data-ttu-id="027f4-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-XXXXXXXXXXXX/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd</span><span class="sxs-lookup"><span data-stu-id="027f4-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span></span> |
| <span data-ttu-id="027f4-110">**securityRules**</span><span class="sxs-lookup"><span data-stu-id="027f4-110">**securityRules**</span></span> |<span data-ttu-id="027f4-111">Lista över säkerhetsregler som utgör NSG: N</span><span class="sxs-lookup"><span data-stu-id="027f4-111">List of security rules that make up the NSG</span></span> |<span data-ttu-id="027f4-112">Se [säkerhetsregeln](#Security-rule) nedan</span><span class="sxs-lookup"><span data-stu-id="027f4-112">See [Security rule](#Security-rule) below</span></span> |
| <span data-ttu-id="027f4-113">**defaultSecurityRules**</span><span class="sxs-lookup"><span data-stu-id="027f4-113">**defaultSecurityRules**</span></span> |<span data-ttu-id="027f4-114">Lista över säkerhet standardregler finns i varje NSG</span><span class="sxs-lookup"><span data-stu-id="027f4-114">List of default security rules present in every NSG</span></span> |<span data-ttu-id="027f4-115">Se [standard säkerhetsregler](#Default-security-rules) nedan</span><span class="sxs-lookup"><span data-stu-id="027f4-115">See [Default security rules](#Default-security-rules) below</span></span> |

* <span data-ttu-id="027f4-116">**Säkerhetsregeln** -en NSG kan ha flera säkerhetsregler som definierats.</span><span class="sxs-lookup"><span data-stu-id="027f4-116">**Security rule** - An NSG can have multiple security rules defined.</span></span> <span data-ttu-id="027f4-117">Varje regel kan tillåta eller neka olika typer av trafik.</span><span class="sxs-lookup"><span data-stu-id="027f4-117">Each rule can allow or deny different types of traffic.</span></span>

### <a name="security-rule"></a><span data-ttu-id="027f4-118">Säkerhetsregeln</span><span class="sxs-lookup"><span data-stu-id="027f4-118">Security rule</span></span>
<span data-ttu-id="027f4-119">En säkerhetsregel är en underordnad resurs som tillhör en NSG som innehåller egenskaperna nedan.</span><span class="sxs-lookup"><span data-stu-id="027f4-119">A security rule is a child resource of an NSG containing the properties below.</span></span>

| <span data-ttu-id="027f4-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="027f4-120">Property</span></span> | <span data-ttu-id="027f4-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="027f4-121">Description</span></span> | <span data-ttu-id="027f4-122">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="027f4-122">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="027f4-123">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="027f4-123">**description**</span></span> |<span data-ttu-id="027f4-124">Beskrivning av regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-124">Description for the rule</span></span> |<span data-ttu-id="027f4-125">Tillåt inkommande trafik för alla virtuella datorer i undernät X</span><span class="sxs-lookup"><span data-stu-id="027f4-125">Allow inbound traffic for all VMs in subnet X</span></span> |
| <span data-ttu-id="027f4-126">**protokollet**</span><span class="sxs-lookup"><span data-stu-id="027f4-126">**protocol**</span></span> |<span data-ttu-id="027f4-127">Protokoll att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-127">Protocol to match for the rule</span></span> |<span data-ttu-id="027f4-128">TCP, UDP eller *</span><span class="sxs-lookup"><span data-stu-id="027f4-128">TCP, UDP, or *</span></span> |
| <span data-ttu-id="027f4-129">**sourcePortRange**</span><span class="sxs-lookup"><span data-stu-id="027f4-129">**sourcePortRange**</span></span> |<span data-ttu-id="027f4-130">Källportintervall att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-130">Source port range to match for the rule</span></span> |<span data-ttu-id="027f4-131">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="027f4-131">80, 100-200, *</span></span> |
| <span data-ttu-id="027f4-132">**destinationPortRange**</span><span class="sxs-lookup"><span data-stu-id="027f4-132">**destinationPortRange**</span></span> |<span data-ttu-id="027f4-133">Målportintervall att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-133">Destination port range to match for the rule</span></span> |<span data-ttu-id="027f4-134">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="027f4-134">80, 100-200, *</span></span> |
| <span data-ttu-id="027f4-135">**sourceAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="027f4-135">**sourceAddressPrefix**</span></span> |<span data-ttu-id="027f4-136">Källadress-prefix för att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-136">Source address prefix to match for the rule</span></span> |<span data-ttu-id="027f4-137">10.10.10.1 10.10.10.0/24 VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="027f4-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="027f4-138">**destinationAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="027f4-138">**destinationAddressPrefix**</span></span> |<span data-ttu-id="027f4-139">Måladress-prefix att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-139">Destination address prefix to match for the rule</span></span> |<span data-ttu-id="027f4-140">10.10.10.1 10.10.10.0/24 VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="027f4-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="027f4-141">**riktning**</span><span class="sxs-lookup"><span data-stu-id="027f4-141">**direction**</span></span> |<span data-ttu-id="027f4-142">Trafikriktning att matcha för regeln</span><span class="sxs-lookup"><span data-stu-id="027f4-142">Direction of traffic to match for the rule</span></span> |<span data-ttu-id="027f4-143">inkommande eller utgående</span><span class="sxs-lookup"><span data-stu-id="027f4-143">inbound or outbound</span></span> |
| <span data-ttu-id="027f4-144">**prioritet**</span><span class="sxs-lookup"><span data-stu-id="027f4-144">**priority**</span></span> |<span data-ttu-id="027f4-145">Prioritet för regeln.</span><span class="sxs-lookup"><span data-stu-id="027f4-145">Priority for the rule.</span></span> <span data-ttu-id="027f4-146">Reglerna kontrolleras i prioritsordning, när en regel testas inga fler regler för matchning.</span><span class="sxs-lookup"><span data-stu-id="027f4-146">Rules are checked int he order of priority, once a rule applies, no more rules are tested for matching.</span></span> |<span data-ttu-id="027f4-147">10, 100, 65000</span><span class="sxs-lookup"><span data-stu-id="027f4-147">10, 100, 65000</span></span> |
| <span data-ttu-id="027f4-148">**åtkomst**</span><span class="sxs-lookup"><span data-stu-id="027f4-148">**access**</span></span> |<span data-ttu-id="027f4-149">Typ av åtkomst som ska tillämpas om regeln matchar</span><span class="sxs-lookup"><span data-stu-id="027f4-149">Type of access to apply if the rule matches</span></span> |<span data-ttu-id="027f4-150">tillåt eller neka</span><span class="sxs-lookup"><span data-stu-id="027f4-150">allow or deny</span></span> |

<span data-ttu-id="027f4-151">Exempel NSG i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="027f4-151">Sample NSG in JSON format:</span></span>

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

### <a name="default-security-rules"></a><span data-ttu-id="027f4-152">Standardregler för säkerhet</span><span class="sxs-lookup"><span data-stu-id="027f4-152">Default security rules</span></span>

<span data-ttu-id="027f4-153">Standard säkerhetsregler ha samma egenskaper som är tillgängliga i säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="027f4-153">Default security rules have the same properties available in security rules.</span></span> <span data-ttu-id="027f4-154">De finns för att ge grundläggande anslutning mellan resurser som har NSG: er som tillämpas på.</span><span class="sxs-lookup"><span data-stu-id="027f4-154">They exist to provide basic connectivity between resources that have NSGs applied to them.</span></span> <span data-ttu-id="027f4-155">Kontrollera att du vet vilken [standard säkerhetsregler](../articles/virtual-network/virtual-networks-nsg.md#default-rules) finns.</span><span class="sxs-lookup"><span data-stu-id="027f4-155">Make sure you know which [default security rules](../articles/virtual-network/virtual-networks-nsg.md#default-rules) exist.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="027f4-156">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="027f4-156">Additional resources</span></span>
* <span data-ttu-id="027f4-157">Hämta mer information [NSG: er](../articles/virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="027f4-157">Get more information about [NSGs](../articles/virtual-network/virtual-networks-nsg.md).</span></span>
* <span data-ttu-id="027f4-158">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="027f4-158">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) for NSGs.</span></span>
* <span data-ttu-id="027f4-159">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) för säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="027f4-159">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) for security rules.</span></span>
