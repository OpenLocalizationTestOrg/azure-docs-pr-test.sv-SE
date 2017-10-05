---
title: "Asynkrona åtgärder i Azure | Microsoft Docs"
description: "Beskriver hur du spårar asynkrona åtgärder i Azure."
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
ms.openlocfilehash: 9fe3d98cd345aae45722295b6c1b7fc3e9036e95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="b657e-103">Spåra asynkrona åtgärder i Azure</span><span class="sxs-lookup"><span data-stu-id="b657e-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="b657e-104">Vissa Azure REST-åtgärder körs asynkront eftersom åtgärden inte går att slutföra snabbt.</span><span class="sxs-lookup"><span data-stu-id="b657e-104">Some Azure REST operations run asynchronously because the operation cannot be completed quickly.</span></span> <span data-ttu-id="b657e-105">Det här avsnittet beskriver hur du spåra statusen för asynkrona åtgärder via värden som returneras i svaret.</span><span class="sxs-lookup"><span data-stu-id="b657e-105">This topic describes how to track the status of asynchronous operations through values returned in the response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="b657e-106">Statuskoder för asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="b657e-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="b657e-107">En asynkron åtgärd returnerar först en HTTP-statuskod antingen:</span><span class="sxs-lookup"><span data-stu-id="b657e-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="b657e-108">201 (skapad)</span><span class="sxs-lookup"><span data-stu-id="b657e-108">201 (Created)</span></span>
* <span data-ttu-id="b657e-109">202 (accepterad)</span><span class="sxs-lookup"><span data-stu-id="b657e-109">202 (Accepted)</span></span> 

<span data-ttu-id="b657e-110">När åtgärden har slutförts, returnerar antingen:</span><span class="sxs-lookup"><span data-stu-id="b657e-110">When the operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="b657e-111">200 (OK)</span><span class="sxs-lookup"><span data-stu-id="b657e-111">200 (OK)</span></span>
* <span data-ttu-id="b657e-112">204 (inget innehåll)</span><span class="sxs-lookup"><span data-stu-id="b657e-112">204 (No Content)</span></span> 

<span data-ttu-id="b657e-113">Referera till den [REST API-dokumentation](/rest/api/) att se svar för åtgärden som du kör.</span><span class="sxs-lookup"><span data-stu-id="b657e-113">Refer to the [REST API documentation](/rest/api/) to see the responses for the operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="b657e-114">Övervaka status för åtgärden</span><span class="sxs-lookup"><span data-stu-id="b657e-114">Monitor status of operation</span></span>
<span data-ttu-id="b657e-115">De asynkrona REST-åtgärderna returvärden huvud som du använder för att avgöra status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b657e-115">The asynchronous REST operations return header values, which you use to determine the status of the operation.</span></span> <span data-ttu-id="b657e-116">Det finns potentiellt tre värden i huvudet för att granska:</span><span class="sxs-lookup"><span data-stu-id="b657e-116">There are potentially three header values to examine:</span></span>

* <span data-ttu-id="b657e-117">`Azure-AsyncOperation`-URL för att kontrollera pågående status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b657e-117">`Azure-AsyncOperation` - URL for checking the ongoing status of the operation.</span></span> <span data-ttu-id="b657e-118">Om åtgärden returnerar det här värdet måste du alltid använda den (i stället för platsen) att spåra status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b657e-118">If your operation returns this value, always use it (instead of Location) to track the status of the operation.</span></span>
* <span data-ttu-id="b657e-119">`Location`-URL för att avgöra när en åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b657e-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="b657e-120">Använd det här värdet bara när Azure-asynkrona åtgärder inte returneras.</span><span class="sxs-lookup"><span data-stu-id="b657e-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="b657e-121">`Retry-After`-Antalet sekunder som ska förflyta innan kontrollerar statusen för den asynkrona åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b657e-121">`Retry-After` - The number of seconds to wait before checking the status of the asynchronous operation.</span></span>

<span data-ttu-id="b657e-122">Men returnerar inte varje asynkron åtgärd dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b657e-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="b657e-123">Du kan behöva utvärdera huvudvärde Azure-asynkrona åtgärder för en åtgärd och huvudvärde platsen för en annan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b657e-123">For example, you may need to evaluate the Azure-AsyncOperation header value for one operation, and the Location header value for another operation.</span></span> 

