---
title: "Azure-instans Metadata Service för Windows | Microsoft Docs"
description: "RESTful-gränssnitt att hämta information om Windows VM beräkning, nätverk och kommande underhållshändelser."
services: virtual-machines-windows
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: 55b97b89cb297dc08dc73f6714c5159d4565a97c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-instance-metadata-service-for-windows-vms"></a><span data-ttu-id="f8645-103">Azure-instans Metadata-tjänsten för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="f8645-103">Azure Instance Metadata service for Windows VMs</span></span>


<span data-ttu-id="f8645-104">Tjänsten Azure instans Metadata innehåller information om virtuella instanser som kan användas för att hantera och konfigurera dina virtuella datorer som körs.</span><span class="sxs-lookup"><span data-stu-id="f8645-104">The Azure Instance Metadata Service provides information about running virtual machine instances that can be used to manage and configure your virtual machines.</span></span>
<span data-ttu-id="f8645-105">Detta omfattar information som SKU, nätverkskonfigurationen och kommande underhållshändelser.</span><span class="sxs-lookup"><span data-stu-id="f8645-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="f8645-106">Mer information om vilken typ av information är tillgänglig finns [metadatakategorier](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="f8645-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="f8645-107">Azures instans Metadata Service är en REST-slutpunkt som är tillgängliga för alla IaaS-VM som skapats via den [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="f8645-107">Azure's Instance Metadata Service is a REST Endpoint accessible to all IaaS VMs created via the [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="f8645-108">Slutpunkten är tillgänglig på en välkänd icke-dirigerbara IP-adress (`169.254.169.254`) som kan nås från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f8645-108">The endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within the VM.</span></span>



> [!IMPORTANT]
> <span data-ttu-id="f8645-109">Den här tjänsten är **allmänt tillgänglig** i globala Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="f8645-109">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="f8645-110">Det är i Public preview för myndigheter, Kina och tyska Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="f8645-110">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="f8645-111">Den tar emot uppdateringar för att exponera ny information om virtuell datorinstans regelbundet.</span><span class="sxs-lookup"><span data-stu-id="f8645-111">It regularly receives updates to expose new information about virtual machine instances.</span></span> <span data-ttu-id="f8645-112">Den här sidan visar den uppdaterade [datakategorier](#instance-metadata-data-categories) tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f8645-112">This page reflects the up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>



## <a name="service-availability"></a><span data-ttu-id="f8645-113">Tjänsttillgänglighet för</span><span class="sxs-lookup"><span data-stu-id="f8645-113">Service Availability</span></span>
<span data-ttu-id="f8645-114">Tjänsten är tillgänglig i alla allmänt tillgänglig Global Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="f8645-114">The service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="f8645-115">Tjänsten är tillgänglig som förhandsversion i Government, Kina eller Tyskland regioner.</span><span class="sxs-lookup"><span data-stu-id="f8645-115">The service is in public preview  in the Government, China, or Germany regions.</span></span>

<span data-ttu-id="f8645-116">Regioner</span><span class="sxs-lookup"><span data-stu-id="f8645-116">Regions</span></span>                                        | <span data-ttu-id="f8645-117">Tillgänglighet?</span><span class="sxs-lookup"><span data-stu-id="f8645-117">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="f8645-118">Alla allmänt tillgänglig Global Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="f8645-118">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/regions/)     | <span data-ttu-id="f8645-119">Allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="f8645-119">Generally Available</span></span> 
[<span data-ttu-id="f8645-120">Azure Government</span><span class="sxs-lookup"><span data-stu-id="f8645-120">Azure Government</span></span>](https://azure.microsoft.com/overview/clouds/government/)              | <span data-ttu-id="f8645-121">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="f8645-121">In Preview</span></span> 
[<span data-ttu-id="f8645-122">Azure Kina</span><span class="sxs-lookup"><span data-stu-id="f8645-122">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="f8645-123">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="f8645-123">In Preview</span></span>
[<span data-ttu-id="f8645-124">Azure Tyskland</span><span class="sxs-lookup"><span data-stu-id="f8645-124">Azure Germany</span></span>](https://azure.microsoft.com/overview/clouds/germany/)                    | <span data-ttu-id="f8645-125">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="f8645-125">In Preview</span></span>

<span data-ttu-id="f8645-126">Den här tabellen uppdateras när tjänsten blir tillgänglig i andra Azure-moln.</span><span class="sxs-lookup"><span data-stu-id="f8645-126">This table is updated when the service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="f8645-127">Om du vill testa tjänsten instans Metadata, skapa en virtuell dator från [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) eller [Azure-portalen](http://portal.azure.com) i ovanstående områden och följ exemplen nedan.</span><span class="sxs-lookup"><span data-stu-id="f8645-127">To try out the Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or the [Azure portal](http://portal.azure.com) in the above regions and follow the examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="f8645-128">Användning</span><span class="sxs-lookup"><span data-stu-id="f8645-128">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="f8645-129">Versionshantering</span><span class="sxs-lookup"><span data-stu-id="f8645-129">Versioning</span></span>
<span data-ttu-id="f8645-130">Tjänsten instans Metadata är en ny version.</span><span class="sxs-lookup"><span data-stu-id="f8645-130">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="f8645-131">Versioner är obligatoriska och den aktuella versionen är `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="f8645-131">Versions are mandatory and the current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-132">Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som den api-versionen.</span><span class="sxs-lookup"><span data-stu-id="f8645-132">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="f8645-133">Det här formatet stöds inte längre och kommer att inaktualiseras i framtiden.</span><span class="sxs-lookup"><span data-stu-id="f8645-133">This format is no longer supported and will be deprecated in the future.</span></span>

<span data-ttu-id="f8645-134">När vi lägger till nya versioner kan äldre versioner fortfarande användas för kompatibilitet om skripten har beroenden på specifika dataformat.</span><span class="sxs-lookup"><span data-stu-id="f8645-134">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="f8645-135">Observera dock att den aktuella preview version(2017-03-01) inte kanske tillgänglig när tjänsten är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="f8645-135">However, note that the current preview version(2017-03-01) may not be available once the service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="f8645-136">Med hjälp av rubriker</span><span class="sxs-lookup"><span data-stu-id="f8645-136">Using headers</span></span>
<span data-ttu-id="f8645-137">När du frågar instans Metadata tjänsten måste du ange rubriken `Metadata: true` så begäran inte omdirigerades oavsiktligt.</span><span class="sxs-lookup"><span data-stu-id="f8645-137">When you query the Instance Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="f8645-138">Hämta metadata</span><span class="sxs-lookup"><span data-stu-id="f8645-138">Retrieving metadata</span></span>

<span data-ttu-id="f8645-139">Metadata för instansen är tillgängliga för att köra virtuella datorer skapas/hanteras med hjälp av [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="f8645-139">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="f8645-140">Komma åt alla datakategorier för en virtuell dator-instans med följande begäran:</span><span class="sxs-lookup"><span data-stu-id="f8645-140">Access all data categories for a virtual machine instance using the following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="f8645-141">Alla metadatafrågor för instansen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f8645-141">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="f8645-142">Utdata</span><span class="sxs-lookup"><span data-stu-id="f8645-142">Data output</span></span>
<span data-ttu-id="f8645-143">Som standard tjänsten instans Metadata returnerar data i JSON-format (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="f8645-143">By default, the Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="f8645-144">Dock kan olika API: er returnera data i olika format om begärt.</span><span class="sxs-lookup"><span data-stu-id="f8645-144">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="f8645-145">I följande tabell är en referens för andra dataformat stöder API: er.</span><span class="sxs-lookup"><span data-stu-id="f8645-145">The following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="f8645-146">API</span><span class="sxs-lookup"><span data-stu-id="f8645-146">API</span></span> | <span data-ttu-id="f8645-147">Standardformatet för Data</span><span class="sxs-lookup"><span data-stu-id="f8645-147">Default Data Format</span></span> | <span data-ttu-id="f8645-148">Andra format</span><span class="sxs-lookup"><span data-stu-id="f8645-148">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="f8645-149">/Instance</span><span class="sxs-lookup"><span data-stu-id="f8645-149">/instance</span></span> | <span data-ttu-id="f8645-150">JSON</span><span class="sxs-lookup"><span data-stu-id="f8645-150">json</span></span> | <span data-ttu-id="f8645-151">Text</span><span class="sxs-lookup"><span data-stu-id="f8645-151">text</span></span>
<span data-ttu-id="f8645-152">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="f8645-152">/scheduledevents</span></span> | <span data-ttu-id="f8645-153">JSON</span><span class="sxs-lookup"><span data-stu-id="f8645-153">json</span></span> | <span data-ttu-id="f8645-154">Ingen</span><span class="sxs-lookup"><span data-stu-id="f8645-154">none</span></span>

<span data-ttu-id="f8645-155">Ange det begärda formatet som en querystring-parameter i begäran för att komma åt en icke-förvalt svarsformat.</span><span class="sxs-lookup"><span data-stu-id="f8645-155">To access a non-default response format, specify the requested format as a querystring parameter in the request.</span></span> <span data-ttu-id="f8645-156">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f8645-156">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="f8645-157">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="f8645-157">Security</span></span>
<span data-ttu-id="f8645-158">Instansen Metadata tjänstslutpunkten är enbart tillgänglig från den virtuella datorn-instansen som körs på en icke-dirigerbara IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f8645-158">The Instance Metadata Service endpoint is accessible only from within the running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="f8645-159">Dessutom begäran med en `X-Forwarded-For` huvud avvisas av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8645-159">In addition, any request with a `X-Forwarded-For` header is rejected by the service.</span></span>
<span data-ttu-id="f8645-160">Vi behöver också begäranden som innehåller en `Metadata: true` sidhuvud så att den faktiska begäranden var direkt avsedda och inte en del av oavsiktlig omdirigering.</span><span class="sxs-lookup"><span data-stu-id="f8645-160">We also require requests to contain a `Metadata: true` header to ensure that the actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="f8645-161">Fel</span><span class="sxs-lookup"><span data-stu-id="f8645-161">Error</span></span>
<span data-ttu-id="f8645-162">Om det finns en dataelement som inte hittades eller en felaktig begäran, returnerar tjänsten instans Metadata standard HTTP-fel.</span><span class="sxs-lookup"><span data-stu-id="f8645-162">If there is a data element not found or a malformed request, the Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="f8645-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f8645-163">For example:</span></span>

<span data-ttu-id="f8645-164">HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="f8645-164">HTTP Status Code</span></span> | <span data-ttu-id="f8645-165">Orsak</span><span class="sxs-lookup"><span data-stu-id="f8645-165">Reason</span></span>
----------------|-------
<span data-ttu-id="f8645-166">200 OK</span><span class="sxs-lookup"><span data-stu-id="f8645-166">200 OK</span></span> |
<span data-ttu-id="f8645-167">400 Felaktig förfrågan</span><span class="sxs-lookup"><span data-stu-id="f8645-167">400 Bad Request</span></span> | <span data-ttu-id="f8645-168">Saknas `Metadata: true` sidhuvud</span><span class="sxs-lookup"><span data-stu-id="f8645-168">Missing `Metadata: true` header</span></span>
<span data-ttu-id="f8645-169">404 Hittades inte</span><span class="sxs-lookup"><span data-stu-id="f8645-169">404 Not Found</span></span> | <span data-ttu-id="f8645-170">Begärt element does't finns</span><span class="sxs-lookup"><span data-stu-id="f8645-170">The requested element does't exist</span></span> 
<span data-ttu-id="f8645-171">405 Metoden tillåts inte</span><span class="sxs-lookup"><span data-stu-id="f8645-171">405 Method Not Allowed</span></span> | <span data-ttu-id="f8645-172">Endast `GET` och `POST` stöds</span><span class="sxs-lookup"><span data-stu-id="f8645-172">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="f8645-173">429 för många begäranden</span><span class="sxs-lookup"><span data-stu-id="f8645-173">429 Too Many Requests</span></span> | <span data-ttu-id="f8645-174">API: et stöder för närvarande högst 5 frågor per sekund</span><span class="sxs-lookup"><span data-stu-id="f8645-174">The API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="f8645-175">500 tjänstfel</span><span class="sxs-lookup"><span data-stu-id="f8645-175">500 Service Error</span></span>     | <span data-ttu-id="f8645-176">Försök igen efter en stund</span><span class="sxs-lookup"><span data-stu-id="f8645-176">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="f8645-177">Exempel</span><span class="sxs-lookup"><span data-stu-id="f8645-177">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-178">Alla API-svar är JSON-strängar.</span><span class="sxs-lookup"><span data-stu-id="f8645-178">All API responses are JSON strings.</span></span> <span data-ttu-id="f8645-179">Alla följande exempel svar är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f8645-179">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="f8645-180">Hämtar nätverksinformation</span><span class="sxs-lookup"><span data-stu-id="f8645-180">Retrieving network information</span></span>

<span data-ttu-id="f8645-181">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-181">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="f8645-182">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-182">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-183">Svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="f8645-183">The response is a JSON string.</span></span> <span data-ttu-id="f8645-184">Följande exempel svaret är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f8645-184">The following example response is pretty-printed for readability.</span></span>

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="f8645-185">Hämta offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="f8645-185">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="f8645-186">Hämta alla metadata för en instans</span><span class="sxs-lookup"><span data-stu-id="f8645-186">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="f8645-187">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-187">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="f8645-188">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-188">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-189">Svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="f8645-189">The response is a JSON string.</span></span> <span data-ttu-id="f8645-190">Följande exempel svaret är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f8645-190">The following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="f8645-191">Hämtning av metadata i Windows-dator</span><span class="sxs-lookup"><span data-stu-id="f8645-191">Retrieving metadata in Windows virtual machine</span></span>

<span data-ttu-id="f8645-192">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-192">**Request**</span></span>

<span data-ttu-id="f8645-193">Instansen metadata kan hämtas i Windows via PowerShell-verktyget `curl`:</span><span class="sxs-lookup"><span data-stu-id="f8645-193">Instance metadata can be retrieved in Windows via the PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="f8645-194">Eller via den `Invoke-RestMethod` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f8645-194">Or through the `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="f8645-195">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-195">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-196">Svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="f8645-196">The response is a JSON string.</span></span> <span data-ttu-id="f8645-197">Följande exempel svaret är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f8645-197">The following example response  is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="f8645-198">Instansen metadata datakategorier</span><span class="sxs-lookup"><span data-stu-id="f8645-198">Instance metadata data categories</span></span>
<span data-ttu-id="f8645-199">Följande datakategorier är tillgängliga via tjänsten instans Metadata:</span><span class="sxs-lookup"><span data-stu-id="f8645-199">The following data categories are available through the Instance Metadata Service:</span></span>

<span data-ttu-id="f8645-200">Data</span><span class="sxs-lookup"><span data-stu-id="f8645-200">Data</span></span> | <span data-ttu-id="f8645-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f8645-201">Description</span></span>
-----|------------
<span data-ttu-id="f8645-202">location</span><span class="sxs-lookup"><span data-stu-id="f8645-202">location</span></span> | <span data-ttu-id="f8645-203">Azure-regionen för den virtuella datorn körs i</span><span class="sxs-lookup"><span data-stu-id="f8645-203">Azure Region the VM is running in</span></span>
<span data-ttu-id="f8645-204">namn</span><span class="sxs-lookup"><span data-stu-id="f8645-204">name</span></span> | <span data-ttu-id="f8645-205">Namnet på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-205">Name of the VM</span></span> 
<span data-ttu-id="f8645-206">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="f8645-206">offer</span></span> | <span data-ttu-id="f8645-207">Tillhandahåller information för VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f8645-207">Offer information for the VM image.</span></span> <span data-ttu-id="f8645-208">Det här värdet finns bara för avbildningar som distribueras från Azure-avbildning gallery.</span><span class="sxs-lookup"><span data-stu-id="f8645-208">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="f8645-209">Publisher</span><span class="sxs-lookup"><span data-stu-id="f8645-209">publisher</span></span> | <span data-ttu-id="f8645-210">Utgivaren av VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="f8645-210">Publisher of the VM image</span></span>
<span data-ttu-id="f8645-211">SKU</span><span class="sxs-lookup"><span data-stu-id="f8645-211">sku</span></span> | <span data-ttu-id="f8645-212">Specifika SKU för VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="f8645-212">Specific SKU for the VM image</span></span>  
<span data-ttu-id="f8645-213">Version</span><span class="sxs-lookup"><span data-stu-id="f8645-213">version</span></span> | <span data-ttu-id="f8645-214">Version av VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="f8645-214">Version of the VM image</span></span> 
<span data-ttu-id="f8645-215">osType</span><span class="sxs-lookup"><span data-stu-id="f8645-215">osType</span></span> | <span data-ttu-id="f8645-216">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="f8645-216">Linux or Windows</span></span> 
<span data-ttu-id="f8645-217">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="f8645-217">platformUpdateDomain</span></span> |  <span data-ttu-id="f8645-218">[Uppdateringsdomän](manage-availability.md) den virtuella datorn körs</span><span class="sxs-lookup"><span data-stu-id="f8645-218">[Update domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="f8645-219">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="f8645-219">platformFaultDomain</span></span> | <span data-ttu-id="f8645-220">[Feldomänen](manage-availability.md) den virtuella datorn körs</span><span class="sxs-lookup"><span data-stu-id="f8645-220">[Fault domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="f8645-221">vmId</span><span class="sxs-lookup"><span data-stu-id="f8645-221">vmId</span></span> | <span data-ttu-id="f8645-222">[Unik identifierare](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-222">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for the VM</span></span>
<span data-ttu-id="f8645-223">vmSize</span><span class="sxs-lookup"><span data-stu-id="f8645-223">vmSize</span></span> | [<span data-ttu-id="f8645-224">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="f8645-224">VM size</span></span>](sizes.md)
<span data-ttu-id="f8645-225">IPv4/privateIpAddress</span><span class="sxs-lookup"><span data-stu-id="f8645-225">ipv4/privateIpAddress</span></span> | <span data-ttu-id="f8645-226">Lokala IPv4-adressen för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-226">Local IPv4 address of the VM</span></span> 
<span data-ttu-id="f8645-227">IPv4/publicIpAddress</span><span class="sxs-lookup"><span data-stu-id="f8645-227">ipv4/publicIpAddress</span></span> | <span data-ttu-id="f8645-228">Offentliga IPv4-adressen för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-228">Public IPv4 address of the VM</span></span>
<span data-ttu-id="f8645-229">undernätet och/eller adress</span><span class="sxs-lookup"><span data-stu-id="f8645-229">subnet/address</span></span> | <span data-ttu-id="f8645-230">Undernätsadress av den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-230">Subnet address of the VM</span></span>
<span data-ttu-id="f8645-231">undernätsprefixet /</span><span class="sxs-lookup"><span data-stu-id="f8645-231">subnet/prefix</span></span> | <span data-ttu-id="f8645-232">Undernätets prefix, exempel 24</span><span class="sxs-lookup"><span data-stu-id="f8645-232">Subnet prefix, example 24</span></span>
<span data-ttu-id="f8645-233">IPv6/IP-adress</span><span class="sxs-lookup"><span data-stu-id="f8645-233">ipv6/ipAddress</span></span> | <span data-ttu-id="f8645-234">Den lokala IPv6-adressen för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-234">Local IPv6 address of the VM</span></span>
<span data-ttu-id="f8645-235">MAC-adress</span><span class="sxs-lookup"><span data-stu-id="f8645-235">macAddress</span></span> | <span data-ttu-id="f8645-236">VM mac-adress</span><span class="sxs-lookup"><span data-stu-id="f8645-236">VM mac address</span></span> 
<span data-ttu-id="f8645-237">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="f8645-237">scheduledevents</span></span> | <span data-ttu-id="f8645-238">För närvarande finns i Public Preview finns [scheduledevents](scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="f8645-238">Currently in Public Preview See [scheduledevents](scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="f8645-239">Exempelscenarier för användning</span><span class="sxs-lookup"><span data-stu-id="f8645-239">Example scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="f8645-240">Spårning av virtuell dator som kör på Azure</span><span class="sxs-lookup"><span data-stu-id="f8645-240">Tracking VM running on Azure</span></span>

<span data-ttu-id="f8645-241">Du kan behöva hålla reda på antalet virtuella datorer som kör programvaran eller att agenter som behöver spåra unikhet för den virtuella datorn som en tjänsteleverantör.</span><span class="sxs-lookup"><span data-stu-id="f8645-241">As a service provider, you may require to track the number of VMs running your software or have agents that need to track uniqueness of the VM.</span></span> <span data-ttu-id="f8645-242">Om du vill kunna hämta ett unikt ID för en virtuell dator använder den `vmId` från Metadata-tjänsten för instansen.</span><span class="sxs-lookup"><span data-stu-id="f8645-242">To be able to get a unique ID for a VM, use the `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="f8645-243">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-243">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="f8645-244">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-244">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="f8645-245">Placering av behållare, partitioner data baserat fel/uppdatera domän</span><span class="sxs-lookup"><span data-stu-id="f8645-245">Placement of containers, data-partitions based fault/update domain</span></span> 

<span data-ttu-id="f8645-246">För vissa scenarier, placering av olika repliker är av yttersta vikt.</span><span class="sxs-lookup"><span data-stu-id="f8645-246">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="f8645-247">Till exempel [HDFS replik placering](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) eller placering av behållare via en [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kanske du behöver känna till den `platformFaultDomain` och `platformUpdateDomain` den virtuella datorn körs på.</span><span class="sxs-lookup"><span data-stu-id="f8645-247">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require to know the `platformFaultDomain` and `platformUpdateDomain` the VM is running on.</span></span>
<span data-ttu-id="f8645-248">Du kan fråga data direkt via tjänsten instans Metadata.</span><span class="sxs-lookup"><span data-stu-id="f8645-248">You can query this data directly via the Instance Metadata Service.</span></span>

<span data-ttu-id="f8645-249">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-249">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="f8645-250">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-250">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a><span data-ttu-id="f8645-251">Mer information om den virtuella datorn under supportärende</span><span class="sxs-lookup"><span data-stu-id="f8645-251">Getting more information about the VM during support case</span></span> 

<span data-ttu-id="f8645-252">Som en leverantör får du ett supportsamtal där du vill veta mer om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f8645-252">As a service provider, you may get a support call where you would like to know more information about the VM.</span></span> <span data-ttu-id="f8645-253">Be kunden att dela beräknings-metadata kan ge grundläggande information för supportpersonal känna till om vilken typ av virtuell dator på Azure.</span><span class="sxs-lookup"><span data-stu-id="f8645-253">Asking the customer to share the compute metadata can provide basic information for the support professional to know about the kind of VM on Azure.</span></span> 

<span data-ttu-id="f8645-254">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="f8645-254">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="f8645-255">**Svar**</span><span class="sxs-lookup"><span data-stu-id="f8645-255">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="f8645-256">Svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="f8645-256">The response is a JSON string.</span></span> <span data-ttu-id="f8645-257">Följande exempel svaret är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f8645-257">The following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a><span data-ttu-id="f8645-258">Exempel på hur metadatatjänsten som använder olika språk inuti den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8645-258">Examples of calling metadata service using different languages inside the VM</span></span> 

<span data-ttu-id="f8645-259">Språk</span><span class="sxs-lookup"><span data-stu-id="f8645-259">Language</span></span> | <span data-ttu-id="f8645-260">Exempel</span><span class="sxs-lookup"><span data-stu-id="f8645-260">Example</span></span> 
---------|----------------
<span data-ttu-id="f8645-261">Ruby</span><span class="sxs-lookup"><span data-stu-id="f8645-261">Ruby</span></span>     | <span data-ttu-id="f8645-262">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="f8645-262">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="f8645-263">Gå Lan</span><span class="sxs-lookup"><span data-stu-id="f8645-263">Go Lan</span></span>   | <span data-ttu-id="f8645-264">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="f8645-264">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="f8645-265">python</span><span class="sxs-lookup"><span data-stu-id="f8645-265">python</span></span>   | <span data-ttu-id="f8645-266">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="f8645-266">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="f8645-267">C++</span><span class="sxs-lookup"><span data-stu-id="f8645-267">C++</span></span>      | <span data-ttu-id="f8645-268">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="f8645-268">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="f8645-269">C#</span><span class="sxs-lookup"><span data-stu-id="f8645-269">C#</span></span>       | <span data-ttu-id="f8645-270">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.CS</span><span class="sxs-lookup"><span data-stu-id="f8645-270">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="f8645-271">Javascript</span><span class="sxs-lookup"><span data-stu-id="f8645-271">Javascript</span></span> | <span data-ttu-id="f8645-272">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="f8645-272">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="f8645-273">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8645-273">Powershell</span></span> | <span data-ttu-id="f8645-274">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="f8645-274">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="f8645-275">Bash</span><span class="sxs-lookup"><span data-stu-id="f8645-275">Bash</span></span>       | <span data-ttu-id="f8645-276">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="f8645-276">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="f8645-277">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="f8645-277">FAQ</span></span>
1. <span data-ttu-id="f8645-278">Jag får felet `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="f8645-278">I am getting the error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="f8645-279">Vad innebär det?</span><span class="sxs-lookup"><span data-stu-id="f8645-279">What does this mean?</span></span>
   * <span data-ttu-id="f8645-280">Instansen Metadata tjänsten kräver huvudet `Metadata: true` som används i begäran.</span><span class="sxs-lookup"><span data-stu-id="f8645-280">The Instance Metadata Service requires the header `Metadata: true` to be passed in the request.</span></span> <span data-ttu-id="f8645-281">Skicka det här sidhuvudet i REST-anrop tillåter åtkomst till tjänsten instans Metadata.</span><span class="sxs-lookup"><span data-stu-id="f8645-281">Passing this header in the REST call allows access to the Instance Metadata Service.</span></span> 
2. <span data-ttu-id="f8645-282">Varför inte inträffar beräknings-information för den virtuella datorn?</span><span class="sxs-lookup"><span data-stu-id="f8645-282">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="f8645-283">Tjänsten instans Metadata stöder för närvarande bara instanser som skapats med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f8645-283">Currently the Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="f8645-284">I framtiden, kan vi lägga till stöd för virtuella datorer för molnet tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8645-284">In the future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="f8645-285">Jag har skapat Min virtuella dator via Azure Resource Manager en tid sedan.</span><span class="sxs-lookup"><span data-stu-id="f8645-285">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="f8645-286">Varför kan jag inte se compute metadatainformation?</span><span class="sxs-lookup"><span data-stu-id="f8645-286">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="f8645-287">För alla virtuella datorer som skapats efter Sep 2016, lägga till en [taggen](../../azure-resource-manager/resource-group-using-tags.md) att starta ser compute metadata.</span><span class="sxs-lookup"><span data-stu-id="f8645-287">For any VMs created after Sep 2016, add a [Tag](../../azure-resource-manager/resource-group-using-tags.md) to start seeing compute metadata.</span></span> <span data-ttu-id="f8645-288">För äldre virtuella datorer (som skapats före Sep 2016), Lägg till/ta bort tillägg eller data diskar till den virtuella datorn för att uppdatera metadata.</span><span class="sxs-lookup"><span data-stu-id="f8645-288">For older VMs (created before Sep 2016), add/remove extensions or data disks to the VM to refresh metadata.</span></span>
4. <span data-ttu-id="f8645-289">Varför får felet `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="f8645-289">Why am I getting the error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="f8645-290">Försök att utföra din begäran utifrån exponentiell tillbaka av systemet.</span><span class="sxs-lookup"><span data-stu-id="f8645-290">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="f8645-291">Kontakta Azure-supporten om problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="f8645-291">If the issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="f8645-292">Där delar ytterligare frågor/kommentarer?</span><span class="sxs-lookup"><span data-stu-id="f8645-292">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="f8645-293">Skicka kommentarer om http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="f8645-293">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="f8645-294">Fungerar detta för Virtual Machine Scale ange instansen?</span><span class="sxs-lookup"><span data-stu-id="f8645-294">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="f8645-295">Ja är Metadata-tjänsten tillgänglig för skala ange instanser.</span><span class="sxs-lookup"><span data-stu-id="f8645-295">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="f8645-296">Hur får jag support för tjänsten?</span><span class="sxs-lookup"><span data-stu-id="f8645-296">How do I get support for the service?</span></span>
   * <span data-ttu-id="f8645-297">Om du vill få support för tjänsten, skapa ett supportproblem i Azure-portalen för den virtuella datorn där du inte kan hämta metadata svar efter lång försök</span><span class="sxs-lookup"><span data-stu-id="f8645-297">To get support for the service, create a support issue in Azure portal for the VM where you are not able to get metadata response after long retries</span></span> 

   ![Stöd för instans-Metadata](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="f8645-299">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8645-299">Next steps</span></span>

- <span data-ttu-id="f8645-300">Lär dig mer om den [schemalagda händelser](scheduled-events.md) API **som förhandsversion** som tillhandahålls av tjänsten för instansen Metadata.</span><span class="sxs-lookup"><span data-stu-id="f8645-300">Learn more about the [Scheduled Events](scheduled-events.md) API **in public preview** provided by the Instance Metadata service.</span></span>
