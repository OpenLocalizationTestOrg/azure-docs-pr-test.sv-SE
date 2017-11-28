---
title: "aaaAzure Mobile Engagement Troubleshooting Guide - tjänsten"
description: "Felsökning för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a><span data-ttu-id="c8e8b-103">Felsökningsguide för problem med tjänsten</span><span class="sxs-lookup"><span data-stu-id="c8e8b-103">Troubleshooting guide for Service issues</span></span>
<span data-ttu-id="c8e8b-104">hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement körs.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-104">hello following are possible issues you may encounter with how Azure Mobile Engagement runs.</span></span>

## <a name="service-outages"></a><span data-ttu-id="c8e8b-105">Serviceavbrott</span><span class="sxs-lookup"><span data-stu-id="c8e8b-105">Service Outages</span></span>
### <a name="issue"></a><span data-ttu-id="c8e8b-106">Problem</span><span class="sxs-lookup"><span data-stu-id="c8e8b-106">Issue</span></span>
* <span data-ttu-id="c8e8b-107">Problem som visas toobe som orsakas av Azure Mobile Engagement driftstopp.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-107">Issues that appear toobe caused by Azure Mobile Engagement Service Outages.</span></span>

### <a name="causes"></a><span data-ttu-id="c8e8b-108">Orsaker</span><span class="sxs-lookup"><span data-stu-id="c8e8b-108">Causes</span></span>
* <span data-ttu-id="c8e8b-109">Problem som visas toobe på grund av driftstopp för Azure Mobile Engagement kan orsakas av flera olika problem:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-109">Issues that appear toobe caused by Azure Mobile Engagement Service Outages can be caused by several different issues:</span></span>
  * <span data-ttu-id="c8e8b-110">Isolerade problem som ursprungligen visas systemfel tooall med Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="c8e8b-110">Isolated issues that originally appear systemic tooall of Azure Mobile Engagement</span></span>
  * <span data-ttu-id="c8e8b-111">Kända problem som orsakas av server-avbrott (inte alltid visas i status-servern):</span><span class="sxs-lookup"><span data-stu-id="c8e8b-111">Known issues caused by server outages (not always shows in server status):</span></span>
  * <span data-ttu-id="c8e8b-112">Schemaläggning av fördröjningar, målinriktning fel, Aktivitetsikon uppdateringsproblem, statistik sluta samla in Push slutar fungera kan API: er slutar att fungera, nya appar eller användare inte skapas, DNS-fel och Timeout-fel i hello Användargränssnittet eller API Apps på en enhet.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-112">Scheduling delays, Targeting errors, Badge update issues, Statistics stop collecting, Push stops working, API's stop working, New apps or users can't be created, DNS errors, and Timeout errors in hello UI, API, or Apps on a device.</span></span>
  * <span data-ttu-id="c8e8b-113">Molnet beroende avbrott [Azure Tjänststatus](http://status.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="c8e8b-113">Cloud Dependency Outages [Azure Service Status](http://status.azure.com/)</span></span>
  * <span data-ttu-id="c8e8b-114">Push Notification Services (PNS) beroende avbrott</span><span class="sxs-lookup"><span data-stu-id="c8e8b-114">Push Notification Services (PNS) Dependency Outages</span></span>
  * <span data-ttu-id="c8e8b-115">App Store avbrott</span><span class="sxs-lookup"><span data-stu-id="c8e8b-115">App Store Outages</span></span>

1) <span data-ttu-id="c8e8b-116">tootest toosee om hello problem är systemfel kan du testa hello samma funktionen från en annan</span><span class="sxs-lookup"><span data-stu-id="c8e8b-116">tootest toosee if hello problem is systemic you can test hello same function from a different</span></span>

