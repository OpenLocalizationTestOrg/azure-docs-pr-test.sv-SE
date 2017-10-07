---
title: "aaaAzure översikt över Mobile Engagement Web SDK | Microsoft Docs"
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 9e60a232b5eb2c41c405041a88e09d7137563513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a><span data-ttu-id="08dd3-103">Azure Mobile Engagement Web SDK</span><span class="sxs-lookup"><span data-stu-id="08dd3-103">Azure Mobile Engagement Web SDK</span></span>
<span data-ttu-id="08dd3-104">Börja här för alla hello information om hur toointegrate Azure Mobile Engagement i en webbapp.</span><span class="sxs-lookup"><span data-stu-id="08dd3-104">Start here for all hello details about how toointegrate Azure Mobile Engagement in a web app.</span></span> <span data-ttu-id="08dd3-105">Om du vill att toogive den ett försök innan du kommer igång med din egen webbprogram finns våra [15 minuter kursen](mobile-engagement-web-app-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="08dd3-105">If you'd like toogive it a try before getting started with your own web app, see our [15-minute tutorial](mobile-engagement-web-app-get-started.md).</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="08dd3-106">Integration procedurer</span><span class="sxs-lookup"><span data-stu-id="08dd3-106">Integration procedures</span></span>
1. <span data-ttu-id="08dd3-107">Läs [hur toointegrate Mobile Engagement i ditt webbprogram](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="08dd3-107">Learn [how toointegrate Mobile Engagement in your web app](mobile-engagement-web-integrate-engagement.md).</span></span>
2. <span data-ttu-id="08dd3-108">Taggen planera implementeringen Läs [hur toouse hello avancerade Mobile Engagement API-märkning i webbappen](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="08dd3-108">For tag plan implementation, learn [how toouse hello advanced Mobile Engagement tagging API in your web app](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="release-notes"></a><span data-ttu-id="08dd3-109">Viktig information</span><span class="sxs-lookup"><span data-stu-id="08dd3-109">Release notes</span></span>
### <a name="202-10182016"></a><span data-ttu-id="08dd3-110">2.0.2 (10/18/2016)</span><span class="sxs-lookup"><span data-stu-id="08dd3-110">2.0.2 (10/18/2016)</span></span>
* <span data-ttu-id="08dd3-111">Fast krascher om privata bläddring (Safari).</span><span class="sxs-lookup"><span data-stu-id="08dd3-111">Fixed crash on private browsing (Safari).</span></span>
* <span data-ttu-id="08dd3-112">Fast krascher i webbläsare med cookies inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="08dd3-112">Fixed crash on browsers with cookies disabled.</span></span>

<span data-ttu-id="08dd3-113">Alla versioner finns hello [slutföra viktig information](mobile-engagement-web-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="08dd3-113">For all versions, please see hello [complete release notes](mobile-engagement-web-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="08dd3-114">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="08dd3-114">Upgrade procedures</span></span>
### <a name="upgrade-from-121-too200"></a><span data-ttu-id="08dd3-115">Uppgradera från 1.2.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="08dd3-115">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="08dd3-116">hello följande avsnitt beskrivs hur toomigrate en Mobile Engagement Web SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS, tooan Azure Mobile Engagement-app.</span><span class="sxs-lookup"><span data-stu-id="08dd3-116">hello following sections describe how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="08dd3-117">Om du migrerar från en tidigare version än 1.2.1, kontakta hello Capptain webbplats toomigrate too1.2.1 först och sedan använda hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="08dd3-117">If you are migrating from a version earlier than 1.2.1, please consult hello Capptain website toomigrate too1.2.1 first, and then apply hello following procedures.</span></span>

<span data-ttu-id="08dd3-118">Den här versionen av hello Mobile Engagement Web SDK stöder inte Samsung Smart TV, Opera TV, webOS eller hello Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-118">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08dd3-119">Capptain och Azure Mobile Engagement hello inte samma tjänst och hello följande procedurer Markera endast hur toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-119">Capptain and Azure Mobile Engagement are not hello same service, and hello following procedures highlight only how toomigrate hello client app.</span></span> <span data-ttu-id="08dd3-120">Migrera hello Mobile Engagement Web SDK i hello app kommer inte att migrera dina data från en Capptain tooa Mobile Engagement-server.</span><span class="sxs-lookup"><span data-stu-id="08dd3-120">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

#### <a name="javascript-files"></a><span data-ttu-id="08dd3-121">JavaScript-filer</span><span class="sxs-lookup"><span data-stu-id="08dd3-121">JavaScript files</span></span>
<span data-ttu-id="08dd3-122">Ersätt hello filen capptain-sdk.js med hello filen azure engagement.js och uppdatera din skript importerar på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="08dd3-122">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

#### <a name="remove-capptain-reach"></a><span data-ttu-id="08dd3-123">Ta bort Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="08dd3-123">Remove Capptain Reach</span></span>
<span data-ttu-id="08dd3-124">Den här versionen av hello Mobile Engagement Web SDK stöder inte hello Reach-funktionen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-124">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="08dd3-125">Om du har integrerat Capptain Reach i ditt program, måste du tooremove den.</span><span class="sxs-lookup"><span data-stu-id="08dd3-125">If you have integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="08dd3-126">Ta bort hello nå CSS import från din sida och ta bort hello relaterade CSS-filen (capptain-reach.css, som standard).</span><span class="sxs-lookup"><span data-stu-id="08dd3-126">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="08dd3-127">Ta bort hello följer räckvidden resurser: hello hello varumärkesikon (capptain-meddelande-ikonen, som standard) och Stäng bild (capptain-close.png, som standard).</span><span class="sxs-lookup"><span data-stu-id="08dd3-127">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="08dd3-128">Ta bort hello nå Användargränssnittet för aviseringar via appen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-128">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="08dd3-129">hello standardlayout ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="08dd3-129">hello default layout looks like this:</span></span>

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

<span data-ttu-id="08dd3-130">Ta bort hello nå Användargränssnittet för text- och meddelanden och avsökningar.</span><span class="sxs-lookup"><span data-stu-id="08dd3-130">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="08dd3-131">hello standardlayout ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="08dd3-131">hello default layout looks like this:</span></span>

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

<span data-ttu-id="08dd3-132">Ta bort hello `reach` objekt från konfigurationen, om den finns.</span><span class="sxs-lookup"><span data-stu-id="08dd3-132">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="08dd3-133">Det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="08dd3-133">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="08dd3-134">Ta bort alla andra Reach anpassning, till exempel.</span><span class="sxs-lookup"><span data-stu-id="08dd3-134">Remove any other Reach customization, such as categories.</span></span>

#### <a name="remove-deprecated-apis"></a><span data-ttu-id="08dd3-135">Ta bort föråldrade API: er</span><span class="sxs-lookup"><span data-stu-id="08dd3-135">Remove deprecated APIs</span></span>
<span data-ttu-id="08dd3-136">Vissa API: er från Capptain är föråldrade i hello Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="08dd3-136">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="08dd3-137">Ta bort alla anrop toohello följande API: er: `agent.connect`, `agent.disconnect`, `agent.pause`, och `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="08dd3-137">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="08dd3-138">Ta bort någon av följande återanrop från konfigurationen Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, och `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="08dd3-138">Remove any of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

#### <a name="configuration"></a><span data-ttu-id="08dd3-139">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="08dd3-139">Configuration</span></span>
<span data-ttu-id="08dd3-140">Mobile Engagement använder en anslutning tooconfigure SDK strängidentifierare, till exempel hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="08dd3-140">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="08dd3-141">Ersätt hello program-ID med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-141">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="08dd3-142">Observera att globala hello-objekt för hello SDK konfiguration ändras från `capptain` för`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="08dd3-142">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="08dd3-143">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="08dd3-143">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="08dd3-144">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="08dd3-144">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="08dd3-145">hello anslutningssträngen för ditt program visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-145">hello connection string for your application is displayed in hello Azure portal.</span></span>

#### <a name="javascript-apis"></a><span data-ttu-id="08dd3-146">JavaScript API: er</span><span class="sxs-lookup"><span data-stu-id="08dd3-146">JavaScript APIs</span></span>
<span data-ttu-id="08dd3-147">hello globala JavaScript-objekt `window.capptain` har bytt `window.azureEngagement`, men du kan använda hello `window.engagement` alias för API-anrop.</span><span class="sxs-lookup"><span data-stu-id="08dd3-147">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement`, but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="08dd3-148">Du kan inte använda den här alias toodefine hello SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="08dd3-148">You can't use this alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="08dd3-149">Till exempel `capptain.deviceId` blir `engagement.deviceId`, `capptain.agent.startActivity` blir `engagement.agent.startActivity`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="08dd3-149">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

<span data-ttu-id="08dd3-150">Om en tidigare version av hello Azure Mobile Engagement Web SDK har redan integrerats i programmet, Läs om [uppgradera procedurer](mobile-engagement-web-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="08dd3-150">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your application, please read about [upgrade procedures](mobile-engagement-web-upgrade-procedure.md).</span></span>

