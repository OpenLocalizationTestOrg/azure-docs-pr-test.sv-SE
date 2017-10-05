---
title: "Avancerad konfiguration för Windows Universal-appar Engagement SDK"
description: "Avancerade konfigurationsalternativ för Azure Mobile Engagement med universella Windows-appar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="cc261-103">Avancerad konfiguration för Windows Universal-appar Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="cc261-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cc261-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="cc261-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="cc261-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="cc261-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="cc261-106">iOS</span><span class="sxs-lookup"><span data-stu-id="cc261-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="cc261-107">Android</span><span class="sxs-lookup"><span data-stu-id="cc261-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="cc261-108">Den här proceduren beskriver hur du konfigurerar olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.</span><span class="sxs-lookup"><span data-stu-id="cc261-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc261-109">Krav</span><span class="sxs-lookup"><span data-stu-id="cc261-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="cc261-110">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="cc261-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="cc261-111">Inaktivera automatisk rapportering om krasch</span><span class="sxs-lookup"><span data-stu-id="cc261-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="cc261-112">Du kan inaktivera automatisk kraschen reporting-funktionen i Engagement.</span><span class="sxs-lookup"><span data-stu-id="cc261-112">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="cc261-113">Sedan, när ett ohanterat undantag inträffar Engagement ingenting.</span><span class="sxs-lookup"><span data-stu-id="cc261-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="cc261-114">Om du inaktiverar den här funktionen, så när en ohanterad kraschar i din app Engagement inte skicka kraschen **och** inte stänga sessionen och jobb.</span><span class="sxs-lookup"><span data-stu-id="cc261-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send the crash **AND** does not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="cc261-115">Om du vill inaktivera automatisk rapportering om krasch anpassa konfigurationen beroende på hur du har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="cc261-115">To disable automatic crash reporting, customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="cc261-116">Från `EngagementConfiguration.xml` fil</span><span class="sxs-lookup"><span data-stu-id="cc261-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="cc261-117">Ange rapport-krascher till `false` mellan `<reportCrash>` och `</reportCrash>` taggar.</span><span class="sxs-lookup"><span data-stu-id="cc261-117">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="cc261-118">Från `EngagementConfiguration` objekt vid körning.</span><span class="sxs-lookup"><span data-stu-id="cc261-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="cc261-119">Ange rapport-krascher till false med EngagementConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="cc261-119">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="cc261-120">Inaktivera rapportering i realtid</span><span class="sxs-lookup"><span data-stu-id="cc261-120">Disable real time reporting</span></span>
<span data-ttu-id="cc261-121">Som standard loggas Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="cc261-121">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="cc261-122">Om ditt program rapporterar loggar ofta, är det bättre att buffra loggarna och rapportera dem på en gång på vanlig tid.</span><span class="sxs-lookup"><span data-stu-id="cc261-122">If your application reports logs frequently, it is better to buffer the logs and to report them all at once on a regular time basis.</span></span> <span data-ttu-id="cc261-123">Detta kallas ”burst läge”.</span><span class="sxs-lookup"><span data-stu-id="cc261-123">This is called “burst mode”.</span></span>

<span data-ttu-id="cc261-124">Det gör du genom att anropa metoden:</span><span class="sxs-lookup"><span data-stu-id="cc261-124">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="cc261-125">Argumentet är ett värde i **millisekunder**.</span><span class="sxs-lookup"><span data-stu-id="cc261-125">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="cc261-126">När du vill återaktivera realtid loggning kan du anropa metoden utan någon parameter eller med värdet 0.</span><span class="sxs-lookup"><span data-stu-id="cc261-126">Whenever you want to reactivate the real-time logging, call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="cc261-127">Burst läge något ökar för batterilivslängd men påverkar Engagement-Övervakare: alla sessioner och jobb varaktighet avrundas till burst tröskelvärdet (alltså sessioner och jobb som är kortare än tröskelvärdet burst inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="cc261-127">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="cc261-128">Vi rekommenderar att du använder ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="cc261-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="cc261-129">Sparade loggarna är begränsade till 300 objekt.</span><span class="sxs-lookup"><span data-stu-id="cc261-129">Saved logs are limited to 300 items.</span></span> <span data-ttu-id="cc261-130">Om skickar är för lång kan förlora du vissa loggar.</span><span class="sxs-lookup"><span data-stu-id="cc261-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="cc261-131">Kan inte konfigureras burst tröskelvärdet till en period som är mindre än en sekund.</span><span class="sxs-lookup"><span data-stu-id="cc261-131">The burst threshold cannot be configured to a period less than one second.</span></span> <span data-ttu-id="cc261-132">Om du gör det visas en spårning med fel SDK och återställer automatiskt till standardvärdet noll sekunder.</span><span class="sxs-lookup"><span data-stu-id="cc261-132">If you do so, the SDK shows a trace with the error and automatically resets to the default value, zero seconds.</span></span> <span data-ttu-id="cc261-133">Detta utlöser SDK om du vill rapportera loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="cc261-133">This triggers the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
