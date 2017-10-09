---
title: "aaaCall en webhook på Azure-aktivitetsloggen aviseringar | Microsoft Docs"
description: "Vidarebefordra aktivitet logga händelser tooother Server för anpassade åtgärder. Till exempel skicka SMS, logga programfel eller meddela ett team via chatt/meddelandetjänst."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a><span data-ttu-id="b8245-104">Anropa en webhook i Azure-aktivitetsloggen aviseringar</span><span class="sxs-lookup"><span data-stu-id="b8245-104">Call a webhook on Azure Activity Log alerts</span></span>
<span data-ttu-id="b8245-105">Webhooks Tillåt tooroute en Azure Varna tooother meddelandesystem för efterbearbetning eller anpassade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b8245-105">Webhooks allow you tooroute an Azure alert notification tooother systems for post-processing or custom actions.</span></span> <span data-ttu-id="b8245-106">Du kan använda en webhook på en avisering tooroute den tooservices som skickar SMS, logga programfel, meddela ett team via chatt/meddelandetjänster eller göra en mängd olika andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b8245-106">You can use a webhook on an alert tooroute it tooservices that send SMS, log bugs, notify a team via chat/messaging services, or do any number of other actions.</span></span> <span data-ttu-id="b8245-107">Den här artikeln beskriver hur tooset en webhook toobe anropas när en varning utlöses Azure-aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="b8245-107">This article describes how tooset a webhook toobe called when an Azure Activity Log alert fires.</span></span> <span data-ttu-id="b8245-108">Här visas också vilken hello nyttolasten för hello HTTP POST tooa webhook verkar vara.</span><span class="sxs-lookup"><span data-stu-id="b8245-108">It also shows what hello payload for hello HTTP POST tooa webhook looks like.</span></span> <span data-ttu-id="b8245-109">Information om hello installationen och schemat för en Azure mått avisering [se den här sidan i stället](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b8245-109">For information on hello setup and schema for an Azure metric alert, [see this page instead](insights-webhooks-alerts.md).</span></span> <span data-ttu-id="b8245-110">Du kan också konfigurera ett e-postmeddelande med aktivitetsloggen avisering toosend när aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b8245-110">You can also set up an Activity Log alert toosend email when activated.</span></span>

> [!NOTE]
> <span data-ttu-id="b8245-111">Den här funktionen är för närvarande under förhandsgranskning och kommer att tas bort på någon punkt i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="b8245-111">This feature is currently in preview and will be removed at some point in hello future.</span></span>
>
>

<span data-ttu-id="b8245-112">Du kan ställa in en aktivitetsloggen avisering med hello [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), [plattformsoberoende CLI](insights-cli-samples.md#work-with-alerts), eller [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8245-112">You can set up an Activity Log alert using hello [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-metric-alerts), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts), or [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span> <span data-ttu-id="b8245-113">För närvarande kan du ange en med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b8245-113">Currently, you cannot set one up using hello Azure portal.</span></span>

## <a name="authenticating-hello-webhook"></a><span data-ttu-id="b8245-114">Autentisera hello-webhook</span><span class="sxs-lookup"><span data-stu-id="b8245-114">Authenticating hello webhook</span></span>
<span data-ttu-id="b8245-115">Hej webhook kan autentisera med någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="b8245-115">hello webhook can authenticate using either of these methods:</span></span>

1. <span data-ttu-id="b8245-116">**Tokenbaserad auktorisering** -hello webhook URI sparas med ett token ID, t.ex.`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span><span class="sxs-lookup"><span data-stu-id="b8245-116">**Token-based authorization** - hello webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span></span>
2. <span data-ttu-id="b8245-117">**Grundläggande auktorisering** -hello webhook URI sparas med ett användarnamn och lösenord, t.ex.`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span><span class="sxs-lookup"><span data-stu-id="b8245-117">**Basic authorization** - hello webhook URI is saved with a username and password, for example, `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span></span>

## <a name="payload-schema"></a><span data-ttu-id="b8245-118">Nyttolasten i schemat</span><span class="sxs-lookup"><span data-stu-id="b8245-118">Payload schema</span></span>
<span data-ttu-id="b8245-119">hello POST-åtgärden innehåller hello följande JSON-nyttolast och schemat för alla aktivitetsloggen-baserade aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b8245-119">hello POST operation contains hello following JSON payload and schema for all Activity Log-based alerts.</span></span> <span data-ttu-id="b8245-120">Det här schemat är liknande toohello en används av måttet-baserade aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b8245-120">This schema is similar toohello one used by metric-based alerts.</span></span>

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| <span data-ttu-id="b8245-121">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="b8245-121">Element Name</span></span> | <span data-ttu-id="b8245-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b8245-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b8245-123">status</span><span class="sxs-lookup"><span data-stu-id="b8245-123">status</span></span> |<span data-ttu-id="b8245-124">Används för mått aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b8245-124">Used for metric alerts.</span></span> <span data-ttu-id="b8245-125">Alltid inställt för ”aktiverad” för aktivitetsloggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b8245-125">Always set too"activated" for Activity Log alerts.</span></span> |
| <span data-ttu-id="b8245-126">Kontexten</span><span class="sxs-lookup"><span data-stu-id="b8245-126">context</span></span> |<span data-ttu-id="b8245-127">Kontexten för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-127">Context of hello event.</span></span> |
| <span data-ttu-id="b8245-128">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="b8245-128">resourceProviderName</span></span> |<span data-ttu-id="b8245-129">Hej resursprovidern av hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="b8245-129">hello resource provider of hello impacted resource.</span></span> |
| <span data-ttu-id="b8245-130">conditionType</span><span class="sxs-lookup"><span data-stu-id="b8245-130">conditionType</span></span> |<span data-ttu-id="b8245-131">Alltid ”Event”.</span><span class="sxs-lookup"><span data-stu-id="b8245-131">Always "Event."</span></span> |
| <span data-ttu-id="b8245-132">namn</span><span class="sxs-lookup"><span data-stu-id="b8245-132">name</span></span> |<span data-ttu-id="b8245-133">Namnet på hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="b8245-133">Name of hello alert rule.</span></span> |
| <span data-ttu-id="b8245-134">id</span><span class="sxs-lookup"><span data-stu-id="b8245-134">id</span></span> |<span data-ttu-id="b8245-135">Resurs-ID för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="b8245-135">Resource ID of hello alert.</span></span> |
| <span data-ttu-id="b8245-136">description</span><span class="sxs-lookup"><span data-stu-id="b8245-136">description</span></span> |<span data-ttu-id="b8245-137">Aviseringsbeskrivningen som angetts under skapandet av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="b8245-137">Alert description as set during creation of hello alert.</span></span> |
| <span data-ttu-id="b8245-138">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="b8245-138">subscriptionId</span></span> |<span data-ttu-id="b8245-139">Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="b8245-139">Azure Subscription ID.</span></span> |
| <span data-ttu-id="b8245-140">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="b8245-140">timestamp</span></span> |<span data-ttu-id="b8245-141">Tiden på vilka hello händelsen skapades av hello Azure-tjänst som bearbetade hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="b8245-141">Time at which hello event was generated by hello Azure service that processed hello request.</span></span> |
| <span data-ttu-id="b8245-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="b8245-142">resourceId</span></span> |<span data-ttu-id="b8245-143">Resurs-ID för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="b8245-143">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="b8245-144">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="b8245-144">resourceGroupName</span></span> |<span data-ttu-id="b8245-145">Namnet på resursgruppen hello för hello påverkas resurs</span><span class="sxs-lookup"><span data-stu-id="b8245-145">Name of hello resource group for hello impacted resource</span></span> |
| <span data-ttu-id="b8245-146">properties</span><span class="sxs-lookup"><span data-stu-id="b8245-146">properties</span></span> |<span data-ttu-id="b8245-147">En uppsättning `<Key, Value>` par (d.v.s. `Dictionary<String, String>`) som innehåller information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-147">Set of `<Key, Value>` pairs (i.e. `Dictionary<String, String>`) that includes details about hello event.</span></span> |
| <span data-ttu-id="b8245-148">Händelse</span><span class="sxs-lookup"><span data-stu-id="b8245-148">event</span></span> |<span data-ttu-id="b8245-149">Element som innehåller metadata om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-149">Element containing metadata about hello event.</span></span> |
| <span data-ttu-id="b8245-150">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="b8245-150">authorization</span></span> |<span data-ttu-id="b8245-151">Hej RBAC egenskaper för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-151">hello RBAC properties of hello event.</span></span> <span data-ttu-id="b8245-152">Dessa omfattar vanligtvis hello ”åtgärd” och ”roll” hello ”omfattningen”.</span><span class="sxs-lookup"><span data-stu-id="b8245-152">These usually include hello “action”, “role” and hello “scope.”</span></span> |
| <span data-ttu-id="b8245-153">category</span><span class="sxs-lookup"><span data-stu-id="b8245-153">category</span></span> |<span data-ttu-id="b8245-154">Kategori hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-154">Category of hello event.</span></span> <span data-ttu-id="b8245-155">Värden som stöds omfattar: administrativa, varning, säkerhet, ServiceHealth och rekommendation.</span><span class="sxs-lookup"><span data-stu-id="b8245-155">Supported values include: Administrative, Alert, Security, ServiceHealth, Recommendation.</span></span> |
| <span data-ttu-id="b8245-156">Anroparen</span><span class="sxs-lookup"><span data-stu-id="b8245-156">caller</span></span> |<span data-ttu-id="b8245-157">E-postadressen hello användaren som utförde hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="b8245-157">Email address of hello user who performed hello operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="b8245-158">Kan vara null för vissa system-anrop.</span><span class="sxs-lookup"><span data-stu-id="b8245-158">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="b8245-159">correlationId</span><span class="sxs-lookup"><span data-stu-id="b8245-159">correlationId</span></span> |<span data-ttu-id="b8245-160">Vanligtvis ett GUID i strängformat.</span><span class="sxs-lookup"><span data-stu-id="b8245-160">Usually a GUID in string format.</span></span> <span data-ttu-id="b8245-161">Händelser med correlationId tillhör toohello samma större åtgärd och dela vanligtvis en correlationId.</span><span class="sxs-lookup"><span data-stu-id="b8245-161">Events with correlationId belong toohello same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="b8245-162">eventDescription</span><span class="sxs-lookup"><span data-stu-id="b8245-162">eventDescription</span></span> |<span data-ttu-id="b8245-163">Statisk textbeskrivning av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-163">Static text description of hello event.</span></span> |
| <span data-ttu-id="b8245-164">eventDataId</span><span class="sxs-lookup"><span data-stu-id="b8245-164">eventDataId</span></span> |<span data-ttu-id="b8245-165">Unik identifierare för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-165">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="b8245-166">eventSource</span><span class="sxs-lookup"><span data-stu-id="b8245-166">eventSource</span></span> |<span data-ttu-id="b8245-167">Namnet på hello Azure-tjänst eller infrastruktur som genererade hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-167">Name of hello Azure service or infrastructure that generated hello event.</span></span> |
| <span data-ttu-id="b8245-168">httpRequest</span><span class="sxs-lookup"><span data-stu-id="b8245-168">httpRequest</span></span> |<span data-ttu-id="b8245-169">Inkluderar vanligtvis hello ”clientRequestId”, ”clientIpAddress” och ”metod” (HTTP-metoden anger t.ex.).</span><span class="sxs-lookup"><span data-stu-id="b8245-169">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method e.g. PUT).</span></span> |
| <span data-ttu-id="b8245-170">nivå</span><span class="sxs-lookup"><span data-stu-id="b8245-170">level</span></span> |<span data-ttu-id="b8245-171">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”.</span><span class="sxs-lookup"><span data-stu-id="b8245-171">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose.”</span></span> |
| <span data-ttu-id="b8245-172">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="b8245-172">operationId</span></span> |<span data-ttu-id="b8245-173">Vanligtvis ett GUID som delas med hello händelser motsvarande toosingle igen.</span><span class="sxs-lookup"><span data-stu-id="b8245-173">Usually a GUID shared among hello events corresponding toosingle operation.</span></span> |
| <span data-ttu-id="b8245-174">operationName</span><span class="sxs-lookup"><span data-stu-id="b8245-174">operationName</span></span> |<span data-ttu-id="b8245-175">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="b8245-175">Name of hello operation.</span></span> |
| <span data-ttu-id="b8245-176">properties</span><span class="sxs-lookup"><span data-stu-id="b8245-176">properties</span></span> |<span data-ttu-id="b8245-177">Egenskaper för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="b8245-177">Properties of hello event.</span></span> |
| <span data-ttu-id="b8245-178">status</span><span class="sxs-lookup"><span data-stu-id="b8245-178">status</span></span> |<span data-ttu-id="b8245-179">Sträng.</span><span class="sxs-lookup"><span data-stu-id="b8245-179">String.</span></span> <span data-ttu-id="b8245-180">Status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b8245-180">Status of hello operation.</span></span> <span data-ttu-id="b8245-181">Vanliga värden är: ”starta”, ”pågående”, ”Succeeded”, ”misslyckades”, ”aktiv”, ”löst”.</span><span class="sxs-lookup"><span data-stu-id="b8245-181">Common values include: "Started", "In Progress", "Succeeded", "Failed", "Active", "Resolved".</span></span> |
| <span data-ttu-id="b8245-182">subStatus</span><span class="sxs-lookup"><span data-stu-id="b8245-182">subStatus</span></span> |<span data-ttu-id="b8245-183">Inkluderar vanligtvis hello HTTP-statuskoden hello motsvarande REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="b8245-183">Usually includes hello HTTP status code of hello corresponding REST call.</span></span> <span data-ttu-id="b8245-184">Det kan även innehålla andra strängar som beskriver en sådan.</span><span class="sxs-lookup"><span data-stu-id="b8245-184">It might also include other strings describing a substatus.</span></span> <span data-ttu-id="b8245-185">Vanliga understatus värden är: OK (HTTP-statuskod: 200), skapade (HTTP-statuskod: 201), godkända (HTTP-statuskod: 202), inte innehåll (HTTP-statuskod: 204), felaktig begäran (HTTP-statuskod: 400), inte att hitta (HTTP-statuskod: 404), konflikt (HTTP-statuskod: 409), internt serverfel (HTTP-statuskod: 500), tjänsten är inte tillgänglig (HTTP-statuskod: 503), Gateway-Timeout (HTTP-statuskod: 504)</span><span class="sxs-lookup"><span data-stu-id="b8245-185">Common substatus values include: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b8245-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8245-186">Next steps</span></span>
* [<span data-ttu-id="b8245-187">Mer information om hello aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="b8245-187">Learn more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="b8245-188">Köra Azure Automation-skript (Runbooks) på Azure-aviseringar</span><span class="sxs-lookup"><span data-stu-id="b8245-188">Execute Azure Automation scripts (Runbooks) on Azure alerts</span></span>](http://go.microsoft.com/fwlink/?LinkId=627081)
* <span data-ttu-id="b8245-189">[Använd Logikapp toosend ett SMS via Twilio från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="b8245-189">[Use Logic App toosend an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="b8245-190">Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.</span><span class="sxs-lookup"><span data-stu-id="b8245-190">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="b8245-191">[Använd Logikapp toosend ett Slack meddelande från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="b8245-191">[Use Logic App toosend a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="b8245-192">Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.</span><span class="sxs-lookup"><span data-stu-id="b8245-192">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="b8245-193">[Använd Logikapp toosend meddelandet tooan Azure Queue från en Azure avisering](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="b8245-193">[Use Logic App toosend a message tooan Azure Queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="b8245-194">Det här exemplet är för mått aviseringar, men kan vara ändrade toowork med en aktivitetsloggen avisering.</span><span class="sxs-lookup"><span data-stu-id="b8245-194">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
