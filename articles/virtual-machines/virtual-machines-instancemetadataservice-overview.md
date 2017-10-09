---
title: "aaaAzure instans översikt över Metadata | Microsoft Docs"
description: "RESTful-gränssnitt tooget information om Virtuella datorns beräkning, nätverk och kommande underhållshändelser."
services: virtual-machines-windows, virtual-machines-linux,virtual-machines-scale-sets, cloud-services
documentationcenter: virtual-machines
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: harijay
ms.openlocfilehash: e87cdf28f80b9ef8cc566b637549c48846862f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service"></a><span data-ttu-id="34933-103">Azure-instans Metadata Service</span><span class="sxs-lookup"><span data-stu-id="34933-103">Azure Instance Metadata Service</span></span> 


<span data-ttu-id="34933-104">hello Azure instans Metadata Service innehåller information om kör instanser för virtuella datorer som kan använda toomanage och konfigurera de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="34933-104">hello Azure Instance Metadata Service provides information about running virtual machine instances that can be used toomanage and configure your virtual machines.</span></span>
<span data-ttu-id="34933-105">Detta omfattar information som SKU, nätverkskonfigurationen och kommande underhållshändelser.</span><span class="sxs-lookup"><span data-stu-id="34933-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="34933-106">Mer information om vilken typ av information är tillgänglig finns [metadatakategorier](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="34933-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="34933-107">Azures instans Metadata Service är en REST-slutpunkt tillgänglig tooall IaaS-VM som skapats via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="34933-107">Azure's Instance Metadata Service is a REST Endpoint accessible tooall IaaS VMs created via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="34933-108">hello-slutpunkten är tillgänglig på en välkänd icke-dirigerbara IP-adress (`169.254.169.254`) som kan nås från inom hello VM.</span><span class="sxs-lookup"><span data-stu-id="34933-108">hello endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within hello VM.</span></span>

### <a name="important-information"></a><span data-ttu-id="34933-109">Viktig information</span><span class="sxs-lookup"><span data-stu-id="34933-109">Important information</span></span>

