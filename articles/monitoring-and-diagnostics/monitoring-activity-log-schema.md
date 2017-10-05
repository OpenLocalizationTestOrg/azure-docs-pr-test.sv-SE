---
title: "Azure händelse för aktivitetslogg | Microsoft Docs"
description: "Förstå Händelseschema för data som sänds till aktivitetsloggen"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: a4ceb822e0ec3e1c1dc31ece1db761834e795f6c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-activity-log-event-schema"></a><span data-ttu-id="078c6-103">Azure aktivitetsloggen Händelseschema</span><span class="sxs-lookup"><span data-stu-id="078c6-103">Azure Activity Log event schema</span></span>
<span data-ttu-id="078c6-104">Den **Azure-aktivitetsloggen** är en logg som ger inblick i prenumerationsnivån händelser som har inträffat i Azure.</span><span class="sxs-lookup"><span data-stu-id="078c6-104">The **Azure Activity Log** is a log that provides insight into any subscription-level events that have occurred in Azure.</span></span> <span data-ttu-id="078c6-105">Den här artikeln beskriver Händelseschema per kategori av data.</span><span class="sxs-lookup"><span data-stu-id="078c6-105">This article describes the event schema per category of data.</span></span>

## <a name="administrative"></a><span data-ttu-id="078c6-106">Administrativa</span><span class="sxs-lookup"><span data-stu-id="078c6-106">Administrative</span></span>
<span data-ttu-id="078c6-107">Den här kategorin innehåller posten för alla skapa, uppdatera, ta bort och åtgärden åtgärder utföras via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="078c6-107">This category contains the record of all create, update, delete, and action operations performed through Resource Manager.</span></span> <span data-ttu-id="078c6-108">Exempel på vilka typer av händelser som visas i den här kategorin är ”Skapa virtuell dator” och ”ta bort nätverkssäkerhetsgrupp” varje åtgärd som en användare eller program med hjälp av hanteraren för filserverresurser modelleras som en åtgärd på en viss resurstyp.</span><span class="sxs-lookup"><span data-stu-id="078c6-108">Examples of the types of events you would see in this category include "create virtual machine" and "delete network security group" Every action taken by a user or application using Resource Manager is modeled as an operation on a particular resource type.</span></span> <span data-ttu-id="078c6-109">Om åtgärdstypen är Write-, Delete- eller åtgärd, är start-och lyckas eller misslyckas för denna åtgärd registrerade i den administrativa kategorin.</span><span class="sxs-lookup"><span data-stu-id="078c6-109">If the operation type is Write, Delete, or Action, the records of both the start and success or fail of that operation are recorded in the Administrative category.</span></span> <span data-ttu-id="078c6-110">Den administrativa kategorin omfattar även ändringar rollbaserad åtkomstkontroll i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="078c6-110">The Administrative category also includes any changes to role-based access control in a subscription.</span></span>

