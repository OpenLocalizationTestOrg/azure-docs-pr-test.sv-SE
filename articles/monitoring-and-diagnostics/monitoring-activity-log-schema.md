---
title: "aaaAzure händelse för aktivitetslogg | Microsoft Docs"
description: "Förstå hello Händelseschema för data som sänds till hello aktivitetsloggen"
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
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a><span data-ttu-id="0eda4-103">Azure aktivitetsloggen Händelseschema</span><span class="sxs-lookup"><span data-stu-id="0eda4-103">Azure Activity Log event schema</span></span>
<span data-ttu-id="0eda4-104">Hej **Azure-aktivitetsloggen** är en logg som ger inblick i prenumerationsnivån händelser som har inträffat i Azure.</span><span class="sxs-lookup"><span data-stu-id="0eda4-104">hello **Azure Activity Log** is a log that provides insight into any subscription-level events that have occurred in Azure.</span></span> <span data-ttu-id="0eda4-105">Den här artikeln beskriver hello Händelseschema per kategori av data.</span><span class="sxs-lookup"><span data-stu-id="0eda4-105">This article describes hello event schema per category of data.</span></span>

## <a name="administrative"></a><span data-ttu-id="0eda4-106">Administrativa</span><span class="sxs-lookup"><span data-stu-id="0eda4-106">Administrative</span></span>
<span data-ttu-id="0eda4-107">Den här kategorin innehåller hello post för alla skapa, uppdatera, ta bort och åtgärden åtgärder utföras via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0eda4-107">This category contains hello record of all create, update, delete, and action operations performed through Resource Manager.</span></span> <span data-ttu-id="0eda4-108">Exempel på typer av händelser som visas i den här kategorin omfattar hello ”Skapa virtuell dator” och ”ta bort nätverkssäkerhetsgruppen” varje åtgärd som en användare eller program med hjälp av hanteraren för filserverresurser modelleras som en åtgärd på en viss resurstyp.</span><span class="sxs-lookup"><span data-stu-id="0eda4-108">Examples of hello types of events you would see in this category include "create virtual machine" and "delete network security group" Every action taken by a user or application using Resource Manager is modeled as an operation on a particular resource type.</span></span> <span data-ttu-id="0eda4-109">Om hello åtgärd av typen skriva, ta bort eller åtgärden hello-poster för både hello start- och lyckas eller misslyckas av åtgärden registreras i hello administrativa kategorin.</span><span class="sxs-lookup"><span data-stu-id="0eda4-109">If hello operation type is Write, Delete, or Action, hello records of both hello start and success or fail of that operation are recorded in hello Administrative category.</span></span> <span data-ttu-id="0eda4-110">hello administrativa kategorin omfattar även eventuella ändringar toorole-baserad åtkomstkontroll i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0eda4-110">hello Administrative category also includes any changes toorole-based access control in a subscription.</span></span>

### <a name="sample-event"></a><span data-ttu-id="0eda4-111">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="0eda4-111">Sample event</span></span>
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

