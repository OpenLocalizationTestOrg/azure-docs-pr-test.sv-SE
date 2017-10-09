---
title: aaaManage access control-poster i StorSimple | Microsoft Docs
description: "Beskriver hur toouse åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="f38e5-103">Använd hello StorSimple Manager service toomanage access control-poster</span><span class="sxs-lookup"><span data-stu-id="f38e5-103">Use hello StorSimple Manager service toomanage access control records</span></span>
## <a name="overview"></a><span data-ttu-id="f38e5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f38e5-104">Overview</span></span>
<span data-ttu-id="f38e5-105">Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f38e5-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="f38e5-106">ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar.</span><span class="sxs-lookup"><span data-stu-id="f38e5-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="f38e5-107">När en värd försöker tooconnect tooa volym, enhetskontroller hello hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning och sedan hello anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="f38e5-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="f38e5-108">hello åtkomstkontroll innehåller avsnitt om hello **konfigurera** sidan visas alla hello access control-poster med hello motsvarande kvalificerade hello värdar.</span><span class="sxs-lookup"><span data-stu-id="f38e5-108">hello access control records section on hello **Configure** page displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="f38e5-109">Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:</span><span class="sxs-lookup"><span data-stu-id="f38e5-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="f38e5-110">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-110">Add an access control record</span></span> 
* <span data-ttu-id="f38e5-111">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-111">Edit an access control record</span></span> 
* <span data-ttu-id="f38e5-112">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-112">Delete an access control record</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="f38e5-113">När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.</span><span class="sxs-lookup"><span data-stu-id="f38e5-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span> 
> * <span data-ttu-id="f38e5-114">När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.</span><span class="sxs-lookup"><span data-stu-id="f38e5-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>
> 
> 

## <a name="add-an-access-control-record"></a><span data-ttu-id="f38e5-115">Lägg till en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-115">Add an access control record</span></span>
<span data-ttu-id="f38e5-116">Du använder hello StorSimple Manager-tjänsten **konfigurera** sidan tooadd ACRs.</span><span class="sxs-lookup"><span data-stu-id="f38e5-116">You use hello StorSimple Manager service **Configure** page tooadd ACRs.</span></span> <span data-ttu-id="f38e5-117">Normalt kopplar du en ACR en volym.</span><span class="sxs-lookup"><span data-stu-id="f38e5-117">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="f38e5-118">Utför följande steg tooadd en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="f38e5-118">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-access-control-record"></a><span data-ttu-id="f38e5-119">tooadd en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-119">tooadd an access control record</span></span>
1. <span data-ttu-id="f38e5-120">På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="f38e5-120">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="f38e5-121">I tabellform lista hello **Access control-poster**, varor en **namn** för din ACR.</span><span class="sxs-lookup"><span data-stu-id="f38e5-121">In hello tabular listing under **Access control records**, supply a **Name** for your ACR.</span></span>
3. <span data-ttu-id="f38e5-122">Ange hello IQN namn för din Windows-värd under **iSCSI-Initierarnamn**.</span><span class="sxs-lookup"><span data-stu-id="f38e5-122">Provide hello IQN name of your Windows host under **iSCSI Initiator Name**.</span></span> <span data-ttu-id="f38e5-123">tooget hello IQN för Windows Server-värd, hello följande:</span><span class="sxs-lookup"><span data-stu-id="f38e5-123">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
   * <span data-ttu-id="f38e5-124">Starta hello Microsoft iSCSI-initieraren på din Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="f38e5-124">Start hello Microsoft iSCSI initiator on your Windows host.</span></span>
   * <span data-ttu-id="f38e5-125">I hello **iSCSI-Initieraregenskaper** fönstret på hello **Configuration** , markera och kopiera hello sträng från hello **Initierarnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="f38e5-125">In hello **iSCSI Initiator Properties** window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
   * <span data-ttu-id="f38e5-126">Klistra in den här strängen i hello **iSCSI-Initierarnamn** på hello ACRs tabellen i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f38e5-126">Paste this string in hello **iSCSI Initiator Name** field on hello ACRs table in hello Azure classic portal.</span></span>
4. <span data-ttu-id="f38e5-127">Klicka på **spara** toosave hello nyskapad ACR.</span><span class="sxs-lookup"><span data-stu-id="f38e5-127">Click **Save** toosave hello newly created ACR.</span></span> <span data-ttu-id="f38e5-128">hello tabular lista kommer vara uppdaterade tooreflect det här tillägget.</span><span class="sxs-lookup"><span data-stu-id="f38e5-128">hello tabular listing will be updated tooreflect this addition.</span></span>

