## <a name="public-ip-address"></a><span data-ttu-id="0befd-101">Offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="0befd-101">Public IP address</span></span>
<span data-ttu-id="0befd-102">En offentlig IP-adressresurs innehåller antingen en reserverad eller dynamiska Internetuppkopplad IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0befd-102">A public IP address resource provides either a reserved or dynamic Internet facing IP address.</span></span> <span data-ttu-id="0befd-103">Även om du kan skapa en offentlig IP-adress som ett fristående objekt, måste du tooassociate den tooanother objektet tooactually använda hello-adress.</span><span class="sxs-lookup"><span data-stu-id="0befd-103">Although you can create a public IP address as a stand alone object, you need tooassociate it tooanother object tooactually use hello address.</span></span> <span data-ttu-id="0befd-104">Du kan associera en offentlig belastningsutjämnare för IP-adress tooa, Programgateway eller en NIC tooprovide Internet access toothose resurser.</span><span class="sxs-lookup"><span data-stu-id="0befd-104">You can associate a public IP address tooa load balancer, application  gateway, or a NIC tooprovide Internet access toothose resources.</span></span>  

| <span data-ttu-id="0befd-105">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0befd-105">Property</span></span> | <span data-ttu-id="0befd-106">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0befd-106">Description</span></span> | <span data-ttu-id="0befd-107">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="0befd-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0befd-108">**publicIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="0befd-108">**publicIPAllocationMethod**</span></span> |<span data-ttu-id="0befd-109">Definierar om hello IP-adressen är *Statiska* eller *dynamiska*.</span><span class="sxs-lookup"><span data-stu-id="0befd-109">Defines if hello IP address is *static* or *dynamic*.</span></span> |<span data-ttu-id="0befd-110">statisk, dynamisk</span><span class="sxs-lookup"><span data-stu-id="0befd-110">static, dynamic</span></span> |
| <span data-ttu-id="0befd-111">**idleTimeoutInMinutes**</span><span class="sxs-lookup"><span data-stu-id="0befd-111">**idleTimeoutInMinutes**</span></span> |<span data-ttu-id="0befd-112">Definierar hello inaktiv timeout med ett standardvärde på 4 minuter.</span><span class="sxs-lookup"><span data-stu-id="0befd-112">Defines hello idle time out, with a default value of 4 minutes.</span></span> <span data-ttu-id="0befd-113">Om inga fler paket för en viss session har tagits emot inom den angivna tiden avslutas hello sessionen.</span><span class="sxs-lookup"><span data-stu-id="0befd-113">If no more packets for a given session is received within this time, hello session is terminated.</span></span> |<span data-ttu-id="0befd-114">ett värde mellan 4 och 30</span><span class="sxs-lookup"><span data-stu-id="0befd-114">any value between 4 and 30</span></span> |
| <span data-ttu-id="0befd-115">**IP-adress**</span><span class="sxs-lookup"><span data-stu-id="0befd-115">**ipAddress**</span></span> |<span data-ttu-id="0befd-116">IP-adress som tilldelats tooobject.</span><span class="sxs-lookup"><span data-stu-id="0befd-116">IP address assigned tooobject.</span></span> <span data-ttu-id="0befd-117">Det här är en skrivskyddad egenskap.</span><span class="sxs-lookup"><span data-stu-id="0befd-117">This is a read-only property.</span></span> |<span data-ttu-id="0befd-118">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="0befd-118">104.42.233.77</span></span> |

### <a name="dns-settings"></a><span data-ttu-id="0befd-119">DNS-inställningar</span><span class="sxs-lookup"><span data-stu-id="0befd-119">DNS settings</span></span>
<span data-ttu-id="0befd-120">Offentliga IP-adresser ha ett underordnat objekt med namnet **dnsSettings** som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0befd-120">Public IP addresses have a child object named **dnsSettings** containing hello following properties:</span></span>

| <span data-ttu-id="0befd-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0befd-121">Property</span></span> | <span data-ttu-id="0befd-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0befd-122">Description</span></span> | <span data-ttu-id="0befd-123">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="0befd-123">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0befd-124">**domainNameLabel**</span><span class="sxs-lookup"><span data-stu-id="0befd-124">**domainNameLabel**</span></span> |<span data-ttu-id="0befd-125">Värden som har namnet används för namnmatchning.</span><span class="sxs-lookup"><span data-stu-id="0befd-125">Host named used for name resolution.</span></span> |<span data-ttu-id="0befd-126">www, ftp, vm1</span><span class="sxs-lookup"><span data-stu-id="0befd-126">www, ftp, vm1</span></span> |
| <span data-ttu-id="0befd-127">**FQDN**</span><span class="sxs-lookup"><span data-stu-id="0befd-127">**fqdn**</span></span> |<span data-ttu-id="0befd-128">Fullständigt kvalificerade namnet för hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="0befd-128">Fully qualified name for hello public IP.</span></span> |<span data-ttu-id="0befd-129">www.westus.cloudapp.Azure.com</span><span class="sxs-lookup"><span data-stu-id="0befd-129">www.westus.cloudapp.azure.com</span></span> |
| <span data-ttu-id="0befd-130">**reverseFqdn**</span><span class="sxs-lookup"><span data-stu-id="0befd-130">**reverseFqdn**</span></span> |<span data-ttu-id="0befd-131">Fullständigt kvalificerat domännamn som matchar toohello IP-adress och är registrerad i DNS som en PTR-post.</span><span class="sxs-lookup"><span data-stu-id="0befd-131">Fully qualified domain name that resolves toohello IP address and is registered in DNS as a PTR record.</span></span> |<span data-ttu-id="0befd-132">www.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="0befd-132">www.contoso.com.</span></span> |

<span data-ttu-id="0befd-133">Exempel offentliga IP-adressen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="0befd-133">Sample public IP address in JSON format:</span></span>

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a><span data-ttu-id="0befd-134">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0befd-134">Additional resources</span></span>
* <span data-ttu-id="0befd-135">Hämta mer information [offentliga IP-adresser](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="0befd-135">Get more information about [public IP addresses](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span></span>
* <span data-ttu-id="0befd-136">Lär dig mer om [instansen nivån offentliga IP-adresser](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="0befd-136">Learn about [instance level public IP addresses](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span></span>
* <span data-ttu-id="0befd-137">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) för det offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0befd-137">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) for public IP addresses.</span></span>