* <span data-ttu-id="c8e8b-117">Azure Mobile Engagement integrerat program</span><span class="sxs-lookup"><span data-stu-id="c8e8b-117">Azure Mobile Engagement integrated application</span></span>
* <span data-ttu-id="c8e8b-118">Testenhet</span><span class="sxs-lookup"><span data-stu-id="c8e8b-118">Test device</span></span>
* <span data-ttu-id="c8e8b-119">Testa enhetens OS-version</span><span class="sxs-lookup"><span data-stu-id="c8e8b-119">Test device OS version</span></span>
* <span data-ttu-id="c8e8b-120">Kampanj</span><span class="sxs-lookup"><span data-stu-id="c8e8b-120">Campaign</span></span>
* <span data-ttu-id="c8e8b-121">Användarkonto</span><span class="sxs-lookup"><span data-stu-id="c8e8b-121">Administrator user account</span></span>
* <span data-ttu-id="c8e8b-122">Webbläsaren (IE, Firefox, Chrome osv.)</span><span class="sxs-lookup"><span data-stu-id="c8e8b-122">Browser (IE, Firefox, Chrome, etc.)</span></span>
* <span data-ttu-id="c8e8b-123">Dator</span><span class="sxs-lookup"><span data-stu-id="c8e8b-123">Computer</span></span>

2) <span data-ttu-id="c8e8b-124">tootest om hello problem påverkar endast hello användargränssnitt eller hello API: er:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-124">tootest if hello problem only affects hello UI or hello API's:</span></span>

* <span data-ttu-id="c8e8b-125">Testa hello samma funktionen från båda hello Azure Mobile Engagement UI och hello Azure Mobile Engagement API: er.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-125">Test hello same function from both hello Azure Mobile Engagement UI and hello Azure Mobile Engagement API's.</span></span>

3) <span data-ttu-id="c8e8b-126">tootest om hello problem med nätverket mobiltelefon:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-126">tootest if hello problem is with your Cellular Phone Network:</span></span>

* <span data-ttu-id="c8e8b-127">Testa när anslutna toohello Internet via Wi-Fi och medan du är ansluten via din mobiltelefon 3G-nätverk.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-127">Test while connected toohello Internet via WIFI and while connected via your 3G cellular phone network.</span></span>
* <span data-ttu-id="c8e8b-128">Bekräfta att brandväggen inte blockerar alla hello Azure Mobile Engagement IP-adresser och portar.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-128">Confirm that your firewall is not blocking any of hello Azure Mobile Engagement IP Addresses or Ports.</span></span>

4) <span data-ttu-id="c8e8b-129">tootest om hello problem med din enhet:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-129">tootest if hello problem is with your Device:</span></span>

* <span data-ttu-id="c8e8b-130">Testa om enheten är kan tooconnect tooAzure Mobile Engagement med en annan integrerad Azure Mobile Engagement-app.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-130">Test if your Device is able tooconnect tooAzure Mobile Engagement with another Azure Mobile Engagement integrated app.</span></span>
* <span data-ttu-id="c8e8b-131">Kontrollera att du kan generera händelser, jobb och krascher från din telefon kan ses i hello Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-131">Test that you can generate Events, Jobs, and Crashes from your phone that can be seen in hello Azure Mobile Engagement UI.</span></span> 
* <span data-ttu-id="c8e8b-132">Testa om du kan skicka push-meddelanden från hello Azure Mobile Engagement UI tooyour enhet baserat på sin enhet-ID.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-132">Test if you can send push notifications from hello Azure Mobile Engagement UI tooyour device based on its Device ID.</span></span> 

5) <span data-ttu-id="c8e8b-133">tootest om hello problem med din App:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-133">tootest if hello problem is with your App:</span></span>

* <span data-ttu-id="c8e8b-134">Installera och testa programmet från en emulator i stället för från en fysisk enhet:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-134">Install and test your application from an emulator instead of from a physical device:</span></span>

6) <span data-ttu-id="c8e8b-135">tootest om hello problem med OS-uppgraderingar tooend användaren enheter, vilket kräver en uppgradering tooresolve SDK:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-135">tootest if hello problem is with OS upgrades tooend user Devices, which require an SDK upgrade tooresolve:</span></span>

