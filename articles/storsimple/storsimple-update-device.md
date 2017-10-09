---
title: aaaUpdate din StorSimple-enhet | Microsoft Docs
description: "Förklarar hur toouse hello StorSimple uppdatera funktionen tooinstall regelbundet och underhåll läge uppdateringar och snabbkorrigeringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a><span data-ttu-id="96fce-103">Uppdatera enheten StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="96fce-103">Update your StorSimple 8000 Series device</span></span>
## <a name="overview"></a><span data-ttu-id="96fce-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="96fce-104">Overview</span></span>
<span data-ttu-id="96fce-105">Hej StorSimple uppdateringar funktionerna kan du tooeasily hålla din StorSimple-enhet som är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="96fce-105">hello StorSimple updates features allow you tooeasily keep your StorSimple device up-to-date.</span></span> <span data-ttu-id="96fce-106">Du kan tillämpa uppdateringar toohello enheten via hello klassiska Azure-portalen eller via Windows PowerShell-gränssnittet för hello beroende på hello uppdateringstyp.</span><span class="sxs-lookup"><span data-stu-id="96fce-106">Depending on hello update type, you can apply updates toohello device via hello Azure classic portal or via hello Windows PowerShell interface.</span></span> <span data-ttu-id="96fce-107">Den här självstudiekursen beskriver hello uppdateringstyperna och hur tooinstall av dem.</span><span class="sxs-lookup"><span data-stu-id="96fce-107">This tutorial describes hello update types and how tooinstall each of them.</span></span>

<span data-ttu-id="96fce-108">Du kan använda två typer av uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="96fce-108">You can apply two types of device updates:</span></span> 

* <span data-ttu-id="96fce-109">Vanliga (eller normalläge) uppdateringar</span><span class="sxs-lookup"><span data-stu-id="96fce-109">Regular (or Normal mode) updates</span></span>
* <span data-ttu-id="96fce-110">Underhåll läge uppdateringar</span><span class="sxs-lookup"><span data-stu-id="96fce-110">Maintenance mode updates</span></span>

<span data-ttu-id="96fce-111">Du kan installera regelbundna uppdateringar via hello klassiska Azure-portalen eller Windows PowerShell; dock måste du använda Windows PowerShell tooinstall Underhåll läge uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-111">You can install regular updates via hello Azure classic portal or Windows PowerShell; however, you must use Windows PowerShell tooinstall Maintenance mode updates.</span></span> 

<span data-ttu-id="96fce-112">Varje uppdatering är separat, nedan.</span><span class="sxs-lookup"><span data-stu-id="96fce-112">Each update type is described separately, below.</span></span>

### <a name="regular-updates"></a><span data-ttu-id="96fce-113">Regelbundna uppdateringar</span><span class="sxs-lookup"><span data-stu-id="96fce-113">Regular updates</span></span>
<span data-ttu-id="96fce-114">Regelbundna uppdateringar är icke-störande uppdateringar som kan installeras när hello enheten är i normalläge.</span><span class="sxs-lookup"><span data-stu-id="96fce-114">Regular updates are non-disruptive updates that can be installed when hello device is in Normal mode.</span></span> <span data-ttu-id="96fce-115">Uppdateringarna tillämpas via hello Microsoft Update-webbplatsen tooeach enhetskontroll.</span><span class="sxs-lookup"><span data-stu-id="96fce-115">These updates are applied through hello Microsoft Update website tooeach device controller.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="96fce-116">En domänkontrollant och växling vid fel kan uppstå under hello uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="96fce-116">A controller failover may occur during hello update process.</span></span> <span data-ttu-id="96fce-117">Men påverkar detta inte systemets tillgänglighet eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="96fce-117">However, this will not affect system availability or operation.</span></span>
> 
> 

