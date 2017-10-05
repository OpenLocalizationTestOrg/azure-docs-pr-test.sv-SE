---
title: "Hantera access control-poster för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur du hanterar access control-poster (ACRs) för att avgöra vilka värdar som kan ansluta till en volym på den virtuella StorSimple-matrisen."
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
ms.openlocfilehash: 2ce65aa4efba735305208f7a6d761bc2814d1b8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a><span data-ttu-id="7b090-103">Använd StorSimple Enhetshanteraren för att hantera access control-poster för virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="7b090-103">Use StorSimple Device Manager to manage access control records for StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="7b090-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7b090-104">Overview</span></span>

<span data-ttu-id="7b090-105">Access control-poster (ACRs) kan du ange vilka värdar som kan ansluta till en volym på den virtuella StorSimple-matrisen (kallas även den lokala virtuella enheten StorSimple).</span><span class="sxs-lookup"><span data-stu-id="7b090-105">Access control records (ACRs) allow you to specify which hosts can connect to a volume on the StorSimple Virtual Array (also known as the StorSimple on-premises virtual device).</span></span> <span data-ttu-id="7b090-106">ACRs är inställd på en viss volym och innehålla iSCSI-kvalificerade namn (kvalificerade) på värdarna.</span><span class="sxs-lookup"><span data-stu-id="7b090-106">ACRs are set to a specific volume and contain the iSCSI Qualified Names (IQNs) of the hosts.</span></span> <span data-ttu-id="7b090-107">När en värd försöker ansluta till en volym enhetskontroller ACR som är kopplad till volymen för IQN namn och om det finns en matchning, upprättas anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7b090-107">When a host tries to connect to a volume, the device checks the ACR associated with that volume for the IQN name, and if there is a match, then the connection is established.</span></span> <span data-ttu-id="7b090-108">Den **Access control-poster** bladet inom den **Configuration** avsnitt i Device Manager-tjänsten visar alla access control poster med motsvarande kvalificerade av värdarna.</span><span class="sxs-lookup"><span data-stu-id="7b090-108">The **Access control records** blade within the **Configuration** section of your Device Manager service displays all the access control records with the corresponding IQNs of the hosts.</span></span>

