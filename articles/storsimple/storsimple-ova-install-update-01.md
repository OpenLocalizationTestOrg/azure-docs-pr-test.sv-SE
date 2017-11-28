---
title: "Installera uppdateringar på en virtuell StorSimple-matris | Microsoft Docs"
description: "Beskriver hur du använder virtuella StorSimple-matris webbgränssnittet för att tillämpa uppdateringar med hjälp av metoden portal och snabbkorrigering"
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
ms.openlocfilehash: bccb0d49c1959a690d513961c32d946763385a87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a><span data-ttu-id="2da12-103">Installera uppdateringar på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="2da12-103">Install Updates on your StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="2da12-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2da12-104">Overview</span></span>
<span data-ttu-id="2da12-105">Den här artikeln beskriver de steg som krävs för att installera uppdateringar på din StorSimple virtuell matrisen via lokala webbgränssnittet och via den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2da12-105">This article describes the steps required to install updates on your StorSimple Virtual Array via the local web UI and via the Azure classic portal.</span></span> <span data-ttu-id="2da12-106">Du måste tillämpa uppdateringar eller snabbkorrigeringar för att hålla det uppdaterat din virtuella StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="2da12-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="2da12-107">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="2da12-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="2da12-108">Med hänsyn till att den virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="2da12-108">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="2da12-109">Innan du installerar en uppdatering, rekommenderar vi att du vidtar volymer eller resurser offline på värden först och sedan enheten.</span><span class="sxs-lookup"><span data-stu-id="2da12-109">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="2da12-110">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="2da12-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2da12-111">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda metoden snabbkorrigeringen via lokala webbgränssnittet för att installera uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="2da12-111">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install update 0.3.</span></span> <span data-ttu-id="2da12-112">Om du kör uppdatering 0,2 rekommenderar vi att du installerar uppdateringar via den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2da12-112">If you are running Update 0.2, we recommend that you install the updates via the Azure classic portal.</span></span>
> 
> 

## <a name="use-the-local-web-ui"></a><span data-ttu-id="2da12-113">Använda lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="2da12-113">Use the local web UI</span></span>
<span data-ttu-id="2da12-114">Det finns två steg när du använder lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="2da12-114">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="2da12-115">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-115">Download the update or the hotfix</span></span>
* <span data-ttu-id="2da12-116">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-116">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="2da12-117">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-117">Download the update or the hotfix</span></span>
<span data-ttu-id="2da12-118">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="2da12-118">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="2da12-119">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-119">To download the update or the hotfix</span></span>
1. <span data-ttu-id="2da12-120">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="2da12-120">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="2da12-121">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="2da12-121">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
3. <span data-ttu-id="2da12-122">Ange antalet den snabbkorrigering som du vill hämta Knowledge Base (KB) i sökrutan i Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="2da12-122">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="2da12-123">Ange **3182061** uppdatering 0.3 för och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="2da12-123">Enter **3182061** for Update 0.3, and then click **Search**.</span></span>
   
    <span data-ttu-id="2da12-124">Snabbkorrigeringen listan visas till exempel **StorSimple virtuell matris uppdatering 0.3**.</span><span class="sxs-lookup"><span data-stu-id="2da12-124">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.3**.</span></span>
   
    ![Sökkatalog](./media/storsimple-ova-install-update-01/download1.png)