### <a name="sample-event"></a><span data-ttu-id="078c6-111">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="078c6-111">Sample event</span></span>
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="078c6-112">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="078c6-112">Property descriptions</span></span>
| <span data-ttu-id="078c6-113">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-113">Element Name</span></span> | <span data-ttu-id="078c6-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="078c6-115">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="078c6-115">authorization</span></span> |<span data-ttu-id="078c6-116">BLOB RBAC egenskaper för händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-116">Blob of RBAC properties of the event.</span></span> <span data-ttu-id="078c6-117">Inkluderar vanligtvis egenskaperna ””, ”roll” och ”omfattning”.</span><span class="sxs-lookup"><span data-stu-id="078c6-117">Usually includes the “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="078c6-118">Anroparen</span><span class="sxs-lookup"><span data-stu-id="078c6-118">caller</span></span> |<span data-ttu-id="078c6-119">E-postadressen för användaren som utförde åtgärden, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="078c6-119">Email address of the user who has performed the operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="078c6-120">kanaler</span><span class="sxs-lookup"><span data-stu-id="078c6-120">channels</span></span> |<span data-ttu-id="078c6-121">Ett av följande värden: ”Admin”, ”åtgärden”</span><span class="sxs-lookup"><span data-stu-id="078c6-121">One of the following values: “Admin”, “Operation”</span></span> |
| <span data-ttu-id="078c6-122">Anspråk</span><span class="sxs-lookup"><span data-stu-id="078c6-122">claims</span></span> |<span data-ttu-id="078c6-123">Detta JWT-token som används av Active Directory för att autentisera användaren eller programmet att utföra åtgärden i hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="078c6-123">The JWT token used by Active Directory to authenticate the user or application to perform this operation in resource manager.</span></span> |
| <span data-ttu-id="078c6-124">correlationId</span><span class="sxs-lookup"><span data-stu-id="078c6-124">correlationId</span></span> |<span data-ttu-id="078c6-125">Vanligtvis ett GUID i strängformatet.</span><span class="sxs-lookup"><span data-stu-id="078c6-125">Usually a GUID in the string format.</span></span> <span data-ttu-id="078c6-126">Händelser som delar en correlationId tillhöra samma uber åtgärd.</span><span class="sxs-lookup"><span data-stu-id="078c6-126">Events that share a correlationId belong to the same uber action.</span></span> |
| <span data-ttu-id="078c6-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-127">description</span></span> |<span data-ttu-id="078c6-128">Statisk textbeskrivning av en händelse.</span><span class="sxs-lookup"><span data-stu-id="078c6-128">Static text description of an event.</span></span> |
| <span data-ttu-id="078c6-129">eventDataId</span><span class="sxs-lookup"><span data-stu-id="078c6-129">eventDataId</span></span> |<span data-ttu-id="078c6-130">Unik identifierare för en händelse.</span><span class="sxs-lookup"><span data-stu-id="078c6-130">Unique identifier of an event.</span></span> |
| <span data-ttu-id="078c6-131">httpRequest</span><span class="sxs-lookup"><span data-stu-id="078c6-131">httpRequest</span></span> |<span data-ttu-id="078c6-132">BLOB som beskriver Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="078c6-132">Blob describing the Http Request.</span></span> <span data-ttu-id="078c6-133">Inkluderar vanligtvis ”clientRequestId”, ”clientIpAddress” och ”metod” (HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="078c6-133">Usually includes the “clientRequestId”, “clientIpAddress” and “method” (HTTP method.</span></span> <span data-ttu-id="078c6-134">Till exempel PLACERA).</span><span class="sxs-lookup"><span data-stu-id="078c6-134">For example, PUT).</span></span> |
| <span data-ttu-id="078c6-135">nivå</span><span class="sxs-lookup"><span data-stu-id="078c6-135">level</span></span> |<span data-ttu-id="078c6-136">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-136">Level of the event.</span></span> <span data-ttu-id="078c6-137">Ett av följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="078c6-137">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="078c6-138">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="078c6-138">resourceGroupName</span></span> |<span data-ttu-id="078c6-139">Namnet på resursgruppen för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="078c6-139">Name of the resource group for the impacted resource.</span></span> |
| <span data-ttu-id="078c6-140">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="078c6-140">resourceProviderName</span></span> |<span data-ttu-id="078c6-141">Namnet på resursprovidern för resursen påverkas</span><span class="sxs-lookup"><span data-stu-id="078c6-141">Name of the resource provider for the impacted resource</span></span> |
| <span data-ttu-id="078c6-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="078c6-142">resourceId</span></span> |<span data-ttu-id="078c6-143">Resurs-id för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="078c6-143">Resource id of the impacted resource.</span></span> |
| <span data-ttu-id="078c6-144">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="078c6-144">operationId</span></span> |<span data-ttu-id="078c6-145">Ett GUID som delas mellan de händelser som motsvarar en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="078c6-145">A GUID shared among the events that correspond to a single operation.</span></span> |
| <span data-ttu-id="078c6-146">operationName</span><span class="sxs-lookup"><span data-stu-id="078c6-146">operationName</span></span> |<span data-ttu-id="078c6-147">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-147">Name of the operation.</span></span> |
| <span data-ttu-id="078c6-148">properties</span><span class="sxs-lookup"><span data-stu-id="078c6-148">properties</span></span> |<span data-ttu-id="078c6-149">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-149">Set of `<Key, Value>` pairs (that is, a Dictionary) describing the details of the event.</span></span> |
| <span data-ttu-id="078c6-150">status</span><span class="sxs-lookup"><span data-stu-id="078c6-150">status</span></span> |<span data-ttu-id="078c6-151">Sträng som beskriver status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-151">String describing the status of the operation.</span></span> <span data-ttu-id="078c6-152">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="078c6-152">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="078c6-153">subStatus</span><span class="sxs-lookup"><span data-stu-id="078c6-153">subStatus</span></span> |<span data-ttu-id="078c6-154">Vanligtvis HTTP-statuskoden motsvarande rest anropa, men kan även innehålla andra strängar som beskriver en sådan, till exempel värdena vanliga: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), inte innehåll (HTTP-statuskod: 204), felaktig begäran (HTTP-statuskod: 400), det gick inte att hitta (HTTP-statuskod: 404), konflikt (HTTP-statuskod : 409), internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504).</span><span class="sxs-lookup"><span data-stu-id="078c6-154">Usually the HTTP status code of the corresponding REST call, but can also include other strings describing a substatus, such as these common values: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504).</span></span> |
| <span data-ttu-id="078c6-155">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-155">eventTimestamp</span></span> |<span data-ttu-id="078c6-156">Tidsstämpel när händelsen skapades av tjänsten Azure motsvarande händelsen begäran bearbetades.</span><span class="sxs-lookup"><span data-stu-id="078c6-156">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="078c6-157">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-157">submissionTimestamp</span></span> |<span data-ttu-id="078c6-158">Tidsstämpel när händelsen blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="078c6-158">Timestamp when the event became available for querying.</span></span> |
| <span data-ttu-id="078c6-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="078c6-159">subscriptionId</span></span> |<span data-ttu-id="078c6-160">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="078c6-160">Azure Subscription Id.</span></span> |

## <a name="service-health"></a><span data-ttu-id="078c6-161">Tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="078c6-161">Service health</span></span>
<span data-ttu-id="078c6-162">Den här kategorin innehåller post för varje service hälsa incidenter som har inträffat i Azure.</span><span class="sxs-lookup"><span data-stu-id="078c6-162">This category contains the record of any service health incidents that have occurred in Azure.</span></span> <span data-ttu-id="078c6-163">Ett exempel på typen av händelse visas i den här kategorin är ”SQL Azure i östra USA upplever driftstopp”.</span><span class="sxs-lookup"><span data-stu-id="078c6-163">An example of the type of event you would see in this category is "SQL Azure in East US is experiencing downtime."</span></span> <span data-ttu-id="078c6-164">Hälsa av tjänsten-händelser finns i fem sorter: åtgärd krävs stödd återställning, Incident, underhåll, Information eller säkerhet, och visas endast om du har en resurs i den prenumeration som skulle påverkas av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-164">Service health events come in five varieties: Action Required, Assisted Recovery, Incident, Maintenance, Information, or Security, and only appear if you have a resource in the subscription that would be impacted by the event.</span></span>

