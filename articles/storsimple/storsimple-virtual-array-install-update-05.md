---
title: "Installera uppdatering 0,5 på virtuella StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur du använder virtuella StorSimple-matris webbgränssnittet för att tillämpa uppdateringar med hjälp av Azure portal och snabbkorrigering metoden"
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
ms.openlocfilehash: c47da5b90c16e2d5b5709e2a6affc026238b9468
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a><span data-ttu-id="d1c69-103">Installera uppdateringen 0,5 på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="d1c69-103">Install Update 0.5 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="d1c69-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d1c69-104">Overview</span></span>

<span data-ttu-id="d1c69-105">Den här artikeln beskriver de steg som krävs för att installera uppdateringen 0,5 på din virtuella StorSimple-matrisen via lokala webbgränssnittet och via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="d1c69-105">This article describes the steps required to install Update 0.5 on your StorSimple Virtual Array via the local web UI and via the Azure portal.</span></span> <span data-ttu-id="d1c69-106">Du måste tillämpa uppdateringar eller snabbkorrigeringar för att hålla det uppdaterat din virtuella StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="d1c69-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="d1c69-107">Innan du installerar en uppdatering, rekommenderar vi att du vidtar volymer eller resurser offline på värden först och sedan enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-107">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="d1c69-108">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="d1c69-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="d1c69-109">När volymer eller resurser som är offline, bör du också se en manuell säkerhetskopiering av enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-109">After the volumes or shares are offline, you should also take a manual backup of the device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="d1c69-110">Uppdatera 0,5 motsvarar **10.0.10290.0** programvaruversionen på enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-110">Update 0.5 corresponds to **10.0.10290.0** software version on your device.</span></span> <span data-ttu-id="d1c69-111">För information om vad är nytt i den här uppdateringen, gå till [viktig information för uppdatering 0,5](storsimple-virtual-array-update-05-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="d1c69-111">For information on what is new in this update, go to [Release notes for Update 0.5](storsimple-virtual-array-update-05-release-notes.md).</span></span>
>
> - <span data-ttu-id="d1c69-112">Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du uppdateringar via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="d1c69-112">If you are running Update 0.2 or later, we recommend that you install the updates via the Azure portal.</span></span> <span data-ttu-id="d1c69-113">Om du kör uppdatering 0.1 eller GA programvaruversioner måste du använda metoden snabbkorrigeringen via lokala webbgränssnittet för att installera uppdateringen 0,5.</span><span class="sxs-lookup"><span data-stu-id="d1c69-113">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install Update 0.5.</span></span>
>
> - <span data-ttu-id="d1c69-114">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="d1c69-115">Med hänsyn till att den virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="d1c69-115">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-the-azure-portal"></a><span data-ttu-id="d1c69-116">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d1c69-116">Use the Azure portal</span></span>

<span data-ttu-id="d1c69-117">Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-117">If running Update 0.2 and later, we recommend that you install updates through the Azure portal.</span></span> <span data-ttu-id="d1c69-118">Portalen proceduren kräver att användaren skanna, hämta och installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="d1c69-118">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="d1c69-119">Den här proceduren tar cirka 7 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d1c69-119">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="d1c69-120">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="d1c69-120">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="d1c69-121">När installationen är klar, gå till Enhetshanteraren för StorSimple-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-121">After the installation is complete, go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="d1c69-122">Välj **enheter** och markera och klicka på den enhet som du precis har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="d1c69-122">Select **Devices** and then select and click the device you just updated.</span></span> <span data-ttu-id="d1c69-123">Gå till **Inställningar > Hantera > Enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-123">Go to **Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="d1c69-124">Programvaruversionen visas ska vara **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-124">The displayed software version should be **10.0.10290.0**.</span></span>

## <a name="use-the-local-web-ui"></a><span data-ttu-id="d1c69-125">Använda lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d1c69-125">Use the local web UI</span></span>

<span data-ttu-id="d1c69-126">Det finns två steg när du använder lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="d1c69-126">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="d1c69-127">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-127">Download the update or the hotfix</span></span>
* <span data-ttu-id="d1c69-128">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-128">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="d1c69-129">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-129">Download the update or the hotfix</span></span>

<span data-ttu-id="d1c69-130">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="d1c69-130">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="d1c69-131">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-131">To download the update or the hotfix</span></span>

1. <span data-ttu-id="d1c69-132">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d1c69-132">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="d1c69-133">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="d1c69-133">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="d1c69-134">Ange antalet den snabbkorrigering som du vill hämta Knowledge Base (KB) i sökrutan i Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-134">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="d1c69-135">Ange **4021576** för Update 0,5 och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-135">Enter **4021576** for Update 0.5, and then click **Search**.</span></span>
   
    <span data-ttu-id="d1c69-136">Snabbkorrigeringen listan visas till exempel **StorSimple virtuell matris uppdatering 0,5**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-136">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.5**.</span></span>
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-05/download1.png)

4. <span data-ttu-id="d1c69-138">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-138">Click **Download**.</span></span> 

5. <span data-ttu-id="d1c69-139">Du bör se två filer ska hämtas en *.msu* och en *CAB* fil.</span><span class="sxs-lookup"><span data-stu-id="d1c69-139">You should see two files to download, a *.msu* and a *.cab* file.</span></span> <span data-ttu-id="d1c69-140">Hämta var och en av dessa filer till en mapp.</span><span class="sxs-lookup"><span data-stu-id="d1c69-140">Download each of those files to a folder.</span></span> <span data-ttu-id="d1c69-141">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-141">The folder can also be copied to a network share that is reachable from the device.</span></span>

