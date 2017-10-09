---
title: "aaaInstall uppdatering 0,6 på virtuella StorSimple-matrisen | Microsoft Docs"
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 2ccd1b5fc1957c35ebec35aa947d331b3ff05464
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a><span data-ttu-id="da5ae-103">Installera uppdateringen 0,6 på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="da5ae-103">Install Update 0.6 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="da5ae-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="da5ae-104">Overview</span></span>

<span data-ttu-id="da5ae-105">Den här artikeln beskriver hello steg krävs tooinstall uppdatering 0,6 på din StorSimple virtuell matrisen via hello lokala webbgränssnittet och via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-105">This article describes hello steps required tooinstall Update 0.6 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="da5ae-106">Du använder hello programvara uppdateringar eller snabbkorrigeringar tookeep StorSimple virtuell matrisen uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="da5ae-106">You apply hello software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="da5ae-107">Innan du installerar en uppdatering rekommenderar vi att du vidta hello volymer eller resurser offline på hello värd först och sedan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="da5ae-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="da5ae-108">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="da5ae-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="da5ae-109">När hello volymer eller resurser är offline kan bör du också utföra en manuell säkerhetskopiering av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="da5ae-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="da5ae-110">Uppdatera 0,6 motsvarar för**10.0.10293.0** programvaruversionen på enheten.</span><span class="sxs-lookup"><span data-stu-id="da5ae-110">Update 0.6 corresponds too**10.0.10293.0** software version on your device.</span></span> <span data-ttu-id="da5ae-111">Information om vad är nytt i den här uppdateringen finns för[viktig information för uppdatering 0,6](storsimple-virtual-array-update-06-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="da5ae-111">For information on what is new in this update, go too[Release notes for Update 0.6](storsimple-virtual-array-update-06-release-notes.md).</span></span>
>
> - <span data-ttu-id="da5ae-112">Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du hello uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="da5ae-113">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda hello snabbkorrigeringen metoden via hello lokala web UI tooinstall uppdatering 0,6.</span><span class="sxs-lookup"><span data-stu-id="da5ae-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.6.</span></span>
>
> - <span data-ttu-id="da5ae-114">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="da5ae-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="da5ae-115">Anges att hello virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="da5ae-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="da5ae-116">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="da5ae-116">Use hello Azure portal</span></span>

<span data-ttu-id="da5ae-117">Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="da5ae-118">hello portal proceduren kräver hello användaren tooscan, hämta och installera hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="da5ae-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="da5ae-119">Den här proceduren tar cirka 7 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="da5ae-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="da5ae-120">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="da5ae-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="da5ae-121">Efter hello är-installationen klar, går tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="da5ae-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="da5ae-122">Välj **enheter** och markera och på hello-enhet som du precis har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="da5ae-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="da5ae-123">Gå för**Inställningar > Hantera > Enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="da5ae-124">hello visas programvaruversionen ska vara **10.0.10293.0**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-124">hello displayed software version should be **10.0.10293.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="da5ae-125">Använd hello lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="da5ae-125">Use hello local web UI</span></span>

<span data-ttu-id="da5ae-126">Det finns två steg när du använder hello lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="da5ae-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="da5ae-127">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="da5ae-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="da5ae-128">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="da5ae-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="da5ae-129">Hämta hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="da5ae-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="da5ae-130">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="da5ae-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="da5ae-131">toodownload hello uppdatering eller hello snabbkorrigering</span><span class="sxs-lookup"><span data-stu-id="da5ae-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="da5ae-132">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="da5ae-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="da5ae-133">Om du använder hello Microsoft Update-katalogen för hello första gången på den här datorn, klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-133">If you are using hello Microsoft Update Catalog for hello first time on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="da5ae-134">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="da5ae-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="da5ae-135">Ange **4023268** för Update 0,6 och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-135">Enter **4023268** for Update 0.6, and then click **Search**.</span></span>
   
    <span data-ttu-id="da5ae-136">hello snabbkorrigeringen visas, till exempel **StorSimple virtuell matris uppdatering 0,6**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.6**.</span></span>
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-06/download1.png)