### <a name="property-descriptions"></a><span data-ttu-id="0eda4-112">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="0eda4-112">Property descriptions</span></span>
| <span data-ttu-id="0eda4-113">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-113">Element Name</span></span> | <span data-ttu-id="0eda4-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0eda4-115">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="0eda4-115">authorization</span></span> |<span data-ttu-id="0eda4-116">BLOB RBAC egenskaper för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-116">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="0eda4-117">Normalt innehåller hello ””, ”roll” och ”omfattning” egenskaper.</span><span class="sxs-lookup"><span data-stu-id="0eda4-117">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="0eda4-118">Anroparen</span><span class="sxs-lookup"><span data-stu-id="0eda4-118">caller</span></span> |<span data-ttu-id="0eda4-119">E-postadress för hello-användare som har utfört hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="0eda4-119">Email address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="0eda4-120">kanaler</span><span class="sxs-lookup"><span data-stu-id="0eda4-120">channels</span></span> |<span data-ttu-id="0eda4-121">En av hello följande värden: ”Admin”, ”åtgärden”</span><span class="sxs-lookup"><span data-stu-id="0eda4-121">One of hello following values: “Admin”, “Operation”</span></span> |
| <span data-ttu-id="0eda4-122">Anspråk</span><span class="sxs-lookup"><span data-stu-id="0eda4-122">claims</span></span> |<span data-ttu-id="0eda4-123">Hej JWT-token som används av Active Directory tooauthenticate hello användaren eller programmet tooperform åtgärden i hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="0eda4-123">hello JWT token used by Active Directory tooauthenticate hello user or application tooperform this operation in resource manager.</span></span> |
| <span data-ttu-id="0eda4-124">correlationId</span><span class="sxs-lookup"><span data-stu-id="0eda4-124">correlationId</span></span> |<span data-ttu-id="0eda4-125">Vanligtvis ett GUID i hello-strängformat.</span><span class="sxs-lookup"><span data-stu-id="0eda4-125">Usually a GUID in hello string format.</span></span> <span data-ttu-id="0eda4-126">Händelser som delar en correlationId tillhör toohello samma uber åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-126">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="0eda4-127">description</span><span class="sxs-lookup"><span data-stu-id="0eda4-127">description</span></span> |<span data-ttu-id="0eda4-128">Statisk textbeskrivning av en händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-128">Static text description of an event.</span></span> |
| <span data-ttu-id="0eda4-129">eventDataId</span><span class="sxs-lookup"><span data-stu-id="0eda4-129">eventDataId</span></span> |<span data-ttu-id="0eda4-130">Unik identifierare för en händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-130">Unique identifier of an event.</span></span> |
| <span data-ttu-id="0eda4-131">httpRequest</span><span class="sxs-lookup"><span data-stu-id="0eda4-131">httpRequest</span></span> |<span data-ttu-id="0eda4-132">BLOB som beskriver hello Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="0eda4-132">Blob describing hello Http Request.</span></span> <span data-ttu-id="0eda4-133">Inkluderar vanligtvis hello ”clientRequestId”, ”clientIpAddress” och ”metod” (HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="0eda4-133">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method.</span></span> <span data-ttu-id="0eda4-134">Till exempel PLACERA).</span><span class="sxs-lookup"><span data-stu-id="0eda4-134">For example, PUT).</span></span> |
| <span data-ttu-id="0eda4-135">nivå</span><span class="sxs-lookup"><span data-stu-id="0eda4-135">level</span></span> |<span data-ttu-id="0eda4-136">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-136">Level of hello event.</span></span> <span data-ttu-id="0eda4-137">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="0eda4-137">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="0eda4-138">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="0eda4-138">resourceGroupName</span></span> |<span data-ttu-id="0eda4-139">Namnet på resursgruppen hello för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="0eda4-139">Name of hello resource group for hello impacted resource.</span></span> |
| <span data-ttu-id="0eda4-140">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="0eda4-140">resourceProviderName</span></span> |<span data-ttu-id="0eda4-141">Namnet på hello resource provider för hello påverkas resurs</span><span class="sxs-lookup"><span data-stu-id="0eda4-141">Name of hello resource provider for hello impacted resource</span></span> |
| <span data-ttu-id="0eda4-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="0eda4-142">resourceId</span></span> |<span data-ttu-id="0eda4-143">Resurs-id för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="0eda4-143">Resource id of hello impacted resource.</span></span> |
| <span data-ttu-id="0eda4-144">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="0eda4-144">operationId</span></span> |<span data-ttu-id="0eda4-145">Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-145">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="0eda4-146">operationName</span><span class="sxs-lookup"><span data-stu-id="0eda4-146">operationName</span></span> |<span data-ttu-id="0eda4-147">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-147">Name of hello operation.</span></span> |
| <span data-ttu-id="0eda4-148">properties</span><span class="sxs-lookup"><span data-stu-id="0eda4-148">properties</span></span> |<span data-ttu-id="0eda4-149">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-149">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="0eda4-150">status</span><span class="sxs-lookup"><span data-stu-id="0eda4-150">status</span></span> |<span data-ttu-id="0eda4-151">Sträng som beskriver hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-151">String describing hello status of hello operation.</span></span> <span data-ttu-id="0eda4-152">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-152">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="0eda4-153">subStatus</span><span class="sxs-lookup"><span data-stu-id="0eda4-153">subStatus</span></span> |<span data-ttu-id="0eda4-154">Vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop, men kan även innehålla andra strängar som beskriver en sådan, till exempel värdena vanliga: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), Nej innehåll (HTTP Statuskod: 204), felaktig begäran (HTTP-statuskod: 400) inte hittades (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409), internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504).</span><span class="sxs-lookup"><span data-stu-id="0eda4-154">Usually hello HTTP status code of hello corresponding REST call, but can also include other strings describing a substatus, such as these common values: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504).</span></span> |
| <span data-ttu-id="0eda4-155">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-155">eventTimestamp</span></span> |<span data-ttu-id="0eda4-156">Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-156">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="0eda4-157">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-157">submissionTimestamp</span></span> |<span data-ttu-id="0eda4-158">Tidsstämpel när hello händelse blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="0eda4-158">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="0eda4-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="0eda4-159">subscriptionId</span></span> |<span data-ttu-id="0eda4-160">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="0eda4-160">Azure Subscription Id.</span></span> |

## <a name="service-health"></a><span data-ttu-id="0eda4-161">Tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="0eda4-161">Service health</span></span>
<span data-ttu-id="0eda4-162">Den här kategorin innehåller hello post för varje service hälsa incidenter som har inträffat i Azure.</span><span class="sxs-lookup"><span data-stu-id="0eda4-162">This category contains hello record of any service health incidents that have occurred in Azure.</span></span> <span data-ttu-id="0eda4-163">Ett exempel på hello typen av händelse visas i den här kategorin är ”SQL Azure i östra USA upplever driftstopp”.</span><span class="sxs-lookup"><span data-stu-id="0eda4-163">An example of hello type of event you would see in this category is "SQL Azure in East US is experiencing downtime."</span></span> <span data-ttu-id="0eda4-164">Hälsa av tjänsten-händelser finns i fem sorter: åtgärd krävs stödd återställning, Incident, underhåll, Information eller säkerhet, och visas endast om du har en resurs i hello-prenumeration som skulle påverkas av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-164">Service health events come in five varieties: Action Required, Assisted Recovery, Incident, Maintenance, Information, or Security, and only appear if you have a resource in hello subscription that would be impacted by hello event.</span></span>