### <a name="sample-event"></a><span data-ttu-id="078c6-165">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="078c6-165">Sample event</span></span>
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a><span data-ttu-id="078c6-166">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="078c6-166">Property descriptions</span></span>
<span data-ttu-id="078c6-167">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-167">Element Name</span></span> | <span data-ttu-id="078c6-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-168">Description</span></span>
-------- | -----------
<span data-ttu-id="078c6-169">kanaler</span><span class="sxs-lookup"><span data-stu-id="078c6-169">channels</span></span> | <span data-ttu-id="078c6-170">Är ett av följande värden: ”Admin”, ”åtgärden”</span><span class="sxs-lookup"><span data-stu-id="078c6-170">Is one of the following values: “Admin”, “Operation”</span></span>
<span data-ttu-id="078c6-171">correlationId</span><span class="sxs-lookup"><span data-stu-id="078c6-171">correlationId</span></span> | <span data-ttu-id="078c6-172">Är vanligtvis ett GUID i strängformatet.</span><span class="sxs-lookup"><span data-stu-id="078c6-172">Is usually a GUID in the string format.</span></span> <span data-ttu-id="078c6-173">Händelser med som tillhör samma uber åtgärd vanligtvis delar samma correlationId.</span><span class="sxs-lookup"><span data-stu-id="078c6-173">Events with that belong to the same uber action usually share the same correlationId.</span></span>
<span data-ttu-id="078c6-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-174">description</span></span> | <span data-ttu-id="078c6-175">Beskrivning av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-175">Description of the event.</span></span>
<span data-ttu-id="078c6-176">eventDataId</span><span class="sxs-lookup"><span data-stu-id="078c6-176">eventDataId</span></span> | <span data-ttu-id="078c6-177">Den unika identifieraren för en händelse.</span><span class="sxs-lookup"><span data-stu-id="078c6-177">The unique identifier of an event.</span></span>
<span data-ttu-id="078c6-178">EventName</span><span class="sxs-lookup"><span data-stu-id="078c6-178">eventName</span></span> | <span data-ttu-id="078c6-179">Rubrik på händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-179">The title of the event.</span></span>
<span data-ttu-id="078c6-180">nivå</span><span class="sxs-lookup"><span data-stu-id="078c6-180">level</span></span> | <span data-ttu-id="078c6-181">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-181">Level of the event.</span></span> <span data-ttu-id="078c6-182">Ett av följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="078c6-182">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span>
<span data-ttu-id="078c6-183">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="078c6-183">resourceProviderName</span></span> | <span data-ttu-id="078c6-184">Namnet på resursprovidern för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="078c6-184">Name of the resource provider for the impacted resource.</span></span> <span data-ttu-id="078c6-185">Detta kan vara null om det är inte känt.</span><span class="sxs-lookup"><span data-stu-id="078c6-185">If not known, this will be null.</span></span>
<span data-ttu-id="078c6-186">resourceType</span><span class="sxs-lookup"><span data-stu-id="078c6-186">resourceType</span></span>| <span data-ttu-id="078c6-187">Typ av resurs för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="078c6-187">The type of resource of the impacted resource.</span></span> <span data-ttu-id="078c6-188">Detta kan vara null om det är inte känt.</span><span class="sxs-lookup"><span data-stu-id="078c6-188">If not known, this will be null.</span></span>
<span data-ttu-id="078c6-189">subStatus</span><span class="sxs-lookup"><span data-stu-id="078c6-189">subStatus</span></span> | <span data-ttu-id="078c6-190">Vanligtvis är null för händelser för Hälsotjänst.</span><span class="sxs-lookup"><span data-stu-id="078c6-190">Usually null for Service Health events.</span></span>
<span data-ttu-id="078c6-191">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-191">eventTimestamp</span></span> | <span data-ttu-id="078c6-192">Tidsstämpel när händelselogg har genererats och skickats till aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="078c6-192">Timestamp when the log event was generated and submitted to the Activity Log.</span></span>
<span data-ttu-id="078c6-193">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-193">submissionTimestamp</span></span> |   <span data-ttu-id="078c6-194">Tidsstämpel när händelsen blev tillgängliga i aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="078c6-194">Timestamp when the event became available in the Activity Log.</span></span>
<span data-ttu-id="078c6-195">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="078c6-195">subscriptionId</span></span> | <span data-ttu-id="078c6-196">Azure-prenumerationen som den här händelsen loggades.</span><span class="sxs-lookup"><span data-stu-id="078c6-196">The Azure subscription in which this event was logged.</span></span>
<span data-ttu-id="078c6-197">status</span><span class="sxs-lookup"><span data-stu-id="078c6-197">status</span></span> | <span data-ttu-id="078c6-198">Sträng som beskriver status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-198">String describing the status of the operation.</span></span> <span data-ttu-id="078c6-199">Vissa vanliga värden: aktiva, löst.</span><span class="sxs-lookup"><span data-stu-id="078c6-199">Some common values are: Active, Resolved.</span></span>
<span data-ttu-id="078c6-200">operationName</span><span class="sxs-lookup"><span data-stu-id="078c6-200">operationName</span></span> | <span data-ttu-id="078c6-201">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-201">Name of the operation.</span></span> <span data-ttu-id="078c6-202">Vanligtvis Microsoft.ServiceHealth/incident/action.</span><span class="sxs-lookup"><span data-stu-id="078c6-202">Usually Microsoft.ServiceHealth/incident/action.</span></span>
<span data-ttu-id="078c6-203">category</span><span class="sxs-lookup"><span data-stu-id="078c6-203">category</span></span> | <span data-ttu-id="078c6-204">”ServiceHealth”</span><span class="sxs-lookup"><span data-stu-id="078c6-204">"ServiceHealth"</span></span>
<span data-ttu-id="078c6-205">resourceId</span><span class="sxs-lookup"><span data-stu-id="078c6-205">resourceId</span></span> | <span data-ttu-id="078c6-206">Resurs-id för påverkas resursen, om den är känd.</span><span class="sxs-lookup"><span data-stu-id="078c6-206">Resource id of the impacted resource, if known.</span></span> <span data-ttu-id="078c6-207">Prenumerations-ID har angetts på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="078c6-207">Subscription ID is provided otherwise.</span></span>
<span data-ttu-id="078c6-208">Properties.title</span><span class="sxs-lookup"><span data-stu-id="078c6-208">Properties.title</span></span> | <span data-ttu-id="078c6-209">Lokaliserade rubriken för den här kommunikationen.</span><span class="sxs-lookup"><span data-stu-id="078c6-209">The localized title for this communication.</span></span> <span data-ttu-id="078c6-210">Engelska är standardspråk.</span><span class="sxs-lookup"><span data-stu-id="078c6-210">English is the default language.</span></span>
<span data-ttu-id="078c6-211">Properties.Communication</span><span class="sxs-lookup"><span data-stu-id="078c6-211">Properties.communication</span></span> | <span data-ttu-id="078c6-212">Lokaliserad information om kommunikationen med HTML-kod.</span><span class="sxs-lookup"><span data-stu-id="078c6-212">The localized details of the communication with HTML markup.</span></span> <span data-ttu-id="078c6-213">Engelska är standard.</span><span class="sxs-lookup"><span data-stu-id="078c6-213">English is the default.</span></span>
<span data-ttu-id="078c6-214">Properties.incidentType</span><span class="sxs-lookup"><span data-stu-id="078c6-214">Properties.incidentType</span></span> | <span data-ttu-id="078c6-215">Möjliga värden: AssistedRecovery, ActionRequired, Information, incidenter, underhåll, säkerhet</span><span class="sxs-lookup"><span data-stu-id="078c6-215">Possible values: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security</span></span>
<span data-ttu-id="078c6-216">Properties.trackingId</span><span class="sxs-lookup"><span data-stu-id="078c6-216">Properties.trackingId</span></span> | <span data-ttu-id="078c6-217">Identifierar den här händelsen är associerad med incidenten.</span><span class="sxs-lookup"><span data-stu-id="078c6-217">Identifies the incident this event is associated with.</span></span> <span data-ttu-id="078c6-218">Används för att korrelera händelser relaterade till en incident.</span><span class="sxs-lookup"><span data-stu-id="078c6-218">Use this to correlate the events related to an incident.</span></span>
<span data-ttu-id="078c6-219">Properties.impactedServices</span><span class="sxs-lookup"><span data-stu-id="078c6-219">Properties.impactedServices</span></span> | <span data-ttu-id="078c6-220">En ESC JSON-blob som beskriver de tjänster och regioner som påverkas av incidenten.</span><span class="sxs-lookup"><span data-stu-id="078c6-220">An escaped JSON blob which describes the services and regions that are impacted by the incident.</span></span> <span data-ttu-id="078c6-221">En lista över tjänster, som har en ServiceName och en lista över ImpactedRegions, som har en RegionName.</span><span class="sxs-lookup"><span data-stu-id="078c6-221">A list of Services, each of which has a ServiceName and a list of ImpactedRegions, each of which has a RegionName.</span></span>
<span data-ttu-id="078c6-222">Properties.defaultLanguageTitle</span><span class="sxs-lookup"><span data-stu-id="078c6-222">Properties.defaultLanguageTitle</span></span> | <span data-ttu-id="078c6-223">Kommunikation på engelska</span><span class="sxs-lookup"><span data-stu-id="078c6-223">The communication in English</span></span>
<span data-ttu-id="078c6-224">Properties.defaultLanguageContent</span><span class="sxs-lookup"><span data-stu-id="078c6-224">Properties.defaultLanguageContent</span></span> | <span data-ttu-id="078c6-225">Kommunikation på engelska som HTML-kod eller oformaterad text</span><span class="sxs-lookup"><span data-stu-id="078c6-225">The communication in English as either html markup or plain text</span></span>
<span data-ttu-id="078c6-226">Properties.Stage</span><span class="sxs-lookup"><span data-stu-id="078c6-226">Properties.stage</span></span> | <span data-ttu-id="078c6-227">Möjliga värden för AssistedRecovery, ActionRequired, Information, incidenter, säkerhet: är aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="078c6-227">Possible values for AssistedRecovery, ActionRequired, Information, Incident, Security: are Active, Resolved.</span></span> <span data-ttu-id="078c6-228">De är för underhåll: aktiv, planerad, InProgress, Avbruten, Rescheduled, löst, Slutför</span><span class="sxs-lookup"><span data-stu-id="078c6-228">For Maintenance they are: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete</span></span>
<span data-ttu-id="078c6-229">Properties.communicationId</span><span class="sxs-lookup"><span data-stu-id="078c6-229">Properties.communicationId</span></span> | <span data-ttu-id="078c6-230">Kommunikationen den här händelsen är kopplat.</span><span class="sxs-lookup"><span data-stu-id="078c6-230">The communication this event is associated.</span></span>

