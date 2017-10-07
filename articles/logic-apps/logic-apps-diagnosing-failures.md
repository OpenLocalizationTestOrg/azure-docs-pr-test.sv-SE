---
title: "fel när aaaDiagnose: Azure Logic Apps | Microsoft Docs"
description: "Vanliga sätt toounderstand där logikappar misslyckas"
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
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="686fa-103">Diagnostisera logik app fel</span><span class="sxs-lookup"><span data-stu-id="686fa-103">Diagnose logic app failures</span></span>
<span data-ttu-id="686fa-104">Om det uppstår problem eller fel med logic apps kan det inte finns några metoder kan hjälpa dig att bättre förstå var hello fel kommer från.</span><span class="sxs-lookup"><span data-stu-id="686fa-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where hello failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="686fa-105">Azure portal-verktyg</span><span class="sxs-lookup"><span data-stu-id="686fa-105">Azure portal tools</span></span>
<span data-ttu-id="686fa-106">hello Azure-portalen innehåller många verktyg toodiagnose varje logikapp i varje steg.</span><span class="sxs-lookup"><span data-stu-id="686fa-106">hello Azure portal provides many tools toodiagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="686fa-107">Utlösaren historik</span><span class="sxs-lookup"><span data-stu-id="686fa-107">Trigger history</span></span>

<span data-ttu-id="686fa-108">Varje logikapp har minst en utlösare.</span><span class="sxs-lookup"><span data-stu-id="686fa-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="686fa-109">Om du märker att appar inte startar, titta först på hello utlösaren historik för mer information.</span><span class="sxs-lookup"><span data-stu-id="686fa-109">If you notice that apps aren't firing, look first at hello trigger history for more information.</span></span> <span data-ttu-id="686fa-110">Du kan komma åt hello utlösaren historik i hello logik app'ss huvudblad.</span><span class="sxs-lookup"><span data-stu-id="686fa-110">You can access hello trigger history on hello logic app'ss main blade.</span></span>

![Hitta hello utlösaren historik][1]

<span data-ttu-id="686fa-112">hello utlösaren Historik visar alla utlösare försök som görs i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="686fa-112">hello trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="686fa-113">Du kan klicka på varje utlösare försök toodrill hello detaljer specifikt, någon inmatningar eller utdata som hello utlösaren försök skapas.</span><span class="sxs-lookup"><span data-stu-id="686fa-113">You can click each trigger attempt toodrill into hello details, specifically, any inputs or outputs that hello trigger attempt generated.</span></span> <span data-ttu-id="686fa-114">Om du hittar misslyckade utlösare Markera hello utlösaren försök och välj hello **utdata** länka tooreview eventuella genererade felmeddelanden, till exempel FTP-autentiseringsuppgifter som inte är giltigt.</span><span class="sxs-lookup"><span data-stu-id="686fa-114">If you find failed triggers, select hello trigger attempt and choose hello **Outputs** link tooreview any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="686fa-115">hello olika statusar som visas är:</span><span class="sxs-lookup"><span data-stu-id="686fa-115">hello different statuses you might see are:</span></span>

* <span data-ttu-id="686fa-116">**Hoppade över**.</span><span class="sxs-lookup"><span data-stu-id="686fa-116">**Skipped**.</span></span> <span data-ttu-id="686fa-117">hello endpoint var Avsökt toocheck för data och tog emot ett svar som att det fanns inga data.</span><span class="sxs-lookup"><span data-stu-id="686fa-117">hello endpoint was polled toocheck for data and received a response that no data was available.</span></span>
* <span data-ttu-id="686fa-118">**Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="686fa-118">**Succeeded**.</span></span> <span data-ttu-id="686fa-119">hello utlösaren emot svaret var data tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="686fa-119">hello trigger received a response that data was available.</span></span> <span data-ttu-id="686fa-120">Den här statusen kan bero på en manuell utlösare, en upprepning utlösare eller en avsökning utlösare.</span><span class="sxs-lookup"><span data-stu-id="686fa-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="686fa-121">Denna status är oftast hello **Fired** status, men kanske inte om du har ett villkor eller SplitOn kommandot i kodvy som inte uppfylls.</span><span class="sxs-lookup"><span data-stu-id="686fa-121">This status is usually accompanied by hello **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="686fa-122">**Det gick inte**.</span><span class="sxs-lookup"><span data-stu-id="686fa-122">**Failed**.</span></span> <span data-ttu-id="686fa-123">Ett fel har genererats.</span><span class="sxs-lookup"><span data-stu-id="686fa-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="686fa-124">Starta en utlösare manuellt</span><span class="sxs-lookup"><span data-stu-id="686fa-124">Start a trigger manually</span></span>

