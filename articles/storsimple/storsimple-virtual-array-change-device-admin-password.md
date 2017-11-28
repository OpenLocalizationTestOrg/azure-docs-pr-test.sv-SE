---
title: "administratörslösenord för aaaChange virtuella StorSimple-matris enhet | Microsoft Docs"
description: "Beskriver hur toouse antingen hello Azure-portalen eller virtuella StorSimple-matris web UI toochange hello enhetens administratörslösenord."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a><span data-ttu-id="eafdb-103">Ändra hello virtuella StorSimple-matris enhetens administratörslösenord via StorSimple Enhetshanteraren</span><span class="sxs-lookup"><span data-stu-id="eafdb-103">Change hello StorSimple Virtual Array device administrator password via StorSimple Device Manager</span></span>

## <a name="overview"></a><span data-ttu-id="eafdb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="eafdb-104">Overview</span></span>

<span data-ttu-id="eafdb-105">När du använder hello Windows PowerShell-gränssnittet tooaccess hello virtuella StorSimple-matrisen är du obligatoriska tooenter administratörslösenord för enheten.</span><span class="sxs-lookup"><span data-stu-id="eafdb-105">When you use hello Windows PowerShell interface tooaccess hello StorSimple Virtual Array, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="eafdb-106">När hello StorSimple-enheten först etablerats och igång, hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="eafdb-106">When hello StorSimple device is first provisioned and started, hello default password is *Password1*.</span></span> <span data-ttu-id="eafdb-107">För hello säkerheten för dina data, hello standardlösenordet upphör att gälla hello första gången du loggar in och du är obligatoriska toochange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="eafdb-107">For hello security of your data, hello default password expires hello first time that you sign in and you are required toochange this password.</span></span>