## <a name="alert"></a><span data-ttu-id="078c6-231">Varning</span><span class="sxs-lookup"><span data-stu-id="078c6-231">Alert</span></span>
<span data-ttu-id="078c6-232">Den här kategorin innehåller posten för alla aktiveringar av Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="078c6-232">This category contains the record of all activations of Azure alerts.</span></span> <span data-ttu-id="078c6-233">Ett exempel på typen av händelse visas i den här kategorin är ”processor på myVM har varit över 80 under de senaste fem minuterna”.</span><span class="sxs-lookup"><span data-stu-id="078c6-233">An example of the type of event you would see in this category is "CPU % on myVM has been over 80 for the past 5 minutes."</span></span> <span data-ttu-id="078c6-234">En mängd Azure system har en aviseringar begrepp – du kan definiera en regel av något slag och ett meddelande när villkor matchar regeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-234">A variety of Azure systems have an alerting concept -- you can define a rule of some sort and receive a notification when conditions match that rule.</span></span> <span data-ttu-id="078c6-235">Varje gång en stöds Azure aviseringstyp 'aktiveras,' eller villkoren är uppfyllda för att generera ett meddelande, en post på aktiveringen skickas även till den här kategorin för aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="078c6-235">Each time a supported Azure alert type 'activates,' or the conditions are met to generate a notification, a record of the activation is also pushed to this category of the Activity Log.</span></span>

