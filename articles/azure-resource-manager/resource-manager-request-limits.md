---
title: "aaaAzure hanteraren begärandebegränsningar | Microsoft Docs"
description: "Beskriver hur toouse begränsning med Azure Resource Manager begär när prenumerationsbegränsningar har uppnåtts."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: ea274f3145af36aac753730e1f280d8a8b59c3bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-resource-manager-requests"></a><span data-ttu-id="0591c-103">Begränsning av Resource Manager-begäranden</span><span class="sxs-lookup"><span data-stu-id="0591c-103">Throttling Resource Manager requests</span></span>
<span data-ttu-id="0591c-104">För varje prenumeration och klient Resource Manager gränser läsa begäranden too15, 000 per timme och skriva begäranden too1, 200 per timme.</span><span class="sxs-lookup"><span data-stu-id="0591c-104">For each subscription and tenant, Resource Manager limits read requests too15,000 per hour and write requests too1,200 per hour.</span></span> <span data-ttu-id="0591c-105">Dessa gränser gäller tooeach Azure Resource Manager-instans. Det finns flera instanser i varje Azure-region och Azure Resource Manager distribuerade tooall Azure regioner.</span><span class="sxs-lookup"><span data-stu-id="0591c-105">These limits apply tooeach Azure Resource Manager instance; there are multiple instances in every Azure region, and Azure Resource Manager is deployed tooall Azure regions.</span></span>  <span data-ttu-id="0591c-106">Så i praktiken gränserna är effektivt mycket högre än de som visas ovan, som användare hanteras begäranden vanligtvis av många olika instanser.</span><span class="sxs-lookup"><span data-stu-id="0591c-106">So, in practice, limits are effectively much higher than those listed above, as user requests are generally serviced by many different instances.</span></span>

<span data-ttu-id="0591c-107">Om ditt program eller skript når dessa begränsningar, måste toothrottle dina önskemål.</span><span class="sxs-lookup"><span data-stu-id="0591c-107">If your application or script reaches these limits, you need toothrottle your requests.</span></span> <span data-ttu-id="0591c-108">Det här avsnittet beskrivs hur toodetermine hello återstående begär du har innan det nådde hello gränsen och hur toorespond när hello gränsen har nåtts.</span><span class="sxs-lookup"><span data-stu-id="0591c-108">This topic shows you how toodetermine hello remaining requests you have before reaching hello limit, and how toorespond when you have reached hello limit.</span></span>

<span data-ttu-id="0591c-109">När du når hello gräns kan du få hello HTTP-statuskod **429 för många begäranden**.</span><span class="sxs-lookup"><span data-stu-id="0591c-109">When you reach hello limit, you receive hello HTTP status code **429 Too many requests**.</span></span>

<span data-ttu-id="0591c-110">Antal begäranden hello är begränsade tooeither prenumerationen eller din klient.</span><span class="sxs-lookup"><span data-stu-id="0591c-110">hello number of requests is scoped tooeither your subscription or your tenant.</span></span> <span data-ttu-id="0591c-111">Om du har flera adderas samtidiga program som skickar begäranden i din prenumeration hello begäranden från programmen toodetermine hello antalet återstående begäranden.</span><span class="sxs-lookup"><span data-stu-id="0591c-111">If you have multiple, concurrent applications making requests in your subscription, hello requests from those applications are added together toodetermine hello number of remaining requests.</span></span>

<span data-ttu-id="0591c-112">Som omfattar-prenumerationsbegäranden är de som hello innebär att skicka ditt prenumerations-id, till exempel hämtar hello resursgrupper i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0591c-112">Subscription scoped requests are ones hello involve passing your subscription id, such as retrieving hello resource groups in your subscription.</span></span> <span data-ttu-id="0591c-113">Klient omfång begäranden inkluderar inte ditt prenumerations-id, till exempel hämtar giltig Azure platser.</span><span class="sxs-lookup"><span data-stu-id="0591c-113">Tenant scoped requests do not include your subscription id, such as retrieving valid Azure locations.</span></span>

## <a name="remaining-requests"></a><span data-ttu-id="0591c-114">Återstående begäranden</span><span class="sxs-lookup"><span data-stu-id="0591c-114">Remaining requests</span></span>
<span data-ttu-id="0591c-115">Du kan fastställa hello antalet återstående begäranden genom att undersöka svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="0591c-115">You can determine hello number of remaining requests by examining response headers.</span></span> <span data-ttu-id="0591c-116">Varje begäran innehåller värden för hello antalet återstående Läs- och skrivbegäranden.</span><span class="sxs-lookup"><span data-stu-id="0591c-116">Each request includes values for hello number of remaining read and write requests.</span></span> <span data-ttu-id="0591c-117">hello beskrivs följande tabell hello svarshuvuden som du kan undersöka för dessa värden:</span><span class="sxs-lookup"><span data-stu-id="0591c-117">hello following table describes hello response headers you can examine for those values:</span></span>

