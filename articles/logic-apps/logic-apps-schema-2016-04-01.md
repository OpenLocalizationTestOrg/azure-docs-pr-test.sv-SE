---
title: aaaSchema uppdaterar juni 1 2016 - Azure Logic Apps | Microsoft Docs
description: "Skapa JSON-definitioner för Logic Apps i Azure med schemaversionen 2016-06-01"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="dd87d-103">Schema-uppdateringar för Logikappar i Azure - 1 juni 2016</span><span class="sxs-lookup"><span data-stu-id="dd87d-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="dd87d-104">Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare toouse:</span><span class="sxs-lookup"><span data-stu-id="dd87d-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

* <span data-ttu-id="dd87d-105">[Scope](#scopes) låter dig kapsla åtgärder som en samling åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dd87d-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="dd87d-106">[Villkor och slingor](#conditions-loops) är nu förstklassigt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dd87d-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="dd87d-107">Mer exakta ordning för att köra åtgärder med hello `runAfter` egenskapen ersätta`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="dd87d-107">More precise ordering for running actions with hello `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="dd87d-108">tooupgrade dina logikappar från hello 1 augusti 2015 Förhandsgranska schemat toohello den 1 juni 2016 schemat [kolla hello uppgradera avsnittet](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="dd87d-108">tooupgrade your logic apps from hello August 1, 2015 preview schema toohello June 1, 2016 schema, [check out hello upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="dd87d-109">Scope</span><span class="sxs-lookup"><span data-stu-id="dd87d-109">Scopes</span></span>

<span data-ttu-id="dd87d-110">Det här schemat innehåller scope, så att du kan gruppera tillsammans eller kapslade åtgärder i varandra.</span><span class="sxs-lookup"><span data-stu-id="dd87d-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="dd87d-111">Ett villkor kan exempelvis innehålla ytterligare villkor.</span><span class="sxs-lookup"><span data-stu-id="dd87d-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="dd87d-112">Lär dig mer om [omfång syntax](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här exemplet grundläggande omfång:</span><span class="sxs-lookup"><span data-stu-id="dd87d-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="dd87d-113">Villkor och slingor ändringar</span><span class="sxs-lookup"><span data-stu-id="dd87d-113">Conditions and loops changes</span></span>

<span data-ttu-id="dd87d-114">I tidigare schemat har versioner, villkor och slingor parametrar som är associerade med en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dd87d-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="dd87d-115">Det här schemat lyfter denna begränsning så villkor och slingor visas nu som Åtgärdstyper.</span><span class="sxs-lookup"><span data-stu-id="dd87d-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="dd87d-116">Lär dig mer om [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här enkla exemplet för en åtgärd som villkoret:</span><span class="sxs-lookup"><span data-stu-id="dd87d-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a><span data-ttu-id="dd87d-117">Egenskapen 'runAfter'</span><span class="sxs-lookup"><span data-stu-id="dd87d-117">'runAfter' property</span></span>

<span data-ttu-id="dd87d-118">Hej `runAfter` egenskapen ersätter `dependsOn`, ger högre precision när du anger hello kör ordning för åtgärder baserat på hello status för tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dd87d-118">hello `runAfter` property replaces `dependsOn`, providing more precision when you specify hello run order for actions based on hello status of previous actions.</span></span>

<span data-ttu-id="dd87d-119">Hej `dependsOn` egenskapen var synonym med ”hello åtgärd kördes och lyckades”, oavsett hur många gånger du vill ha tooexecute en åtgärd, baserat på om hello föregående åtgärd har slutförts, misslyckades eller hoppas över.</span><span class="sxs-lookup"><span data-stu-id="dd87d-119">hello `dependsOn` property was synonymous with "hello action ran and was successful", no matter how many times you wanted tooexecute an action, based on whether hello previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="dd87d-120">Hej `runAfter` egenskapen ger den flexibiliteten som ett objekt som anger alla hello åtgärdsnamn efter vilken hello objektet körs.</span><span class="sxs-lookup"><span data-stu-id="dd87d-120">hello `runAfter` property provides that flexibility as an object that specifies all hello action names after which hello object runs.</span></span> <span data-ttu-id="dd87d-121">Den här egenskapen definierar också en matris med statuslägen som är godkända som utlösare.</span><span class="sxs-lookup"><span data-stu-id="dd87d-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="dd87d-122">Till exempel om du vill toorun efter steget A slutförs och även efter steg B lyckas eller misslyckas kan du skapa det här `runAfter` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="dd87d-122">For example, if you wanted toorun after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="dd87d-123">Uppgradera schemat</span><span class="sxs-lookup"><span data-stu-id="dd87d-123">Upgrade your schema</span></span>

<span data-ttu-id="dd87d-124">Uppgradera toohello tar nya schemat bara några steg.</span><span class="sxs-lookup"><span data-stu-id="dd87d-124">Upgrading toohello new schema only takes a few steps.</span></span> <span data-ttu-id="dd87d-125">hello uppgraderingsprocessen innehåller kör hello uppgraderingsskript, Spara som en ny logikapp och du kan eventuellt att skriva över hello tidigare logikapp.</span><span class="sxs-lookup"><span data-stu-id="dd87d-125">hello upgrade process includes running hello upgrade script, saving as a new logic app, and if you want, possibly overwriting hello previous logic app.</span></span>

1. <span data-ttu-id="dd87d-126">Öppna logikappen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dd87d-126">In hello Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="dd87d-127">Gå för**översikt**.</span><span class="sxs-lookup"><span data-stu-id="dd87d-127">Go too**Overview**.</span></span> <span data-ttu-id="dd87d-128">Välj hello logik app verktygsfältet **uppdatera Schema**.</span><span class="sxs-lookup"><span data-stu-id="dd87d-128">On hello logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Välj Uppdatera Schema][1]
   
    <span data-ttu-id="dd87d-130">hello returneras uppgraderade definition, som du kan kopiera och klistra in i en resursdefinition om det behövs.</span><span class="sxs-lookup"><span data-stu-id="dd87d-130">hello upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="dd87d-131">Men vi **rekommenderar** du väljer **Spara som** toomake till att alla referenser för anslutningen är giltiga i hello uppgraderas logikapp.</span><span class="sxs-lookup"><span data-stu-id="dd87d-131">However, we **strongly recommend** you choose **Save As** toomake sure that all connection references are valid in hello upgraded logic app.</span></span>

3. <span data-ttu-id="dd87d-132">I hello uppgradera bladet i verktygsfältet väljer **Spara som**.</span><span class="sxs-lookup"><span data-stu-id="dd87d-132">In hello upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="dd87d-133">Ange hello logik namn och status.</span><span class="sxs-lookup"><span data-stu-id="dd87d-133">Enter hello logic name and status.</span></span> <span data-ttu-id="dd87d-134">toodeploy logikappen uppgraderade väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="dd87d-134">toodeploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="dd87d-135">Bekräfta att logikappen uppgraderade fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="dd87d-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dd87d-136">Om du använder en manuell eller begäran utlösare ändras hello återanrop URL i den nya logikappen.</span><span class="sxs-lookup"><span data-stu-id="dd87d-136">If you are using a manual or request trigger, hello callback URL changes in your new logic app.</span></span> <span data-ttu-id="dd87d-137">Test hello nya URL: en toomake att hello slutpunkt till slutpunkt-upplevelse fungerar.</span><span class="sxs-lookup"><span data-stu-id="dd87d-137">Test hello new URL toomake sure hello end-to-end experience works.</span></span> <span data-ttu-id="dd87d-138">toopreserve tidigare URL: er, kan du klona via din befintliga logikapp.</span><span class="sxs-lookup"><span data-stu-id="dd87d-138">toopreserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="dd87d-139">*Valfria* toooverwrite logikappen tidigare med hello ny schemaversion hello i verktygsfältet väljer **klona**, nästa för**uppdatera Schema**.</span><span class="sxs-lookup"><span data-stu-id="dd87d-139">*Optional* toooverwrite your previous logic app with hello new schema version, on hello toolbar, choose **Clone**, next too**Update Schema**.</span></span> <span data-ttu-id="dd87d-140">Det här steget är nödvändigt endast om du vill tookeep hello samma resurs-ID eller begäran utlösaren URL för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="dd87d-140">This step is necessary only if you want tookeep hello same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="dd87d-141">Uppgradering av verktyget</span><span class="sxs-lookup"><span data-stu-id="dd87d-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="dd87d-142">Mappningsvillkoren</span><span class="sxs-lookup"><span data-stu-id="dd87d-142">Mapping conditions</span></span>

<span data-ttu-id="dd87d-143">I hello uppgraderas definition är hello-verktyget bästa prestanda på Gruppera true och false gren åtgärder som omfattning.</span><span class="sxs-lookup"><span data-stu-id="dd87d-143">In hello upgraded definition, hello tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="dd87d-144">Mer specifikt hello designer mönstret för `@equals(actions('a').status, 'Skipped')` ska visas som en `else` åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dd87d-144">Specifically, hello designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="dd87d-145">Om hello upptäcks inte kan tolkas mönster, kan hello verktyget Skapa olika villkor för både hello true och hello falsk förgrening.</span><span class="sxs-lookup"><span data-stu-id="dd87d-145">However, if hello tool detects unrecognizable patterns, hello tool might create separate conditions for both hello true and hello false branch.</span></span> <span data-ttu-id="dd87d-146">Du kan mappa om åtgärder efter uppgraderingen, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="dd87d-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="dd87d-147">'foreach-loop med villkoret</span><span class="sxs-lookup"><span data-stu-id="dd87d-147">'foreach' loop with condition</span></span>

<span data-ttu-id="dd87d-148">I nya hello-schemat kan du använda hello åtgärd tooreplicate hello filtermönstret av en `foreach` med ett villkor per artikel, men den här ändringen ska ske automatiskt när du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="dd87d-148">In hello new schema, you can use hello filter action tooreplicate hello pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="dd87d-149">hello villkor blir en filteråtgärd innan hello foreach-slinga för att returnera en matris av objekt som matchar villkoret hello och att matris skickades till hello foreach-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dd87d-149">hello condition becomes a filter action before hello foreach loop for returning only an array of items that match hello condition, and that array is passed into hello foreach action.</span></span> <span data-ttu-id="dd87d-150">Ett exempel finns [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="dd87d-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="dd87d-151">Resurstaggar</span><span class="sxs-lookup"><span data-stu-id="dd87d-151">Resource tags</span></span>

<span data-ttu-id="dd87d-152">När du har uppgraderat tas resurstaggar bort, så måste du återställa dem för hello uppgraderas arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="dd87d-152">After you upgrade, resource tags are removed, so you must reset them for hello upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="dd87d-153">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="dd87d-153">Other changes</span></span>

### <a name="renamed-manual-trigger-toorequest-trigger"></a><span data-ttu-id="dd87d-154">Byta namn på ”manuell” utlösaren too'request-utlösare</span><span class="sxs-lookup"><span data-stu-id="dd87d-154">Renamed 'manual' trigger too'request' trigger</span></span>

<span data-ttu-id="dd87d-155">Hej `manual` utlösartypen var föråldrad och har fått nytt namn för`request` med typen `http`.</span><span class="sxs-lookup"><span data-stu-id="dd87d-155">hello `manual` trigger type was deprecated and renamed too`request` with type `http`.</span></span> <span data-ttu-id="dd87d-156">Den här ändringen skapar mer konsekvens för hello typ av mönster som hello utlösare är används toobuild.</span><span class="sxs-lookup"><span data-stu-id="dd87d-156">This change creates more consistency for hello kind of pattern that hello trigger is used toobuild.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="dd87d-157">Ny ”filter”-åtgärd</span><span class="sxs-lookup"><span data-stu-id="dd87d-157">New 'filter' action</span></span>

<span data-ttu-id="dd87d-158">toofilter en stor matris ned tooa mindre uppsättning objekt, hello nya `filter` accepterar en matris och ett villkor, utvärderar hello villkor för varje objekt och returnerar en matris med objekt som uppfyller villkoret hello.</span><span class="sxs-lookup"><span data-stu-id="dd87d-158">toofilter a large array down tooa smaller set of items, hello new `filter` type accepts an array and a condition, evaluates hello condition for each item, and returns an array with items meeting hello condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="dd87d-159">Begränsningar för 'foreach ”och” till ”-åtgärder</span><span class="sxs-lookup"><span data-stu-id="dd87d-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="dd87d-160">Hej `foreach` och `until` loop är begränsad tooa enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dd87d-160">hello `foreach` and `until` loop are restricted tooa single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="dd87d-161">Ny 'trackedProperties' för åtgärder</span><span class="sxs-lookup"><span data-stu-id="dd87d-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="dd87d-162">Åtgärder kan nu använda en annan egenskap som kallas `trackedProperties`, vilket är samma nivå toohello `runAfter` och `type` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dd87d-162">Actions can now have an additional property called `trackedProperties`, which is sibling toohello `runAfter` and `type` properties.</span></span> <span data-ttu-id="dd87d-163">Det här objektet anger vissa åtgärd indata eller utdata som du vill tooinclude i hello Azure diagnostisk telemetri orsakat som en del av ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd87d-163">This object specifies certain action inputs or outputs that you want tooinclude in hello Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="dd87d-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="dd87d-164">For example:</span></span>

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="dd87d-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd87d-165">Next Steps</span></span>
* [<span data-ttu-id="dd87d-166">Skapa arbetsflödesdefinitioner för logic apps</span><span class="sxs-lookup"><span data-stu-id="dd87d-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="dd87d-167">Skapa mallar för distribution för logic apps</span><span class="sxs-lookup"><span data-stu-id="dd87d-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
