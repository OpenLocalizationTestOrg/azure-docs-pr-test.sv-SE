---
title: aaaManage access control-poster i StorSimple | Microsoft Docs
description: "Beskriver hur toouse åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet."
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
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="e7664-103">Använd hello StorSimple Manager service toomanage access control-poster</span><span class="sxs-lookup"><span data-stu-id="e7664-103">Use hello StorSimple Manager service toomanage access control records</span></span>

## <a name="overview"></a><span data-ttu-id="e7664-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e7664-104">Overview</span></span>
<span data-ttu-id="e7664-105">Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="e7664-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="e7664-106">ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar.</span><span class="sxs-lookup"><span data-stu-id="e7664-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="e7664-107">När en värd försöker tooconnect tooa volym, enhetskontroller hello hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning och sedan hello anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="e7664-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="e7664-108">Hej access control-poster i hello **Configuration** avsnitt i din StorSimple Enhetshanteraren service bladet visas alla hello access control-poster med hello motsvarande kvalificerade hello värdar.</span><span class="sxs-lookup"><span data-stu-id="e7664-108">hello access control records in hello **Configuration** section of your StorSimple Device Manager service blade display all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="e7664-109">Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e7664-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="e7664-110">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-110">Add an access control record</span></span>
* <span data-ttu-id="e7664-111">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-111">Edit an access control record</span></span>
* <span data-ttu-id="e7664-112">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-112">Delete an access control record</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="e7664-113">När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.</span><span class="sxs-lookup"><span data-stu-id="e7664-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="e7664-114">När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="e7664-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>

## <a name="get-hello-iqn"></a><span data-ttu-id="e7664-115">Hämta hello IQN</span><span class="sxs-lookup"><span data-stu-id="e7664-115">Get hello IQN</span></span>

<span data-ttu-id="e7664-116">Utför följande steg tooget hello IQN för en Windows-värd som kör Windows Server 2012 hello.</span><span class="sxs-lookup"><span data-stu-id="e7664-116">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a><span data-ttu-id="e7664-117">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-117">Add an access control record</span></span>
<span data-ttu-id="e7664-118">Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet tooadd ACRs avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e7664-118">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooadd ACRs.</span></span> <span data-ttu-id="e7664-119">Normalt kopplar du en ACR en volym.</span><span class="sxs-lookup"><span data-stu-id="e7664-119">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="e7664-120">Utför följande steg tooadd en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e7664-120">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="e7664-121">tooadd en ACR</span><span class="sxs-lookup"><span data-stu-id="e7664-121">tooadd an ACR</span></span>

1. <span data-ttu-id="e7664-122">Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="e7664-122">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="e7664-123">I hello **Access control-poster** bladet, klickar du på **+ Lägg till ACR**.</span><span class="sxs-lookup"><span data-stu-id="e7664-123">In hello **Access control records** blade, click **+ Add ACR**.</span></span>

    ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. <span data-ttu-id="e7664-125">I hello **lägga till ACR** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7664-125">In hello **Add ACR** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="e7664-126">Ange ett namn för din ACR.</span><span class="sxs-lookup"><span data-stu-id="e7664-126">Supply a name for your ACR.</span></span>
    
    2. <span data-ttu-id="e7664-127">Ange hello IQN namnet på din Windows Server-värden under **iSCSI Initiator namn (IQN)**.</span><span class="sxs-lookup"><span data-stu-id="e7664-127">Provide hello IQN name of your Windows Server host under **iSCSI Initiator Name (IQN)**.</span></span>

    3. <span data-ttu-id="e7664-128">Klicka på **Lägg till** toocreate hello ACR.</span><span class="sxs-lookup"><span data-stu-id="e7664-128">Click **Add** toocreate hello ACR.</span></span>

        ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  <span data-ttu-id="e7664-130">hello tillagda nyligen ACR visas i hello tabular lista över ACRs.</span><span class="sxs-lookup"><span data-stu-id="e7664-130">hello newly added ACR will display in hello tabular listing of ACRs.</span></span>

    ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a><span data-ttu-id="e7664-132">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-132">Edit an access control record</span></span>
