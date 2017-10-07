---
title: "aaaInstall uppdateringar på virtuella StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur toouse hello virtuella StorSimple-matris web UI tooapply uppdateringar med hjälp av hello Azure portal och snabbkorrigeringen metod"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 6165b305fb0d404b404cf3a11dd0ade2f2f13b2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a><span data-ttu-id="ae776-103">Installera uppdateringen 0,4 på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="ae776-103">Install Update 0.4 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="ae776-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ae776-104">Overview</span></span>

<span data-ttu-id="ae776-105">Den här artikeln beskriver hello steg krävs tooinstall uppdatering 0,4 på din StorSimple virtuell matrisen via hello lokala webbgränssnittet och via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae776-105">This article describes hello steps required tooinstall Update 0.4 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="ae776-106">Du måste tooapply programvara uppdateringar eller snabbkorrigeringar tookeep din virtuella StorSimple-matris uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="ae776-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="ae776-107">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="ae776-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="ae776-108">Anges att hello virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="ae776-108">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="ae776-109">Innan du installerar en uppdatering rekommenderar vi att du vidta hello volymer eller resurser offline på hello värd först och sedan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="ae776-109">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="ae776-110">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="ae776-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae776-111">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda hello snabbkorrigeringen metoden via hello lokala web UI tooinstall uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="ae776-111">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall update 0.3.</span></span> <span data-ttu-id="ae776-112">Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du hello uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae776-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span>
 

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="ae776-113">Använd hello lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="ae776-113">Use hello local web UI</span></span>

<span data-ttu-id="ae776-114">Det finns två steg när du använder hello lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="ae776-114">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="ae776-115">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="ae776-115">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="ae776-116">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="ae776-116">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="ae776-117">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="ae776-117">Download hello update or hello hotfix</span></span>

<span data-ttu-id="ae776-118">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="ae776-118">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="ae776-119">toodownload hello uppdatering eller hello snabbkorrigering</span><span class="sxs-lookup"><span data-stu-id="ae776-119">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="ae776-120">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ae776-120">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="ae776-121">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ae776-121">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="ae776-122">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="ae776-122">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="ae776-123">Ange **3216577** för Update 0,4 och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="ae776-123">Enter **3216577** for Update 0.4, and then click **Search**.</span></span>
   
    <span data-ttu-id="ae776-124">hello snabbkorrigeringen visas, till exempel **StorSimple virtuell matris uppdatering 0,4**.</span><span class="sxs-lookup"><span data-stu-id="ae776-124">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.4**.</span></span>
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-04/download1.png)

4. <span data-ttu-id="ae776-126">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ae776-126">Click **Add**.</span></span> <span data-ttu-id="ae776-127">hello uppdateringen läggs toohello korg.</span><span class="sxs-lookup"><span data-stu-id="ae776-127">hello update is added toohello basket.</span></span>

5. <span data-ttu-id="ae776-128">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="ae776-128">Click **View Basket**.</span></span>

6. <span data-ttu-id="ae776-129">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="ae776-129">Click **Download**.</span></span> <span data-ttu-id="ae776-130">Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear.</span><span class="sxs-lookup"><span data-stu-id="ae776-130">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="ae776-131">hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello.</span><span class="sxs-lookup"><span data-stu-id="ae776-131">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="ae776-132">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="ae776-132">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

7. <span data-ttu-id="ae776-133">Öppna hello kopierade mappen, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="ae776-133">Open hello copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="ae776-134">Den här filen är används tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="ae776-134">This file is used tooinstall hello update or hotfix.</span></span>

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="ae776-135">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="ae776-135">Install hello update or hello hotfix</span></span>

<span data-ttu-id="ae776-136">Tidigare toohello uppdatering eller snabbkorrigering installation, kontrollera att du har hello uppdatering eller hello snabbkorrigering hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="ae776-136">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="ae776-137">Använd den här metoden tooinstall uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="ae776-137">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="ae776-138">Den här proceduren tar mindre än 2 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ae776-138">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="ae776-139">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="ae776-139">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="ae776-140">tooinstall hello uppdatering eller snabbkorrigering hello</span><span class="sxs-lookup"><span data-stu-id="ae776-140">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="ae776-141">Hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="ae776-141">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update1m.png)

2. <span data-ttu-id="ae776-143">I **uppdatering filsökväg**, ange hello filnamn för hello uppdatering eller hello snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="ae776-143">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="ae776-144">Du kan även bläddra toohello uppdatering eller snabbkorrigering installationsfilen om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="ae776-144">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="ae776-145">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="ae776-145">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update2m.png)

3. <span data-ttu-id="ae776-147">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="ae776-147">A warning is displayed.</span></span> <span data-ttu-id="ae776-148">Angivna detta är en enskild nod-enhet när hello uppdateringen har genomförts, hello enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="ae776-148">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="ae776-149">Klicka på kryssikonen hello.</span><span class="sxs-lookup"><span data-stu-id="ae776-149">Click hello check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update3m.png)

4. <span data-ttu-id="ae776-151">hello uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="ae776-151">hello update starts.</span></span> <span data-ttu-id="ae776-152">När hello enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="ae776-152">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="ae776-153">hello är lokala Användargränssnittet inte tillgänglig i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="ae776-153">hello local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update5m.png)

5. <span data-ttu-id="ae776-155">När hello omstarten är klar, tas du toohello **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="ae776-155">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="ae776-156">tooverify som hello enhetens programvara har uppdaterats i hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="ae776-156">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="ae776-157">hello visas programvaruversionen ska vara **10.0.0.0.0.10289.0** för uppdatering 0,4.</span><span class="sxs-lookup"><span data-stu-id="ae776-157">hello displayed software version should be **10.0.0.0.0.10289.0** for Update 0.4.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ae776-158">Vi rapporterar hello programvaruversioner på något annat sätt i hello lokala webbgränssnittet och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae776-158">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="ae776-159">Till exempel hello lokala UI-webbrapporter **10.0.0.0.0.10289** och hello Azure portal rapporter **10.0.10289.0** för hello samma version.</span><span class="sxs-lookup"><span data-stu-id="ae776-159">For example, hello local web UI reports **10.0.0.0.0.10289** and hello Azure portal reports **10.0.10289.0** for hello same version.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-hello-azure-portal"></a><span data-ttu-id="ae776-161">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ae776-161">Use hello Azure portal</span></span>

<span data-ttu-id="ae776-162">Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae776-162">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="ae776-163">hello portal proceduren kräver hello användaren tooscan, hämta och installera hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ae776-163">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="ae776-164">Den här proceduren tar cirka 7 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ae776-164">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="ae776-165">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="ae776-165">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="ae776-166">Efter hello är-installationen klar (som visas jobbets status till 100%), gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae776-166">After hello installation is complete (as indicated by job status at 100 %), go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="ae776-167">Välj **enheter** och markera och på hello enheten tooupdate hello listan över enheter anslutna toothis service.</span><span class="sxs-lookup"><span data-stu-id="ae776-167">Select **Devices** and then select and click hello device you want tooupdate from hello list of devices connected toothis service.</span></span> <span data-ttu-id="ae776-168">I hello **inställningar** bladet går för**hantera** avsnittet och väljer **enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="ae776-168">In hello **Settings** blade, go too**Manage** section and select **Device updates**.</span></span> <span data-ttu-id="ae776-169">hello visas programvaruversionen ska vara **10.0.10289.0**.</span><span class="sxs-lookup"><span data-stu-id="ae776-169">hello displayed software version should be **10.0.10289.0**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ae776-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae776-170">Next steps</span></span>

<span data-ttu-id="ae776-171">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="ae776-171">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

