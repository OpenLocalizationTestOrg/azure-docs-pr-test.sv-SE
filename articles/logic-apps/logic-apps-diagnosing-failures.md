---
title: Diagnostisera fel - Azure Logic Apps | Microsoft Docs
description: "Vanliga sätt att förstå där logikappar misslyckas"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="edfee-103">Diagnostisera logik app fel</span><span class="sxs-lookup"><span data-stu-id="edfee-103">Diagnose logic app failures</span></span>
<span data-ttu-id="edfee-104">Om det uppstår problem eller fel med logic apps kan det inte finns några metoder kan hjälpa dig att bättre förstå var felen kommer från.</span><span class="sxs-lookup"><span data-stu-id="edfee-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where the failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="edfee-105">Azure portal-verktyg</span><span class="sxs-lookup"><span data-stu-id="edfee-105">Azure portal tools</span></span>
<span data-ttu-id="edfee-106">Azure-portalen innehåller många verktyg för att diagnostisera varje logikapp i varje steg.</span><span class="sxs-lookup"><span data-stu-id="edfee-106">The Azure portal provides many tools to diagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="edfee-107">Utlösaren historik</span><span class="sxs-lookup"><span data-stu-id="edfee-107">Trigger history</span></span>

<span data-ttu-id="edfee-108">Varje logikapp har minst en utlösare.</span><span class="sxs-lookup"><span data-stu-id="edfee-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="edfee-109">Om du märker att appar inte startar, leta i utlösaren historiken för mer information.</span><span class="sxs-lookup"><span data-stu-id="edfee-109">If you notice that apps aren't firing, look first at the trigger history for more information.</span></span> <span data-ttu-id="edfee-110">Du kan komma åt logik app'ss huvudbladet utlösaren historik.</span><span class="sxs-lookup"><span data-stu-id="edfee-110">You can access the trigger history on the logic app'ss main blade.</span></span>

![Hitta trigger-historik][1]

<span data-ttu-id="edfee-112">Utlösaren historiken visas alla utlösare försök som görs i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="edfee-112">The trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="edfee-113">Du kan klicka på varje utlösare försök att visa detaljer, särskilt, några indata eller utdata som genererats av utlösare försöket.</span><span class="sxs-lookup"><span data-stu-id="edfee-113">You can click each trigger attempt to drill into the details, specifically, any inputs or outputs that the trigger attempt generated.</span></span> <span data-ttu-id="edfee-114">Om du hittar misslyckade utlösare Markera försöket utlösare och välj den **utdata** länken för att granska alla genererade felmeddelanden, till exempel för FTP-autentiseringsuppgifter som inte är giltigt.</span><span class="sxs-lookup"><span data-stu-id="edfee-114">If you find failed triggers, select the trigger attempt and choose the **Outputs** link to review any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="edfee-115">De olika statuslägen som kan visas är:</span><span class="sxs-lookup"><span data-stu-id="edfee-115">The different statuses you might see are:</span></span>

* <span data-ttu-id="edfee-116">**Hoppade över**.</span><span class="sxs-lookup"><span data-stu-id="edfee-116">**Skipped**.</span></span> <span data-ttu-id="edfee-117">Slutpunkten har avsökts om du vill söka efter data och tog emot ett svar som att det fanns inga data.</span><span class="sxs-lookup"><span data-stu-id="edfee-117">The endpoint was polled to check for data and received a response that no data was available.</span></span>
* <span data-ttu-id="edfee-118">**Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="edfee-118">**Succeeded**.</span></span> <span data-ttu-id="edfee-119">Utlösaren tog emot ett svar var data tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="edfee-119">The trigger received a response that data was available.</span></span> <span data-ttu-id="edfee-120">Den här statusen kan bero på en manuell utlösare, en upprepning utlösare eller en avsökning utlösare.</span><span class="sxs-lookup"><span data-stu-id="edfee-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="edfee-121">Denna status är oftast den **Fired** status, men kanske inte om du har ett villkor eller SplitOn kommandot i kodvy som inte uppfylls.</span><span class="sxs-lookup"><span data-stu-id="edfee-121">This status is usually accompanied by the **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="edfee-122">**Det gick inte**.</span><span class="sxs-lookup"><span data-stu-id="edfee-122">**Failed**.</span></span> <span data-ttu-id="edfee-123">Ett fel har genererats.</span><span class="sxs-lookup"><span data-stu-id="edfee-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="edfee-124">Starta en utlösare manuellt</span><span class="sxs-lookup"><span data-stu-id="edfee-124">Start a trigger manually</span></span>