![Hantera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

<span data-ttu-id="7b090-110">Den här självstudiekursen beskriver följande vanliga ACR-relaterade aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="7b090-110">This tutorial explains the following common ACR-related tasks:</span></span>

* <span data-ttu-id="7b090-111">Hämta IQN</span><span class="sxs-lookup"><span data-stu-id="7b090-111">Get the IQN</span></span>
* <span data-ttu-id="7b090-112">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="7b090-112">Add an access control record</span></span>
* <span data-ttu-id="7b090-113">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="7b090-113">Edit an access control record</span></span>
* <span data-ttu-id="7b090-114">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="7b090-114">Delete an access control record</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="7b090-115">När du tilldelar en ACR till en volym, se till att volymen inte samtidigt kan kommas åt av mer än en icke-klustrad värd eftersom detta kan skada volymen.</span><span class="sxs-lookup"><span data-stu-id="7b090-115">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>
> * <span data-ttu-id="7b090-116">När du tar bort en ACR från en volym, se till att motsvarande värden inte har åtkomst till volymen eftersom borttagningen kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="7b090-116">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>


## <a name="get-the-iqn"></a><span data-ttu-id="7b090-117">Hämta IQN</span><span class="sxs-lookup"><span data-stu-id="7b090-117">Get the IQN</span></span>

<span data-ttu-id="7b090-118">Utför följande steg för att hämta IQN för en Windows-värd som kör Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="7b090-118">Perform the following steps to get the IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a><span data-ttu-id="7b090-119">Lägg till en ACR</span><span class="sxs-lookup"><span data-stu-id="7b090-119">Add an ACR</span></span>

<span data-ttu-id="7b090-120">Du använder **Access control-poster** bladet inom den **Configuration** avsnitt i Enhetshanteraren för StorSimple-tjänsten för att lägga till ACRs.</span><span class="sxs-lookup"><span data-stu-id="7b090-120">You use **Access control records** blade within the **Configuration** section of your StorSimple Device Manager service to add ACRs.</span></span> <span data-ttu-id="7b090-121">Normalt kan du associera en ACR med en volym.</span><span class="sxs-lookup"><span data-stu-id="7b090-121">Typically, you associate one ACR with one volume.</span></span>

<span data-ttu-id="7b090-122">Information om hur du kopplar en ACR med en volym, gå till [lägga till en volym](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="7b090-122">For information about associating an ACR with a volume, go to [add a volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b090-123">När du tilldelar en ACR till en volym, se till att volymen inte samtidigt kan kommas åt av mer än en icke-klustrad värd eftersom detta kan skada volymen.</span><span class="sxs-lookup"><span data-stu-id="7b090-123">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>


<span data-ttu-id="7b090-124">Utför följande steg för att lägga till en ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-124">Perform the following steps to add an ACR.</span></span>

#### <a name="to-add-an-acr"></a><span data-ttu-id="7b090-125">Att lägga till en ACR</span><span class="sxs-lookup"><span data-stu-id="7b090-125">To add an ACR</span></span>

1. <span data-ttu-id="7b090-126">Markera din tjänst på Landningssida för tjänsten, dubbelklickar du på tjänstens namn och sedan inom den **Configuration** klickar du på **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="7b090-126">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="7b090-127">I den **Access control-poster** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7b090-127">In the **Access control records** blade, click **Add**.</span></span>
3. <span data-ttu-id="7b090-128">I den **lägga till ACR** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7b090-128">In the **Add ACR** blade, do the following:</span></span>
   
    1. <span data-ttu-id="7b090-129">Ange ett **namn** för din ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-129">Supply a **Name** for your ACR.</span></span>
    
    2. <span data-ttu-id="7b090-130">Under **iSCSI-Initierarnamn**, ange IQN namnet på din Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="7b090-130">Under **iSCSI Initiator Name**, provide the IQN name of your Windows host.</span></span> <span data-ttu-id="7b090-131">Om du vill hämta IQN för Windows Server-värd, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7b090-131">To get the IQN of your Windows Server host, do the following:</span></span>
   
    3. <span data-ttu-id="7b090-132">Starta Microsoft iSCSI-initieraren på din Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="7b090-132">Start the Microsoft iSCSI initiator on your Windows host.</span></span> <span data-ttu-id="7b090-133">I fönstret iSCSI-Initieraregenskaper på den **Configuration** , markera och kopiera strängen från den **Initierarnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="7b090-133">In the iSCSI Initiator Properties window, on the **Configuration** tab, select and copy the string from the **Initiator Name** field.</span></span>
    <span data-ttu-id="7b090-134">Klistra in den här strängen i den **IQN** i den **lägga till ACR** bladet.</span><span class="sxs-lookup"><span data-stu-id="7b090-134">Paste this string in the **IQN** field in the **Add ACR** blade.</span></span>
   
    6. <span data-ttu-id="7b090-135">Klicka på **Lägg till** att lägga till ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-135">Click **Add** to add the ACR.</span></span>  
   
        ![Lägg till access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. <span data-ttu-id="7b090-137">Tabell listan uppdateras det här tillägget.</span><span class="sxs-lookup"><span data-stu-id="7b090-137">The tabular listing is updated to reflect this addition.</span></span>

## <a name="edit-an-acr"></a><span data-ttu-id="7b090-138">Redigera en ACR</span><span class="sxs-lookup"><span data-stu-id="7b090-138">Edit an ACR</span></span>

<span data-ttu-id="7b090-139">Du använder den **Access control-poster** bladet inom den **Configuration** avsnitt i Enhetshanteraren tjänsten i Azure portal för att redigera ACRs.</span><span class="sxs-lookup"><span data-stu-id="7b090-139">You use the **Access control records** blade within the **Configuration** section of your Device Manager service in the Azure portal to edit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="7b090-140">Du bör inte ändra en ACR som används för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7b090-140">You should not modify an ACR that is currently in use.</span></span> <span data-ttu-id="7b090-141">Om du vill redigera en ACR som är kopplade till en volym som används för närvarande, bör du först se volymen offline.</span><span class="sxs-lookup"><span data-stu-id="7b090-141">To edit an ACR associated with a volume that is currently in use, you should first take the volume offline.</span></span>


<span data-ttu-id="7b090-142">Utför följande steg om du vill redigera en ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-142">Perform the following steps to edit an ACR.</span></span>

#### <a name="to-edit-an-acr"></a><span data-ttu-id="7b090-143">Så här redigerar du en ACR</span><span class="sxs-lookup"><span data-stu-id="7b090-143">To edit an ACR</span></span>

1. <span data-ttu-id="7b090-144">Markera din tjänst på Landningssida för tjänsten, dubbelklickar du på tjänstens namn och sedan inom den **Configuration** avsnittet **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="7b090-144">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, **Access control records**.</span></span>
2. <span data-ttu-id="7b090-145">I den **Access control-poster** bladet från tabular lista av access control-poster, dubbelklicka på ACR som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="7b090-145">In the **Access control records** blade, from the tabular listing of the access control records, double-click the ACR that you wish to modify.</span></span>
3. <span data-ttu-id="7b090-146">I den **access control poster** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7b090-146">In the **Edit access control records** blade, do the following:</span></span>
   
    1. <span data-ttu-id="7b090-147">Ange det kvalificerade iSCSI-namnet för ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-147">Supply the IQN for the ACR.</span></span>
   
    2. <span data-ttu-id="7b090-148">Klicka på **spara** längst upp på bladet för att spara ändrade ACR.</span><span class="sxs-lookup"><span data-stu-id="7b090-148">Click **Save** at the top of the blade to save the modified ACR.</span></span> <span data-ttu-id="7b090-149">Du ser följande bekräftelsemeddelande:</span><span class="sxs-lookup"><span data-stu-id="7b090-149">You see the following confirmation message:</span></span>
   
        ![Redigera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a><span data-ttu-id="7b090-151">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="7b090-151">Delete an access control record</span></span>

<span data-ttu-id="7b090-152">Du använder den **Configuration** sida i Azure portal för att ta bort ACRs.</span><span class="sxs-lookup"><span data-stu-id="7b090-152">You use the **Configuration** page in the Azure portal to delete ACRs.</span></span>

> [!NOTE]
> 
> * <span data-ttu-id="7b090-153">Du bör inte ta bort en ACR som används för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7b090-153">You should not delete an ACR that is currently in use.</span></span> <span data-ttu-id="7b090-154">Om du vill ta bort en ACR som är kopplade till en volym som används måste ha du först volymen offline.</span><span class="sxs-lookup"><span data-stu-id="7b090-154">To delete an ACR associated with a volume that is currently in use, you should first take the volume offline.</span></span>
> * <span data-ttu-id="7b090-155">När du tar bort en ACR från en volym, se till att motsvarande värden inte har åtkomst till volymen eftersom borttagningen kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="7b090-155">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>


<span data-ttu-id="7b090-156">Utför följande steg för att ta bort en åtkomstkontrollpost.</span><span class="sxs-lookup"><span data-stu-id="7b090-156">Perform the following steps to delete an access control record.</span></span>

#### <a name="to-delete-an-access-control-record"></a><span data-ttu-id="7b090-157">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="7b090-157">To delete an access control record</span></span>

1. <span data-ttu-id="7b090-158">Markera din tjänst på Landningssida för tjänsten, dubbelklickar du på tjänstens namn och sedan inom den **Configuration** avsnittet **Access control-poster**.</span><span class="sxs-lookup"><span data-stu-id="7b090-158">On the service landing page, select your service, double-click the service name, and then within the **Configuration** section, **Access control records**.</span></span>

2. <span data-ttu-id="7b090-159">I den **Access control-poster** bladet från tabular lista av access control-poster, dubbelklicka på ACR som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="7b090-159">In the **Access control records** blade, from the tabular listing of the access control records, double-click the ACR that you wish to delete.</span></span>

3. <span data-ttu-id="7b090-160">Klicka på Redigera access control poster bladet **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7b090-160">In the Edit access control records blade, click **Delete**.</span></span>
   
    ![Ta bort ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. <span data-ttu-id="7b090-162">När du uppmanas att bekräfta, klickar du på **ta bort** att ta bort.</span><span class="sxs-lookup"><span data-stu-id="7b090-162">When prompted for confirmation, click **Delete** to continue with the deletion.</span></span> <span data-ttu-id="7b090-163">Tabell listan uppdateras borttagningen.</span><span class="sxs-lookup"><span data-stu-id="7b090-163">The tabular listing is updated to reflect the deletion.</span></span>
   
   ![Varningsmeddelande](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a><span data-ttu-id="7b090-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b090-165">Next steps</span></span>

* <span data-ttu-id="7b090-166">Lär dig mer om [lägga till volymer och konfigurera ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="7b090-166">Learn more about [adding volumes and configuring ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

