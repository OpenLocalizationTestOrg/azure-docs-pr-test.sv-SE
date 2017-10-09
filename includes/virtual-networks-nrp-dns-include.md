## <a name="azure-dns"></a><span data-ttu-id="3a895-101">Azure DNS</span><span class="sxs-lookup"><span data-stu-id="3a895-101">Azure DNS</span></span>
<span data-ttu-id="3a895-102">Azure DNS är värdtjänsten för DNS-domäner som tillhandahåller namnmatchning med hjälp av Microsoft Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="3a895-102">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span>

| <span data-ttu-id="3a895-103">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3a895-103">Property</span></span> | <span data-ttu-id="3a895-104">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3a895-104">Description</span></span> | <span data-ttu-id="3a895-105">Exempelvärde</span><span class="sxs-lookup"><span data-stu-id="3a895-105">Sample Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3a895-106">**DNSzones**</span><span class="sxs-lookup"><span data-stu-id="3a895-106">**DNSzones**</span></span> |<span data-ttu-id="3a895-107">Domän zonen information toohost DNS-posterna för en viss domän</span><span class="sxs-lookup"><span data-stu-id="3a895-107">Domain zone information toohost DNS records of a particular domain</span></span> |<span data-ttu-id="3a895-108">/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com ”</span><span class="sxs-lookup"><span data-stu-id="3a895-108">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com"</span></span> |

### <a name="dns-record-sets"></a><span data-ttu-id="3a895-109">DNS-postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="3a895-109">DNS record sets</span></span>
<span data-ttu-id="3a895-110">DNS-zoner ha ett underordnat objekt med namnet postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="3a895-110">DNS zones have a child object named record set.</span></span> <span data-ttu-id="3a895-111">Postuppsättningar är en samling värdposter efter typ för en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="3a895-111">Record sets are a collection of host records by type for a DNS zone.</span></span> <span data-ttu-id="3a895-112">Posttyper är A, AAAA, CNAME, MX, NS, SOA, SRV och TXT.</span><span class="sxs-lookup"><span data-stu-id="3a895-112">Record types are A, AAAA, CNAME, MX, NS, SOA,SRV and TXT.</span></span>

| <span data-ttu-id="3a895-113">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3a895-113">Property</span></span> | <span data-ttu-id="3a895-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3a895-114">Description</span></span> | <span data-ttu-id="3a895-115">Exempelvärde</span><span class="sxs-lookup"><span data-stu-id="3a895-115">Sample value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3a895-116">A</span><span class="sxs-lookup"><span data-stu-id="3a895-116">A</span></span> |<span data-ttu-id="3a895-117">IPv4-posttyp</span><span class="sxs-lookup"><span data-stu-id="3a895-117">IPv4 record type</span></span> |<span data-ttu-id="3a895-118">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www</span><span class="sxs-lookup"><span data-stu-id="3a895-118">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www</span></span> |
| <span data-ttu-id="3a895-119">AAAA</span><span class="sxs-lookup"><span data-stu-id="3a895-119">AAAA</span></span> |<span data-ttu-id="3a895-120">IPv6-typ</span><span class="sxs-lookup"><span data-stu-id="3a895-120">IPv6 record type</span></span> |<span data-ttu-id="3a895-121">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span><span class="sxs-lookup"><span data-stu-id="3a895-121">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span></span> |
| <span data-ttu-id="3a895-122">CNAME</span><span class="sxs-lookup"><span data-stu-id="3a895-122">CNAME</span></span> |<span data-ttu-id="3a895-123">kanoniskt namn posttyp <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="3a895-123">canonical name record type <sup>1</sup></span></span> |<span data-ttu-id="3a895-124">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span><span class="sxs-lookup"><span data-stu-id="3a895-124">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span></span> |
| <span data-ttu-id="3a895-125">MX</span><span class="sxs-lookup"><span data-stu-id="3a895-125">MX</span></span> |<span data-ttu-id="3a895-126">e-post</span><span class="sxs-lookup"><span data-stu-id="3a895-126">mail record type</span></span> |<span data-ttu-id="3a895-127">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/Mail</span><span class="sxs-lookup"><span data-stu-id="3a895-127">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span></span> |
| <span data-ttu-id="3a895-128">NS</span><span class="sxs-lookup"><span data-stu-id="3a895-128">NS</span></span> |<span data-ttu-id="3a895-129">namn på server-posttyp</span><span class="sxs-lookup"><span data-stu-id="3a895-129">name server record type</span></span> |<span data-ttu-id="3a895-130">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/</span><span class="sxs-lookup"><span data-stu-id="3a895-130">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/</span></span> |
| <span data-ttu-id="3a895-131">SOA</span><span class="sxs-lookup"><span data-stu-id="3a895-131">SOA</span></span> |<span data-ttu-id="3a895-132">Början av myndigheten posttyp <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="3a895-132">Start of Authority record type <sup>2</sup></span></span> |<span data-ttu-id="3a895-133">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA</span><span class="sxs-lookup"><span data-stu-id="3a895-133">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA</span></span> |
| <span data-ttu-id="3a895-134">SRV</span><span class="sxs-lookup"><span data-stu-id="3a895-134">SRV</span></span> |<span data-ttu-id="3a895-135">typ av tjänst</span><span class="sxs-lookup"><span data-stu-id="3a895-135">service record type</span></span> |<span data-ttu-id="3a895-136">/subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV</span><span class="sxs-lookup"><span data-stu-id="3a895-136">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV</span></span> |

<span data-ttu-id="3a895-137"><sup>1</sup> kan bara ett värde per uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="3a895-137"><sup>1</sup> only allows one value per record set.</span></span>

<span data-ttu-id="3a895-138"><sup>2</sup> tillåter endast en posttyp SOA per DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="3a895-138"><sup>2</sup> only allows one record type SOA per DNS zone.</span></span> 

<span data-ttu-id="3a895-139">Exempel på DNS-zonen i Json-format:</span><span class="sxs-lookup"><span data-stu-id="3a895-139">Sample of DNS zone in Json format:</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "hello name of hello DNS zone toobe created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "hello name of hello DNS record toobe created.  hello name is relative toohello zone, not hello FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a><span data-ttu-id="3a895-140">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3a895-140">Additional resources</span></span>
<span data-ttu-id="3a895-141">Läs hello [REST API-dokumentation för DNS-zoner ](https://msdn.microsoft.com/library/azure/mt130626.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3a895-141">Read hello [REST API documentation for DNS zones ](https://msdn.microsoft.com/library/azure/mt130626.aspx) for more information.</span></span>

<span data-ttu-id="3a895-142">Läs hello [REST API-dokumentation för DNS-postuppsättningar](https://msdn.microsoft.com/library/azure/mt130627.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3a895-142">Read hello [REST API documentation for DNS record sets](https://msdn.microsoft.com/library/azure/mt130627.aspx) for more information.</span></span>