<span data-ttu-id="686fa-125">Om du vill hello logik app toocheck för en utlösare som är tillgängliga omedelbart utan att vänta på hello nästa återkommande **Välj utlösare** på hello huvudblad tooforce en kontroll.</span><span class="sxs-lookup"><span data-stu-id="686fa-125">If you want hello logic app toocheck for an available trigger immediately without waiting for hello next recurrence, click **Select Trigger** on hello main blade tooforce a check.</span></span> <span data-ttu-id="686fa-126">Till exempel gör klickar på länken med en Dropbox-utlösare hello arbetsflöde tooimmediately avsökning Dropbox för nya filer.</span><span class="sxs-lookup"><span data-stu-id="686fa-126">For example, clicking this link with a Dropbox trigger causes hello workflow tooimmediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="686fa-127">Kör historik</span><span class="sxs-lookup"><span data-stu-id="686fa-127">Run history</span></span>

<span data-ttu-id="686fa-128">Varje Eldad utlösare resulterar i en körning.</span><span class="sxs-lookup"><span data-stu-id="686fa-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="686fa-129">Du kan komma åt information om körning från hello huvudblad som innehåller många detaljer som hjälper dig att förstå vad som hände under hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="686fa-129">You can access run information from hello main blade, which contains many details that can help you understand what happened during hello workflow.</span></span>

![Hitta hello kör historik][2]

<span data-ttu-id="686fa-131">Körs visas något av följande statusar hello:</span><span class="sxs-lookup"><span data-stu-id="686fa-131">A run displays one of hello following statuses:</span></span>

