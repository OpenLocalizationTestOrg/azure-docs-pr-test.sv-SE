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
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="7b08b-103">Spåra asynkrona åtgärder i Azure</span><span class="sxs-lookup"><span data-stu-id="7b08b-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="7b08b-104">Vissa Azure REST-åtgärder körs asynkront eftersom hello-åtgärden inte kan slutföras snabbt.</span><span class="sxs-lookup"><span data-stu-id="7b08b-104">Some Azure REST operations run asynchronously because hello operation cannot be completed quickly.</span></span> <span data-ttu-id="7b08b-105">Det här avsnittet beskrivs hur tootrack hello status för asynkrona åtgärder via värden returneras i hello svar.</span><span class="sxs-lookup"><span data-stu-id="7b08b-105">This topic describes how tootrack hello status of asynchronous operations through values returned in hello response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="7b08b-106">Statuskoder för asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="7b08b-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="7b08b-107">En asynkron åtgärd returnerar först en HTTP-statuskod antingen:</span><span class="sxs-lookup"><span data-stu-id="7b08b-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="7b08b-108">201 (skapad)</span><span class="sxs-lookup"><span data-stu-id="7b08b-108">201 (Created)</span></span>
* <span data-ttu-id="7b08b-109">202 (accepterad)</span><span class="sxs-lookup"><span data-stu-id="7b08b-109">202 (Accepted)</span></span> 

<span data-ttu-id="7b08b-110">När hello-åtgärden har slutförts, returnerar antingen:</span><span class="sxs-lookup"><span data-stu-id="7b08b-110">When hello operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="7b08b-111">200 (OK)</span><span class="sxs-lookup"><span data-stu-id="7b08b-111">200 (OK)</span></span>
* <span data-ttu-id="7b08b-112">204 (inget innehåll)</span><span class="sxs-lookup"><span data-stu-id="7b08b-112">204 (No Content)</span></span> 

<span data-ttu-id="7b08b-113">Se toohello [REST API-dokumentation](/rest/api/) toosee hello svar för hello åtgärden som du kör.</span><span class="sxs-lookup"><span data-stu-id="7b08b-113">Refer toohello [REST API documentation](/rest/api/) toosee hello responses for hello operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="7b08b-114">Övervaka status för åtgärden</span><span class="sxs-lookup"><span data-stu-id="7b08b-114">Monitor status of operation</span></span>
<span data-ttu-id="7b08b-115">hello asynkron REST operations returnera huvudvärden som du använder toodetermine hello status för hello igen.</span><span class="sxs-lookup"><span data-stu-id="7b08b-115">hello asynchronous REST operations return header values, which you use toodetermine hello status of hello operation.</span></span> <span data-ttu-id="7b08b-116">Det finns potentiellt tre sidhuvud värden tooexamine:</span><span class="sxs-lookup"><span data-stu-id="7b08b-116">There are potentially three header values tooexamine:</span></span>

* <span data-ttu-id="7b08b-117">`Azure-AsyncOperation`-URL för att kontrollera hello pågående status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7b08b-117">`Azure-AsyncOperation` - URL for checking hello ongoing status of hello operation.</span></span> <span data-ttu-id="7b08b-118">Om åtgärden returnerar det här värdet måste alltid använda it (i stället för plats) tootrack hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7b08b-118">If your operation returns this value, always use it (instead of Location) tootrack hello status of hello operation.</span></span>
* <span data-ttu-id="7b08b-119">`Location`-URL för att avgöra när en åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7b08b-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="7b08b-120">Använd det här värdet bara när Azure-asynkrona åtgärder inte returneras.</span><span class="sxs-lookup"><span data-stu-id="7b08b-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="7b08b-121">`Retry-After`-hello antal sekunder toowait innan hello status för hello asynkron åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7b08b-121">`Retry-After` - hello number of seconds toowait before checking hello status of hello asynchronous operation.</span></span>

<span data-ttu-id="7b08b-122">Men returnerar inte varje asynkron åtgärd dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7b08b-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="7b08b-123">Exempelvis kan du behöva tooevaluate hello Azure-asynkrona åtgärder huvudets värde för en åtgärd och hello plats huvudvärde för en annan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7b08b-123">For example, you may need tooevaluate hello Azure-AsyncOperation header value for one operation, and hello Location header value for another operation.</span></span> 

