---
title: "aaaGet startas med automatisk skalning av anpassade mått i Azure | Microsoft Docs"
description: "Lär dig hur tooscale resurs efter anpassade mått i Azure."
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
ms.openlocfilehash: d3e268ec322698d0d367361cd9c156b21e0fb6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a><span data-ttu-id="7b96a-103">Kom igång med Autoskala efter anpassade mått i Azure</span><span class="sxs-lookup"><span data-stu-id="7b96a-103">Get started with auto scale by custom metric in Azure</span></span>
<span data-ttu-id="7b96a-104">Den här artikeln beskriver hur tooscale resurs av ett anpassat mått i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7b96a-104">This article describes how tooscale your resource by a custom metric in Azure portal.</span></span>

<span data-ttu-id="7b96a-105">Azure övervakaren Autoskala gäller endast tooVirtual datorn skala uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="7b96a-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="7b96a-106">Kan komma igång</span><span class="sxs-lookup"><span data-stu-id="7b96a-106">Lets get started</span></span>
<span data-ttu-id="7b96a-107">Den här artikeln förutsätter att du har ett webbprogram med application insights konfigureras.</span><span class="sxs-lookup"><span data-stu-id="7b96a-107">This article assumes that you have a web app with application insights configured.</span></span> <span data-ttu-id="7b96a-108">Om du inte har någon redan, kan du [konfigurera Application Insights för ASP.NET-webbplats][1]</span><span class="sxs-lookup"><span data-stu-id="7b96a-108">If you don't have one already, you can [set up Application Insights for your ASP.NET website][1]</span></span>

- <span data-ttu-id="7b96a-109">Öppna [Azure-portalen][2]</span><span class="sxs-lookup"><span data-stu-id="7b96a-109">Open [Azure portal][2]</span></span>
- <span data-ttu-id="7b96a-110">Klicka på ikonen för Azure i hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="7b96a-110">Click on Azure Monitor icon in hello left navigation pane.</span></span>
  <span data-ttu-id="7b96a-111">![Starta Azure-övervakning][3]</span><span class="sxs-lookup"><span data-stu-id="7b96a-111">![Launch Azure Monitor][3]</span></span>
- <span data-ttu-id="7b96a-112">Klicka på Autoskalningsinställning tooview alla hello resurser för vilka automatisk skalning är tillämpligt, tillsammans med dess aktuella status Autoskala ![identifiera Autoskala i Azure-monitor][4]</span><span class="sxs-lookup"><span data-stu-id="7b96a-112">Click on Autoscale setting tooview all hello resources for which auto scale is applicable, along with its current autoscale status ![Discover auto scale in Azure monitor][4]</span></span>
- <span data-ttu-id="7b96a-113">Öppna 'Autoskala-bladet i Azure-Monitor och välj en resurs du vill tooscale</span><span class="sxs-lookup"><span data-stu-id="7b96a-113">Open 'Autoscale' blade in Azure Monitor and select a resource you want tooscale</span></span>
> <span data-ttu-id="7b96a-114">Obs: hello stegen nedan använder en app service-plan som är associerade med ett webbprogram som har app insights konfigureras.</span><span class="sxs-lookup"><span data-stu-id="7b96a-114">Note: hello steps below use an app service plan associated with a web app that has app insights configured.</span></span>
- <span data-ttu-id="7b96a-115">Observera att hello aktuella standardinstansantalet är 1 i hello skala inställningen bladet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="7b96a-115">In hello scale setting blade for hello resource, notice that hello current instance count is 1.</span></span> <span data-ttu-id="7b96a-116">Klicka på ”Aktivera Autoskala'.</span><span class="sxs-lookup"><span data-stu-id="7b96a-116">Click on 'Enable autoscale'.</span></span>
  <span data-ttu-id="7b96a-117">![Skalinställningen för ny webbapp][5]</span><span class="sxs-lookup"><span data-stu-id="7b96a-117">![Scale setting for new web app][5]</span></span>
