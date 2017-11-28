---
title: "läge för aaaChange StorSimple-enhet | Microsoft Docs"
description: "Beskriver hello StorSimple enheten lägen och förklarar hur toouse Windows PowerShell för StorSimple toochange hello enhet-läge."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 058ca6cc38954bce3679cc21b39d341b10cb4dfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a><span data-ttu-id="e40a3-103">Ändra hello Enhetsläge på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="e40a3-103">Change hello device mode on your StorSimple device</span></span>

<span data-ttu-id="e40a3-104">Den här artikeln innehåller en kort beskrivning av hello respektive läge där din StorSimple-enhet fungerar.</span><span class="sxs-lookup"><span data-stu-id="e40a3-104">This article provides a brief description of hello various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="e40a3-105">Din StorSimple-enhet kan fungera i tre lägen: normal, underhåll och återställning.</span><span class="sxs-lookup"><span data-stu-id="e40a3-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span>

<span data-ttu-id="e40a3-106">När du har läst den här artikeln, vet du:</span><span class="sxs-lookup"><span data-stu-id="e40a3-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="e40a3-107">Hej StorSimple enheten lägen är</span><span class="sxs-lookup"><span data-stu-id="e40a3-107">What hello StorSimple device modes are</span></span>
* <span data-ttu-id="e40a3-108">Hur toofigure i vilket läge hello StorSimple-enheten är i</span><span class="sxs-lookup"><span data-stu-id="e40a3-108">How toofigure out which mode hello StorSimple device is in</span></span>
* <span data-ttu-id="e40a3-109">Hur toochange från normala toomaintenance läge och *tvärtom*</span><span class="sxs-lookup"><span data-stu-id="e40a3-109">How toochange from normal toomaintenance mode and *vice versa*</span></span>

<span data-ttu-id="e40a3-110">hello ovan hanteringsuppgifter kan endast utföras via Windows PowerShell-gränssnittet för hello av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="e40a3-110">hello above management tasks can only be performed via hello Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="e40a3-111">Om lägen för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="e40a3-111">About StorSimple device modes</span></span>

<span data-ttu-id="e40a3-112">Din StorSimple-enhet kan köras i läget normal, underhåll eller återställning.</span><span class="sxs-lookup"><span data-stu-id="e40a3-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="e40a3-113">Var och en av dessa lägen kortfattat beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="e40a3-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="e40a3-114">Normalt läge</span><span class="sxs-lookup"><span data-stu-id="e40a3-114">Normal mode</span></span>

<span data-ttu-id="e40a3-115">Detta definieras som hello operativa normalläge för en helt konfigurerade StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="e40a3-115">This is defined as hello normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="e40a3-116">Som standard måste enheten vara i normalläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="e40a3-117">Underhållsläge</span><span class="sxs-lookup"><span data-stu-id="e40a3-117">Maintenance mode</span></span>

<span data-ttu-id="e40a3-118">Ibland behöver hello StorSimple-enhet toobe placeras i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-118">Sometimes hello StorSimple device may need toobe placed into maintenance mode.</span></span> <span data-ttu-id="e40a3-119">Det här läget kan du tooperform Underhåll på hello enhet och installera störande uppdateringar som de relaterade toodisk inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="e40a3-119">This mode allows you tooperform maintenance on hello device and install disruptive updates, such as those related toodisk firmware.</span></span>

<span data-ttu-id="e40a3-120">Du kan placera hello system i underhållsläge endast via hello Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e40a3-120">You can put hello system into maintenance mode only via hello Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="e40a3-121">Alla i/o-begäranden är pausade i det här läget.</span><span class="sxs-lookup"><span data-stu-id="e40a3-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="e40a3-122">Tjänster, till exempel beständiga RAM-minne (NVRAM) eller hello klustertjänsten också har stoppats.</span><span class="sxs-lookup"><span data-stu-id="e40a3-122">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="e40a3-123">Båda hello domänkontrollanter startas om när du anger eller avsluta det här läget.</span><span class="sxs-lookup"><span data-stu-id="e40a3-123">Both hello controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="e40a3-124">När du avslutar hello underhållsläge återupptas alla hello-tjänster och bör vara felfritt.</span><span class="sxs-lookup"><span data-stu-id="e40a3-124">When you exit hello maintenance mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="e40a3-125">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="e40a3-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="e40a3-126">**Underhållsläge stöds bara på en korrekt fungerande enhet. Den stöds inte på en enhet där en eller båda av hello domänkontrollanter inte fungerar.**</span><span class="sxs-lookup"><span data-stu-id="e40a3-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of hello controllers are not functioning.**</span></span>


