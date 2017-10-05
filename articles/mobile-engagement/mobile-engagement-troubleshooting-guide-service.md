---
title: "Azure Mobile Engagement felsökningsguide för - tjänsten"
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
ms.openlocfilehash: f13fd0540b783120014b3a8d4e41f78808c7fade
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a><span data-ttu-id="d48c1-103">Felsökningsguide för problem med tjänsten</span><span class="sxs-lookup"><span data-stu-id="d48c1-103">Troubleshooting guide for Service issues</span></span>
<span data-ttu-id="d48c1-104">Följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement körs.</span><span class="sxs-lookup"><span data-stu-id="d48c1-104">The following are possible issues you may encounter with how Azure Mobile Engagement runs.</span></span>

## <a name="service-outages"></a><span data-ttu-id="d48c1-105">Serviceavbrott</span><span class="sxs-lookup"><span data-stu-id="d48c1-105">Service Outages</span></span>
### <a name="issue"></a><span data-ttu-id="d48c1-106">Problem</span><span class="sxs-lookup"><span data-stu-id="d48c1-106">Issue</span></span>
* <span data-ttu-id="d48c1-107">Problem som kan orsakas av driftstörningar i Azure Mobile Engagement tjänsten visas.</span><span class="sxs-lookup"><span data-stu-id="d48c1-107">Issues that appear to be caused by Azure Mobile Engagement Service Outages.</span></span>

### <a name="causes"></a><span data-ttu-id="d48c1-108">Orsaker</span><span class="sxs-lookup"><span data-stu-id="d48c1-108">Causes</span></span>
* <span data-ttu-id="d48c1-109">Problem som visas kan orsakas av serviceavbrott för Azure Mobile Engagement kan orsakas av flera olika problem:</span><span class="sxs-lookup"><span data-stu-id="d48c1-109">Issues that appear to be caused by Azure Mobile Engagement Service Outages can be caused by several different issues:</span></span>
  * <span data-ttu-id="d48c1-110">Isolerade problem som ursprungligen visas systemfel till alla Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="d48c1-110">Isolated issues that originally appear systemic to all of Azure Mobile Engagement</span></span>
  * <span data-ttu-id="d48c1-111">Kända problem som orsakas av server-avbrott (inte alltid visas i status-servern):</span><span class="sxs-lookup"><span data-stu-id="d48c1-111">Known issues caused by server outages (not always shows in server status):</span></span>
  * <span data-ttu-id="d48c1-112">Schemaläggning av fördröjningar, målinriktning fel, Aktivitetsikon uppdateringsproblem, statistik sluta samla in Push slutar fungera kan API: er slutar att fungera, nya appar eller användare inte skapas, DNS-fel och Timeout-fel i Användargränssnittet, API eller appar på en enhet.</span><span class="sxs-lookup"><span data-stu-id="d48c1-112">Scheduling delays, Targeting errors, Badge update issues, Statistics stop collecting, Push stops working, API's stop working, New apps or users can't be created, DNS errors, and Timeout errors in the UI, API, or Apps on a device.</span></span>
  * <span data-ttu-id="d48c1-113">Molnet beroende avbrott [Azure Tjänststatus](http://status.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="d48c1-113">Cloud Dependency Outages [Azure Service Status](http://status.azure.com/)</span></span>
  * <span data-ttu-id="d48c1-114">Push Notification Services (PNS) beroende avbrott</span><span class="sxs-lookup"><span data-stu-id="d48c1-114">Push Notification Services (PNS) Dependency Outages</span></span>
  * <span data-ttu-id="d48c1-115">App Store avbrott</span><span class="sxs-lookup"><span data-stu-id="d48c1-115">App Store Outages</span></span>

1) <span data-ttu-id="d48c1-116">Om du vill testa om problemet är systemfel kan du testa samma funktion i en annan</span><span class="sxs-lookup"><span data-stu-id="d48c1-116">To test to see if the problem is systemic you can test the same function from a different</span></span>