<span data-ttu-id="e7664-133">Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet tooedit ACRs avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e7664-133">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="e7664-134">Vi rekommenderar att du ändrar dessa ACRs som för närvarande inte används.</span><span class="sxs-lookup"><span data-stu-id="e7664-134">It is recommended that you modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="e7664-135">tooedit en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="e7664-135">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="e7664-136">Utför följande steg tooedit en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e7664-136">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="e7664-137">tooedit en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-137">tooedit an access control record</span></span>
1.  <span data-ttu-id="e7664-138">Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="e7664-138">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="e7664-140">Klicka i hello tabular lista över hello access control-poster, och välj hello ACR toomodify gärna.</span><span class="sxs-lookup"><span data-stu-id="e7664-140">In hello tabular listing of hello access control records, click and select hello ACR that you wish toomodify.</span></span>

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr1.png)

3. <span data-ttu-id="e7664-142">I hello **redigera åtkomstkontrollpost** bladet, ange en annan IQN motsvarande tooanother värd.</span><span class="sxs-lookup"><span data-stu-id="e7664-142">In hello **Edit access control record** blade, provide a different IQN corresponding tooanother host.</span></span>

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr2.png)

4. <span data-ttu-id="e7664-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e7664-144">Click **Save**.</span></span> <span data-ttu-id="e7664-145">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e7664-145">When prompted for confirmation, click **Yes**.</span></span> 

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr3.png)

5. <span data-ttu-id="e7664-147">Du meddelas när hello ACR uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e7664-147">You are notified when hello ACR is updated.</span></span> <span data-ttu-id="e7664-148">hello tabular lista uppdateras också tooreflect hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="e7664-148">hello tabular listing also updates tooreflect hello change.</span></span>

   
## <a name="delete-an-access-control-record"></a><span data-ttu-id="e7664-149">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-149">Delete an access control record</span></span>
<span data-ttu-id="e7664-150">Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet toodelete ACRs avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e7664-150">You use hello **Configuration** section in hello StorSimple Device Manager service blade toodelete ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="e7664-151">Du kan ta bort dessa ACRs som för närvarande inte används.</span><span class="sxs-lookup"><span data-stu-id="e7664-151">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="e7664-152">toodelete en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="e7664-152">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="e7664-153">Utför följande steg toodelete en åtkomstkontrollpost hello.</span><span class="sxs-lookup"><span data-stu-id="e7664-153">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="e7664-154">toodelete en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="e7664-154">toodelete an access control record</span></span>
1.  <span data-ttu-id="e7664-155">Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="e7664-155">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="e7664-157">Klicka i hello tabular lista över hello access control-poster, och välj hello ACR toodelete gärna.</span><span class="sxs-lookup"><span data-stu-id="e7664-157">In hello tabular listing of hello access control records, click and select hello ACR that you wish toodelete.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. <span data-ttu-id="e7664-159">Högerklicka på tooinvoke hello snabbmenyn och välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e7664-159">Right-click tooinvoke hello context menu and select **Delete**.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. <span data-ttu-id="e7664-161">När du uppmanas att bekräfta, granska hello information och klickar sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e7664-161">When prompted for confirmation, review hello information and then click **Delete**.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. <span data-ttu-id="e7664-163">Du meddelas när hello borttagningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e7664-163">You are notified when hello deletion completes.</span></span> <span data-ttu-id="e7664-164">hello tabular lista är uppdaterade tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="e7664-164">hello tabular listing is updated tooreflect hello deletion.</span></span>

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a><span data-ttu-id="e7664-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7664-166">Next steps</span></span>
* <span data-ttu-id="e7664-167">Lär dig mer om [hantera StorSimple-volymer](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="e7664-167">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="e7664-168">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e7664-168">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