### <a name="sample-event"></a><span data-ttu-id="078c6-236">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="078c6-236">Sample event</span></span>

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="078c6-237">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="078c6-237">Property descriptions</span></span>
| <span data-ttu-id="078c6-238">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-238">Element Name</span></span> | <span data-ttu-id="078c6-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-239">Description</span></span> |
| --- | --- |
| <span data-ttu-id="078c6-240">Anroparen</span><span class="sxs-lookup"><span data-stu-id="078c6-240">caller</span></span> | <span data-ttu-id="078c6-241">Alltid Microsoft.Insights/alertRules</span><span class="sxs-lookup"><span data-stu-id="078c6-241">Always Microsoft.Insights/alertRules</span></span> |
| <span data-ttu-id="078c6-242">kanaler</span><span class="sxs-lookup"><span data-stu-id="078c6-242">channels</span></span> | <span data-ttu-id="078c6-243">Alltid ”Admin, åtgärden”</span><span class="sxs-lookup"><span data-stu-id="078c6-243">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="078c6-244">Anspråk</span><span class="sxs-lookup"><span data-stu-id="078c6-244">claims</span></span> | <span data-ttu-id="078c6-245">JSON-blob med typen SPN (service principal name) eller resurs varning-motorn.</span><span class="sxs-lookup"><span data-stu-id="078c6-245">JSON blob with the SPN (service principal name), or resource type, of the alert engine.</span></span> |
| <span data-ttu-id="078c6-246">correlationId</span><span class="sxs-lookup"><span data-stu-id="078c6-246">correlationId</span></span> | <span data-ttu-id="078c6-247">Ett GUID i strängformatet.</span><span class="sxs-lookup"><span data-stu-id="078c6-247">A GUID in the string format.</span></span> |
| <span data-ttu-id="078c6-248">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-248">description</span></span> |<span data-ttu-id="078c6-249">Statisk textbeskrivning av händelsen avisering.</span><span class="sxs-lookup"><span data-stu-id="078c6-249">Static text description of the alert event.</span></span> |
| <span data-ttu-id="078c6-250">eventDataId</span><span class="sxs-lookup"><span data-stu-id="078c6-250">eventDataId</span></span> |<span data-ttu-id="078c6-251">Unik identifierare för varning avisering.</span><span class="sxs-lookup"><span data-stu-id="078c6-251">Unique identifier of the alert event.</span></span> |
| <span data-ttu-id="078c6-252">nivå</span><span class="sxs-lookup"><span data-stu-id="078c6-252">level</span></span> |<span data-ttu-id="078c6-253">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-253">Level of the event.</span></span> <span data-ttu-id="078c6-254">Ett av följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="078c6-254">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="078c6-255">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="078c6-255">resourceGroupName</span></span> |<span data-ttu-id="078c6-256">Namnet på resursgruppen för resursen påverkas om det är en avisering om mått.</span><span class="sxs-lookup"><span data-stu-id="078c6-256">Name of the resource group for the impacted resource if it is a metric alert.</span></span> <span data-ttu-id="078c6-257">För andra aviseringstyper är namnet på resursgruppen som innehåller aviseringen sig själv.</span><span class="sxs-lookup"><span data-stu-id="078c6-257">For other alert types, this is the name of the resource group that contains the alert itself.</span></span> |
| <span data-ttu-id="078c6-258">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="078c6-258">resourceProviderName</span></span> |<span data-ttu-id="078c6-259">Namnet på resursprovidern för resursen påverkas om det är en avisering om mått.</span><span class="sxs-lookup"><span data-stu-id="078c6-259">Name of the resource provider for the impacted resource if it is a metric alert.</span></span> <span data-ttu-id="078c6-260">För andra aviseringstyper kan är det här namnet på resursprovidern för aviseringen sig själv.</span><span class="sxs-lookup"><span data-stu-id="078c6-260">For other alert types, this is the name of the resource provider for the alert itself.</span></span> |
| <span data-ttu-id="078c6-261">resourceId</span><span class="sxs-lookup"><span data-stu-id="078c6-261">resourceId</span></span> | <span data-ttu-id="078c6-262">Namnet på resurs-ID för resursen påverkas om det är en avisering om mått.</span><span class="sxs-lookup"><span data-stu-id="078c6-262">Name of the resource ID for the impacted resource if it is a metric alert.</span></span> <span data-ttu-id="078c6-263">För andra aviseringstyper är resurs-ID för avisering resursen sig själv.</span><span class="sxs-lookup"><span data-stu-id="078c6-263">For other alert types, this is the resource ID of the alert resource itself.</span></span> |
| <span data-ttu-id="078c6-264">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="078c6-264">operationId</span></span> |<span data-ttu-id="078c6-265">Ett GUID som delas mellan de händelser som motsvarar en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="078c6-265">A GUID shared among the events that correspond to a single operation.</span></span> |
| <span data-ttu-id="078c6-266">operationName</span><span class="sxs-lookup"><span data-stu-id="078c6-266">operationName</span></span> |<span data-ttu-id="078c6-267">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-267">Name of the operation.</span></span> |
| <span data-ttu-id="078c6-268">properties</span><span class="sxs-lookup"><span data-stu-id="078c6-268">properties</span></span> |<span data-ttu-id="078c6-269">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-269">Set of `<Key, Value>` pairs (that is, a Dictionary) describing the details of the event.</span></span> |
| <span data-ttu-id="078c6-270">status</span><span class="sxs-lookup"><span data-stu-id="078c6-270">status</span></span> |<span data-ttu-id="078c6-271">Sträng som beskriver status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-271">String describing the status of the operation.</span></span> <span data-ttu-id="078c6-272">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="078c6-272">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="078c6-273">subStatus</span><span class="sxs-lookup"><span data-stu-id="078c6-273">subStatus</span></span> | <span data-ttu-id="078c6-274">Vanligtvis är null för aviseringar.</span><span class="sxs-lookup"><span data-stu-id="078c6-274">Usually null for alerts.</span></span> |
| <span data-ttu-id="078c6-275">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-275">eventTimestamp</span></span> |<span data-ttu-id="078c6-276">Tidsstämpel när händelsen skapades av tjänsten Azure motsvarande händelsen begäran bearbetades.</span><span class="sxs-lookup"><span data-stu-id="078c6-276">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="078c6-277">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-277">submissionTimestamp</span></span> |<span data-ttu-id="078c6-278">Tidsstämpel när händelsen blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="078c6-278">Timestamp when the event became available for querying.</span></span> |
| <span data-ttu-id="078c6-279">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="078c6-279">subscriptionId</span></span> |<span data-ttu-id="078c6-280">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="078c6-280">Azure Subscription Id.</span></span> |

