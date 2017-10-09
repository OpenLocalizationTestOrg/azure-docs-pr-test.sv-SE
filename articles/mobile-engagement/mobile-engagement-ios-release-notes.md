---
title: aaaAzure Mobile Engagement iOS SDK viktig information | Microsoft Docs
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a><span data-ttu-id="ccd01-103">Azure Mobile Engagement iOS SDK viktig information</span><span class="sxs-lookup"><span data-stu-id="ccd01-103">Azure Mobile Engagement iOS SDK release notes</span></span>

## <a name="410-07172017"></a><span data-ttu-id="ccd01-104">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="ccd01-104">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="ccd01-105">Fast Aktivitetsikoner avmarkerad i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="ccd01-105">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="ccd01-106">Fast varningar på XCode 9 om API: er som inte anropas i huvudkön.</span><span class="sxs-lookup"><span data-stu-id="ccd01-106">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="ccd01-107">Fast en minnesläcka i Reach-avsökningar.</span><span class="sxs-lookup"><span data-stu-id="ccd01-107">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="ccd01-108">Bort stöd för iOS 6.X.</span><span class="sxs-lookup"><span data-stu-id="ccd01-108">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="ccd01-109">Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 7.</span><span class="sxs-lookup"><span data-stu-id="ccd01-109">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

## <a name="401-12132016"></a><span data-ttu-id="ccd01-110">4.0.1 (12/13/2016)</span><span class="sxs-lookup"><span data-stu-id="ccd01-110">4.0.1 (12/13/2016)</span></span>
* <span data-ttu-id="ccd01-111">Bättre loggen leverans i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="ccd01-111">Improved log delivery in background.</span></span>

## <a name="400-09122016"></a><span data-ttu-id="ccd01-112">4.0.0 (09/12/2016)</span><span class="sxs-lookup"><span data-stu-id="ccd01-112">4.0.0 (09/12/2016)</span></span>
* <span data-ttu-id="ccd01-113">Fast meddelande inte åtgärdade på iOS 10-enheter.</span><span class="sxs-lookup"><span data-stu-id="ccd01-113">Fixed notification not actioned on iOS 10 devices.</span></span>
* <span data-ttu-id="ccd01-114">Föråldrad XCode 7.</span><span class="sxs-lookup"><span data-stu-id="ccd01-114">Deprecate XCode 7.</span></span>

## <a name="324-06302016"></a><span data-ttu-id="ccd01-115">3.2.4 (06/30/2016)</span><span class="sxs-lookup"><span data-stu-id="ccd01-115">3.2.4 (06/30/2016)</span></span>
* <span data-ttu-id="ccd01-116">Fast aggregering mellan tekniska loggar och andra loggar.</span><span class="sxs-lookup"><span data-stu-id="ccd01-116">Fixed aggregation between technical logs and other logs.</span></span>

## <a name="323-06072016"></a><span data-ttu-id="ccd01-117">3.2.3 (06/07/2016)</span><span class="sxs-lookup"><span data-stu-id="ccd01-117">3.2.3 (06/07/2016)</span></span>
* <span data-ttu-id="ccd01-118">Fast hello bugg där leverans av feedback inte har rapporterats om appen hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="ccd01-118">Fixed hello bug where delivery feedback is not reported when app is in hello background.</span></span>
* <span data-ttu-id="ccd01-119">Optimerad hello skickar tekniska loggar.</span><span class="sxs-lookup"><span data-stu-id="ccd01-119">Optimized hello sending of technical logs.</span></span>

## <a name="322-04072016"></a><span data-ttu-id="ccd01-120">3.2.2 (04/07/2016)</span><span class="sxs-lookup"><span data-stu-id="ccd01-120">3.2.2 (04/07/2016)</span></span>
* <span data-ttu-id="ccd01-121">Fast programfel på http-begäran annullering, vilket ibland leder toocrash.</span><span class="sxs-lookup"><span data-stu-id="ccd01-121">Fixed bug on HTTP request cancellation which sometimes leads toocrash.</span></span>

## <a name="321-12112015"></a><span data-ttu-id="ccd01-122">3.2.1 (12/11/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-122">3.2.1 (12/11/2015)</span></span>
* <span data-ttu-id="ccd01-123">Fast hello fördröjning när en ny instans av app ska utlösas av ett meddelande med djuplänkar</span><span class="sxs-lookup"><span data-stu-id="ccd01-123">Fixed hello delay when a new app instance is triggered by a notification with deep links</span></span>

