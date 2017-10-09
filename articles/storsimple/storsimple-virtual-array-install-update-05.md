---
title: "aaaInstall uppdatering 0,5 på virtuella StorSimple-matrisen | Microsoft Docs"
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
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: c38daa85daa0086e67cf0206d76cb19d9c8b21b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a><span data-ttu-id="2c04e-103">Installera uppdateringen 0,5 på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="2c04e-103">Install Update 0.5 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="2c04e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2c04e-104">Overview</span></span>

<span data-ttu-id="2c04e-105">Den här artikeln beskriver hello steg krävs tooinstall uppdatering 0,5 på din StorSimple virtuell matrisen via hello lokala webbgränssnittet och via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-105">This article describes hello steps required tooinstall Update 0.5 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="2c04e-106">Du måste tooapply programvara uppdateringar eller snabbkorrigeringar tookeep din virtuella StorSimple-matris uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="2c04e-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="2c04e-107">Innan du installerar en uppdatering rekommenderar vi att du vidta hello volymer eller resurser offline på hello värd först och sedan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2c04e-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="2c04e-108">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="2c04e-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="2c04e-109">När hello volymer eller resurser är offline kan bör du också utföra en manuell säkerhetskopiering av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2c04e-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="2c04e-110">Uppdatera 0,5 motsvarar för**10.0.10290.0** programvaruversionen på enheten.</span><span class="sxs-lookup"><span data-stu-id="2c04e-110">Update 0.5 corresponds too**10.0.10290.0** software version on your device.</span></span> <span data-ttu-id="2c04e-111">Information om vad är nytt i den här uppdateringen finns för[viktig information för uppdatering 0,5](storsimple-virtual-array-update-05-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="2c04e-111">For information on what is new in this update, go too[Release notes for Update 0.5](storsimple-virtual-array-update-05-release-notes.md).</span></span>
>
> - <span data-ttu-id="2c04e-112">Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du hello uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="2c04e-113">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda hello snabbkorrigeringen metoden via hello lokala web UI tooinstall uppdatering 0,5.</span><span class="sxs-lookup"><span data-stu-id="2c04e-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.5.</span></span>
>
> - <span data-ttu-id="2c04e-114">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="2c04e-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="2c04e-115">Anges att hello virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="2c04e-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="2c04e-116">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2c04e-116">Use hello Azure portal</span></span>

<span data-ttu-id="2c04e-117">Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="2c04e-118">hello portal proceduren kräver hello användaren tooscan, hämta och installera hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="2c04e-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="2c04e-119">Den här proceduren tar cirka 7 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2c04e-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="2c04e-120">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2c04e-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="2c04e-121">Efter hello är-installationen klar, går tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2c04e-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="2c04e-122">Välj **enheter** och markera och på hello-enhet som du precis har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="2c04e-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="2c04e-123">Gå för**Inställningar > Hantera > Enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="2c04e-124">hello visas programvaruversionen ska vara **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-124">hello displayed software version should be **10.0.10290.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="2c04e-125">Använd hello lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="2c04e-125">Use hello local web UI</span></span>

<span data-ttu-id="2c04e-126">Det finns två steg när du använder hello lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="2c04e-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="2c04e-127">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="2c04e-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="2c04e-128">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="2c04e-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="2c04e-129">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="2c04e-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="2c04e-130">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="2c04e-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="2c04e-131">toodownload hello uppdatering eller hello snabbkorrigering</span><span class="sxs-lookup"><span data-stu-id="2c04e-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="2c04e-132">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="2c04e-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="2c04e-133">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-133">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="2c04e-134">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="2c04e-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="2c04e-135">Ange **4021576** för Update 0,5 och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-135">Enter **4021576** for Update 0.5, and then click **Search**.</span></span>
   
    <span data-ttu-id="2c04e-136">hello snabbkorrigeringen visas, till exempel **StorSimple virtuell matris uppdatering 0,5**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.5**.</span></span>
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-05/download1.png)

4. <span data-ttu-id="2c04e-138">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-138">Click **Download**.</span></span> 

