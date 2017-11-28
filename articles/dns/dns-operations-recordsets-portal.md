---
title: "Hantera uppsättningar av DNS-poster och poster med Azure DNS | Microsoft Docs"
description: "Azure DNS ger möjlighet att hantera uppsättningar av DNS-poster och poster om värd för din domän."
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
ms.openlocfilehash: 001b80ccba43beab44f6a598f820df65a85a345f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a><span data-ttu-id="fe0a6-103">Hantera DNS-poster och postuppsättningar med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="fe0a6-103">Manage DNS records and record sets by using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fe0a6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fe0a6-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="fe0a6-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fe0a6-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="fe0a6-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fe0a6-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="fe0a6-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe0a6-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="fe0a6-108">Den här artikeln visar hur du hanterar postuppsättningar och posterna för DNS-zonen med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-108">This article shows you how to manage record sets and records for your DNS zone by using the Azure portal.</span></span>

<span data-ttu-id="fe0a6-109">Det är viktigt att förstå skillnaden mellan DNS-postuppsättningar och enskilda DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-109">It's important to understand the difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="fe0a6-110">En postuppsättning är en uppsättning poster i en zon som har samma namn och är av samma typ.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-110">A record set is a collection of records in a zone that have the same name and are the same type.</span></span> <span data-ttu-id="fe0a6-111">Mer information finns i [skapa DNS-postuppsättningar och poster med hjälp av Azure portal](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-111">For more information, see [Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="fe0a6-112">Skapa en ny postuppsättning och registrera</span><span class="sxs-lookup"><span data-stu-id="fe0a6-112">Create a new record set and record</span></span>

<span data-ttu-id="fe0a6-113">Om du vill skapa en postuppsättning i Azure portal finns [skapa DNS-poster med hjälp av Azure portal](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-113">To create a record set in the Azure portal, see [Create DNS records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="fe0a6-114">Visa en postuppsättning</span><span class="sxs-lookup"><span data-stu-id="fe0a6-114">View a record set</span></span>

1. <span data-ttu-id="fe0a6-115">I Azure-portalen går du till den **DNS-zonen** bladet.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-115">In the Azure portal, go to the **DNS zone** blade.</span></span>
2. <span data-ttu-id="fe0a6-116">Sök efter uppsättningen av poster och markera den.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-116">Search for the record set and select it.</span></span> <span data-ttu-id="fe0a6-117">Detta öppnar egenskaperna för uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-117">This opens the record set properties.</span></span>

    ![Sök efter en postuppsättning](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a><span data-ttu-id="fe0a6-119">Lägg till en ny post i en postuppsättning</span><span class="sxs-lookup"><span data-stu-id="fe0a6-119">Add a new record to a record set</span></span>

<span data-ttu-id="fe0a6-120">Du kan lägga till upp till 20 poster en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-120">You can add up to 20 records to any record set.</span></span> <span data-ttu-id="fe0a6-121">En postuppsättning får inte innehålla två identiska poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="fe0a6-122">Tom postuppsättningar (med noll poster) kan skapas, men visas inte i Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-122">Empty record sets (with zero records) can be created, but do not appear on the Azure DNS name servers.</span></span> <span data-ttu-id="fe0a6-123">Postuppsättningar av typen CNAME kan som mest innehålla en post.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="fe0a6-124">På den **postuppsättning egenskaper** bladet för din DNS-zonen, klickar du på postuppsättningen som du vill lägga till en post.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-124">On the **Record set properties** blade for your DNS zone, click the record set that you want to add a record to.</span></span>

    ![Välj en uppsättning poster](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="fe0a6-126">Ange egenskaper för postuppsättning genom att fylla i fälten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-126">Specify the record set properties by filling in the fields.</span></span>

    ![Lägga till en post](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="fe0a6-128">Klicka på **spara** längst upp på bladet för att spara dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-128">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="fe0a6-129">Stäng bladet.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-129">Then close the blade.</span></span>
4. <span data-ttu-id="fe0a6-130">I hörnet ser du att posten sparas.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-130">In the corner, you will see that the record is saving.</span></span>

    ![Spara uppsättningen av poster](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="fe0a6-132">När posten har sparats, värdena på den **DNS-zonen** bladet visar den nya posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-132">After the record has been saved, the values on the **DNS zone** blade will reflect the new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="fe0a6-133">Uppdatera en post</span><span class="sxs-lookup"><span data-stu-id="fe0a6-133">Update a record</span></span>

<span data-ttu-id="fe0a6-134">När du uppdaterar en post i en befintlig postuppsättning beror fälten som du kan uppdatera på vilken typ av post du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-134">When you update a record in an existing record set, the fields you can update depend on the type of record you're working with.</span></span>

1. <span data-ttu-id="fe0a6-135">På den **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-135">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="fe0a6-136">Ändra posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-136">Modify the record.</span></span> <span data-ttu-id="fe0a6-137">När du ändrar en post kan du ändra tillgängliga inställningar för posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-137">When you modify a record, you can change the available settings for the record.</span></span> <span data-ttu-id="fe0a6-138">I följande exempel visas den **IP-adress** är markerat och IP-adressen håller på att ändras.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-138">In the following example, the **IP address** field is selected, and the IP address is in the process of being modified.</span></span>

    ![Ändra en post](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="fe0a6-140">Klicka på **spara** längst upp på bladet för att spara dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-140">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="fe0a6-141">I det övre högra hörnet ser du meddelandet som posten har sparats.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-141">In the upper right corner, you'll see the notification that the record has been saved.</span></span>

    ![Uppsättningen av poster](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="fe0a6-143">När posten har sparats, ange värden för posten för den **DNS-zonen** bladet visar den uppdaterade posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-143">After the record has been saved, the values for the record set on the **DNS zone** blade will reflect the updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="fe0a6-144">Ta bort en post från en postuppsättning</span><span class="sxs-lookup"><span data-stu-id="fe0a6-144">Remove a record from a record set</span></span>

<span data-ttu-id="fe0a6-145">Du kan använda Azure-portalen för att ta bort poster från en postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-145">You can use the Azure portal to remove records from a record set.</span></span> <span data-ttu-id="fe0a6-146">Observera att den sista posten från en postuppsättning inte bort uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-146">Note that removing the last record from a record set does not delete the record set.</span></span>

1. <span data-ttu-id="fe0a6-147">På den **postuppsättning egenskaper** bladet för postuppsättningen, Sök efter posten.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-147">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="fe0a6-148">Klicka på posten som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-148">Click the record that you want to remove.</span></span> <span data-ttu-id="fe0a6-149">Välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-149">Then select **Remove**.</span></span>

    ![Ta bort en post](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="fe0a6-151">Klicka på **spara** längst upp på bladet för att spara dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-151">Click **Save** at the top of the blade to save your settings.</span></span>
4. <span data-ttu-id="fe0a6-152">När den har tagits bort, värden för posten på den **DNS-zonen** bladet visar borttagningen.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-152">After the record has been removed, the values for the record on the **DNS zone** blade will reflect the removal.</span></span>

## <span data-ttu-id="fe0a6-153"><a name="delete"></a>Ta bort en uppsättning poster</span><span class="sxs-lookup"><span data-stu-id="fe0a6-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="fe0a6-154">På den **postuppsättning egenskaper** bladet för postuppsättningen, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-154">On the **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![Ta bort en uppsättning poster](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="fe0a6-156">Ett meddelande visas som frågar om du vill ta bort uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-156">A message appears asking if you want to delete the record set.</span></span>
3. <span data-ttu-id="fe0a6-157">Kontrollera att namnet matchar uppsättningen av poster som du vill ta bort och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-157">Verify that the name matches the record set that you want to delete, and then click **Yes**.</span></span>
4. <span data-ttu-id="fe0a6-158">På den **DNS-zonen** bladet, kontrollera att uppsättningen av poster visas inte längre.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-158">On the **DNS zone** blade, verify that the record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="fe0a6-159">Arbeta med NS och SOA-poster</span><span class="sxs-lookup"><span data-stu-id="fe0a6-159">Work with NS and SOA records</span></span>

<span data-ttu-id="fe0a6-160">NS och SOA-poster som skapas automatiskt hanteras annorlunda från andra typer av poster.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="fe0a6-161">Ändra SOA-poster</span><span class="sxs-lookup"><span data-stu-id="fe0a6-161">Modify SOA records</span></span>

<span data-ttu-id="fe0a6-162">Du kan inte lägga till eller ta bort poster från den automatiskt skapade SOA-postuppsättning på zonens apex (namn = ”@”).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-162">You cannot add or remove records from the automatically created SOA record set at the zone apex (name = "@").</span></span> <span data-ttu-id="fe0a6-163">Men du kan ändra någon av parametrarna i SOA-post (utom ”värd”) och postuppsättning TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-163">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

### <a name="modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="fe0a6-164">Ändra NS-poster på zonens apex</span><span class="sxs-lookup"><span data-stu-id="fe0a6-164">Modify NS records at the zone apex</span></span>

<span data-ttu-id="fe0a6-165">NS-postuppsättning på zonens apex skapas automatiskt med varje DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-165">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="fe0a6-166">Den innehåller namnen på Azure DNS-namnservrar tilldelas zonen.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-166">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="fe0a6-167">Du kan lägga till ytterligare servrar till den här NS-post inställda på en värd domäner med mer än en DNS-leverantör har stöd ett namn.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-167">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="fe0a6-168">Du kan också ändra TTL-värde och metadata för den här postuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-168">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="fe0a6-169">Du kan inte ta bort eller ändra de i förväg Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-169">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="fe0a6-170">Observera att detta gäller endast NS-postuppsättning på zonens apex.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-170">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="fe0a6-171">Andra NS postuppsättningar i zonen (som används för att delegera underordnade zoner) kan ändras utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-171">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="fe0a6-172">Ta bort SOA- eller NS postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="fe0a6-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="fe0a6-173">Du kan inte ta bort SOA och NS-post anger på zonens apex (namn = ”@”) som skapas automatiskt när zonen skapas.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-173">You cannot delete the SOA and NS record sets at the zone apex (name = "@") that are created automatically when the zone is created.</span></span> <span data-ttu-id="fe0a6-174">De tas bort automatiskt när du tar bort zonen.</span><span class="sxs-lookup"><span data-stu-id="fe0a6-174">They are deleted automatically when you delete the zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe0a6-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe0a6-175">Next steps</span></span>

* <span data-ttu-id="fe0a6-176">Mer information om Azure DNS finns i [översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-176">For more information about Azure DNS, see the [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="fe0a6-177">Mer information om hur du automatiserar DNS finns [skapar DNS-zoner och postuppsättningar med .NET SDK](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-177">For more information about automating DNS, see [Creating DNS zones and record sets using the .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="fe0a6-178">Mer information om omvänd DNS-poster finns [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe0a6-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