* <span data-ttu-id="686fa-132">**Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="686fa-132">**Succeeded**.</span></span> <span data-ttu-id="686fa-133">Alla åtgärder har slutförts.</span><span class="sxs-lookup"><span data-stu-id="686fa-133">All actions succeeded.</span></span> <span data-ttu-id="686fa-134">Om ett fel uppstod har felet hanterats av en åtgärd som inträffat senare i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="686fa-134">If a failure happened, that failure was handled by an action that occurred later in hello workflow.</span></span> <span data-ttu-id="686fa-135">Det vill säga hanterades hello fel av en åtgärd som har ställts in toorun efter en misslyckad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="686fa-135">That is, hello failure was handled by an action that was set toorun after a failed action.</span></span>
* <span data-ttu-id="686fa-136">**Det gick inte**.</span><span class="sxs-lookup"><span data-stu-id="686fa-136">**Failed**.</span></span> <span data-ttu-id="686fa-137">Minst en åtgärd hade fel som inte hanterades av en åtgärd senare i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="686fa-137">At least one action had a failure that was not handled by an action later in hello workflow.</span></span>
* <span data-ttu-id="686fa-138">**Avbröt**.</span><span class="sxs-lookup"><span data-stu-id="686fa-138">**Cancelled**.</span></span> <span data-ttu-id="686fa-139">hello arbetsflödet kördes, men tog emot en begäran om avbrytning.</span><span class="sxs-lookup"><span data-stu-id="686fa-139">hello workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="686fa-140">**Kör**.</span><span class="sxs-lookup"><span data-stu-id="686fa-140">**Running**.</span></span> <span data-ttu-id="686fa-141">hello arbetsflödet körs.</span><span class="sxs-lookup"><span data-stu-id="686fa-141">hello workflow is currently running.</span></span> <span data-ttu-id="686fa-142">Den här statusen kan uppstå för begränsad arbetsflöden eller på grund av hello aktuella priser plan.</span><span class="sxs-lookup"><span data-stu-id="686fa-142">This status might occur for throttled workflows, or because of hello current pricing plan.</span></span> <span data-ttu-id="686fa-143">Mer information finns i [åtgärd gränserna för hello sida med priser](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="686fa-143">For details, see [action limits on hello pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="686fa-144">Konfigurera diagnostik (hello diagrammen som visas under hello kör historik) kan också ge information om begränsning av händelser som inträffar.</span><span class="sxs-lookup"><span data-stu-id="686fa-144">Configuring diagnostics (hello charts that appear under hello run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="686fa-145">När du tittar på ett körningshistorik detaljer mer information.</span><span class="sxs-lookup"><span data-stu-id="686fa-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="686fa-146">Utlösaren utdata</span><span class="sxs-lookup"><span data-stu-id="686fa-146">Trigger outputs</span></span>

<span data-ttu-id="686fa-147">Utlösaren matar ut visa hello data som kommer från hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="686fa-147">Trigger outputs show hello data that came from hello trigger.</span></span> <span data-ttu-id="686fa-148">Dessa utdata kan hjälpa dig att avgöra om alla egenskaper returneras som förväntat.</span><span class="sxs-lookup"><span data-stu-id="686fa-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="686fa-149">Om du ser allt innehåll som du inte förstår lär du dig hur Azure Logic Apps [hanterar olika typer av innehåll](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="686fa-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Utlösaren utdata-exempel][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="686fa-151">Åtgärden indata och utdata</span><span class="sxs-lookup"><span data-stu-id="686fa-151">Action inputs and outputs</span></span>

<span data-ttu-id="686fa-152">Du kan detaljerat hello in- och utgångar som en åtgärd togs emot.</span><span class="sxs-lookup"><span data-stu-id="686fa-152">You can drill into hello inputs and outputs that an action received.</span></span> <span data-ttu-id="686fa-153">Informationen är användbar för att förstå hello storlek och form hello utdata och för att söka efter eventuella felmeddelanden som kan ha genererats.</span><span class="sxs-lookup"><span data-stu-id="686fa-153">This data is useful for understanding hello size and shape of hello outputs, and also for finding any error messages that might have been generated.</span></span>

![Åtgärden indata och utdata][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="686fa-155">Felsöka arbetsflödets körtid</span><span class="sxs-lookup"><span data-stu-id="686fa-155">Debug workflow runtime</span></span>

<span data-ttu-id="686fa-156">Tillsammans med övervakning hello indata, utdata och utlösare av en körning, kan du lägga till några steg tooa arbetsflöde som hjälp med felsökning.</span><span class="sxs-lookup"><span data-stu-id="686fa-156">Along with monitoring hello inputs, outputs, and triggers of a run, you could add some steps tooa workflow that help with debugging.</span></span> 
<span data-ttu-id="686fa-157">[RequestBin](http://requestb.in) är ett kraftfullt verktyg som du kan lägga till som ett steg i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="686fa-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="686fa-158">Du kan konfigurera en HTTP-begäran inspector toodetermine hello exakt storlek, form och format för en HTTP-begäran med hjälp av RequestBin.</span><span class="sxs-lookup"><span data-stu-id="686fa-158">By using RequestBin, you can set up an HTTP request inspector toodetermine hello exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="686fa-159">Du kan skapa en RequestBin och klistra in hello URL i en logikapp HTTP POST-åtgärd med brödtextinnehåll som du vill tootest, till exempel, ett uttryck eller en annan steg utdata.</span><span class="sxs-lookup"><span data-stu-id="686fa-159">You can create a RequestBin and paste hello URL in a logic app HTTP POST action with body content that you want tootest, for example, an expression or another step output.</span></span> <span data-ttu-id="686fa-160">När du har kört hello logikapp kan du uppdatera din RequestBin toosee hur hello begäran bildades när genererats från hello Logic Apps-motorn.</span><span class="sxs-lookup"><span data-stu-id="686fa-160">After you run hello logic app, you can refresh your RequestBin toosee how hello request was formed when generated from hello Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
