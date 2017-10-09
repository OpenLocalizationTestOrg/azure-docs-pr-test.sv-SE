---
title: "aaaAzure asynkrona åtgärder | Microsoft Docs"
description: "Beskriver hur tootrack asynkrona åtgärder i Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: b81254196013adf87998eff11a50993efa52d40d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="track-asynchronous-azure-operations"></a>Spåra asynkrona åtgärder i Azure
Vissa Azure REST-åtgärder körs asynkront eftersom hello-åtgärden inte kan slutföras snabbt. Det här avsnittet beskrivs hur tootrack hello status för asynkrona åtgärder via värden returneras i hello svar.  

## <a name="status-codes-for-asynchronous-operations"></a>Statuskoder för asynkrona åtgärder
En asynkron åtgärd returnerar först en HTTP-statuskod antingen:

* 201 (skapad)
* 202 (accepterad) 

När hello-åtgärden har slutförts, returnerar antingen:

* 200 (OK)
* 204 (inget innehåll) 

Se toohello [REST API-dokumentation](/rest/api/) toosee hello svar för hello åtgärden som du kör. 

## <a name="monitor-status-of-operation"></a>Övervaka status för åtgärden
hello asynkron REST operations returnera huvudvärden som du använder toodetermine hello status för hello igen. Det finns potentiellt tre sidhuvud värden tooexamine:

* `Azure-AsyncOperation`-URL för att kontrollera hello pågående status för hello-åtgärd. Om åtgärden returnerar det här värdet måste alltid använda it (i stället för plats) tootrack hello status för hello-åtgärd.
* `Location`-URL för att avgöra när en åtgärd har slutförts. Använd det här värdet bara när Azure-asynkrona åtgärder inte returneras.
* `Retry-After`-hello antal sekunder toowait innan hello status för hello asynkron åtgärd.

Men returnerar inte varje asynkron åtgärd dessa värden. Exempelvis kan du behöva tooevaluate hello Azure-asynkrona åtgärder huvudets värde för en åtgärd och hello plats huvudvärde för en annan åtgärd. 

Du kan hämta hello huvudvärden som du vill hämta alla huvudvärde för en begäran. Till exempel i C# kan du hämta hello huvudvärde från en `HttpWebResponse` objekt med namnet `response` med hello följande kod:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure-asynkrona åtgärder förfrågan och svar

tooget hello status för hello asynkron åtgärd, skickar en GET-begäran toohello URL i Azure-asynkrona åtgärder huvudvärde.

hello innehåller hello svar från den här åtgärden information om hello igen. hello visar följande exempel hello möjliga värden som returneras från hello-åtgärd:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

Endast `status` returneras för alla svar. hello felobjekt returneras när hello status är misslyckad eller avbruten. Alla andra värden är valfritt. därför se hello svar du får annorlunda ut än hello exempel.

## <a name="provisioningstate-values"></a>provisioningState värden

Åtgärder som att skapa, uppdatera eller ta bort (PUT, korrigering, ta bort) en resurs vanligtvis returnera en `provisioningState` värde. När en åtgärd har slutförts, returneras ett av följande tre värden: 

* Lyckades
* Det gick inte
* Avbrutna

Alla andra värden indikera hello-åtgärden körs fortfarande. Hej resursprovidern kan returnera ett anpassat värde som anger dess tillstånd. Du kan till exempel få **godkända** när hello begäran har tagits emot och körs.

## <a name="example-requests-and-responses"></a>Exempel och -svar

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Starta den virtuella datorn (202 med Azure-asynkrona åtgärder)
Det här exemplet illustrerar hur toodetermine hello status för **starta** åtgärden för virtuella datorer. hello första begäran har hello följande format:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Den returnerar statuskod 202. Bland hello huvudvärden visas:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

toocheck hello status för hello asynkron åtgärd, skickar ett annat toothat URL-begäran.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

hello svarstexten innehåller hello status för hello åtgärd:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Distribuera resurser (201 med Azure-asynkrona åtgärder)

Det här exemplet illustrerar hur toodetermine hello status för **distributioner** åtgärden för att distribuera resurser tooAzure. hello första begäran har hello följande format:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

Den returnerar statuskod 201. hello brödtexten i svaret hello innehåller:

```json
"provisioningState":"Accepted",
```

Bland hello huvudvärden visas:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

toocheck hello status för hello asynkron åtgärd, skickar ett annat toothat URL-begäran.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

hello svarstexten innehåller hello status för hello åtgärd:

```json
{"status":"Running"}
```

När hello distributionen är klar innehåller hello svar:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Skapa lagringskonto (202 med plats och försök igen efter)

Det här exemplet illustrerar hur toodetermine hello status för hello **skapa** åtgärden för storage-konton. hello första begäran har hello följande format:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

Och hello begärandetexten innehåller egenskaper för hello storage-konto:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Den returnerar statuskod 202. Bland hello huvudvärden finns hello följande två värden:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Efter antal sekunder som anges i försök igen efter markerar hello status för hello asynkron åtgärd genom att skicka en annan toothat URL-begäran.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

Om hello begäran körs får du en statuskod 202. Om hello begäran har slutförts, din får en statuskod 200 och hello hello svaret innehåller hello egenskaper för hello storage-konto som har skapats.

## <a name="next-steps"></a>Nästa steg

* Dokumentation om varje REST-åtgärd finns [REST API-dokumentation](/rest/api/).
* Information om hur du hanterar resurser via hello Resource Manager REST-API finns [Using hello Resource Manager REST API](resource-manager-rest-api.md).
* information om hur du distribuerar mallar via hello Resource Manager REST-API finns [distribuera resurser med Resource Manager-mallar och Resource Manager REST API](resource-group-template-deploy-rest.md).
