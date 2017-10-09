---
title: "aaaInstall uppdateringar på en virtuell StorSimple-matris | Microsoft Docs"
description: Beskriver hur toouse hello virtuella StorSimple-matris web UI tooapply uppdaterar metoden hello portal och snabbkorrigering
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7f1b1e7d-04d0-4ca2-9dbb-77077ff19bb9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/07/2016
ms.author: alkohli
ms.openlocfilehash: 10af0f52abb75a5b41562704194157f0d35710bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a><span data-ttu-id="6cb01-103">Installera uppdateringar på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="6cb01-103">Install Updates on your StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="6cb01-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6cb01-104">Overview</span></span>
<span data-ttu-id="6cb01-105">Den här artikeln beskrivs hello steg krävs tooinstall uppdateringar på din StorSimple virtuell matrisen via hello lokala webbgränssnittet och via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6cb01-105">This article describes hello steps required tooinstall updates on your StorSimple Virtual Array via hello local web UI and via hello Azure classic portal.</span></span> <span data-ttu-id="6cb01-106">Du måste tooapply programvara uppdateringar eller snabbkorrigeringar tookeep din virtuella StorSimple-matris uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="6cb01-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="6cb01-107">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="6cb01-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="6cb01-108">Anges att hello virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="6cb01-108">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="6cb01-109">Innan du installerar en uppdatering rekommenderar vi att du vidta hello volymer eller resurser offline på hello värd först och sedan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="6cb01-109">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="6cb01-110">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="6cb01-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6cb01-111">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda hello snabbkorrigeringen metoden via hello lokala web UI tooinstall uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="6cb01-111">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall update 0.3.</span></span> <span data-ttu-id="6cb01-112">Om du kör uppdatering 0,2 rekommenderar vi att du installerar hello uppdateringar via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6cb01-112">If you are running Update 0.2, we recommend that you install hello updates via hello Azure classic portal.</span></span>
> 
> 

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="6cb01-113">Använd hello lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="6cb01-113">Use hello local web UI</span></span>
<span data-ttu-id="6cb01-114">Det finns två steg när du använder hello lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="6cb01-114">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="6cb01-115">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="6cb01-115">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="6cb01-116">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="6cb01-116">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="6cb01-117">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="6cb01-117">Download hello update or hello hotfix</span></span>
<span data-ttu-id="6cb01-118">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="6cb01-118">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="6cb01-119">toodownload hello uppdatering eller hello snabbkorrigering</span><span class="sxs-lookup"><span data-stu-id="6cb01-119">toodownload hello update or hello hotfix</span></span>
1. <span data-ttu-id="6cb01-120">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="6cb01-120">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="6cb01-121">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="6cb01-121">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
3. <span data-ttu-id="6cb01-122">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="6cb01-122">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="6cb01-123">Ange **3182061** uppdatering 0.3 för och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-123">Enter **3182061** for Update 0.3, and then click **Search**.</span></span>
   
    <span data-ttu-id="6cb01-124">hello snabbkorrigeringen visas, till exempel **StorSimple virtuell matris uppdatering 0.3**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-124">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.3**.</span></span>
   
    ![Sökkatalog](./media/storsimple-ova-install-update-01/download1.png)
