---
title: "Ändra läge för StorSimple-enhet | Microsoft Docs"
description: "Beskriver lägen för StorSimple-enhet och förklarar hur du använder Windows PowerShell för StorSimple för att ändra läget för enheten."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a><span data-ttu-id="74e8b-103">Ändra läget för enheten på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="74e8b-103">Change the device mode on your StorSimple device</span></span>
<span data-ttu-id="74e8b-104">Den här artikeln innehåller en kort beskrivning av de olika lägena för där din StorSimple-enhet fungerar.</span><span class="sxs-lookup"><span data-stu-id="74e8b-104">This article provides a brief description of the various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="74e8b-105">Din StorSimple-enhet kan fungera i tre lägen: normal, underhåll och återställning.</span><span class="sxs-lookup"><span data-stu-id="74e8b-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span> 

<span data-ttu-id="74e8b-106">När du har läst den här artikeln, vet du:</span><span class="sxs-lookup"><span data-stu-id="74e8b-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="74e8b-107">Vilka StorSimple enheten läget är</span><span class="sxs-lookup"><span data-stu-id="74e8b-107">What the StorSimple device modes are</span></span>
* <span data-ttu-id="74e8b-108">Hur du ta reda på vilket läge StorSimple-enheten är i</span><span class="sxs-lookup"><span data-stu-id="74e8b-108">How to figure out which mode the StorSimple device is in</span></span>
* <span data-ttu-id="74e8b-109">Så här ändrar du från normal till underhållsläge och *tvärtom*</span><span class="sxs-lookup"><span data-stu-id="74e8b-109">How to change from normal to maintenance mode and *vice versa*</span></span>

<span data-ttu-id="74e8b-110">Ovanstående hanteringsuppgifter kan endast utföras via Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="74e8b-110">The above management tasks can only be performed via the Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="74e8b-111">Om lägen för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="74e8b-111">About StorSimple device modes</span></span>
<span data-ttu-id="74e8b-112">Din StorSimple-enhet kan köras i läget normal, underhåll eller återställning.</span><span class="sxs-lookup"><span data-stu-id="74e8b-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="74e8b-113">Var och en av dessa lägen kortfattat beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="74e8b-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="74e8b-114">Normalt läge</span><span class="sxs-lookup"><span data-stu-id="74e8b-114">Normal mode</span></span>
<span data-ttu-id="74e8b-115">Detta definieras som operativa normalläge för en helt konfigurerade StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="74e8b-115">This is defined as the normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="74e8b-116">Som standard måste enheten vara i normalläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="74e8b-117">Underhållsläge</span><span class="sxs-lookup"><span data-stu-id="74e8b-117">Maintenance mode</span></span>
<span data-ttu-id="74e8b-118">StorSimple-enhet kan ibland behöva placeras i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-118">Sometimes the StorSimple device may need to be placed into maintenance mode.</span></span> <span data-ttu-id="74e8b-119">Det här läget kan du utföra underhåll på enheten och installera störande uppdateringar, till exempel de som är relaterade till disk inbyggda programvaran.</span><span class="sxs-lookup"><span data-stu-id="74e8b-119">This mode allows you to perform maintenance on the device and install disruptive updates, such as those related to disk firmware.</span></span>

<span data-ttu-id="74e8b-120">Du kan försätta datorn i underhållsläge endast via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="74e8b-120">You can put the system into maintenance mode only via the Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="74e8b-121">Alla i/o-begäranden är pausade i det här läget.</span><span class="sxs-lookup"><span data-stu-id="74e8b-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="74e8b-122">Tjänster, till exempel beständiga RAM-minne (NVRAM) eller klustertjänsten stoppas.</span><span class="sxs-lookup"><span data-stu-id="74e8b-122">Services such as non-volatile random access memory (NVRAM) or the clustering service are also stopped.</span></span> <span data-ttu-id="74e8b-123">Båda domänkontrollanterna startas om när du anger eller avsluta det här läget.</span><span class="sxs-lookup"><span data-stu-id="74e8b-123">Both the controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="74e8b-124">När du avslutar underhållsläge återupptas alla tjänster och bör vara felfritt.</span><span class="sxs-lookup"><span data-stu-id="74e8b-124">When you exit the maintenance mode, all the services will resume and should be healthy.</span></span> <span data-ttu-id="74e8b-125">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="74e8b-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="74e8b-126">**Underhållsläge stöds bara på en korrekt fungerande enhet. Den stöds inte på en enhet där en eller båda av domänkontrollanter inte fungerar.**
> </span><span class="sxs-lookup"><span data-stu-id="74e8b-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of the controllers are not functioning.**
</span></span></br>
> 
> 

