---
title: "Lägg till villkor och starta arbetsflöden - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-logic-apps-features"></a><span data-ttu-id="d6825-103">Använda funktioner i Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d6825-103">Use Logic Apps features</span></span>

<span data-ttu-id="d6825-104">I en [föregående avsnitt](../logic-apps/logic-apps-create-a-logic-app.md), du har skapat din första logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6825-104">In a [previous topic](../logic-apps/logic-apps-create-a-logic-app.md), you created your first logic app.</span></span> <span data-ttu-id="d6825-105">Du kan ange olika sökvägar för din logikapp för att köra och bearbeta data i matriser, samlingar och batchar för att kontrollera din logikapp arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d6825-105">To control your logic app's workflow, you can specify different paths for your logic app to run and how to process data in arrays, collections, and batches.</span></span> <span data-ttu-id="d6825-106">Du kan inkludera dessa element i arbetsflödet logik app:</span><span class="sxs-lookup"><span data-stu-id="d6825-106">You can include these elements in your logic app workflow:</span></span>

* <span data-ttu-id="d6825-107">Villkor och [växla instruktioner](../logic-apps/logic-apps-switch-case.md) låta din logikapp som kör olika åtgärder baserat på om särskilda villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="d6825-107">Conditions and [switch statements](../logic-apps/logic-apps-switch-case.md) let your logic app run different actions based on whether specific conditions are met.</span></span>

* <span data-ttu-id="d6825-108">[Slingor](../logic-apps/logic-apps-loops-and-scopes.md) låta din logikapp köra steg upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="d6825-108">[Loops](../logic-apps/logic-apps-loops-and-scopes.md) let your logic app run steps repeatedly.</span></span> <span data-ttu-id="d6825-109">T.ex, du kan upprepa åtgärder via en matris när du använder en **For_each** loop.</span><span class="sxs-lookup"><span data-stu-id="d6825-109">For example, you can repeat actions over an array when you use a **For_each** loop.</span></span> <span data-ttu-id="d6825-110">Du kan upprepa åtgärder förrän villkor när du använder en **tills** loop.</span><span class="sxs-lookup"><span data-stu-id="d6825-110">Or you can repeat actions until a condition is met when you use an **Until** loop.</span></span>

* <span data-ttu-id="d6825-111">[Scope](../logic-apps/logic-apps-loops-and-scopes.md) kan du gruppera flera instruktioner, till exempel om du vill implementera undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="d6825-111">[Scopes](../logic-apps/logic-apps-loops-and-scopes.md) let you group series of actions together, for example, to implement exception handling.</span></span>

* <span data-ttu-id="d6825-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) kan logikappen startar separat arbetsflöden för objekt i en matris när du använder den **SplitOn** kommando.</span><span class="sxs-lookup"><span data-stu-id="d6825-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lets your logic app start separate workflows for items in an array when you use the **SplitOn** command.</span></span>

<span data-ttu-id="d6825-113">Det här avsnittet beskriver andra begrepp för att skapa din logikapp:</span><span class="sxs-lookup"><span data-stu-id="d6825-113">This topic introduces other concepts for building your logic app:</span></span>

* <span data-ttu-id="d6825-114">Kodvy att redigera en befintlig logikapp</span><span class="sxs-lookup"><span data-stu-id="d6825-114">Code view to edit an existing logic app</span></span>
* <span data-ttu-id="d6825-115">Alternativ för att starta ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="d6825-115">Options for starting a workflow</span></span>

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a><span data-ttu-id="d6825-116">Villkor: Köra steg efter uppfyller ett villkor</span><span class="sxs-lookup"><span data-stu-id="d6825-116">Conditions: Run steps only after meeting a condition</span></span>

<span data-ttu-id="d6825-117">Om du vill att din logikapp steg endast körs när data uppfyller specifika villkor kan du lägga till ett villkor som jämför data i arbetsflödet mot specifika fält eller värden.</span><span class="sxs-lookup"><span data-stu-id="d6825-117">To have your logic app run steps only when data meets specific criteria, you can add a condition that compares data in the workflow against specific fields or values.</span></span>

