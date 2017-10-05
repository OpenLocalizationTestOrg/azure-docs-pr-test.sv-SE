---
title: "Installera uppdateringar på virtuella StorSimple-matrisen | Microsoft Docs"
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
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 3fb246b1515e7a637e6cff6499bf324c3f80dd45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a><span data-ttu-id="a9d15-103">Installera uppdateringen 0,4 på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="a9d15-103">Install Update 0.4 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="a9d15-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a9d15-104">Overview</span></span>

<span data-ttu-id="a9d15-105">Den här artikeln beskriver de steg som krävs för att installera uppdateringen 0,4 på din virtuella StorSimple-matrisen via lokala webbgränssnittet och via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a9d15-105">This article describes the steps required to install Update 0.4 on your StorSimple Virtual Array via the local web UI and via the Azure portal.</span></span> <span data-ttu-id="a9d15-106">Du måste tillämpa uppdateringar eller snabbkorrigeringar för att hålla det uppdaterat din virtuella StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="a9d15-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="a9d15-107">Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="a9d15-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="a9d15-108">Med hänsyn till att den virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp.</span><span class="sxs-lookup"><span data-stu-id="a9d15-108">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="a9d15-109">Innan du installerar en uppdatering, rekommenderar vi att du vidtar volymer eller resurser offline på värden först och sedan enheten.</span><span class="sxs-lookup"><span data-stu-id="a9d15-109">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="a9d15-110">Detta minimerar möjlighet att data skadas.</span><span class="sxs-lookup"><span data-stu-id="a9d15-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9d15-111">Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda metoden snabbkorrigeringen via lokala webbgränssnittet för att installera uppdatering 0.3.</span><span class="sxs-lookup"><span data-stu-id="a9d15-111">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install update 0.3.</span></span> <span data-ttu-id="a9d15-112">Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du uppdateringar via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a9d15-112">If you are running Update 0.2 or later, we recommend that you install the updates via the Azure portal.</span></span>
 

## <a name="use-the-local-web-ui"></a><span data-ttu-id="a9d15-113">Använda lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="a9d15-113">Use the local web UI</span></span>

<span data-ttu-id="a9d15-114">Det finns två steg när du använder lokala webbgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="a9d15-114">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="a9d15-115">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-115">Download the update or the hotfix</span></span>
* <span data-ttu-id="a9d15-116">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-116">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="a9d15-117">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-117">Download the update or the hotfix</span></span>

<span data-ttu-id="a9d15-118">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="a9d15-118">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="a9d15-119">Hämta uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-119">To download the update or the hotfix</span></span>

1. <span data-ttu-id="a9d15-120">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a9d15-120">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="a9d15-121">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="a9d15-121">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="a9d15-122">Ange antalet den snabbkorrigering som du vill hämta Knowledge Base (KB) i sökrutan i Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-122">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="a9d15-123">Ange **3216577** för Update 0,4 och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-123">Enter **3216577** for Update 0.4, and then click **Search**.</span></span>
   
    <span data-ttu-id="a9d15-124">Snabbkorrigeringen listan visas till exempel **StorSimple virtuell matris uppdatering 0,4**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-124">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.4**.</span></span>
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-04/download1.png)

4. <span data-ttu-id="a9d15-126">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-126">Click **Add**.</span></span> <span data-ttu-id="a9d15-127">Uppdateringen läggs till i korgen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-127">The update is added to the basket.</span></span>

5. <span data-ttu-id="a9d15-128">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="a9d15-128">Click **View Basket**.</span></span>

6. <span data-ttu-id="a9d15-129">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-129">Click **Download**.</span></span> <span data-ttu-id="a9d15-130">Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas.</span><span class="sxs-lookup"><span data-stu-id="a9d15-130">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="a9d15-131">Uppdateringarna hämtas till den angivna platsen och placeras i en undermapp med samma namn som uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-131">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="a9d15-132">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="a9d15-132">The folder can also be copied to a network share that is reachable from the device.</span></span>

7. <span data-ttu-id="a9d15-133">Öppna mappen kopierade, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="a9d15-133">Open the copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="a9d15-134">Den här filen används för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="a9d15-134">This file is used to install the update or hotfix.</span></span>

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="a9d15-135">Installera uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-135">Install the update or the hotfix</span></span>