4. <span data-ttu-id="da5ae-138">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-138">Click **Download**.</span></span>

5. <span data-ttu-id="da5ae-139">Du bör se fem filer toodownload.</span><span class="sxs-lookup"><span data-stu-id="da5ae-139">You should see five files toodownload.</span></span> <span data-ttu-id="da5ae-140">Hämta var och en av dessa tooa-mappen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="da5ae-141">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="da5ae-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="da5ae-142">Öppna hello mapp där hello filerna lagras.</span><span class="sxs-lookup"><span data-stu-id="da5ae-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="da5ae-143">![Filer i hello-paket](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span><span class="sxs-lookup"><span data-stu-id="da5ae-143">![Files in hello package](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span></span>

    <span data-ttu-id="da5ae-144">Du ser:</span><span class="sxs-lookup"><span data-stu-id="da5ae-144">You see:</span></span>
    -  <span data-ttu-id="da5ae-145">En paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="da5ae-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="da5ae-146">Den här filen är används tooupdate hello enhetens programvara.</span><span class="sxs-lookup"><span data-stu-id="da5ae-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="da5ae-147">En paketfil Geneva Monitoring Agent `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="da5ae-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="da5ae-148">Den här filen finns används tooupdate hello övervaknings- och diagnostikfunktionerna service (MDS)-agent.</span><span class="sxs-lookup"><span data-stu-id="da5ae-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="da5ae-149">Dubbelklicka på hello cab-filen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-149">Double-click hello cab file.</span></span> <span data-ttu-id="da5ae-150">En _.msi_ visas.</span><span class="sxs-lookup"><span data-stu-id="da5ae-150">A _.msi_ is displayed.</span></span> <span data-ttu-id="da5ae-151">Välj hello-fil, högerklicka och sedan **extrahera** hello-filen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="da5ae-152">Du använder hello _.msi_ file tooupdate hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="da5ae-152">You use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Extrahera uppdateringsfil MDS-agenten](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > <span data-ttu-id="da5ae-154">Du behöver inte tooupdate hello MDS-agent om du kör StorSimple uppdatering 0,5 (0.0.10293.0).</span><span class="sxs-lookup"><span data-stu-id="da5ae-154">You do not need tooupdate hello MDS agent if you are running StorSimple Update 0.5 (0.0.10293.0).</span></span>

    - <span data-ttu-id="da5ae-155">Tre filer som innehåller viktiga uppdateringar för Windows-säkerhet, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, och `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="da5ae-155">Three files that contain critical Windows security updates, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span>


### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="da5ae-156">Installera hello uppdatering eller snabbkorrigering för hello</span><span class="sxs-lookup"><span data-stu-id="da5ae-156">Install hello update or hello hotfix</span></span>

<span data-ttu-id="da5ae-157">Tidigare toohello uppdatering eller snabbkorrigering installation, kontrollera att du har hello uppdatering eller hello snabbkorrigering hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="da5ae-157">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="da5ae-158">Använd den här metoden tooinstall uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="da5ae-158">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="da5ae-159">Den här proceduren tar cirka 12 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="da5ae-159">This procedure takes approximately 12 minutes toocomplete.</span></span> <span data-ttu-id="da5ae-160">Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="da5ae-160">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="da5ae-161">tooinstall hello uppdatering eller snabbkorrigering hello</span><span class="sxs-lookup"><span data-stu-id="da5ae-161">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="da5ae-162">Hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-162">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="da5ae-163">Anteckna hello programvaruversion av som du kör.</span><span class="sxs-lookup"><span data-stu-id="da5ae-163">Make a note of hello software version that you are running.</span></span> <span data-ttu-id="da5ae-164">Om du kör **10.0.10290.0**, behöver du inte tooupdate hello MDS-agenten i steg 6.</span><span class="sxs-lookup"><span data-stu-id="da5ae-164">If you are running **10.0.10290.0**, you do not need tooupdate hello MDS agent in step 6.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="da5ae-166">I **uppdatering filsökväg**, ange hello filnamn för hello uppdatering eller hello snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="da5ae-166">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="da5ae-167">Du kan även bläddra toohello uppdatering eller snabbkorrigering installationsfilen om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="da5ae-167">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="da5ae-168">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-168">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="da5ae-170">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="da5ae-170">A warning is displayed.</span></span> <span data-ttu-id="da5ae-171">Det angivna hello är virtuella matris en enskild nod-enhet när hello uppdateringen har genomförts, hello enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="da5ae-171">Given hello virtual array is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="da5ae-172">Klicka på kryssikonen hello.</span><span class="sxs-lookup"><span data-stu-id="da5ae-172">Click hello check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="da5ae-174">hello uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="da5ae-174">hello update starts.</span></span> <span data-ttu-id="da5ae-175">När hello enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="da5ae-175">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="da5ae-176">hello är lokala Användargränssnittet inte tillgänglig i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="da5ae-176">hello local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="da5ae-178">När hello omstarten är klar, tas du toohello **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="da5ae-178">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="da5ae-179">tooverify som hello enhetens programvara har uppdaterats i hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="da5ae-179">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="da5ae-180">hello visas programvaruversionen ska vara **10.0.0.0.0.10293** för uppdatering 0,6.</span><span class="sxs-lookup"><span data-stu-id="da5ae-180">hello displayed software version should be **10.0.0.0.0.10293** for Update 0.6.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="da5ae-181">Vi rapporterar hello programvaruversioner på något annat sätt i hello lokala webbgränssnittet och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-181">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="da5ae-182">Till exempel hello lokala UI-webbrapporter **10.0.0.0.0.10293** och hello Azure portal rapporter **10.0.10293.0** för hello samma version.</span><span class="sxs-lookup"><span data-stu-id="da5ae-182">For example, hello local web UI reports **10.0.0.0.0.10293** and hello Azure portal reports **10.0.10293.0** for hello same version.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. <span data-ttu-id="da5ae-184">Hoppa över detta steg om du körde StorSimple virtuell matris uppdatering 0,5 (**10.0.10290.0**) innan du installerat den här uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="da5ae-184">Skip this step if you were running StorSimple Virtual Array Update 0.5 (**10.0.10290.0**) before you applied this update.</span></span> <span data-ttu-id="da5ae-185">Du antecknade hello programvaruversionen i steg 1 innan du startade tooupdate.</span><span class="sxs-lookup"><span data-stu-id="da5ae-185">You made a note of hello software version in step 1 before you began tooupdate.</span></span> <span data-ttu-id="da5ae-186">Om du körde uppdateringen 0,5 är MDS-agenten redan uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="da5ae-186">If you were running Update 0.5, your MDS agent is already up-to-date .</span></span>

    <span data-ttu-id="da5ae-187">Om du kör en programvara version tidigare tooUpdate 0,5 är hello nästa steg du tooupdate hello MDS-agenten.</span><span class="sxs-lookup"><span data-stu-id="da5ae-187">If you are running a software version prior tooUpdate 0.5, hello next step for you is tooupdate hello MDS agent.</span></span> <span data-ttu-id="da5ae-188">I hello **programuppdateringen** sidan finns toohello **uppdatering filsökväg** och bläddra toohello `GenevaMonitoringAgentPackageInstaller.msi` fil.</span><span class="sxs-lookup"><span data-stu-id="da5ae-188">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="da5ae-189">Upprepa steg 2-4.</span><span class="sxs-lookup"><span data-stu-id="da5ae-189">Repeat steps 2-4.</span></span> <span data-ttu-id="da5ae-190">När hello virtuella matris har startat om, logga in på hello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="da5ae-190">After hello virtual array restarts, sign into hello local web UI.</span></span>

7. <span data-ttu-id="da5ae-191">Upprepa steg 2-4 tooinstall hello Windows säkerhetskorrigeringar använder filer `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, och `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="da5ae-191">Repeat step 2-4 tooinstall hello Windows security fixes using files `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span> <span data-ttu-id="da5ae-192">hello virtuella matris startas om efter varje installation och du behöver toosign hello lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="da5ae-192">hello virtual array restarts after each install and you need toosign into hello local web UI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da5ae-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da5ae-193">Next steps</span></span>

<span data-ttu-id="da5ae-194">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="da5ae-194">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