* <span data-ttu-id="c8e8b-136">Testa programmet på olika enheter med olika versioner av hello OS.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-136">Test your application on different devices with different versions of hello OS.</span></span>
* <span data-ttu-id="c8e8b-137">Bekräfta att du använder hello senaste versionen av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-137">Confirm that you are using hello most recent version of hello SDK.</span></span>

## <a name="connectivity-and-incorrect-information-issues"></a><span data-ttu-id="c8e8b-138">Anslutning och felaktig Information problem</span><span class="sxs-lookup"><span data-stu-id="c8e8b-138">Connectivity and Incorrect Information Issues</span></span>
### <a name="issue"></a><span data-ttu-id="c8e8b-139">Problem</span><span class="sxs-lookup"><span data-stu-id="c8e8b-139">Issue</span></span>
* <span data-ttu-id="c8e8b-140">Problem som loggar in på hello Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-140">Problems logging into hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="c8e8b-141">Anslutningsfel med hello Azure Mobile Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-141">Connection errors with hello Azure Mobile Engagement API's.</span></span>
* <span data-ttu-id="c8e8b-142">Problem med att överföra appen Info taggar via hello Device API.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-142">Problems uploading App Info Tags via hello Device API.</span></span>
* <span data-ttu-id="c8e8b-143">Problem vid hämtning av loggar eller exporterade data från Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-143">Problems downloading logs or exported data from Azure Mobile Engagement.</span></span>
* <span data-ttu-id="c8e8b-144">Felaktig information visas i hello Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-144">Incorrect information shown in hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="c8e8b-145">Felaktig information visas i Azure Mobile Engagement-loggar.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-145">Incorrect information shown in Azure Mobile Engagement logs.</span></span>

### <a name="causes"></a><span data-ttu-id="c8e8b-146">Orsaker</span><span class="sxs-lookup"><span data-stu-id="c8e8b-146">Causes</span></span>
* <span data-ttu-id="c8e8b-147">Bekräfta ditt användarkonto har tillräcklig behörighet tooperform hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-147">Confirm your user account has sufficient permissions tooperform hello task.</span></span>
* <span data-ttu-id="c8e8b-148">Bekräfta att hello problemet inte är isolerad tooone dator eller det lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-148">Confirm that hello problem is not isolated tooone computer or your local network.</span></span>
* <span data-ttu-id="c8e8b-149">Bekräfta att den hello Azure Mobile Engagement-tjänsten har ingen rapporterade stillestånd.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-149">Confirm that that hello Azure Mobile Engagement service has no reported outages.</span></span>
* <span data-ttu-id="c8e8b-150">Bekräfta att din App Info märka filer följer alla de här reglerna:</span><span class="sxs-lookup"><span data-stu-id="c8e8b-150">Confirm that your App Info Tag files follow all of these rules:</span></span>
  * <span data-ttu-id="c8e8b-151">Använd endast hello UTF8-teckenuppsättning (hello ANSI-teckenuppsättningen stöds inte).</span><span class="sxs-lookup"><span data-stu-id="c8e8b-151">Use only hello UTF8 character set (hello ANSI character set is not supported).</span></span>
  * <span data-ttu-id="c8e8b-152">Använda ett kommatecken ””, som hello avgränsningstecknet (du kan öppna en service begäran toorequest toochange hello .csv avgränsningstecknet från ett kommatecken ””, tooanother-tecken, till exempel ett semikolon ””;).</span><span class="sxs-lookup"><span data-stu-id="c8e8b-152">Use a comma "," as hello separator character (you can open a service request toorequest toochange hello .csv separator character from a comma "," tooanother character such as a semi-colon ";").</span></span>
  * <span data-ttu-id="c8e8b-153">Använd alla gemen för booleska värden ”true” och ”false”.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-153">Use all lower case for Boolean values "true" and "false".</span></span>
  * <span data-ttu-id="c8e8b-154">Använda en fil som är mindre än hello maximal filstorlek 35 MB.</span><span class="sxs-lookup"><span data-stu-id="c8e8b-154">Use a file that is smaller than hello maximum file size of 35MB.</span></span>