* <span data-ttu-id="96fce-118">Mer information om hur tooinstall regelbundet uppdaterar via hello klassiska Azure-portalen finns [installera regelbundna uppdateringar via hello klassiska Azure-portalen](#install-regular-updates-via-the-azure-classic-portal).</span><span class="sxs-lookup"><span data-stu-id="96fce-118">For details on how tooinstall regular updates via hello Azure classic portal, see [Install regular updates via hello Azure classic portal](#install-regular-updates-via-the-azure-classic-portal).</span></span>
* <span data-ttu-id="96fce-119">Du kan också installera regelbundna uppdateringar via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96fce-119">You can also install regular updates via Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="96fce-120">Mer information finns i [installerar regelbundna uppdateringar via Windows PowerShell för StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="96fce-120">For details, see [Install regular updates via Windows PowerShell for StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span></span>

### <a name="maintenance-mode-updates"></a><span data-ttu-id="96fce-121">Underhåll läge uppdateringar</span><span class="sxs-lookup"><span data-stu-id="96fce-121">Maintenance mode updates</span></span>
<span data-ttu-id="96fce-122">Underhåll läge uppdateringar är störande uppdateringar, till exempel disk firmware-uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="96fce-122">Maintenance Mode updates are disruptive updates such as disk firmware upgrades.</span></span> <span data-ttu-id="96fce-123">De här uppdateringarna kräver hello enheten toobe försätta i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="96fce-123">These updates require hello device toobe put into Maintenance mode.</span></span> <span data-ttu-id="96fce-124">Mer information finns i [steg 2: Ange underhållsläge](#step2).</span><span class="sxs-lookup"><span data-stu-id="96fce-124">For details, see [Step 2: Enter Maintenance mode](#step2).</span></span> <span data-ttu-id="96fce-125">Du kan inte använda hello Azure klassiska portal tooinstall Underhåll läge uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-125">You cannot use hello Azure classic portal tooinstall Maintenance mode updates.</span></span> <span data-ttu-id="96fce-126">I stället måste du använda Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96fce-126">Instead, you must use Windows PowerShell for StorSimple.</span></span> 

<span data-ttu-id="96fce-127">Mer information om hur tooinstall underhållsläge uppdaterar finns [installera Underhåll läge uppdateringar via Windows PowerShell för StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="96fce-127">For details on how tooinstall Maintenance mode updates, see [Install Maintenance mode updates via Windows PowerShell for StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96fce-128">Underhållsläge uppdateringar måste vara tillämpas separat tooeach domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="96fce-128">Maintenance mode updates must be applied separately tooeach controller.</span></span> 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a><span data-ttu-id="96fce-129">Installera regelbundna uppdateringar via hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="96fce-129">Install regular updates via hello Azure classic portal</span></span>
<span data-ttu-id="96fce-130">Du kan använda hello Azure klassiska portal tooapply uppdateringar tooyour StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="96fce-130">You can use hello Azure classic portal tooapply updates tooyour StorSimple device.</span></span>

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="96fce-131">Installera regelbundna uppdateringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="96fce-131">Install regular updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="96fce-132">Alternativt kan du använda Windows PowerShell för StorSimple tooapply regular (normalt läge) uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-132">Alternatively, you can use Windows PowerShell for StorSimple tooapply regular (Normal mode) updates.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96fce-133">Du kan installera regelbundna uppdateringar med hjälp av Windows PowerShell för StorSimple, rekommenderar vi att du installerar regelbundna uppdateringar via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96fce-133">Although you can install regular updates using Windows PowerShell for StorSimple, we strongly recommend that you install regular updates through hello Azure classic portal.</span></span> <span data-ttu-id="96fce-134">Från och med uppdatering 1, kommer inledande kontroller att utföras tidigare tooinstalling uppdateringar från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="96fce-134">Beginning with Update 1, pre-checks will be performed prior tooinstalling updates from hello portal.</span></span> <span data-ttu-id="96fce-135">Kontrollerna före åsidosätter fel och ge en jämnare upplevelse.</span><span class="sxs-lookup"><span data-stu-id="96fce-135">These pre-checks will preempt failures and ensure a smoother experience.</span></span> 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="96fce-136">Installera Underhåll läge uppdateringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="96fce-136">Install Maintenance mode updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="96fce-137">Du kan använda Windows PowerShell för StorSimple tooapply Underhåll läge uppdateringar tooyour StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="96fce-137">You use Windows PowerShell for StorSimple tooapply Maintenance mode updates tooyour StorSimple device.</span></span> <span data-ttu-id="96fce-138">Alla i/o-begäranden är pausade i det här läget.</span><span class="sxs-lookup"><span data-stu-id="96fce-138">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="96fce-139">Tjänster, till exempel beständiga RAM-minne (NVRAM) eller hello klustertjänsten också har stoppats.</span><span class="sxs-lookup"><span data-stu-id="96fce-139">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="96fce-140">Både domänkontrollanter startas om när du anger eller avsluta det här läget.</span><span class="sxs-lookup"><span data-stu-id="96fce-140">Both controllers are rebooted when you enter or exit this mode.</span></span> <span data-ttu-id="96fce-141">När du lämnar det här läget återupptas alla hello-tjänster och bör vara felfritt.</span><span class="sxs-lookup"><span data-stu-id="96fce-141">When you exit this mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="96fce-142">(Detta kan ta några minuter.)</span><span class="sxs-lookup"><span data-stu-id="96fce-142">(This may take a few minutes.)</span></span>

<span data-ttu-id="96fce-143">Om du behöver tooapply Underhåll läge uppdateringar visas en avisering via hello klassiska Azure-portalen som du har uppdateringar som måste installeras.</span><span class="sxs-lookup"><span data-stu-id="96fce-143">If you need tooapply Maintenance mode updates, you will receive an alert through hello Azure classic portal that you have updates that must be installed.</span></span> <span data-ttu-id="96fce-144">Den här aviseringen innehåller instruktioner för att använda Windows PowerShell för StorSimple tooinstall hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-144">This alert will include instructions for using Windows PowerShell for StorSimple tooinstall hello updates.</span></span> <span data-ttu-id="96fce-145">När du har uppdaterat enheten använda hello samma procedur toochange hello enhet tooRegular-läge.</span><span class="sxs-lookup"><span data-stu-id="96fce-145">After you update your device, use hello same procedure toochange hello device tooRegular mode.</span></span> <span data-ttu-id="96fce-146">Stegvisa instruktioner finns [steg 4: avsluta underhållsläget](#step4).</span><span class="sxs-lookup"><span data-stu-id="96fce-146">For step-by-step instructions, see [Step 4: Exit Maintenance mode](#step4).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="96fce-147">Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att kontrollera hello **maskinvarustatus** på hello **Underhåll** sida i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96fce-147">Before entering Maintenance mode, verify that both device controllers are healthy by checking hello **Hardware Status** on hello **Maintenance** page in hello Azure classic portal.</span></span> <span data-ttu-id="96fce-148">Kontakta Microsofts Support för hello nästa steg om hello domänkontrollanten inte är felfri.</span><span class="sxs-lookup"><span data-stu-id="96fce-148">If hello controller is not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="96fce-149">Mer information finns i tooContact Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="96fce-149">For more information, go tooContact Microsoft Support.</span></span> 
> * <span data-ttu-id="96fce-150">När du är i underhållsläge, måste tooapply hello uppdatering först på en domänkontrollant och sedan på hello andra domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="96fce-150">When you are in Maintenance mode, you need tooapply hello update first on one controller and then on hello other controller.</span></span>
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a><span data-ttu-id="96fce-151">Steg 1: Anslut toohello seriekonsol<a name="step1"></span><span class="sxs-lookup"><span data-stu-id="96fce-151">Step 1: Connect toohello serial console <a name="step1"></span></span>
<span data-ttu-id="96fce-152">Använd först ett program som PuTTY tooaccess hello seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="96fce-152">First, use an application such as PuTTY tooaccess hello serial console.</span></span> <span data-ttu-id="96fce-153">hello nedan förklaras hur toouse PuTTY tooconnect toohello seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="96fce-153">hello following procedure explains how toouse PuTTY tooconnect toohello serial console.</span></span>

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a><span data-ttu-id="96fce-154">Steg 2: Ange underhållsläge<a name="step2"></span><span class="sxs-lookup"><span data-stu-id="96fce-154">Step 2: Enter Maintenance mode <a name="step2"></span></span>
<span data-ttu-id="96fce-155">När du har anslutit toohello konsolen avgöra om det finns uppdateringar tooinstall och ange Underhåll läge tooinstall dem.</span><span class="sxs-lookup"><span data-stu-id="96fce-155">After you connect toohello console, determine whether there are updates tooinstall, and enter Maintenance mode tooinstall them.</span></span>

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a><span data-ttu-id="96fce-156">Steg 3: Installera uppdateringar<a name="step3"></span><span class="sxs-lookup"><span data-stu-id="96fce-156">Step 3: Install your updates <a name="step3"></span></span>
<span data-ttu-id="96fce-157">Installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="96fce-157">Next, install your updates.</span></span>

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a><span data-ttu-id="96fce-158">Steg 4: Avsluta underhållsläge<a name="step4"></span><span class="sxs-lookup"><span data-stu-id="96fce-158">Step 4: Exit Maintenance mode <a name="step4"></span></span>
<span data-ttu-id="96fce-159">Slutligen ska du avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="96fce-159">Finally, exit Maintenance mode.</span></span>

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="96fce-160">Installera snabbkorrigeringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="96fce-160">Install hotfixes via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="96fce-161">Till skillnad från uppdateringar för Microsoft Azure StorSimple installeras snabbkorrigeringarna från en delad mapp.</span><span class="sxs-lookup"><span data-stu-id="96fce-161">Unlike updates for Microsoft Azure StorSimple, hotfixes are installed from a shared folder.</span></span> <span data-ttu-id="96fce-162">Precis som med uppdateringar, finns det två typer av snabbkorrigeringar:</span><span class="sxs-lookup"><span data-stu-id="96fce-162">As with updates, there are two types of hotfixes:</span></span> 

* <span data-ttu-id="96fce-163">Vanliga snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="96fce-163">Regular hotfixes</span></span> 
* <span data-ttu-id="96fce-164">Underhåll läge snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="96fce-164">Maintenance mode hotfixes</span></span>  

<span data-ttu-id="96fce-165">hello följande procedurer beskriver hur toouse Windows PowerShell för StorSimple tooinstall regelbundet och underhåll läge snabbkorrigeringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-165">hello following procedures explain how toouse Windows PowerShell for StorSimple tooinstall regular and Maintenance mode hotfixes.</span></span>

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a><span data-ttu-id="96fce-166">Vad händer tooupdates om du utför en återställning av hello enheten?</span><span class="sxs-lookup"><span data-stu-id="96fce-166">What happens tooupdates if you perform a factory reset of hello device?</span></span>
<span data-ttu-id="96fce-167">Om en enhet är Återställ toofactory inställningar kan förloras alla hello-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="96fce-167">If a device is reset toofactory settings, then all hello updates are lost.</span></span> <span data-ttu-id="96fce-168">När hello fabriksåterställning enheten är registrerad och konfigurerad, måste toomanually installera uppdateringar via hello klassiska Azure-portalen och/eller Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96fce-168">After hello factory-reset device is registered and configured, you will need toomanually install updates through hello Azure classic portal and/or Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="96fce-169">Läs mer om fabriksåterställning [återställer standardinställningarna för hello enheten toofactory](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="96fce-169">For more information about factory reset, see [Reset hello device toofactory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>

## <a name="next-steps"></a><span data-ttu-id="96fce-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96fce-170">Next steps</span></span>
* <span data-ttu-id="96fce-171">Lär dig mer om [använder Windows PowerShell för StorSimple tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="96fce-171">Learn more about [using Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="96fce-172">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="96fce-172">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

