---
title: Azure Mobile Engagement Windows Universal SDK Integration | Microsoft Docs
description: "Windows Universal-integrering för SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: d616ad58156a19e89b3e106639a38df67cbd0abb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a><span data-ttu-id="42fd9-103">Windows Universal SDK-Integration för Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="42fd9-103">Windows Universal SDK Integration for Azure Mobile Engagement</span></span>
<span data-ttu-id="42fd9-104">Det här dokumentet beskriver alla integrering och konfiguration tillgängliga alternativ för Azure Mobile Engagement Windows Universal SDK.</span><span class="sxs-lookup"><span data-stu-id="42fd9-104">This document describes all the integration and configuration options available for the Azure Mobile Engagement Windows Universal SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42fd9-105">Krav</span><span class="sxs-lookup"><span data-stu-id="42fd9-105">Prerequisites</span></span>
<span data-ttu-id="42fd9-106">Innan du påbörjar de här självstudierna måste du först slutföra vår [15 minuter kursen](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="42fd9-106">Before starting this tutorial, you must first complete our [15-minute tutorial](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="advanced-features"></a><span data-ttu-id="42fd9-107">Avancerade funktioner</span><span class="sxs-lookup"><span data-stu-id="42fd9-107">Advanced features</span></span>
### <a name="reporting-features"></a><span data-ttu-id="42fd9-108">Funktioner för rapportering</span><span class="sxs-lookup"><span data-stu-id="42fd9-108">Reporting features</span></span>
<span data-ttu-id="42fd9-109">Du kan lägga till dessa funktioner:</span><span class="sxs-lookup"><span data-stu-id="42fd9-109">You can add these features:</span></span>

1. [<span data-ttu-id="42fd9-110">Avancerade alternativ för rapportering</span><span class="sxs-lookup"><span data-stu-id="42fd9-110">Advanced reporting options</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
2. [<span data-ttu-id="42fd9-111">Avancerade konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="42fd9-111">Advanced configuration options</span></span>](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a><span data-ttu-id="42fd9-112">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="42fd9-112">Notifications</span></span>
[<span data-ttu-id="42fd9-113">Hur du integrerar räckvidden (meddelanden) i din universella Windows-app</span><span class="sxs-lookup"><span data-stu-id="42fd9-113">How to integrate Reach (Notifications) in your Windows Universal app</span></span>](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a><span data-ttu-id="42fd9-114">Taggen planera implementering:</span><span class="sxs-lookup"><span data-stu-id="42fd9-114">Tag plan implementation:</span></span>
[<span data-ttu-id="42fd9-115">Hur du använder avancerade Mobile Engagement API-märkning i din universella Windows-app</span><span class="sxs-lookup"><span data-stu-id="42fd9-115">How to use the advanced Mobile Engagement tagging API in your Windows Universal app</span></span>](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a><span data-ttu-id="42fd9-116">Viktig information</span><span class="sxs-lookup"><span data-stu-id="42fd9-116">Release notes</span></span>
### <a name="341-11032016"></a><span data-ttu-id="42fd9-117">3.4.1 (11/03/2016)</span><span class="sxs-lookup"><span data-stu-id="42fd9-117">3.4.1 (11/03/2016)</span></span>

* <span data-ttu-id="42fd9-118">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="42fd9-118">Stability improvements.</span></span>

<span data-ttu-id="42fd9-119">Tidigare versioner finns i [slutföra viktig information](mobile-engagement-windows-store-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="42fd9-119">For earlier versions, see the [complete release notes](mobile-engagement-windows-store-release-notes.md)</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="42fd9-120">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="42fd9-120">Upgrade procedures</span></span>
<span data-ttu-id="42fd9-121">Om du redan har integrerat en äldre version av Engagement till programmet, måste du Tänk på följande när du uppgraderar SDK.</span><span class="sxs-lookup"><span data-stu-id="42fd9-121">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="42fd9-122">Om du har missat flera versioner av SDK kanske att följa flera procedurer.</span><span class="sxs-lookup"><span data-stu-id="42fd9-122">If you missed several versions of the SDK, you may have to follow several procedures.</span></span> <span data-ttu-id="42fd9-123">Se hela [uppgradera procedurer](mobile-engagement-windows-store-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="42fd9-123">See the complete [Upgrade Procedures](mobile-engagement-windows-store-upgrade-procedure.md).</span></span> <span data-ttu-id="42fd9-124">Till exempel om du migrerar från 0.10.1 till 0.11.0 måste du först Följ proceduren ”från 0.9.0 till 0.10.1” sedan proceduren ”från 0.10.1 till 0.11.0”.</span><span class="sxs-lookup"><span data-stu-id="42fd9-124">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

### <a name="from-330-to-340"></a><span data-ttu-id="42fd9-125">Från 3.3.0 till 3.4.0</span><span class="sxs-lookup"><span data-stu-id="42fd9-125">From 3.3.0 to 3.4.0</span></span>
#### <a name="test-logs"></a><span data-ttu-id="42fd9-126">Testa loggar</span><span class="sxs-lookup"><span data-stu-id="42fd9-126">Test logs</span></span>
<span data-ttu-id="42fd9-127">Konsolen loggar som genereras av SDK kan nu vara aktiverat/inaktiverat/filtreras.</span><span class="sxs-lookup"><span data-stu-id="42fd9-127">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="42fd9-128">Om du vill anpassa, uppdatera egenskapen `EngagementAgent.Instance.TestLogEnabled` till någon av värdena som är tillgängliga från den `EngagementTestLogLevel` uppräkning, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="42fd9-128">To customize, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the values available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a><span data-ttu-id="42fd9-129">Resurser</span><span class="sxs-lookup"><span data-stu-id="42fd9-129">Resources</span></span>
<span data-ttu-id="42fd9-130">Reach-överlägget har förbättrats.</span><span class="sxs-lookup"><span data-stu-id="42fd9-130">The Reach overlay has been improved.</span></span> <span data-ttu-id="42fd9-131">Det är en del av resurserna som SDK NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="42fd9-131">It is part of the SDK NuGet package resources.</span></span>

<span data-ttu-id="42fd9-132">Du kan välja om du vill behålla dina befintliga filer från mappen överlägget för dina resurser eller inte när du uppgraderar till den nya versionen av SDK:</span><span class="sxs-lookup"><span data-stu-id="42fd9-132">While upgrading to the new version of the SDK, you can choose whether you want to keep your existing files from the overlay folder of your resources or not:</span></span>

* <span data-ttu-id="42fd9-133">Om det tidigare överlägget fungerar för dig, eller du integrerar de `WebView` element manuellt, sedan kan du välja att behålla din avslutas filer, den kommer fortfarande att fungera.</span><span class="sxs-lookup"><span data-stu-id="42fd9-133">If the previous overlay is working for you, or you are integrating the `WebView` elements manually, then you can decide to keep your exiting files, it will still work.</span></span>
* <span data-ttu-id="42fd9-134">Om du vill uppdatera till det nya överlägget ersätta hela `overlay` mapp från dina resurser med den nya från SDK-paketet (UWP-appar: efter uppgraderingen måste du hämta mappen överlägget från % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="42fd9-134">To update to the new overlay, replace the whole `overlay` folder from your resources with the new one from the SDK package (UWP apps: after the upgrade, you can get the new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="42fd9-135">Med det nya överlägget skriver över alla anpassningar som gjorts på den tidigare versionen.</span><span class="sxs-lookup"><span data-stu-id="42fd9-135">Using the new overlay overwrites any customizations made on the previous version.</span></span>
> 
> 

### <a name="upgrade-from-older-versions"></a><span data-ttu-id="42fd9-136">Uppgradera från tidigare versioner</span><span class="sxs-lookup"><span data-stu-id="42fd9-136">Upgrade from older versions</span></span>
<span data-ttu-id="42fd9-137">Se [uppgradera procedurer](mobile-engagement-windows-store-upgrade-procedure.md)</span><span class="sxs-lookup"><span data-stu-id="42fd9-137">See [Upgrade Procedures](mobile-engagement-windows-store-upgrade-procedure.md)</span></span>

