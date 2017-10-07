---
title: aaaAzure Mobile Engagement Troubleshooting Guide - Analytics
description: "Felsökning av problem med Analytics, övervakning, segmentering och instrumentpanelen i Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a><span data-ttu-id="c2522-103">Felsökningsguide för Analytics, övervakning, segmentering och instrumentpanelen problem</span><span class="sxs-lookup"><span data-stu-id="c2522-103">Troubleshooting guide for Analytics, Monitoring, Segmentation, and Dashboard issues</span></span>
<span data-ttu-id="c2522-104">hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement samlar in information om dina program, enheter och användare.</span><span class="sxs-lookup"><span data-stu-id="c2522-104">hello following are possible issues you may encounter with how Azure Mobile Engagement gathers information about your applications, devices, and users.</span></span>

## <a name="missingdelayed-information"></a><span data-ttu-id="c2522-105">Information saknas/fördröjd</span><span class="sxs-lookup"><span data-stu-id="c2522-105">Missing/Delayed information</span></span>
### <a name="issue"></a><span data-ttu-id="c2522-106">Problem</span><span class="sxs-lookup"><span data-stu-id="c2522-106">Issue</span></span>
* <span data-ttu-id="c2522-107">Visas i som visas i Analytics, segmentering eller instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2522-107">Information is delayed in appearing in Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="c2522-108">Information saknas från övervakning.</span><span class="sxs-lookup"><span data-stu-id="c2522-108">Information is missing from Monitoring.</span></span>
* <span data-ttu-id="c2522-109">Information saknas från Analytics, segmentering eller instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2522-109">Information is missing from Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="c2522-110">Träffa segmentering begränsar.</span><span class="sxs-lookup"><span data-stu-id="c2522-110">Hitting segmentation limits.</span></span>

### <a name="causes"></a><span data-ttu-id="c2522-111">Orsaker</span><span class="sxs-lookup"><span data-stu-id="c2522-111">Causes</span></span>
* <span data-ttu-id="c2522-112">Du kan använda hello Analytics API övervakaren API och segment API toosee om några data du saknas från hello Användargränssnittet syns via hello API: er.</span><span class="sxs-lookup"><span data-stu-id="c2522-112">You can use hello Analytics API, Monitor API, and Segments API toosee if any data you are missing from hello UI is visible through hello APIs.</span></span>
* <span data-ttu-id="c2522-113">Om hello Azure Mobile Engagement SDK inte är korrekt integrerad i din app kommer inte att kunna toosee informationen i hello Analytics, segmentering, övervakning och instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="c2522-113">If hello Azure Mobile Engagement SDK is not correctly integrated into your app then you won't be able toosee information in hello Analytics, Segmentation, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="c2522-114">Segment får inte vara ändras när de skapas, segment kan endast vara ”klonade” (kopieras) eller ”förstörs” (bort).</span><span class="sxs-lookup"><span data-stu-id="c2522-114">Segments can't be changed once they are created, segments can only be "cloned" (copied) or "destroyed" (deleted).</span></span> <span data-ttu-id="c2522-115">Segment kan bara innehålla 10 kriterier.</span><span class="sxs-lookup"><span data-stu-id="c2522-115">Segments can only contain 10 criteria.</span></span>
* <span data-ttu-id="c2522-116">hello bästa sätt tootest saknas information från övervakning är toosetup en testenhet, avinstallera och installera om hello app på hello testenhet.</span><span class="sxs-lookup"><span data-stu-id="c2522-116">hello best way tootest missing information from monitoring is toosetup a test device, uninstall and/or reinstall hello app on hello test device.</span></span>
* <span data-ttu-id="c2522-117">Informationen uppdateras var 24: e timme för Analytics, segmentering eller instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="c2522-117">Information is refreshed every 24 hours for Analytics, Segmentation, or Dashboards.</span></span>
* <span data-ttu-id="c2522-118">Informationen i nya segment visas inte förrän 24 timmar efter att de skapas även om hello segment baserat på information som tidigare.</span><span class="sxs-lookup"><span data-stu-id="c2522-118">Information in new segments may not be displayed until 24 hours after they are created even if hello segment is based on previous information.</span></span>
* <span data-ttu-id="c2522-119">Filtrera data i hello UI analytics visas alla exempel på den här typen oavsett hello version av appen (t.ex.) ”Kraschar” filtrerade efter namn visas från version 1 och version 2 av appen).</span><span class="sxs-lookup"><span data-stu-id="c2522-119">Filtering your analytics data in hello UI will show all examples of this type regardless of hello version of your app (e.g. "Crashes" filtered by name will show from version 1 and version 2 of your app).</span></span>
* <span data-ttu-id="c2522-120">hello baseras tidsperiod för Analytics på hello datum från hello användarnas Enhetsinställningar, så att en användare vars telefonen har hello datum felaktigt som kan visas i hello fel tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c2522-120">hello time period for Analytics is based on hello date from hello users' device settings, so a user whose phone has hello date incorrectly set could show up in hello wrong time period.</span></span>
* <span data-ttu-id="c2522-121">Inga data loggas när du använder hello-knappen på serversidan för ”test” push-meddelanden, data loggas endast för verkliga push-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="c2522-121">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