* <span data-ttu-id="d48c1-117">Azure Mobile Engagement integrerat program</span><span class="sxs-lookup"><span data-stu-id="d48c1-117">Azure Mobile Engagement integrated application</span></span>
* <span data-ttu-id="d48c1-118">Testenhet</span><span class="sxs-lookup"><span data-stu-id="d48c1-118">Test device</span></span>
* <span data-ttu-id="d48c1-119">Testa enhetens OS-version</span><span class="sxs-lookup"><span data-stu-id="d48c1-119">Test device OS version</span></span>
* <span data-ttu-id="d48c1-120">Kampanj</span><span class="sxs-lookup"><span data-stu-id="d48c1-120">Campaign</span></span>
* <span data-ttu-id="d48c1-121">Användarkonto</span><span class="sxs-lookup"><span data-stu-id="d48c1-121">Administrator user account</span></span>
* <span data-ttu-id="d48c1-122">Webbläsaren (IE, Firefox, Chrome osv.)</span><span class="sxs-lookup"><span data-stu-id="d48c1-122">Browser (IE, Firefox, Chrome, etc.)</span></span>
* <span data-ttu-id="d48c1-123">Dator</span><span class="sxs-lookup"><span data-stu-id="d48c1-123">Computer</span></span>

2) <span data-ttu-id="d48c1-124">Kontrollera att problemet påverkar endast av Användargränssnittet eller API: er:</span><span class="sxs-lookup"><span data-stu-id="d48c1-124">To test if the problem only affects the UI or the API's:</span></span>

* <span data-ttu-id="d48c1-125">Testa samma funktion från Gränssnittet för Azure Mobile Engagement och i Azure Mobile Engagement API: er.</span><span class="sxs-lookup"><span data-stu-id="d48c1-125">Test the same function from both the Azure Mobile Engagement UI and the Azure Mobile Engagement API's.</span></span>

3) <span data-ttu-id="d48c1-126">Kontrollera att problemet har med din mobiltelefon nätverk:</span><span class="sxs-lookup"><span data-stu-id="d48c1-126">To test if the problem is with your Cellular Phone Network:</span></span>

* <span data-ttu-id="d48c1-127">Testa medan du är ansluten till Internet via Wi-Fi och medan du är ansluten via din mobiltelefon 3G-nätverk.</span><span class="sxs-lookup"><span data-stu-id="d48c1-127">Test while connected to the Internet via WIFI and while connected via your 3G cellular phone network.</span></span>
* <span data-ttu-id="d48c1-128">Bekräfta att brandväggen inte blockerar alla Azure Mobile Engagement IP-adresser och portar.</span><span class="sxs-lookup"><span data-stu-id="d48c1-128">Confirm that your firewall is not blocking any of the Azure Mobile Engagement IP Addresses or Ports.</span></span>

4) <span data-ttu-id="d48c1-129">Kontrollera att problemet har med din enhet:</span><span class="sxs-lookup"><span data-stu-id="d48c1-129">To test if the problem is with your Device:</span></span>

* <span data-ttu-id="d48c1-130">Testa om enheten är ansluta till Azure Mobile Engagement med en annan integrerad Azure Mobile Engagement-app.</span><span class="sxs-lookup"><span data-stu-id="d48c1-130">Test if your Device is able to connect to Azure Mobile Engagement with another Azure Mobile Engagement integrated app.</span></span>
* <span data-ttu-id="d48c1-131">Kontrollera att du kan generera händelser, jobb och krascher från din telefon kan ses i Användargränssnittet för Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d48c1-131">Test that you can generate Events, Jobs, and Crashes from your phone that can be seen in the Azure Mobile Engagement UI.</span></span> 
* <span data-ttu-id="d48c1-132">Testa om du kan skicka push-meddelanden från Azure Mobile Engagement-Gränssnittet till enheten baserat på sin enhet-ID.</span><span class="sxs-lookup"><span data-stu-id="d48c1-132">Test if you can send push notifications from the Azure Mobile Engagement UI to your device based on its Device ID.</span></span> 

5) <span data-ttu-id="d48c1-133">Kontrollera att problemet har med din App:</span><span class="sxs-lookup"><span data-stu-id="d48c1-133">To test if the problem is with your App:</span></span>

* <span data-ttu-id="d48c1-134">Installera och testa programmet från en emulator i stället för från en fysisk enhet:</span><span class="sxs-lookup"><span data-stu-id="d48c1-134">Install and test your application from an emulator instead of from a physical device:</span></span>

6) <span data-ttu-id="d48c1-135">Att testa om problemet med OS-uppgraderingar till slutanvändare enheter, vilket kräver uppgradering SDK för att matcha:</span><span class="sxs-lookup"><span data-stu-id="d48c1-135">To test if the problem is with OS upgrades to end user Devices, which require an SDK upgrade to resolve:</span></span>

