---
title: "aaaUse Autoskala åtgärder toosend e-post och webhook varningsmeddelanden. | Microsoft Docs"
description: "Se hur toouse Autoskala åtgärder toocall Webb-URL: er eller skicka e-postmeddelanden i Azure-Monitor. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: f611a18f5a808412fbdd0c89e3addb36437064c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a><span data-ttu-id="d9353-104">Använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="d9353-104">Use autoscale actions toosend email and webhook alert notifications in Azure Monitor</span></span>
<span data-ttu-id="d9353-105">Den här artikeln lär du hur du konfigurerar utlösare så att du kan anropa specifika webbadresser eller skicka e-postmeddelanden baserat på Autoskala åtgärder i Azure.</span><span class="sxs-lookup"><span data-stu-id="d9353-105">This article shows you how set up triggers so that you can call specific web URLs or send emails based on autoscale actions in Azure.</span></span>  

## <a name="webhooks"></a><span data-ttu-id="d9353-106">Webhooks</span><span class="sxs-lookup"><span data-stu-id="d9353-106">Webhooks</span></span>
<span data-ttu-id="d9353-107">Webhooks Tillåt tooroute hello Azure varningsmeddelanden tooother system för efterbearbetning eller anpassade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d9353-107">Webhooks allow you tooroute hello Azure alert notifications tooother systems for post-processing or custom notifications.</span></span> <span data-ttu-id="d9353-108">Till exempel routning hello avisering tooservices som kan hantera en inkommande web begäran toosend SMS, log buggar, meddela ett team med hjälp av chatt eller tjänster, etc. hello webhook URI måste vara en giltig HTTP eller HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d9353-108">For example, routing hello alert tooservices that can handle an incoming web request toosend SMS, log bugs, notify a team using chat or messaging services, etc. hello webhook URI must be a valid HTTP or HTTPS endpoint.</span></span>

## <a name="email"></a><span data-ttu-id="d9353-109">E-post</span><span class="sxs-lookup"><span data-stu-id="d9353-109">Email</span></span>
<span data-ttu-id="d9353-110">E-post kan skickas tooany giltig e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d9353-110">Email can be sent tooany valid email address.</span></span> <span data-ttu-id="d9353-111">Administratörer och medadministratörer för hello prenumeration där hello regeln körs meddelas också.</span><span class="sxs-lookup"><span data-stu-id="d9353-111">Administrators and co-administrators of hello subscription where hello rule is running will also be notified.</span></span>

## <a name="cloud-services-and-web-apps"></a><span data-ttu-id="d9353-112">Molntjänster och Web Apps</span><span class="sxs-lookup"><span data-stu-id="d9353-112">Cloud Services and Web Apps</span></span>
<span data-ttu-id="d9353-113">Du kan anmäla sig från hello Azure-portalen för molntjänster och servergrupper (webbprogram).</span><span class="sxs-lookup"><span data-stu-id="d9353-113">You can opt-in from hello Azure portal for Cloud Services and Server Farms (Web Apps).</span></span>

* <span data-ttu-id="d9353-114">Välj hello **skala genom** mått.</span><span class="sxs-lookup"><span data-stu-id="d9353-114">Choose hello **scale by** metric.</span></span>

