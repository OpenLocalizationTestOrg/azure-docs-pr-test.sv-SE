---
title: aaaManage virtuella StorSimple-matris delar | Microsoft Docs
description: "Beskriver hello StorSimple Enhetshanteraren och förklarar hur toouse den toomanage resurser på din virtuella StorSimple-matrisen."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a><span data-ttu-id="e90d9-103">Använda hello StorSimple Enhetshanteraren service toomanage resurser på hello virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="e90d9-103">Use hello StorSimple Device Manager service toomanage shares on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="e90d9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e90d9-104">Overview</span></span>

<span data-ttu-id="e90d9-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera resurser på din virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage shares on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="e90d9-106">Hej StorSimple enheten Manager-tjänsten är ett tillägg i hello Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e90d9-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="e90d9-107">I tillägg toomanaging filresurser och volymer kan du använda hello StorSimple Enhetshanteraren service tooview och hantera enheter, Visa aviseringar, hantera principer för säkerhetskopiering och hantera hello säkerhetskopieringskatalogen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, manage backup policies, and manage hello backup catalog.</span></span>

## <a name="share-types"></a><span data-ttu-id="e90d9-108">Dela typer</span><span class="sxs-lookup"><span data-stu-id="e90d9-108">Share Types</span></span>

<span data-ttu-id="e90d9-109">StorSimple resurser kan vara:</span><span class="sxs-lookup"><span data-stu-id="e90d9-109">StorSimple shares can be:</span></span>