<span data-ttu-id="7b08b-124">Du kan hämta hello huvudvärden som du vill hämta alla huvudvärde för en begäran.</span><span class="sxs-lookup"><span data-stu-id="7b08b-124">You retrieve hello header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="7b08b-125">Till exempel i C# kan du hämta hello huvudvärde från en `HttpWebResponse` objekt med namnet `response` med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="7b08b-125">For example, in C#, you retrieve hello header value from an `HttpWebResponse` object named `response` with hello following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="7b08b-126">Azure-asynkrona åtgärder förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="7b08b-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="7b08b-127">tooget hello status för hello asynkron åtgärd, skickar en GET-begäran toohello URL i Azure-asynkrona åtgärder huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="7b08b-127">tooget hello status of hello asynchronous operation, send a GET request toohello URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="7b08b-128">hello innehåller hello svar från den här åtgärden information om hello igen.</span><span class="sxs-lookup"><span data-stu-id="7b08b-128">hello body of hello response from this operation contains information about hello operation.</span></span> <span data-ttu-id="7b08b-129">hello visar följande exempel hello möjliga värden som returneras från hello-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="7b08b-129">hello following example shows hello possible values returned from hello operation:</span></span>

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

<span data-ttu-id="7b08b-130">Endast `status` returneras för alla svar.</span><span class="sxs-lookup"><span data-stu-id="7b08b-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="7b08b-131">hello felobjekt returneras när hello status är misslyckad eller avbruten.</span><span class="sxs-lookup"><span data-stu-id="7b08b-131">hello error object is returned when hello status is Failed or Canceled.</span></span> <span data-ttu-id="7b08b-132">Alla andra värden är valfritt. därför se hello svar du får annorlunda ut än hello exempel.</span><span class="sxs-lookup"><span data-stu-id="7b08b-132">All other values are optional; therefore, hello response you receive may look different than hello example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="7b08b-133">provisioningState värden</span><span class="sxs-lookup"><span data-stu-id="7b08b-133">provisioningState values</span></span>

<span data-ttu-id="7b08b-134">Åtgärder som att skapa, uppdatera eller ta bort (PUT, korrigering, ta bort) en resurs vanligtvis returnera en `provisioningState` värde.</span><span class="sxs-lookup"><span data-stu-id="7b08b-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="7b08b-135">När en åtgärd har slutförts, returneras ett av följande tre värden:</span><span class="sxs-lookup"><span data-stu-id="7b08b-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="7b08b-136">Lyckades</span><span class="sxs-lookup"><span data-stu-id="7b08b-136">Succeeded</span></span>
* <span data-ttu-id="7b08b-137">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="7b08b-137">Failed</span></span>
* <span data-ttu-id="7b08b-138">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="7b08b-138">Canceled</span></span>

