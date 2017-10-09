---
title: "aaaManage access control-poster för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur toomanage åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volym på hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a><span data-ttu-id="2b3d7-103">Använd StorSimple Enhetshanteraren toomanage kontroll åtkomstregister för virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="2b3d7-103">Use StorSimple Device Manager toomanage access control records for StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="2b3d7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2b3d7-104">Overview</span></span>

<span data-ttu-id="2b3d7-105">Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volym på hello virtuella StorSimple-matris (även kallat hello lokala virtuella enheten StorSimple).</span><span class="sxs-lookup"><span data-stu-id="2b3d7-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device).</span></span> <span data-ttu-id="2b3d7-106">ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="2b3d7-107">När en värd försöker tooconnect tooa volym, hello enhetskontroller hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning, upprättas hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name, and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="2b3d7-108">Hej **Access control-poster** bladet inom hello **Configuration** avsnitt i Device Manager-tjänsten visar alla hello access control-poster med hello motsvarande kvalificerade hello värdar.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-108">hello **Access control records** blade within hello **Configuration** section of your Device Manager service displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

![Hantera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

<span data-ttu-id="2b3d7-110">Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:</span><span class="sxs-lookup"><span data-stu-id="2b3d7-110">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="2b3d7-111">Hämta hello IQN</span><span class="sxs-lookup"><span data-stu-id="2b3d7-111">Get hello IQN</span></span>
* <span data-ttu-id="2b3d7-112">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="2b3d7-112">Add an access control record</span></span>
* <span data-ttu-id="2b3d7-113">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="2b3d7-113">Edit an access control record</span></span>
* <span data-ttu-id="2b3d7-114">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="2b3d7-114">Delete an access control record</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="2b3d7-115">När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-115">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="2b3d7-116">När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-116">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


## <a name="get-hello-iqn"></a><span data-ttu-id="2b3d7-117">Hämta hello IQN</span><span class="sxs-lookup"><span data-stu-id="2b3d7-117">Get hello IQN</span></span>

<span data-ttu-id="2b3d7-118">Utför följande steg tooget hello IQN för en Windows-värd som kör Windows Server 2012 hello.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-118">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a><span data-ttu-id="2b3d7-119">Lägg till en ACR</span><span class="sxs-lookup"><span data-stu-id="2b3d7-119">Add an ACR</span></span>

<span data-ttu-id="2b3d7-120">Du använder **Access control-poster** bladet inom hello **Configuration** avsnitt i din StorSimple Enhetshanteraren service tooadd ACRs.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-120">You use **Access control records** blade within hello **Configuration** section of your StorSimple Device Manager service tooadd ACRs.</span></span> <span data-ttu-id="2b3d7-121">Normalt kan du associera en ACR med en volym.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-121">Typically, you associate one ACR with one volume.</span></span>

<span data-ttu-id="2b3d7-122">Information om hur du kopplar en ACR med en volym finns för[lägga till en volym](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="2b3d7-122">For information about associating an ACR with a volume, go too[add a volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b3d7-123">När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-123">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>


<span data-ttu-id="2b3d7-124">Utför följande steg tooadd en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-124">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="2b3d7-125">tooadd en ACR</span><span class="sxs-lookup"><span data-stu-id="2b3d7-125">tooadd an ACR</span></span>

1. <span data-ttu-id="2b3d7-126">Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-126">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="2b3d7-127">I hello **Access control-poster** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-127">In hello **Access control records** blade, click **Add**.</span></span>
3. <span data-ttu-id="2b3d7-128">I hello **lägga till ACR** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b3d7-128">In hello **Add ACR** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="2b3d7-129">Ange ett **namn** för din ACR.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-129">Supply a **Name** for your ACR.</span></span>
    
    2. <span data-ttu-id="2b3d7-130">Under **iSCSI-Initierarnamn**, ange hello IQN namnet på din Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-130">Under **iSCSI Initiator Name**, provide hello IQN name of your Windows host.</span></span> <span data-ttu-id="2b3d7-131">tooget hello IQN för Windows Server-värd, hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b3d7-131">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
    3. <span data-ttu-id="2b3d7-132">Starta hello Microsoft iSCSI-initieraren på din Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-132">Start hello Microsoft iSCSI initiator on your Windows host.</span></span> <span data-ttu-id="2b3d7-133">I hello iSCSI-initieraren fönstret Egenskaper på hello **Configuration** , markera och kopiera hello sträng från hello **Initierarnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-133">In hello iSCSI Initiator Properties window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
    <span data-ttu-id="2b3d7-134">Klistra in den här strängen i hello **IQN** i hello **lägga till ACR** bladet.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-134">Paste this string in hello **IQN** field in hello **Add ACR** blade.</span></span>
   
    6. <span data-ttu-id="2b3d7-135">Klicka på **Lägg till** tooadd hello ACR.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-135">Click **Add** tooadd hello ACR.</span></span>  
   
        ![Lägg till access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. <span data-ttu-id="2b3d7-137">Hej tabular lista är uppdaterade tooreflect det här tillägget.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-137">hello tabular listing is updated tooreflect this addition.</span></span>

## <a name="edit-an-acr"></a><span data-ttu-id="2b3d7-138">Redigera en ACR</span><span class="sxs-lookup"><span data-stu-id="2b3d7-138">Edit an ACR</span></span>

<span data-ttu-id="2b3d7-139">Du använder hello **Access control-poster** bladet inom hello **Configuration** avsnitt i Enhetshanteraren tjänsten i hello Azure portal tooedit ACRs.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-139">You use hello **Access control records** blade within hello **Configuration** section of your Device Manager service in hello Azure portal tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="2b3d7-140">Du bör inte ändra en ACR som används för närvarande.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-140">You should not modify an ACR that is currently in use.</span></span> <span data-ttu-id="2b3d7-141">tooedit en ACR som är kopplade till en volym som används för närvarande, bör du först se hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-141">tooedit an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>


<span data-ttu-id="2b3d7-142">Utför följande steg tooedit en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-142">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-acr"></a><span data-ttu-id="2b3d7-143">tooedit en ACR</span><span class="sxs-lookup"><span data-stu-id="2b3d7-143">tooedit an ACR</span></span>

1. <span data-ttu-id="2b3d7-144">Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** avsnittet **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-144">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>
2. <span data-ttu-id="2b3d7-145">I hello **Access control-poster** bladet från hello tabular lista över hello access control-poster, dubbelklicka på hello ACR toomodify gärna.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-145">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="2b3d7-146">I hello **access control poster** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b3d7-146">In hello **Edit access control records** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="2b3d7-147">Ange hello IQN för hello ACR.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-147">Supply hello IQN for hello ACR.</span></span>
   
    2. <span data-ttu-id="2b3d7-148">Klicka på **spara** hello överst i hello bladet toosave hello ändrade ACR.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-148">Click **Save** at hello top of hello blade toosave hello modified ACR.</span></span> <span data-ttu-id="2b3d7-149">Du ser hello följande bekräftelsemeddelande:</span><span class="sxs-lookup"><span data-stu-id="2b3d7-149">You see hello following confirmation message:</span></span>
   
        ![Redigera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a><span data-ttu-id="2b3d7-151">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="2b3d7-151">Delete an access control record</span></span>

<span data-ttu-id="2b3d7-152">Du använder hello **Configuration** sida i hello Azure portal toodelete ACRs.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-152">You use hello **Configuration** page in hello Azure portal toodelete ACRs.</span></span>

> [!NOTE]
> 
> * <span data-ttu-id="2b3d7-153">Du bör inte ta bort en ACR som används för närvarande.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-153">You should not delete an ACR that is currently in use.</span></span> <span data-ttu-id="2b3d7-154">toodelete en ACR som är kopplade till en volym som används för närvarande, bör du först se hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-154">toodelete an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>
> * <span data-ttu-id="2b3d7-155">När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-155">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


<span data-ttu-id="2b3d7-156">Utför följande steg toodelete en åtkomstkontrollpost hello.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-156">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="2b3d7-157">toodelete en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="2b3d7-157">toodelete an access control record</span></span>

1. <span data-ttu-id="2b3d7-158">Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** avsnittet **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-158">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>

2. <span data-ttu-id="2b3d7-159">I hello **Access control-poster** bladet från hello tabular lista över hello access control-poster, dubbelklicka på hello ACR toodelete gärna.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-159">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toodelete.</span></span>

3. <span data-ttu-id="2b3d7-160">I hello redigera access control poster-bladet klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-160">In hello Edit access control records blade, click **Delete**.</span></span>
   
    ![Ta bort ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. <span data-ttu-id="2b3d7-162">När du uppmanas att bekräfta, klickar du på **ta bort** toocontinue med hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-162">When prompted for confirmation, click **Delete** toocontinue with hello deletion.</span></span> <span data-ttu-id="2b3d7-163">hello tabular lista är uppdaterade tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="2b3d7-163">hello tabular listing is updated tooreflect hello deletion.</span></span>
   
   ![Varningsmeddelande](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a><span data-ttu-id="2b3d7-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b3d7-165">Next steps</span></span>

* <span data-ttu-id="2b3d7-166">Lär dig mer om [lägga till volymer och konfigurera ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="2b3d7-166">Learn more about [adding volumes and configuring ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