| <span data-ttu-id="0591c-118">Svarshuvud</span><span class="sxs-lookup"><span data-stu-id="0591c-118">Response header</span></span> | <span data-ttu-id="0591c-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0591c-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0591c-120">x-MS-ratelimit-Remaining-Subscription-Reads</span><span class="sxs-lookup"><span data-stu-id="0591c-120">x-ms-ratelimit-remaining-subscription-reads</span></span> |<span data-ttu-id="0591c-121">Prenumeration som omfattar läser återstående</span><span class="sxs-lookup"><span data-stu-id="0591c-121">Subscription scoped reads remaining</span></span> |
| <span data-ttu-id="0591c-122">x-MS-ratelimit-Remaining-Subscription-Writes</span><span class="sxs-lookup"><span data-stu-id="0591c-122">x-ms-ratelimit-remaining-subscription-writes</span></span> |<span data-ttu-id="0591c-123">Prenumeration som omfattar skriver återstående</span><span class="sxs-lookup"><span data-stu-id="0591c-123">Subscription scoped writes remaining</span></span> |
| <span data-ttu-id="0591c-124">x-MS-ratelimit-Remaining-tenant-Reads</span><span class="sxs-lookup"><span data-stu-id="0591c-124">x-ms-ratelimit-remaining-tenant-reads</span></span> |<span data-ttu-id="0591c-125">Klient omfång läser återstående</span><span class="sxs-lookup"><span data-stu-id="0591c-125">Tenant scoped reads remaining</span></span> |
| <span data-ttu-id="0591c-126">x-MS-ratelimit-Remaining-tenant-Writes</span><span class="sxs-lookup"><span data-stu-id="0591c-126">x-ms-ratelimit-remaining-tenant-writes</span></span> |<span data-ttu-id="0591c-127">Klient omfång skriver återstående</span><span class="sxs-lookup"><span data-stu-id="0591c-127">Tenant scoped writes remaining</span></span> |
| <span data-ttu-id="0591c-128">x-MS-ratelimit-Remaining-Subscription-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="0591c-128">x-ms-ratelimit-remaining-subscription-resource-requests</span></span> |<span data-ttu-id="0591c-129">Prenumerationen omfattas resurs typen begäranden kvar.</span><span class="sxs-lookup"><span data-stu-id="0591c-129">Subscription scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="0591c-130">Värdet i huvudet returneras bara om en tjänst har åsidosatt hello Standardgränsen.</span><span class="sxs-lookup"><span data-stu-id="0591c-130">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="0591c-131">Resource Manager lägger till det här värdet i stället för hello prenumeration läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="0591c-131">Resource Manager adds this value instead of hello subscription reads or writes.</span></span> |
| <span data-ttu-id="0591c-132">x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="0591c-132">x-ms-ratelimit-remaining-subscription-resource-entities-read</span></span> |<span data-ttu-id="0591c-133">Prenumerationen omfattas resurs typen begäranden om diagnostikdatainsamling kvar.</span><span class="sxs-lookup"><span data-stu-id="0591c-133">Subscription scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="0591c-134">Värdet i huvudet returneras bara om en tjänst har åsidosatt hello Standardgränsen.</span><span class="sxs-lookup"><span data-stu-id="0591c-134">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="0591c-135">Det här värdet anger hello antalet återstående begäranden om diagnostikdatainsamling (lista över resurser).</span><span class="sxs-lookup"><span data-stu-id="0591c-135">This value provides hello number of remaining collection requests (list resources).</span></span> |
| <span data-ttu-id="0591c-136">x-MS-ratelimit-Remaining-tenant-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="0591c-136">x-ms-ratelimit-remaining-tenant-resource-requests</span></span> |<span data-ttu-id="0591c-137">Klient omfång resurs typen begäranden kvar.</span><span class="sxs-lookup"><span data-stu-id="0591c-137">Tenant scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="0591c-138">Den här rubriken läggs endast för begäranden på klient-nivå och endast om en tjänst har åsidosatt hello Standardgränsen.</span><span class="sxs-lookup"><span data-stu-id="0591c-138">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> <span data-ttu-id="0591c-139">Resource Manager lägger till det här värdet i stället för hello klient läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="0591c-139">Resource Manager adds this value instead of hello tenant reads or writes.</span></span> |
| <span data-ttu-id="0591c-140">x-MS-ratelimit-Remaining-tenant-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="0591c-140">x-ms-ratelimit-remaining-tenant-resource-entities-read</span></span> |<span data-ttu-id="0591c-141">Klient omfattas resurs typen begäranden om diagnostikdatainsamling kvar.</span><span class="sxs-lookup"><span data-stu-id="0591c-141">Tenant scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="0591c-142">Den här rubriken läggs endast för begäranden på klient-nivå och endast om en tjänst har åsidosatt hello Standardgränsen.</span><span class="sxs-lookup"><span data-stu-id="0591c-142">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> |