<span data-ttu-id="34933-110">Den här tjänsten är **allmänt tillgänglig** i globala Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="34933-110">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="34933-111">Det är i Public preview för myndigheter, Kina och tyska Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="34933-111">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="34933-112">Den tar emot uppdateringar tooexpose ny information om virtuell datorinstans regelbundet.</span><span class="sxs-lookup"><span data-stu-id="34933-112">It regularly receives updates tooexpose new information about virtual machine instances.</span></span> <span data-ttu-id="34933-113">Den här sidan visar hello uppdaterade [datakategorier](#instance-metadata-data-categories) tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="34933-113">This page reflects hello up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>

## <a name="service-availability"></a><span data-ttu-id="34933-114">Tjänsttillgänglighet för</span><span class="sxs-lookup"><span data-stu-id="34933-114">Service Availability</span></span>
<span data-ttu-id="34933-115">hello-tjänsten är tillgänglig i alla allmänt tillgänglig Global Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="34933-115">hello service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="34933-116">hello-tjänsten är tillgänglig som förhandsversion i hello myndigheter, Kina eller Tyskland regioner.</span><span class="sxs-lookup"><span data-stu-id="34933-116">hello service is in public preview  in hello Government, China, or Germany regions.</span></span>

<span data-ttu-id="34933-117">Regioner</span><span class="sxs-lookup"><span data-stu-id="34933-117">Regions</span></span>                                        | <span data-ttu-id="34933-118">Tillgänglighet?</span><span class="sxs-lookup"><span data-stu-id="34933-118">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="34933-119">Alla allmänt tillgänglig Global Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="34933-119">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/en-us/regions/)     | <span data-ttu-id="34933-120">Allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="34933-120">Generally Available</span></span> 
[<span data-ttu-id="34933-121">Azure Government</span><span class="sxs-lookup"><span data-stu-id="34933-121">Azure Government</span></span>](https://azure.microsoft.com/en-us/overview/clouds/government/)              | <span data-ttu-id="34933-122">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="34933-122">In Preview</span></span> 
[<span data-ttu-id="34933-123">Azure Kina</span><span class="sxs-lookup"><span data-stu-id="34933-123">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="34933-124">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="34933-124">In Preview</span></span>
[<span data-ttu-id="34933-125">Azure Tyskland</span><span class="sxs-lookup"><span data-stu-id="34933-125">Azure Germany</span></span>](https://azure.microsoft.com/en-us/overview/clouds/germany/)                    | <span data-ttu-id="34933-126">I förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="34933-126">In Preview</span></span>

<span data-ttu-id="34933-127">Den här tabellen uppdateras när hello tjänsten blir tillgänglig i andra Azure-moln.</span><span class="sxs-lookup"><span data-stu-id="34933-127">This table is updated when hello service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="34933-128">tootry ut hello instans Metadata tjänsten, skapa en virtuell dator från [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) eller hello [Azure-portalen](http://portal.azure.com) i hello över regioner och följ hello exemplen nedan.</span><span class="sxs-lookup"><span data-stu-id="34933-128">tootry out hello Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or hello [Azure portal](http://portal.azure.com) in hello above regions and follow hello examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="34933-129">Användning</span><span class="sxs-lookup"><span data-stu-id="34933-129">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="34933-130">Versionshantering</span><span class="sxs-lookup"><span data-stu-id="34933-130">Versioning</span></span>
<span data-ttu-id="34933-131">hello instans Metadata Service är en ny version.</span><span class="sxs-lookup"><span data-stu-id="34933-131">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="34933-132">Versioner är obligatoriska och hello aktuella versionen är `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="34933-132">Versions are mandatory and hello current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-133">Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som hello api-versionen.</span><span class="sxs-lookup"><span data-stu-id="34933-133">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="34933-134">Det här formatet stöds inte längre och kommer att inaktualiseras i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="34933-134">This format is no longer supported and will be deprecated in hello future.</span></span>

<span data-ttu-id="34933-135">När vi lägger till nya versioner kan äldre versioner fortfarande användas för kompatibilitet om skripten har beroenden på specifika dataformat.</span><span class="sxs-lookup"><span data-stu-id="34933-135">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="34933-136">Observera dock att hello aktuella preview version(2017-03-01) inte kanske tillgänglig när hello-tjänsten är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="34933-136">However, note that hello current preview version(2017-03-01) may not be available once hello service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="34933-137">Med hjälp av rubriker</span><span class="sxs-lookup"><span data-stu-id="34933-137">Using Headers</span></span>
<span data-ttu-id="34933-138">När du frågar hello instans Metadata tjänsten måste du ange hello huvud `Metadata: true` tooensure hello begäran omdirigerades inte oavsiktligt.</span><span class="sxs-lookup"><span data-stu-id="34933-138">When you query hello Instance Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="34933-139">Hämta metadata</span><span class="sxs-lookup"><span data-stu-id="34933-139">Retrieving metadata</span></span>

<span data-ttu-id="34933-140">Metadata för instansen är tillgängliga för att köra virtuella datorer skapas/hanteras med hjälp av [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="34933-140">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="34933-141">Komma åt alla datakategorier för en virtuell dator-instans med hello på begäran:</span><span class="sxs-lookup"><span data-stu-id="34933-141">Access all data categories for a virtual machine instance using hello following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="34933-142">Alla metadatafrågor för instansen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="34933-142">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="34933-143">Utdata</span><span class="sxs-lookup"><span data-stu-id="34933-143">Data output</span></span>
<span data-ttu-id="34933-144">Som standard hello instans Metadata tjänsten returnerar data i JSON-format (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="34933-144">By default, hello Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="34933-145">Dock kan olika API: er returnera data i olika format om begärt.</span><span class="sxs-lookup"><span data-stu-id="34933-145">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="34933-146">hello är följande tabell en referens för andra dataformat stöder API: er.</span><span class="sxs-lookup"><span data-stu-id="34933-146">hello following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="34933-147">API</span><span class="sxs-lookup"><span data-stu-id="34933-147">API</span></span> | <span data-ttu-id="34933-148">Standardformatet för Data</span><span class="sxs-lookup"><span data-stu-id="34933-148">Default Data Format</span></span> | <span data-ttu-id="34933-149">Andra format</span><span class="sxs-lookup"><span data-stu-id="34933-149">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="34933-150">/Instance</span><span class="sxs-lookup"><span data-stu-id="34933-150">/instance</span></span> | <span data-ttu-id="34933-151">JSON</span><span class="sxs-lookup"><span data-stu-id="34933-151">json</span></span> | <span data-ttu-id="34933-152">Text</span><span class="sxs-lookup"><span data-stu-id="34933-152">text</span></span>
<span data-ttu-id="34933-153">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="34933-153">/scheduledevents</span></span> | <span data-ttu-id="34933-154">JSON</span><span class="sxs-lookup"><span data-stu-id="34933-154">json</span></span> | <span data-ttu-id="34933-155">Ingen</span><span class="sxs-lookup"><span data-stu-id="34933-155">none</span></span>

<span data-ttu-id="34933-156">tooaccess ett svarsformat som inte är standard, ange hello begärda formatet som en querystring-parameter i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="34933-156">tooaccess a non-default response format, specify hello requested format as a querystring parameter in hello request.</span></span> <span data-ttu-id="34933-157">Exempel:</span><span class="sxs-lookup"><span data-stu-id="34933-157">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="34933-158">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="34933-158">Security</span></span>
<span data-ttu-id="34933-159">hello instans Metadata tjänstslutpunkten är enbart tillgänglig från hello som kör virtuella instans på en icke-dirigerbara IP-adress.</span><span class="sxs-lookup"><span data-stu-id="34933-159">hello Instance Metadata Service endpoint is accessible only from within hello running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="34933-160">Dessutom begäran med en `X-Forwarded-For` huvud avvisas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34933-160">In addition, any request with a `X-Forwarded-For` header is rejected by hello service.</span></span>
<span data-ttu-id="34933-161">Vi behöver också begäranden toocontain en `Metadata: true` huvud tooensure som hello faktiska begäran var direkt avsedda och inte en del av oavsiktlig omdirigering.</span><span class="sxs-lookup"><span data-stu-id="34933-161">We also require requests toocontain a `Metadata: true` header tooensure that hello actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="34933-162">Fel</span><span class="sxs-lookup"><span data-stu-id="34933-162">Error</span></span>
<span data-ttu-id="34933-163">Om det finns en dataelement som inte hittades eller en felaktig begäran, returnerar hello instans Metadata Service standard HTTP-fel.</span><span class="sxs-lookup"><span data-stu-id="34933-163">If there is a data element not found or a malformed request, hello Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="34933-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="34933-164">For example:</span></span>

<span data-ttu-id="34933-165">HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="34933-165">HTTP Status Code</span></span> | <span data-ttu-id="34933-166">Orsak</span><span class="sxs-lookup"><span data-stu-id="34933-166">Reason</span></span>
----------------|-------
<span data-ttu-id="34933-167">200 OK</span><span class="sxs-lookup"><span data-stu-id="34933-167">200 OK</span></span> |
<span data-ttu-id="34933-168">400 Felaktig förfrågan</span><span class="sxs-lookup"><span data-stu-id="34933-168">400 Bad Request</span></span> | <span data-ttu-id="34933-169">Saknas `Metadata: true` sidhuvud</span><span class="sxs-lookup"><span data-stu-id="34933-169">Missing `Metadata: true` header</span></span>
<span data-ttu-id="34933-170">404 Hittades inte</span><span class="sxs-lookup"><span data-stu-id="34933-170">404 Not Found</span></span> | <span data-ttu-id="34933-171">Hej does't begärt element finns</span><span class="sxs-lookup"><span data-stu-id="34933-171">hello requested element does't exist</span></span> 
<span data-ttu-id="34933-172">405 Metoden tillåts inte</span><span class="sxs-lookup"><span data-stu-id="34933-172">405 Method Not Allowed</span></span> | <span data-ttu-id="34933-173">Endast `GET` och `POST` stöds</span><span class="sxs-lookup"><span data-stu-id="34933-173">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="34933-174">429 för många begäranden</span><span class="sxs-lookup"><span data-stu-id="34933-174">429 Too Many Requests</span></span> | <span data-ttu-id="34933-175">hello API stöder för närvarande högst 5 frågor per sekund</span><span class="sxs-lookup"><span data-stu-id="34933-175">hello API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="34933-176">500 tjänstfel</span><span class="sxs-lookup"><span data-stu-id="34933-176">500 Service Error</span></span>     | <span data-ttu-id="34933-177">Försök igen efter en stund</span><span class="sxs-lookup"><span data-stu-id="34933-177">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="34933-178">Exempel</span><span class="sxs-lookup"><span data-stu-id="34933-178">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-179">Alla API-svar är JSON-strängar.</span><span class="sxs-lookup"><span data-stu-id="34933-179">All API responses are JSON strings.</span></span> <span data-ttu-id="34933-180">Alla följande exempel svar är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="34933-180">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="34933-181">Hämtar nätverksinformation</span><span class="sxs-lookup"><span data-stu-id="34933-181">Retrieving network information</span></span>

<span data-ttu-id="34933-182">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-182">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="34933-183">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-183">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-184">hello svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="34933-184">hello response is a JSON string.</span></span> <span data-ttu-id="34933-185">följande exempelsvar hello är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="34933-185">hello following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="34933-186">Hämta offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="34933-186">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="34933-187">Hämta alla metadata för en instans</span><span class="sxs-lookup"><span data-stu-id="34933-187">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="34933-188">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-188">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="34933-189">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-189">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-190">hello svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="34933-190">hello response is a JSON string.</span></span> <span data-ttu-id="34933-191">följande exempelsvar hello är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="34933-191">hello following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="34933-192">Hämtning av metadata i Windows-dator</span><span class="sxs-lookup"><span data-stu-id="34933-192">Retrieving metadata in Windows Virtual Machine</span></span>

<span data-ttu-id="34933-193">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-193">**Request**</span></span>

<span data-ttu-id="34933-194">Instansen metadata kan hämtas i Windows via hello PowerShell verktyget `curl`:</span><span class="sxs-lookup"><span data-stu-id="34933-194">Instance metadata can be retrieved in Windows via hello PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="34933-195">Eller via hello `Invoke-RestMethod` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="34933-195">Or through hello `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="34933-196">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-196">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-197">hello svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="34933-197">hello response is a JSON string.</span></span> <span data-ttu-id="34933-198">följande exempelsvar hello är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="34933-198">hello following example response  is pretty-printed for readability.</span></span>

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

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="34933-199">Instansen metadata datakategorier</span><span class="sxs-lookup"><span data-stu-id="34933-199">Instance metadata data categories</span></span>
<span data-ttu-id="34933-200">hello följande datakategorier finns tillgängliga hello instans Metadata tjänsten:</span><span class="sxs-lookup"><span data-stu-id="34933-200">hello following data categories are available through hello Instance Metadata Service:</span></span>

<span data-ttu-id="34933-201">Data</span><span class="sxs-lookup"><span data-stu-id="34933-201">Data</span></span> | <span data-ttu-id="34933-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="34933-202">Description</span></span>
-----|------------
<span data-ttu-id="34933-203">location</span><span class="sxs-lookup"><span data-stu-id="34933-203">location</span></span> | <span data-ttu-id="34933-204">Azure Region hello virtuell dator som körs</span><span class="sxs-lookup"><span data-stu-id="34933-204">Azure Region hello VM is running in</span></span>
<span data-ttu-id="34933-205">namn</span><span class="sxs-lookup"><span data-stu-id="34933-205">name</span></span> | <span data-ttu-id="34933-206">Namnet på hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-206">Name of hello VM</span></span> 
<span data-ttu-id="34933-207">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="34933-207">offer</span></span> | <span data-ttu-id="34933-208">Ger information om hello VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="34933-208">Offer information for hello VM image.</span></span> <span data-ttu-id="34933-209">Det här värdet finns bara för avbildningar som distribueras från Azure-avbildning gallery.</span><span class="sxs-lookup"><span data-stu-id="34933-209">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="34933-210">Publisher</span><span class="sxs-lookup"><span data-stu-id="34933-210">publisher</span></span> | <span data-ttu-id="34933-211">Utgivaren av hello VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="34933-211">Publisher of hello VM image</span></span>
<span data-ttu-id="34933-212">SKU</span><span class="sxs-lookup"><span data-stu-id="34933-212">sku</span></span> | <span data-ttu-id="34933-213">Specifika SKU för hello VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="34933-213">Specific SKU for hello VM image</span></span>  
<span data-ttu-id="34933-214">Version</span><span class="sxs-lookup"><span data-stu-id="34933-214">version</span></span> | <span data-ttu-id="34933-215">Version av hello VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="34933-215">Version of hello VM image</span></span> 
<span data-ttu-id="34933-216">osType</span><span class="sxs-lookup"><span data-stu-id="34933-216">osType</span></span> | <span data-ttu-id="34933-217">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="34933-217">Linux or Windows</span></span> 
<span data-ttu-id="34933-218">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="34933-218">platformUpdateDomain</span></span> |  <span data-ttu-id="34933-219">[Uppdateringsdomän](virtual-machines-windows-manage-availability.md) hello VM körs i</span><span class="sxs-lookup"><span data-stu-id="34933-219">[Update domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="34933-220">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="34933-220">platformFaultDomain</span></span> | <span data-ttu-id="34933-221">[Feldomänen](virtual-machines-windows-manage-availability.md) hello VM körs i</span><span class="sxs-lookup"><span data-stu-id="34933-221">[Fault domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="34933-222">vmId</span><span class="sxs-lookup"><span data-stu-id="34933-222">vmId</span></span> | <span data-ttu-id="34933-223">[Unik identifierare](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) för hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-223">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for hello VM</span></span>
<span data-ttu-id="34933-224">vmSize</span><span class="sxs-lookup"><span data-stu-id="34933-224">vmSize</span></span> | [<span data-ttu-id="34933-225">VM-storlek</span><span class="sxs-lookup"><span data-stu-id="34933-225">VM size</span></span>](virtual-machines-windows-sizes.md)
<span data-ttu-id="34933-226">IPv4/privateIpAddress</span><span class="sxs-lookup"><span data-stu-id="34933-226">ipv4/privateIpAddress</span></span> | <span data-ttu-id="34933-227">Lokala IPv4-adressen för hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-227">Local IPv4 address of hello VM</span></span> 
<span data-ttu-id="34933-228">IPv4/publicIpAddress</span><span class="sxs-lookup"><span data-stu-id="34933-228">ipv4/publicIpAddress</span></span> | <span data-ttu-id="34933-229">Offentliga IPv4-adressen för hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-229">Public IPv4 address of hello VM</span></span>
<span data-ttu-id="34933-230">undernätet och/eller adress</span><span class="sxs-lookup"><span data-stu-id="34933-230">subnet/address</span></span> | <span data-ttu-id="34933-231">Undernätsadress av hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-231">Subnet address of hello VM</span></span>
<span data-ttu-id="34933-232">undernätsprefixet /</span><span class="sxs-lookup"><span data-stu-id="34933-232">subnet/prefix</span></span> | <span data-ttu-id="34933-233">Undernätets prefix, exempel 24</span><span class="sxs-lookup"><span data-stu-id="34933-233">Subnet prefix, example 24</span></span>
<span data-ttu-id="34933-234">IPv6/IP-adress</span><span class="sxs-lookup"><span data-stu-id="34933-234">ipv6/ipAddress</span></span> | <span data-ttu-id="34933-235">Den lokala IPv6-adressen för hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-235">Local IPv6 address of hello VM</span></span>
<span data-ttu-id="34933-236">MAC-adress</span><span class="sxs-lookup"><span data-stu-id="34933-236">macAddress</span></span> | <span data-ttu-id="34933-237">VM mac-adress</span><span class="sxs-lookup"><span data-stu-id="34933-237">VM mac address</span></span> 
<span data-ttu-id="34933-238">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="34933-238">scheduledevents</span></span> | <span data-ttu-id="34933-239">För närvarande finns i Public Preview finns [scheduledevents](virtual-machines-scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="34933-239">Currently in Public Preview See [scheduledevents](virtual-machines-scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="34933-240">Exempelscenarier för användning</span><span class="sxs-lookup"><span data-stu-id="34933-240">Example Scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="34933-241">Spårning av virtuell dator som kör på Azure</span><span class="sxs-lookup"><span data-stu-id="34933-241">Tracking VM running on Azure</span></span>

<span data-ttu-id="34933-242">Du kan behöva tootrack hello antalet virtuella datorer som kör programvaran eller agenter som behöver tootrack unika hello VM som en tjänstprovider.</span><span class="sxs-lookup"><span data-stu-id="34933-242">As a service provider, you may require tootrack hello number of VMs running your software or have agents that need tootrack uniqueness of hello VM.</span></span> <span data-ttu-id="34933-243">toobe kan tooget ett unikt ID för en virtuell dator, Använd hello `vmId` från Metadata-tjänsten för instansen.</span><span class="sxs-lookup"><span data-stu-id="34933-243">toobe able tooget a unique ID for a VM, use hello `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="34933-244">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-244">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="34933-245">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-245">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="34933-246">Placering av behållare, partitioner data baserat fel/uppdatera domän</span><span class="sxs-lookup"><span data-stu-id="34933-246">Placement of containers, data-partitions based Fault/Update domain</span></span> 

<span data-ttu-id="34933-247">För vissa scenarier, placering av olika repliker är av yttersta vikt.</span><span class="sxs-lookup"><span data-stu-id="34933-247">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="34933-248">Till exempel [HDFS replik placering](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) eller placering av behållare via en [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kan du kräva tooknow hello `platformFaultDomain` och `platformUpdateDomain` hello virtuella datorn körs på.</span><span class="sxs-lookup"><span data-stu-id="34933-248">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require tooknow hello `platformFaultDomain` and `platformUpdateDomain` hello VM is running on.</span></span>
<span data-ttu-id="34933-249">Du kan fråga data direkt via hello instans Metadata-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34933-249">You can query this data directly via hello Instance Metadata Service.</span></span>

<span data-ttu-id="34933-250">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-250">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="34933-251">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-251">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a><span data-ttu-id="34933-252">Mer information om hello VM under supportärende</span><span class="sxs-lookup"><span data-stu-id="34933-252">Getting more information about hello VM during support case</span></span> 

<span data-ttu-id="34933-253">Som en leverantör får du ett supportsamtal där du vill ha tooknow mer information om hello VM.</span><span class="sxs-lookup"><span data-stu-id="34933-253">As a service provider, you may get a support call where you would like tooknow more information about hello VM.</span></span> <span data-ttu-id="34933-254">Frågar hello kunden tooshare innehåller hello beräkning metadata grundläggande information för hello support professional tooknow hello typ av VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="34933-254">Asking hello customer tooshare hello compute metadata can provide basic information for hello support professional tooknow about hello kind of VM on Azure.</span></span> 

<span data-ttu-id="34933-255">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="34933-255">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="34933-256">**Svar**</span><span class="sxs-lookup"><span data-stu-id="34933-256">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="34933-257">hello svaret är en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="34933-257">hello response is a JSON string.</span></span> <span data-ttu-id="34933-258">följande exempelsvar hello är pretty ut för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="34933-258">hello following example response is pretty-printed for readability.</span></span>

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a><span data-ttu-id="34933-259">Exempel på hur metadatatjänsten som använder olika språk i hello VM</span><span class="sxs-lookup"><span data-stu-id="34933-259">Examples of calling metadata service using different languages inside hello VM</span></span> 

<span data-ttu-id="34933-260">Språk</span><span class="sxs-lookup"><span data-stu-id="34933-260">Language</span></span> | <span data-ttu-id="34933-261">Exempel</span><span class="sxs-lookup"><span data-stu-id="34933-261">Example</span></span> 
---------|----------------
<span data-ttu-id="34933-262">Ruby</span><span class="sxs-lookup"><span data-stu-id="34933-262">Ruby</span></span>     | <span data-ttu-id="34933-263">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="34933-263">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="34933-264">Gå Lan</span><span class="sxs-lookup"><span data-stu-id="34933-264">Go Lan</span></span>   | <span data-ttu-id="34933-265">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="34933-265">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="34933-266">python</span><span class="sxs-lookup"><span data-stu-id="34933-266">python</span></span>   | <span data-ttu-id="34933-267">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="34933-267">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="34933-268">C++</span><span class="sxs-lookup"><span data-stu-id="34933-268">C++</span></span>      | <span data-ttu-id="34933-269">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="34933-269">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="34933-270">C#</span><span class="sxs-lookup"><span data-stu-id="34933-270">C#</span></span>       | <span data-ttu-id="34933-271">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.CS</span><span class="sxs-lookup"><span data-stu-id="34933-271">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="34933-272">Javascript</span><span class="sxs-lookup"><span data-stu-id="34933-272">Javascript</span></span> | <span data-ttu-id="34933-273">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="34933-273">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="34933-274">PowerShell</span><span class="sxs-lookup"><span data-stu-id="34933-274">Powershell</span></span> | <span data-ttu-id="34933-275">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="34933-275">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="34933-276">Bash</span><span class="sxs-lookup"><span data-stu-id="34933-276">Bash</span></span>       | <span data-ttu-id="34933-277">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="34933-277">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="34933-278">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="34933-278">FAQ</span></span>
1. <span data-ttu-id="34933-279">Jag får hello fel `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="34933-279">I am getting hello error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="34933-280">Vad innebär det?</span><span class="sxs-lookup"><span data-stu-id="34933-280">What does this mean?</span></span>
   * <span data-ttu-id="34933-281">hello instans Metadata tjänsten kräver hello huvud `Metadata: true` toobe skickades hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="34933-281">hello Instance Metadata Service requires hello header `Metadata: true` toobe passed in hello request.</span></span> <span data-ttu-id="34933-282">Skicka det här sidhuvudet i hello REST-anrop kan åtkomst toohello instans Metadata-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34933-282">Passing this header in hello REST call allows access toohello Instance Metadata Service.</span></span> 
2. <span data-ttu-id="34933-283">Varför inte inträffar beräknings-information för den virtuella datorn?</span><span class="sxs-lookup"><span data-stu-id="34933-283">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="34933-284">Hello instans Metadata tjänsten stöder för närvarande bara instanser som skapats med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="34933-284">Currently hello Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="34933-285">Hello framtida, kan vi lägga till stöd för virtuella datorer för molnet tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34933-285">In hello future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="34933-286">Jag har skapat Min virtuella dator via Azure Resource Manager en tid sedan.</span><span class="sxs-lookup"><span data-stu-id="34933-286">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="34933-287">Varför kan jag inte se compute metadatainformation?</span><span class="sxs-lookup"><span data-stu-id="34933-287">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="34933-288">För alla virtuella datorer som skapats efter Sep 2016, lägga till en [taggen](../azure-resource-manager/resource-group-using-tags.md) toostart Se beräkning metadata.</span><span class="sxs-lookup"><span data-stu-id="34933-288">For any VMs created after Sep 2016, add a [Tag](../azure-resource-manager/resource-group-using-tags.md) toostart seeing compute metadata.</span></span> <span data-ttu-id="34933-289">För äldre virtuella datorer (som skapats före Sep 2016), Lägg till/ta bort tillägg eller data diskar toohello VM toorefresh metadata.</span><span class="sxs-lookup"><span data-stu-id="34933-289">For older VMs (created before Sep 2016), add/remove extensions or data disks toohello VM toorefresh metadata.</span></span>
4. <span data-ttu-id="34933-290">Varför får hello fel `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="34933-290">Why am I getting hello error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="34933-291">Försök att utföra din begäran utifrån exponentiell tillbaka av systemet.</span><span class="sxs-lookup"><span data-stu-id="34933-291">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="34933-292">Kontakta Azure-supporten om hello problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="34933-292">If hello issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="34933-293">Där delar ytterligare frågor/kommentarer?</span><span class="sxs-lookup"><span data-stu-id="34933-293">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="34933-294">Skicka kommentarer om http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="34933-294">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="34933-295">Fungerar detta för Virtual Machine Scale ange instansen?</span><span class="sxs-lookup"><span data-stu-id="34933-295">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="34933-296">Ja är Metadata-tjänsten tillgänglig för skala ange instanser.</span><span class="sxs-lookup"><span data-stu-id="34933-296">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="34933-297">Hur får jag support för hello tjänsten?</span><span class="sxs-lookup"><span data-stu-id="34933-297">How do I get support for hello service?</span></span>
   * <span data-ttu-id="34933-298">tooget stöd för hello-tjänsten, skapa ett supportproblem i Azure portal för hello VM där du inte är kan tooget metadata svar efter lång försök</span><span class="sxs-lookup"><span data-stu-id="34933-298">tooget support for hello service, create a support issue in Azure portal for hello VM where you are not able tooget metadata response after long retries</span></span> 

   ![Stöd för instans-Metadata](./media/virtual-machines-instancemetadataservice-overview/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="34933-300">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34933-300">Next Steps</span></span>

- <span data-ttu-id="34933-301">Mer information om hello [scheduledevents](virtual-machines-scheduled-events.md) API **i Public Preview** tillhandahålls av hello instans Metadata-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="34933-301">Learn more about hello [scheduledevents](virtual-machines-scheduled-events.md) API **In Public Preview** provided by hello Instance Metadata Service.</span></span>