## <a name="320-10082015"></a><span data-ttu-id="ccd01-124">3.2.0 (10/08/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-124">3.2.0 (10/08/2015)</span></span>
* <span data-ttu-id="ccd01-125">Aktivera Bitcode i hello SDK toomake den fungerar med **Xcode 7**.</span><span class="sxs-lookup"><span data-stu-id="ccd01-125">Enabled Bitcode in hello SDK toomake it work with **Xcode 7**.</span></span>
* <span data-ttu-id="ccd01-126">Fast buggar relaterade tooin appen meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ccd01-126">Fixed bugs related tooin-app notifications.</span></span>
* <span data-ttu-id="ccd01-127">Gjort hello i appen meddelanden mer tillförlitlig vid låg batterinivå och andra sådana scenarier.</span><span class="sxs-lookup"><span data-stu-id="ccd01-127">Made hello in-app notifications more reliable in case of low battery and other such scenarios.</span></span>
* <span data-ttu-id="ccd01-128">Ta bort extra konsolen loggar som genereras av 3 part-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ccd01-128">Removed extra console logs generated by 3rd party library.</span></span>

## <a name="310-08262015"></a><span data-ttu-id="ccd01-129">3.1.0 (08/26/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-129">3.1.0 (08/26/2015)</span></span>
* <span data-ttu-id="ccd01-130">Åtgärda fel för iOS 9-kompatibilitet med tredjeparts-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ccd01-130">Fix iOS 9 compatibility bug with a third party library.</span></span> <span data-ttu-id="ccd01-131">Den orsakar krascher medan skickar avsöker resultat, information om programmet eller extra data.</span><span class="sxs-lookup"><span data-stu-id="ccd01-131">It was causing crashes while sending polls results, application information or extra data.</span></span>

## <a name="300-06192015"></a><span data-ttu-id="ccd01-132">3.0.0 (06/19/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-132">3.0.0 (06/19/2015)</span></span>
* <span data-ttu-id="ccd01-133">Mobile Engagement använder tysta Push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ccd01-133">Mobile Engagement uses Silent Push Notifications.</span></span>
* <span data-ttu-id="ccd01-134">Bort stöd för iOS 4.X.</span><span class="sxs-lookup"><span data-stu-id="ccd01-134">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="ccd01-135">Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 6.</span><span class="sxs-lookup"><span data-stu-id="ccd01-135">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

## <a name="220-05212015"></a><span data-ttu-id="ccd01-136">2.2.0 (05/21/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-136">2.2.0 (05/21/2015)</span></span>
* <span data-ttu-id="ccd01-137">hello Mobile Engagement enhets-id för enheter < iOS 6 nu baserat på ett GUID som genereras vid tidpunkten för installationen.</span><span class="sxs-lookup"><span data-stu-id="ccd01-137">hello Mobile Engagement device id for devices < iOS 6 is now based on a GUID generated at installation time.</span></span>

## <a name="210-04242015"></a><span data-ttu-id="ccd01-138">2.1.0 (04/24/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-138">2.1.0 (04/24/2015)</span></span>
* <span data-ttu-id="ccd01-139">Tillagda Swift kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ccd01-139">Added Swift compatibility.</span></span>
* <span data-ttu-id="ccd01-140">När du klickar på ett meddelande köra hello åtgärd URL: en är nu höger när hello-programmet öppnas.</span><span class="sxs-lookup"><span data-stu-id="ccd01-140">When clicking on a notification, hello action URL is now executed right after hello application is opened.</span></span>
* <span data-ttu-id="ccd01-141">Tillagda saknas huvudfilen i SDK-paketet.</span><span class="sxs-lookup"><span data-stu-id="ccd01-141">Added missing header file in SDK package.</span></span>
* <span data-ttu-id="ccd01-142">Ett problem har åtgärdats när hello Mobile Engagement krasch rapport har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="ccd01-142">Fixed an issue when hello Mobile Engagement crash reporter was disabled.</span></span>

## <a name="200-02172015"></a><span data-ttu-id="ccd01-143">2.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="ccd01-143">2.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="ccd01-144">Första versionen av Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="ccd01-144">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="ccd01-145">konfiguration av appId/sdkKey ersätts av en sträng anslutningskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="ccd01-145">appId/sdkKey configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="ccd01-146">Ta bort API toosend och ta emot godtycklig XMPP meddelanden från valfri XMPP entiteter.</span><span class="sxs-lookup"><span data-stu-id="ccd01-146">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="ccd01-147">Ta bort API toosend och ta emot meddelanden mellan enheter.</span><span class="sxs-lookup"><span data-stu-id="ccd01-147">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="ccd01-148">Förbättringar av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ccd01-148">Security improvements.</span></span>
* <span data-ttu-id="ccd01-149">Spårning av SmartAd tas bort.</span><span class="sxs-lookup"><span data-stu-id="ccd01-149">SmartAd tracking removed.</span></span>
