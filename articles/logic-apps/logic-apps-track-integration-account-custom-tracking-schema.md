---
title: "Spårning av anpassade scheman för övervakning av B2B - Azure Logic Apps | Microsoft Docs"
description: "Skapa anpassade spårning scheman för att övervaka B2B-meddelanden från transaktioner i ditt Azure-konto för integrering."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b71a4938dde2a71f1ce29403af7aa9101358d64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="dd40f-103">Aktivera spårning att övervaka hela arbetsflödet, slutpunkt-till-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="dd40f-103">Enable tracking to monitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="dd40f-104">Det finns inbyggda spåra att du kan aktivera för olika delar av ditt företag att arbetsflödet, till exempel uppföljning AS2 eller X12 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dd40f-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="dd40f-105">När omfattar du skapar arbetsflöden som en logikapp, BizTalk Server, SQL Server eller något annat lager kan du aktivera anpassade spårning loggar händelser från början till slutet av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="dd40f-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from the beginning to the end of your workflow.</span></span> 

<span data-ttu-id="dd40f-106">Det här avsnittet innehåller anpassad kod som du kan använda i lagren utanför din logikapp.</span><span class="sxs-lookup"><span data-stu-id="dd40f-106">This topic provides custom code that you can use in the layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="dd40f-107">Spårning av anpassade schemat</span><span class="sxs-lookup"><span data-stu-id="dd40f-107">Custom tracking schema</span></span>
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| <span data-ttu-id="dd40f-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="dd40f-108">Property</span></span> | <span data-ttu-id="dd40f-109">Typ</span><span class="sxs-lookup"><span data-stu-id="dd40f-109">Type</span></span> | <span data-ttu-id="dd40f-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dd40f-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dd40f-111">SourceType</span><span class="sxs-lookup"><span data-stu-id="dd40f-111">sourceType</span></span> |   | <span data-ttu-id="dd40f-112">Typ av kör källan.</span><span class="sxs-lookup"><span data-stu-id="dd40f-112">Type of the run source.</span></span> <span data-ttu-id="dd40f-113">Tillåtna värden är **Microsoft.Logic/workflows** och **anpassade**.</span><span class="sxs-lookup"><span data-stu-id="dd40f-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="dd40f-114">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-114">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-115">Källa</span><span class="sxs-lookup"><span data-stu-id="dd40f-115">Source</span></span> |   | <span data-ttu-id="dd40f-116">Om källtypen är **Microsoft.Logic/workflows**, källinformation måste följa det här schemat.</span><span class="sxs-lookup"><span data-stu-id="dd40f-116">If the source type is **Microsoft.Logic/workflows**, the source information needs to follow this schema.</span></span> <span data-ttu-id="dd40f-117">Om källtypen är **anpassade**, schemat är en JToken.</span><span class="sxs-lookup"><span data-stu-id="dd40f-117">If the source type is **custom**, the schema is a JToken.</span></span> <span data-ttu-id="dd40f-118">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-118">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-119">system-ID</span><span class="sxs-lookup"><span data-stu-id="dd40f-119">systemId</span></span> | <span data-ttu-id="dd40f-120">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-120">String</span></span> | <span data-ttu-id="dd40f-121">Logik app system-ID.</span><span class="sxs-lookup"><span data-stu-id="dd40f-121">Logic app system ID.</span></span> <span data-ttu-id="dd40f-122">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-122">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-123">runId</span><span class="sxs-lookup"><span data-stu-id="dd40f-123">runId</span></span> | <span data-ttu-id="dd40f-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-124">String</span></span> | <span data-ttu-id="dd40f-125">Logikapp kör ID.</span><span class="sxs-lookup"><span data-stu-id="dd40f-125">Logic app run ID.</span></span> <span data-ttu-id="dd40f-126">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-126">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-127">operationName</span><span class="sxs-lookup"><span data-stu-id="dd40f-127">operationName</span></span> | <span data-ttu-id="dd40f-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-128">String</span></span> | <span data-ttu-id="dd40f-129">Namnet på åtgärden (till exempel åtgärd eller utlösare).</span><span class="sxs-lookup"><span data-stu-id="dd40f-129">Name of the operation (for example, action or trigger).</span></span> <span data-ttu-id="dd40f-130">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-130">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="dd40f-131">repeatItemScopeName</span></span> | <span data-ttu-id="dd40f-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-132">String</span></span> | <span data-ttu-id="dd40f-133">Upprepa objektnamnet om åtgärden i en `foreach` / `until` loop.</span><span class="sxs-lookup"><span data-stu-id="dd40f-133">Repeat item name if the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="dd40f-134">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-134">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="dd40f-135">repeatItemIndex</span></span> | <span data-ttu-id="dd40f-136">Integer</span><span class="sxs-lookup"><span data-stu-id="dd40f-136">Integer</span></span> | <span data-ttu-id="dd40f-137">Om instruktionen är inuti en `foreach` / `until` loop.</span><span class="sxs-lookup"><span data-stu-id="dd40f-137">Whether the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="dd40f-138">Anger det upprepade objekt indexet.</span><span class="sxs-lookup"><span data-stu-id="dd40f-138">Indicates the repeated item index.</span></span> <span data-ttu-id="dd40f-139">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-139">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="dd40f-140">trackingId</span></span> | <span data-ttu-id="dd40f-141">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-141">String</span></span> | <span data-ttu-id="dd40f-142">Spårnings-ID för att korrelera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dd40f-142">Tracking ID, to correlate the messages.</span></span> <span data-ttu-id="dd40f-143">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-143">(Optional)</span></span> |
| <span data-ttu-id="dd40f-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="dd40f-144">correlationId</span></span> | <span data-ttu-id="dd40f-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-145">String</span></span> | <span data-ttu-id="dd40f-146">Korrelations-ID, att korrelera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dd40f-146">Correlation ID, to correlate the messages.</span></span> <span data-ttu-id="dd40f-147">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-147">(Optional)</span></span> |
| <span data-ttu-id="dd40f-148">ClientRequestId</span><span class="sxs-lookup"><span data-stu-id="dd40f-148">clientRequestId</span></span> | <span data-ttu-id="dd40f-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="dd40f-149">String</span></span> | <span data-ttu-id="dd40f-150">Klienten kan fylla i den för att korrelera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dd40f-150">Client can populate it to correlate messages.</span></span> <span data-ttu-id="dd40f-151">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-151">(Optional)</span></span> |
| <span data-ttu-id="dd40f-152">EventLevel</span><span class="sxs-lookup"><span data-stu-id="dd40f-152">eventLevel</span></span> |   | <span data-ttu-id="dd40f-153">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="dd40f-153">Level of the event.</span></span> <span data-ttu-id="dd40f-154">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-154">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-155">EventTime</span><span class="sxs-lookup"><span data-stu-id="dd40f-155">eventTime</span></span> |   | <span data-ttu-id="dd40f-156">Tid för händelse, i UTC-format åååå-MM-DDTHH:MM:SS.00000Z.</span><span class="sxs-lookup"><span data-stu-id="dd40f-156">Time of the event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="dd40f-157">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-157">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-158">RecordType</span><span class="sxs-lookup"><span data-stu-id="dd40f-158">recordType</span></span> |   | <span data-ttu-id="dd40f-159">Typ av post spåra.</span><span class="sxs-lookup"><span data-stu-id="dd40f-159">Type of the track record.</span></span> <span data-ttu-id="dd40f-160">Tillåtna värde är **anpassade**.</span><span class="sxs-lookup"><span data-stu-id="dd40f-160">Allowed value is **custom**.</span></span> <span data-ttu-id="dd40f-161">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-161">(Mandatory)</span></span> |
| <span data-ttu-id="dd40f-162">Post</span><span class="sxs-lookup"><span data-stu-id="dd40f-162">record</span></span> |   | <span data-ttu-id="dd40f-163">Anpassad posttyp.</span><span class="sxs-lookup"><span data-stu-id="dd40f-163">Custom record type.</span></span> <span data-ttu-id="dd40f-164">Tillåtna format är JToken.</span><span class="sxs-lookup"><span data-stu-id="dd40f-164">Allowed format is JToken.</span></span> <span data-ttu-id="dd40f-165">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="dd40f-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="dd40f-166">B2B-protokollet spårning scheman</span><span class="sxs-lookup"><span data-stu-id="dd40f-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="dd40f-167">Information om spårning av scheman B2B-protokollet finns i:</span><span class="sxs-lookup"><span data-stu-id="dd40f-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="dd40f-168">AS2-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="dd40f-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="dd40f-169">X12-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="dd40f-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="dd40f-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd40f-170">Next steps</span></span>
* <span data-ttu-id="dd40f-171">Lär dig mer om [övervakning B2B-meddelanden](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="dd40f-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="dd40f-172">Lär dig mer om [spåra B2B-meddelanden i Operations Management Suite-portalen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="dd40f-172">Learn about [tracking B2B messages in the Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="dd40f-173">Lär dig mer om den [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dd40f-173">Learn more about the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