### <a name="recovery-mode"></a><span data-ttu-id="74e8b-127">Återställningsläge</span><span class="sxs-lookup"><span data-stu-id="74e8b-127">Recovery mode</span></span>
<span data-ttu-id="74e8b-128">Återställningsläge kan beskrivas som ”Windows felsäkert läge med nätverksstöd”.</span><span class="sxs-lookup"><span data-stu-id="74e8b-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="74e8b-129">Återställningsläge aktiverar Microsoft Support-teamet och gör att de kan utföra diagnostik i systemet.</span><span class="sxs-lookup"><span data-stu-id="74e8b-129">Recovery mode engages the Microsoft Support team and allows them to perform diagnostics on the system.</span></span> <span data-ttu-id="74e8b-130">Det främsta målet med återställningsläge är att hämta systemloggarna.</span><span class="sxs-lookup"><span data-stu-id="74e8b-130">The primary goal of recovery mode is to retrieve the system logs.</span></span>

<span data-ttu-id="74e8b-131">Om datorn försätts i återställningsläge, bör du kontakta Microsoft Support för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="74e8b-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="74e8b-132">Mer information finns på [kontakta Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="74e8b-132">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="74e8b-133">**Du kan inte placera enheten i återställningsläget. Om enheten är i ett dåligt tillstånd, försöker återställningsläge hämta enheten i ett tillstånd där Microsoft Support-personal undersöka den.**</span><span class="sxs-lookup"><span data-stu-id="74e8b-133">**You cannot place the device in recovery mode. If the device is in a bad state, recovery mode tries to get the device into a state in which Microsoft Support personnel can examine it.**</span></span>
> 
> 

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="74e8b-134">Identifiera läge för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="74e8b-134">Determine StorSimple device mode</span></span>
#### <a name="to-determine-the-current-device-mode"></a><span data-ttu-id="74e8b-135">Att fastställa det aktuella läget för enhet</span><span class="sxs-lookup"><span data-stu-id="74e8b-135">To determine the current device mode</span></span>
1. <span data-ttu-id="74e8b-136">Logga in på enhetens seriekonsol genom att följa stegen i [Använd PuTTY för att ansluta till enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="74e8b-136">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="74e8b-137">Titta på Banderollmeddelandet i menyn för seriekonsolen för enheten.</span><span class="sxs-lookup"><span data-stu-id="74e8b-137">Look at the banner message in the serial console menu of the device.</span></span> <span data-ttu-id="74e8b-138">Det här meddelandet anger uttryckligen om enheten är i läget underhåll eller återställning.</span><span class="sxs-lookup"><span data-stu-id="74e8b-138">This message explicitly indicates whether the device is in maintenance or recovery mode.</span></span> <span data-ttu-id="74e8b-139">Enheten är i normalläge om meddelandet inte innehåller någon särskild information som rör system-läge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-139">If the message does not contain any specific information pertaining to the system mode, the device is in normal mode.</span></span>

## <a name="change-the-storsimple-device-mode"></a><span data-ttu-id="74e8b-140">Ändra läge för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="74e8b-140">Change the StorSimple device mode</span></span>
<span data-ttu-id="74e8b-141">Du kan placera StorSimple-enhet i underhållsläge (från normalt läge) för att utföra underhåll eller installera uppdateringar för underhåll läge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-141">You can place the StorSimple device into maintenance mode (from normal mode) to perform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="74e8b-142">Utför följande procedurer för att ange eller avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="74e8b-142">Perform the following procedures to enter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74e8b-143">Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att öppna den **maskinvarustatus** på den **Underhåll** sida i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="74e8b-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing the **Hardware Status** on the **Maintenance** page in the Azure classic portal.</span></span> <span data-ttu-id="74e8b-144">Om en eller båda domänkontrollanterna inte är felfri, kan du kontakta Microsofts Support för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="74e8b-144">If one or both the controllers are not healthy, contact Microsoft Support for the next steps.</span></span> <span data-ttu-id="74e8b-145">Mer information finns på [kontakta Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="74e8b-145">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
> 
> 

#### <a name="to-enter-maintenance-mode"></a><span data-ttu-id="74e8b-146">Ange underhållsläge</span><span class="sxs-lookup"><span data-stu-id="74e8b-146">To enter maintenance mode</span></span>
1. <span data-ttu-id="74e8b-147">Logga in på enhetens seriekonsol genom att följa stegen i [Använd PuTTY för att ansluta till enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="74e8b-147">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="74e8b-148">I menyn för seriekonsolen väljer du alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="74e8b-148">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="74e8b-149">När du uppmanas, ange den **enhetens administratörslösenord**.</span><span class="sxs-lookup"><span data-stu-id="74e8b-149">When prompted, provide the **device administrator password**.</span></span> <span data-ttu-id="74e8b-150">Standardlösenordet är: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="74e8b-150">The default password is: `Password1`.</span></span>
3. <span data-ttu-id="74e8b-151">I Kommandotolken, skriver du:</span><span class="sxs-lookup"><span data-stu-id="74e8b-151">At the command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="74e8b-152">Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever anslutningen till den klassiska Azure portalen och du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="74e8b-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever the connection to the Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="74e8b-153">Typen **Y** anger underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-153">Type **Y** to enter maintenance mode.</span></span>
5. <span data-ttu-id="74e8b-154">Både domänkontrollanter startas om.</span><span class="sxs-lookup"><span data-stu-id="74e8b-154">Both controllers will restart.</span></span> <span data-ttu-id="74e8b-155">När omstarten är klar, visas ett annat meddelande som anger att enheten är i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-155">When the restart is complete, another message will appear indicating that the device is in maintenance mode.</span></span>

#### <a name="to-exit-maintenance-mode"></a><span data-ttu-id="74e8b-156">Avsluta underhållsläge</span><span class="sxs-lookup"><span data-stu-id="74e8b-156">To exit maintenance mode</span></span>
1. <span data-ttu-id="74e8b-157">Logga in på enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="74e8b-157">Log on to the device serial console.</span></span> <span data-ttu-id="74e8b-158">Kontrollera från Banderollmeddelandet som enheten är i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-158">Verify from the banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="74e8b-159">Skriv följande vid kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="74e8b-159">At the command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="74e8b-160">Ett varningsmeddelande och ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="74e8b-160">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="74e8b-161">Typen **Y** du avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="74e8b-161">Type **Y** to exit maintenance mode.</span></span>
4. <span data-ttu-id="74e8b-162">Både domänkontrollanter startas om.</span><span class="sxs-lookup"><span data-stu-id="74e8b-162">Both controllers will restart.</span></span> <span data-ttu-id="74e8b-163">När omstarten är klar, visas ett annat meddelande som anger att enheten är i normalläge.</span><span class="sxs-lookup"><span data-stu-id="74e8b-163">When the restart is complete, another message will appear indicating that the device is in normal mode.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74e8b-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74e8b-164">Next steps</span></span>
<span data-ttu-id="74e8b-165">Lär dig hur du [tillämpa uppdateringar för normal och underhåll läge](storsimple-update-device.md) på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="74e8b-165">Learn how to [apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

