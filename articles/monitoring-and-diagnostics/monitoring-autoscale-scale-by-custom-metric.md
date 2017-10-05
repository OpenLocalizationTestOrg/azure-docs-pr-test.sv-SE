---
title: "Kom igång med Autoskala efter anpassade mått i Azure | Microsoft Docs"
description: "Lär dig mer om att skala din resurs med anpassade mått i Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: de8f7acadc282e4b81c657b1723f00fd3e5fd4f2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a><span data-ttu-id="1acb2-103">Kom igång med Autoskala efter anpassade mått i Azure</span><span class="sxs-lookup"><span data-stu-id="1acb2-103">Get started with auto scale by custom metric in Azure</span></span>
<span data-ttu-id="1acb2-104">Den här artikeln beskriver hur du skala resursen med ett anpassat mått i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1acb2-104">This article describes how to scale your resource by a custom metric in Azure portal.</span></span>

<span data-ttu-id="1acb2-105">Azure övervakaren Autoskala gäller enbart för Virtual Machine Scale uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="1acb2-105">Azure Monitor auto scale applies only to Virtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="1acb2-106">Kan komma igång</span><span class="sxs-lookup"><span data-stu-id="1acb2-106">Lets get started</span></span>
<span data-ttu-id="1acb2-107">Den här artikeln förutsätter att du har ett webbprogram med application insights konfigureras.</span><span class="sxs-lookup"><span data-stu-id="1acb2-107">This article assumes that you have a web app with application insights configured.</span></span> <span data-ttu-id="1acb2-108">Om du inte har någon redan, kan du [konfigurera Application Insights för ASP.NET-webbplats][1]</span><span class="sxs-lookup"><span data-stu-id="1acb2-108">If you don't have one already, you can [set up Application Insights for your ASP.NET website][1]</span></span>

- <span data-ttu-id="1acb2-109">Öppna [Azure-portalen][2]</span><span class="sxs-lookup"><span data-stu-id="1acb2-109">Open [Azure portal][2]</span></span>
- <span data-ttu-id="1acb2-110">Klicka på ikonen för Azure i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="1acb2-110">Click on Azure Monitor icon in the left navigation pane.</span></span>
  <span data-ttu-id="1acb2-111">![Starta Azure-övervakning][3]</span><span class="sxs-lookup"><span data-stu-id="1acb2-111">![Launch Azure Monitor][3]</span></span>
- <span data-ttu-id="1acb2-112">Klicka på autoskalningsinställning som du vill visa alla resurser för vilka automatisk skalning är tillämpligt, tillsammans med dess aktuella status Autoskala ![identifiera Autoskala i Azure-monitor][4]</span><span class="sxs-lookup"><span data-stu-id="1acb2-112">Click on Autoscale setting to view all the resources for which auto scale is applicable, along with its current autoscale status ![Discover auto scale in Azure monitor][4]</span></span>
- <span data-ttu-id="1acb2-113">Öppna 'Autoskala-bladet i Azure-Monitor och välj en resurs som du vill skala</span><span class="sxs-lookup"><span data-stu-id="1acb2-113">Open 'Autoscale' blade in Azure Monitor and select a resource you want to scale</span></span>
> <span data-ttu-id="1acb2-114">Obs: Stegen nedan använder en app service-plan som är associerade med ett webbprogram som har app insights konfigureras.</span><span class="sxs-lookup"><span data-stu-id="1acb2-114">Note: The steps below use an app service plan associated with a web app that has app insights configured.</span></span>
- <span data-ttu-id="1acb2-115">Observera att den aktuella standardinstansantalet är 1 i bladet skala inställningen för resursen.</span><span class="sxs-lookup"><span data-stu-id="1acb2-115">In the scale setting blade for the resource, notice that the current instance count is 1.</span></span> <span data-ttu-id="1acb2-116">Klicka på ”Aktivera Autoskala'.</span><span class="sxs-lookup"><span data-stu-id="1acb2-116">Click on 'Enable autoscale'.</span></span>
  <span data-ttu-id="1acb2-117">![Skalinställningen för ny webbapp][5]</span><span class="sxs-lookup"><span data-stu-id="1acb2-117">![Scale setting for new web app][5]</span></span>