* <span data-ttu-id="d48c1-136">Testa programmet på olika enheter med olika versioner av Operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="d48c1-136">Test your application on different devices with different versions of the OS.</span></span>
* <span data-ttu-id="d48c1-137">Bekräfta att du använder den senaste versionen av SDK.</span><span class="sxs-lookup"><span data-stu-id="d48c1-137">Confirm that you are using the most recent version of the SDK.</span></span>

## <a name="connectivity-and-incorrect-information-issues"></a><span data-ttu-id="d48c1-138">Anslutning och felaktig Information problem</span><span class="sxs-lookup"><span data-stu-id="d48c1-138">Connectivity and Incorrect Information Issues</span></span>
### <a name="issue"></a><span data-ttu-id="d48c1-139">Problem</span><span class="sxs-lookup"><span data-stu-id="d48c1-139">Issue</span></span>
* <span data-ttu-id="d48c1-140">Problem med att logga in på Azure Mobile Engagement-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d48c1-140">Problems logging into the Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="d48c1-141">Anslutningsfel med den Azure Mobile Engagement API: er.</span><span class="sxs-lookup"><span data-stu-id="d48c1-141">Connection errors with the Azure Mobile Engagement API's.</span></span>
* <span data-ttu-id="d48c1-142">Problem med att överföra appen Info taggar via Device API.</span><span class="sxs-lookup"><span data-stu-id="d48c1-142">Problems uploading App Info Tags via the Device API.</span></span>
* <span data-ttu-id="d48c1-143">Problem vid hämtning av loggar eller exporterade data från Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d48c1-143">Problems downloading logs or exported data from Azure Mobile Engagement.</span></span>
* <span data-ttu-id="d48c1-144">Felaktig information som visas i Användargränssnittet för Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d48c1-144">Incorrect information shown in the Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="d48c1-145">Felaktig information visas i Azure Mobile Engagement-loggar.</span><span class="sxs-lookup"><span data-stu-id="d48c1-145">Incorrect information shown in Azure Mobile Engagement logs.</span></span>

### <a name="causes"></a><span data-ttu-id="d48c1-146">Orsaker</span><span class="sxs-lookup"><span data-stu-id="d48c1-146">Causes</span></span>
* <span data-ttu-id="d48c1-147">Bekräfta ditt användarkonto har behörighet att utföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d48c1-147">Confirm your user account has sufficient permissions to perform the task.</span></span>
* <span data-ttu-id="d48c1-148">Kontrollera att problemet inte är isolerad för att en dator eller det lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="d48c1-148">Confirm that the problem is not isolated to one computer or your local network.</span></span>
* <span data-ttu-id="d48c1-149">Bekräfta att att Azure Mobile Engagement-tjänsten inte har någon rapporterat avbrott.</span><span class="sxs-lookup"><span data-stu-id="d48c1-149">Confirm that that the Azure Mobile Engagement service has no reported outages.</span></span>
* <span data-ttu-id="d48c1-150">Bekräfta att din App Info märka filer följer alla de här reglerna:</span><span class="sxs-lookup"><span data-stu-id="d48c1-150">Confirm that your App Info Tag files follow all of these rules:</span></span>
  * <span data-ttu-id="d48c1-151">Använd endast den UTF8 teckenuppsättning (ANSI-teckenuppsättningen inte stöds).</span><span class="sxs-lookup"><span data-stu-id="d48c1-151">Use only the UTF8 character set (the ANSI character set is not supported).</span></span>
  * <span data-ttu-id="d48c1-152">Använda ett kommatecken ””, som avgränsningstecken (du kan öppna en tjänst begäran till begäran att ändra avgränsningstecknet CSV från en kommaavgränsad ””, tecken, till exempel ett semikolon ””;).</span><span class="sxs-lookup"><span data-stu-id="d48c1-152">Use a comma "," as the separator character (you can open a service request to request to change the .csv separator character from a comma "," to another character such as a semi-colon ";").</span></span>
  * <span data-ttu-id="d48c1-153">Använd alla gemen för booleska värden ”true” och ”false”.</span><span class="sxs-lookup"><span data-stu-id="d48c1-153">Use all lower case for Boolean values "true" and "false".</span></span>
  * <span data-ttu-id="d48c1-154">Använda en fil som är mindre än den maximala filstorleken 35 MB.</span><span class="sxs-lookup"><span data-stu-id="d48c1-154">Use a file that is smaller than the maximum file size of 35MB.</span></span>