<span data-ttu-id="eafdb-108">Du kan också använda antingen hello lokala web användargränssnitt eller hello Azure portal toochange hello administratörslösenord för enheten när som helst efter hello enheten har distribuerats i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="eafdb-108">You can also use either hello local web UI or hello Azure portal toochange hello device administrator password at any time after hello device is deployed in your production environment.</span></span> <span data-ttu-id="eafdb-109">Var och en av de här procedurerna beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="eafdb-109">Each of these procedures is described in this article.</span></span>

 ![Bladet för enheter](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a><span data-ttu-id="eafdb-111">Använd hello Azure portal toochange hello lösenord</span><span class="sxs-lookup"><span data-stu-id="eafdb-111">Use hello Azure portal toochange hello password</span></span>

<span data-ttu-id="eafdb-112">Utföra hello följande steg toochange hello enhetens administratörslösenord via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="eafdb-112">Perform hello following steps toochange hello device administrator password through hello Azure portal.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a><span data-ttu-id="eafdb-113">Hej toochange enhetens administratörslösenord via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="eafdb-113">toochange hello device administrator password via hello Azure portal</span></span>

1. <span data-ttu-id="eafdb-114">Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Management** klickar du på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="eafdb-114">On hello service landing page, select your service, double-click hello service name, and then within hello **Management** section, click **Devices**.</span></span> <span data-ttu-id="eafdb-115">Då öppnas hello **enheter** blad som visar en lista över alla virtuella StorSimple-matris-enheter.</span><span class="sxs-lookup"><span data-stu-id="eafdb-115">This opens hello **Devices** blade that lists all your StorSimple Virtual Array devices.</span></span>

2. <span data-ttu-id="eafdb-116">I hello **enheter** bladet dubbelklickar på hello-enhet som kräver en ändring av lösenord.</span><span class="sxs-lookup"><span data-stu-id="eafdb-116">In hello **Devices** blade, double-click hello device that requires a change of password.</span></span>

3. <span data-ttu-id="eafdb-117">I hello **inställningar** bladet för din enhet klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="eafdb-117">In hello **Settings** blade for your device, click **Security**.</span></span>

4. <span data-ttu-id="eafdb-118">I hello **säkerhetsinställningar** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="eafdb-118">In hello **Security Settings** blade, do hello following:</span></span>
   
   1. <span data-ttu-id="eafdb-119">Bläddra nedåt toohello **administratörslösenordet för enheten** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eafdb-119">Scroll down toohello **Device Administrator Password** section.</span></span> <span data-ttu-id="eafdb-120">Ange ett administratörslösenord som innehåller från 8 too15 tecken.</span><span class="sxs-lookup"><span data-stu-id="eafdb-120">Provide an administrator password that contains from 8 too15 characters.</span></span>
   2. <span data-ttu-id="eafdb-121">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="eafdb-121">Confirm hello password.</span></span>
   3. <span data-ttu-id="eafdb-122">Klicka på **spara** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="eafdb-122">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="eafdb-123">hello enhetens administratörslösenord har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="eafdb-123">hello device administrator password is now updated.</span></span> <span data-ttu-id="eafdb-124">Du kan använda den här ändrade lösenord tooaccess hello enheten lokalt.</span><span class="sxs-lookup"><span data-stu-id="eafdb-124">You can use this modified password tooaccess hello device locally.</span></span>

![Säkerhet inställningsbladet](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a><span data-ttu-id="eafdb-126">Använd hello lokala web UI toochange hello lösenord</span><span class="sxs-lookup"><span data-stu-id="eafdb-126">Use hello local web UI toochange hello password</span></span>

<span data-ttu-id="eafdb-127">Utför följande steg toochange hello enhetens administratörslösenord via hello lokala webbgränssnittet hello.</span><span class="sxs-lookup"><span data-stu-id="eafdb-127">Perform hello following steps toochange hello device administrator password through hello local web UI.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a><span data-ttu-id="eafdb-128">Hej toochange enhetens administratörslösenord via hello lokala webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="eafdb-128">toochange hello device administrator password via hello local web UI</span></span>

1. <span data-ttu-id="eafdb-129">I hello lokala webbgränssnittet, klickar du på **Underhåll** > **lösenordsändring** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="eafdb-129">In hello local web UI, click **Maintenance** > **Password change** for your device.</span></span>
   
    ![Ändra password1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. <span data-ttu-id="eafdb-131">Ange hello **nuvarande lösenord**.</span><span class="sxs-lookup"><span data-stu-id="eafdb-131">Enter hello **Current password**.</span></span>
3. <span data-ttu-id="eafdb-132">Ange en **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="eafdb-132">Provide a **New Password**.</span></span> <span data-ttu-id="eafdb-133">hello lösenordet måste vara minst 8 tecken.</span><span class="sxs-lookup"><span data-stu-id="eafdb-133">hello password must be at least 8 characters long.</span></span> <span data-ttu-id="eafdb-134">Det måste innehålla 3 av 4 av hello följande: versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="eafdb-134">It must contain 3 of 4 of hello following: uppercase, lowercase, numeric, and special characters.</span></span>
   
    <span data-ttu-id="eafdb-135">Observera att lösenordet inte får vara hello samma som hello senast 24 lösenord.</span><span class="sxs-lookup"><span data-stu-id="eafdb-135">Note that your password cannot be hello same as hello last 24 passwords.</span></span>
4. <span data-ttu-id="eafdb-136">Ange hello lösenord tooconfirm den.</span><span class="sxs-lookup"><span data-stu-id="eafdb-136">Reenter hello password tooconfirm it.</span></span>
   
    ![Ändra Lösenord2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. <span data-ttu-id="eafdb-138">Hello längst hello-sidan, klickar du på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="eafdb-138">At hello bottom of hello page, click **Apply**.</span></span> <span data-ttu-id="eafdb-139">hello nytt lösenord används nu.</span><span class="sxs-lookup"><span data-stu-id="eafdb-139">hello new password is now applied.</span></span> <span data-ttu-id="eafdb-140">Om hello lösenordsändring inte lyckas kan du se hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="eafdb-140">If hello password change is not successful, you see hello following error:</span></span>
   
    ![felaktigt lösenord](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    <span data-ttu-id="eafdb-142">När hello lösenord har uppdaterats, visas ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="eafdb-142">After hello password is successfully updated, you are notified.</span></span> <span data-ttu-id="eafdb-143">Du kan sedan använda den här ändrade lösenord tooaccess hello enheten lokalt.</span><span class="sxs-lookup"><span data-stu-id="eafdb-143">You can then use this modified password tooaccess hello device locally.</span></span>


## <a name="next-steps"></a><span data-ttu-id="eafdb-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eafdb-144">Next steps</span></span>
<span data-ttu-id="eafdb-145">Lär dig hur för[administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="eafdb-145">Learn how too[administer your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

