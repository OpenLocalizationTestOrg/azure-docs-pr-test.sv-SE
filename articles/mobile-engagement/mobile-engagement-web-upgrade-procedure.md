---
title: Azure Mobile Engagement Web SDK uppgraderingsprocesser | Microsoft Docs
description: "De senaste uppdateringarna och procedurer för webbtjänst-SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: afa8037dcb7a53042fa606e2c4014b442d4be326
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a><span data-ttu-id="0b93e-103">Azure Mobile Engagement Web SDK uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="0b93e-103">Azure Mobile Engagement Web SDK upgrade procedures</span></span>
<span data-ttu-id="0b93e-104">Om du redan har integrerat en tidigare version av Azure Mobile Engagement Web SDK i ditt webbprogram som du behöver Tänk på följande när du uppgraderar SDK.</span><span class="sxs-lookup"><span data-stu-id="0b93e-104">If you have already integrated an earlier version of the Azure Mobile Engagement Web SDK into your web application, you need to consider the following points when you upgrade the SDK.</span></span>

<span data-ttu-id="0b93e-105">Om du hoppade över flera versioner av Mobile Engagement Web SDK, kanske du måste utföra flera procedurer under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-105">If you skipped multiple versions of the Mobile Engagement Web SDK, you might need to complete several procedures during the upgrade process.</span></span> <span data-ttu-id="0b93e-106">Till exempel om du migrerar från 1.4.0 till 1.6.0 först följer du procedurerna för att uppgradera från 1.4.0 till 1.5.0.</span><span class="sxs-lookup"><span data-stu-id="0b93e-106">For example, if you migrate from 1.4.0 to 1.6.0, first follow the procedures to upgrade from 1.4.0 to 1.5.0.</span></span> <span data-ttu-id="0b93e-107">Följ procedurerna för att uppgradera från 1.5.0 till 1.6.0.</span><span class="sxs-lookup"><span data-stu-id="0b93e-107">Then, follow the procedures to upgrade from 1.5.0 to 1.6.0.</span></span>

<span data-ttu-id="0b93e-108">Oavsett vilken version som du uppgraderar från, ersätta en tidigare version av filen azure-engagement.js med den senaste versionen av filen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-108">Whichever version you upgrade from, replace any earlier version of the file azure-engagement.js with the latest version of the file.</span></span>

## <a name="upgrade-from-121-to-200"></a><span data-ttu-id="0b93e-109">Uppgradera från 1.2.1 till 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0b93e-109">Upgrade from 1.2.1 to 2.0.0</span></span>
<span data-ttu-id="0b93e-110">Det här avsnittet beskrivs hur du migrerar en Mobile Engagement Web SDK-integration från tjänsten Capptain som erbjuds av Capptain SAS för en Azure Mobile Engagement-app.</span><span class="sxs-lookup"><span data-stu-id="0b93e-110">This section describes how to migrate a Mobile Engagement Web SDK integration from the Capptain service, offered by Capptain SAS, to an Azure Mobile Engagement app.</span></span> <span data-ttu-id="0b93e-111">Om du migrerar från en tidigare version, kontakta webbplatsen Capptain om du vill migrera först till 1.2.1 och tillämpa sedan följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="0b93e-111">If you are migrating from an earlier version, please consult the Capptain website to first migrate to 1.2.1, and then apply the following procedures.</span></span>

<span data-ttu-id="0b93e-112">Den här versionen av Mobile Engagement Web SDK stöder inte Samsung Smart TV, Opera TV, webOS eller Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-112">This version of the Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or the Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b93e-113">Capptain och Azure Mobile Engagement är inte samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="0b93e-113">Capptain and Azure Mobile Engagement are not the same service.</span></span> <span data-ttu-id="0b93e-114">Följande procedur visar endast hur du migrerar klientappen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-114">The following procedure highlights only how to migrate the client app.</span></span> <span data-ttu-id="0b93e-115">Migrera Mobile Engagement Web SDK i appen kommer inte migrera dina data från en Capptain server till en Mobile Engagement-server.</span><span class="sxs-lookup"><span data-stu-id="0b93e-115">Migrating the Mobile Engagement Web SDK in the app will not migrate your data from a Capptain server to a Mobile Engagement server.</span></span>
> 
> 

### <a name="javascript-files"></a><span data-ttu-id="0b93e-116">JavaScript-filer</span><span class="sxs-lookup"><span data-stu-id="0b93e-116">JavaScript files</span></span>
<span data-ttu-id="0b93e-117">Ersätt filen capptain-sdk.js med filen azure-engagement.js och uppdatera din skript importerar på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="0b93e-117">Replace the file capptain-sdk.js with the file azure-engagement.js, and then update your script imports accordingly.</span></span>