<span data-ttu-id="edfee-125">Om du vill att söka efter en tillgänglig utlösare omedelbart utan att vänta på nästa återkommande logikappen **Välj utlösare** på huvudbladet att tvinga en kontroll.</span><span class="sxs-lookup"><span data-stu-id="edfee-125">If you want the logic app to check for an available trigger immediately without waiting for the next recurrence, click **Select Trigger** on the main blade to force a check.</span></span> <span data-ttu-id="edfee-126">Till exempel gör klickar på länken med en Dropbox-utlösare att arbetsflödet att genast söka Dropbox för nya filer.</span><span class="sxs-lookup"><span data-stu-id="edfee-126">For example, clicking this link with a Dropbox trigger causes the workflow to immediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="edfee-127">Kör historik</span><span class="sxs-lookup"><span data-stu-id="edfee-127">Run history</span></span>

<span data-ttu-id="edfee-128">Varje Eldad utlösare resulterar i en körning.</span><span class="sxs-lookup"><span data-stu-id="edfee-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="edfee-129">Du kan komma åt information om körning från huvudblad som innehåller många detaljer som hjälper dig att förstå vad som hände under arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="edfee-129">You can access run information from the main blade, which contains many details that can help you understand what happened during the workflow.</span></span>

![Hitta körningshistoriken][2]

<span data-ttu-id="edfee-131">Körs visas något av följande status:</span><span class="sxs-lookup"><span data-stu-id="edfee-131">A run displays one of the following statuses:</span></span>