4. <span data-ttu-id="2da12-126">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2da12-126">Click **Add**.</span></span> <span data-ttu-id="2da12-127">Uppdateringen läggs till i korgen.</span><span class="sxs-lookup"><span data-stu-id="2da12-127">The update is added to the basket.</span></span>
5. <span data-ttu-id="2da12-128">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="2da12-128">Click **View Basket**.</span></span>
6. <span data-ttu-id="2da12-129">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="2da12-129">Click **Download**.</span></span> <span data-ttu-id="2da12-130">Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas.</span><span class="sxs-lookup"><span data-stu-id="2da12-130">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="2da12-131">Uppdateringarna hämtas till den angivna platsen och placeras i en undermapp med samma namn som uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="2da12-131">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="2da12-132">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="2da12-132">The folder can also be copied to a network share that is reachable from the device.</span></span>
7. <span data-ttu-id="2da12-133">Öppna mappen kopierade, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="2da12-133">Open the copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="2da12-134">Den här filen används för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2da12-134">This file is used to install the update or hotfix.</span></span>

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="2da12-135">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-135">Install the update or the hotfix</span></span>
<span data-ttu-id="2da12-136">Kontrollera att du har uppdateringen eller snabbkorrigeringen hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs före installationen uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2da12-136">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="2da12-137">Använd den här metoden för att installera uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="2da12-137">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="2da12-138">Den här proceduren tar mindre än 2 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="2da12-138">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="2da12-139">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2da12-139">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="2da12-140">Så här installerar du uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="2da12-140">To install the update or the hotfix</span></span>
1. <span data-ttu-id="2da12-141">Gå till i det lokala webbgränssnittet **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="2da12-141">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update1m.png)
2. <span data-ttu-id="2da12-143">I **uppdatering filsökväg**, ange filnamnet för uppdateringen eller snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="2da12-143">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="2da12-144">Du kan även bläddra till installationsfilen uppdatering eller snabbkorrigering om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="2da12-144">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="2da12-145">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="2da12-145">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update2m.png)
3. <span data-ttu-id="2da12-147">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="2da12-147">A warning is displayed.</span></span> <span data-ttu-id="2da12-148">Angivna detta är en enskild nod-enhet när uppdateringen har genomförts, enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="2da12-148">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="2da12-149">Klicka på kryssikonen.</span><span class="sxs-lookup"><span data-stu-id="2da12-149">Click the check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-ova-install-update-01/update3m.png)
4. <span data-ttu-id="2da12-151">Uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="2da12-151">The update starts.</span></span> <span data-ttu-id="2da12-152">När enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="2da12-152">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="2da12-153">Lokala Användargränssnittet är inte tillgängligt i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="2da12-153">The local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update5m.png)
5. <span data-ttu-id="2da12-155">När omstarten är klar, kommer du till den **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="2da12-155">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="2da12-156">Kontrollera att enhetens programvara har uppdaterats i lokala webbgränssnittet, gå till **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="2da12-156">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="2da12-157">Programvaruversionen visas ska vara **10.0.0.0.0.10288.0** för uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="2da12-157">The displayed software version should be **10.0.0.0.0.10288.0** for Update 0.3.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2da12-158">Vi rapporterar programvaruversioner i ett lite annorlunda sätt i lokala webbgränssnittet och den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2da12-158">We report the software versions in a slightly different way in the local web UI and the Azure classic portal.</span></span> <span data-ttu-id="2da12-159">Till exempel lokala webbgränssnittet rapporterar **10.0.0.0.0.10288** och Azure klassiska portal rapporter **10.0.10288.0** för samma version.</span><span class="sxs-lookup"><span data-stu-id="2da12-159">For example, the local web UI reports **10.0.0.0.0.10288** and the Azure classic portal reports **10.0.10288.0** for the same version.</span></span> 
   > 
   > 
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-the-azure-classic-portal"></a><span data-ttu-id="2da12-161">Använd den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2da12-161">Use the Azure classic portal</span></span>
<span data-ttu-id="2da12-162">Om du kör uppdatering 0,2, rekommenderar vi att du installerar uppdateringar via den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2da12-162">If running Update 0.2, we recommend that you install updates through the Azure classic portal.</span></span> <span data-ttu-id="2da12-163">Portalen proceduren kräver att användaren skanna, hämta och installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="2da12-163">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="2da12-164">Den här proceduren tar cirka 7 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="2da12-164">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="2da12-165">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="2da12-165">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

<span data-ttu-id="2da12-166">När installationen är klar (som visas jobbets status till 100%), går du till **enheter > Underhåll > programuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="2da12-166">After the installation is complete (as indicated by job status at 100 %), go to **Devices > Maintenance > Software Updates**.</span></span> <span data-ttu-id="2da12-167">Den visa programvaruversionen ska 10.0.10288.0.</span><span class="sxs-lookup"><span data-stu-id="2da12-167">The displayed software version should be 10.0.10288.0.</span></span>

![uppdatera enhet](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a><span data-ttu-id="2da12-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2da12-169">Next steps</span></span>
<span data-ttu-id="2da12-170">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="2da12-170">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

