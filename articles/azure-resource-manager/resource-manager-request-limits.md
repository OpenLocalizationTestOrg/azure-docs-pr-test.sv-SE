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
# <a name="throttling-resource-manager-requests"></a>Begränsning av Resource Manager-begäranden
För varje prenumeration och klient Resource Manager gränser läsa begäranden too15, 000 per timme och skriva begäranden too1, 200 per timme. Dessa gränser gäller tooeach Azure Resource Manager-instans. Det finns flera instanser i varje Azure-region och Azure Resource Manager distribuerade tooall Azure regioner.  Så i praktiken gränserna är effektivt mycket högre än de som visas ovan, som användare hanteras begäranden vanligtvis av många olika instanser.

Om ditt program eller skript når dessa begränsningar, måste toothrottle dina önskemål. Det här avsnittet beskrivs hur toodetermine hello återstående begär du har innan det nådde hello gränsen och hur toorespond när hello gränsen har nåtts.

När du når hello gräns kan du få hello HTTP-statuskod **429 för många begäranden**.

Antal begäranden hello är begränsade tooeither prenumerationen eller din klient. Om du har flera adderas samtidiga program som skickar begäranden i din prenumeration hello begäranden från programmen toodetermine hello antalet återstående begäranden.

Som omfattar-prenumerationsbegäranden är de som hello innebär att skicka ditt prenumerations-id, till exempel hämtar hello resursgrupper i din prenumeration. Klient omfång begäranden inkluderar inte ditt prenumerations-id, till exempel hämtar giltig Azure platser.

## <a name="remaining-requests"></a>Återstående begäranden
Du kan fastställa hello antalet återstående begäranden genom att undersöka svarshuvuden. Varje begäran innehåller värden för hello antalet återstående Läs- och skrivbegäranden. hello beskrivs följande tabell hello svarshuvuden som du kan undersöka för dessa värden:

| Svarshuvud | Beskrivning |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-Reads |Prenumeration som omfattar läser återstående |
| x-MS-ratelimit-Remaining-Subscription-Writes |Prenumeration som omfattar skriver återstående |
| x-MS-ratelimit-Remaining-tenant-Reads |Klient omfång läser återstående |
| x-MS-ratelimit-Remaining-tenant-Writes |Klient omfång skriver återstående |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests |Prenumerationen omfattas resurs typen begäranden kvar.<br /><br />Värdet i huvudet returneras bara om en tjänst har åsidosatt hello Standardgränsen. Resource Manager lägger till det här värdet i stället för hello prenumeration läsning och skrivning. |
| x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read |Prenumerationen omfattas resurs typen begäranden om diagnostikdatainsamling kvar.<br /><br />Värdet i huvudet returneras bara om en tjänst har åsidosatt hello Standardgränsen. Det här värdet anger hello antalet återstående begäranden om diagnostikdatainsamling (lista över resurser). |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests |Klient omfång resurs typen begäranden kvar.<br /><br />Den här rubriken läggs endast för begäranden på klient-nivå och endast om en tjänst har åsidosatt hello Standardgränsen. Resource Manager lägger till det här värdet i stället för hello klient läsning och skrivning. |
| x-MS-ratelimit-Remaining-tenant-Resource-entities-Read |Klient omfattas resurs typen begäranden om diagnostikdatainsamling kvar.<br /><br />Den här rubriken läggs endast för begäranden på klient-nivå och endast om en tjänst har åsidosatt hello Standardgränsen. |

## <a name="retrieving-hello-header-values"></a>Hämtar hello huvudvärden
Hämtar dessa värden i huvudet i din kod eller skript är inte annorlunda än att hämta alla huvudvärde. 

Till exempel i **C#**, du hämta hello huvudvärde från en **HttpWebResponse** objekt med namnet **svar** med hello följande kod:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

I **PowerShell**, du hämta hello huvudvärde från ett Invoke-WebRequest-åtgärd.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Eller, om vill toosee hello återstående begäranden för felsökning kan du ange hello **-Debug** parameter på din **PowerShell** cmdlet.

```powershell
Get-AzureRmResourceGroup -Debug
```

Som returnerar flera värden, inklusive hello efter Svarsvärde:

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

I **Azure CLI**, du hämta hello huvudvärde med hello mer utförlig alternativet.

```azurecli
azure group list -vv --json
```

Som returnerar flera värden, inklusive hello följande objekt:

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

## <a name="waiting-before-sending-next-request"></a>Väntar innan nästa begäran skickas
När du når gränsen hello Resource Manager returnerar hello **429** HTTP-statuskoden och en **försök igen efter** värdet i hello-rubriken. Hej **försök igen efter** värdet anger hello antalet sekunder som programmet ska vänta (eller strömsparläge) innan du skickar nästa hello-begäran. Om du skickar en begäran innan hello retry-värdet har gått ut, behandlas inte din begäran och ett nytt försök värde returneras.

## <a name="next-steps"></a>Nästa steg

* Mer information om begränsningar och kvoter finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).
* toolearn om hantering av asynkrona REST-begäranden finns [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).