* <span data-ttu-id="edfee-132">**Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="edfee-132">**Succeeded**.</span></span> <span data-ttu-id="edfee-133">Alla åtgärder har slutförts.</span><span class="sxs-lookup"><span data-stu-id="edfee-133">All actions succeeded.</span></span> <span data-ttu-id="edfee-134">Om ett fel uppstod har felet hanterats av en åtgärd som inträffat senare i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="edfee-134">If a failure happened, that failure was handled by an action that occurred later in the workflow.</span></span> <span data-ttu-id="edfee-135">Det vill säga hanterades felet av en åtgärd som har angetts ska köras efter en misslyckad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="edfee-135">That is, the failure was handled by an action that was set to run after a failed action.</span></span>
* <span data-ttu-id="edfee-136">**Det gick inte**.</span><span class="sxs-lookup"><span data-stu-id="edfee-136">**Failed**.</span></span> <span data-ttu-id="edfee-137">Minst en åtgärd hade fel som inte hanterades av en åtgärd senare i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="edfee-137">At least one action had a failure that was not handled by an action later in the workflow.</span></span>
* <span data-ttu-id="edfee-138">**Avbröt**.</span><span class="sxs-lookup"><span data-stu-id="edfee-138">**Cancelled**.</span></span> <span data-ttu-id="edfee-139">Arbetsflödet kördes, men tog emot en begäran om avbrytning.</span><span class="sxs-lookup"><span data-stu-id="edfee-139">The workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="edfee-140">**Kör**.</span><span class="sxs-lookup"><span data-stu-id="edfee-140">**Running**.</span></span> <span data-ttu-id="edfee-141">Arbetsflödet körs.</span><span class="sxs-lookup"><span data-stu-id="edfee-141">The workflow is currently running.</span></span> <span data-ttu-id="edfee-142">Den här statusen kan uppstå för begränsad arbetsflöden eller på grund av den aktuella prisavtal.</span><span class="sxs-lookup"><span data-stu-id="edfee-142">This status might occur for throttled workflows, or because of the current pricing plan.</span></span> <span data-ttu-id="edfee-143">Mer information finns i [åtgärd gränser för prissättningssidan](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="edfee-143">For details, see [action limits on the pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="edfee-144">Konfigurera diagnostik (diagrammen som visas under körningshistoriken) kan också ge information om begränsning av händelser som inträffar.</span><span class="sxs-lookup"><span data-stu-id="edfee-144">Configuring diagnostics (the charts that appear under the run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="edfee-145">När du tittar på ett körningshistorik detaljer mer information.</span><span class="sxs-lookup"><span data-stu-id="edfee-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="edfee-146">Utlösaren utdata</span><span class="sxs-lookup"><span data-stu-id="edfee-146">Trigger outputs</span></span>

<span data-ttu-id="edfee-147">Utlösaren utdata visar data som kommer från utlösaren.</span><span class="sxs-lookup"><span data-stu-id="edfee-147">Trigger outputs show the data that came from the trigger.</span></span> <span data-ttu-id="edfee-148">Dessa utdata kan hjälpa dig att avgöra om alla egenskaper returneras som förväntat.</span><span class="sxs-lookup"><span data-stu-id="edfee-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="edfee-149">Om du ser allt innehåll som du inte förstår lär du dig hur Azure Logic Apps [hanterar olika typer av innehåll](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="edfee-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Utlösaren utdata-exempel][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="edfee-151">Åtgärden indata och utdata</span><span class="sxs-lookup"><span data-stu-id="edfee-151">Action inputs and outputs</span></span>

<span data-ttu-id="edfee-152">Du kan gå till in- och utgångar som en åtgärd togs emot.</span><span class="sxs-lookup"><span data-stu-id="edfee-152">You can drill into the inputs and outputs that an action received.</span></span> <span data-ttu-id="edfee-153">Informationen är användbar för att förstå storlek och form av utdata och för att söka efter eventuella felmeddelanden som kan ha genererats.</span><span class="sxs-lookup"><span data-stu-id="edfee-153">This data is useful for understanding the size and shape of the outputs, and also for finding any error messages that might have been generated.</span></span>

![Åtgärden indata och utdata][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="edfee-155">Felsöka arbetsflödets körtid</span><span class="sxs-lookup"><span data-stu-id="edfee-155">Debug workflow runtime</span></span>

<span data-ttu-id="edfee-156">Du kan lägga till några steg i ett arbetsflöde som hjälp med felsökning tillsammans med övervakning indata, utdata och utlösare av en körning.</span><span class="sxs-lookup"><span data-stu-id="edfee-156">Along with monitoring the inputs, outputs, and triggers of a run, you could add some steps to a workflow that help with debugging.</span></span> 
<span data-ttu-id="edfee-157">[RequestBin](http://requestb.in) är ett kraftfullt verktyg som du kan lägga till som ett steg i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="edfee-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="edfee-158">Genom att använda RequestBin kan ställa du in en HTTP-begäran inspector att fastställa exakt storlek, form och format för en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="edfee-158">By using RequestBin, you can set up an HTTP request inspector to determine the exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="edfee-159">Du kan skapa en RequestBin och klistra in Webbadressen i en logikapp HTTP POST-åtgärd med brödtextinnehåll som du vill testa, till exempel, ett uttryck eller en annan steg utdata.</span><span class="sxs-lookup"><span data-stu-id="edfee-159">You can create a RequestBin and paste the URL in a logic app HTTP POST action with body content that you want to test, for example, an expression or another step output.</span></span> <span data-ttu-id="edfee-160">När du kör logikappen kan uppdatera du din RequestBin om du vill se hur begäran bildades när genererats från Logic Apps-motorn.</span><span class="sxs-lookup"><span data-stu-id="edfee-160">After you run the logic app, you can refresh your RequestBin to see how the request was formed when generated from the Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