### <a name="sample-event"></a><span data-ttu-id="0eda4-165">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="0eda4-165">Sample event</span></span>
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
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a><span data-ttu-id="0eda4-166">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="0eda4-166">Property descriptions</span></span>
<span data-ttu-id="0eda4-167">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-167">Element Name</span></span> | <span data-ttu-id="0eda4-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-168">Description</span></span>
-------- | -----------
<span data-ttu-id="0eda4-169">kanaler</span><span class="sxs-lookup"><span data-stu-id="0eda4-169">channels</span></span> | <span data-ttu-id="0eda4-170">Är en av hello följande värden: ”Admin”, ”åtgärden”</span><span class="sxs-lookup"><span data-stu-id="0eda4-170">Is one of hello following values: “Admin”, “Operation”</span></span>
<span data-ttu-id="0eda4-171">correlationId</span><span class="sxs-lookup"><span data-stu-id="0eda4-171">correlationId</span></span> | <span data-ttu-id="0eda4-172">Är vanligtvis ett GUID i hello-strängformat.</span><span class="sxs-lookup"><span data-stu-id="0eda4-172">Is usually a GUID in hello string format.</span></span> <span data-ttu-id="0eda4-173">Händelser med som tillhör toohello vanligtvis delar samma uber åtgärd hello samma correlationId.</span><span class="sxs-lookup"><span data-stu-id="0eda4-173">Events with that belong toohello same uber action usually share hello same correlationId.</span></span>
<span data-ttu-id="0eda4-174">description</span><span class="sxs-lookup"><span data-stu-id="0eda4-174">description</span></span> | <span data-ttu-id="0eda4-175">Beskrivning av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-175">Description of hello event.</span></span>
<span data-ttu-id="0eda4-176">eventDataId</span><span class="sxs-lookup"><span data-stu-id="0eda4-176">eventDataId</span></span> | <span data-ttu-id="0eda4-177">hello Unik identifierare för en händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-177">hello unique identifier of an event.</span></span>
<span data-ttu-id="0eda4-178">EventName</span><span class="sxs-lookup"><span data-stu-id="0eda4-178">eventName</span></span> | <span data-ttu-id="0eda4-179">hello titel hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-179">hello title of hello event.</span></span>
<span data-ttu-id="0eda4-180">nivå</span><span class="sxs-lookup"><span data-stu-id="0eda4-180">level</span></span> | <span data-ttu-id="0eda4-181">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-181">Level of hello event.</span></span> <span data-ttu-id="0eda4-182">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="0eda4-182">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span>
<span data-ttu-id="0eda4-183">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="0eda4-183">resourceProviderName</span></span> | <span data-ttu-id="0eda4-184">Namnet på hello resource provider för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="0eda4-184">Name of hello resource provider for hello impacted resource.</span></span> <span data-ttu-id="0eda4-185">Detta kan vara null om det är inte känt.</span><span class="sxs-lookup"><span data-stu-id="0eda4-185">If not known, this will be null.</span></span>
<span data-ttu-id="0eda4-186">resourceType</span><span class="sxs-lookup"><span data-stu-id="0eda4-186">resourceType</span></span>| <span data-ttu-id="0eda4-187">hello typ av resurs av hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="0eda4-187">hello type of resource of hello impacted resource.</span></span> <span data-ttu-id="0eda4-188">Detta kan vara null om det är inte känt.</span><span class="sxs-lookup"><span data-stu-id="0eda4-188">If not known, this will be null.</span></span>
<span data-ttu-id="0eda4-189">subStatus</span><span class="sxs-lookup"><span data-stu-id="0eda4-189">subStatus</span></span> | <span data-ttu-id="0eda4-190">Vanligtvis är null för händelser för Hälsotjänst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-190">Usually null for Service Health events.</span></span>
<span data-ttu-id="0eda4-191">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-191">eventTimestamp</span></span> | <span data-ttu-id="0eda4-192">Tidsstämpel när hello logga en händelse har genererats och skickats toohello aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-192">Timestamp when hello log event was generated and submitted toohello Activity Log.</span></span>
<span data-ttu-id="0eda4-193">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-193">submissionTimestamp</span></span> |   <span data-ttu-id="0eda4-194">Tidsstämpel när hello händelse blev tillgängliga i hello aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-194">Timestamp when hello event became available in hello Activity Log.</span></span>
<span data-ttu-id="0eda4-195">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="0eda4-195">subscriptionId</span></span> | <span data-ttu-id="0eda4-196">hello Azure-prenumeration som den här händelsen loggades.</span><span class="sxs-lookup"><span data-stu-id="0eda4-196">hello Azure subscription in which this event was logged.</span></span>
<span data-ttu-id="0eda4-197">status</span><span class="sxs-lookup"><span data-stu-id="0eda4-197">status</span></span> | <span data-ttu-id="0eda4-198">Sträng som beskriver hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-198">String describing hello status of hello operation.</span></span> <span data-ttu-id="0eda4-199">Vissa vanliga värden: aktiva, löst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-199">Some common values are: Active, Resolved.</span></span>
<span data-ttu-id="0eda4-200">operationName</span><span class="sxs-lookup"><span data-stu-id="0eda4-200">operationName</span></span> | <span data-ttu-id="0eda4-201">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-201">Name of hello operation.</span></span> <span data-ttu-id="0eda4-202">Vanligtvis Microsoft.ServiceHealth/incident/action.</span><span class="sxs-lookup"><span data-stu-id="0eda4-202">Usually Microsoft.ServiceHealth/incident/action.</span></span>
<span data-ttu-id="0eda4-203">category</span><span class="sxs-lookup"><span data-stu-id="0eda4-203">category</span></span> | <span data-ttu-id="0eda4-204">”ServiceHealth”</span><span class="sxs-lookup"><span data-stu-id="0eda4-204">"ServiceHealth"</span></span>
<span data-ttu-id="0eda4-205">resourceId</span><span class="sxs-lookup"><span data-stu-id="0eda4-205">resourceId</span></span> | <span data-ttu-id="0eda4-206">Resurs-id för hello påverkas resurs, om den är känd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-206">Resource id of hello impacted resource, if known.</span></span> <span data-ttu-id="0eda4-207">Prenumerations-ID har angetts på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="0eda4-207">Subscription ID is provided otherwise.</span></span>
<span data-ttu-id="0eda4-208">Properties.title</span><span class="sxs-lookup"><span data-stu-id="0eda4-208">Properties.title</span></span> | <span data-ttu-id="0eda4-209">hello lokaliserade rubrik för den här kommunikationen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-209">hello localized title for this communication.</span></span> <span data-ttu-id="0eda4-210">Engelska är standardspråk för hello.</span><span class="sxs-lookup"><span data-stu-id="0eda4-210">English is hello default language.</span></span>
<span data-ttu-id="0eda4-211">Properties.Communication</span><span class="sxs-lookup"><span data-stu-id="0eda4-211">Properties.communication</span></span> | <span data-ttu-id="0eda4-212">hello lokaliserad information hello-kommunikation med HTML-kod.</span><span class="sxs-lookup"><span data-stu-id="0eda4-212">hello localized details of hello communication with HTML markup.</span></span> <span data-ttu-id="0eda4-213">Engelska är hello som standard.</span><span class="sxs-lookup"><span data-stu-id="0eda4-213">English is hello default.</span></span>
<span data-ttu-id="0eda4-214">Properties.incidentType</span><span class="sxs-lookup"><span data-stu-id="0eda4-214">Properties.incidentType</span></span> | <span data-ttu-id="0eda4-215">Möjliga värden: AssistedRecovery, ActionRequired, Information, incidenter, underhåll, säkerhet</span><span class="sxs-lookup"><span data-stu-id="0eda4-215">Possible values: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security</span></span>
<span data-ttu-id="0eda4-216">Properties.trackingId</span><span class="sxs-lookup"><span data-stu-id="0eda4-216">Properties.trackingId</span></span> | <span data-ttu-id="0eda4-217">Identifierar hello incident som den här händelsen är associerad med.</span><span class="sxs-lookup"><span data-stu-id="0eda4-217">Identifies hello incident this event is associated with.</span></span> <span data-ttu-id="0eda4-218">Använd den här toocorrelate hello händelser relaterade tooan incidenten.</span><span class="sxs-lookup"><span data-stu-id="0eda4-218">Use this toocorrelate hello events related tooan incident.</span></span>
<span data-ttu-id="0eda4-219">Properties.impactedServices</span><span class="sxs-lookup"><span data-stu-id="0eda4-219">Properties.impactedServices</span></span> | <span data-ttu-id="0eda4-220">En ESC JSON-blob som beskriver hello tjänster och områden som påverkas av hello incident.</span><span class="sxs-lookup"><span data-stu-id="0eda4-220">An escaped JSON blob which describes hello services and regions that are impacted by hello incident.</span></span> <span data-ttu-id="0eda4-221">En lista över tjänster, som har en ServiceName och en lista över ImpactedRegions, som har en RegionName.</span><span class="sxs-lookup"><span data-stu-id="0eda4-221">A list of Services, each of which has a ServiceName and a list of ImpactedRegions, each of which has a RegionName.</span></span>
<span data-ttu-id="0eda4-222">Properties.defaultLanguageTitle</span><span class="sxs-lookup"><span data-stu-id="0eda4-222">Properties.defaultLanguageTitle</span></span> | <span data-ttu-id="0eda4-223">hello-kommunikation på engelska</span><span class="sxs-lookup"><span data-stu-id="0eda4-223">hello communication in English</span></span>
<span data-ttu-id="0eda4-224">Properties.defaultLanguageContent</span><span class="sxs-lookup"><span data-stu-id="0eda4-224">Properties.defaultLanguageContent</span></span> | <span data-ttu-id="0eda4-225">hello-kommunikation på engelska som HTML-kod eller oformaterad text</span><span class="sxs-lookup"><span data-stu-id="0eda4-225">hello communication in English as either html markup or plain text</span></span>
<span data-ttu-id="0eda4-226">Properties.Stage</span><span class="sxs-lookup"><span data-stu-id="0eda4-226">Properties.stage</span></span> | <span data-ttu-id="0eda4-227">Möjliga värden för AssistedRecovery, ActionRequired, Information, incidenter, säkerhet: är aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-227">Possible values for AssistedRecovery, ActionRequired, Information, Incident, Security: are Active, Resolved.</span></span> <span data-ttu-id="0eda4-228">De är för underhåll: aktiv, planerad, InProgress, Avbruten, Rescheduled, löst, Slutför</span><span class="sxs-lookup"><span data-stu-id="0eda4-228">For Maintenance they are: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete</span></span>
<span data-ttu-id="0eda4-229">Properties.communicationId</span><span class="sxs-lookup"><span data-stu-id="0eda4-229">Properties.communicationId</span></span> | <span data-ttu-id="0eda4-230">hello kommunikation den här händelsen är kopplat.</span><span class="sxs-lookup"><span data-stu-id="0eda4-230">hello communication this event is associated.</span></span>