<span data-ttu-id="7b08b-139">Alla andra värden indikera hello-åtgärden körs fortfarande.</span><span class="sxs-lookup"><span data-stu-id="7b08b-139">All other values indicate hello operation is still running.</span></span> <span data-ttu-id="7b08b-140">Hej resursprovidern kan returnera ett anpassat värde som anger dess tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7b08b-140">hello resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="7b08b-141">Du kan till exempel få **godkända** när hello begäran har tagits emot och körs.</span><span class="sxs-lookup"><span data-stu-id="7b08b-141">For example, you may receive **Accepted** when hello request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="7b08b-142">Exempel och -svar</span><span class="sxs-lookup"><span data-stu-id="7b08b-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="7b08b-143">Starta den virtuella datorn (202 med Azure-asynkrona åtgärder)</span><span class="sxs-lookup"><span data-stu-id="7b08b-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="7b08b-144">Det här exemplet illustrerar hur toodetermine hello status för **starta** åtgärden för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7b08b-144">This example shows how toodetermine hello status of **start** operation for virtual machines.</span></span> <span data-ttu-id="7b08b-145">hello första begäran har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7b08b-145">hello initial request is in hello following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="7b08b-146">Den returnerar statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="7b08b-146">It returns status code 202.</span></span> <span data-ttu-id="7b08b-147">Bland hello huvudvärden visas:</span><span class="sxs-lookup"><span data-stu-id="7b08b-147">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="7b08b-148">toocheck hello status för hello asynkron åtgärd, skickar ett annat toothat URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="7b08b-148">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="7b08b-149">hello svarstexten innehåller hello status för hello åtgärd:</span><span class="sxs-lookup"><span data-stu-id="7b08b-149">hello response body contains hello status of hello operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="7b08b-150">Distribuera resurser (201 med Azure-asynkrona åtgärder)</span><span class="sxs-lookup"><span data-stu-id="7b08b-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="7b08b-151">Det här exemplet illustrerar hur toodetermine hello status för **distributioner** åtgärden för att distribuera resurser tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7b08b-151">This example shows how toodetermine hello status of **deployments** operation for deploying resources tooAzure.</span></span> <span data-ttu-id="7b08b-152">hello första begäran har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7b08b-152">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="7b08b-153">Den returnerar statuskod 201.</span><span class="sxs-lookup"><span data-stu-id="7b08b-153">It returns status code 201.</span></span> <span data-ttu-id="7b08b-154">hello brödtexten i svaret hello innehåller:</span><span class="sxs-lookup"><span data-stu-id="7b08b-154">hello body of hello response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="7b08b-155">Bland hello huvudvärden visas:</span><span class="sxs-lookup"><span data-stu-id="7b08b-155">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="7b08b-156">toocheck hello status för hello asynkron åtgärd, skickar ett annat toothat URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="7b08b-156">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="7b08b-157">hello svarstexten innehåller hello status för hello åtgärd:</span><span class="sxs-lookup"><span data-stu-id="7b08b-157">hello response body contains hello status of hello operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="7b08b-158">När hello distributionen är klar innehåller hello svar:</span><span class="sxs-lookup"><span data-stu-id="7b08b-158">When hello deployment is finished, hello response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="7b08b-159">Skapa lagringskonto (202 med plats och försök igen efter)</span><span class="sxs-lookup"><span data-stu-id="7b08b-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="7b08b-160">Det här exemplet illustrerar hur toodetermine hello status för hello **skapa** åtgärden för storage-konton.</span><span class="sxs-lookup"><span data-stu-id="7b08b-160">This example shows how toodetermine hello status of hello **create** operation for storage accounts.</span></span> <span data-ttu-id="7b08b-161">hello första begäran har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7b08b-161">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="7b08b-162">Och hello begärandetexten innehåller egenskaper för hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="7b08b-162">And hello request body contains properties for hello storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="7b08b-163">Den returnerar statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="7b08b-163">It returns status code 202.</span></span> <span data-ttu-id="7b08b-164">Bland hello huvudvärden finns hello följande två värden:</span><span class="sxs-lookup"><span data-stu-id="7b08b-164">Among hello header values, you see hello following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="7b08b-165">Efter antal sekunder som anges i försök igen efter markerar hello status för hello asynkron åtgärd genom att skicka en annan toothat URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="7b08b-165">After waiting for number of seconds specified in Retry-After, check hello status of hello asynchronous operation by sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="7b08b-166">Om hello begäran körs får du en statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="7b08b-166">If hello request is still running, you receive a status code 202.</span></span> <span data-ttu-id="7b08b-167">Om hello begäran har slutförts, din får en statuskod 200 och hello hello svaret innehåller hello egenskaper för hello storage-konto som har skapats.</span><span class="sxs-lookup"><span data-stu-id="7b08b-167">If hello request has completed, your receive a status code 200, and hello body of hello response contains hello properties of hello storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b08b-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b08b-168">Next steps</span></span>

* <span data-ttu-id="7b08b-169">Dokumentation om varje REST-åtgärd finns [REST API-dokumentation](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="7b08b-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="7b08b-170">Information om hur du hanterar resurser via hello Resource Manager REST-API finns [Using hello Resource Manager REST API](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="7b08b-170">For information about managing resources through hello Resource Manager REST API, see [Using hello Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="7b08b-171">information om hur du distribuerar mallar via hello Resource Manager REST-API finns [distribuera resurser med Resource Manager-mallar och Resource Manager REST API](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="7b08b-171">for information about deploying templates through hello Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>