<span data-ttu-id="b657e-124">Du kan hämta huvudvärden som du vill hämta alla huvudvärde för en begäran.</span><span class="sxs-lookup"><span data-stu-id="b657e-124">You retrieve the header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="b657e-125">Till exempel i C# kan du hämta huvudvärde från en `HttpWebResponse` objekt med namnet `response` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b657e-125">For example, in C#, you retrieve the header value from an `HttpWebResponse` object named `response` with the following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="b657e-126">Azure-asynkrona åtgärder förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="b657e-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="b657e-127">Om du vill hämta status för den asynkrona åtgärden att skicka en GET-begäran till URL: en i Azure-asynkrona åtgärder huvudets värde.</span><span class="sxs-lookup"><span data-stu-id="b657e-127">To get the status of the asynchronous operation, send a GET request to the URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="b657e-128">Brödtexten i svaret från den här åtgärden innehåller information om åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b657e-128">The body of the response from this operation contains information about the operation.</span></span> <span data-ttu-id="b657e-129">I följande exempel visas de möjliga värdena som returnerades från igen:</span><span class="sxs-lookup"><span data-stu-id="b657e-129">The following example shows the possible values returned from the operation:</span></span>

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

<span data-ttu-id="b657e-130">Endast `status` returneras för alla svar.</span><span class="sxs-lookup"><span data-stu-id="b657e-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="b657e-131">Felobjekt returneras när det har status misslyckad eller annullerad.</span><span class="sxs-lookup"><span data-stu-id="b657e-131">The error object is returned when the status is Failed or Canceled.</span></span> <span data-ttu-id="b657e-132">Alla andra värden är valfritt. därför se det svar du får annorlunda ut än exemplet.</span><span class="sxs-lookup"><span data-stu-id="b657e-132">All other values are optional; therefore, the response you receive may look different than the example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="b657e-133">provisioningState värden</span><span class="sxs-lookup"><span data-stu-id="b657e-133">provisioningState values</span></span>

<span data-ttu-id="b657e-134">Åtgärder som att skapa, uppdatera eller ta bort (PUT, korrigering, ta bort) en resurs vanligtvis returnera en `provisioningState` värde.</span><span class="sxs-lookup"><span data-stu-id="b657e-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="b657e-135">När en åtgärd har slutförts, returneras ett av följande tre värden:</span><span class="sxs-lookup"><span data-stu-id="b657e-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="b657e-136">Lyckades</span><span class="sxs-lookup"><span data-stu-id="b657e-136">Succeeded</span></span>
* <span data-ttu-id="b657e-137">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="b657e-137">Failed</span></span>
* <span data-ttu-id="b657e-138">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="b657e-138">Canceled</span></span>

