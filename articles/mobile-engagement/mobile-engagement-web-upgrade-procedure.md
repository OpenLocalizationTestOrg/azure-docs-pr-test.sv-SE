---
title: aaaAzure Mobile Engagement Web SDK uppgraderingsprocesser | Microsoft Docs
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
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
ms.openlocfilehash: a2df65904c6b56584ce6588ed26a9b79f3aa27ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a><span data-ttu-id="3a9e4-103">Azure Mobile Engagement Web SDK uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="3a9e4-103">Azure Mobile Engagement Web SDK upgrade procedures</span></span>
<span data-ttu-id="3a9e4-104">Om du redan har integrerat en tidigare version av hello Azure Mobile Engagement Web SDK i ditt webbprogram måste tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-104">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your web application, you need tooconsider hello following points when you upgrade hello SDK.</span></span>

<span data-ttu-id="3a9e4-105">Om du hoppade över flera versioner av hello Mobile Engagement Web SDK, kanske du måste toocomplete flera procedurer hello uppgraderingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-105">If you skipped multiple versions of hello Mobile Engagement Web SDK, you might need toocomplete several procedures during hello upgrade process.</span></span> <span data-ttu-id="3a9e4-106">Till exempel om du migrerar från 1.4.0 too1.6.0 första Följ hello procedurer tooupgrade från 1.4.0 too1.5.0.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-106">For example, if you migrate from 1.4.0 too1.6.0, first follow hello procedures tooupgrade from 1.4.0 too1.5.0.</span></span> <span data-ttu-id="3a9e4-107">Följ sedan hello procedurer tooupgrade 1.5.0 too1.6.0.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-107">Then, follow hello procedures tooupgrade from 1.5.0 too1.6.0.</span></span>

<span data-ttu-id="3a9e4-108">Oavsett vilken version som du uppgraderar från, ersätta en tidigare version av hello filen azure-engagement.js med hello senaste versionen av hello-fil.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-108">Whichever version you upgrade from, replace any earlier version of hello file azure-engagement.js with hello latest version of hello file.</span></span>

## <a name="upgrade-from-121-too200"></a><span data-ttu-id="3a9e4-109">Uppgradera från 1.2.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="3a9e4-109">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="3a9e4-110">Det här avsnittet beskrivs hur toomigrate en Mobile Engagement Web SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS, tooan Azure Mobile Engagement-app.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-110">This section describes how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="3a9e4-111">Om du migrerar från en tidigare version du finns hello Capptain webbplats toofirst migrera too1.2.1 och tillämpa sedan hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-111">If you are migrating from an earlier version, please consult hello Capptain website toofirst migrate too1.2.1, and then apply hello following procedures.</span></span>

<span data-ttu-id="3a9e4-112">Den här versionen av hello Mobile Engagement Web SDK stöder inte Samsung Smart TV, Opera TV, webOS eller hello Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-112">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a9e4-113">Capptain och Azure Mobile Engagement är inte hello samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-113">Capptain and Azure Mobile Engagement are not hello same service.</span></span> <span data-ttu-id="3a9e4-114">hello följande procedur visar hur endast toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-114">hello following procedure highlights only how toomigrate hello client app.</span></span> <span data-ttu-id="3a9e4-115">Migrera hello Mobile Engagement Web SDK i hello app kommer inte att migrera dina data från en Capptain tooa Mobile Engagement-server.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-115">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

### <a name="javascript-files"></a><span data-ttu-id="3a9e4-116">JavaScript-filer</span><span class="sxs-lookup"><span data-stu-id="3a9e4-116">JavaScript files</span></span>
<span data-ttu-id="3a9e4-117">Ersätt hello filen capptain-sdk.js med hello filen azure engagement.js och uppdatera din skript importerar på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-117">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

### <a name="remove-capptain-reach"></a><span data-ttu-id="3a9e4-118">Ta bort Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="3a9e4-118">Remove Capptain Reach</span></span>
<span data-ttu-id="3a9e4-119">Den här versionen av hello Mobile Engagement Web SDK stöder inte hello Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-119">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="3a9e4-120">Om du har integrerat Capptain Reach i ditt program måste tooremove den.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-120">If you integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="3a9e4-121">Ta bort hello nå CSS import från din sida och ta bort hello relaterade CSS-filen (capptain-reach.css, som standard).</span><span class="sxs-lookup"><span data-stu-id="3a9e4-121">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="3a9e4-122">Ta bort hello följer räckvidden resurser: hello hello varumärkesikon (capptain-meddelande-ikonen, som standard) och Stäng bild (capptain-close.png, som standard).</span><span class="sxs-lookup"><span data-stu-id="3a9e4-122">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="3a9e4-123">Ta bort hello nå Användargränssnittet för aviseringar via appen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-123">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="3a9e4-124">hello standardlayout ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="3a9e4-124">hello default layout looks like this:</span></span>

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

<span data-ttu-id="3a9e4-125">Ta bort hello nå Användargränssnittet för text- och meddelanden och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-125">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="3a9e4-126">hello standardlayout ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="3a9e4-126">hello default layout looks like this:</span></span>

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

<span data-ttu-id="3a9e4-127">Ta bort hello `reach` objekt från konfigurationen, om den finns.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-127">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="3a9e4-128">Det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="3a9e4-128">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="3a9e4-129">Ta bort alla andra Reach anpassning, till exempel.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-129">Remove any other Reach customization, such as categories.</span></span>

### <a name="remove-deprecated-apis"></a><span data-ttu-id="3a9e4-130">Ta bort föråldrade API: er</span><span class="sxs-lookup"><span data-stu-id="3a9e4-130">Remove deprecated APIs</span></span>
<span data-ttu-id="3a9e4-131">Vissa API: er från Capptain är föråldrade i hello Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-131">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="3a9e4-132">Ta bort alla anrop toohello följande API: er: `agent.connect`, `agent.disconnect`, `agent.pause`, och `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-132">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="3a9e4-133">Ta bort alla instanser av följande återanrop från konfigurationen Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, och `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-133">Remove any instances of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

### <a name="configuration"></a><span data-ttu-id="3a9e4-134">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="3a9e4-134">Configuration</span></span>
<span data-ttu-id="3a9e4-135">Mobile Engagement använder en anslutning tooconfigure SDK strängidentifierare, till exempel hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-135">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="3a9e4-136">Ersätt hello program-ID med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-136">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="3a9e4-137">Observera att globala hello-objekt för hello SDK konfiguration ändras från `capptain` för`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-137">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="3a9e4-138">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="3a9e4-138">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="3a9e4-139">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="3a9e4-139">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="3a9e4-140">hello anslutningssträngen för ditt program visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-140">hello connection string for your application is displayed in hello Azure Portal.</span></span>

### <a name="javascript-apis"></a><span data-ttu-id="3a9e4-141">JavaScript API: er</span><span class="sxs-lookup"><span data-stu-id="3a9e4-141">JavaScript APIs</span></span>
<span data-ttu-id="3a9e4-142">hello globala JavaScript-objekt `window.capptain` har bytt `window.azureEngagement` men du kan använda hello `window.engagement` alias för API-anrop.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-142">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="3a9e4-143">Du kan inte använda hello-alias toodefine hello SDK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-143">You can't use hello alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="3a9e4-144">Till exempel `capptain.deviceId` blir `engagement.deviceId`, `capptain.agent.startActivity` blir `engagement.agent.startActivity`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="3a9e4-144">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

