---
title: "Förstå webhook-schemat som används i aktiviteten loggen aviseringar | Microsoft Docs"
description: "Läs mer om schemat för JSON som skickas till en Webhooksadressen när en aktivitet loggen avisering aktiveras."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a><span data-ttu-id="07a9c-103">Webhooks för Azure aktiviteten Logga varningar</span><span class="sxs-lookup"><span data-stu-id="07a9c-103">Webhooks for Azure activity log alerts</span></span>
<span data-ttu-id="07a9c-104">Som en del av definitionen för en grupp, kan du konfigurera webhook slutpunkter för att ta emot aviseringar för aktiviteten loggen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-104">As part of the definition of an action group, you can configure webhook endpoints to receive activity log alert notifications.</span></span> <span data-ttu-id="07a9c-105">Du kan använda webhooks, för att vidarebefordra meddelandena till andra system för efterbearbetning eller anpassade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="07a9c-105">With webhooks, you can route these notifications to other systems for post-processing or custom actions.</span></span> <span data-ttu-id="07a9c-106">Den här artikeln visar hur nyttolasten för HTTP POST till en webhook ser ut.</span><span class="sxs-lookup"><span data-stu-id="07a9c-106">This article shows what the payload for the HTTP POST to a webhook looks like.</span></span>

<span data-ttu-id="07a9c-107">Mer information om aktiviteten loggen aviseringar finns så [skapa Azure aktivitet Logga varningar](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="07a9c-107">For more information on activity log alerts, see how to [create Azure activity log alerts](monitoring-activity-log-alerts.md).</span></span>

<span data-ttu-id="07a9c-108">Mer information om åtgärdsgrupper finns så [skapa åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="07a9c-108">For information on action groups, see how to [create action groups](monitoring-action-groups.md).</span></span>

## <a name="authenticate-the-webhook"></a><span data-ttu-id="07a9c-109">Autentisera webhooken</span><span class="sxs-lookup"><span data-stu-id="07a9c-109">Authenticate the webhook</span></span>
<span data-ttu-id="07a9c-110">Webhooken kan du använda tokenbaserad auktorisering för autentisering.</span><span class="sxs-lookup"><span data-stu-id="07a9c-110">The webhook can optionally use token-based authorization for authentication.</span></span> <span data-ttu-id="07a9c-111">Webhooken URI sparas med ett token ID, till exempel `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span><span class="sxs-lookup"><span data-stu-id="07a9c-111">The webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span></span>

## <a name="payload-schema"></a><span data-ttu-id="07a9c-112">Nyttolasten i schemat</span><span class="sxs-lookup"><span data-stu-id="07a9c-112">Payload schema</span></span>
<span data-ttu-id="07a9c-113">JSON-nyttolast som ingår i POST-åtgärden skiljer sig baserat på den nyttolasten data.context.activityLog.eventSource fält.</span><span class="sxs-lookup"><span data-stu-id="07a9c-113">The JSON payload contained in the POST operation differs based on the payload's data.context.activityLog.eventSource field.</span></span>

###<a name="common"></a><span data-ttu-id="07a9c-114">Vanliga</span><span class="sxs-lookup"><span data-stu-id="07a9c-114">Common</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a><span data-ttu-id="07a9c-115">Administrativa</span><span class="sxs-lookup"><span data-stu-id="07a9c-115">Administrative</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a><span data-ttu-id="07a9c-116">ServiceHealth</span><span class="sxs-lookup"><span data-stu-id="07a9c-116">ServiceHealth</span></span>
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

<span data-ttu-id="07a9c-117">Specifika schemainformation om tjänsten notification aktivitet loggen hälsovarningar, finns [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="07a9c-117">For specific schema details on service health notification activity log alerts, see [Service health notifications](monitoring-service-notifications.md).</span></span>

<span data-ttu-id="07a9c-118">Information om specifika schemat på alla andra aktiviteten loggen aviseringar finns [översikt över Azure aktivitetsloggen](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="07a9c-118">For specific schema details on all other activity log alerts, see [Overview of the Azure activity log](monitoring-overview-activity-logs.md).</span></span>

| <span data-ttu-id="07a9c-119">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="07a9c-119">Element name</span></span> | <span data-ttu-id="07a9c-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="07a9c-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="07a9c-121">status</span><span class="sxs-lookup"><span data-stu-id="07a9c-121">status</span></span> |<span data-ttu-id="07a9c-122">Används för mått aviseringar.</span><span class="sxs-lookup"><span data-stu-id="07a9c-122">Used for metric alerts.</span></span> <span data-ttu-id="07a9c-123">Alltid inställt på ”aktiverad” för aktiviteten loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="07a9c-123">Always set to "activated" for activity log alerts.</span></span> |
| <span data-ttu-id="07a9c-124">Kontexten</span><span class="sxs-lookup"><span data-stu-id="07a9c-124">context</span></span> |<span data-ttu-id="07a9c-125">Kontexten för händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-125">Context of the event.</span></span> |
| <span data-ttu-id="07a9c-126">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="07a9c-126">resourceProviderName</span></span> |<span data-ttu-id="07a9c-127">Resursprovidern för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="07a9c-127">The resource provider of the impacted resource.</span></span> |
| <span data-ttu-id="07a9c-128">conditionType</span><span class="sxs-lookup"><span data-stu-id="07a9c-128">conditionType</span></span> |<span data-ttu-id="07a9c-129">Alltid ”Event”.</span><span class="sxs-lookup"><span data-stu-id="07a9c-129">Always "Event."</span></span> |
| <span data-ttu-id="07a9c-130">namn</span><span class="sxs-lookup"><span data-stu-id="07a9c-130">name</span></span> |<span data-ttu-id="07a9c-131">Namnet på regeln.</span><span class="sxs-lookup"><span data-stu-id="07a9c-131">Name of the alert rule.</span></span> |
| <span data-ttu-id="07a9c-132">id</span><span class="sxs-lookup"><span data-stu-id="07a9c-132">id</span></span> |<span data-ttu-id="07a9c-133">Resurs-ID för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-133">Resource ID of the alert.</span></span> |
| <span data-ttu-id="07a9c-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="07a9c-134">description</span></span> |<span data-ttu-id="07a9c-135">Aviseringsbeskrivningen när aviseringen har skapats.</span><span class="sxs-lookup"><span data-stu-id="07a9c-135">Alert description set when the alert is created.</span></span> |
| <span data-ttu-id="07a9c-136">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="07a9c-136">subscriptionId</span></span> |<span data-ttu-id="07a9c-137">Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="07a9c-137">Azure subscription ID.</span></span> |
| <span data-ttu-id="07a9c-138">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="07a9c-138">timestamp</span></span> |<span data-ttu-id="07a9c-139">Tid då händelsen skapades av Azure-tjänsten som bearbetade förfrågan.</span><span class="sxs-lookup"><span data-stu-id="07a9c-139">Time at which the event was generated by the Azure service that processed the request.</span></span> |
| <span data-ttu-id="07a9c-140">resourceId</span><span class="sxs-lookup"><span data-stu-id="07a9c-140">resourceId</span></span> |<span data-ttu-id="07a9c-141">Resurs-ID för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="07a9c-141">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="07a9c-142">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="07a9c-142">resourceGroupName</span></span> |<span data-ttu-id="07a9c-143">Namnet på resursgruppen för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="07a9c-143">Name of the resource group for the impacted resource.</span></span> |
| <span data-ttu-id="07a9c-144">properties</span><span class="sxs-lookup"><span data-stu-id="07a9c-144">properties</span></span> |<span data-ttu-id="07a9c-145">En uppsättning `<Key, Value>` par (det vill säga `Dictionary<String, String>`) som innehåller information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-145">Set of `<Key, Value>` pairs (that is, `Dictionary<String, String>`) that includes details about the event.</span></span> |
| <span data-ttu-id="07a9c-146">Händelse</span><span class="sxs-lookup"><span data-stu-id="07a9c-146">event</span></span> |<span data-ttu-id="07a9c-147">Element som innehåller metadata om händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-147">Element that contains metadata about the event.</span></span> |
| <span data-ttu-id="07a9c-148">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="07a9c-148">authorization</span></span> |<span data-ttu-id="07a9c-149">Rollbaserad åtkomstkontroll egenskaperna för händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-149">The Role-Based Access Control properties of the event.</span></span> <span data-ttu-id="07a9c-150">Dessa egenskaper innehåller vanligtvis åtgärden rollen och omfång.</span><span class="sxs-lookup"><span data-stu-id="07a9c-150">These properties usually include the action, the role, and the scope.</span></span> |
| <span data-ttu-id="07a9c-151">category</span><span class="sxs-lookup"><span data-stu-id="07a9c-151">category</span></span> |<span data-ttu-id="07a9c-152">Kategori för händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-152">Category of the event.</span></span> <span data-ttu-id="07a9c-153">Värden som stöds omfattar administrativa, varning, säkerhet, ServiceHealth och rekommendation.</span><span class="sxs-lookup"><span data-stu-id="07a9c-153">Supported values include Administrative, Alert, Security, ServiceHealth, and Recommendation.</span></span> |
| <span data-ttu-id="07a9c-154">Anroparen</span><span class="sxs-lookup"><span data-stu-id="07a9c-154">caller</span></span> |<span data-ttu-id="07a9c-155">E-postadressen för användaren som utförde åtgärden, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="07a9c-155">Email address of the user who performed the operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="07a9c-156">Kan vara null för vissa system-anrop.</span><span class="sxs-lookup"><span data-stu-id="07a9c-156">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="07a9c-157">correlationId</span><span class="sxs-lookup"><span data-stu-id="07a9c-157">correlationId</span></span> |<span data-ttu-id="07a9c-158">Vanligtvis ett GUID i strängformat.</span><span class="sxs-lookup"><span data-stu-id="07a9c-158">Usually a GUID in string format.</span></span> <span data-ttu-id="07a9c-159">Händelser med correlationId tillhör samma större åtgärd och vanligtvis delar en correlationId.</span><span class="sxs-lookup"><span data-stu-id="07a9c-159">Events with correlationId belong to the same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="07a9c-160">eventDescription</span><span class="sxs-lookup"><span data-stu-id="07a9c-160">eventDescription</span></span> |<span data-ttu-id="07a9c-161">Statisk textbeskrivning av händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-161">Static text description of the event.</span></span> |
| <span data-ttu-id="07a9c-162">eventDataId</span><span class="sxs-lookup"><span data-stu-id="07a9c-162">eventDataId</span></span> |<span data-ttu-id="07a9c-163">Unik identifierare för händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-163">Unique identifier for the event.</span></span> |
| <span data-ttu-id="07a9c-164">eventSource</span><span class="sxs-lookup"><span data-stu-id="07a9c-164">eventSource</span></span> |<span data-ttu-id="07a9c-165">Namnet på Azure-tjänsten eller infrastruktur som genererade händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-165">Name of the Azure service or infrastructure that generated the event.</span></span> |
| <span data-ttu-id="07a9c-166">httpRequest</span><span class="sxs-lookup"><span data-stu-id="07a9c-166">httpRequest</span></span> |<span data-ttu-id="07a9c-167">Begäran innehåller vanligtvis clientRequestId, clientIpAddress och HTTP-metoden (till exempel PLACERA).</span><span class="sxs-lookup"><span data-stu-id="07a9c-167">The request usually includes the clientRequestId, clientIpAddress, and HTTP method (for example, PUT).</span></span> |
| <span data-ttu-id="07a9c-168">nivå</span><span class="sxs-lookup"><span data-stu-id="07a9c-168">level</span></span> |<span data-ttu-id="07a9c-169">Ett av följande värden: kritisk, fel, varning, information och utförlig.</span><span class="sxs-lookup"><span data-stu-id="07a9c-169">One of the following values: Critical, Error, Warning, Informational, and Verbose.</span></span> |
| <span data-ttu-id="07a9c-170">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="07a9c-170">operationId</span></span> |<span data-ttu-id="07a9c-171">Vanligtvis ett GUID som delas mellan de händelser som motsvarar en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="07a9c-171">Usually a GUID shared among the events corresponding to single operation.</span></span> |
| <span data-ttu-id="07a9c-172">operationName</span><span class="sxs-lookup"><span data-stu-id="07a9c-172">operationName</span></span> |<span data-ttu-id="07a9c-173">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="07a9c-173">Name of the operation.</span></span> |
| <span data-ttu-id="07a9c-174">properties</span><span class="sxs-lookup"><span data-stu-id="07a9c-174">properties</span></span> |<span data-ttu-id="07a9c-175">Egenskaper för händelsen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-175">Properties of the event.</span></span> |
| <span data-ttu-id="07a9c-176">status</span><span class="sxs-lookup"><span data-stu-id="07a9c-176">status</span></span> |<span data-ttu-id="07a9c-177">Sträng.</span><span class="sxs-lookup"><span data-stu-id="07a9c-177">String.</span></span> <span data-ttu-id="07a9c-178">Status för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="07a9c-178">Status of the operation.</span></span> <span data-ttu-id="07a9c-179">Vanliga värden är igång, pågår, slutfört, misslyckades, aktiv och löst.</span><span class="sxs-lookup"><span data-stu-id="07a9c-179">Common values include Started, In Progress, Succeeded, Failed, Active, and Resolved.</span></span> |
| <span data-ttu-id="07a9c-180">subStatus</span><span class="sxs-lookup"><span data-stu-id="07a9c-180">subStatus</span></span> |<span data-ttu-id="07a9c-181">Normalt innehåller HTTP-statuskod för motsvarande REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="07a9c-181">Usually includes the HTTP status code of the corresponding REST call.</span></span> <span data-ttu-id="07a9c-182">Det kan även innehålla andra strängar som beskriver en sådan.</span><span class="sxs-lookup"><span data-stu-id="07a9c-182">It might also include other strings that describe a substatus.</span></span> <span data-ttu-id="07a9c-183">Vanliga understatus värden är OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), inte innehåll (HTTP-statuskod: 204), felaktig begäran (HTTP-statuskod: 400), det gick inte att hitta (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409 ), Internt serverfel (HTTP-statuskod: 500), tjänsten inte tillgänglig (HTTP-statuskod: 503), och Gateway-Timeout (HTTP-statuskod: 504).</span><span class="sxs-lookup"><span data-stu-id="07a9c-183">Common substatus values include OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), and Gateway Timeout (HTTP Status Code: 504).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="07a9c-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07a9c-184">Next steps</span></span>
* <span data-ttu-id="07a9c-185">[Mer information om aktivitetsloggen](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="07a9c-185">[Learn more about the activity log](monitoring-overview-activity-logs.md).</span></span>
* <span data-ttu-id="07a9c-186">[Köra Azure automation-skript (Runbooks) på Azure-aviseringar](http://go.microsoft.com/fwlink/?LinkId=627081).</span><span class="sxs-lookup"><span data-stu-id="07a9c-186">[Execute Azure automation scripts (Runbooks) on Azure alerts](http://go.microsoft.com/fwlink/?LinkId=627081).</span></span>
* <span data-ttu-id="07a9c-187">[Använd en logikapp för att skicka ett SMS via Twilio från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="07a9c-187">[Use a logic app to send an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="07a9c-188">Det här exemplet är för mått aviseringar, men den kan ändras för att fungera med en varning för loggen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-188">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="07a9c-189">[Använd en logikapp för att skicka en Slack-meddelande från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="07a9c-189">[Use a logic app to send a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="07a9c-190">Det här exemplet är för mått aviseringar, men den kan ändras för att fungera med en varning för loggen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-190">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="07a9c-191">[Använd en logikapp för att skicka ett meddelande till en Azure-kö i en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="07a9c-191">[Use a logic app to send a message to an Azure queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="07a9c-192">Det här exemplet är för mått aviseringar, men den kan ändras för att fungera med en varning för loggen.</span><span class="sxs-lookup"><span data-stu-id="07a9c-192">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
