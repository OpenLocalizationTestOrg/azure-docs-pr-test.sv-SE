---
title: "aaaWindows universella appar SDK innehåll"
description: "Läs mer om hello innehållet i hello Windows Universal-appar SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a8013d2433c0be62d737c8bc6e8360ed79bbe532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="d0354-103">Windows Universal SDK för appar innehåll</span><span class="sxs-lookup"><span data-stu-id="d0354-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="d0354-104">Det här dokumentet listar och beskriver hello innehåll som distribueras av hello SDK i ditt program.</span><span class="sxs-lookup"><span data-stu-id="d0354-104">This document lists and describes hello content deployed by hello SDK in your application.</span></span>

## <a name="hello-resources-folder"></a><span data-ttu-id="d0354-105">Hej `/Resources` mapp</span><span class="sxs-lookup"><span data-stu-id="d0354-105">hello `/Resources` folder</span></span>
<span data-ttu-id="d0354-106">Den här mappen innehåller alla hello-resurser som krävs för Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d0354-106">This folder contains all hello resources that Mobile Engagement needs.</span></span> <span data-ttu-id="d0354-107">Du kan också anpassa dem toofit din app.</span><span class="sxs-lookup"><span data-stu-id="d0354-107">You can also customize them toofit your app.</span></span>

* <span data-ttu-id="d0354-108">`EngagementConfiguration.xml`: Hej Mobile Engagement konfigurationsfil, det är där du kan anpassa Mobile Engagement-inställningar (Mobile Engagement anslutningssträngen, rapporten krascher...).</span><span class="sxs-lookup"><span data-stu-id="d0354-108">`EngagementConfiguration.xml` : hello Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="d0354-109">/ HTML-mappen</span><span class="sxs-lookup"><span data-stu-id="d0354-109">/html folder</span></span>
* <span data-ttu-id="d0354-110">`EngagementNotification.html`: hello `Notification` webbdesign visa html för sidhuvud i appen.</span><span class="sxs-lookup"><span data-stu-id="d0354-110">`EngagementNotification.html` : hello `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="d0354-111">`EngagementAnnouncement.html`: hello `Announcement` webbdesign visa html för app mellan enskilda muskler vyer.</span><span class="sxs-lookup"><span data-stu-id="d0354-111">`EngagementAnnouncement.html` : hello `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="d0354-112">/ Images mapp</span><span class="sxs-lookup"><span data-stu-id="d0354-112">/images folder</span></span>
* <span data-ttu-id="d0354-113">`EngagementIconNotification.png`: hello varumärkesikon som visas på hello till vänster i ett meddelande, ersätter det här objektet med varumärken-ikonen.</span><span class="sxs-lookup"><span data-stu-id="d0354-113">`EngagementIconNotification.png` : hello brand icon displayed at hello left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="d0354-114">`EngagementIconOk.png`: hello `Ok` ikon för hello reach innehållssidor för hello åtgärd eller validering knapp.</span><span class="sxs-lookup"><span data-stu-id="d0354-114">`EngagementIconOk.png` : hello `Ok` icon of hello reach content pages for hello action or validation button.</span></span>
* <span data-ttu-id="d0354-115">`EngagementIconNOK.png`: hello `NOK` ikonen som används när hello verifiering för hello reach innehållssidor är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="d0354-115">`EngagementIconNOK.png` : hello `NOK` icon used when hello validation button of hello reach content pages is disabled.</span></span>
* <span data-ttu-id="d0354-116">`EngagementIconClose.png`: hello `Close` ikonen för hello nå meddelanden och innehållet för hello Ignorera knappen.</span><span class="sxs-lookup"><span data-stu-id="d0354-116">`EngagementIconClose.png` : hello `Close` icon of hello reach notifications and contents for hello dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="d0354-117">/overlay mapp</span><span class="sxs-lookup"><span data-stu-id="d0354-117">/overlay folder</span></span>
* <span data-ttu-id="d0354-118">`EngagementPageOverlay.cs`: hello överlägget sidan ansvarar för att lägga till hello Engagement nå i appen UI tooits underordnade.</span><span class="sxs-lookup"><span data-stu-id="d0354-118">`EngagementPageOverlay.cs` : hello overlay page responsible for adding hello Engagement reach in-app UI tooits child.</span></span>