### <a name="properties-field-per-alert-type"></a><span data-ttu-id="078c6-281">Egenskapsfältet per typ av avisering</span><span class="sxs-lookup"><span data-stu-id="078c6-281">Properties field per alert type</span></span>
<span data-ttu-id="078c6-282">Egenskapsfältet innehåller olika värden beroende på aviseringen händelsens källa.</span><span class="sxs-lookup"><span data-stu-id="078c6-282">The properties field will contain different values depending on the source of the alert event.</span></span> <span data-ttu-id="078c6-283">Två vanliga varning avisering providers är aktivitetsloggen aviseringar och mått aviseringar.</span><span class="sxs-lookup"><span data-stu-id="078c6-283">Two common alert event providers are Activity Log alerts and metric alerts.</span></span>

#### <a name="properties-for-activity-log-alerts"></a><span data-ttu-id="078c6-284">Egenskaper för aktivitetsloggen aviseringar</span><span class="sxs-lookup"><span data-stu-id="078c6-284">Properties for Activity Log alerts</span></span>
| <span data-ttu-id="078c6-285">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-285">Element Name</span></span> | <span data-ttu-id="078c6-286">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="078c6-287">properties.subscriptionId</span><span class="sxs-lookup"><span data-stu-id="078c6-287">properties.subscriptionId</span></span> | <span data-ttu-id="078c6-288">Prenumerations-ID från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-288">The subscription ID from the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-289">properties.eventDataId</span><span class="sxs-lookup"><span data-stu-id="078c6-289">properties.eventDataId</span></span> | <span data-ttu-id="078c6-290">Händelse data-ID från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-290">The event data ID from the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-291">properties.resourceGroup</span><span class="sxs-lookup"><span data-stu-id="078c6-291">properties.resourceGroup</span></span> | <span data-ttu-id="078c6-292">Resursgruppen från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-292">The resource group from the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-293">properties.resourceId</span><span class="sxs-lookup"><span data-stu-id="078c6-293">properties.resourceId</span></span> | <span data-ttu-id="078c6-294">Resurs-ID från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-294">The resource ID from the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-295">properties.eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-295">properties.eventTimestamp</span></span> | <span data-ttu-id="078c6-296">Händelsen tidsstämpel för den aktiviteten logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-296">The event timestamp of the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-297">properties.operationName</span><span class="sxs-lookup"><span data-stu-id="078c6-297">properties.operationName</span></span> | <span data-ttu-id="078c6-298">Åtgärdsnamnet från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-298">The operation name from the activity log event which caused this activity log alert rule to be activated.</span></span> |
| <span data-ttu-id="078c6-299">Properties.status</span><span class="sxs-lookup"><span data-stu-id="078c6-299">properties.status</span></span> | <span data-ttu-id="078c6-300">Status från den aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="078c6-300">The status from the activity log event which caused this activity log alert rule to be activated.</span></span>|