## <a name="alert"></a><span data-ttu-id="0eda4-231">Varning</span><span class="sxs-lookup"><span data-stu-id="0eda4-231">Alert</span></span>
<span data-ttu-id="0eda4-232">Den här kategorin innehåller hello post för alla aktiveringar av Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0eda4-232">This category contains hello record of all activations of Azure alerts.</span></span> <span data-ttu-id="0eda4-233">Ett exempel på hello typen av händelse visas i den här kategorin är ”processor på myVM har över 80 för hello senaste 5 minuter”.</span><span class="sxs-lookup"><span data-stu-id="0eda4-233">An example of hello type of event you would see in this category is "CPU % on myVM has been over 80 for hello past 5 minutes."</span></span> <span data-ttu-id="0eda4-234">En mängd Azure system har en aviseringar begrepp – du kan definiera en regel av något slag och ett meddelande när villkor matchar regeln.</span><span class="sxs-lookup"><span data-stu-id="0eda4-234">A variety of Azure systems have an alerting concept -- you can define a rule of some sort and receive a notification when conditions match that rule.</span></span> <span data-ttu-id="0eda4-235">Varje gång en stöds Azure aviseringstyp 'aktiveras,' eller hello villkor är uppfyllda toogenerate ett meddelande, en post för hello aktiveringen skickas också hello aktivitetsloggen toothis kategori.</span><span class="sxs-lookup"><span data-stu-id="0eda4-235">Each time a supported Azure alert type 'activates,' or hello conditions are met toogenerate a notification, a record of hello activation is also pushed toothis category of hello Activity Log.</span></span>

