---
title: "aaaAdd villkor och starta arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Styra hur arbetsflöden körs i Azure Logic Apps genom att lägga till införa villkorslogik, utlösare, åtgärder och parametrar."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a><span data-ttu-id="5a64f-103">Använda funktioner i Logic Apps</span><span class="sxs-lookup"><span data-stu-id="5a64f-103">Use Logic Apps features</span></span>

<span data-ttu-id="5a64f-104">I en [föregående avsnitt](../logic-apps/logic-apps-create-a-logic-app.md), du har skapat din första logikapp.</span><span class="sxs-lookup"><span data-stu-id="5a64f-104">In a [previous topic](../logic-apps/logic-apps-create-a-logic-app.md), you created your first logic app.</span></span> <span data-ttu-id="5a64f-105">toocontrol din logikapp arbetsflöde, du kan ange olika sökvägar för dina logic app toorun och hur för bearbetar data i matriser, samlingar och batchar.</span><span class="sxs-lookup"><span data-stu-id="5a64f-105">toocontrol your logic app's workflow, you can specify different paths for your logic app toorun and how too process data in arrays, collections, and batches.</span></span> <span data-ttu-id="5a64f-106">Du kan inkludera dessa element i arbetsflödet logik app:</span><span class="sxs-lookup"><span data-stu-id="5a64f-106">You can include these elements in your logic app workflow:</span></span>

* <span data-ttu-id="5a64f-107">Villkor och [växla instruktioner](../logic-apps/logic-apps-switch-case.md) låta din logikapp som kör olika åtgärder baserat på om särskilda villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="5a64f-107">Conditions and [switch statements](../logic-apps/logic-apps-switch-case.md) let your logic app run different actions based on whether specific conditions are met.</span></span>

* <span data-ttu-id="5a64f-108">[Slingor](../logic-apps/logic-apps-loops-and-scopes.md) låta din logikapp köra steg upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="5a64f-108">[Loops](../logic-apps/logic-apps-loops-and-scopes.md) let your logic app run steps repeatedly.</span></span> <span data-ttu-id="5a64f-109">T.ex, du kan upprepa åtgärder via en matris när du använder en **For_each** loop.</span><span class="sxs-lookup"><span data-stu-id="5a64f-109">For example, you can repeat actions over an array when you use a **For_each** loop.</span></span> <span data-ttu-id="5a64f-110">Du kan upprepa åtgärder förrän villkor när du använder en **tills** loop.</span><span class="sxs-lookup"><span data-stu-id="5a64f-110">Or you can repeat actions until a condition is met when you use an **Until** loop.</span></span>

* <span data-ttu-id="5a64f-111">[Scope](../logic-apps/logic-apps-loops-and-scopes.md) kan du gruppera flera instruktioner, till exempel tooimplement undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="5a64f-111">[Scopes](../logic-apps/logic-apps-loops-and-scopes.md) let you group series of actions together, for example, tooimplement exception handling.</span></span>

* <span data-ttu-id="5a64f-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) kan logikappen startar separat arbetsflöden för objekt i en matris när du använder hello **SplitOn** kommando.</span><span class="sxs-lookup"><span data-stu-id="5a64f-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lets your logic app start separate workflows for items in an array when you use hello **SplitOn** command.</span></span>

<span data-ttu-id="5a64f-113">Det här avsnittet beskriver andra begrepp för att skapa din logikapp:</span><span class="sxs-lookup"><span data-stu-id="5a64f-113">This topic introduces other concepts for building your logic app:</span></span>

* <span data-ttu-id="5a64f-114">Code visa tooedit en befintlig logikapp</span><span class="sxs-lookup"><span data-stu-id="5a64f-114">Code view tooedit an existing logic app</span></span>
* <span data-ttu-id="5a64f-115">Alternativ för att starta ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="5a64f-115">Options for starting a workflow</span></span>

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a><span data-ttu-id="5a64f-116">Villkor: Köra steg efter uppfyller ett villkor</span><span class="sxs-lookup"><span data-stu-id="5a64f-116">Conditions: Run steps only after meeting a condition</span></span>

