---
title: StorSimple virtuell matris web UI administration | Microsoft Docs
description: "Beskriver hur du utför grundläggande enhet administrationsuppgifter via webbgränssnittet för virtuell StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 989e7b697f9b527df549fb32be18edd1d3c8d224
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a><span data-ttu-id="0732a-103">Använda Webbgränssnittet för att administrera din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="0732a-103">Use the Web UI to administer your StorSimple Virtual Array</span></span>
![processflöde för installationen](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="0732a-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0732a-105">Overview</span></span>
<span data-ttu-id="0732a-106">Självstudiekurser i den här artikeln gäller för Microsoft Azure StorSimple virtuell matrisen (kallas även den lokala virtuella enheten StorSimple) kör mars 2016 allmän tillgänglighet (GA) version.</span><span class="sxs-lookup"><span data-stu-id="0732a-106">The tutorials in this article apply to the Microsoft Azure StorSimple Virtual Array (also known as the StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="0732a-107">Den här artikeln beskrivs några av komplexa arbetsflöden och administrativa uppgifter som kan utföras på den virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="0732a-107">This article describes some of the complex workflows and management tasks that can be performed on the StorSimple Virtual Array.</span></span> <span data-ttu-id="0732a-108">Du kan hantera den virtuella StorSimple-matrisen med hjälp av StorSimple Manager service användargränssnitt (kallas i portalens användargränssnitt) och lokala webbgränssnittet för enheten.</span><span class="sxs-lookup"><span data-stu-id="0732a-108">You can manage the StorSimple Virtual Array using the StorSimple Manager service UI (referred to as the portal UI) and the local web UI for the device.</span></span> <span data-ttu-id="0732a-109">Den här artikeln fokuserar på de uppgifter som du kan utföra med hjälp av webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0732a-109">This article focuses on the tasks that you can perform using the web UI.</span></span>

<span data-ttu-id="0732a-110">Den här artikeln innehåller följande kurser:</span><span class="sxs-lookup"><span data-stu-id="0732a-110">This article includes the following tutorials:</span></span>

* <span data-ttu-id="0732a-111">Hämta krypteringsnyckel för tjänstdata</span><span class="sxs-lookup"><span data-stu-id="0732a-111">Get the service data encryption key</span></span>
* <span data-ttu-id="0732a-112">Felsöka web UI installationen</span><span class="sxs-lookup"><span data-stu-id="0732a-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="0732a-113">Generera en logg-paket</span><span class="sxs-lookup"><span data-stu-id="0732a-113">Generate a log package</span></span>
* <span data-ttu-id="0732a-114">Stänga av eller starta om enheten</span><span class="sxs-lookup"><span data-stu-id="0732a-114">Shut down or restart your device</span></span>

## <a name="get-the-service-data-encryption-key"></a><span data-ttu-id="0732a-115">Hämta krypteringsnyckel för tjänstdata</span><span class="sxs-lookup"><span data-stu-id="0732a-115">Get the service data encryption key</span></span>
<span data-ttu-id="0732a-116">En krypteringsnyckel för tjänstdata ska genereras när du registrerar din första enhet med StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0732a-116">A service data encryption key is generated when you register your first device with the StorSimple Manager service.</span></span> <span data-ttu-id="0732a-117">Den här nyckeln är sedan krävs med nyckeln för tjänstregistrering för att registrera ytterligare enheter med StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0732a-117">This key is then required with the service registration key to register additional devices with the StorSimple Manager service.</span></span>

<span data-ttu-id="0732a-118">Om du har tappat bort din krypteringsnyckel för tjänstdata och behovet av att hämta den, utför följande steg i det lokala webbgränssnittet för enheten registreras med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="0732a-118">If you have misplaced your service data encryption key and need to retrieve it, perform the following steps in the local web UI of the device registered with your service.</span></span>

#### <a name="to-get-the-service-data-encryption-key"></a><span data-ttu-id="0732a-119">Hämta krypteringsnyckel för tjänstdata</span><span class="sxs-lookup"><span data-stu-id="0732a-119">To get the service data encryption key</span></span>
1. <span data-ttu-id="0732a-120">Ansluta till lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0732a-120">Connect to the local web UI.</span></span> <span data-ttu-id="0732a-121">Gå till **Configuration** > **Molninställningar**.</span><span class="sxs-lookup"><span data-stu-id="0732a-121">Go to **Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="0732a-122">Längst ned på sidan klickar du på **hämta krypteringsnyckel för tjänstdata**.</span><span class="sxs-lookup"><span data-stu-id="0732a-122">At the bottom of the page, click **Get service data encryption key**.</span></span> <span data-ttu-id="0732a-123">En nyckel visas.</span><span class="sxs-lookup"><span data-stu-id="0732a-123">A key will appear.</span></span> <span data-ttu-id="0732a-124">Kopiera och spara den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="0732a-124">Copy and save this key.</span></span>
   
    ![Hämta krypteringsnyckel för tjänstdata 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="0732a-126">Felsöka web UI installationen</span><span class="sxs-lookup"><span data-stu-id="0732a-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="0732a-127">I vissa fall när du konfigurerar enheten via det lokala webbgränssnittet kanske du stöter på fel.</span><span class="sxs-lookup"><span data-stu-id="0732a-127">In some instances when you configure the device through the local web UI, you might run into errors.</span></span> <span data-ttu-id="0732a-128">Du kan köra testerna diagnostik för att diagnostisera och felsöka dessa.</span><span class="sxs-lookup"><span data-stu-id="0732a-128">To diagnose and troubleshoot such errors, you can run the diagnostics tests.</span></span>

#### <a name="to-run-the-diagnostic-tests"></a><span data-ttu-id="0732a-129">Köra diagnostiska tester</span><span class="sxs-lookup"><span data-stu-id="0732a-129">To run the diagnostic tests</span></span>
1. <span data-ttu-id="0732a-130">Gå till i det lokala webbgränssnittet **felsökning** > **diagnostiska testerna**.</span><span class="sxs-lookup"><span data-stu-id="0732a-130">In the local web UI, go to **Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![Kör diagnostik 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="0732a-132">Längst ned på sidan klickar du på **kör diagnostiktest**.</span><span class="sxs-lookup"><span data-stu-id="0732a-132">At the bottom of the page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="0732a-133">Detta kommer att initiera tester för att diagnostisera eventuella problem med nätverket, enhet, webbproxy, tid eller molninställningar.</span><span class="sxs-lookup"><span data-stu-id="0732a-133">This will initiate tests to diagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="0732a-134">Du meddelas att enheten kör test.</span><span class="sxs-lookup"><span data-stu-id="0732a-134">You will be notified that the device is running tests.</span></span>
3. <span data-ttu-id="0732a-135">När testerna har slutförts visas resultatet.</span><span class="sxs-lookup"><span data-stu-id="0732a-135">After the tests have completed, the results will be displayed.</span></span> <span data-ttu-id="0732a-136">I följande exempel visas resultatet av diagnostiska test.</span><span class="sxs-lookup"><span data-stu-id="0732a-136">The following example shows the results of diagnostic tests.</span></span> <span data-ttu-id="0732a-137">Observera att webbproxyinställningarna inte har konfigurerats på den här enheten och därför web proxy testet kördes inte.</span><span class="sxs-lookup"><span data-stu-id="0732a-137">Note that the web proxy settings were not configured on this device, and therefore, the web proxy test was not run.</span></span> <span data-ttu-id="0732a-138">Alla andra test för nätverksinställningar, DNS-server och tidsinställningar lyckades.</span><span class="sxs-lookup"><span data-stu-id="0732a-138">All the other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![Kör diagnostik 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="0732a-140">Generera en logg-paket</span><span class="sxs-lookup"><span data-stu-id="0732a-140">Generate a log package</span></span>
<span data-ttu-id="0732a-141">En logg paketet består av alla relevanta loggar som kan hjälpa Microsoft-supporten att felsöka eventuella enhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="0732a-141">A log package is comprised of all the relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="0732a-142">I den här versionen kan en logg-paketet skapas via lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0732a-142">In this release, a log package can be generated via the local web UI.</span></span>

#### <a name="to-generate-the-log-package"></a><span data-ttu-id="0732a-143">Generera log-paketet</span><span class="sxs-lookup"><span data-stu-id="0732a-143">To generate the log package</span></span>
1. <span data-ttu-id="0732a-144">Gå till i det lokala webbgränssnittet **felsökning** > **systemloggar**.</span><span class="sxs-lookup"><span data-stu-id="0732a-144">In the local web UI, go to **Troubleshooting** > **System logs**.</span></span>
   
    ![Generera loggen paketet 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="0732a-146">Längst ned på sidan klickar du på **skapa loggen paketet**.</span><span class="sxs-lookup"><span data-stu-id="0732a-146">At the bottom of the page, click **Create log package**.</span></span> <span data-ttu-id="0732a-147">Ett paket med systemloggarna kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="0732a-147">A package of the system logs will be created.</span></span> <span data-ttu-id="0732a-148">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="0732a-148">This will take a couple of minutes.</span></span>
   
    ![Generera loggen paketet 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="0732a-150">Du meddelas när paketet har skapats och sidan uppdateras för att visa tid och datum när paketet har skapats.</span><span class="sxs-lookup"><span data-stu-id="0732a-150">You will be notified after the package is successfully created, and the page will be updated to indicate the time and date when the package was created.</span></span>
   
    ![Generera loggen paketet 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="0732a-152">Klicka på **loggen hämtningspaketet**.</span><span class="sxs-lookup"><span data-stu-id="0732a-152">Click **Download log package**.</span></span> <span data-ttu-id="0732a-153">Komprimerade paketet kommer att hämtas i systemet.</span><span class="sxs-lookup"><span data-stu-id="0732a-153">A zipped package will be downloaded on your system.</span></span>
   
    ![Generera loggen paketet 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="0732a-155">Du kan packa upp paketet hämtade loggen och visa systemloggfilerna.</span><span class="sxs-lookup"><span data-stu-id="0732a-155">You can unzip the downloaded log package and view the system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="0732a-156">Stäng och starta om enheten</span><span class="sxs-lookup"><span data-stu-id="0732a-156">Shut down and restart your device</span></span>
<span data-ttu-id="0732a-157">Du kan stänga av eller starta om din virtuella enhet använder lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0732a-157">You can shut down or restart your virtual device using the local web UI.</span></span> <span data-ttu-id="0732a-158">Vi rekommenderar att innan du startar om ta volymer eller resurser offline på värden och sedan på enheten.</span><span class="sxs-lookup"><span data-stu-id="0732a-158">We recommend that before you restart, take the volumes or shares offline on the host and then the device.</span></span> <span data-ttu-id="0732a-159">Detta minimerar risken för varje möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="0732a-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="to-shut-down-your-virtual-device"></a><span data-ttu-id="0732a-160">Att stänga av den virtuella enheten</span><span class="sxs-lookup"><span data-stu-id="0732a-160">To shut down your virtual device</span></span>
1. <span data-ttu-id="0732a-161">Gå till i det lokala webbgränssnittet **Underhåll** > **energiinställningar**.</span><span class="sxs-lookup"><span data-stu-id="0732a-161">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="0732a-162">Längst ned på sidan klickar du på **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="0732a-162">At the bottom of the page, click **Shutdown**.</span></span>
   
    ![enheten avstängning 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="0732a-164">En varning visas om att stängas av enheten kommer att avbryta alla i/o som pågår, vilket resulterar i ett driftstopp.</span><span class="sxs-lookup"><span data-stu-id="0732a-164">A warning will appear stating that a shutdown of the device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="0732a-165">Klicka på kryssikonen</span><span class="sxs-lookup"><span data-stu-id="0732a-165">Click the check icon</span></span> ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="0732a-167">.</span><span class="sxs-lookup"><span data-stu-id="0732a-167">.</span></span>
   
    ![enheten avstängning varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="0732a-169">Du meddelas att avstängningen har initierats.</span><span class="sxs-lookup"><span data-stu-id="0732a-169">You will be notified that the shutdown has been initiated.</span></span>
   
    ![enheten avstängning igång](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="0732a-171">Enheten stängs nu.</span><span class="sxs-lookup"><span data-stu-id="0732a-171">The device will now shut down.</span></span> <span data-ttu-id="0732a-172">Om du vill starta enheten behöver du göra det via Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="0732a-172">If you want to start your device, you will need to do that through the Hyper-V Manager.</span></span>

#### <a name="to-restart-your-virtual-device"></a><span data-ttu-id="0732a-173">Starta om din virtuella enhet</span><span class="sxs-lookup"><span data-stu-id="0732a-173">To restart your virtual device</span></span>
1. <span data-ttu-id="0732a-174">Gå till i det lokala webbgränssnittet **Underhåll** > **energiinställningar**.</span><span class="sxs-lookup"><span data-stu-id="0732a-174">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="0732a-175">Längst ned på sidan klickar du på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="0732a-175">At the bottom of the page, click **Restart**.</span></span>
   
    ![omstart av enheten](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="0732a-177">En varning visas om att du startar om enheten att avbryta alla IOs som pågår, vilket resulterar i ett driftstopp.</span><span class="sxs-lookup"><span data-stu-id="0732a-177">A warning will appear stating that restarting the device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="0732a-178">Klicka på kryssikonen</span><span class="sxs-lookup"><span data-stu-id="0732a-178">Click the check icon</span></span> ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="0732a-180">.</span><span class="sxs-lookup"><span data-stu-id="0732a-180">.</span></span>
   
    ![Starta om varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="0732a-182">Du meddelas att omstarten har initierats.</span><span class="sxs-lookup"><span data-stu-id="0732a-182">You will be notified that the restart has been initiated.</span></span>
   
    ![Starta om initieras](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="0732a-184">När omstarten pågår förlorar anslutningen till Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0732a-184">While the restart is in progress, you will lose the connection to the UI.</span></span> <span data-ttu-id="0732a-185">Du kan övervaka omstarten genom att uppdatera Användargränssnittet med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="0732a-185">You can monitor the restart by refreshing the UI periodically.</span></span> <span data-ttu-id="0732a-186">Du kan också övervaka omstart Enhetsstatus via Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="0732a-186">Alternatively, you can monitor the device restart status through the Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0732a-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0732a-187">Next steps</span></span>
<span data-ttu-id="0732a-188">Lär dig hur du [använda StorSimple Manager-tjänsten för att hantera enheten](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="0732a-188">Learn how to [use the StorSimple Manager service to manage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

