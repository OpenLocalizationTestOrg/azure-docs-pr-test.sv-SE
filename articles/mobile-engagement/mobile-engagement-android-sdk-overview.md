---
title: "aaaAndroid SDK-Integration för Azure Mobile Engagement"
description: Beskriver hur toointegrate Azure Mobile Engagement SDK i Android-appar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0c63bfaf673abbda7ea498390f8282c43e2fb8df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a><span data-ttu-id="2ec41-103">Android SDK-Integration för Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="2ec41-103">Android SDK Integration for Azure Mobile Engagement</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ec41-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="2ec41-104">Universal Windows</span></span>](mobile-engagement-windows-store-sdk-overview.md)
> * [<span data-ttu-id="2ec41-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="2ec41-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-sdk-overview.md)
> * [<span data-ttu-id="2ec41-106">iOS</span><span class="sxs-lookup"><span data-stu-id="2ec41-106">iOS</span></span>](mobile-engagement-ios-sdk-overview.md)
> * [<span data-ttu-id="2ec41-107">Android</span><span class="sxs-lookup"><span data-stu-id="2ec41-107">Android</span></span>](mobile-engagement-android-sdk-overview.md)
> 
> 

<span data-ttu-id="2ec41-108">Det här dokumentet beskriver alla hello integrering och vilka konfigurationsalternativ tillgängliga för Azure Mobile Engagement Android SDK.</span><span class="sxs-lookup"><span data-stu-id="2ec41-108">This document describes all hello integration and configuration options available for Azure Mobile Engagement Android SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ec41-109">Krav</span><span class="sxs-lookup"><span data-stu-id="2ec41-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a><span data-ttu-id="2ec41-110">Avancerade funktioner</span><span class="sxs-lookup"><span data-stu-id="2ec41-110">Advanced Features</span></span>
### <a name="reporting-features"></a><span data-ttu-id="2ec41-111">Funktioner för rapportering</span><span class="sxs-lookup"><span data-stu-id="2ec41-111">Reporting Features</span></span>
<span data-ttu-id="2ec41-112">Du kan lägga till dessa funktioner:</span><span class="sxs-lookup"><span data-stu-id="2ec41-112">You can add these features:</span></span>

1. [<span data-ttu-id="2ec41-113">Avancerade alternativ för rapportering</span><span class="sxs-lookup"><span data-stu-id="2ec41-113">Advanced reporting options</span></span>](mobile-engagement-android-advanced-reporting.md)
2. [<span data-ttu-id="2ec41-114">Alternativ för rapportering</span><span class="sxs-lookup"><span data-stu-id="2ec41-114">Location Reporting options</span></span>](mobile-engagement-android-location-reporting.md)
3. [<span data-ttu-id="2ec41-115">Avancerade konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="2ec41-115">Advanced Configuration options</span></span>](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a><span data-ttu-id="2ec41-116">Meddelanden:</span><span class="sxs-lookup"><span data-stu-id="2ec41-116">Notifications:</span></span>
[<span data-ttu-id="2ec41-117">Hur toointegrate Reach (meddelanden) i din Android-app</span><span class="sxs-lookup"><span data-stu-id="2ec41-117">How toointegrate Reach (Notifications) in your Android app</span></span>](mobile-engagement-android-integrate-engagement-reach.md)

1. <span data-ttu-id="2ec41-118">Google Cloud Messaging (GCM): [hur tooIntegrate GCM med Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="2ec41-118">Google Cloud Messaging (GCM): [How tooIntegrate GCM with Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span></span>
2. <span data-ttu-id="2ec41-119">Amazon Device Messaging (ADM): [hur tooIntegrate ADM med Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="2ec41-119">Amazon Device Messaging (ADM): [How tooIntegrate ADM with Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span></span>

### <a name="tag-plan-implementation"></a><span data-ttu-id="2ec41-120">Taggen planera implementering:</span><span class="sxs-lookup"><span data-stu-id="2ec41-120">Tag plan implementation:</span></span>
[<span data-ttu-id="2ec41-121">Hur toouse hello avancerade Mobile Engagement API-märkning i din Android-app</span><span class="sxs-lookup"><span data-stu-id="2ec41-121">How toouse hello advanced Mobile Engagement tagging API in your Android app</span></span>](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a><span data-ttu-id="2ec41-122">Viktig information</span><span class="sxs-lookup"><span data-stu-id="2ec41-122">Release notes</span></span>

### <a name="431-07172017"></a><span data-ttu-id="2ec41-123">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="2ec41-123">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="2ec41-124">Åtgärda en krasch sällan kan inträffa när du anropar `EngagementAgentUtils.isInDedicatedEngagementProcess`, som också används av hello `EngagementApplication` klass.</span><span class="sxs-lookup"><span data-stu-id="2ec41-124">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

### <a name="430-06272017"></a><span data-ttu-id="2ec41-125">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="2ec41-125">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="2ec41-126">8 stöd för Android (tidigare versioner av hello SDK inte fungerar på Android 8).</span><span class="sxs-lookup"><span data-stu-id="2ec41-126">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="2ec41-127">Inget mer beroende stödbibliotek.</span><span class="sxs-lookup"><span data-stu-id="2ec41-127">No more dependency on support library.</span></span>
* <span data-ttu-id="2ec41-128">Ta bort `EngagementFragmentActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="2ec41-128">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="2ec41-129">Förfallodatum för[bakgrund körning gränser](https://developer.android.com/preview/features/background.html) på Android 8 loggar i bakgrunden kan fördröjas tills hello användaren interagerar med hello enhet, detta kan påverka Push-kampanj **levererade** och **Systemmeddelande visas** statistik att försenas om hello enheten viloläge (hello-meddelande fortfarande visas, kommer ring och vibrerar i realtid utan problem).</span><span class="sxs-lookup"><span data-stu-id="2ec41-129">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="2ec41-130">Förfallodatum för[bakgrund plats gränser](https://developer.android.com/preview/features/background-location-limits.html), hello realtid platsen i bakgrunden kommer inte att uppdateras ofta på Android 8.</span><span class="sxs-lookup"><span data-stu-id="2ec41-130">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

<span data-ttu-id="2ec41-131">Alla versioner finns hello [slutföra viktig information](mobile-engagement-android-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="2ec41-131">For all versions, see hello [complete release notes](mobile-engagement-android-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="2ec41-132">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="2ec41-132">Upgrade procedures</span></span>
<span data-ttu-id="2ec41-133">Om du redan har integrerat en äldre version av våra SDK i programmet, bör du konsultera [uppgradera procedurer](mobile-engagement-android-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="2ec41-133">If you already have integrated an older version of our SDK into your application, consult [Upgrade Procedures](mobile-engagement-android-upgrade-procedure.md).</span></span>