<span data-ttu-id="5a64f-117">toohave logikappen steg endast körs när data uppfyller specifika villkor, du kan lägga till ett villkor som jämför data i hello arbetsflöde mot specifika fält eller värden.</span><span class="sxs-lookup"><span data-stu-id="5a64f-117">toohave your logic app run steps only when data meets specific criteria, you can add a condition that compares data in hello workflow against specific fields or values.</span></span>

<span data-ttu-id="5a64f-118">Anta att du har en logikapp som skickar för många e-postmeddelanden för inlägg på en webbplats RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="5a64f-118">For example, suppose you have a logic app that sends you too many emails for posts on a website's RSS feed.</span></span> <span data-ttu-id="5a64f-119">Du kan lägga till ett villkor så att din logikapp skickar e-postmeddelande endast när nya hello bokför tillhör tooa specifik kategori.</span><span class="sxs-lookup"><span data-stu-id="5a64f-119">You can add a condition so that your logic app sends email only when hello new post belongs tooa specific category.</span></span>

1. <span data-ttu-id="5a64f-120">I hello [Azure-portalen](https://portal.azure.com), hitta och öppna logikappen i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="5a64f-120">In hello [Azure portal](https://portal.azure.com), find and open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="5a64f-121">Lägg till ett villkor toohello arbetsflöde plats som du vill.</span><span class="sxs-lookup"><span data-stu-id="5a64f-121">Add a condition toohello workflow location that you want.</span></span> 

   <span data-ttu-id="5a64f-122">tooadd hello villkoret mellan befintliga stegen i hello logik app arbetsflödet pekaren hello över hello pilen där du vill att tooadd hello villkor.</span><span class="sxs-lookup"><span data-stu-id="5a64f-122">tooadd hello condition between existing steps in hello logic app workflow, move hello pointer over hello arrow where you want tooadd hello condition.</span></span> 
   <span data-ttu-id="5a64f-123">Välj hello **plustecknet** (**+**), Välj **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-123">Choose hello **plus sign** (**+**), then choose **Add a condition**.</span></span> <span data-ttu-id="5a64f-124">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5a64f-124">For example:</span></span>

   ![Lägg till villkor toologic app](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > <span data-ttu-id="5a64f-126">Om du vill tooadd ett villkor hello slutet av arbetsflödet aktuella gå toohello längst ned i din logikapp och välj **+ nytt steg**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-126">If you want tooadd a condition at hello end of your current workflow, go toohello bottom of your logic app, and choose **+ New step**.</span></span>

3. <span data-ttu-id="5a64f-127">Nu ska du ange hello villkor.</span><span class="sxs-lookup"><span data-stu-id="5a64f-127">Now define hello condition.</span></span> <span data-ttu-id="5a64f-128">Ange hello fält i datakällan som du vill tooevaluate, hello åtgärden tooperform och hello målvärdet eller fältet.</span><span class="sxs-lookup"><span data-stu-id="5a64f-128">Specify hello source field that you want tooevaluate, hello operation tooperform, and hello target value or field.</span></span> <span data-ttu-id="5a64f-129">tooadd befintliga fält tooyour tillstånd, Välj hello **Lägg till dynamisk innehållslistan**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-129">tooadd existing fields tooyour condition, choose from hello **Add dynamic content list**.</span></span>

   <span data-ttu-id="5a64f-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5a64f-130">For example:</span></span>

   ![Redigera villkor i grundläggande läge](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   <span data-ttu-id="5a64f-132">Här är hello fullständig villkor:</span><span class="sxs-lookup"><span data-stu-id="5a64f-132">Here's hello complete condition:</span></span>

   ![Slutförd](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > <span data-ttu-id="5a64f-134">toodefine hello villkor i koden, Välj **redigera i Avancerat läge**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-134">toodefine hello condition in code, choose **Edit in advanced mode**.</span></span> <span data-ttu-id="5a64f-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5a64f-135">For example:</span></span>
   > 
   > ![Redigera villkor i koden](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. <span data-ttu-id="5a64f-137">Under **Ja om** och **om Nej**, lägga till hello steg tooperform baserat på om hello villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="5a64f-137">Under **IF YES** and **IF NO**, add hello steps tooperform based on whether hello condition is met.</span></span>

   <span data-ttu-id="5a64f-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5a64f-138">For example:</span></span>

   ![Villkor med Ja och inga sökvägar](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > <span data-ttu-id="5a64f-140">Du kan dra befintliga åtgärder till hello **Ja om** och **om Nej** sökvägar.</span><span class="sxs-lookup"><span data-stu-id="5a64f-140">You can drag existing actions into hello **IF YES** and **IF NO** paths.</span></span>

5. <span data-ttu-id="5a64f-141">När du är klar sparar du din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5a64f-141">When you're done, save your logic app.</span></span>

<span data-ttu-id="5a64f-142">Nu får du e-postmeddelanden endast när hello inlägg uppfyller villkoret.</span><span class="sxs-lookup"><span data-stu-id="5a64f-142">Now you get emails only when hello posts meet your condition.</span></span>

## <a name="repeat-actions-over-a-list-with-foreach"></a><span data-ttu-id="5a64f-143">Upprepa åtgärder via en lista med forEach</span><span class="sxs-lookup"><span data-stu-id="5a64f-143">Repeat actions over a list with forEach</span></span>

<span data-ttu-id="5a64f-144">hello forEach loop anger en matris toorepeat åtgärden över.</span><span class="sxs-lookup"><span data-stu-id="5a64f-144">hello forEach loop specifies an array toorepeat an action over.</span></span> <span data-ttu-id="5a64f-145">Hello flödet misslyckas om den inte är en matris.</span><span class="sxs-lookup"><span data-stu-id="5a64f-145">If it is not an array, hello flow fails.</span></span> <span data-ttu-id="5a64f-146">Om du har action1 som en matris med meddelanden och du vill toosend varje meddelande, inkludera du den här forEach satsen i hello egenskaper för åtgärden:`forEach : "@action('action1').outputs.messages"`</span><span class="sxs-lookup"><span data-stu-id="5a64f-146">For example, if you have action1 that outputs an array of messages, and you want toosend each message, you can include this forEach statement in hello properties of your action: `forEach : "@action('action1').outputs.messages"`</span></span>

## <a name="edit-hello-code-definition-for-a-logic-app"></a><span data-ttu-id="5a64f-147">Redigera hello koddefinitionen för en logikapp</span><span class="sxs-lookup"><span data-stu-id="5a64f-147">Edit hello code definition for a logic app</span></span>

<span data-ttu-id="5a64f-148">Även om du har hello logik App Designer kan redigera du hello-kod som definierar en logikapp direkt.</span><span class="sxs-lookup"><span data-stu-id="5a64f-148">Although you have hello Logic App Designer, you can directly edit hello code that defines a logic app.</span></span>

1. <span data-ttu-id="5a64f-149">Välj på hello kommandofältet **vyn kod**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-149">On hello command bar, choose **Code view**.</span></span>

    <span data-ttu-id="5a64f-150">En fullständig redigerare öppnas och visar hello definition av du redigerade.</span><span class="sxs-lookup"><span data-stu-id="5a64f-150">A full editor opens and shows hello definition you edited.</span></span>

    ![Kodvy](media/logic-apps-use-logic-app-features/codeview.png)

    <span data-ttu-id="5a64f-152">I hello textredigerare som du kan kopiera och klistra in valfritt antal åtgärder i hello samma logikapp eller mellan logikappar.</span><span class="sxs-lookup"><span data-stu-id="5a64f-152">In hello text editor, you can copy and paste any number of actions within hello same logic app or between logic apps.</span></span> 
    <span data-ttu-id="5a64f-153">Du kan också enkelt lägga till eller ta bort hela avsnitt från hello definition och du kan också dela definitioner med andra.</span><span class="sxs-lookup"><span data-stu-id="5a64f-153">You can also easily add or remove entire sections from hello definition, and you can also share definitions with others.</span></span>

2. <span data-ttu-id="5a64f-154">toosave ändringarna, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-154">toosave your edits, choose **Save**.</span></span>

## <a name="parameters"></a><span data-ttu-id="5a64f-155">Parametrar</span><span class="sxs-lookup"><span data-stu-id="5a64f-155">Parameters</span></span>

<span data-ttu-id="5a64f-156">Vissa funktioner i Logic Apps är endast tillgängliga i kodvy, till exempel parametrar.</span><span class="sxs-lookup"><span data-stu-id="5a64f-156">Some Logic Apps capabilities are available only in code view, for example, parameters.</span></span> <span data-ttu-id="5a64f-157">Parametrar som gör det enkelt tooreuse värden i hela din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5a64f-157">Parameters make it easy tooreuse values throughout your logic app.</span></span> <span data-ttu-id="5a64f-158">Om du har en e-postadress som du vill använda i flera åtgärder, bör du till exempel definiera den e-postadressen som en parameter.</span><span class="sxs-lookup"><span data-stu-id="5a64f-158">For example, if you have an email address that you want use in several actions, you should define that email address as a parameter.</span></span>

<span data-ttu-id="5a64f-159">Parametrarna är bra för att dra ut värden att du är mycket troligt toochange.</span><span class="sxs-lookup"><span data-stu-id="5a64f-159">Parameters are good for pulling out values that you are likely toochange a lot.</span></span> <span data-ttu-id="5a64f-160">De är särskilt användbar när du behöver toooverride parametrar i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="5a64f-160">They are especially useful when you need toooverride parameters in different environments.</span></span> <span data-ttu-id="5a64f-161">hur toooverride parametrar baserat på miljön, se toolearn [redigera logik app definitioner](../logic-apps/logic-apps-author-definitions.md) och [REST API-dokumentation](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="5a64f-161">toolearn how toooverride parameters based on environment, see [Author logic app definitions](../logic-apps/logic-apps-author-definitions.md) and [REST API documentation](https://docs.microsoft.com/rest/api/logic).</span></span>

<span data-ttu-id="5a64f-162">Det här exemplet illustrerar hur tooupdate logikappen befintliga så att du kan använda parametrarna för hello frågeterm.</span><span class="sxs-lookup"><span data-stu-id="5a64f-162">This example shows how tooupdate your existing logic app so that you can use parameters for hello query term.</span></span>

1. <span data-ttu-id="5a64f-163">I kodvyn, hitta hello `parameters : {}` objekt och lägger till en `currentFeedUrl` objekt:</span><span class="sxs-lookup"><span data-stu-id="5a64f-163">In code view, find hello `parameters : {}` object, and add a `currentFeedUrl` object:</span></span>

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. <span data-ttu-id="5a64f-164">Gå toohello `When_a_feed-item_is_published` åtgärd, hitta hello `queries` avsnittet och Ersätt hello läsa värde med:`"feedUrl": "#@{parameters('currentFeedUrl')}"`</span><span class="sxs-lookup"><span data-stu-id="5a64f-164">Go toohello `When_a_feed-item_is_published` action, find hello `queries` section, and replace hello query value with : `"feedUrl": "#@{parameters('currentFeedUrl')}"`</span></span> 

    <span data-ttu-id="5a64f-165">toojoin två eller flera strängar, du kan också använda hello `concat` funktion.</span><span class="sxs-lookup"><span data-stu-id="5a64f-165">toojoin two or more strings, you can also use hello `concat` function.</span></span> 
    <span data-ttu-id="5a64f-166">Till exempel `"@concat('#',parameters('currentFeedUrl'))"` fungerar hello samma som hello ovan.</span><span class="sxs-lookup"><span data-stu-id="5a64f-166">For example, `"@concat('#',parameters('currentFeedUrl'))"` works hello same as hello above.</span></span>

3.  <span data-ttu-id="5a64f-167">När du är klar väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="5a64f-167">When you're done, choose **Save**.</span></span> 

    <span data-ttu-id="5a64f-168">Nu kan du ändra hello webbplats RSS-feed genom att ange en annan URL via hello `currentFeedURL` objekt.</span><span class="sxs-lookup"><span data-stu-id="5a64f-168">Now you can change hello website's RSS feed by passing a different URL through hello `currentFeedURL` object.</span></span>

<span data-ttu-id="5a64f-169">Lär dig mer om [hur tooauthor logik app definitioner](../logic-apps/logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="5a64f-169">Learn more about [how tooauthor logic app definitions](../logic-apps/logic-apps-author-definitions.md).</span></span>

## <a name="start-logic-app-workflows"></a><span data-ttu-id="5a64f-170">Starta logik app arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="5a64f-170">Start logic app workflows</span></span>

<span data-ttu-id="5a64f-171">Har du olika alternativ för att starta hello arbetsflöde som definierats i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5a64f-171">You have different options for starting hello workflow defined in your logic app.</span></span> <span data-ttu-id="5a64f-172">Du kan alltid starta ett arbetsflöde på begäran i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="5a64f-172">You can always start a workflow on-demand in hello [Azure portal].</span></span>

### <a name="recurrence-triggers"></a><span data-ttu-id="5a64f-173">Utlösare för upprepning</span><span class="sxs-lookup"><span data-stu-id="5a64f-173">Recurrence triggers</span></span>

<span data-ttu-id="5a64f-174">En utlösare för upprepning körs vid ett intervall som du anger.</span><span class="sxs-lookup"><span data-stu-id="5a64f-174">A recurrence trigger runs at an interval that you specify.</span></span> <span data-ttu-id="5a64f-175">När hello utlösaren har införa villkorslogik, avgör hello utlösaren om hello arbetsflödet måste toorun.</span><span class="sxs-lookup"><span data-stu-id="5a64f-175">When hello trigger has conditional logic, hello trigger determines whether hello workflow needs toorun.</span></span> <span data-ttu-id="5a64f-176">En utlösare Anger hello arbetsflödet ska köras genom att returnera en `200` statuskod.</span><span class="sxs-lookup"><span data-stu-id="5a64f-176">A trigger indicates hello workflow should run by returning a `200` status code.</span></span> <span data-ttu-id="5a64f-177">När hello arbetsflödet inte behöver toorun, hello utlösaren returnerar en `202` statuskod.</span><span class="sxs-lookup"><span data-stu-id="5a64f-177">When hello workflow doesn't need toorun, hello trigger returns a `202` status code.</span></span>

### <a name="callback-using-rest-apis"></a><span data-ttu-id="5a64f-178">Återanrop med hjälp av REST API: er</span><span class="sxs-lookup"><span data-stu-id="5a64f-178">Callback using REST APIs</span></span>

<span data-ttu-id="5a64f-179">toostart ett arbetsflöde, tjänster kan anropa en logik app slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5a64f-179">toostart a workflow, services can call a logic app endpoint.</span></span> <span data-ttu-id="5a64f-180">den här typen av logik app på begäran, väljer du toostart **kör nu** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="5a64f-180">toostart this kind of logic app on-demand, choose **Run now** on hello command bar.</span></span> <span data-ttu-id="5a64f-181">Se [starta arbetsflöden genom att anropa logik app slutpunkter som utlösare](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="5a64f-181">See [Start workflows by calling logic app endpoints as triggers](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<!-- Shared links -->
[Azure-portalen]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="5a64f-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a64f-183">Next steps</span></span>

* [<span data-ttu-id="5a64f-184">Växeln-instruktioner</span><span class="sxs-lookup"><span data-stu-id="5a64f-184">Switch statements</span></span>](../logic-apps/logic-apps-switch-case.md) 
* [<span data-ttu-id="5a64f-185">Loopar, omfattningar och debatching</span><span class="sxs-lookup"><span data-stu-id="5a64f-185">Loops, scopes, and debatching</span></span>](../logic-apps/logic-apps-loops-and-scopes.md)
* [<span data-ttu-id="5a64f-186">Skapa Logic App-definitioner</span><span class="sxs-lookup"><span data-stu-id="5a64f-186">Author logic app definitions</span></span>](../logic-apps/logic-apps-author-definitions.md)