6. <span data-ttu-id="d1c69-142">Öppna mappen där filerna finns.</span><span class="sxs-lookup"><span data-stu-id="d1c69-142">Open the folder where the files are located.</span></span>
    <span data-ttu-id="d1c69-143">![Filer i paketet](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span><span class="sxs-lookup"><span data-stu-id="d1c69-143">![Files in the package](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span></span>

    <span data-ttu-id="d1c69-144">Du ser:</span><span class="sxs-lookup"><span data-stu-id="d1c69-144">You see:</span></span>
    -  <span data-ttu-id="d1c69-145">En paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="d1c69-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="d1c69-146">Den här filen används för att uppdatera enheten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-146">This file is used to update the device software.</span></span>
    - <span data-ttu-id="d1c69-147">En paketfil Geneva Monitoring Agent `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="d1c69-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="d1c69-148">Den här filen används för att uppdatera agenten övervaknings- och diagnostikfunktionerna service (MDS).</span><span class="sxs-lookup"><span data-stu-id="d1c69-148">This file is used to update the Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="d1c69-149">Dubbelklicka på cab-filen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-149">Double-click the cab file.</span></span> <span data-ttu-id="d1c69-150">En .msi visas.</span><span class="sxs-lookup"><span data-stu-id="d1c69-150">A .msi is displayed.</span></span> <span data-ttu-id="d1c69-151">Markera filen, högerklicka och sedan **extrahera** filen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-151">Select the file, right-click, and then **Extract** the file.</span></span> <span data-ttu-id="d1c69-152">Du kommer att använda den _.msi_ fil att uppdatera agenten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-152">You will use the _.msi_ file to update the agent.</span></span>

        ![Extrahera uppdateringsfil MDS-agenten](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="d1c69-154">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-154">Install the update or the hotfix</span></span>

<span data-ttu-id="d1c69-155">Kontrollera att du har uppdateringen eller snabbkorrigeringen hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs före installationen uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="d1c69-155">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="d1c69-156">Använd den här metoden för att installera uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="d1c69-156">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="d1c69-157">Den här proceduren tar mindre än 2 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d1c69-157">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="d1c69-158">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="d1c69-158">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="d1c69-159">Så här installerar du uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="d1c69-159">To install the update or the hotfix</span></span>

1. <span data-ttu-id="d1c69-160">Gå till i det lokala webbgränssnittet **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-160">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="d1c69-162">I **uppdatering filsökväg**, ange filnamnet för uppdateringen eller snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-162">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="d1c69-163">Du kan även bläddra till installationsfilen uppdatering eller snabbkorrigering om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="d1c69-163">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="d1c69-164">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-164">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="d1c69-166">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="d1c69-166">A warning is displayed.</span></span> <span data-ttu-id="d1c69-167">Angivna detta är en enskild nod-enhet när uppdateringen har genomförts, enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="d1c69-167">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="d1c69-168">Klicka på kryssikonen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-168">Click the check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="d1c69-170">Uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="d1c69-170">The update starts.</span></span> <span data-ttu-id="d1c69-171">När enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="d1c69-171">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="d1c69-172">Lokala Användargränssnittet är inte tillgängligt i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="d1c69-172">The local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="d1c69-174">När omstarten är klar, kommer du till den **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="d1c69-174">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="d1c69-175">Kontrollera att enhetens programvara har uppdaterats i lokala webbgränssnittet, gå till **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="d1c69-175">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="d1c69-176">Programvaruversionen visas ska vara **10.0.0.0.0.10290.0** för uppdatering 0,5.</span><span class="sxs-lookup"><span data-stu-id="d1c69-176">The displayed software version should be **10.0.0.0.0.10290.0** for Update 0.5.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d1c69-177">Vi rapporterar programvaruversioner i ett lite annorlunda sätt i lokala webbgränssnittet och Azure portal.</span><span class="sxs-lookup"><span data-stu-id="d1c69-177">We report the software versions in a slightly different way in the local web UI and the Azure portal.</span></span> <span data-ttu-id="d1c69-178">Till exempel lokala webbgränssnittet rapporterar **10.0.0.0.0.10290** och Azure portal rapporter **10.0.10290.0** för samma version.</span><span class="sxs-lookup"><span data-stu-id="d1c69-178">For example, the local web UI reports **10.0.0.0.0.10290** and the Azure portal reports **10.0.10290.0** for the same version.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. <span data-ttu-id="d1c69-180">Nästa steg är att uppdatera MDS-agenten.</span><span class="sxs-lookup"><span data-stu-id="d1c69-180">The next step is to update the MDS agent.</span></span> <span data-ttu-id="d1c69-181">I den **programuppdateringen** går du till den **uppdatering filsökväg** och bläddra till den `GenevaMonitoringAgentPackageInstaller.msi` filen.</span><span class="sxs-lookup"><span data-stu-id="d1c69-181">In the **Software Update** page, go to the **Update file path** and browse to the `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="d1c69-182">Upprepa steg 2-4.</span><span class="sxs-lookup"><span data-stu-id="d1c69-182">Repeat steps 2-4.</span></span> <span data-ttu-id="d1c69-183">När den virtuella matrisen har startats om logga in på det lokala webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d1c69-183">After the virtual array restarts, sign into the local web UI.</span></span>

<span data-ttu-id="d1c69-184">Uppdateringen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="d1c69-184">The update is now complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1c69-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1c69-185">Next steps</span></span>

<span data-ttu-id="d1c69-186">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="d1c69-186">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