## <a name="cant-locate-items-in-ui"></a><span data-ttu-id="c2522-122">Det går inte att hitta objekt i Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="c2522-122">Can't locate items in UI</span></span>
### <a name="issue"></a><span data-ttu-id="c2522-123">Problem</span><span class="sxs-lookup"><span data-stu-id="c2522-123">Issue</span></span>
* <span data-ttu-id="c2522-124">Kan inte skapa segment baserat på vissa inbyggda eller anpassade appinfo tagga kriterier.</span><span class="sxs-lookup"><span data-stu-id="c2522-124">Can't create segments based on certain built in or custom app info tag criteria.</span></span>
* <span data-ttu-id="c2522-125">Kan inte hitta vissa inbyggda eller anpassade appinfo tagga kriterier i Analytics, övervakning och instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="c2522-125">Can't find certain built in or custom app info tag criteria in Analytics, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="c2522-126">Det går inte att tolka hello data i Analytics, övervakning, segmentering och instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="c2522-126">Can't interpret hello data in Analytics, Monitoring, Segmentation, or Dashboards.</span></span>

### <a name="causes"></a><span data-ttu-id="c2522-127">Orsaker</span><span class="sxs-lookup"><span data-stu-id="c2522-127">Causes</span></span>
* <span data-ttu-id="c2522-128">Vissa inbyggda objekt och appinfo taggar är bara tillgängliga som push-villkor, men kanske inte läggas till tooa segment eller synliga från Analytics, övervakning eller instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2522-128">Some built in items and app info tags are only available as push criteria but may not be added tooa segment or visible from Analytics, Monitoring, or Dashboard.</span></span> 
* <span data-ttu-id="c2522-129">För inbyggda objekt och appinfo taggar som inte går att lägga till tooa segment, behöver du toosetup lista över villkor i varje kampanj tooperform hello samma fungera som mål baserat på ett segment som mål.</span><span class="sxs-lookup"><span data-stu-id="c2522-129">For built in items and app info tags that can't be added tooa segment, you will need toosetup list of targeting criteria in each campaign tooperform hello same function as targeting based on a segment.</span></span>
* <span data-ttu-id="c2522-130">Se hello snabbmenyer i hello Analytics, övervakning, segmentering och instrumentpaneler avsnitt i hello Azure Mobile Engagement Användargränssnittet för mer hjälp och hur tooinformation.</span><span class="sxs-lookup"><span data-stu-id="c2522-130">See hello context menus in hello Analytics, Monitoring, Segmentation, and Dashboards sections of hello Azure Mobile Engagement UI for more help and how tooinformation.</span></span>

## <a name="crash-troubleshooting"></a><span data-ttu-id="c2522-131">Krasch felsökning</span><span class="sxs-lookup"><span data-stu-id="c2522-131">Crash troubleshooting</span></span>
### <a name="issue"></a><span data-ttu-id="c2522-132">Problem</span><span class="sxs-lookup"><span data-stu-id="c2522-132">Issue</span></span>
* <span data-ttu-id="c2522-133">Programmet kraschar som visas i Analytics, övervakning eller instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2522-133">Application Crashes appearing in Analytics, Monitoring, or Dashboard.</span></span>

### <a name="causes"></a><span data-ttu-id="c2522-134">Orsaker</span><span class="sxs-lookup"><span data-stu-id="c2522-134">Causes</span></span>
* <span data-ttu-id="c2522-135">tootroubleshoot programmet kraschar ses i Analytics, övervakning eller instrumentpanel gör att toocheck hello viktig information om kända problem med tidigare versioner av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="c2522-135">tootroubleshoot Application Crashes seen in Analytics, Monitoring, or Dashboard make sure toocheck hello release notes for known issues with previous versions of hello SDK.</span></span>
* <span data-ttu-id="c2522-136">toofurther felsöka program kraschar utför en händelse från en testenhet med ditt program installerat och leta upp din enhets-ID under hello ”övervaka – händelser” hello Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c2522-136">toofurther troubleshoot application crashes perform an event from a test device with your application installed and look up your device ID in hello “Monitor – Events” section of hello Azure Mobile Engagement UI.</span></span> <span data-ttu-id="c2522-137">Sedan utföra hello händelse som gör att din ansökan toocrash och leta upp ytterligare information i hello ”övervakaren – krascher” avsnittet av hello Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c2522-137">Then perform hello event that is causing your application toocrash and look up additional information in hello “Monitor – Crash” section of hello Azure Mobile Engagement UI.</span></span> 

