---
title: "aaaChange StorSimple-lösenord | Microsoft Docs"
description: "Beskriver hur hello toouse StorSimple Enhetshanteraren service toochange administratörslösenord StorSimple Snapshot Manager och enhet."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a><span data-ttu-id="c9b6b-103">Använd hello StorSimple Enhetshanteraren service toochange StorSimple-lösenord</span><span class="sxs-lookup"><span data-stu-id="c9b6b-103">Use hello StorSimple Device Manager service toochange your StorSimple passwords</span></span>

## <a name="overview"></a><span data-ttu-id="c9b6b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c9b6b-104">Overview</span></span>
<span data-ttu-id="c9b6b-105">hello Azure-portalen **Enhetsinställningar** alternativet innehåller alla hello enheten parametrar som du kan konfigurera om på en StorSimple-enhet som hanteras av en tjänst för StorSimple Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-105">hello Azure portal **Device settings** option contains all hello device parameters that you can reconfigure on a StorSimple device that is managed by a StorSimple Device Manager service.</span></span> <span data-ttu-id="c9b6b-106">Den här självstudiekursen beskrivs hur du kan använda hello **säkerhet** alternativ **Enhetsinställningar** toochange enhetsadministratören eller lösenordet för StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-106">This tutorial explains how you can use hello **Security** option under **Device settings** toochange your device administrator or StorSimple Snapshot Manager password.</span></span>