#### <a name="properties-for-metric-alerts"></a><span data-ttu-id="078c6-301">Egenskaper för mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="078c6-301">Properties for metric alerts</span></span>
| <span data-ttu-id="078c6-302">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-302">Element Name</span></span> | <span data-ttu-id="078c6-303">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-303">Description</span></span> |
| --- | --- |
| <span data-ttu-id="078c6-304">Egenskaper. RuleUri</span><span class="sxs-lookup"><span data-stu-id="078c6-304">properties.RuleUri</span></span> | <span data-ttu-id="078c6-305">Resurs-ID för mått varningsregeln sig själv.</span><span class="sxs-lookup"><span data-stu-id="078c6-305">Resource ID of the metric alert rule itself.</span></span> |
| <span data-ttu-id="078c6-306">Egenskaper. Regelnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-306">properties.RuleName</span></span> | <span data-ttu-id="078c6-307">Namnet på regeln mått.</span><span class="sxs-lookup"><span data-stu-id="078c6-307">The name of the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-308">Egenskaper. RuleDescription</span><span class="sxs-lookup"><span data-stu-id="078c6-308">properties.RuleDescription</span></span> | <span data-ttu-id="078c6-309">Beskrivning av mått varningsregeln (enligt definitionen i varningsregeln).</span><span class="sxs-lookup"><span data-stu-id="078c6-309">The description of the metric alert rule (as defined in the alert rule).</span></span> |
| <span data-ttu-id="078c6-310">Egenskaper. Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="078c6-310">properties.Threshold</span></span> | <span data-ttu-id="078c6-311">Tröskelvärdet används i utvärderingen av mått varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-311">The threshold value used in the evaluation of the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-312">Egenskaper. WindowSizeInMinutes</span><span class="sxs-lookup"><span data-stu-id="078c6-312">properties.WindowSizeInMinutes</span></span> | <span data-ttu-id="078c6-313">Fönsterstorleken användas vid utvärderingen av mått varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-313">The window size used in the evaluation of the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-314">Egenskaper. Aggregeringen</span><span class="sxs-lookup"><span data-stu-id="078c6-314">properties.Aggregation</span></span> | <span data-ttu-id="078c6-315">Sammansättningstyp som definierats i regeln mått.</span><span class="sxs-lookup"><span data-stu-id="078c6-315">The aggregation type defined in the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-316">Egenskaper. Operatorn</span><span class="sxs-lookup"><span data-stu-id="078c6-316">properties.Operator</span></span> | <span data-ttu-id="078c6-317">Villkorsstyrd operatör som används i utvärderingen av mått varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-317">The conditional operator used in the evaluation of the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-318">Egenskaper. MetricName</span><span class="sxs-lookup"><span data-stu-id="078c6-318">properties.MetricName</span></span> | <span data-ttu-id="078c6-319">Måttnamnet av måttet används i utvärderingen av mått varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-319">The metric name of the metric used in the evaluation of the metric alert rule.</span></span> |
| <span data-ttu-id="078c6-320">Egenskaper. MetricUnit</span><span class="sxs-lookup"><span data-stu-id="078c6-320">properties.MetricUnit</span></span> | <span data-ttu-id="078c6-321">Mått enhet för måttet används i utvärderingen av mått varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="078c6-321">The metric unit for the metric used in the evaluation of the metric alert rule.</span></span> |

## <a name="autoscale"></a><span data-ttu-id="078c6-322">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="078c6-322">Autoscale</span></span>
<span data-ttu-id="078c6-323">Den här kategorin innehåller en förteckning över alla händelser relaterade till åtgärden Autoskala motorns baserat på automatiska inställningar du har definierat i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="078c6-323">This category contains the record of any events related to the operation of the autoscale engine based on any autoscale settings you have defined in your subscription.</span></span> <span data-ttu-id="078c6-324">Ett exempel på typen av händelse visas i den här kategorin är ”Autoskala skalas upp misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="078c6-324">An example of the type of event you would see in this category is "Autoscale scale up action failed."</span></span> <span data-ttu-id="078c6-325">Använda autoskalning kan du automatiskt skala ut eller skala antalet instanser i en stöds resurstyp baserat på tid på dagen och/eller belastningen (mått) data med hjälp av en autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="078c6-325">Using autoscale, you can automatically scale out or scale in the number of instances in a supported resource type based on time of day and/or load (metric) data using an autoscale setting.</span></span> <span data-ttu-id="078c6-326">När villkoren är uppfyllda skala upp eller ned början och lyckades eller registreras misslyckade händelser i den här kategorin.</span><span class="sxs-lookup"><span data-stu-id="078c6-326">When the conditions are met to scale up or down, the start and succeeded or failed events will be recorded in this category.</span></span>