## <a name="edit-an-access-control-record"></a><span data-ttu-id="f38e5-129">Redigera en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-129">Edit an access control record</span></span>
<span data-ttu-id="f38e5-130">Du använder hello **konfigurera** sida i hello Azure klassiska portal tooedit ACRs.</span><span class="sxs-lookup"><span data-stu-id="f38e5-130">You use hello **Configure** page in hello Azure classic portal tooedit ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="f38e5-131">Du kan ändra de ACRs som för närvarande inte används.</span><span class="sxs-lookup"><span data-stu-id="f38e5-131">You can modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="f38e5-132">tooedit en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="f38e5-132">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="f38e5-133">Utför följande steg tooedit en ACR hello.</span><span class="sxs-lookup"><span data-stu-id="f38e5-133">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="f38e5-134">tooedit en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-134">tooedit an access control record</span></span>
1. <span data-ttu-id="f38e5-135">På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="f38e5-135">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="f38e5-136">Hovra över hello ACR toomodify gärna i hello tabular lista av hello access control-poster.</span><span class="sxs-lookup"><span data-stu-id="f38e5-136">In hello tabular listing of hello access control records, hover over hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="f38e5-137">Ange ett nytt namn och/eller IQN för hello ACR.</span><span class="sxs-lookup"><span data-stu-id="f38e5-137">Supply a new name and/or IQN for hello ACR.</span></span>
4. <span data-ttu-id="f38e5-138">Klicka på **spara** toosave hello ändrade ACR.</span><span class="sxs-lookup"><span data-stu-id="f38e5-138">Click **Save** toosave hello modified ACR.</span></span> <span data-ttu-id="f38e5-139">hello tabular lista kommer vara uppdaterade tooreflect ändringen.</span><span class="sxs-lookup"><span data-stu-id="f38e5-139">hello tabular listing will be updated tooreflect this change.</span></span>

## <a name="delete-an-access-control-record"></a><span data-ttu-id="f38e5-140">Ta bort en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-140">Delete an access control record</span></span>
<span data-ttu-id="f38e5-141">Du använder hello **konfigurera** sida i hello Azure klassiska portal toodelete ACRs.</span><span class="sxs-lookup"><span data-stu-id="f38e5-141">You use hello **Configure** page in hello Azure classic portal toodelete ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="f38e5-142">Du kan ta bort dessa ACRs som för närvarande inte används.</span><span class="sxs-lookup"><span data-stu-id="f38e5-142">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="f38e5-143">toodelete en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="f38e5-143">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="f38e5-144">Utför följande steg toodelete en åtkomstkontrollpost hello.</span><span class="sxs-lookup"><span data-stu-id="f38e5-144">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="f38e5-145">toodelete en åtkomstkontrollpost</span><span class="sxs-lookup"><span data-stu-id="f38e5-145">toodelete an access control record</span></span>
1. <span data-ttu-id="f38e5-146">På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="f38e5-146">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="f38e5-147">Hovra över hello ACR toodelete gärna i hello tabular lista av hello access control-poster (ACRs).</span><span class="sxs-lookup"><span data-stu-id="f38e5-147">In hello tabular listing of hello access control records (ACRs), hover over hello ACR that you wish toodelete.</span></span>
3. <span data-ttu-id="f38e5-148">En borttagningsikonen (**x**) visas i hello extrema högra kolumnen för hello ACR som du väljer.</span><span class="sxs-lookup"><span data-stu-id="f38e5-148">A delete icon (**x**) will appear in hello extreme right column for hello ACR that you select.</span></span> <span data-ttu-id="f38e5-149">Klicka på hello **x** ikonen toodelete hello ACR.</span><span class="sxs-lookup"><span data-stu-id="f38e5-149">Click hello **x** icon toodelete hello ACR.</span></span>
4. <span data-ttu-id="f38e5-150">När du uppmanas att bekräfta, klickar du på **Ja** toocontinue med hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="f38e5-150">When prompted for confirmation, click **YES** toocontinue with hello deletion.</span></span> <span data-ttu-id="f38e5-151">hello tabular lista kommer att vara uppdaterade tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="f38e5-151">hello tabular listing will be updated tooreflect hello deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f38e5-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f38e5-152">Next steps</span></span>
* <span data-ttu-id="f38e5-153">Lär dig mer om [hantera StorSimple-volymer](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="f38e5-153">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="f38e5-154">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f38e5-154">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