5. <span data-ttu-id="2c04e-139">Du bör se två filer toodownload en *.msu* och en *CAB* fil.</span><span class="sxs-lookup"><span data-stu-id="2c04e-139">You should see two files toodownload, a *.msu* and a *.cab* file.</span></span> <span data-ttu-id="2c04e-140">Hämta var och en av dessa tooa-mappen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="2c04e-141">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2c04e-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="2c04e-142">Öppna hello mapp där hello filerna lagras.</span><span class="sxs-lookup"><span data-stu-id="2c04e-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="2c04e-143">![Filer i hello-paket](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span><span class="sxs-lookup"><span data-stu-id="2c04e-143">![Files in hello package](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span></span>

    <span data-ttu-id="2c04e-144">Du ser:</span><span class="sxs-lookup"><span data-stu-id="2c04e-144">You see:</span></span>
    -  <span data-ttu-id="2c04e-145">En paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="2c04e-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="2c04e-146">Den här filen är används tooupdate hello enhetens programvara.</span><span class="sxs-lookup"><span data-stu-id="2c04e-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="2c04e-147">En paketfil Geneva Monitoring Agent `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="2c04e-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="2c04e-148">Den här filen finns används tooupdate hello övervaknings- och diagnostikfunktionerna service (MDS)-agent.</span><span class="sxs-lookup"><span data-stu-id="2c04e-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="2c04e-149">Dubbelklicka på hello cab-filen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-149">Double-click hello cab file.</span></span> <span data-ttu-id="2c04e-150">En .msi visas.</span><span class="sxs-lookup"><span data-stu-id="2c04e-150">A .msi is displayed.</span></span> <span data-ttu-id="2c04e-151">Välj hello-fil, högerklicka och sedan **extrahera** hello-filen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="2c04e-152">Du kommer att använda hello _.msi_ file tooupdate hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="2c04e-152">You will use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Extrahera uppdateringsfil MDS-agenten](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="2c04e-154">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="2c04e-154">Install hello update or hello hotfix</span></span>

<span data-ttu-id="2c04e-155">Tidigare toohello uppdatering eller snabbkorrigering installation, kontrollera att du har hello uppdatering eller hello snabbkorrigering hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="2c04e-155">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="2c04e-156">Använd den här metoden tooinstall uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="2c04e-156">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="2c04e-157">Den här proceduren tar mindre än 2 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2c04e-157">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="2c04e-158">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2c04e-158">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="2c04e-159">tooinstall hello uppdatering eller snabbkorrigering hello</span><span class="sxs-lookup"><span data-stu-id="2c04e-159">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="2c04e-160">Hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-160">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="2c04e-162">I **uppdatering filsökväg**, ange hello filnamn för hello uppdatering eller hello snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2c04e-162">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="2c04e-163">Du kan även bläddra toohello uppdatering eller snabbkorrigering installationsfilen om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="2c04e-163">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="2c04e-164">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-164">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="2c04e-166">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="2c04e-166">A warning is displayed.</span></span> <span data-ttu-id="2c04e-167">Angivna detta är en enskild nod-enhet när hello uppdateringen har genomförts, hello enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="2c04e-167">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="2c04e-168">Klicka på kryssikonen hello.</span><span class="sxs-lookup"><span data-stu-id="2c04e-168">Click hello check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="2c04e-170">hello uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="2c04e-170">hello update starts.</span></span> <span data-ttu-id="2c04e-171">När hello enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="2c04e-171">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="2c04e-172">hello är lokala Användargränssnittet inte tillgänglig i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="2c04e-172">hello local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="2c04e-174">När hello omstarten är klar, tas du toohello **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="2c04e-174">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="2c04e-175">tooverify som hello enhetens programvara har uppdaterats i hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="2c04e-175">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="2c04e-176">hello visas programvaruversionen ska vara **10.0.0.0.0.10290.0** för uppdatering 0,5.</span><span class="sxs-lookup"><span data-stu-id="2c04e-176">hello displayed software version should be **10.0.0.0.0.10290.0** for Update 0.5.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2c04e-177">Vi rapporterar hello programvaruversioner på något annat sätt i hello lokala webbgränssnittet och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c04e-177">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="2c04e-178">Till exempel hello lokala UI-webbrapporter **10.0.0.0.0.10290** och hello Azure portal rapporter **10.0.10290.0** för hello samma version.</span><span class="sxs-lookup"><span data-stu-id="2c04e-178">For example, hello local web UI reports **10.0.0.0.0.10290** and hello Azure portal reports **10.0.10290.0** for hello same version.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. <span data-ttu-id="2c04e-180">hello nästa steg är tooupdate hello MDS-agenten.</span><span class="sxs-lookup"><span data-stu-id="2c04e-180">hello next step is tooupdate hello MDS agent.</span></span> <span data-ttu-id="2c04e-181">I hello **programuppdateringen** sidan finns toohello **uppdatering filsökväg** och bläddra toohello `GenevaMonitoringAgentPackageInstaller.msi` fil.</span><span class="sxs-lookup"><span data-stu-id="2c04e-181">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="2c04e-182">Upprepa steg 2-4.</span><span class="sxs-lookup"><span data-stu-id="2c04e-182">Repeat steps 2-4.</span></span> <span data-ttu-id="2c04e-183">När hello virtuella matris har startat om, logga in på hello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2c04e-183">After hello virtual array restarts, sign into hello local web UI.</span></span>

<span data-ttu-id="2c04e-184">hello uppdateringen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="2c04e-184">hello update is now complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c04e-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c04e-185">Next steps</span></span>

<span data-ttu-id="2c04e-186">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="2c04e-186">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