## <a name="retrieving-hello-header-values"></a><span data-ttu-id="0591c-143">Hämtar hello huvudvärden</span><span class="sxs-lookup"><span data-stu-id="0591c-143">Retrieving hello header values</span></span>
<span data-ttu-id="0591c-144">Hämtar dessa värden i huvudet i din kod eller skript är inte annorlunda än att hämta alla huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="0591c-144">Retrieving these header values in your code or script is no different than retrieving any header value.</span></span> 

<span data-ttu-id="0591c-145">Till exempel i **C#**, du hämta hello huvudvärde från en **HttpWebResponse** objekt med namnet **svar** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="0591c-145">For example, in **C#**, you retrieve hello header value from an **HttpWebResponse** object named **response** with hello following code:</span></span>

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

<span data-ttu-id="0591c-146">I **PowerShell**, du hämta hello huvudvärde från ett Invoke-WebRequest-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0591c-146">In **PowerShell**, you retrieve hello header value from an Invoke-WebRequest operation.</span></span>

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

<span data-ttu-id="0591c-147">Eller, om vill toosee hello återstående begäranden för felsökning kan du ange hello **-Debug** parameter på din **PowerShell** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0591c-147">Or, if want toosee hello remaining requests for debugging, you can provide hello **-Debug** parameter on your **PowerShell** cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -Debug
```

<span data-ttu-id="0591c-148">Som returnerar flera värden, inklusive hello efter Svarsvärde:</span><span class="sxs-lookup"><span data-stu-id="0591c-148">Which returns many values, including hello following response value:</span></span>

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

<span data-ttu-id="0591c-149">I **Azure CLI**, du hämta hello huvudvärde med hello mer utförlig alternativet.</span><span class="sxs-lookup"><span data-stu-id="0591c-149">In **Azure CLI**, you retrieve hello header value by using hello more verbose option.</span></span>

```azurecli
azure group list -vv --json
```

<span data-ttu-id="0591c-150">Som returnerar flera värden, inklusive hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0591c-150">Which returns many values, including hello following object:</span></span>

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a><span data-ttu-id="0591c-151">Väntar innan nästa begäran skickas</span><span class="sxs-lookup"><span data-stu-id="0591c-151">Waiting before sending next request</span></span>
<span data-ttu-id="0591c-152">När du når gränsen hello Resource Manager returnerar hello **429** HTTP-statuskoden och en **försök igen efter** värdet i hello-rubriken.</span><span class="sxs-lookup"><span data-stu-id="0591c-152">When you reach hello request limit, Resource Manager returns hello **429** HTTP status code and a **Retry-After** value in hello header.</span></span> <span data-ttu-id="0591c-153">Hej **försök igen efter** värdet anger hello antalet sekunder som programmet ska vänta (eller strömsparläge) innan du skickar nästa hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="0591c-153">hello **Retry-After** value specifies hello number of seconds your application should wait (or sleep) before sending hello next request.</span></span> <span data-ttu-id="0591c-154">Om du skickar en begäran innan hello retry-värdet har gått ut, behandlas inte din begäran och ett nytt försök värde returneras.</span><span class="sxs-lookup"><span data-stu-id="0591c-154">If you send a request before hello retry value has elapsed, your request is not processed and a new retry value is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0591c-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0591c-155">Next steps</span></span>

* <span data-ttu-id="0591c-156">Mer information om begränsningar och kvoter finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="0591c-156">For more information about limits and quotas, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
* <span data-ttu-id="0591c-157">toolearn om hantering av asynkrona REST-begäranden finns [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="0591c-157">toolearn about handling asynchronous REST requests, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
