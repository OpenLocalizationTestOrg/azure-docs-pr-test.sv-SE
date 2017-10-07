---
title: "aaaAdvanced konfiguration för Windows Universal-appar Engagement SDK"
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
ms.openlocfilehash: 23bd05012bc25d438d8d4985a112280bed0292b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="668f5-103">Avancerad konfiguration för Windows Universal-appar Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="668f5-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="668f5-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="668f5-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="668f5-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="668f5-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="668f5-106">iOS</span><span class="sxs-lookup"><span data-stu-id="668f5-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="668f5-107">Android</span><span class="sxs-lookup"><span data-stu-id="668f5-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="668f5-108">Den här proceduren beskriver hur tooconfigure olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.</span><span class="sxs-lookup"><span data-stu-id="668f5-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="668f5-109">Krav</span><span class="sxs-lookup"><span data-stu-id="668f5-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="668f5-110">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="668f5-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="668f5-111">Inaktivera automatisk rapportering om krasch</span><span class="sxs-lookup"><span data-stu-id="668f5-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="668f5-112">Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement.</span><span class="sxs-lookup"><span data-stu-id="668f5-112">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="668f5-113">Sedan, när ett ohanterat undantag inträffar Engagement ingenting.</span><span class="sxs-lookup"><span data-stu-id="668f5-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="668f5-114">Om du inaktiverar den här funktionen, så när ett ohanterat kraschar i din app Engagement inte skicka hello krascher **och** stängs inte hello-session och jobb.</span><span class="sxs-lookup"><span data-stu-id="668f5-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send hello crash **AND** does not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="668f5-115">toodisable automatisk krascher rapportering, anpassa konfigurationen beroende på hello sätt som du har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="668f5-115">toodisable automatic crash reporting, customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="668f5-116">Från `EngagementConfiguration.xml` fil</span><span class="sxs-lookup"><span data-stu-id="668f5-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="668f5-117">Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.</span><span class="sxs-lookup"><span data-stu-id="668f5-117">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="668f5-118">Från `EngagementConfiguration` objekt vid körning.</span><span class="sxs-lookup"><span data-stu-id="668f5-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="668f5-119">Ange rapporten krascher toofalse med EngagementConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="668f5-119">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="668f5-120">Inaktivera rapportering i realtid</span><span class="sxs-lookup"><span data-stu-id="668f5-120">Disable real time reporting</span></span>
<span data-ttu-id="668f5-121">Som standard loggas hello Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="668f5-121">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="668f5-122">Om ditt program rapporterar loggar ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en vanlig tidsschema.</span><span class="sxs-lookup"><span data-stu-id="668f5-122">If your application reports logs frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time basis.</span></span> <span data-ttu-id="668f5-123">Detta kallas ”burst läge”.</span><span class="sxs-lookup"><span data-stu-id="668f5-123">This is called “burst mode”.</span></span>

<span data-ttu-id="668f5-124">toodo anropa så hello-metoden:</span><span class="sxs-lookup"><span data-stu-id="668f5-124">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="668f5-125">hello-argumentet är ett värde i **millisekunder**.</span><span class="sxs-lookup"><span data-stu-id="668f5-125">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="668f5-126">När du vill tooreactivate hello realtid loggning anropa hello metod utan någon parameter eller med hello 0-värde.</span><span class="sxs-lookup"><span data-stu-id="668f5-126">Whenever you want tooreactivate hello real-time logging, call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="668f5-127">Burst läge något ökar hello batterilivslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb varaktighet är avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="668f5-127">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="668f5-128">Vi rekommenderar att du använder ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="668f5-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="668f5-129">Sparade loggarna är begränsad too300 objekt.</span><span class="sxs-lookup"><span data-stu-id="668f5-129">Saved logs are limited too300 items.</span></span> <span data-ttu-id="668f5-130">Om skickar är för lång kan förlora du vissa loggar.</span><span class="sxs-lookup"><span data-stu-id="668f5-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="668f5-131">hello burst tröskelvärde får inte vara konfigurerade tooa perioden som är mindre än en sekund.</span><span class="sxs-lookup"><span data-stu-id="668f5-131">hello burst threshold cannot be configured tooa period less than one second.</span></span> <span data-ttu-id="668f5-132">Om du gör det visas en spårning med hello fel hello SDK och återställer automatiskt toohello standardvärdet noll sekunder.</span><span class="sxs-lookup"><span data-stu-id="668f5-132">If you do so, hello SDK shows a trace with hello error and automatically resets toohello default value, zero seconds.</span></span> <span data-ttu-id="668f5-133">Den här utlösare hello SDK tooreport hello loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="668f5-133">This triggers hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
