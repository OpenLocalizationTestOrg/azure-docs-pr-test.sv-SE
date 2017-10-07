---
title: "aaaCustom upp scheman för B2B övervakning - Azure Logic Apps | Microsoft Docs"
description: "Skapa anpassade spårning scheman toomonitor B2B-meddelanden från transaktioner i ditt Azure-konto för integrering."
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
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="328ae-103">Aktivera spårning toomonitor hela arbetsflödet, slutpunkt-till-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="328ae-103">Enable tracking toomonitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="328ae-104">Det finns inbyggda spåra att du kan aktivera för olika delar av ditt företag att arbetsflödet, till exempel uppföljning AS2 eller X12 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="328ae-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="328ae-105">När du skapar arbetsflöden som omfattar en logikapp, BizTalk Server, SQL Server eller något annat lager kan du aktivera anpassade spårning loggar händelser från hello början toohello slutet av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="328ae-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from hello beginning toohello end of your workflow.</span></span> 

<span data-ttu-id="328ae-106">Det här avsnittet innehåller anpassad kod som du kan använda i hello lager utanför din logikapp.</span><span class="sxs-lookup"><span data-stu-id="328ae-106">This topic provides custom code that you can use in hello layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="328ae-107">Spårning av anpassade schemat</span><span class="sxs-lookup"><span data-stu-id="328ae-107">Custom tracking schema</span></span>
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