<span data-ttu-id="d6825-118">Anta att du har en logikapp som skickar för många e-postmeddelanden för inlägg på en webbplats RSS-feed.</span><span class="sxs-lookup"><span data-stu-id="d6825-118">For example, suppose you have a logic app that sends you too many emails for posts on a website's RSS feed.</span></span> <span data-ttu-id="d6825-119">Du kan lägga till ett villkor för din logikapp skickar e-postmeddelande när det nya inlägget tillhör en viss kategori.</span><span class="sxs-lookup"><span data-stu-id="d6825-119">You can add a condition so that your logic app sends email only when the new post belongs to a specific category.</span></span>

1. <span data-ttu-id="d6825-120">I den [Azure-portalen](https://portal.azure.com), hitta och öppna logikappen i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="d6825-120">In the [Azure portal](https://portal.azure.com), find and open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="d6825-121">Lägga till ett villkor i arbetsflödet platsen som du vill.</span><span class="sxs-lookup"><span data-stu-id="d6825-121">Add a condition to the workflow location that you want.</span></span> 

   <span data-ttu-id="d6825-122">Om du vill lägga till villkor mellan befintliga stegen i logik app arbetsflödet muspekaren på pilen där du vill lägga till villkoret.</span><span class="sxs-lookup"><span data-stu-id="d6825-122">To add the condition between existing steps in the logic app workflow, move the pointer over the arrow where you want to add the condition.</span></span> 
   <span data-ttu-id="d6825-123">Välj den **plustecknet** (**+**), Välj **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="d6825-123">Choose the **plus sign** (**+**), then choose **Add a condition**.</span></span> <span data-ttu-id="d6825-124">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d6825-124">For example:</span></span>

   ![Lägg till villkor logikapp](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > <span data-ttu-id="d6825-126">Om du vill lägga till ett villkor i slutet av arbetsflödet aktuella gå längst ned i din logikapp och välj **+ nytt steg**.</span><span class="sxs-lookup"><span data-stu-id="d6825-126">If you want to add a condition at the end of your current workflow, go to the bottom of your logic app, and choose **+ New step**.</span></span>

3. <span data-ttu-id="d6825-127">Nu ska du ange villkoret.</span><span class="sxs-lookup"><span data-stu-id="d6825-127">Now define the condition.</span></span> <span data-ttu-id="d6825-128">Ange det fält i datakällan som du vill utvärdera åtgärden som ska utföras och målvärdet eller fältet.</span><span class="sxs-lookup"><span data-stu-id="d6825-128">Specify the source field that you want to evaluate, the operation to perform, and the target value or field.</span></span> <span data-ttu-id="d6825-129">Lägg till befintliga fält i dina villkor, Välj den **Lägg till dynamisk innehållslistan**.</span><span class="sxs-lookup"><span data-stu-id="d6825-129">To add existing fields to your condition, choose from the **Add dynamic content list**.</span></span>

   <span data-ttu-id="d6825-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d6825-130">For example:</span></span>

   ![Redigera villkor i grundläggande läge](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   <span data-ttu-id="d6825-132">Här är det fullständiga villkoret:</span><span class="sxs-lookup"><span data-stu-id="d6825-132">Here's the complete condition:</span></span>

   ![Slutförd](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > <span data-ttu-id="d6825-134">Om du vill ange villkoret i koden, Välj **redigera i Avancerat läge**.</span><span class="sxs-lookup"><span data-stu-id="d6825-134">To define the condition in code, choose **Edit in advanced mode**.</span></span> <span data-ttu-id="d6825-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d6825-135">For example:</span></span>
   > 
   > ![Redigera villkor i koden](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. <span data-ttu-id="d6825-137">Under **Ja om** och **om Nej**, lägga till stegen för att utföra baserat på om villkoret är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="d6825-137">Under **IF YES** and **IF NO**, add the steps to perform based on whether the condition is met.</span></span>

   <span data-ttu-id="d6825-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d6825-138">For example:</span></span>

   ![Villkor med Ja och inga sökvägar](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > <span data-ttu-id="d6825-140">Du kan dra befintliga åtgärder i den **Ja om** och **om Nej** sökvägar.</span><span class="sxs-lookup"><span data-stu-id="d6825-140">You can drag existing actions into the **IF YES** and **IF NO** paths.</span></span>

5. <span data-ttu-id="d6825-141">När du är klar sparar du din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6825-141">When you're done, save your logic app.</span></span>

<span data-ttu-id="d6825-142">Nu får du e-postmeddelanden endast när meddelandena uppfyller villkoret.</span><span class="sxs-lookup"><span data-stu-id="d6825-142">Now you get emails only when the posts meet your condition.</span></span>

## <a name="repeat-actions-over-a-list-with-foreach"></a><span data-ttu-id="d6825-143">Upprepa åtgärder via en lista med forEach</span><span class="sxs-lookup"><span data-stu-id="d6825-143">Repeat actions over a list with forEach</span></span>

<span data-ttu-id="d6825-144">ForEach-loop anger en matris om du vill upprepa åtgärden över.</span><span class="sxs-lookup"><span data-stu-id="d6825-144">The forEach loop specifies an array to repeat an action over.</span></span> <span data-ttu-id="d6825-145">Flödet misslyckas om den inte är en matris.</span><span class="sxs-lookup"><span data-stu-id="d6825-145">If it is not an array, the flow fails.</span></span> <span data-ttu-id="d6825-146">Om du har action1 som en matris med meddelanden och du vill skicka meddelandet, inkludera du den här forEach satsen i egenskaperna för åtgärden:`forEach : "@action('action1').outputs.messages"`</span><span class="sxs-lookup"><span data-stu-id="d6825-146">For example, if you have action1 that outputs an array of messages, and you want to send each message, you can include this forEach statement in the properties of your action: `forEach : "@action('action1').outputs.messages"`</span></span>

## <a name="edit-the-code-definition-for-a-logic-app"></a><span data-ttu-id="d6825-147">Redigera koden definitionen för en logikapp</span><span class="sxs-lookup"><span data-stu-id="d6825-147">Edit the code definition for a logic app</span></span>

<span data-ttu-id="d6825-148">Även om du har logik App Designer kan redigera du den kod som definierar en logikapp direkt.</span><span class="sxs-lookup"><span data-stu-id="d6825-148">Although you have the Logic App Designer, you can directly edit the code that defines a logic app.</span></span>

1. <span data-ttu-id="d6825-149">Välj på kommandofältet **vyn kod**.</span><span class="sxs-lookup"><span data-stu-id="d6825-149">On the command bar, choose **Code view**.</span></span>

    <span data-ttu-id="d6825-150">En fullständig redigerare öppnas och visar du redigerade definitionen.</span><span class="sxs-lookup"><span data-stu-id="d6825-150">A full editor opens and shows the definition you edited.</span></span>

    ![Kodvy](media/logic-apps-use-logic-app-features/codeview.png)

    <span data-ttu-id="d6825-152">Du kan kopiera och klistra in flera olika åtgärder i samma logikappen eller mellan logikappar i textredigeraren.</span><span class="sxs-lookup"><span data-stu-id="d6825-152">In the text editor, you can copy and paste any number of actions within the same logic app or between logic apps.</span></span> 
    <span data-ttu-id="d6825-153">Du kan också enkelt lägga till eller ta bort hela avsnitt från definitionen och du kan också dela definitioner med andra.</span><span class="sxs-lookup"><span data-stu-id="d6825-153">You can also easily add or remove entire sections from the definition, and you can also share definitions with others.</span></span>

2. <span data-ttu-id="d6825-154">Om du vill spara ändringarna, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="d6825-154">To save your edits, choose **Save**.</span></span>

## <a name="parameters"></a><span data-ttu-id="d6825-155">Parametrar</span><span class="sxs-lookup"><span data-stu-id="d6825-155">Parameters</span></span>

<span data-ttu-id="d6825-156">Vissa funktioner i Logic Apps är endast tillgängliga i kodvy, till exempel parametrar.</span><span class="sxs-lookup"><span data-stu-id="d6825-156">Some Logic Apps capabilities are available only in code view, for example, parameters.</span></span> <span data-ttu-id="d6825-157">Parametrar som gör det lättare att återanvända värden i hela din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6825-157">Parameters make it easy to reuse values throughout your logic app.</span></span> <span data-ttu-id="d6825-158">Om du har en e-postadress som du vill använda i flera åtgärder, bör du till exempel definiera den e-postadressen som en parameter.</span><span class="sxs-lookup"><span data-stu-id="d6825-158">For example, if you have an email address that you want use in several actions, you should define that email address as a parameter.</span></span>

<span data-ttu-id="d6825-159">Parametrarna är bra för att dra ut värden som troligen kommer att ändras mycket.</span><span class="sxs-lookup"><span data-stu-id="d6825-159">Parameters are good for pulling out values that you are likely to change a lot.</span></span> <span data-ttu-id="d6825-160">De är särskilt användbar när du vill åsidosätta parametrar i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="d6825-160">They are especially useful when you need to override parameters in different environments.</span></span> <span data-ttu-id="d6825-161">Information om hur du åsidosätter parametrar utifrån miljö finns [redigera logik app definitioner](../logic-apps/logic-apps-author-definitions.md) och [REST API-dokumentation](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="d6825-161">To learn how to override parameters based on environment, see [Author logic app definitions](../logic-apps/logic-apps-author-definitions.md) and [REST API documentation](https://docs.microsoft.com/rest/api/logic).</span></span>

<span data-ttu-id="d6825-162">Det här exemplet visar hur du uppdaterar din befintliga logikapp så att du kan använda parametrarna för frågetermen.</span><span class="sxs-lookup"><span data-stu-id="d6825-162">This example shows how to update your existing logic app so that you can use parameters for the query term.</span></span>

1. <span data-ttu-id="d6825-163">I kodvyn, hitta den `parameters : {}` objekt och lägger till en `currentFeedUrl` objekt:</span><span class="sxs-lookup"><span data-stu-id="d6825-163">In code view, find the `parameters : {}` object, and add a `currentFeedUrl` object:</span></span>

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. <span data-ttu-id="d6825-164">Gå till den `When_a_feed-item_is_published` åtgärd, hitta den `queries` avsnittet och Ersätt värdet för frågan med:`"feedUrl": "#@{parameters('currentFeedUrl')}"`</span><span class="sxs-lookup"><span data-stu-id="d6825-164">Go to the `When_a_feed-item_is_published` action, find the `queries` section, and replace the query value with : `"feedUrl": "#@{parameters('currentFeedUrl')}"`</span></span> 

    <span data-ttu-id="d6825-165">För att ansluta två eller flera strängar kan du också använda den `concat` funktion.</span><span class="sxs-lookup"><span data-stu-id="d6825-165">To join two or more strings, you can also use the `concat` function.</span></span> 
    <span data-ttu-id="d6825-166">Till exempel `"@concat('#',parameters('currentFeedUrl'))"` fungerar på samma sätt som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="d6825-166">For example, `"@concat('#',parameters('currentFeedUrl'))"` works the same as the above.</span></span>

3.  <span data-ttu-id="d6825-167">När du är klar väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="d6825-167">When you're done, choose **Save**.</span></span> 

    <span data-ttu-id="d6825-168">Nu kan du ändra webbplatsens RSS-feed genom att ange en annan URL till den `currentFeedURL` objekt.</span><span class="sxs-lookup"><span data-stu-id="d6825-168">Now you can change the website's RSS feed by passing a different URL through the `currentFeedURL` object.</span></span>

<span data-ttu-id="d6825-169">Lär dig mer om [hur du skapar logiken app definitioner](../logic-apps/logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="d6825-169">Learn more about [how to author logic app definitions](../logic-apps/logic-apps-author-definitions.md).</span></span>

## <a name="start-logic-app-workflows"></a><span data-ttu-id="d6825-170">Starta logik app arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="d6825-170">Start logic app workflows</span></span>

<span data-ttu-id="d6825-171">Har du olika alternativ för att starta arbetsflödet som definierats i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6825-171">You have different options for starting the workflow defined in your logic app.</span></span> <span data-ttu-id="d6825-172">Du kan alltid starta ett arbetsflöde på begäran i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="d6825-172">You can always start a workflow on-demand in the [Azure portal].</span></span>

### <a name="recurrence-triggers"></a><span data-ttu-id="d6825-173">Utlösare för upprepning</span><span class="sxs-lookup"><span data-stu-id="d6825-173">Recurrence triggers</span></span>

<span data-ttu-id="d6825-174">En utlösare för upprepning körs vid ett intervall som du anger.</span><span class="sxs-lookup"><span data-stu-id="d6825-174">A recurrence trigger runs at an interval that you specify.</span></span> <span data-ttu-id="d6825-175">När utlösaren har införa villkorslogik, avgör utlösaren om arbetsflödet måste köras.</span><span class="sxs-lookup"><span data-stu-id="d6825-175">When the trigger has conditional logic, the trigger determines whether the workflow needs to run.</span></span> <span data-ttu-id="d6825-176">En utlösare Anger att arbetsflödet ska köras genom att returnera en `200` statuskod.</span><span class="sxs-lookup"><span data-stu-id="d6825-176">A trigger indicates the workflow should run by returning a `200` status code.</span></span> <span data-ttu-id="d6825-177">När arbetsflödet inte behöver köra utlösaren returnerar en `202` statuskod.</span><span class="sxs-lookup"><span data-stu-id="d6825-177">When the workflow doesn't need to run, the trigger returns a `202` status code.</span></span>

### <a name="callback-using-rest-apis"></a><span data-ttu-id="d6825-178">Återanrop med hjälp av REST API: er</span><span class="sxs-lookup"><span data-stu-id="d6825-178">Callback using REST APIs</span></span>

<span data-ttu-id="d6825-179">Om du vill starta ett arbetsflöde, kan tjänster anropa en logik app slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d6825-179">To start a workflow, services can call a logic app endpoint.</span></span> <span data-ttu-id="d6825-180">Om du vill starta den här typen av logik app på begäran, Välj **kör nu** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="d6825-180">To start this kind of logic app on-demand, choose **Run now** on the command bar.</span></span> <span data-ttu-id="d6825-181">Se [starta arbetsflöden genom att anropa logik app slutpunkter som utlösare](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d6825-181">See [Start workflows by calling logic app endpoints as triggers](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<!-- Shared links -->
<span data-ttu-id="d6825-182">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="d6825-182">[Azure portal]: https://portal.azure.com</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6825-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6825-183">Next steps</span></span>

* [<span data-ttu-id="d6825-184">Växeln-instruktioner</span><span class="sxs-lookup"><span data-stu-id="d6825-184">Switch statements</span></span>](../logic-apps/logic-apps-switch-case.md) 
* [<span data-ttu-id="d6825-185">Loopar, omfattningar och debatching</span><span class="sxs-lookup"><span data-stu-id="d6825-185">Loops, scopes, and debatching</span></span>](../logic-apps/logic-apps-loops-and-scopes.md)
* [<span data-ttu-id="d6825-186">Skapa Logic App-definitioner</span><span class="sxs-lookup"><span data-stu-id="d6825-186">Author logic app definitions</span></span>](../logic-apps/logic-apps-author-definitions.md)