### <a name="remove-capptain-reach"></a><span data-ttu-id="0b93e-118">Ta bort Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="0b93e-118">Remove Capptain Reach</span></span>
<span data-ttu-id="0b93e-119">Den här versionen av Mobile Engagement Web SDK stöder inte Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-119">This version of the Mobile Engagement Web SDK doesn't support the Reach feature.</span></span> <span data-ttu-id="0b93e-120">Om du har integrerat Capptain Reach i ditt program måste du ta bort den.</span><span class="sxs-lookup"><span data-stu-id="0b93e-120">If you integrated Capptain Reach into your application, you need to remove it.</span></span>

<span data-ttu-id="0b93e-121">Ta bort nå CSS-import från din sida och ta bort relaterade CSS-filen (capptain-reach.css, som standard).</span><span class="sxs-lookup"><span data-stu-id="0b93e-121">Remove the Reach CSS import from your page and delete the related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="0b93e-122">Ta bort följande Reach-resurser: Stäng avbildningen (capptain-close.png, som standard) och ikonen varumärken (capptain-meddelande-ikonen, som standard).</span><span class="sxs-lookup"><span data-stu-id="0b93e-122">Delete the following Reach resources: the close image (capptain-close.png, by default) and the brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="0b93e-123">Ta bort nå Gränssnittet för aviseringar via appen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-123">Remove the Reach UI for in-app notifications.</span></span> <span data-ttu-id="0b93e-124">Standard ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0b93e-124">The default layout looks like this:</span></span>

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

<span data-ttu-id="0b93e-125">Ta bort nå Gränssnittet för text och web meddelanden och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="0b93e-125">Remove the Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="0b93e-126">Standard ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0b93e-126">The default layout looks like this:</span></span>

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

<span data-ttu-id="0b93e-127">Ta bort den `reach` objekt från konfigurationen, om den finns.</span><span class="sxs-lookup"><span data-stu-id="0b93e-127">Remove the `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="0b93e-128">Det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0b93e-128">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="0b93e-129">Ta bort alla andra Reach anpassning, till exempel.</span><span class="sxs-lookup"><span data-stu-id="0b93e-129">Remove any other Reach customization, such as categories.</span></span>

### <a name="remove-deprecated-apis"></a><span data-ttu-id="0b93e-130">Ta bort föråldrade API: er</span><span class="sxs-lookup"><span data-stu-id="0b93e-130">Remove deprecated APIs</span></span>
<span data-ttu-id="0b93e-131">Vissa API: er från Capptain är föråldrade i Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="0b93e-131">Some APIs from Capptain are deprecated in the Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="0b93e-132">Ta bort alla anrop till följande API: er: `agent.connect`, `agent.disconnect`, `agent.pause`, och `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="0b93e-132">Remove any calls to the following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="0b93e-133">Ta bort alla instanser av följande återanrop från konfigurationen Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, och `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="0b93e-133">Remove any instances of the following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

### <a name="configuration"></a><span data-ttu-id="0b93e-134">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="0b93e-134">Configuration</span></span>
<span data-ttu-id="0b93e-135">Mobile Engagement använder en anslutningssträng för att konfigurera SDK identifierare, t.ex, program-ID.</span><span class="sxs-lookup"><span data-stu-id="0b93e-135">Mobile Engagement uses a connection string to configure SDK identifiers, for example, the application identifier.</span></span>

<span data-ttu-id="0b93e-136">Ersätt det program-ID med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0b93e-136">Replace the application ID with your connection string.</span></span> <span data-ttu-id="0b93e-137">Observera att det globala objektet för SDK-konfiguration ändras från `capptain` till `azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="0b93e-137">Note that the global object for the SDK configuration changes from `capptain` to `azureEngagement`.</span></span>

<span data-ttu-id="0b93e-138">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="0b93e-138">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="0b93e-139">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="0b93e-139">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="0b93e-140">Anslutningssträngen för ditt program visas i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0b93e-140">The connection string for your application is displayed in the Azure Portal.</span></span>

### <a name="javascript-apis"></a><span data-ttu-id="0b93e-141">JavaScript API: er</span><span class="sxs-lookup"><span data-stu-id="0b93e-141">JavaScript APIs</span></span>
<span data-ttu-id="0b93e-142">Objektet global JavaScript `window.capptain` har bytt `window.azureEngagement` men du kan använda den `window.engagement` alias för API-anrop.</span><span class="sxs-lookup"><span data-stu-id="0b93e-142">The global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use the `window.engagement` alias for API calls.</span></span> <span data-ttu-id="0b93e-143">Du kan inte använda aliaset för att definiera SDK-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0b93e-143">You can't use the alias to define the SDK configuration.</span></span>

<span data-ttu-id="0b93e-144">Till exempel `capptain.deviceId` blir `engagement.deviceId`, `capptain.agent.startActivity` blir `engagement.agent.startActivity`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0b93e-144">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