<span data-ttu-id="b657e-139">Alla andra värden anger åtgärden körs fortfarande.</span><span class="sxs-lookup"><span data-stu-id="b657e-139">All other values indicate the operation is still running.</span></span> <span data-ttu-id="b657e-140">Resursprovidern kan returnera ett anpassat värde som anger dess tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b657e-140">The resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="b657e-141">Du kan till exempel få **godkända** när begäran har tagits emot och körs.</span><span class="sxs-lookup"><span data-stu-id="b657e-141">For example, you may receive **Accepted** when the request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="b657e-142">Exempel och -svar</span><span class="sxs-lookup"><span data-stu-id="b657e-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="b657e-143">Starta den virtuella datorn (202 med Azure-asynkrona åtgärder)</span><span class="sxs-lookup"><span data-stu-id="b657e-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="b657e-144">Det här exemplet visar hur du bestämma tillståndet hos **starta** åtgärden för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b657e-144">This example shows how to determine the status of **start** operation for virtual machines.</span></span> <span data-ttu-id="b657e-145">Den första begäranden är i följande format:</span><span class="sxs-lookup"><span data-stu-id="b657e-145">The initial request is in the following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="b657e-146">Den returnerar statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="b657e-146">It returns status code 202.</span></span> <span data-ttu-id="b657e-147">Mellan huvudvärden visas:</span><span class="sxs-lookup"><span data-stu-id="b657e-147">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="b657e-148">För att kontrollera status för den asynkrona åtgärden som en annan begäran skickades till URL: en.</span><span class="sxs-lookup"><span data-stu-id="b657e-148">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="b657e-149">Svarstexten innehåller status för åtgärden:</span><span class="sxs-lookup"><span data-stu-id="b657e-149">The response body contains the status of the operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="b657e-150">Distribuera resurser (201 med Azure-asynkrona åtgärder)</span><span class="sxs-lookup"><span data-stu-id="b657e-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="b657e-151">Det här exemplet visar hur du bestämma tillståndet hos **distributioner** åtgärden för att distribuera resurser till Azure.</span><span class="sxs-lookup"><span data-stu-id="b657e-151">This example shows how to determine the status of **deployments** operation for deploying resources to Azure.</span></span> <span data-ttu-id="b657e-152">Den första begäranden är i följande format:</span><span class="sxs-lookup"><span data-stu-id="b657e-152">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="b657e-153">Den returnerar statuskod 201.</span><span class="sxs-lookup"><span data-stu-id="b657e-153">It returns status code 201.</span></span> <span data-ttu-id="b657e-154">Brödtexten i svaret innehåller:</span><span class="sxs-lookup"><span data-stu-id="b657e-154">The body of the response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="b657e-155">Mellan huvudvärden visas:</span><span class="sxs-lookup"><span data-stu-id="b657e-155">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="b657e-156">För att kontrollera status för den asynkrona åtgärden som en annan begäran skickades till URL: en.</span><span class="sxs-lookup"><span data-stu-id="b657e-156">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="b657e-157">Svarstexten innehåller status för åtgärden:</span><span class="sxs-lookup"><span data-stu-id="b657e-157">The response body contains the status of the operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="b657e-158">När distributionen är klar visas innehåller svaret:</span><span class="sxs-lookup"><span data-stu-id="b657e-158">When the deployment is finished, the response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="b657e-159">Skapa lagringskonto (202 med plats och försök igen efter)</span><span class="sxs-lookup"><span data-stu-id="b657e-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="b657e-160">Det här exemplet visar hur du bestämma tillståndet hos den **skapa** åtgärden för storage-konton.</span><span class="sxs-lookup"><span data-stu-id="b657e-160">This example shows how to determine the status of the **create** operation for storage accounts.</span></span> <span data-ttu-id="b657e-161">Den första begäranden är i följande format:</span><span class="sxs-lookup"><span data-stu-id="b657e-161">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="b657e-162">Och begärandetexten innehåller egenskaper för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="b657e-162">And the request body contains properties for the storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="b657e-163">Den returnerar statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="b657e-163">It returns status code 202.</span></span> <span data-ttu-id="b657e-164">Bland huvudvärden Se följande två värden:</span><span class="sxs-lookup"><span data-stu-id="b657e-164">Among the header values, you see the following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="b657e-165">När väntar antalet sekunder anges i försök igen efter, kontrollera status för den asynkrona åtgärden genom att skicka en annan begäran till URL: en.</span><span class="sxs-lookup"><span data-stu-id="b657e-165">After waiting for number of seconds specified in Retry-After, check the status of the asynchronous operation by sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="b657e-166">Om begäran körs får du en statuskod 202.</span><span class="sxs-lookup"><span data-stu-id="b657e-166">If the request is still running, you receive a status code 202.</span></span> <span data-ttu-id="b657e-167">Om begäran har slutförts, din får en statuskod 200 och brödtexten i svaret innehåller egenskaper för det lagringskonto som har skapats.</span><span class="sxs-lookup"><span data-stu-id="b657e-167">If the request has completed, your receive a status code 200, and the body of the response contains the properties of the storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b657e-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b657e-168">Next steps</span></span>

* <span data-ttu-id="b657e-169">Dokumentation om varje REST-åtgärd finns [REST API-dokumentation](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="b657e-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="b657e-170">Information om hur du hanterar resurser via Hanteraren för filserverresurser REST-API finns [med hjälp av hanteraren för filserverresurser REST API](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="b657e-170">For information about managing resources through the Resource Manager REST API, see [Using the Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="b657e-171">information om hur du distribuerar mallar via Hanteraren för filserverresurser REST-API finns [distribuera resurser med Resource Manager-mallar och Resource Manager REST API](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="b657e-171">for information about deploying templates through the Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>