## <a name="change-hello-device-administrator-password"></a><span data-ttu-id="c9b6b-107">Ändra hello enhetens administratörslösenord</span><span class="sxs-lookup"><span data-stu-id="c9b6b-107">Change hello device administrator password</span></span>
<span data-ttu-id="c9b6b-108">När du använder Windows PowerShell-gränssnittet tooaccess hello StorSimple-enhet är obligatoriska tooenter administratörslösenord för enheten.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-108">When you use Windows PowerShell interface tooaccess hello StorSimple device, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="c9b6b-109">När hello första StorSimple-enhet är registrerad med en tjänst hello standardlösenordet för det här gränssnittet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-109">When hello first StorSimple device is registered with a service, hello default password for this interface is *Password1*.</span></span> <span data-ttu-id="c9b6b-110">Hello säkerheten för dina data, du är obligatoriska toochange lösenordet vid hello slutet av hello registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-110">For hello security of your data, you are required toochange this password at hello end of hello registration process.</span></span> <span data-ttu-id="c9b6b-111">Du kan avsluta från hello registreringen utan att ändra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-111">You cannot exit from hello registration process without changing this password.</span></span> <span data-ttu-id="c9b6b-112">Mer information finns i [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="c9b6b-112">For more information, see [Step 3: Configure and register hello device through Windows PowerShell for StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span></span>

<span data-ttu-id="c9b6b-113">hello-lösenord som först har angetts via Windows PowerShell-gränssnittet för hello under registreringen kan ändras senare via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-113">hello password that was first set through hello Windows PowerShell interface during registration can be changed later via hello Azure portal.</span></span> <span data-ttu-id="c9b6b-114">Utför följande steg toochange hello enhetens administratörslösenord hello.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-114">Perform hello following steps toochange hello device administrator password.</span></span>

#### <a name="toochange-hello-device-administrator-password"></a><span data-ttu-id="c9b6b-115">toochange hello enhetens administratörslösenord</span><span class="sxs-lookup"><span data-stu-id="c9b6b-115">toochange hello device administrator password</span></span>
1. <span data-ttu-id="c9b6b-116">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-116">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="c9b6b-117">Välj hello tabular lista över enheter, och klicka på hello enhet vars lösenord du avser toochange.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-117">From hello tabular listing of devices, select and click hello device whose password you intend toochange.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="c9b6b-118">I hello **inställningar** bladet går för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-118">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="c9b6b-119">I hello **säkerhetsinställningar** bladet, klickar du på **lösenord** toochange hello enhetens administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-119">In hello **Security settings** blade, click **Password** toochange hello device administrator password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. <span data-ttu-id="c9b6b-120">I hello **lösenord** bladet, ange ett administratörslösenord som innehåller från 8 too15 tecken.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-120">In hello **Password** blade, provide an administrator password that contains from 8 too15 characters.</span></span> <span data-ttu-id="c9b6b-121">hello lösenordet måste vara en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-121">hello password must be a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="c9b6b-122">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-122">Confirm hello password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. <span data-ttu-id="c9b6b-123">Klicka på **spara** och när du uppmanas att bekräfta på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-123">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="c9b6b-124">hello enhetens administratörslösenord ska nu uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-124">hello device administrator password should now be updated.</span></span> <span data-ttu-id="c9b6b-125">Du kan använda den här ändrade lösenord tooaccess hello Windows PowerShell-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-125">You can use this modified password tooaccess hello Windows PowerShell interface.</span></span>

## <a name="set-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="c9b6b-126">Ange hello lösenordet för StorSimple Snapshot Manager</span><span class="sxs-lookup"><span data-stu-id="c9b6b-126">Set hello StorSimple Snapshot Manager password</span></span>
<span data-ttu-id="c9b6b-127">Programvaran StorSimple Manager finns på din Windows-värd och kan administratörer toomanage säkerhetskopior av StorSimple-enheten i hello form av lokala och molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-127">StorSimple Snapshot Manager software resides on your Windows host and allows administrators toomanage backups of your StorSimple device in hello form of local and cloud snapshots.</span></span>

<span data-ttu-id="c9b6b-128">När du konfigurerar en enhet i StorSimple Snapshot Manager, uppmanas du att tooprovide hello enhetens IP-adress och lösenord tooauthenticate lagringsenheten.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-128">When configuring a device in StorSimple Snapshot Manager, you will be prompted tooprovide hello device IP address and password tooauthenticate your storage device.</span></span>

<span data-ttu-id="c9b6b-129">Du kan ange eller ändra hello lösenordet för StorSimple Snapshot Manager via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-129">You can set or change hello password for StorSimple Snapshot Manager via hello Azure portal.</span></span> <span data-ttu-id="c9b6b-130">Utför följande steg tooset hello eller ändra hello lösenordet för StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-130">Perform hello following steps tooset or change hello StorSimple Snapshot Manager password.</span></span>

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="c9b6b-131">lösenordet för tooset hello StorSimple Snapshot Manager</span><span class="sxs-lookup"><span data-stu-id="c9b6b-131">tooset hello StorSimple Snapshot Manager password</span></span>
1. <span data-ttu-id="c9b6b-132">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-132">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="c9b6b-133">Välj hello tabular lista över enheter, och klicka på hello enhet vars lösenordet för StorSimple Snapshot Manager du avser tooset eller ändra.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-133">From hello tabular listing of devices, select and click hello device whose StorSimple Snapshot Manager password you intend tooset or change.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="c9b6b-134">I hello **inställningar** bladet går för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-134">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="c9b6b-135">I hello **säkerhetsinställningar** bladet, klickar du på **lösenord** lösenordet för StorSimple Snapshot Manager hello tooset eller ändra.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-135">In hello **Security settings** blade, click **Password** tooset or change hello StorSimple Snapshot Manager password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. <span data-ttu-id="c9b6b-136">I hello **lösenord** bladet, ange ett lösenord som är 14 eller 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-136">In hello **Password** blade, enter a password that is 14 or 15 characters.</span></span> <span data-ttu-id="c9b6b-137">Kontrollera att lösenordet hello innehåller en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-137">Make sure that hello password contains a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="c9b6b-138">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-138">Confirm hello password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. <span data-ttu-id="c9b6b-139">Klicka på **spara** och när du uppmanas att bekräfta på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-139">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="c9b6b-140">hello lösenordet för StorSimple Snapshot Manager ska nu uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c9b6b-140">hello StorSimple Snapshot Manager password should now be updated.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9b6b-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9b6b-141">Next steps</span></span>
* <span data-ttu-id="c9b6b-142">Lär dig mer om [StorSimple-säkerhet](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="c9b6b-142">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="c9b6b-143">Lär dig mer om [ändra din enhetskonfiguration](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="c9b6b-143">Learn more about [modifying your device configuration](storsimple-8000-modify-device-config.md).</span></span>
* <span data-ttu-id="c9b6b-144">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c9b6b-144">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