### <a name="sample-event"></a><span data-ttu-id="0eda4-236">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="0eda4-236">Sample event</span></span>

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
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

### <a name="property-descriptions"></a><span data-ttu-id="0eda4-237">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="0eda4-237">Property descriptions</span></span>
| <span data-ttu-id="0eda4-238">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-238">Element Name</span></span> | <span data-ttu-id="0eda4-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-239">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0eda4-240">Anroparen</span><span class="sxs-lookup"><span data-stu-id="0eda4-240">caller</span></span> | <span data-ttu-id="0eda4-241">Alltid Microsoft.Insights/alertRules</span><span class="sxs-lookup"><span data-stu-id="0eda4-241">Always Microsoft.Insights/alertRules</span></span> |
| <span data-ttu-id="0eda4-242">kanaler</span><span class="sxs-lookup"><span data-stu-id="0eda4-242">channels</span></span> | <span data-ttu-id="0eda4-243">Alltid ”Admin, åtgärden”</span><span class="sxs-lookup"><span data-stu-id="0eda4-243">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="0eda4-244">Anspråk</span><span class="sxs-lookup"><span data-stu-id="0eda4-244">claims</span></span> | <span data-ttu-id="0eda4-245">JSON-blob med hello SPN (service principal name) eller resurs typ, varning hello-motorn.</span><span class="sxs-lookup"><span data-stu-id="0eda4-245">JSON blob with hello SPN (service principal name), or resource type, of hello alert engine.</span></span> |
| <span data-ttu-id="0eda4-246">correlationId</span><span class="sxs-lookup"><span data-stu-id="0eda4-246">correlationId</span></span> | <span data-ttu-id="0eda4-247">Ett GUID i hello-strängformat.</span><span class="sxs-lookup"><span data-stu-id="0eda4-247">A GUID in hello string format.</span></span> |
| <span data-ttu-id="0eda4-248">description</span><span class="sxs-lookup"><span data-stu-id="0eda4-248">description</span></span> |<span data-ttu-id="0eda4-249">Statisk textbeskrivning av hello varning avisering.</span><span class="sxs-lookup"><span data-stu-id="0eda4-249">Static text description of hello alert event.</span></span> |
| <span data-ttu-id="0eda4-250">eventDataId</span><span class="sxs-lookup"><span data-stu-id="0eda4-250">eventDataId</span></span> |<span data-ttu-id="0eda4-251">Unik identifierare för hello varning avisering.</span><span class="sxs-lookup"><span data-stu-id="0eda4-251">Unique identifier of hello alert event.</span></span> |
| <span data-ttu-id="0eda4-252">nivå</span><span class="sxs-lookup"><span data-stu-id="0eda4-252">level</span></span> |<span data-ttu-id="0eda4-253">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-253">Level of hello event.</span></span> <span data-ttu-id="0eda4-254">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="0eda4-254">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="0eda4-255">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="0eda4-255">resourceGroupName</span></span> |<span data-ttu-id="0eda4-256">Namnet på resursgruppen hello för hello påverkas resursen om den här varningen är mått.</span><span class="sxs-lookup"><span data-stu-id="0eda4-256">Name of hello resource group for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="0eda4-257">För andra aviseringstyper är hello namnet hello resursgruppen som innehåller hello avisering sig själv.</span><span class="sxs-lookup"><span data-stu-id="0eda4-257">For other alert types, this is hello name of hello resource group that contains hello alert itself.</span></span> |
| <span data-ttu-id="0eda4-258">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="0eda4-258">resourceProviderName</span></span> |<span data-ttu-id="0eda4-259">Namnet på hello resource provider för hello påverkas resursen om den här varningen är mått.</span><span class="sxs-lookup"><span data-stu-id="0eda4-259">Name of hello resource provider for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="0eda4-260">För andra aviseringstyper är hello namnet på hello resource provider för hello aviseringen sig själv.</span><span class="sxs-lookup"><span data-stu-id="0eda4-260">For other alert types, this is hello name of hello resource provider for hello alert itself.</span></span> |
| <span data-ttu-id="0eda4-261">resourceId</span><span class="sxs-lookup"><span data-stu-id="0eda4-261">resourceId</span></span> | <span data-ttu-id="0eda4-262">Namnet på hello resurs-ID för hello påverkas resursen om den här varningen är mått.</span><span class="sxs-lookup"><span data-stu-id="0eda4-262">Name of hello resource ID for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="0eda4-263">För andra aviseringstyper är hello resurs-ID för hello avisering resurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="0eda4-263">For other alert types, this is hello resource ID of hello alert resource itself.</span></span> |
| <span data-ttu-id="0eda4-264">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="0eda4-264">operationId</span></span> |<span data-ttu-id="0eda4-265">Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-265">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="0eda4-266">operationName</span><span class="sxs-lookup"><span data-stu-id="0eda4-266">operationName</span></span> |<span data-ttu-id="0eda4-267">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-267">Name of hello operation.</span></span> |
| <span data-ttu-id="0eda4-268">properties</span><span class="sxs-lookup"><span data-stu-id="0eda4-268">properties</span></span> |<span data-ttu-id="0eda4-269">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-269">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="0eda4-270">status</span><span class="sxs-lookup"><span data-stu-id="0eda4-270">status</span></span> |<span data-ttu-id="0eda4-271">Sträng som beskriver hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-271">String describing hello status of hello operation.</span></span> <span data-ttu-id="0eda4-272">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-272">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="0eda4-273">subStatus</span><span class="sxs-lookup"><span data-stu-id="0eda4-273">subStatus</span></span> | <span data-ttu-id="0eda4-274">Vanligtvis är null för aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0eda4-274">Usually null for alerts.</span></span> |
| <span data-ttu-id="0eda4-275">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-275">eventTimestamp</span></span> |<span data-ttu-id="0eda4-276">Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-276">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="0eda4-277">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-277">submissionTimestamp</span></span> |<span data-ttu-id="0eda4-278">Tidsstämpel när hello händelse blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="0eda4-278">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="0eda4-279">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="0eda4-279">subscriptionId</span></span> |<span data-ttu-id="0eda4-280">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="0eda4-280">Azure Subscription Id.</span></span> |

