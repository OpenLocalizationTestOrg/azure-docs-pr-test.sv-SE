---
title: "aaaManage DNS registrera uppsättningar och poster med Azure DNS | Microsoft Docs"
description: "Azure DNS ger hello kapaciteten toomanage DNS-post anger och registrerar när värd för din domän."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 2e62d017341589eaf8d1f8df2fe5db4b973381d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a><span data-ttu-id="e6f22-103">Hantera DNS-poster och postuppsättningar med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e6f22-103">Manage DNS records and record sets by using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6f22-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6f22-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="e6f22-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e6f22-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="e6f22-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e6f22-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="e6f22-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6f22-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="e6f22-108">Den här artikeln visar hur toomanage postuppsättningar och poster för DNS-zonen med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6f22-108">This article shows you how toomanage record sets and records for your DNS zone by using hello Azure portal.</span></span>

<span data-ttu-id="e6f22-109">Det är viktigt toounderstand hello skillnaden mellan DNS-postuppsättningar och enskilda DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="e6f22-109">It's important toounderstand hello difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="e6f22-110">En postuppsättning är en uppsättning poster i en zon som har hello samma namn och är hello samma skriver.</span><span class="sxs-lookup"><span data-stu-id="e6f22-110">A record set is a collection of records in a zone that have hello same name and are hello same type.</span></span> <span data-ttu-id="e6f22-111">Mer information finns i [skapa DNS-postuppsättningar och poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e6f22-111">For more information, see [Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="e6f22-112">Skapa en ny postuppsättning och registrera</span><span class="sxs-lookup"><span data-stu-id="e6f22-112">Create a new record set and record</span></span>