![skala med](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a><span data-ttu-id="d9353-116">Skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="d9353-116">Virtual Machine scale sets</span></span>
<span data-ttu-id="d9353-117">För nya virtuella datorer skapas med Resource Manager (skalningsuppsättningar i virtuella datorer), kan du konfigurera detta med hjälp av REST-API, Resource Manager-mallar, PowerShell och CLI.</span><span class="sxs-lookup"><span data-stu-id="d9353-117">For newer Virtual Machines created with Resource Manager (Virtual Machine scale sets), you can configure this using REST API, Resource Manager templates, PowerShell, and CLI.</span></span> <span data-ttu-id="d9353-118">Ett gränssnitt för portalen är inte tillgänglig ännu.</span><span class="sxs-lookup"><span data-stu-id="d9353-118">A portal interface is not yet available.</span></span>
<span data-ttu-id="d9353-119">När du använder hello REST API eller Resource Manager-mall, inkluderar du hello meddelanden element med hello följande alternativ.</span><span class="sxs-lookup"><span data-stu-id="d9353-119">When using hello REST API or Resource Manager template, include hello notifications element with hello following options.</span></span>

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| <span data-ttu-id="d9353-120">Fält</span><span class="sxs-lookup"><span data-stu-id="d9353-120">Field</span></span> | <span data-ttu-id="d9353-121">Obligatorisk?</span><span class="sxs-lookup"><span data-stu-id="d9353-121">Mandatory?</span></span> | <span data-ttu-id="d9353-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d9353-122">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9353-123">åtgärden</span><span class="sxs-lookup"><span data-stu-id="d9353-123">operation</span></span> |<span data-ttu-id="d9353-124">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-124">yes</span></span> |<span data-ttu-id="d9353-125">Värdet måste vara ”skala”</span><span class="sxs-lookup"><span data-stu-id="d9353-125">value must be "Scale"</span></span> |
| <span data-ttu-id="d9353-126">sendToSubscriptionAdministrator</span><span class="sxs-lookup"><span data-stu-id="d9353-126">sendToSubscriptionAdministrator</span></span> |<span data-ttu-id="d9353-127">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-127">yes</span></span> |<span data-ttu-id="d9353-128">Värdet måste vara ”sant” eller ”false”</span><span class="sxs-lookup"><span data-stu-id="d9353-128">value must be "true" or "false"</span></span> |
| <span data-ttu-id="d9353-129">sendToSubscriptionCoAdministrators</span><span class="sxs-lookup"><span data-stu-id="d9353-129">sendToSubscriptionCoAdministrators</span></span> |<span data-ttu-id="d9353-130">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-130">yes</span></span> |<span data-ttu-id="d9353-131">Värdet måste vara ”sant” eller ”false”</span><span class="sxs-lookup"><span data-stu-id="d9353-131">value must be "true" or "false"</span></span> |
| <span data-ttu-id="d9353-132">customEmails</span><span class="sxs-lookup"><span data-stu-id="d9353-132">customEmails</span></span> |<span data-ttu-id="d9353-133">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-133">yes</span></span> |<span data-ttu-id="d9353-134">Värdet kan vara null [] eller Strängmatrisen för e-post</span><span class="sxs-lookup"><span data-stu-id="d9353-134">value can be null [] or string array of emails</span></span> |
| <span data-ttu-id="d9353-135">Webhooks</span><span class="sxs-lookup"><span data-stu-id="d9353-135">webhooks</span></span> |<span data-ttu-id="d9353-136">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-136">yes</span></span> |<span data-ttu-id="d9353-137">Värdet kan vara null eller giltiga Uri</span><span class="sxs-lookup"><span data-stu-id="d9353-137">value can be null or valid Uri</span></span> |
| <span data-ttu-id="d9353-138">serviceUri</span><span class="sxs-lookup"><span data-stu-id="d9353-138">serviceUri</span></span> |<span data-ttu-id="d9353-139">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-139">yes</span></span> |<span data-ttu-id="d9353-140">en giltig https Uri</span><span class="sxs-lookup"><span data-stu-id="d9353-140">a valid https Uri</span></span> |
| <span data-ttu-id="d9353-141">properties</span><span class="sxs-lookup"><span data-stu-id="d9353-141">properties</span></span> |<span data-ttu-id="d9353-142">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-142">yes</span></span> |<span data-ttu-id="d9353-143">Värdet måste vara tom {} eller kan innehålla nyckel / värde-par</span><span class="sxs-lookup"><span data-stu-id="d9353-143">value must be empty {} or can contain key-value pairs</span></span> |

## <a name="authentication-in-webhooks"></a><span data-ttu-id="d9353-144">Autentisering i webhooks</span><span class="sxs-lookup"><span data-stu-id="d9353-144">Authentication in webhooks</span></span>
<span data-ttu-id="d9353-145">Hej webhook kan autentisera med tokenbaserad autentisering var du sparar hello webhook URI med ett token-ID som en frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="d9353-145">hello webhook can authenticate using token-based authentication, where you save hello webhook URI with a token ID as a query parameter.</span></span> <span data-ttu-id="d9353-146">Till exempel https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span><span class="sxs-lookup"><span data-stu-id="d9353-146">For example, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span></span>

## <a name="autoscale-notification-webhook-payload-schema"></a><span data-ttu-id="d9353-147">Autoskala meddelande webhook nyttolast schemat</span><span class="sxs-lookup"><span data-stu-id="d9353-147">Autoscale notification webhook payload schema</span></span>
<span data-ttu-id="d9353-148">När hello Autoskala avisering genereras ingår hello följande metadata i hello webhook nyttolast:</span><span class="sxs-lookup"><span data-stu-id="d9353-148">When hello autoscale notification is generated, hello following metadata is included in hello webhook payload:</span></span>

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' toocapacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| <span data-ttu-id="d9353-149">Fält</span><span class="sxs-lookup"><span data-stu-id="d9353-149">Field</span></span> | <span data-ttu-id="d9353-150">Obligatorisk?</span><span class="sxs-lookup"><span data-stu-id="d9353-150">Mandatory?</span></span> | <span data-ttu-id="d9353-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d9353-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9353-152">status</span><span class="sxs-lookup"><span data-stu-id="d9353-152">status</span></span> |<span data-ttu-id="d9353-153">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-153">yes</span></span> |<span data-ttu-id="d9353-154">hello status som anger att en Autoskala åtgärd har skapats</span><span class="sxs-lookup"><span data-stu-id="d9353-154">hello status that indicates that an autoscale action was generated</span></span> |
| <span data-ttu-id="d9353-155">åtgärden</span><span class="sxs-lookup"><span data-stu-id="d9353-155">operation</span></span> |<span data-ttu-id="d9353-156">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-156">yes</span></span> |<span data-ttu-id="d9353-157">För en ökning av instanser blir ”skala ut” och för en minskning av instanserna blir ”skala i”</span><span class="sxs-lookup"><span data-stu-id="d9353-157">For an increase of instances, it will be "Scale Out" and for a decrease in instances, it will be "Scale In"</span></span> |
| <span data-ttu-id="d9353-158">Kontexten</span><span class="sxs-lookup"><span data-stu-id="d9353-158">context</span></span> |<span data-ttu-id="d9353-159">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-159">yes</span></span> |<span data-ttu-id="d9353-160">hello Autoskala åtgärd kontext</span><span class="sxs-lookup"><span data-stu-id="d9353-160">hello autoscale action context</span></span> |
| <span data-ttu-id="d9353-161">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="d9353-161">timestamp</span></span> |<span data-ttu-id="d9353-162">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-162">yes</span></span> |<span data-ttu-id="d9353-163">Tidsstämpel när hello Autoskala åtgärd utlöstes</span><span class="sxs-lookup"><span data-stu-id="d9353-163">Time stamp when hello autoscale action was triggered</span></span> |
| <span data-ttu-id="d9353-164">id</span><span class="sxs-lookup"><span data-stu-id="d9353-164">id</span></span> |<span data-ttu-id="d9353-165">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-165">Yes</span></span> |<span data-ttu-id="d9353-166">Resource Manager-ID för hello autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="d9353-166">Resource Manager ID of hello autoscale setting</span></span> |
| <span data-ttu-id="d9353-167">namn</span><span class="sxs-lookup"><span data-stu-id="d9353-167">name</span></span> |<span data-ttu-id="d9353-168">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-168">Yes</span></span> |<span data-ttu-id="d9353-169">hello namnet på hello autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="d9353-169">hello name of hello autoscale setting</span></span> |
| <span data-ttu-id="d9353-170">Information</span><span class="sxs-lookup"><span data-stu-id="d9353-170">details</span></span> |<span data-ttu-id="d9353-171">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-171">Yes</span></span> |<span data-ttu-id="d9353-172">Förklaring av hello-åtgärd som hello Autoskala tjänsten tog och hello ändra i hello-instanser</span><span class="sxs-lookup"><span data-stu-id="d9353-172">Explanation of hello action that hello autoscale service took and hello change in hello instance count</span></span> |
| <span data-ttu-id="d9353-173">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="d9353-173">subscriptionId</span></span> |<span data-ttu-id="d9353-174">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-174">Yes</span></span> |<span data-ttu-id="d9353-175">Prenumerations-ID för hello målresurs som som skalas</span><span class="sxs-lookup"><span data-stu-id="d9353-175">Subscription ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d9353-176">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="d9353-176">resourceGroupName</span></span> |<span data-ttu-id="d9353-177">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-177">Yes</span></span> |<span data-ttu-id="d9353-178">Resursgruppens namn av hello målresurs som som skalas</span><span class="sxs-lookup"><span data-stu-id="d9353-178">Resource Group name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d9353-179">resourceName</span><span class="sxs-lookup"><span data-stu-id="d9353-179">resourceName</span></span> |<span data-ttu-id="d9353-180">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-180">Yes</span></span> |<span data-ttu-id="d9353-181">Namnet på hello målresurs som som skalas</span><span class="sxs-lookup"><span data-stu-id="d9353-181">Name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d9353-182">resourceType</span><span class="sxs-lookup"><span data-stu-id="d9353-182">resourceType</span></span> |<span data-ttu-id="d9353-183">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-183">Yes</span></span> |<span data-ttu-id="d9353-184">hello stöds tre värden: ”microsoft.classiccompute/domainnames/slots/roles” - Molntjänsten roller, ”microsoft.compute/virtualmachinescalesets” - Skalningsuppsättningar i virtuella datorer, och ”Microsoft.Web/serverfarms” - webbprogram</span><span class="sxs-lookup"><span data-stu-id="d9353-184">hello three supported values: "microsoft.classiccompute/domainnames/slots/roles" - Cloud Service roles, "microsoft.compute/virtualmachinescalesets" - Virtual Machine Scale Sets,  and "Microsoft.Web/serverfarms" - Web App</span></span> |
| <span data-ttu-id="d9353-185">resourceId</span><span class="sxs-lookup"><span data-stu-id="d9353-185">resourceId</span></span> |<span data-ttu-id="d9353-186">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-186">Yes</span></span> |<span data-ttu-id="d9353-187">Resource Manager-ID för hello målresurs som som skalas</span><span class="sxs-lookup"><span data-stu-id="d9353-187">Resource Manager ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d9353-188">portalLink</span><span class="sxs-lookup"><span data-stu-id="d9353-188">portalLink</span></span> |<span data-ttu-id="d9353-189">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-189">Yes</span></span> |<span data-ttu-id="d9353-190">Azure portal-länk toohello sammanfattningssidan hello målresurs</span><span class="sxs-lookup"><span data-stu-id="d9353-190">Azure portal link toohello summary page of hello target resource</span></span> |
| <span data-ttu-id="d9353-191">oldCapacity</span><span class="sxs-lookup"><span data-stu-id="d9353-191">oldCapacity</span></span> |<span data-ttu-id="d9353-192">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-192">Yes</span></span> |<span data-ttu-id="d9353-193">hello aktuella (gamla) instansantalet när Autoskala tog en skalningsåtgärd</span><span class="sxs-lookup"><span data-stu-id="d9353-193">hello current (old) instance count when Autoscale took a scale action</span></span> |
| <span data-ttu-id="d9353-194">newCapacity</span><span class="sxs-lookup"><span data-stu-id="d9353-194">newCapacity</span></span> |<span data-ttu-id="d9353-195">Ja</span><span class="sxs-lookup"><span data-stu-id="d9353-195">Yes</span></span> |<span data-ttu-id="d9353-196">hello nya instanser Autoskala skalas hello resurs för</span><span class="sxs-lookup"><span data-stu-id="d9353-196">hello new instance count that Autoscale scaled hello resource too</span></span>|
| <span data-ttu-id="d9353-197">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="d9353-197">Properties</span></span> |<span data-ttu-id="d9353-198">Nej</span><span class="sxs-lookup"><span data-stu-id="d9353-198">No</span></span> |<span data-ttu-id="d9353-199">Valfri.</span><span class="sxs-lookup"><span data-stu-id="d9353-199">Optional.</span></span> <span data-ttu-id="d9353-200">Ange < nyckel, värde > par (till exempel ordlista < String, String >).</span><span class="sxs-lookup"><span data-stu-id="d9353-200">Set of <Key, Value> pairs (for example,  Dictionary <String, String>).</span></span> <span data-ttu-id="d9353-201">hello egenskapsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="d9353-201">hello properties field is optional.</span></span> <span data-ttu-id="d9353-202">Du kan ange nycklar och värden som kan skickas med hello nyttolast i en anpassad användargränssnittet eller logik app baserat arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d9353-202">In a custom user interface  or Logic app based workflow, you can enter keys and values that can be passed using hello payload.</span></span> <span data-ttu-id="d9353-203">Ett annat sätt toopass anpassade egenskaper tillbaka toohello utgående webhook anrop är toouse hello webhook URI (som frågeparametrar)</span><span class="sxs-lookup"><span data-stu-id="d9353-203">An alternate way toopass custom properties back toohello outgoing webhook call is toouse hello webhook URI itself (as query parameters)</span></span> |