### <a name="properties-field-per-alert-type"></a><span data-ttu-id="0eda4-281">Egenskapsfältet per typ av avisering</span><span class="sxs-lookup"><span data-stu-id="0eda4-281">Properties field per alert type</span></span>
<span data-ttu-id="0eda4-282">hello innehåller egenskaper fältet olika värden beroende på hello källan för hello varning avisering.</span><span class="sxs-lookup"><span data-stu-id="0eda4-282">hello properties field will contain different values depending on hello source of hello alert event.</span></span> <span data-ttu-id="0eda4-283">Två vanliga varning avisering providers är aktivitetsloggen aviseringar och mått aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0eda4-283">Two common alert event providers are Activity Log alerts and metric alerts.</span></span>

#### <a name="properties-for-activity-log-alerts"></a><span data-ttu-id="0eda4-284">Egenskaper för aktivitetsloggen aviseringar</span><span class="sxs-lookup"><span data-stu-id="0eda4-284">Properties for Activity Log alerts</span></span>
| <span data-ttu-id="0eda4-285">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-285">Element Name</span></span> | <span data-ttu-id="0eda4-286">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0eda4-287">properties.subscriptionId</span><span class="sxs-lookup"><span data-stu-id="0eda4-287">properties.subscriptionId</span></span> | <span data-ttu-id="0eda4-288">hello prenumerations-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-288">hello subscription ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-289">properties.eventDataId</span><span class="sxs-lookup"><span data-stu-id="0eda4-289">properties.eventDataId</span></span> | <span data-ttu-id="0eda4-290">hello händelse data-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-290">hello event data ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-291">properties.resourceGroup</span><span class="sxs-lookup"><span data-stu-id="0eda4-291">properties.resourceGroup</span></span> | <span data-ttu-id="0eda4-292">hello resursgrupp från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-292">hello resource group from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-293">properties.resourceId</span><span class="sxs-lookup"><span data-stu-id="0eda4-293">properties.resourceId</span></span> | <span data-ttu-id="0eda4-294">hello resurs-ID från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-294">hello resource ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-295">properties.eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-295">properties.eventTimestamp</span></span> | <span data-ttu-id="0eda4-296">hello händelse tidsstämpel hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-296">hello event timestamp of hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-297">properties.operationName</span><span class="sxs-lookup"><span data-stu-id="0eda4-297">properties.operationName</span></span> | <span data-ttu-id="0eda4-298">hello åtgärdsnamn från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-298">hello operation name from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="0eda4-299">Properties.status</span><span class="sxs-lookup"><span data-stu-id="0eda4-299">properties.status</span></span> | <span data-ttu-id="0eda4-300">hello status från hello aktivitet logga en händelse som orsakat den här aktiviteten loggen varningsregeln toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0eda4-300">hello status from hello activity log event which caused this activity log alert rule toobe activated.</span></span>|

#### <a name="properties-for-metric-alerts"></a><span data-ttu-id="0eda4-301">Egenskaper för mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="0eda4-301">Properties for metric alerts</span></span>
| <span data-ttu-id="0eda4-302">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-302">Element Name</span></span> | <span data-ttu-id="0eda4-303">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-303">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0eda4-304">Egenskaper. RuleUri</span><span class="sxs-lookup"><span data-stu-id="0eda4-304">properties.RuleUri</span></span> | <span data-ttu-id="0eda4-305">Resurs-ID för hello mått varningsregeln sig själv.</span><span class="sxs-lookup"><span data-stu-id="0eda4-305">Resource ID of hello metric alert rule itself.</span></span> |
| <span data-ttu-id="0eda4-306">Egenskaper. Regelnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-306">properties.RuleName</span></span> | <span data-ttu-id="0eda4-307">hello namnet hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-307">hello name of hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-308">Egenskaper. RuleDescription</span><span class="sxs-lookup"><span data-stu-id="0eda4-308">properties.RuleDescription</span></span> | <span data-ttu-id="0eda4-309">hello beskrivning av hello mått varningsregeln (enligt definitionen i hello varningsregeln).</span><span class="sxs-lookup"><span data-stu-id="0eda4-309">hello description of hello metric alert rule (as defined in hello alert rule).</span></span> |
| <span data-ttu-id="0eda4-310">Egenskaper. Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="0eda4-310">properties.Threshold</span></span> | <span data-ttu-id="0eda4-311">hello tröskelvärdet används i hello utvärdering av hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-311">hello threshold value used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-312">Egenskaper. WindowSizeInMinutes</span><span class="sxs-lookup"><span data-stu-id="0eda4-312">properties.WindowSizeInMinutes</span></span> | <span data-ttu-id="0eda4-313">hello fönsterstorlek används i hello utvärdering av hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-313">hello window size used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-314">Egenskaper. Aggregeringen</span><span class="sxs-lookup"><span data-stu-id="0eda4-314">properties.Aggregation</span></span> | <span data-ttu-id="0eda4-315">hello Aggregeringstyp har definierats i hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-315">hello aggregation type defined in hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-316">Egenskaper. Operatorn</span><span class="sxs-lookup"><span data-stu-id="0eda4-316">properties.Operator</span></span> | <span data-ttu-id="0eda4-317">hello villkorsstyrd operatör används i hello utvärdering av hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-317">hello conditional operator used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-318">Egenskaper. MetricName</span><span class="sxs-lookup"><span data-stu-id="0eda4-318">properties.MetricName</span></span> | <span data-ttu-id="0eda4-319">hello måttnamnet av hello mått används i hello utvärdering av hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-319">hello metric name of hello metric used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="0eda4-320">Egenskaper. MetricUnit</span><span class="sxs-lookup"><span data-stu-id="0eda4-320">properties.MetricUnit</span></span> | <span data-ttu-id="0eda4-321">hello mått enhet för hello måttet används i hello utvärdering av hello mått varningsregel.</span><span class="sxs-lookup"><span data-stu-id="0eda4-321">hello metric unit for hello metric used in hello evaluation of hello metric alert rule.</span></span> |

