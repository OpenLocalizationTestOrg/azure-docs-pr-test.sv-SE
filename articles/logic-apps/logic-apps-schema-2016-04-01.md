---
title: Schemat uppdateras juni 1 2016 - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: 43df04d6478e44c82c88b17d916cfc9fe4afc03e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="6e5e3-103">Schema-uppdateringar för Logikappar i Azure - 1 juni 2016</span><span class="sxs-lookup"><span data-stu-id="6e5e3-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="6e5e3-104">Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare att använda:</span><span class="sxs-lookup"><span data-stu-id="6e5e3-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

* <span data-ttu-id="6e5e3-105">[Scope](#scopes) låter dig kapsla åtgärder som en samling åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="6e5e3-106">[Villkor och slingor](#conditions-loops) är nu förstklassigt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="6e5e3-107">Mer exakta ordning för åtgärder med körs den `runAfter` egenskapen ersätta`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="6e5e3-107">More precise ordering for running actions with the `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="6e5e3-108">Att uppgradera dina logic apps från 1 augusti 2015 preview schemat till den 1 juni 2016-schemat [kolla avsnittet Uppgradera](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="6e5e3-108">To upgrade your logic apps from the August 1, 2015 preview schema to the June 1, 2016 schema, [check out the upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="6e5e3-109">Scope</span><span class="sxs-lookup"><span data-stu-id="6e5e3-109">Scopes</span></span>

<span data-ttu-id="6e5e3-110">Det här schemat innehåller scope, så att du kan gruppera tillsammans eller kapslade åtgärder i varandra.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="6e5e3-111">Ett villkor kan exempelvis innehålla ytterligare villkor.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="6e5e3-112">Lär dig mer om [omfång syntax](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här exemplet grundläggande omfång:</span><span class="sxs-lookup"><span data-stu-id="6e5e3-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

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
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="6e5e3-113">Villkor och slingor ändringar</span><span class="sxs-lookup"><span data-stu-id="6e5e3-113">Conditions and loops changes</span></span>

<span data-ttu-id="6e5e3-114">I tidigare schemat har versioner, villkor och slingor parametrar som är associerade med en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="6e5e3-115">Det här schemat lyfter denna begränsning så villkor och slingor visas nu som Åtgärdstyper.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="6e5e3-116">Lär dig mer om [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här enkla exemplet för en åtgärd som villkoret:</span><span class="sxs-lookup"><span data-stu-id="6e5e3-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

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
## <a name="runafter-property"></a><span data-ttu-id="6e5e3-117">Egenskapen 'runAfter'</span><span class="sxs-lookup"><span data-stu-id="6e5e3-117">'runAfter' property</span></span>

<span data-ttu-id="6e5e3-118">Den `runAfter` egenskapen ersätter `dependsOn`, ger högre precision när du anger kör ordning för åtgärder baserat på status för tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-118">The `runAfter` property replaces `dependsOn`, providing more precision when you specify the run order for actions based on the status of previous actions.</span></span>

<span data-ttu-id="6e5e3-119">Den `dependsOn` egenskapen var synonym med ”åtgärden kördes och lyckades”, oavsett hur många gånger som du vill utföra en åtgärd, baserat på om den föregående åtgärden lyckades, misslyckades eller hoppas över.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-119">The `dependsOn` property was synonymous with "the action ran and was successful", no matter how many times you wanted to execute an action, based on whether the previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="6e5e3-120">Den `runAfter` egenskapen ger den flexibiliteten som ett objekt som anger alla åtgärdsnamn efter vilken objektet körs.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-120">The `runAfter` property provides that flexibility as an object that specifies all the action names after which the object runs.</span></span> <span data-ttu-id="6e5e3-121">Den här egenskapen definierar också en matris med statuslägen som är godkända som utlösare.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="6e5e3-122">Till exempel om du vill köra när steget A lyckas och även efter steg B lyckas eller misslyckas kan du skapa detta `runAfter` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="6e5e3-122">For example, if you wanted to run after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="6e5e3-123">Uppgradera schemat</span><span class="sxs-lookup"><span data-stu-id="6e5e3-123">Upgrade your schema</span></span>

<span data-ttu-id="6e5e3-124">Uppgradera till det nya schemat tar bara några steg.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-124">Upgrading to the new schema only takes a few steps.</span></span> <span data-ttu-id="6e5e3-125">Uppgraderingsprocessen innehåller kör uppgraderingsskriptet, spara i en ny logikapp och du kan eventuellt att skriva över tidigare logikappen.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-125">The upgrade process includes running the upgrade script, saving as a new logic app, and if you want, possibly overwriting the previous logic app.</span></span>

1. <span data-ttu-id="6e5e3-126">Öppna logikappen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-126">In the Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="6e5e3-127">Gå till **översikt**.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-127">Go to **Overview**.</span></span> <span data-ttu-id="6e5e3-128">I verktygsfältet logik app välja **uppdatera Schema**.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-128">On the logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Välj Uppdatera Schema][1]
   
    <span data-ttu-id="6e5e3-130">Definitionen för uppgraderade returneras som du kan kopiera och klistra in i en resursdefinition om det behövs.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-130">The upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="6e5e3-131">Men vi **rekommenderar** du väljer **Spara som** att se till att alla referenser för anslutningen är giltig i uppgraderade logikappen.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-131">However, we **strongly recommend** you choose **Save As** to make sure that all connection references are valid in the upgraded logic app.</span></span>

3. <span data-ttu-id="6e5e3-132">I verktygsfältet uppgradera bladet väljer **Spara som**.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-132">In the upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="6e5e3-133">Ange logiken namn och status.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-133">Enter the logic name and status.</span></span> <span data-ttu-id="6e5e3-134">För att distribuera din uppgraderade logikapp, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-134">To deploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="6e5e3-135">Bekräfta att logikappen uppgraderade fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6e5e3-136">Om du använder en manuell eller begäran utlösare ändrar återanrop URL i den nya logikappen.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-136">If you are using a manual or request trigger, the callback URL changes in your new logic app.</span></span> <span data-ttu-id="6e5e3-137">Testa den nya URL Om du vill kontrollera slutpunkt till slutpunkt experience fungerar.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-137">Test the new URL to make sure the end-to-end experience works.</span></span> <span data-ttu-id="6e5e3-138">För att bevara tidigare URL: er, kan du klona via din befintliga logikapp.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-138">To preserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="6e5e3-139">*Valfria* om du vill skriva över tidigare logikappen med den nya schemaversionen i verktygsfältet väljer **klona**, bredvid **uppdatera Schema**.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-139">*Optional* To overwrite your previous logic app with the new schema version, on the toolbar, choose **Clone**, next to **Update Schema**.</span></span> <span data-ttu-id="6e5e3-140">Det här steget krävs endast om du vill behålla samma resurs-ID eller begära utlösaren URL för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-140">This step is necessary only if you want to keep the same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="6e5e3-141">Uppgradering av verktyget</span><span class="sxs-lookup"><span data-stu-id="6e5e3-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="6e5e3-142">Mappningsvillkoren</span><span class="sxs-lookup"><span data-stu-id="6e5e3-142">Mapping conditions</span></span>

<span data-ttu-id="6e5e3-143">I den uppgraderade definitionen gör verktyget bästa prestanda på Gruppera true och false gren åtgärder som omfattning.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-143">In the upgraded definition, the tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="6e5e3-144">Mer specifikt designer mönstret för `@equals(actions('a').status, 'Skipped')` ska visas som en `else` åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-144">Specifically, the designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="6e5e3-145">Om verktyget upptäcker inte kan tolkas mönster, kan verktyget Skapa olika villkor för både på true och false grenen.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-145">However, if the tool detects unrecognizable patterns, the tool might create separate conditions for both the true and the false branch.</span></span> <span data-ttu-id="6e5e3-146">Du kan mappa om åtgärder efter uppgraderingen, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="6e5e3-147">'foreach-loop med villkoret</span><span class="sxs-lookup"><span data-stu-id="6e5e3-147">'foreach' loop with condition</span></span>

<span data-ttu-id="6e5e3-148">I det nya schemat kan du använda filteråtgärd för att replikera mönstret för en `foreach` med ett villkor per artikel, men den här ändringen ska ske automatiskt när du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-148">In the new schema, you can use the filter action to replicate the pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="6e5e3-149">Villkoret blir en filteråtgärd innan foreach-slinga för att returnera en matris med objekt som matchar villkoret och den matrisen skickades till foreach-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-149">The condition becomes a filter action before the foreach loop for returning only an array of items that match the condition, and that array is passed into the foreach action.</span></span> <span data-ttu-id="6e5e3-150">Ett exempel finns [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="6e5e3-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="6e5e3-151">Resurstaggar</span><span class="sxs-lookup"><span data-stu-id="6e5e3-151">Resource tags</span></span>

<span data-ttu-id="6e5e3-152">När du har uppgraderat tas resurstaggar bort, så måste du återställa dem för det uppgraderade arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-152">After you upgrade, resource tags are removed, so you must reset them for the upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="6e5e3-153">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="6e5e3-153">Other changes</span></span>

### <a name="renamed-manual-trigger-to-request-trigger"></a><span data-ttu-id="6e5e3-154">Har bytt namn till ”manuell” utlösaren till ”begäran” utlösare</span><span class="sxs-lookup"><span data-stu-id="6e5e3-154">Renamed 'manual' trigger to 'request' trigger</span></span>

<span data-ttu-id="6e5e3-155">Den `manual` utlösartypen var föråldrad och bytt namn till `request` med typen `http`.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-155">The `manual` trigger type was deprecated and renamed to `request` with type `http`.</span></span> <span data-ttu-id="6e5e3-156">Den här ändringen skapar mer konsekvens för typ av mönster som utlösaren används för att skapa.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-156">This change creates more consistency for the kind of pattern that the trigger is used to build.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="6e5e3-157">Ny ”filter”-åtgärd</span><span class="sxs-lookup"><span data-stu-id="6e5e3-157">New 'filter' action</span></span>

<span data-ttu-id="6e5e3-158">Filtrera en stor matris till en mindre uppsättning objekt som den nya `filter` accepterar en matris och ett villkor, utvärderar villkor för varje objekt och returnerar en matris med objekt som uppfyller villkoret.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-158">To filter a large array down to a smaller set of items, the new `filter` type accepts an array and a condition, evaluates the condition for each item, and returns an array with items meeting the condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="6e5e3-159">Begränsningar för 'foreach ”och” till ”-åtgärder</span><span class="sxs-lookup"><span data-stu-id="6e5e3-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="6e5e3-160">Den `foreach` och `until` loop är begränsade till en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-160">The `foreach` and `until` loop are restricted to a single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="6e5e3-161">Ny 'trackedProperties' för åtgärder</span><span class="sxs-lookup"><span data-stu-id="6e5e3-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="6e5e3-162">Åtgärder kan nu använda en annan egenskap som kallas `trackedProperties`, som är på samma nivå i `runAfter` och `type` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-162">Actions can now have an additional property called `trackedProperties`, which is sibling to the `runAfter` and `type` properties.</span></span> <span data-ttu-id="6e5e3-163">Det här objektet anger vissa åtgärd indata eller utdata som du vill inkludera i Azure diagnostisk telemetri, orsakat som en del av ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="6e5e3-163">This object specifies certain action inputs or outputs that you want to include in the Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="6e5e3-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6e5e3-164">For example:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6e5e3-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e5e3-165">Next Steps</span></span>
* [<span data-ttu-id="6e5e3-166">Skapa arbetsflödesdefinitioner för logic apps</span><span class="sxs-lookup"><span data-stu-id="6e5e3-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="6e5e3-167">Skapa mallar för distribution för logic apps</span><span class="sxs-lookup"><span data-stu-id="6e5e3-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