### <a name="sample-event"></a><span data-ttu-id="078c6-327">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="078c6-327">Sample event</span></span>
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a><span data-ttu-id="078c6-328">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="078c6-328">Property descriptions</span></span>
| <span data-ttu-id="078c6-329">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="078c6-329">Element Name</span></span> | <span data-ttu-id="078c6-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-330">Description</span></span> |
| --- | --- |
| <span data-ttu-id="078c6-331">Anroparen</span><span class="sxs-lookup"><span data-stu-id="078c6-331">caller</span></span> | <span data-ttu-id="078c6-332">Alltid Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="078c6-332">Always Microsoft.Insights/autoscaleSettings</span></span> |
| <span data-ttu-id="078c6-333">kanaler</span><span class="sxs-lookup"><span data-stu-id="078c6-333">channels</span></span> | <span data-ttu-id="078c6-334">Alltid ”Admin, åtgärden”</span><span class="sxs-lookup"><span data-stu-id="078c6-334">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="078c6-335">Anspråk</span><span class="sxs-lookup"><span data-stu-id="078c6-335">claims</span></span> | <span data-ttu-id="078c6-336">JSON-blob med typen SPN (service principal name) eller resurs Autoskala-motorn.</span><span class="sxs-lookup"><span data-stu-id="078c6-336">JSON blob with the SPN (service principal name), or resource type, of the autoscale engine.</span></span> |
| <span data-ttu-id="078c6-337">correlationId</span><span class="sxs-lookup"><span data-stu-id="078c6-337">correlationId</span></span> | <span data-ttu-id="078c6-338">Ett GUID i strängformatet.</span><span class="sxs-lookup"><span data-stu-id="078c6-338">A GUID in the string format.</span></span> |
| <span data-ttu-id="078c6-339">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-339">description</span></span> |<span data-ttu-id="078c6-340">Statisk textbeskrivning av händelsen Autoskala.</span><span class="sxs-lookup"><span data-stu-id="078c6-340">Static text description of the autoscale event.</span></span> |
| <span data-ttu-id="078c6-341">eventDataId</span><span class="sxs-lookup"><span data-stu-id="078c6-341">eventDataId</span></span> |<span data-ttu-id="078c6-342">Unik identifierare för händelsen Autoskala.</span><span class="sxs-lookup"><span data-stu-id="078c6-342">Unique identifier of the autoscale event.</span></span> |
| <span data-ttu-id="078c6-343">nivå</span><span class="sxs-lookup"><span data-stu-id="078c6-343">level</span></span> |<span data-ttu-id="078c6-344">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-344">Level of the event.</span></span> <span data-ttu-id="078c6-345">Ett av följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="078c6-345">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="078c6-346">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="078c6-346">resourceGroupName</span></span> |<span data-ttu-id="078c6-347">Namnet på resursgruppen för autoskalningsinställningen.</span><span class="sxs-lookup"><span data-stu-id="078c6-347">Name of the resource group for the autoscale setting.</span></span> |
| <span data-ttu-id="078c6-348">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="078c6-348">resourceProviderName</span></span> |<span data-ttu-id="078c6-349">Namnet på resursprovidern för autoskalningsinställningen.</span><span class="sxs-lookup"><span data-stu-id="078c6-349">Name of the resource provider for the autoscale setting.</span></span> |
| <span data-ttu-id="078c6-350">resourceId</span><span class="sxs-lookup"><span data-stu-id="078c6-350">resourceId</span></span> |<span data-ttu-id="078c6-351">Resurs-id autoskalningsinställningens.</span><span class="sxs-lookup"><span data-stu-id="078c6-351">Resource id of the autoscale setting.</span></span> |
| <span data-ttu-id="078c6-352">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="078c6-352">operationId</span></span> |<span data-ttu-id="078c6-353">Ett GUID som delas mellan de händelser som motsvarar en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="078c6-353">A GUID shared among the events that correspond to a single operation.</span></span> |
| <span data-ttu-id="078c6-354">operationName</span><span class="sxs-lookup"><span data-stu-id="078c6-354">operationName</span></span> |<span data-ttu-id="078c6-355">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-355">Name of the operation.</span></span> |
| <span data-ttu-id="078c6-356">properties</span><span class="sxs-lookup"><span data-stu-id="078c6-356">properties</span></span> |<span data-ttu-id="078c6-357">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="078c6-357">Set of `<Key, Value>` pairs (that is, a Dictionary) describing the details of the event.</span></span> |
| <span data-ttu-id="078c6-358">Egenskaper. Beskrivning</span><span class="sxs-lookup"><span data-stu-id="078c6-358">properties.Description</span></span> | <span data-ttu-id="078c6-359">Detaljerad beskrivning av vad Autoskala motorn gör.</span><span class="sxs-lookup"><span data-stu-id="078c6-359">Detailed description of what the autoscale engine was doing.</span></span> |
| <span data-ttu-id="078c6-360">Egenskaper. ResourceName</span><span class="sxs-lookup"><span data-stu-id="078c6-360">properties.ResourceName</span></span> | <span data-ttu-id="078c6-361">Resurs-ID för resursen påverkas (den resurs där skalningsåtgärden utfördes)</span><span class="sxs-lookup"><span data-stu-id="078c6-361">Resource ID of the impacted resource (the resource on which the scale action was being performed)</span></span> |
| <span data-ttu-id="078c6-362">Egenskaper. OldInstancesCount</span><span class="sxs-lookup"><span data-stu-id="078c6-362">properties.OldInstancesCount</span></span> | <span data-ttu-id="078c6-363">Antalet instanser innan åtgärden Autoskala tog effekt.</span><span class="sxs-lookup"><span data-stu-id="078c6-363">The number of instances before the autoscale action took effect.</span></span> |
| <span data-ttu-id="078c6-364">Egenskaper. NewInstancesCount</span><span class="sxs-lookup"><span data-stu-id="078c6-364">properties.NewInstancesCount</span></span> | <span data-ttu-id="078c6-365">Antalet instanser efter åtgärden Autoskala tog effekt.</span><span class="sxs-lookup"><span data-stu-id="078c6-365">The number of instances after the autoscale action took effect.</span></span> |
| <span data-ttu-id="078c6-366">Egenskaper. LastScaleActionTime</span><span class="sxs-lookup"><span data-stu-id="078c6-366">properties.LastScaleActionTime</span></span> | <span data-ttu-id="078c6-367">Tidsstämpel när Autoskala åtgärden utfördes.</span><span class="sxs-lookup"><span data-stu-id="078c6-367">The timestamp of when the autoscale action occurred.</span></span> |
| <span data-ttu-id="078c6-368">status</span><span class="sxs-lookup"><span data-stu-id="078c6-368">status</span></span> |<span data-ttu-id="078c6-369">Sträng som beskriver status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="078c6-369">String describing the status of the operation.</span></span> <span data-ttu-id="078c6-370">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="078c6-370">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="078c6-371">subStatus</span><span class="sxs-lookup"><span data-stu-id="078c6-371">subStatus</span></span> | <span data-ttu-id="078c6-372">Vanligtvis är null för Autoskala.</span><span class="sxs-lookup"><span data-stu-id="078c6-372">Usually null for autoscale.</span></span> |
| <span data-ttu-id="078c6-373">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-373">eventTimestamp</span></span> |<span data-ttu-id="078c6-374">Tidsstämpel när händelsen skapades av tjänsten Azure motsvarande händelsen begäran bearbetades.</span><span class="sxs-lookup"><span data-stu-id="078c6-374">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="078c6-375">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="078c6-375">submissionTimestamp</span></span> |<span data-ttu-id="078c6-376">Tidsstämpel när händelsen blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="078c6-376">Timestamp when the event became available for querying.</span></span> |
| <span data-ttu-id="078c6-377">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="078c6-377">subscriptionId</span></span> |<span data-ttu-id="078c6-378">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="078c6-378">Azure Subscription Id.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="078c6-379">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="078c6-379">Next steps</span></span>
* [<span data-ttu-id="078c6-380">Mer information om aktivitetsloggen (tidigare granskningsloggar)</span><span class="sxs-lookup"><span data-stu-id="078c6-380">Learn more about the Activity Log (formerly Audit Logs)</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="078c6-381">Dataströmmen Azure aktivitetsloggen i Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="078c6-381">Stream the Azure Activity Log to Event Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