## <a name="autoscale"></a><span data-ttu-id="0eda4-322">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="0eda4-322">Autoscale</span></span>
<span data-ttu-id="0eda4-323">Den här kategorin innehåller hello post för varje händelser relaterade toohello användning av hello Autoskala motorn baserat på automatiska inställningar du har definierat i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0eda4-323">This category contains hello record of any events related toohello operation of hello autoscale engine based on any autoscale settings you have defined in your subscription.</span></span> <span data-ttu-id="0eda4-324">Ett exempel på hello typen av händelse visas i den här kategorin är ”Autoskala skalas upp misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="0eda4-324">An example of hello type of event you would see in this category is "Autoscale scale up action failed."</span></span> <span data-ttu-id="0eda4-325">Använda autoskalning kan du automatiskt skala ut eller skala i hello antalet instanser i en stöds resurstyp baserat på tid på dagen och/eller belastningen (mått) data med hjälp av en autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="0eda4-325">Using autoscale, you can automatically scale out or scale in hello number of instances in a supported resource type based on time of day and/or load (metric) data using an autoscale setting.</span></span> <span data-ttu-id="0eda4-326">När hello villkor är uppfyllda tooscale uppåt eller nedåt hello start och lyckades eller registreras misslyckade händelser i den här kategorin.</span><span class="sxs-lookup"><span data-stu-id="0eda4-326">When hello conditions are met tooscale up or down, hello start and succeeded or failed events will be recorded in this category.</span></span>

### <a name="sample-event"></a><span data-ttu-id="0eda4-327">Exempelhändelse</span><span class="sxs-lookup"><span data-stu-id="0eda4-327">Sample event</span></span>
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
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
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
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

