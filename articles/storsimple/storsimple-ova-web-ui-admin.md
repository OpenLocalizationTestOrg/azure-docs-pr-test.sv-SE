---
title: aaaStorSimple virtuella matris web UI administration | Microsoft Docs
description: "Beskriver hur tooperform grundläggande enhet administrativa uppgifter via hello virtuella StorSimple-matris webbgränssnittet."
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
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a><span data-ttu-id="f3a86-103">Använd hello Webbgränssnittet tooadminister din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="f3a86-103">Use hello Web UI tooadminister your StorSimple Virtual Array</span></span>
![processflöde för installationen](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="f3a86-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="f3a86-105">Overview</span></span>
<span data-ttu-id="f3a86-106">hello självstudiekurser i den här artikeln gäller toohello Microsoft Azure StorSimple virtuell matris (även kallat hello lokala virtuella enheten StorSimple) körs mars 2016 allmän tillgänglighet (GA) versionen.</span><span class="sxs-lookup"><span data-stu-id="f3a86-106">hello tutorials in this article apply toohello Microsoft Azure StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="f3a86-107">Den här artikeln beskrivs några av hello komplexa arbetsflöden och administrativa uppgifter som kan utföras på hello virtuella StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="f3a86-107">This article describes some of hello complex workflows and management tasks that can be performed on hello StorSimple Virtual Array.</span></span> <span data-ttu-id="f3a86-108">Du kan hantera hello virtuella StorSimple-matris med hello StorSimple Manager-tjänsten Användargränssnittet (refererad tooas hello portalens användargränssnitt) och hello lokala webbgränssnittet för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-108">You can manage hello StorSimple Virtual Array using hello StorSimple Manager service UI (referred tooas hello portal UI) and hello local web UI for hello device.</span></span> <span data-ttu-id="f3a86-109">Den här artikeln fokuserar på hello aktiviteter som du kan utföra med hello webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-109">This article focuses on hello tasks that you can perform using hello web UI.</span></span>

<span data-ttu-id="f3a86-110">Den här artikeln innehåller hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="f3a86-110">This article includes hello following tutorials:</span></span>

* <span data-ttu-id="f3a86-111">Hämta krypteringsnyckel för tjänstdata hello</span><span class="sxs-lookup"><span data-stu-id="f3a86-111">Get hello service data encryption key</span></span>
* <span data-ttu-id="f3a86-112">Felsöka web UI installationen</span><span class="sxs-lookup"><span data-stu-id="f3a86-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="f3a86-113">Generera en logg-paket</span><span class="sxs-lookup"><span data-stu-id="f3a86-113">Generate a log package</span></span>
* <span data-ttu-id="f3a86-114">Stänga av eller starta om enheten</span><span class="sxs-lookup"><span data-stu-id="f3a86-114">Shut down or restart your device</span></span>

## <a name="get-hello-service-data-encryption-key"></a><span data-ttu-id="f3a86-115">Hämta krypteringsnyckel för tjänstdata hello</span><span class="sxs-lookup"><span data-stu-id="f3a86-115">Get hello service data encryption key</span></span>
<span data-ttu-id="f3a86-116">En krypteringsnyckel för tjänstdata ska genereras när du registrerar din första enhet med hello StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3a86-116">A service data encryption key is generated when you register your first device with hello StorSimple Manager service.</span></span> <span data-ttu-id="f3a86-117">Den här nyckeln är sedan krävs med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3a86-117">This key is then required with hello service registration key tooregister additional devices with hello StorSimple Manager service.</span></span>

<span data-ttu-id="f3a86-118">Om du har tappat bort din krypteringsnyckel för tjänstdata och behöver tooretrieve, utföra hello följande steg i hello lokala webbgränssnittet för hello enheten registreras med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f3a86-118">If you have misplaced your service data encryption key and need tooretrieve it, perform hello following steps in hello local web UI of hello device registered with your service.</span></span>

#### <a name="tooget-hello-service-data-encryption-key"></a><span data-ttu-id="f3a86-119">tooget hello krypteringsnyckel för tjänstdata</span><span class="sxs-lookup"><span data-stu-id="f3a86-119">tooget hello service data encryption key</span></span>
1. <span data-ttu-id="f3a86-120">Ansluta toohello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-120">Connect toohello local web UI.</span></span> <span data-ttu-id="f3a86-121">Gå för**Configuration** > **Molninställningar**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-121">Go too**Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="f3a86-122">Hello längst hello-sidan, klickar du på **hämta krypteringsnyckel för tjänstdata**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-122">At hello bottom of hello page, click **Get service data encryption key**.</span></span> <span data-ttu-id="f3a86-123">En nyckel visas.</span><span class="sxs-lookup"><span data-stu-id="f3a86-123">A key will appear.</span></span> <span data-ttu-id="f3a86-124">Kopiera och spara den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="f3a86-124">Copy and save this key.</span></span>
   
    ![Hämta krypteringsnyckel för tjänstdata 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="f3a86-126">Felsöka web UI installationen</span><span class="sxs-lookup"><span data-stu-id="f3a86-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="f3a86-127">I vissa fall när du konfigurerar hello enheten via hello lokala webbgränssnittet, kanske du stöter på fel.</span><span class="sxs-lookup"><span data-stu-id="f3a86-127">In some instances when you configure hello device through hello local web UI, you might run into errors.</span></span> <span data-ttu-id="f3a86-128">toodiagnose och felsökning av dessa fel kan du köra hello diagnostiktest.</span><span class="sxs-lookup"><span data-stu-id="f3a86-128">toodiagnose and troubleshoot such errors, you can run hello diagnostics tests.</span></span>

#### <a name="toorun-hello-diagnostic-tests"></a><span data-ttu-id="f3a86-129">toorun hello diagnostiktest</span><span class="sxs-lookup"><span data-stu-id="f3a86-129">toorun hello diagnostic tests</span></span>
1. <span data-ttu-id="f3a86-130">Hello lokala webbgränssnittet, gå för**felsökning** > **diagnostiska testerna**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-130">In hello local web UI, go too**Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![Kör diagnostik 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="f3a86-132">Hello längst hello-sidan, klickar du på **kör diagnostiktest**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-132">At hello bottom of hello page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="f3a86-133">Detta initierar testerna toodiagnose möjliga problem med ditt nätverk, enhet, webbproxy, tid eller molninställningar.</span><span class="sxs-lookup"><span data-stu-id="f3a86-133">This will initiate tests toodiagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="f3a86-134">Du meddelas att hello-enheten kör test.</span><span class="sxs-lookup"><span data-stu-id="f3a86-134">You will be notified that hello device is running tests.</span></span>
3. <span data-ttu-id="f3a86-135">Hello resultat visas när hello testerna har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f3a86-135">After hello tests have completed, hello results will be displayed.</span></span> <span data-ttu-id="f3a86-136">hello visar följande exempel hello resultaten av diagnostiska test.</span><span class="sxs-lookup"><span data-stu-id="f3a86-136">hello following example shows hello results of diagnostic tests.</span></span> <span data-ttu-id="f3a86-137">Observera att hello webbproxyinställningarna inte har konfigurerats på den här enheten och därför hello web proxy testet kördes inte.</span><span class="sxs-lookup"><span data-stu-id="f3a86-137">Note that hello web proxy settings were not configured on this device, and therefore, hello web proxy test was not run.</span></span> <span data-ttu-id="f3a86-138">Alla hello andra tester för nätverksinställningar, DNS-server och tidsinställningar lyckades.</span><span class="sxs-lookup"><span data-stu-id="f3a86-138">All hello other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![Kör diagnostik 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="f3a86-140">Generera en logg-paket</span><span class="sxs-lookup"><span data-stu-id="f3a86-140">Generate a log package</span></span>
<span data-ttu-id="f3a86-141">En logg paketet består av alla relevanta hello-loggar som kan hjälpa Microsoft-supporten att felsöka eventuella enhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="f3a86-141">A log package is comprised of all hello relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="f3a86-142">I den här versionen kan en logg-paketet skapas via hello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-142">In this release, a log package can be generated via hello local web UI.</span></span>

#### <a name="toogenerate-hello-log-package"></a><span data-ttu-id="f3a86-143">toogenerate hello loggen paketet</span><span class="sxs-lookup"><span data-stu-id="f3a86-143">toogenerate hello log package</span></span>
1. <span data-ttu-id="f3a86-144">Hello lokala webbgränssnittet, gå för**felsökning** > **systemloggar**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-144">In hello local web UI, go too**Troubleshooting** > **System logs**.</span></span>
   
    ![Generera loggen paketet 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="f3a86-146">Hello längst hello-sidan, klickar du på **skapa loggen paketet**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-146">At hello bottom of hello page, click **Create log package**.</span></span> <span data-ttu-id="f3a86-147">Ett paket med hello systemloggar kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="f3a86-147">A package of hello system logs will be created.</span></span> <span data-ttu-id="f3a86-148">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="f3a86-148">This will take a couple of minutes.</span></span>
   
    ![Generera loggen paketet 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="f3a86-150">Du meddelas när hello paketet har skapats och hello sidan uppdateras tooindicate hello datum och tid då hello paketet skapades.</span><span class="sxs-lookup"><span data-stu-id="f3a86-150">You will be notified after hello package is successfully created, and hello page will be updated tooindicate hello time and date when hello package was created.</span></span>
   
    ![Generera loggen paketet 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="f3a86-152">Klicka på **loggen hämtningspaketet**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-152">Click **Download log package**.</span></span> <span data-ttu-id="f3a86-153">Komprimerade paketet kommer att hämtas i systemet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-153">A zipped package will be downloaded on your system.</span></span>
   
    ![Generera loggen paketet 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="f3a86-155">Du kan packa hello hämtade loggen paketet och visa hello systemloggfilerna.</span><span class="sxs-lookup"><span data-stu-id="f3a86-155">You can unzip hello downloaded log package and view hello system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="f3a86-156">Stäng och starta om enheten</span><span class="sxs-lookup"><span data-stu-id="f3a86-156">Shut down and restart your device</span></span>
<span data-ttu-id="f3a86-157">Du kan stänga av eller starta om din virtuella enhet använder hello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-157">You can shut down or restart your virtual device using hello local web UI.</span></span> <span data-ttu-id="f3a86-158">Vi rekommenderar att innan du startar om ta hello volymer eller resurser offline på hello värden och hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-158">We recommend that before you restart, take hello volumes or shares offline on hello host and then hello device.</span></span> <span data-ttu-id="f3a86-159">Detta minimerar risken för varje möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="f3a86-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="tooshut-down-your-virtual-device"></a><span data-ttu-id="f3a86-160">tooshut ned din virtuella enhet</span><span class="sxs-lookup"><span data-stu-id="f3a86-160">tooshut down your virtual device</span></span>
1. <span data-ttu-id="f3a86-161">Hello lokala webbgränssnittet, gå för**Underhåll** > **energiinställningar**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-161">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="f3a86-162">Hello längst hello-sidan, klickar du på **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-162">At hello bottom of hello page, click **Shutdown**.</span></span>
   
    ![enheten avstängning 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="f3a86-164">En varning visas om att stängas av hello enheten kommer att avbryta alla i/o som pågår, vilket resulterar i ett driftstopp.</span><span class="sxs-lookup"><span data-stu-id="f3a86-164">A warning will appear stating that a shutdown of hello device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="f3a86-165">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="f3a86-165">Click hello check icon</span></span> ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="f3a86-167">.</span><span class="sxs-lookup"><span data-stu-id="f3a86-167">.</span></span>
   
    ![enheten avstängning varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="f3a86-169">Du meddelas att hello avsluta har initierats.</span><span class="sxs-lookup"><span data-stu-id="f3a86-169">You will be notified that hello shutdown has been initiated.</span></span>
   
    ![enheten avstängning igång](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="f3a86-171">hello enheten stängs nu.</span><span class="sxs-lookup"><span data-stu-id="f3a86-171">hello device will now shut down.</span></span> <span data-ttu-id="f3a86-172">Om du vill toostart enheten behöver toodo som via hello Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="f3a86-172">If you want toostart your device, you will need toodo that through hello Hyper-V Manager.</span></span>

#### <a name="toorestart-your-virtual-device"></a><span data-ttu-id="f3a86-173">toorestart din virtuella enhet</span><span class="sxs-lookup"><span data-stu-id="f3a86-173">toorestart your virtual device</span></span>
1. <span data-ttu-id="f3a86-174">Hello lokala webbgränssnittet, gå för**Underhåll** > **energiinställningar**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-174">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="f3a86-175">Hello längst hello-sidan, klickar du på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="f3a86-175">At hello bottom of hello page, click **Restart**.</span></span>
   
    ![omstart av enheten](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="f3a86-177">En varning visas om att starta om hello enheten kommer att avbryta alla IOs som pågår, vilket resulterar i ett driftstopp.</span><span class="sxs-lookup"><span data-stu-id="f3a86-177">A warning will appear stating that restarting hello device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="f3a86-178">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="f3a86-178">Click hello check icon</span></span> ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="f3a86-180">.</span><span class="sxs-lookup"><span data-stu-id="f3a86-180">.</span></span>
   
    ![Starta om varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="f3a86-182">Du meddelas att hello omstart har initierats.</span><span class="sxs-lookup"><span data-stu-id="f3a86-182">You will be notified that hello restart has been initiated.</span></span>
   
    ![Starta om initieras](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="f3a86-184">När hello omstart pågår förlorar hello anslutning toohello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3a86-184">While hello restart is in progress, you will lose hello connection toohello UI.</span></span> <span data-ttu-id="f3a86-185">Du kan övervaka hello omstart genom att uppdatera hello användargränssnitt med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="f3a86-185">You can monitor hello restart by refreshing hello UI periodically.</span></span> <span data-ttu-id="f3a86-186">Du kan också övervaka hello omstart Enhetsstatus via hello Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="f3a86-186">Alternatively, you can monitor hello device restart status through hello Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3a86-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3a86-187">Next steps</span></span>
<span data-ttu-id="f3a86-188">Lär dig hur för[Använd hello StorSimple Manager service toomanage enheten](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f3a86-188">Learn how too[use hello StorSimple Manager service toomanage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