- <span data-ttu-id="1acb2-118">Ange ett namn för inställningen skala och klicka på ”Lägg till en regel”.</span><span class="sxs-lookup"><span data-stu-id="1acb2-118">Provide a name for the scale setting, and the click on "Add a rule".</span></span> <span data-ttu-id="1acb2-119">Lägg märke till regeln skalningsalternativ som öppnas som en kontext panel i den högra sidan.</span><span class="sxs-lookup"><span data-stu-id="1acb2-119">Notice the scale rule options that opens as a context pane in the right hand side.</span></span> <span data-ttu-id="1acb2-120">Som standard anges möjligheten att skala din instanser av 1 om CPU-percetage på resursen överskrider 70%.</span><span class="sxs-lookup"><span data-stu-id="1acb2-120">By default, it sets the option to scale your instance count by 1 if the CPU percetage of the resource exceeds 70%.</span></span> <span data-ttu-id="1acb2-121">Ändra mått källan längst upp till ”Application Insights”, Välj app insights-resurs i listrutan 'resurs- och väljer sedan anpassade mått baserade på som du vill skala.</span><span class="sxs-lookup"><span data-stu-id="1acb2-121">Change the metric source at the top to "Application Insights", select the app insights resource in the 'Resource' dropdown and then select the custom metric based on which you want to scale.</span></span>
  <span data-ttu-id="1acb2-122">![Skala med anpassade mått][6]</span><span class="sxs-lookup"><span data-stu-id="1acb2-122">![Scale by custom metric][6]</span></span>
- <span data-ttu-id="1acb2-123">Liknar steg ovan, lägga till en regel för skalan som skala och minska antalet skala med 1 om anpassade mått är under ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="1acb2-123">Similar to the step above, add a scale rule that will scale in and decrease the scale count by 1 if the custom metric is below a threshold.</span></span>
  <span data-ttu-id="1acb2-124">![Skala baserat på cpu][7]</span><span class="sxs-lookup"><span data-stu-id="1acb2-124">![Scale based on cpu][7]</span></span>
- <span data-ttu-id="1acb2-125">Ange den du instansen gränser.</span><span class="sxs-lookup"><span data-stu-id="1acb2-125">Set the you instance limits.</span></span> <span data-ttu-id="1acb2-126">Till exempel om du vill skala mellan 2 och 5 instanser beroende på de anpassade mått variationerna in minsta till '2', högsta '5' och 'default' till ' 2'</span><span class="sxs-lookup"><span data-stu-id="1acb2-126">For example, if you want to scale between 2-5 instances depending on the custom metric fluctuations, set 'minimum' to '2', 'maximum' to '5' and 'default' to '2'</span></span>
> <span data-ttu-id="1acb2-127">Obs: om det inte går att läsa resurs mått och den aktuella kapaciteten är lägre än standardkapaciteten sedan för att säkerställa tillgängligheten för resursen, Autoskala skalas ut till standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1acb2-127">Note: In case there is a problem reading the resource metrics and the current capacity is below the default capacity, then to ensure the availability of the resource, Autoscale will scale out to the default value.</span></span> <span data-ttu-id="1acb2-128">Om den aktuella kapaciteten redan är högre än standardkapaciteten, skalar inte Autoskala i.</span><span class="sxs-lookup"><span data-stu-id="1acb2-128">If the current capacity is already higher than default capacity, Autoscale will not scale in.</span></span>
- <span data-ttu-id="1acb2-129">Klicka på ”Save”</span><span class="sxs-lookup"><span data-stu-id="1acb2-129">Click on 'Save'</span></span>

<span data-ttu-id="1acb2-130">Grattis.</span><span class="sxs-lookup"><span data-stu-id="1acb2-130">Congratulations.</span></span> <span data-ttu-id="1acb2-131">Du kan nu nu har skapat din skala inställningen för Automatisk skala ditt webbprogram baserat på ett anpassat mått.</span><span class="sxs-lookup"><span data-stu-id="1acb2-131">You now now succesfully created your scale setting to auto scale your web app based on a custom metric.</span></span>

> <span data-ttu-id="1acb2-132">Obs: Samma steg kan användas för att komma igång med en rolltjänst för VMSS eller molnet.</span><span class="sxs-lookup"><span data-stu-id="1acb2-132">Note: The same steps are applicable to get started with a VMSS or cloud service role.</span></span>

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