### <a name="property-descriptions"></a><span data-ttu-id="0eda4-328">Egenskapsbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="0eda4-328">Property descriptions</span></span>
| <span data-ttu-id="0eda4-329">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="0eda4-329">Element Name</span></span> | <span data-ttu-id="0eda4-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-330">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0eda4-331">Anroparen</span><span class="sxs-lookup"><span data-stu-id="0eda4-331">caller</span></span> | <span data-ttu-id="0eda4-332">Alltid Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="0eda4-332">Always Microsoft.Insights/autoscaleSettings</span></span> |
| <span data-ttu-id="0eda4-333">kanaler</span><span class="sxs-lookup"><span data-stu-id="0eda4-333">channels</span></span> | <span data-ttu-id="0eda4-334">Alltid ”Admin, åtgärden”</span><span class="sxs-lookup"><span data-stu-id="0eda4-334">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="0eda4-335">Anspråk</span><span class="sxs-lookup"><span data-stu-id="0eda4-335">claims</span></span> | <span data-ttu-id="0eda4-336">JSON-blob med hello SPN (service principal name) eller resurs typ av hello Autoskala motorn.</span><span class="sxs-lookup"><span data-stu-id="0eda4-336">JSON blob with hello SPN (service principal name), or resource type, of hello autoscale engine.</span></span> |
| <span data-ttu-id="0eda4-337">correlationId</span><span class="sxs-lookup"><span data-stu-id="0eda4-337">correlationId</span></span> | <span data-ttu-id="0eda4-338">Ett GUID i hello-strängformat.</span><span class="sxs-lookup"><span data-stu-id="0eda4-338">A GUID in hello string format.</span></span> |
| <span data-ttu-id="0eda4-339">description</span><span class="sxs-lookup"><span data-stu-id="0eda4-339">description</span></span> |<span data-ttu-id="0eda4-340">Statisk textbeskrivning av hello Autoskala händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-340">Static text description of hello autoscale event.</span></span> |
| <span data-ttu-id="0eda4-341">eventDataId</span><span class="sxs-lookup"><span data-stu-id="0eda4-341">eventDataId</span></span> |<span data-ttu-id="0eda4-342">Unik identifierare för hello Autoskala händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-342">Unique identifier of hello autoscale event.</span></span> |
| <span data-ttu-id="0eda4-343">nivå</span><span class="sxs-lookup"><span data-stu-id="0eda4-343">level</span></span> |<span data-ttu-id="0eda4-344">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-344">Level of hello event.</span></span> <span data-ttu-id="0eda4-345">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="0eda4-345">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="0eda4-346">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="0eda4-346">resourceGroupName</span></span> |<span data-ttu-id="0eda4-347">Namnet på resursgruppen hello för hello autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="0eda4-347">Name of hello resource group for hello autoscale setting.</span></span> |
| <span data-ttu-id="0eda4-348">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="0eda4-348">resourceProviderName</span></span> |<span data-ttu-id="0eda4-349">Namnet på hello resource provider för hello autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="0eda4-349">Name of hello resource provider for hello autoscale setting.</span></span> |
| <span data-ttu-id="0eda4-350">resourceId</span><span class="sxs-lookup"><span data-stu-id="0eda4-350">resourceId</span></span> |<span data-ttu-id="0eda4-351">Resurs-id för hello autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="0eda4-351">Resource id of hello autoscale setting.</span></span> |
| <span data-ttu-id="0eda4-352">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="0eda4-352">operationId</span></span> |<span data-ttu-id="0eda4-353">Ett GUID som delas med hello händelser som motsvarar tooa enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-353">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="0eda4-354">operationName</span><span class="sxs-lookup"><span data-stu-id="0eda4-354">operationName</span></span> |<span data-ttu-id="0eda4-355">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="0eda4-355">Name of hello operation.</span></span> |
| <span data-ttu-id="0eda4-356">properties</span><span class="sxs-lookup"><span data-stu-id="0eda4-356">properties</span></span> |<span data-ttu-id="0eda4-357">En uppsättning `<Key, Value>` par (det vill säga en ordlista) som beskriver hello information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-357">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="0eda4-358">Egenskaper. Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eda4-358">properties.Description</span></span> | <span data-ttu-id="0eda4-359">Detaljerad beskrivning av vad hello Autoskala motorn utfördes.</span><span class="sxs-lookup"><span data-stu-id="0eda4-359">Detailed description of what hello autoscale engine was doing.</span></span> |
| <span data-ttu-id="0eda4-360">Egenskaper. ResourceName</span><span class="sxs-lookup"><span data-stu-id="0eda4-360">properties.ResourceName</span></span> | <span data-ttu-id="0eda4-361">Resurs-ID för hello påverkas resurs (hello resurs på vilka hello skalningsåtgärd utfördes)</span><span class="sxs-lookup"><span data-stu-id="0eda4-361">Resource ID of hello impacted resource (hello resource on which hello scale action was being performed)</span></span> |
| <span data-ttu-id="0eda4-362">Egenskaper. OldInstancesCount</span><span class="sxs-lookup"><span data-stu-id="0eda4-362">properties.OldInstancesCount</span></span> | <span data-ttu-id="0eda4-363">hello antal instanser före hello Autoskala åtgärden tog effekt.</span><span class="sxs-lookup"><span data-stu-id="0eda4-363">hello number of instances before hello autoscale action took effect.</span></span> |
| <span data-ttu-id="0eda4-364">Egenskaper. NewInstancesCount</span><span class="sxs-lookup"><span data-stu-id="0eda4-364">properties.NewInstancesCount</span></span> | <span data-ttu-id="0eda4-365">hello antalet instanser när hello Autoskala åtgärden tog effekt.</span><span class="sxs-lookup"><span data-stu-id="0eda4-365">hello number of instances after hello autoscale action took effect.</span></span> |
| <span data-ttu-id="0eda4-366">Egenskaper. LastScaleActionTime</span><span class="sxs-lookup"><span data-stu-id="0eda4-366">properties.LastScaleActionTime</span></span> | <span data-ttu-id="0eda4-367">hello tidsstämpel när hello Autoskala åtgärd utfördes.</span><span class="sxs-lookup"><span data-stu-id="0eda4-367">hello timestamp of when hello autoscale action occurred.</span></span> |
| <span data-ttu-id="0eda4-368">status</span><span class="sxs-lookup"><span data-stu-id="0eda4-368">status</span></span> |<span data-ttu-id="0eda4-369">Sträng som beskriver hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0eda4-369">String describing hello status of hello operation.</span></span> <span data-ttu-id="0eda4-370">Vissa vanliga värden: startas i pågår, slutfört, misslyckades, aktiv, löst.</span><span class="sxs-lookup"><span data-stu-id="0eda4-370">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="0eda4-371">subStatus</span><span class="sxs-lookup"><span data-stu-id="0eda4-371">subStatus</span></span> | <span data-ttu-id="0eda4-372">Vanligtvis är null för Autoskala.</span><span class="sxs-lookup"><span data-stu-id="0eda4-372">Usually null for autoscale.</span></span> |
| <span data-ttu-id="0eda4-373">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-373">eventTimestamp</span></span> |<span data-ttu-id="0eda4-374">Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="0eda4-374">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="0eda4-375">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="0eda4-375">submissionTimestamp</span></span> |<span data-ttu-id="0eda4-376">Tidsstämpel när hello händelse blev tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="0eda4-376">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="0eda4-377">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="0eda4-377">subscriptionId</span></span> |<span data-ttu-id="0eda4-378">Azure prenumerations-Id.</span><span class="sxs-lookup"><span data-stu-id="0eda4-378">Azure Subscription Id.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0eda4-379">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0eda4-379">Next steps</span></span>
* [<span data-ttu-id="0eda4-380">Mer information om hello aktivitetsloggen (tidigare granskningsloggar)</span><span class="sxs-lookup"><span data-stu-id="0eda4-380">Learn more about hello Activity Log (formerly Audit Logs)</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="0eda4-381">Strömma hello Azure-aktivitetsloggen tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="0eda4-381">Stream hello Azure Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