* <span data-ttu-id="e90d9-110">**Lokalt Fäst**: Data i resurserna ligger på hello matrisen vid alla tidpunkter och inte läcker över toohello moln.</span><span class="sxs-lookup"><span data-stu-id="e90d9-110">**Locally pinned**: Data in these shares stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="e90d9-111">**Nivåer**: Data i resurserna kan läcker över toohello moln.</span><span class="sxs-lookup"><span data-stu-id="e90d9-111">**Tiered**: Data in these shares can spill toohello cloud.</span></span> <span data-ttu-id="e90d9-112">När du skapar en nivåindelad resurs cirka 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-112">When you create a tiered share, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="e90d9-113">Till exempel om du har etablerat en resurs för 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer.</span><span class="sxs-lookup"><span data-stu-id="e90d9-113">For example, if you provisioned a 1 TB share, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="e90d9-114">I sin tur innebär detta att du slut alla hello lokalt utrymme på hello enheten inte kan etablera en skiktad resurs (eftersom hello 10% krävs på hello lokala nivå inte är tillgänglig).</span><span class="sxs-lookup"><span data-stu-id="e90d9-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered share (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="e90d9-115">Etablerad kapacitet</span><span class="sxs-lookup"><span data-stu-id="e90d9-115">Provisioned capacity</span></span>

<span data-ttu-id="e90d9-116">Mer information finns i den följande tabellen för maximal kapacitet för varje resurstyp av som etablerade toohello.</span><span class="sxs-lookup"><span data-stu-id="e90d9-116">Refer toohello following table for maximum provisioned capacity for each share type.</span></span>

| <span data-ttu-id="e90d9-117">**Gränsen identifierare**</span><span class="sxs-lookup"><span data-stu-id="e90d9-117">**Limit identifier**</span></span> | <span data-ttu-id="e90d9-118">**Gränsen**</span><span class="sxs-lookup"><span data-stu-id="e90d9-118">**Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="e90d9-119">Minsta storlek på en skiktad resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-119">Minimum size of a tiered share</span></span> |<span data-ttu-id="e90d9-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="e90d9-120">500 GB</span></span> |
| <span data-ttu-id="e90d9-121">Maximal storlek för en skiktad resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-121">Maximum size of a tiered share</span></span> |<span data-ttu-id="e90d9-122">20 TB</span><span class="sxs-lookup"><span data-stu-id="e90d9-122">20 TB</span></span> |
| <span data-ttu-id="e90d9-123">Minsta storleken för en lokalt Fäst resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-123">Minimum size of a locally pinned share</span></span> |<span data-ttu-id="e90d9-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="e90d9-124">50 GB</span></span> |
| <span data-ttu-id="e90d9-125">Maximal storlek för en lokalt Fäst resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-125">Maximum size of a locally pinned share</span></span> |<span data-ttu-id="e90d9-126">2 TB</span><span class="sxs-lookup"><span data-stu-id="e90d9-126">2 TB</span></span> |

## <a name="hello-shares-blade"></a><span data-ttu-id="e90d9-127">hello resurser bladet</span><span class="sxs-lookup"><span data-stu-id="e90d9-127">hello Shares blade</span></span>

<span data-ttu-id="e90d9-128">Hej **resurser** menyn på din StorSimple-tjänsten sammanfattning bladet visar hello listan över resurser för lagring på en viss StorSimple-matris och gör att du toomanage dem.</span><span class="sxs-lookup"><span data-stu-id="e90d9-128">hello **Shares** menu on your StorSimple service summary blade displays hello list of storage shares on a given StorSimple array and allows you toomanage them.</span></span>

![Resurser-bladet](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

<span data-ttu-id="e90d9-130">En resurs består av en serie med attribut:</span><span class="sxs-lookup"><span data-stu-id="e90d9-130">A share consists of a series of attributes:</span></span>

* <span data-ttu-id="e90d9-131">**Resursnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-131">**Share Name** – A descriptive name that must be unique and helps identify hello share.</span></span>
* <span data-ttu-id="e90d9-132">**Status för** – kan vara online eller offline.</span><span class="sxs-lookup"><span data-stu-id="e90d9-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="e90d9-133">Om en resurs som är offline, användare hello-resursen inte kan tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="e90d9-133">If a share is offline, users of hello share will not be able tooaccess it.</span></span>
* <span data-ttu-id="e90d9-134">**Typen** – anger om hello resursen är **skiktindelade** (hello standard) eller **lokalt Fäst**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-134">**Type** – Indicates whether hello share is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="e90d9-135">**Kapacitet** – anger hello mängden data som används som jämfört med toohello totala mängden data som kan lagras på hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="e90d9-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored on hello share.</span></span>
* <span data-ttu-id="e90d9-136">**Beskrivning** – en valfri inställning som hjälper till att beskriva hello resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-136">**Description** – An optional setting that helps describe hello share.</span></span>
* <span data-ttu-id="e90d9-137">**Behörigheter** -hello NTFS-behörigheter toohello resurs som kan hanteras via Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="e90d9-137">**Permissions** - hello NTFS permissions toohello share that can be managed through Windows Explorer.</span></span>
* <span data-ttu-id="e90d9-138">**Säkerhetskopiering** – om av hello virtuella StorSimple-matris, aktiveras automatiskt alla resurser för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="e90d9-138">**Backup** – In case of hello StorSimple Virtual Array, all shares are automatically enabled for backup.</span></span>

![Delar information](./media/storsimple-virtual-array-manage-shares/share-details.png)

<span data-ttu-id="e90d9-140">Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e90d9-140">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="e90d9-141">Lägg till en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-141">Add a share</span></span>
* <span data-ttu-id="e90d9-142">Ändra en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-142">Modify a share</span></span>
* <span data-ttu-id="e90d9-143">Koppla från en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-143">Take a share offline</span></span>
* <span data-ttu-id="e90d9-144">Ta bort en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-144">Delete a share</span></span>

## <a name="add-a-share"></a><span data-ttu-id="e90d9-145">Lägg till en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-145">Add a share</span></span>

1. <span data-ttu-id="e90d9-146">Från hello StorSimple-tjänsten sammanfattning-bladet, klickar du på **+ Lägg till resursen** från hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-146">From hello StorSimple service summary blade, click **+ Add share** from hello command bar.</span></span> <span data-ttu-id="e90d9-147">Detta öppnar hello **Lägg till resursen** bladet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-147">This opens up hello **Add share** blade.</span></span>

    ![Lägg till resurs](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. <span data-ttu-id="e90d9-149">I hello **Lägg till resursen** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="e90d9-149">In hello **Add share** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="e90d9-150">I hello **resursnamn** , ange ett unikt namn för din resurs.</span><span class="sxs-lookup"><span data-stu-id="e90d9-150">In hello **Share name** field, enter a unique name for your share.</span></span> <span data-ttu-id="e90d9-151">hello namn måste vara en sträng som innehåller 3 too127 tecken.</span><span class="sxs-lookup"><span data-stu-id="e90d9-151">hello name must be a string that contains 3 too127 characters.</span></span>

    2. <span data-ttu-id="e90d9-152">En valfri **beskrivning** för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="e90d9-152">An optional **Description** for hello share.</span></span> <span data-ttu-id="e90d9-153">hello beskrivning hjälper dig att identifiera hello resurs-ägare.</span><span class="sxs-lookup"><span data-stu-id="e90d9-153">hello description will help identify hello share owners.</span></span>

    3. <span data-ttu-id="e90d9-154">I hello **typen** listrutan anger om toocreate en **skiktindelade** eller **lokalt Fäst** delar.</span><span class="sxs-lookup"><span data-stu-id="e90d9-154">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** share.</span></span> <span data-ttu-id="e90d9-155">Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj **lokalt Fäst resursen**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-155">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned share**.</span></span> <span data-ttu-id="e90d9-156">Alla andra data, Välj **skiktindelade** delar.</span><span class="sxs-lookup"><span data-stu-id="e90d9-156">For all other data, select **Tiered** share.</span></span>

    4. <span data-ttu-id="e90d9-157">I hello **kapacitet** anger hello storlek hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-157">In hello **Capacity** field, specify hello size of hello share.</span></span> <span data-ttu-id="e90d9-158">En skiktad resursen måste vara mellan 500 GB och 20 TB och en lokalt Fäst resursen måste vara mellan 50 GB och 2 TB.</span><span class="sxs-lookup"><span data-stu-id="e90d9-158">A tiered share must be between 500 GB and 20 TB and a locally pinned share must be between 50 GB and 2 TB.</span></span>

    5. <span data-ttu-id="e90d9-159">I hello **Ange standard fullständig behörighet** fältet, tilldela hello behörigheter toohello användare eller hello-grupp som har åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-159">In hello **Set default full permissions to** field, assign hello permissions toohello user, or hello group that is accessing this share.</span></span> <span data-ttu-id="e90d9-160">Ange hello namn hello användaren eller användargruppen för hello i  _john@contoso.com_  format.</span><span class="sxs-lookup"><span data-stu-id="e90d9-160">Specify hello name of hello user or hello user group in _john@contoso.com_ format.</span></span> <span data-ttu-id="e90d9-161">Vi rekommenderar att du använder en användare grupp (i stället för en enskild användare) tooallow admin privilegier tooaccess dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="e90d9-161">We recommend that you use a user group (instead of a single user) tooallow admin privileges tooaccess these shares.</span></span> <span data-ttu-id="e90d9-162">När du har tilldelat hello behörigheter här kan kan du sedan använda Utforskaren toomodify dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="e90d9-162">After you have assigned hello permissions here, you can then use File Explorer toomodify these permissions.</span></span>
3. <span data-ttu-id="e90d9-163">När du har konfigurerat din resurs klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-163">When you've finished configuring your share, click **Create**.</span></span> <span data-ttu-id="e90d9-164">En resurs med hello som angetts kommer att skapas inställningar och du ser ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="e90d9-164">A share will be created with hello specified settings and you will see a notification.</span></span> <span data-ttu-id="e90d9-165">Som standard ska säkerhetskopiering aktiveras för hello resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-165">By default, backup will be enabled for hello share.</span></span>
4. <span data-ttu-id="e90d9-166">tooconfirm som hello resursen har har skapats, gå toohello **resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-166">tooconfirm that hello share was successfully created, go toohello **Shares** blade.</span></span> <span data-ttu-id="e90d9-167">Du bör se hello dela listan.</span><span class="sxs-lookup"><span data-stu-id="e90d9-167">You should see hello share listed.</span></span>
   
    ![Dela skapa lyckades](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a><span data-ttu-id="e90d9-169">Ändra en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-169">Modify a share</span></span>

<span data-ttu-id="e90d9-170">Ändra en resurs när du behöver toochange hello beskrivning av hello resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-170">Modify a share when you need toochange hello description of hello share.</span></span> <span data-ttu-id="e90d9-171">Inga andra resursegenskaper kan inte ändras när hello resursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="e90d9-171">No other share properties can be modified once hello share is created.</span></span>

#### <a name="toomodify-a-share"></a><span data-ttu-id="e90d9-172">toomodify en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-172">toomodify a share</span></span>

1. <span data-ttu-id="e90d9-173">Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matrisen på vilka hello resursen som du vill toomodify finns.</span><span class="sxs-lookup"><span data-stu-id="e90d9-173">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you toomodify resides.</span></span>
2. <span data-ttu-id="e90d9-174">**Välj** hello resursen tooview hello aktuella beskrivning och ändra den.</span><span class="sxs-lookup"><span data-stu-id="e90d9-174">**Select** hello share tooview hello current description and modify it.</span></span>
3. <span data-ttu-id="e90d9-175">Spara dina ändringar genom att klicka på hello **spara** kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-175">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="e90d9-176">Inställningarna för angivna tillämpas och ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="e90d9-176">Your specified settings will be applied and you will see a notification.</span></span>
   
    ![ <span data-ttu-id="e90d9-177">Redigera resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-177">Edit share</span></span>](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a><span data-ttu-id="e90d9-178">Koppla från en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-178">Take a share offline</span></span>

<span data-ttu-id="e90d9-179">Du kanske måste tootake en resurs offline när du planerar toomodify den eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="e90d9-179">You may need tootake a share offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="e90d9-180">När en resurs är offline, är den inte tillgänglig för skrivskyddad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e90d9-180">When a share is offline, it is not available for read-write access.</span></span> <span data-ttu-id="e90d9-181">Du behöver tootake hello resursen offline på värden för hello samt på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-181">You will need tootake hello share offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-share-offline"></a><span data-ttu-id="e90d9-182">tootake en resurs som är offline</span><span class="sxs-lookup"><span data-stu-id="e90d9-182">tootake a share offline</span></span>

1. <span data-ttu-id="e90d9-183">Kontrollera att den aktuella hello resursen inte används innan den tas offline.</span><span class="sxs-lookup"><span data-stu-id="e90d9-183">Make sure that hello share in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="e90d9-184">Ta hello resursen på hello matrisen offline genom att utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e90d9-184">Take hello share on hello array offline by performing hello following steps:</span></span>
   
    1. <span data-ttu-id="e90d9-185">Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matrisen på vilka hello resursen som du vill tootake offline finns.</span><span class="sxs-lookup"><span data-stu-id="e90d9-185">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you tootake offline resides.</span></span>

    2. <span data-ttu-id="e90d9-186">**Välj** hello resursen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta offline**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-186">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Frånkopplad resurs](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. <span data-ttu-id="e90d9-188">Studera hello hello **ta offline** bladet och bekräfta ditt godkännande av hello igen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-188">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="e90d9-189">Klicka på **ta offline** tootake hello resursen offline.</span><span class="sxs-lookup"><span data-stu-id="e90d9-189">Click **Take offline** tootake hello share offline.</span></span> <span data-ttu-id="e90d9-190">Visas ett meddelande om hello-åtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="e90d9-190">You will see a notification of hello operation in progress.</span></span>

    4. <span data-ttu-id="e90d9-191">tooconfirm som hello resursen skapades har offline, gå toohello **resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="e90d9-191">tooconfirm that hello share was successfully taken offline, go toohello **Shares** blade.</span></span> <span data-ttu-id="e90d9-192">Du bör se hello statusen som offline hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-192">You should see hello status of hello share as offline.</span></span>

## <a name="delete-a-share"></a><span data-ttu-id="e90d9-193">Ta bort en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-193">Delete a share</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e90d9-194">Du kan ta bort en resurs bara om den är offline.</span><span class="sxs-lookup"><span data-stu-id="e90d9-194">You can delete a share only if it is offline.</span></span>


<span data-ttu-id="e90d9-195">Slutför följande steg toodelete en resurs hello.</span><span class="sxs-lookup"><span data-stu-id="e90d9-195">Complete hello following steps toodelete a share.</span></span>

#### <a name="toodelete-a-share"></a><span data-ttu-id="e90d9-196">toodelete en resurs</span><span class="sxs-lookup"><span data-stu-id="e90d9-196">toodelete a share</span></span>

1. <span data-ttu-id="e90d9-197">Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris som du vill toodelete finns på vilken hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="e90d9-197">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish toodelete resides.</span></span>
2. <span data-ttu-id="e90d9-198">**Välj** hello resursen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-198">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Ta bort resursen](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. <span data-ttu-id="e90d9-200">Kontrollera status för hello hello resursen som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="e90d9-200">Check hello status of hello share you want toodelete.</span></span> <span data-ttu-id="e90d9-201">Om hello resursen som du vill toodelete inte är offline, koppla från den först.</span><span class="sxs-lookup"><span data-stu-id="e90d9-201">If hello share you want toodelete is not offline, take it offline first.</span></span> <span data-ttu-id="e90d9-202">Gör så hello i [koppla en resurs från](#take-a-share-offline).</span><span class="sxs-lookup"><span data-stu-id="e90d9-202">Follow hello steps in [Take a share offline](#take-a-share-offline).</span></span>
4. <span data-ttu-id="e90d9-203">När du uppmanas att bekräfta i hello **ta bort** bladet acceptera hello bekräftelse och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e90d9-203">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="e90d9-204">hello resursen tas bort och hello **resurser** bladet visar hello uppdatera listan över resurser inom hello virtuella matris.</span><span class="sxs-lookup"><span data-stu-id="e90d9-204">hello share will now be deleted and hello **Shares** blade shows hello updated list of shares within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e90d9-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e90d9-205">Next steps</span></span>
<span data-ttu-id="e90d9-206">Lär dig hur för[klona en StorSimple-resurs](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="e90d9-206">Learn how too[clone a StorSimple share](storsimple-virtual-array-clone.md).</span></span>