| <span data-ttu-id="328ae-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="328ae-108">Property</span></span> | <span data-ttu-id="328ae-109">Typ</span><span class="sxs-lookup"><span data-stu-id="328ae-109">Type</span></span> | <span data-ttu-id="328ae-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="328ae-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="328ae-111">SourceType</span><span class="sxs-lookup"><span data-stu-id="328ae-111">sourceType</span></span> |   | <span data-ttu-id="328ae-112">Typ av hello kör källa.</span><span class="sxs-lookup"><span data-stu-id="328ae-112">Type of hello run source.</span></span> <span data-ttu-id="328ae-113">Tillåtna värden är **Microsoft.Logic/workflows** och **anpassade**.</span><span class="sxs-lookup"><span data-stu-id="328ae-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="328ae-114">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-114">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-115">Källa</span><span class="sxs-lookup"><span data-stu-id="328ae-115">Source</span></span> |   | <span data-ttu-id="328ae-116">Om hello källtypen är **Microsoft.Logic/workflows**, hello källinformation måste toofollow det här schemat.</span><span class="sxs-lookup"><span data-stu-id="328ae-116">If hello source type is **Microsoft.Logic/workflows**, hello source information needs toofollow this schema.</span></span> <span data-ttu-id="328ae-117">Om hello källtypen är **anpassade**, hello-schemat är en JToken.</span><span class="sxs-lookup"><span data-stu-id="328ae-117">If hello source type is **custom**, hello schema is a JToken.</span></span> <span data-ttu-id="328ae-118">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-118">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-119">system-ID</span><span class="sxs-lookup"><span data-stu-id="328ae-119">systemId</span></span> | <span data-ttu-id="328ae-120">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-120">String</span></span> | <span data-ttu-id="328ae-121">Logik app system-ID.</span><span class="sxs-lookup"><span data-stu-id="328ae-121">Logic app system ID.</span></span> <span data-ttu-id="328ae-122">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-122">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-123">runId</span><span class="sxs-lookup"><span data-stu-id="328ae-123">runId</span></span> | <span data-ttu-id="328ae-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-124">String</span></span> | <span data-ttu-id="328ae-125">Logikapp kör ID.</span><span class="sxs-lookup"><span data-stu-id="328ae-125">Logic app run ID.</span></span> <span data-ttu-id="328ae-126">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-126">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-127">operationName</span><span class="sxs-lookup"><span data-stu-id="328ae-127">operationName</span></span> | <span data-ttu-id="328ae-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-128">String</span></span> | <span data-ttu-id="328ae-129">Namn på hello-åtgärd (till exempel åtgärd eller utlösare).</span><span class="sxs-lookup"><span data-stu-id="328ae-129">Name of hello operation (for example, action or trigger).</span></span> <span data-ttu-id="328ae-130">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-130">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="328ae-131">repeatItemScopeName</span></span> | <span data-ttu-id="328ae-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-132">String</span></span> | <span data-ttu-id="328ae-133">Upprepa objektnamnet om hello åtgärd inuti en `foreach` / `until` loop.</span><span class="sxs-lookup"><span data-stu-id="328ae-133">Repeat item name if hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="328ae-134">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-134">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="328ae-135">repeatItemIndex</span></span> | <span data-ttu-id="328ae-136">Integer</span><span class="sxs-lookup"><span data-stu-id="328ae-136">Integer</span></span> | <span data-ttu-id="328ae-137">Om hello åtgärden är inuti en `foreach` / `until` loop.</span><span class="sxs-lookup"><span data-stu-id="328ae-137">Whether hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="328ae-138">Anger hello upprepade objektindex.</span><span class="sxs-lookup"><span data-stu-id="328ae-138">Indicates hello repeated item index.</span></span> <span data-ttu-id="328ae-139">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-139">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="328ae-140">trackingId</span></span> | <span data-ttu-id="328ae-141">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-141">String</span></span> | <span data-ttu-id="328ae-142">Spårnings-ID, toocorrelate hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="328ae-142">Tracking ID, toocorrelate hello messages.</span></span> <span data-ttu-id="328ae-143">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="328ae-143">(Optional)</span></span> |
| <span data-ttu-id="328ae-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="328ae-144">correlationId</span></span> | <span data-ttu-id="328ae-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-145">String</span></span> | <span data-ttu-id="328ae-146">Korrelations-ID, toocorrelate hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="328ae-146">Correlation ID, toocorrelate hello messages.</span></span> <span data-ttu-id="328ae-147">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="328ae-147">(Optional)</span></span> |
| <span data-ttu-id="328ae-148">ClientRequestId</span><span class="sxs-lookup"><span data-stu-id="328ae-148">clientRequestId</span></span> | <span data-ttu-id="328ae-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="328ae-149">String</span></span> | <span data-ttu-id="328ae-150">Klienten kan fylla i den toocorrelate meddelanden.</span><span class="sxs-lookup"><span data-stu-id="328ae-150">Client can populate it toocorrelate messages.</span></span> <span data-ttu-id="328ae-151">(Valfritt)</span><span class="sxs-lookup"><span data-stu-id="328ae-151">(Optional)</span></span> |
| <span data-ttu-id="328ae-152">EventLevel</span><span class="sxs-lookup"><span data-stu-id="328ae-152">eventLevel</span></span> |   | <span data-ttu-id="328ae-153">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="328ae-153">Level of hello event.</span></span> <span data-ttu-id="328ae-154">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-154">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-155">EventTime</span><span class="sxs-lookup"><span data-stu-id="328ae-155">eventTime</span></span> |   | <span data-ttu-id="328ae-156">Tid för hello händelsen i UTC-formatet ÅÅÅÅ-MM-DDTHH:MM:SS.00000Z.</span><span class="sxs-lookup"><span data-stu-id="328ae-156">Time of hello event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="328ae-157">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-157">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-158">RecordType</span><span class="sxs-lookup"><span data-stu-id="328ae-158">recordType</span></span> |   | <span data-ttu-id="328ae-159">Typ av hello spåra post.</span><span class="sxs-lookup"><span data-stu-id="328ae-159">Type of hello track record.</span></span> <span data-ttu-id="328ae-160">Tillåtna värde är **anpassade**.</span><span class="sxs-lookup"><span data-stu-id="328ae-160">Allowed value is **custom**.</span></span> <span data-ttu-id="328ae-161">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-161">(Mandatory)</span></span> |
| <span data-ttu-id="328ae-162">Post</span><span class="sxs-lookup"><span data-stu-id="328ae-162">record</span></span> |   | <span data-ttu-id="328ae-163">Anpassad posttyp.</span><span class="sxs-lookup"><span data-stu-id="328ae-163">Custom record type.</span></span> <span data-ttu-id="328ae-164">Tillåtna format är JToken.</span><span class="sxs-lookup"><span data-stu-id="328ae-164">Allowed format is JToken.</span></span> <span data-ttu-id="328ae-165">(Obligatoriskt)</span><span class="sxs-lookup"><span data-stu-id="328ae-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="328ae-166">B2B-protokollet spårning scheman</span><span class="sxs-lookup"><span data-stu-id="328ae-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="328ae-167">Information om spårning av scheman B2B-protokollet finns i:</span><span class="sxs-lookup"><span data-stu-id="328ae-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="328ae-168">AS2-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="328ae-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="328ae-169">X12-spårningsscheman</span><span class="sxs-lookup"><span data-stu-id="328ae-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="328ae-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="328ae-170">Next steps</span></span>
* <span data-ttu-id="328ae-171">Lär dig mer om [övervakning B2B-meddelanden](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="328ae-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="328ae-172">Lär dig mer om [spåra B2B-meddelanden i hello Operations Management Suite-portalen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="328ae-172">Learn about [tracking B2B messages in hello Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="328ae-173">Mer information om hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="328ae-173">Learn more about hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