<span data-ttu-id="e6f22-113">toocreate en postuppsättning i hello Azure-portalen finns [skapa DNS-poster med hjälp av hello Azure-portalen](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e6f22-113">toocreate a record set in hello Azure portal, see [Create DNS records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="e6f22-114">Visa en postuppsättning</span><span class="sxs-lookup"><span data-stu-id="e6f22-114">View a record set</span></span>

1. <span data-ttu-id="e6f22-115">I hello Azure-portalen, går toohello **DNS-zonen** bladet.</span><span class="sxs-lookup"><span data-stu-id="e6f22-115">In hello Azure portal, go toohello **DNS zone** blade.</span></span>
2. <span data-ttu-id="e6f22-116">Sök efter hello postuppsättning och markera den.</span><span class="sxs-lookup"><span data-stu-id="e6f22-116">Search for hello record set and select it.</span></span> <span data-ttu-id="e6f22-117">Hello postuppsättning egenskaper öppnas.</span><span class="sxs-lookup"><span data-stu-id="e6f22-117">This opens hello record set properties.</span></span>

    ![Sök efter en postuppsättning](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a><span data-ttu-id="e6f22-119">Lägg till en ny post tooa postuppsättning</span><span class="sxs-lookup"><span data-stu-id="e6f22-119">Add a new record tooa record set</span></span>

<span data-ttu-id="e6f22-120">Du kan lägga upp too20 poster tooany postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="e6f22-120">You can add up too20 records tooany record set.</span></span> <span data-ttu-id="e6f22-121">En postuppsättning får inte innehålla två identiska poster.</span><span class="sxs-lookup"><span data-stu-id="e6f22-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="e6f22-122">Tom postuppsättningar (med noll poster) kan skapas, men visas inte i hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="e6f22-122">Empty record sets (with zero records) can be created, but do not appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="e6f22-123">Postuppsättningar av typen CNAME kan som mest innehålla en post.</span><span class="sxs-lookup"><span data-stu-id="e6f22-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="e6f22-124">På hello **postuppsättning egenskaper** bladet för din DNS-zonen på hello post anges som du vill tooadd en post.</span><span class="sxs-lookup"><span data-stu-id="e6f22-124">On hello **Record set properties** blade for your DNS zone, click hello record set that you want tooadd a record to.</span></span>

    ![Välj en uppsättning poster](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="e6f22-126">Ange hello postuppsättning egenskaper i hello fält.</span><span class="sxs-lookup"><span data-stu-id="e6f22-126">Specify hello record set properties by filling in hello fields.</span></span>

    ![Lägga till en post](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="e6f22-128">Klicka på **spara** på hello överkant hello bladet toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="e6f22-128">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="e6f22-129">Stäng sedan hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="e6f22-129">Then close hello blade.</span></span>
4. <span data-ttu-id="e6f22-130">I hello hörnet ser du att hello post sparas.</span><span class="sxs-lookup"><span data-stu-id="e6f22-130">In hello corner, you will see that hello record is saving.</span></span>

    ![Spara uppsättningen av poster](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="e6f22-132">När hello posten har sparats, hello värden på hello **DNS-zonen** bladet visar hello nya posten.</span><span class="sxs-lookup"><span data-stu-id="e6f22-132">After hello record has been saved, hello values on hello **DNS zone** blade will reflect hello new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="e6f22-133">Uppdatera en post</span><span class="sxs-lookup"><span data-stu-id="e6f22-133">Update a record</span></span>

<span data-ttu-id="e6f22-134">När du uppdaterar en post i en befintlig postuppsättning beroende hello fält som du kan uppdatera hello typ av post du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e6f22-134">When you update a record in an existing record set, hello fields you can update depend on hello type of record you're working with.</span></span>

1. <span data-ttu-id="e6f22-135">På hello **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter hello-post.</span><span class="sxs-lookup"><span data-stu-id="e6f22-135">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="e6f22-136">Ändra hello posten.</span><span class="sxs-lookup"><span data-stu-id="e6f22-136">Modify hello record.</span></span> <span data-ttu-id="e6f22-137">När du ändrar en post kan ändra du hello tillgängliga inställningar för hello-post.</span><span class="sxs-lookup"><span data-stu-id="e6f22-137">When you modify a record, you can change hello available settings for hello record.</span></span> <span data-ttu-id="e6f22-138">I följande exempel hello, hello **IP-adress** är markerat och hello IP-adress är i hello processen att ändras.</span><span class="sxs-lookup"><span data-stu-id="e6f22-138">In hello following example, hello **IP address** field is selected, and hello IP address is in hello process of being modified.</span></span>

    ![Ändra en post](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="e6f22-140">Klicka på **spara** på hello överkant hello bladet toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="e6f22-140">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="e6f22-141">I hello övre högra hörnet ser du hello-meddelande som hello posten har sparats.</span><span class="sxs-lookup"><span data-stu-id="e6f22-141">In hello upper right corner, you'll see hello notification that hello record has been saved.</span></span>

    ![Uppsättningen av poster](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="e6f22-143">När hello posten har sparats, hello värden för hello post ange på hello **DNS-zonen** bladet visar hello uppdatera posten.</span><span class="sxs-lookup"><span data-stu-id="e6f22-143">After hello record has been saved, hello values for hello record set on hello **DNS zone** blade will reflect hello updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="e6f22-144">Ta bort en post från en postuppsättning</span><span class="sxs-lookup"><span data-stu-id="e6f22-144">Remove a record from a record set</span></span>

<span data-ttu-id="e6f22-145">Du kan använda hello Azure portal tooremove poster från en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="e6f22-145">You can use hello Azure portal tooremove records from a record set.</span></span> <span data-ttu-id="e6f22-146">Observera att hello sista posten från en postuppsättning inte bort hello uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="e6f22-146">Note that removing hello last record from a record set does not delete hello record set.</span></span>

1. <span data-ttu-id="e6f22-147">På hello **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter hello-post.</span><span class="sxs-lookup"><span data-stu-id="e6f22-147">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="e6f22-148">Klicka på hello-post som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="e6f22-148">Click hello record that you want tooremove.</span></span> <span data-ttu-id="e6f22-149">Välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e6f22-149">Then select **Remove**.</span></span>

    ![Ta bort en post](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="e6f22-151">Klicka på **spara** på hello överkant hello bladet toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="e6f22-151">Click **Save** at hello top of hello blade toosave your settings.</span></span>
4. <span data-ttu-id="e6f22-152">När hello har tagits bort, hello värden för hello-post på hello **DNS-zonen** bladet visar hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="e6f22-152">After hello record has been removed, hello values for hello record on hello **DNS zone** blade will reflect hello removal.</span></span>

## <span data-ttu-id="e6f22-153"><a name="delete"></a>Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="e6f22-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="e6f22-154">På hello **postuppsättning egenskaper** bladet för postuppsättningen, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e6f22-154">On hello **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![Ta bort en uppsättning poster](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="e6f22-156">Ett meddelande visas som frågar om du vill toodelete hello postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="e6f22-156">A message appears asking if you want toodelete hello record set.</span></span>
3. <span data-ttu-id="e6f22-157">Kontrollera att hello namn matchar hello post du vill toodelete och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="e6f22-157">Verify that hello name matches hello record set that you want toodelete, and then click **Yes**.</span></span>
4. <span data-ttu-id="e6f22-158">På hello **DNS-zonen** bladet Kontrollera hello postuppsättning inte längre visas.</span><span class="sxs-lookup"><span data-stu-id="e6f22-158">On hello **DNS zone** blade, verify that hello record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="e6f22-159">Arbeta med NS och SOA-poster</span><span class="sxs-lookup"><span data-stu-id="e6f22-159">Work with NS and SOA records</span></span>

<span data-ttu-id="e6f22-160">NS och SOA-poster som skapas automatiskt hanteras annorlunda från andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="e6f22-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="e6f22-161">Ändra SOA-poster</span><span class="sxs-lookup"><span data-stu-id="e6f22-161">Modify SOA records</span></span>

<span data-ttu-id="e6f22-162">Du kan inte lägga till eller ta bort poster från hello skapas automatiskt SOA-postuppsättning på zonens apex hello (namn = ”@”).</span><span class="sxs-lookup"><span data-stu-id="e6f22-162">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (name = "@").</span></span> <span data-ttu-id="e6f22-163">Men du kan ändra någon av parametrarna hello inom hello SOA-post (utom ”värd”) och hello postuppsättning TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="e6f22-163">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

### <a name="modify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="e6f22-164">Ändra NS-poster på hello zonens apex</span><span class="sxs-lookup"><span data-stu-id="e6f22-164">Modify NS records at hello zone apex</span></span>

<span data-ttu-id="e6f22-165">hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="e6f22-165">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="e6f22-166">Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.</span><span class="sxs-lookup"><span data-stu-id="e6f22-166">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="e6f22-167">Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör.</span><span class="sxs-lookup"><span data-stu-id="e6f22-167">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="e6f22-168">Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="e6f22-168">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="e6f22-169">Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="e6f22-169">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="e6f22-170">Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello.</span><span class="sxs-lookup"><span data-stu-id="e6f22-170">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="e6f22-171">Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="e6f22-171">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="e6f22-172">Ta bort SOA- eller NS postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="e6f22-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="e6f22-173">Du kan inte ta bort hello SOA- och NS postuppsättningar vid hello zonens apex (namn = ”@”) som skapas automatiskt när hello zonen skapas.</span><span class="sxs-lookup"><span data-stu-id="e6f22-173">You cannot delete hello SOA and NS record sets at hello zone apex (name = "@") that are created automatically when hello zone is created.</span></span> <span data-ttu-id="e6f22-174">De tas bort automatiskt när du tar bort hello zonen.</span><span class="sxs-lookup"><span data-stu-id="e6f22-174">They are deleted automatically when you delete hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6f22-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6f22-175">Next steps</span></span>

* <span data-ttu-id="e6f22-176">Mer information om Azure DNS finns hello [översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6f22-176">For more information about Azure DNS, see hello [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="e6f22-177">Mer information om hur du automatiserar DNS finns [skapar DNS-zoner och postuppsättningar med hello .NET SDK](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="e6f22-177">For more information about automating DNS, see [Creating DNS zones and record sets using hello .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="e6f22-178">Mer information om omvänd DNS-poster finns [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6f22-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
