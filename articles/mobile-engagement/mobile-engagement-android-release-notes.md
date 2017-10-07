---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a><span data-ttu-id="5c825-103">Viktig information</span><span class="sxs-lookup"><span data-stu-id="5c825-103">Release notes</span></span>

## <a name="431-07172017"></a><span data-ttu-id="5c825-104">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="5c825-104">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="5c825-105">Åtgärda en krasch sällan kan inträffa när du anropar `EngagementAgentUtils.isInDedicatedEngagementProcess`, som också används av hello `EngagementApplication` klass.</span><span class="sxs-lookup"><span data-stu-id="5c825-105">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

## <a name="430-06272017"></a><span data-ttu-id="5c825-106">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="5c825-106">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="5c825-107">8 stöd för Android (tidigare versioner av hello SDK inte fungerar på Android 8).</span><span class="sxs-lookup"><span data-stu-id="5c825-107">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="5c825-108">Inget mer beroende stödbibliotek.</span><span class="sxs-lookup"><span data-stu-id="5c825-108">No more dependency on support library.</span></span>
* <span data-ttu-id="5c825-109">Ta bort `EngagementFragmentActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="5c825-109">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="5c825-110">Förfallodatum för[bakgrund körning gränser](https://developer.android.com/preview/features/background.html) på Android 8 loggar i bakgrunden kan fördröjas tills hello användaren interagerar med hello enhet, detta kan påverka Push-kampanj **levererade** och **Systemmeddelande visas** statistik att försenas om hello enheten viloläge (hello-meddelande fortfarande visas, kommer ring och vibrerar i realtid utan problem).</span><span class="sxs-lookup"><span data-stu-id="5c825-110">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="5c825-111">Förfallodatum för[bakgrund plats gränser](https://developer.android.com/preview/features/background-location-limits.html), hello realtid platsen i bakgrunden kommer inte att uppdateras ofta på Android 8.</span><span class="sxs-lookup"><span data-stu-id="5c825-111">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

## <a name="424-03302017"></a><span data-ttu-id="5c825-112">4.2.4 (03/30/2017)</span><span class="sxs-lookup"><span data-stu-id="5c825-112">4.2.4 (03/30/2017)</span></span>
* <span data-ttu-id="5c825-113">Åtgärda meddelande i appen färger på Android 7 toobe hello samma som äldre versioner av Android.</span><span class="sxs-lookup"><span data-stu-id="5c825-113">Fix in-app notification text colors on Android 7 toobe hello same as older Android versions.</span></span>

## <a name="423-08102016"></a><span data-ttu-id="5c825-114">4.2.3 (08/10/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-114">4.2.3 (08/10/2016)</span></span>
* <span data-ttu-id="5c825-115">Ingen mer WIFI-Lås.</span><span class="sxs-lookup"><span data-stu-id="5c825-115">No more WIFI lock.</span></span>
* <span data-ttu-id="5c825-116">Åtgärda ett dödläge vid anrop av getDeviceId innan init (bugg introducerades i 4.2.0).</span><span class="sxs-lookup"><span data-stu-id="5c825-116">Fix a deadlock when calling getDeviceId before init (bug introduced in 4.2.0).</span></span>

## <a name="422-05172016"></a><span data-ttu-id="5c825-117">4.2.2 (05/17/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-117">4.2.2 (05/17/2016)</span></span>
* <span data-ttu-id="5c825-118">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-118">Stability improvements.</span></span>

## <a name="421-05102016"></a><span data-ttu-id="5c825-119">4.2.1 (05/10/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-119">4.2.1 (05/10/2016)</span></span>
* <span data-ttu-id="5c825-120">Säkerhet: inaktivera webbåtkomst visa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="5c825-120">Security: disable web view local file access.</span></span>
* <span data-ttu-id="5c825-121">Säkerhet: ta bort `EngagementPreferenceActivity` klass som utökar föråldrat och osäkra `PreferenceActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="5c825-121">Security: remove `EngagementPreferenceActivity` class that extends obsolete and unsecure `PreferenceActivity` class.</span></span>
* <span data-ttu-id="5c825-122">Säkerhet: reach aktiviteter är nu dokumenterat toouse `exported="false"`, den här flaggan kan också användas i tidigare versioner av SDK.</span><span class="sxs-lookup"><span data-stu-id="5c825-122">Security: reach activities are now documented toouse `exported="false"`, this flag can also be used in previous SDK versions.</span></span>

## <a name="420-03112016"></a><span data-ttu-id="5c825-123">4.2.0 (03/11/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-123">4.2.0 (03/11/2016)</span></span>
* <span data-ttu-id="5c825-124">hello SDK är nu licensierad under MIT.</span><span class="sxs-lookup"><span data-stu-id="5c825-124">hello SDK is now licensed under MIT.</span></span>
* <span data-ttu-id="5c825-125">Tillåt att ange en anpassad enhetsidentifierare till Initieringstid SDK.</span><span class="sxs-lookup"><span data-stu-id="5c825-125">Allow specifying a custom device identifier at SDK initialization time.</span></span>