<span data-ttu-id="a9d15-136">Kontrollera att du har uppdateringen eller snabbkorrigeringen hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs före installationen uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="a9d15-136">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="a9d15-137">Använd den här metoden för att installera uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner.</span><span class="sxs-lookup"><span data-stu-id="a9d15-137">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="a9d15-138">Den här proceduren tar mindre än 2 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="a9d15-138">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="a9d15-139">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="a9d15-139">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="a9d15-140">Så här installerar du uppdateringen eller snabbkorrigeringen</span><span class="sxs-lookup"><span data-stu-id="a9d15-140">To install the update or the hotfix</span></span>

1. <span data-ttu-id="a9d15-141">Gå till i det lokala webbgränssnittet **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-141">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update1m.png)

2. <span data-ttu-id="a9d15-143">I **uppdatering filsökväg**, ange filnamnet för uppdateringen eller snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-143">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="a9d15-144">Du kan även bläddra till installationsfilen uppdatering eller snabbkorrigering om placeras på en nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="a9d15-144">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="a9d15-145">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-145">Click **Apply**.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update2m.png)

3. <span data-ttu-id="a9d15-147">En varning visas.</span><span class="sxs-lookup"><span data-stu-id="a9d15-147">A warning is displayed.</span></span> <span data-ttu-id="a9d15-148">Angivna detta är en enskild nod-enhet när uppdateringen har genomförts, enheten startas om och det finns en avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="a9d15-148">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="a9d15-149">Klicka på kryssikonen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-149">Click the check icon.</span></span>
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update3m.png)

4. <span data-ttu-id="a9d15-151">Uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="a9d15-151">The update starts.</span></span> <span data-ttu-id="a9d15-152">När enheten har uppdaterats, startas om.</span><span class="sxs-lookup"><span data-stu-id="a9d15-152">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="a9d15-153">Lokala Användargränssnittet är inte tillgängligt i den här tiden.</span><span class="sxs-lookup"><span data-stu-id="a9d15-153">The local UI is not accessible in this duration.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update5m.png)

5. <span data-ttu-id="a9d15-155">När omstarten är klar, kommer du till den **inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="a9d15-155">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="a9d15-156">Kontrollera att enhetens programvara har uppdaterats i lokala webbgränssnittet, gå till **Underhåll** > **programuppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-156">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="a9d15-157">Programvaruversionen visas ska vara **10.0.0.0.0.10289.0** för uppdatering 0,4.</span><span class="sxs-lookup"><span data-stu-id="a9d15-157">The displayed software version should be **10.0.0.0.0.10289.0** for Update 0.4.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a9d15-158">Vi rapporterar programvaruversioner i ett lite annorlunda sätt i lokala webbgränssnittet och Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a9d15-158">We report the software versions in a slightly different way in the local web UI and the Azure portal.</span></span> <span data-ttu-id="a9d15-159">Till exempel lokala webbgränssnittet rapporterar **10.0.0.0.0.10289** och Azure portal rapporter **10.0.10289.0** för samma version.</span><span class="sxs-lookup"><span data-stu-id="a9d15-159">For example, the local web UI reports **10.0.0.0.0.10289** and the Azure portal reports **10.0.10289.0** for the same version.</span></span>
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a><span data-ttu-id="a9d15-161">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a9d15-161">Use the Azure portal</span></span>

<span data-ttu-id="a9d15-162">Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a9d15-162">If running Update 0.2 and later, we recommend that you install updates through the Azure portal.</span></span> <span data-ttu-id="a9d15-163">Portalen proceduren kräver att användaren skanna, hämta och installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="a9d15-163">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="a9d15-164">Den här proceduren tar cirka 7 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="a9d15-164">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="a9d15-165">Utför följande steg för att installera uppdatering eller snabbkorrigering.</span><span class="sxs-lookup"><span data-stu-id="a9d15-165">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="a9d15-166">När installationen är klar (som visas jobbets status till 100%) Gå till Enhetshanteraren för StorSimple-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a9d15-166">After the installation is complete (as indicated by job status at 100 %), go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="a9d15-167">Välj **enheter** och markera och klicka på den enhet som du vill uppdatera listan över enheter som är anslutna till den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a9d15-167">Select **Devices** and then select and click the device you want to update from the list of devices connected to this service.</span></span> <span data-ttu-id="a9d15-168">I den **inställningar** gå till bladet **hantera** avsnittet och väljer **enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-168">In the **Settings** blade, go to **Manage** section and select **Device updates**.</span></span> <span data-ttu-id="a9d15-169">Programvaruversionen visas ska vara **10.0.10289.0**.</span><span class="sxs-lookup"><span data-stu-id="a9d15-169">The displayed software version should be **10.0.10289.0**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a9d15-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9d15-170">Next steps</span></span>

<span data-ttu-id="a9d15-171">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="a9d15-171">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