### <a name="recovery-mode"></a><span data-ttu-id="e40a3-127">Återställningsläge</span><span class="sxs-lookup"><span data-stu-id="e40a3-127">Recovery mode</span></span>

<span data-ttu-id="e40a3-128">Återställningsläge kan beskrivas som ”Windows felsäkert läge med nätverksstöd”.</span><span class="sxs-lookup"><span data-stu-id="e40a3-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="e40a3-129">Återställningsläge snabbt tillkallar hello Microsoft Support-teamet och låter dem tooperform diagnostik på hello system.</span><span class="sxs-lookup"><span data-stu-id="e40a3-129">Recovery mode engages hello Microsoft Support team and allows them tooperform diagnostics on hello system.</span></span> <span data-ttu-id="e40a3-130">hello primära målet för återställningsläge är tooretrieve hello systemloggar.</span><span class="sxs-lookup"><span data-stu-id="e40a3-130">hello primary goal of recovery mode is tooretrieve hello system logs.</span></span>

<span data-ttu-id="e40a3-131">Om datorn försätts i återställningsläge, bör du kontakta Microsoft Support för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e40a3-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="e40a3-132">Mer information finns för[kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="e40a3-132">For more information, go too[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e40a3-133">**Du kan inte placera hello enheten i återställningsläget. Om hello enheten är i ett dåligt tillstånd, försöker återställningsläge tooget hello enhet i ett tillstånd där Microsoft Support-personal undersöka den.**</span><span class="sxs-lookup"><span data-stu-id="e40a3-133">**You cannot place hello device in recovery mode. If hello device is in a bad state, recovery mode tries tooget hello device into a state in which Microsoft Support personnel can examine it.**</span></span>

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="e40a3-134">Identifiera läge för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="e40a3-134">Determine StorSimple device mode</span></span>

#### <a name="toodetermine-hello-current-device-mode"></a><span data-ttu-id="e40a3-135">toodetermine hello aktuell enhet-läge</span><span class="sxs-lookup"><span data-stu-id="e40a3-135">toodetermine hello current device mode</span></span>

1. <span data-ttu-id="e40a3-136">Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="e40a3-136">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="e40a3-137">Titta på hello Banderollmeddelandet i menyn för seriekonsolen hello av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e40a3-137">Look at hello banner message in hello serial console menu of hello device.</span></span> <span data-ttu-id="e40a3-138">Det här meddelandet anger uttryckligen om hello enheten är i läget underhåll eller återställning.</span><span class="sxs-lookup"><span data-stu-id="e40a3-138">This message explicitly indicates whether hello device is in maintenance or recovery mode.</span></span> <span data-ttu-id="e40a3-139">Om hello-meddelande inte innehåller någon särskild information som rör toohello system läge, är hello-enheten i normalläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-139">If hello message does not contain any specific information pertaining toohello system mode, hello device is in normal mode.</span></span>

## <a name="change-hello-storsimple-device-mode"></a><span data-ttu-id="e40a3-140">Ändra läge för hello StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="e40a3-140">Change hello StorSimple device mode</span></span>

<span data-ttu-id="e40a3-141">Du kan placera hello StorSimple-enhet i underhållsläge läge (från normalläge) tooperform underhåll eller installera uppdateringar för underhåll lägen.</span><span class="sxs-lookup"><span data-stu-id="e40a3-141">You can place hello StorSimple device into maintenance mode (from normal mode) tooperform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="e40a3-142">Utför följande procedurer tooenter eller avsluta underhållsläget hello.</span><span class="sxs-lookup"><span data-stu-id="e40a3-142">Perform hello following procedures tooenter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e40a3-143">Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att öppna hello **Enhetsinställningar > maskinvara hälsa** för din enhet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e40a3-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing hello **Device settings > Hardware health** for your device in hello Azure portal.</span></span> <span data-ttu-id="e40a3-144">Om en eller båda hello domänkontrollanter inte är felfri, kan du kontakta Microsofts Support för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e40a3-144">If one or both hello controllers are not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="e40a3-145">Mer information finns för[kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="e40a3-145">For more information, go too[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>
 

#### <a name="tooenter-maintenance-mode"></a><span data-ttu-id="e40a3-146">tooenter underhållsläge</span><span class="sxs-lookup"><span data-stu-id="e40a3-146">tooenter maintenance mode</span></span>

1. <span data-ttu-id="e40a3-147">Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="e40a3-147">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="e40a3-148">I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="e40a3-148">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="e40a3-149">När du uppmanas att ange hello **enhetens administratörslösenord**.</span><span class="sxs-lookup"><span data-stu-id="e40a3-149">When prompted, provide hello **device administrator password**.</span></span> <span data-ttu-id="e40a3-150">hello standardlösenordet är: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="e40a3-150">hello default password is: `Password1`.</span></span>
3. <span data-ttu-id="e40a3-151">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="e40a3-151">At hello command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="e40a3-152">Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever hello anslutning toohello Azure-portalen och du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="e40a3-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever hello connection toohello Azure portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="e40a3-153">Typen **Y** tooenter underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-153">Type **Y** tooenter maintenance mode.</span></span>
5. <span data-ttu-id="e40a3-154">Både domänkontrollanter startas om.</span><span class="sxs-lookup"><span data-stu-id="e40a3-154">Both controllers will restart.</span></span> <span data-ttu-id="e40a3-155">När hello omstarten är klar, visar hello seriekonsolen banderoll hello enheten är i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-155">When hello restart is complete, hello serial console banner will indicate that hello device is in maintenance mode.</span></span> <span data-ttu-id="e40a3-156">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e40a3-156">A sample output is shown below.</span></span>

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="tooexit-maintenance-mode"></a><span data-ttu-id="e40a3-157">tooexit underhållsläge</span><span class="sxs-lookup"><span data-stu-id="e40a3-157">tooexit maintenance mode</span></span>

1. <span data-ttu-id="e40a3-158">Logga in toohello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="e40a3-158">Log on toohello device serial console.</span></span> <span data-ttu-id="e40a3-159">Kontrollera från hello Banderollmeddelandet som enheten är i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-159">Verify from hello banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="e40a3-160">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="e40a3-160">At hello command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="e40a3-161">Ett varningsmeddelande och ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="e40a3-161">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="e40a3-162">Typen **Y** tooexit underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-162">Type **Y** tooexit maintenance mode.</span></span>
4. <span data-ttu-id="e40a3-163">Både domänkontrollanter startas om.</span><span class="sxs-lookup"><span data-stu-id="e40a3-163">Both controllers will restart.</span></span> <span data-ttu-id="e40a3-164">När hello omstarten är klar, visar hello seriekonsolen banderoll hello enheten är i normalläge.</span><span class="sxs-lookup"><span data-stu-id="e40a3-164">When hello restart is complete, hello serial console banner indicates that hello device is in normal mode.</span></span> <span data-ttu-id="e40a3-165">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e40a3-165">A sample output is shown below.</span></span>

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure tooinstall on each controller could result in data corruption. Exiting maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooexit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a><span data-ttu-id="e40a3-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e40a3-166">Next steps</span></span>

<span data-ttu-id="e40a3-167">Lär dig hur för[tillämpa uppdateringar för normal och underhåll läge](storsimple-update-device.md) på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="e40a3-167">Learn how too[apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