## <a name="415-02012016"></a><span data-ttu-id="5c825-126">4.1.5 (02/01/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-126">4.1.5 (02/01/2016)</span></span>
* <span data-ttu-id="5c825-127">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-127">Stability improvements.</span></span>

## <a name="414-01262016"></a><span data-ttu-id="5c825-128">4.1.4 (01/26/2016)</span><span class="sxs-lookup"><span data-stu-id="5c825-128">4.1.4 (01/26/2016)</span></span>
* <span data-ttu-id="5c825-129">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-129">Stability improvements.</span></span>

## <a name="413-1292015"></a><span data-ttu-id="5c825-130">4.1.3 (12/9/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-130">4.1.3 (12/9/2015)</span></span>
* <span data-ttu-id="5c825-131">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-131">Stability improvements.</span></span>

## <a name="412-11252015"></a><span data-ttu-id="5c825-132">4.1.2 (11/25/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-132">4.1.2 (11/25/2015)</span></span>
* <span data-ttu-id="5c825-133">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-133">Stability improvements.</span></span>

## <a name="411-11042015"></a><span data-ttu-id="5c825-134">4.1.1 (11/04/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-134">4.1.1 (11/04/2015)</span></span>
* <span data-ttu-id="5c825-135">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-135">Stability improvements.</span></span>

## <a name="410-08252015"></a><span data-ttu-id="5c825-136">4.1.0 (08/25/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-136">4.1.0 (08/25/2015)</span></span>
* <span data-ttu-id="5c825-137">Hantera nya behörighetsmodellen för Android M.</span><span class="sxs-lookup"><span data-stu-id="5c825-137">Handle new permission model for Android M.</span></span>
* <span data-ttu-id="5c825-138">Kan nu konfigurera funktioner för plats vid körning i stället för `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="5c825-138">Can now configure location features at runtime instead of using  `AndroidManifest.xml`.</span></span>
* <span data-ttu-id="5c825-139">Åtgärda ett programfel behörighet: Om du använder `ACCESS_FINE_LOCATION`, sedan `ACCESS_COARSE_LOCATION` behövs inte längre.</span><span class="sxs-lookup"><span data-stu-id="5c825-139">Fix a permission bug: if you use `ACCESS_FINE_LOCATION`, then `ACCESS_COARSE_LOCATION` is not needed anymore.</span></span>
* <span data-ttu-id="5c825-140">Stabilitetsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="5c825-140">Stability improvements.</span></span>

## <a name="400-07062015"></a><span data-ttu-id="5c825-141">4.0.0 (07/06/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-141">4.0.0 (07/06/2015)</span></span>
* <span data-ttu-id="5c825-142">Interna protokollet ändras toomake analyser och push mer tillförlitlig.</span><span class="sxs-lookup"><span data-stu-id="5c825-142">Internal protocol changes toomake analytics and push more reliable.</span></span>
* <span data-ttu-id="5c825-143">Intern push (GCM/ADM) nu används också för i appen meddelanden så att du måste konfigurera hello native push-autentiseringsuppgifter för alla typer av push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="5c825-143">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>
* <span data-ttu-id="5c825-144">Åtgärda helheten meddelande: de var 10-endast tal visas efter att pushas.</span><span class="sxs-lookup"><span data-stu-id="5c825-144">Fix big picture notification: they were displayed only 10s after being pushed.</span></span>
* <span data-ttu-id="5c825-145">Åtgärda ett programfel i webbvyn: Klicka på en länk kördes också hello standard åtgärds-URL.</span><span class="sxs-lookup"><span data-stu-id="5c825-145">Fix a bug in web view: clicking on a link was also executing hello default action URL.</span></span>
* <span data-ttu-id="5c825-146">Åtgärda en sällsynt krasch relaterade toolocal lagringshantering.</span><span class="sxs-lookup"><span data-stu-id="5c825-146">Fix a rare crash related toolocal storage management.</span></span>
* <span data-ttu-id="5c825-147">Åtgärda dynamisk sträng konfigurationshantering.</span><span class="sxs-lookup"><span data-stu-id="5c825-147">Fix dynamic configuration string management.</span></span>
* <span data-ttu-id="5c825-148">Uppdatera LICENSAVTALET.</span><span class="sxs-lookup"><span data-stu-id="5c825-148">Update EULA.</span></span>

## <a name="300-02172015"></a><span data-ttu-id="5c825-149">3.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="5c825-149">3.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="5c825-150">Första versionen av Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="5c825-150">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="5c825-151">appId configuration ersätts av en sträng anslutningskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="5c825-151">appId configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="5c825-152">Ta bort API toosend och ta emot godtycklig XMPP meddelanden från valfri XMPP entiteter.</span><span class="sxs-lookup"><span data-stu-id="5c825-152">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="5c825-153">Ta bort API toosend och ta emot meddelanden mellan enheter.</span><span class="sxs-lookup"><span data-stu-id="5c825-153">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="5c825-154">Förbättringar av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="5c825-154">Security improvements.</span></span>
* <span data-ttu-id="5c825-155">Spårning av Google Play och SmartAd tas bort.</span><span class="sxs-lookup"><span data-stu-id="5c825-155">Google Play and SmartAd tracking removed.</span></span>

