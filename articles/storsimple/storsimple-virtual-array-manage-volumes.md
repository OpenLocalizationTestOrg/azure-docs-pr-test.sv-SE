---
title: "aaaManage volymer på virtuella StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hello StorSimple Enhetshanteraren och förklarar hur toouse den toomanage volymer på din virtuella StorSimple-matrisen."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a><span data-ttu-id="c4ab7-103">Använd StorSimple Enhetshanteraren service toomanage volymer på hello virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="c4ab7-103">Use StorSimple Device Manager service toomanage volumes on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="c4ab7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c4ab7-104">Overview</span></span>

<span data-ttu-id="c4ab7-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera volymer på din virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage volumes on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="c4ab7-106">Hej StorSimple enheten Manager-tjänsten är ett tillägg i hello Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="c4ab7-107">I tillägg toomanaging filresurser och volymer kan du använda hello StorSimple Enhetshanteraren service tooview och hantera enheter, Visa aviseringar, och visa och hantera principer för säkerhetskopiering och hello säkerhetskopieringskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, and view and manage backup policies and hello backup catalog.</span></span>

## <a name="volume-types"></a><span data-ttu-id="c4ab7-108">Volymtyper</span><span class="sxs-lookup"><span data-stu-id="c4ab7-108">Volume Types</span></span>

<span data-ttu-id="c4ab7-109">StorSimple-volymer kan vara:</span><span class="sxs-lookup"><span data-stu-id="c4ab7-109">StorSimple volumes can be:</span></span>

* <span data-ttu-id="c4ab7-110">**Lokalt Fäst**: Data på dessa volymer förblir på hello matrisen och inte läcker över toohello moln.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-110">**Locally pinned**: Data in these volumes stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="c4ab7-111">**Nivåer**: Data i dessa volymer kan läcker över toohello moln.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-111">**Tiered**: Data in these volumes can spill toohello cloud.</span></span> <span data-ttu-id="c4ab7-112">När du skapar en nivåindelad volym cirka 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-112">When you create a tiered volume, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="c4ab7-113">Till exempel om du har etablerat en volym 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-113">For example, if you provisioned a 1 TB volume, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="c4ab7-114">I sin tur innebär detta att du slut alla hello lokalt utrymme på hello enheten inte kan etablera en nivåindelad volym (eftersom hello 10% krävs på hello lokala nivå inte är tillgänglig).</span><span class="sxs-lookup"><span data-stu-id="c4ab7-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered volume (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="c4ab7-115">Etablerad kapacitet</span><span class="sxs-lookup"><span data-stu-id="c4ab7-115">Provisioned capacity</span></span>
<span data-ttu-id="c4ab7-116">Läs toohello i den följande tabellen för maximal etablerad kapacitet för varje volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-116">Refer toohello following table for maximum provisioned capacity for each volume type.</span></span>

| <span data-ttu-id="c4ab7-117">**Gränsen identifierare**</span><span class="sxs-lookup"><span data-stu-id="c4ab7-117">**Limit identifier**</span></span>                                       | <span data-ttu-id="c4ab7-118">**Gränsen**</span><span class="sxs-lookup"><span data-stu-id="c4ab7-118">**Limit**</span></span>     |
|------------------------------------------------------------|---------------|
| <span data-ttu-id="c4ab7-119">Minsta storlek på en nivåindelad volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-119">Minimum size of a tiered volume</span></span>                            | <span data-ttu-id="c4ab7-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="c4ab7-120">500 GB</span></span>        |
| <span data-ttu-id="c4ab7-121">Maximal storlek för en nivåindelad volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-121">Maximum size of a tiered volume</span></span>                            | <span data-ttu-id="c4ab7-122">5 TB</span><span class="sxs-lookup"><span data-stu-id="c4ab7-122">5 TB</span></span>          |
| <span data-ttu-id="c4ab7-123">Minsta storleken för en lokalt Fäst volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-123">Minimum size of a locally pinned volume</span></span>                    | <span data-ttu-id="c4ab7-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="c4ab7-124">50 GB</span></span>         |
| <span data-ttu-id="c4ab7-125">Maximal storlek för en lokalt Fäst volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-125">Maximum size of a locally pinned volume</span></span>                    | <span data-ttu-id="c4ab7-126">500 GB</span><span class="sxs-lookup"><span data-stu-id="c4ab7-126">500 GB</span></span>        |