4. <span data-ttu-id="6cb01-126">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-126">Click **Add**.</span></span> <span data-ttu-id="6cb01-127">hello uppdateringen läggs toohello korg.</span><span class="sxs-lookup"><span data-stu-id="6cb01-127">hello update is added toohello basket.</span></span>
5. <span data-ttu-id="6cb01-128">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="6cb01-128">Click **View Basket**.</span></span>
6. <span data-ttu-id="6cb01-129">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-129">Click **Download**.</span></span> <span data-ttu-id="6cb01-130">Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear.</span><span class="sxs-lookup"><span data-stu-id="6cb01-130">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="6cb01-131">hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello.</span><span class="sxs-lookup"><span data-stu-id="6cb01-131">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="6cb01-132">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="6cb01-132">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>
7. <span data-ttu-id="6cb01-133">Öppna hello kopierade mappen, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="6cb01-133">Open hello copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="6cb01-134">Den här filen är används tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="6cb01-134">This file is used tooinstall hello update or hotfix.</span></span>

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="6cb01-135">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="6cb01-135">Install hello update or hello hotfix</span></span>
<span data-ttu-id="6cb01-136">Tidigare toohello uppdatering eller snabbkorrigering installation, kontrollera att du har hello uppdatering eller hello snabbkorrigering hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="6cb01-136">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="6cb01-137">Använd den här metoden tooinstall uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="6cb01-137">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="6cb01-138">Den här proceduren tar mindre än 2 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6cb01-138">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="6cb01-139">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="6cb01-139">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="6cb01-140">tooinstall hello uppdatering eller snabbkorrigering hello</span><span class="sxs-lookup"><span data-stu-id="6cb01-140">tooinstall hello update or hello hotfix</span></span>
1. <span data-ttu-id="6cb01-141">Hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-141">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update1m.png)
2. <span data-ttu-id="6cb01-143">I **uppdatering filsökväg**, ange hello filnamn för hello uppdatering eller hello snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="6cb01-143">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="6cb01-144">Du kan även bläddra toohello uppdatering eller snabbkorrigering installationsfilen om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="6cb01-144">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="6cb01-145">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-145">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update2m.png)
3. <span data-ttu-id="6cb01-147">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="6cb01-147">A warning is displayed.</span></span> <span data-ttu-id="6cb01-148">Angivna detta är en enskild nod-enhet när hello uppdateringen har genomförts, hello enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="6cb01-148">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="6cb01-149">Klicka på kryssikonen hello.</span><span class="sxs-lookup"><span data-stu-id="6cb01-149">Click hello check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-ova-install-update-01/update3m.png)
4. <span data-ttu-id="6cb01-151">hello uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="6cb01-151">hello update starts.</span></span> <span data-ttu-id="6cb01-152">När hello enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="6cb01-152">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="6cb01-153">hello är lokala Användargränssnittet inte tillgänglig i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="6cb01-153">hello local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update5m.png)
5. <span data-ttu-id="6cb01-155">När hello omstarten är klar, tas du toohello **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="6cb01-155">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="6cb01-156">tooverify som hello enhetens programvara har uppdaterats i hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-156">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="6cb01-157">hello visas programvaruversionen ska vara **10.0.0.0.0.10288.0** för uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="6cb01-157">hello displayed software version should be **10.0.0.0.0.10288.0** for Update 0.3.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6cb01-158">Vi rapporterar hello programvaruversioner på något annat sätt i hello lokala webbgränssnittet och hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6cb01-158">We report hello software versions in a slightly different way in hello local web UI and hello Azure classic portal.</span></span> <span data-ttu-id="6cb01-159">Till exempel hello lokala UI-webbrapporter **10.0.0.0.0.10288** och hello Azure klassiska portal rapporter **10.0.10288.0** för hello samma version.</span><span class="sxs-lookup"><span data-stu-id="6cb01-159">For example, hello local web UI reports **10.0.0.0.0.10288** and hello Azure classic portal reports **10.0.10288.0** for hello same version.</span></span> 
   > 
   > 
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-hello-azure-classic-portal"></a><span data-ttu-id="6cb01-161">Använd hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6cb01-161">Use hello Azure classic portal</span></span>
<span data-ttu-id="6cb01-162">Om du kör uppdatering 0,2, rekommenderar vi att du installerar uppdateringar via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6cb01-162">If running Update 0.2, we recommend that you install updates through hello Azure classic portal.</span></span> <span data-ttu-id="6cb01-163">hello portal proceduren kräver hello användaren tooscan, hämta och installera hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="6cb01-163">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="6cb01-164">Den här proceduren tar cirka 7 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6cb01-164">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="6cb01-165">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="6cb01-165">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

<span data-ttu-id="6cb01-166">Efter hello installationen är klar (vilket indikeras med jobbstatus till 100%) finns för**enheter > Underhåll > programuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="6cb01-166">After hello installation is complete (as indicated by job status at 100 %), go too**Devices > Maintenance > Software Updates**.</span></span> <span data-ttu-id="6cb01-167">hello visas programvaruversionen ska 10.0.10288.0.</span><span class="sxs-lookup"><span data-stu-id="6cb01-167">hello displayed software version should be 10.0.10288.0.</span></span>

![uppdatera enhet](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a><span data-ttu-id="6cb01-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6cb01-169">Next steps</span></span>
<span data-ttu-id="6cb01-170">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="6cb01-170">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

