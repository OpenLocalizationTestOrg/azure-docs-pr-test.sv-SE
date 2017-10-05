---
title: "Windows Universal SDK för appar innehåll"
description: "Läs mer om innehållet i Windows Universal-appar SDK för Azure Mobile Engagement"
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
ms.openlocfilehash: b28d525ab16487b963772e23fdecd11f94dcabd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="ed997-103">Windows Universal SDK för appar innehåll</span><span class="sxs-lookup"><span data-stu-id="ed997-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="ed997-104">Det här dokumentet listar och beskriver det innehåll som distribueras av SDK i ditt program.</span><span class="sxs-lookup"><span data-stu-id="ed997-104">This document lists and describes the content deployed by the SDK in your application.</span></span>

## <a name="the-resources-folder"></a><span data-ttu-id="ed997-105">Den `/Resources` mapp</span><span class="sxs-lookup"><span data-stu-id="ed997-105">The `/Resources` folder</span></span>
<span data-ttu-id="ed997-106">Den här mappen innehåller alla resurser som krävs för Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="ed997-106">This folder contains all the resources that Mobile Engagement needs.</span></span> <span data-ttu-id="ed997-107">Du kan också anpassa dem så att de passar din app.</span><span class="sxs-lookup"><span data-stu-id="ed997-107">You can also customize them to fit your app.</span></span>

* <span data-ttu-id="ed997-108">`EngagementConfiguration.xml`: Mobile Engagement konfigurationsfil, det är där du kan anpassa Mobile Engagement-inställningar (Mobile Engagement anslutningssträngen, rapporten krascher...).</span><span class="sxs-lookup"><span data-stu-id="ed997-108">`EngagementConfiguration.xml` : The Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="ed997-109">/ HTML-mappen</span><span class="sxs-lookup"><span data-stu-id="ed997-109">/html folder</span></span>
* <span data-ttu-id="ed997-110">`EngagementNotification.html`: `Notification` Webbdesign visa html för sidhuvud i appen.</span><span class="sxs-lookup"><span data-stu-id="ed997-110">`EngagementNotification.html` : The `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="ed997-111">`EngagementAnnouncement.html`: `Announcement` Webbdesign visa html för app mellan enskilda muskler vyer.</span><span class="sxs-lookup"><span data-stu-id="ed997-111">`EngagementAnnouncement.html` : The `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="ed997-112">/ Images mapp</span><span class="sxs-lookup"><span data-stu-id="ed997-112">/images folder</span></span>
* <span data-ttu-id="ed997-113">`EngagementIconNotification.png`: Varumärken ikonen visas till vänster på ett meddelande ersätter den här med ditt varumärkesikon.</span><span class="sxs-lookup"><span data-stu-id="ed997-113">`EngagementIconNotification.png` : The brand icon displayed at the left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="ed997-114">`EngagementIconOk.png`: `Ok` Ikon för reach innehållssidor för knappen åtgärd eller validering.</span><span class="sxs-lookup"><span data-stu-id="ed997-114">`EngagementIconOk.png` : The `Ok` icon of the reach content pages for the action or validation button.</span></span>
* <span data-ttu-id="ed997-115">`EngagementIconNOK.png`: `NOK` Ikonen som används när knappen verifiering för reach innehållssidor är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="ed997-115">`EngagementIconNOK.png` : The `NOK` icon used when the validation button of the reach content pages is disabled.</span></span>
* <span data-ttu-id="ed997-116">`EngagementIconClose.png`: `Close` Ikonen för reach-meddelanden och innehåll för knappen Stäng.</span><span class="sxs-lookup"><span data-stu-id="ed997-116">`EngagementIconClose.png` : The `Close` icon of the reach notifications and contents for the dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="ed997-117">/overlay mapp</span><span class="sxs-lookup"><span data-stu-id="ed997-117">/overlay folder</span></span>
* <span data-ttu-id="ed997-118">`EngagementPageOverlay.cs`: Överlägget sidan ansvarar för att lägga till Engagement nå i appen användargränssnitt till dess underordnade.</span><span class="sxs-lookup"><span data-stu-id="ed997-118">`EngagementPageOverlay.cs` : The overlay page responsible for adding the Engagement reach in-app UI to its child.</span></span>