## <a name="hello-volumes-blade"></a><span data-ttu-id="c4ab7-127">hello volymer bladet</span><span class="sxs-lookup"><span data-stu-id="c4ab7-127">hello Volumes blade</span></span>
<span data-ttu-id="c4ab7-128">Hej **volymer** menyn på din StorSimple-tjänsten sammanfattning bladet visar hello lista över lagringsvolymer på en viss StorSimple-matris och gör att du toomanage dem.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-128">hello **Volumes** menu on your StorSimple service summary blade displays hello list of storage volumes on a given StorSimple array and allows you toomanage them.</span></span>

![Volymer bladet](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

<span data-ttu-id="c4ab7-130">En volym som består av en serie med attribut:</span><span class="sxs-lookup"><span data-stu-id="c4ab7-130">A volume consists of a series of attributes:</span></span>

* <span data-ttu-id="c4ab7-131">**Volymnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-131">**Volume Name** – A descriptive name that must be unique and helps identify hello volume.</span></span>
* <span data-ttu-id="c4ab7-132">**Status för** – kan vara online eller offline.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="c4ab7-133">Om en volym är offline, men det är inte synliga tooinitiators (servrar) som tillåts åtkomst toouse hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-133">If a volume is offline, it is not visible tooinitiators (servers) that are allowed access toouse hello volume.</span></span>
* <span data-ttu-id="c4ab7-134">**Typen** – anger om hello volymen är **skiktindelade** (hello standard) eller **lokalt Fäst**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-134">**Type** – Indicates whether hello volume is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="c4ab7-135">**Kapacitet** – anger hello mängden data som används som jämfört med toohello totala mängden data som kan lagras av hello initierare (server).</span><span class="sxs-lookup"><span data-stu-id="c4ab7-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored by hello initiator (server).</span></span>
* <span data-ttu-id="c4ab7-136">**Säkerhetskopiering** – om av hello virtuella StorSimple-matris, aktiveras automatiskt alla volymer för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-136">**Backup** – In case of hello StorSimple Virtual Array, all volumes are automatically enabled for backup.</span></span>
* <span data-ttu-id="c4ab7-137">**Ansluten värdar** – anger hello initierarna (servrarna) som tillåts åtkomst toothis volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-137">**Connected hosts** – Specifies hello initiators (servers) that are allowed access toothis volume.</span></span>

![Information om volymer](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

<span data-ttu-id="c4ab7-139">Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c4ab7-139">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="c4ab7-140">Lägg till en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-140">Add a volume</span></span>
* <span data-ttu-id="c4ab7-141">Ändra en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-141">Modify a volume</span></span>
* <span data-ttu-id="c4ab7-142">Koppla från en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-142">Take a volume offline</span></span>
* <span data-ttu-id="c4ab7-143">Ta bort en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-143">Delete a volume</span></span>

## <a name="add-a-volume"></a><span data-ttu-id="c4ab7-144">Lägg till en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-144">Add a volume</span></span>

1. <span data-ttu-id="c4ab7-145">Från hello StorSimple-tjänsten sammanfattning-bladet, klickar du på **+ Lägg till volymen** från hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-145">From hello StorSimple service summary blade, click **+ Add volume** from hello command bar.</span></span> <span data-ttu-id="c4ab7-146">Detta öppnar hello **Lägg till volymen** bladet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-146">This opens up hello **Add volume** blade.</span></span>
   
    ![Lägg till volym](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. <span data-ttu-id="c4ab7-148">I hello **Lägg till volymen** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="c4ab7-148">In hello **Add volume** blade, do hello following:</span></span>
   
   * <span data-ttu-id="c4ab7-149">I hello **volymnamn** , ange ett unikt namn för volymen.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-149">In hello **Volume name** field, enter a unique name for your volume.</span></span> <span data-ttu-id="c4ab7-150">hello namn måste vara en sträng som innehåller 3 too127 tecken.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-150">hello name must be a string that contains 3 too127 characters.</span></span>
   * <span data-ttu-id="c4ab7-151">I hello **typen** listrutan anger om toocreate en **skiktindelade** eller **lokalt Fäst** volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-151">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** volume.</span></span> <span data-ttu-id="c4ab7-152">Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj **lokalt Fäst volym**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-152">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned volume**.</span></span> <span data-ttu-id="c4ab7-153">Alla andra data, Välj **skiktindelade** volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-153">For all other data, select **Tiered** volume.</span></span>
   * <span data-ttu-id="c4ab7-154">I hello **kapacitet** anger hello storleken på hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-154">In hello **Capacity** field, specify hello size of hello volume.</span></span> <span data-ttu-id="c4ab7-155">En nivåindelad volym måste vara mellan 500 GB och 5 TB och en lokalt Fäst volym måste vara mellan 50 och 500 GB.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-155">A tiered volume must be between 500 GB and 5 TB and a locally pinned volume must be between 50 GB and 500 GB.</span></span>
   * * <span data-ttu-id="c4ab7-156">Klicka på **anslutna värdar**, Välj en access control post (ACR) motsvarande toohello iSCSI-initierare du vill tooconnect toothis volymen och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-156">Click **Connected hosts**, select an access control record (ACR) corresponding toohello iSCSI initiator that you want tooconnect toothis volume, and then click **Select**.</span></span>
3. <span data-ttu-id="c4ab7-157">tooadd en ny anslutna värddator klickar du på **Lägg till ny**, ange ett namn för hello värden och dess iSCSI-kvalificerade namn (IQN) och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-157">tooadd a new connected host, click **Add new**, enter a name for hello host and its iSCSI Qualified Name (IQN), and then click **Add**.</span></span>
   
    ![Lägg till volym](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. <span data-ttu-id="c4ab7-159">När du har konfigurerat din volym klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-159">When you've finished configuring your volume, click **Create**.</span></span> <span data-ttu-id="c4ab7-160">En volym skapas med hello angetts inställningar och du visas ett meddelande på hello har skapats hello samma.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-160">A volume will be created with hello specified settings and you will see a notification on hello successful creation of hello same.</span></span> <span data-ttu-id="c4ab7-161">Som standard aktiveras säkerhetskopiering för hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-161">By default backup will be enabled for hello volume.</span></span>
5. <span data-ttu-id="c4ab7-162">tooconfirm som hello volymen har har skapats, gå toohello **volymer** bladet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-162">tooconfirm that hello volume was successfully created, go toohello **Volumes** blade.</span></span> <span data-ttu-id="c4ab7-163">Du bör se hello volym som visas.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-163">You should see hello volume listed.</span></span>
   
    ![Volymen skapa lyckades](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a><span data-ttu-id="c4ab7-165">Ändra en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-165">Modify a volume</span></span>

<span data-ttu-id="c4ab7-166">Ändra en volym när du behöver toochange hello värdar som har åtkomst till hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-166">Modify a volume when you need toochange hello hosts that access hello volume.</span></span> <span data-ttu-id="c4ab7-167">hello kan andra attribut för en volym inte ändras när hello volymen har skapats.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-167">hello other attributes of a volume cannot be modified once hello volume has been created.</span></span>

#### <a name="toomodify-a-volume"></a><span data-ttu-id="c4ab7-168">toomodify en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-168">toomodify a volume</span></span>

1. <span data-ttu-id="c4ab7-169">Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill toomodify finns.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-169">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toomodify resides.</span></span>
2. <span data-ttu-id="c4ab7-170">**Välj** hello volymen och klickar på **anslutna värdar** tooview hello anslutna värden och ändra den tooa annan server.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-170">**Select** hello volume and click **Connected hosts** tooview hello currently connected host and modify it tooa different server.</span></span>
   
    ![Redigera volym](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. <span data-ttu-id="c4ab7-172">Spara dina ändringar genom att klicka på hello **spara** kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-172">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="c4ab7-173">Inställningarna för angivna tillämpas och ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-173">Your specified settings will be applied and you will see a notification.</span></span>

## <a name="take-a-volume-offline"></a><span data-ttu-id="c4ab7-174">Koppla från en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-174">Take a volume offline</span></span>

<span data-ttu-id="c4ab7-175">Du kanske måste tootake en volym offline när du planerar toomodify den eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-175">You may need tootake a volume offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="c4ab7-176">När en volym är offline, är den inte tillgänglig för skrivskyddad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-176">When a volume is offline, it is not available for read-write access.</span></span> <span data-ttu-id="c4ab7-177">Du behöver tootake hello volymen offline på värden för hello samt på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-177">You will need tootake hello volume offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-volume-offline"></a><span data-ttu-id="c4ab7-178">tootake en volym som är offline</span><span class="sxs-lookup"><span data-stu-id="c4ab7-178">tootake a volume offline</span></span>

1. <span data-ttu-id="c4ab7-179">Kontrollera att hello volymen i fråga inte används innan den tas offline.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-179">Make sure that hello volume in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="c4ab7-180">Ta hello volymen offline på värden för hello första.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-180">Take hello volume offline on hello host first.</span></span> <span data-ttu-id="c4ab7-181">Detta tar bort alla potentiella risken för korrupta data på hello volym.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-181">This eliminates any potential risk of data corruption on hello volume.</span></span> <span data-ttu-id="c4ab7-182">Specifika anvisningar finns i toohello instruktioner för operativsystemet för värden.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-182">For specific steps, refer toohello instructions for your host operating system.</span></span>
3. <span data-ttu-id="c4ab7-183">När hello volymen på hello värden är offline kan du ta hello volymen på hello matrisen offline genom att utföra följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="c4ab7-183">After hello volume on hello host is offline, take hello volume on hello array  offline by performing hello following steps:</span></span>
   
   * <span data-ttu-id="c4ab7-184">Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill tootake offline finns.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-184">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you tootake offline resides.</span></span>
   * <span data-ttu-id="c4ab7-185">**Välj** hello volymen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta offline**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-185">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Offline volym](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * <span data-ttu-id="c4ab7-187">Studera hello hello **ta offline** bladet och bekräfta ditt godkännande av hello igen.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-187">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="c4ab7-188">Klicka på **ta offline** tootake hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-188">Click **Take offline** tootake hello volume offline.</span></span> <span data-ttu-id="c4ab7-189">Visas ett meddelande om hello-åtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-189">You will see a notification of hello operation in progress.</span></span>
   * <span data-ttu-id="c4ab7-190">tooconfirm hello volymen har sparats offline, gå toohello **volymer** bladet.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-190">tooconfirm that hello volume was successfully taken offline, go toohello **Volumes** blade.</span></span> <span data-ttu-id="c4ab7-191">Du bör se hello status för hello volym som offline.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-191">You should see hello status of hello volume as offline.</span></span>
     
       ![Offline volym bekräftelse](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a><span data-ttu-id="c4ab7-193">Ta bort en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-193">Delete a volume</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4ab7-194">Du kan ta bort en volym endast om den är offline.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-194">You can delete a volume only if it is offline.</span></span>
> 
> 

<span data-ttu-id="c4ab7-195">Slutför följande steg toodelete en volym hello.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-195">Complete hello following steps toodelete a volume.</span></span>

#### <a name="toodelete-a-volume"></a><span data-ttu-id="c4ab7-196">toodelete en volym</span><span class="sxs-lookup"><span data-stu-id="c4ab7-196">toodelete a volume</span></span>

1. <span data-ttu-id="c4ab7-197">Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill toodelete finns.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-197">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toodelete resides.</span></span>
2. <span data-ttu-id="c4ab7-198">**Välj** hello volymen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-198">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Ta bort volym](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. <span data-ttu-id="c4ab7-200">Kontrollera status för hello hello volym som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-200">Check hello status of hello volume you want toodelete.</span></span> <span data-ttu-id="c4ab7-201">Om hello volymen som du vill toodelete inte är offline kan försättas offline första, följande hello steg [kopplar från en volym](#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="c4ab7-201">If hello volume you want toodelete is not offline, take it offline first, following hello steps in [Take a volume offline](#take-a-volume-offline).</span></span>
4. <span data-ttu-id="c4ab7-202">När du uppmanas att bekräfta i hello **ta bort** bladet acceptera hello bekräftelse och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-202">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="c4ab7-203">hello volymen tas bort och hello **volymer** bladet visar hello uppdaterad lista över volymer inom hello virtuella matris.</span><span class="sxs-lookup"><span data-stu-id="c4ab7-203">hello volume will now be deleted and hello **Volumes** blade will show hello updated list of volumes within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4ab7-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4ab7-204">Next steps</span></span>

<span data-ttu-id="c4ab7-205">Lär dig hur för[klona en StorSimple-volym](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="c4ab7-205">Learn how too[clone a StorSimple volume](storsimple-virtual-array-clone.md).</span></span>