- <span data-ttu-id="7b96a-118">Ange ett namn för hello skala inställningen och hello klickar du på ”Lägg till en regel”.</span><span class="sxs-lookup"><span data-stu-id="7b96a-118">Provide a name for hello scale setting, and hello click on "Add a rule".</span></span> <span data-ttu-id="7b96a-119">Observera hello regeln skalningsalternativ som öppnas som en kontext panel i hello höger.</span><span class="sxs-lookup"><span data-stu-id="7b96a-119">Notice hello scale rule options that opens as a context pane in hello right hand side.</span></span> <span data-ttu-id="7b96a-120">Som standard anges hello alternativet tooscale din instans räkna med 1 om hello CPU percetage hello resurs överskrider 70%.</span><span class="sxs-lookup"><span data-stu-id="7b96a-120">By default, it sets hello option tooscale your instance count by 1 if hello CPU percetage of hello resource exceeds 70%.</span></span> <span data-ttu-id="7b96a-121">Ändra hello mått källa hello överst för ”Application Insights”, Välj hello app insights-resurs i hello-resursen' listrutan och välj sedan hello anpassat mått baserade som du vill använda tooscale.</span><span class="sxs-lookup"><span data-stu-id="7b96a-121">Change hello metric source at hello top too"Application Insights", select hello app insights resource in hello 'Resource' dropdown and then select hello custom metric based on which you want tooscale.</span></span>
  <span data-ttu-id="7b96a-122">![Skala med anpassade mått][6]</span><span class="sxs-lookup"><span data-stu-id="7b96a-122">![Scale by custom metric][6]</span></span>
- <span data-ttu-id="7b96a-123">Liknande toohello ovanstående steg, lägga till en scale-regel som ska skalas i och minska hello skalningsantalet med 1 om hello anpassat mått ligger under ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="7b96a-123">Similar toohello step above, add a scale rule that will scale in and decrease hello scale count by 1 if hello custom metric is below a threshold.</span></span>
  <span data-ttu-id="7b96a-124">![Skala baserat på cpu][7]</span><span class="sxs-lookup"><span data-stu-id="7b96a-124">![Scale based on cpu][7]</span></span>
- <span data-ttu-id="7b96a-125">Ange hello instansen av gränser.</span><span class="sxs-lookup"><span data-stu-id="7b96a-125">Set hello you instance limits.</span></span> <span data-ttu-id="7b96a-126">Till exempel om du vill tooscale mellan 2 och 5 instanser beroende på hello anpassade mått variationer, ange 'minsta' för '2', 'maximalt' för '5' och 'default' för 2</span><span class="sxs-lookup"><span data-stu-id="7b96a-126">For example, if you want tooscale between 2-5 instances depending on hello custom metric fluctuations, set 'minimum' too'2', 'maximum' too'5' and 'default' too'2'</span></span>
> <span data-ttu-id="7b96a-127">Obs: om det inte går att läsa hello resurs mått och hello aktuella kapaciteten understiger hello standardkapaciteten sedan tooensure hello tillgängligheten för hello resurs Autoskala kommer skala ut toohello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="7b96a-127">Note: In case there is a problem reading hello resource metrics and hello current capacity is below hello default capacity, then tooensure hello availability of hello resource, Autoscale will scale out toohello default value.</span></span> <span data-ttu-id="7b96a-128">Om hello aktuella kapacitet redan är högre än standardkapaciteten, skalar inte Autoskala i.</span><span class="sxs-lookup"><span data-stu-id="7b96a-128">If hello current capacity is already higher than default capacity, Autoscale will not scale in.</span></span>
- <span data-ttu-id="7b96a-129">Klicka på ”Save”</span><span class="sxs-lookup"><span data-stu-id="7b96a-129">Click on 'Save'</span></span>

<span data-ttu-id="7b96a-130">Grattis!</span><span class="sxs-lookup"><span data-stu-id="7b96a-130">Congratulations.</span></span> <span data-ttu-id="7b96a-131">Du kan nu nu har skapat din skala inställningen tooauto skala ditt webbprogram baserat på ett anpassat mått.</span><span class="sxs-lookup"><span data-stu-id="7b96a-131">You now now succesfully created your scale setting tooauto scale your web app based on a custom metric.</span></span>

> <span data-ttu-id="7b96a-132">Obs: hello likadant är tillämpliga tooget igång med en rolltjänst för VMSS eller molnet.</span><span class="sxs-lookup"><span data-stu-id="7b96a-132">Note: hello same steps are applicable tooget started with a VMSS or cloud service role.</span></span>

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
