---
title: "aaaGet igång med Azure DNS med hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera dina första DNS-zonen och posten med hello Azure-portalen."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 5cea01d840d794001cccac64defed8b329d948db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-hello-azure-portal"></a><span data-ttu-id="50407-104">Kom igång med Azure DNS med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="50407-104">Get started with Azure DNS using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="50407-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50407-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="50407-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50407-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="50407-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50407-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="50407-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50407-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="50407-109">Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zonen och posten med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50407-109">This article walks you through hello steps toocreate your first DNS zone and record using hello Azure portal.</span></span> <span data-ttu-id="50407-110">Du kan också göra detta med hjälp av Azure PowerShell eller hello plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="50407-110">You can also perform these steps using Azure PowerShell or hello cross-platform Azure CLI.</span></span>

<span data-ttu-id="50407-111">En DNS-zon är används toohost hello DNS-poster för en viss domän.</span><span class="sxs-lookup"><span data-stu-id="50407-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="50407-112">toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet.</span><span class="sxs-lookup"><span data-stu-id="50407-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="50407-113">Varje DNS-post för din domän skapas sedan i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="50407-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="50407-114">Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="50407-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="50407-115">Var och en av dessa steg beskrivs i följande hello.</span><span class="sxs-lookup"><span data-stu-id="50407-115">Each of these steps is described in hello following steps.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="50407-116">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="50407-116">Create a DNS zone</span></span>

1. <span data-ttu-id="50407-117">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="50407-117">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="50407-118">Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="50407-118">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-getstarted-portal/openzone650.png)

4. <span data-ttu-id="50407-120">På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="50407-120">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>


   | <span data-ttu-id="50407-121">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="50407-121">**Setting**</span></span> | <span data-ttu-id="50407-122">**Värde**</span><span class="sxs-lookup"><span data-stu-id="50407-122">**Value**</span></span> | <span data-ttu-id="50407-123">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="50407-123">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="50407-124">**Namn**</span><span class="sxs-lookup"><span data-stu-id="50407-124">**Name**</span></span>|<span data-ttu-id="50407-125">contoso.com</span><span class="sxs-lookup"><span data-stu-id="50407-125">contoso.com</span></span>|<span data-ttu-id="50407-126">hello namnet på hello DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="50407-126">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="50407-127">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="50407-127">**Subscription**</span></span>|<span data-ttu-id="50407-128">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="50407-128">[Your subscription]</span></span>|<span data-ttu-id="50407-129">Välj en prenumeration toocreate hello DNS-zonen i.</span><span class="sxs-lookup"><span data-stu-id="50407-129">Select a subscription toocreate hello DNS zone in.</span></span>|
   |<span data-ttu-id="50407-130">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="50407-130">**Resource group**</span></span>|<span data-ttu-id="50407-131">**Skapa ny:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="50407-131">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="50407-132">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="50407-132">Create a resource group.</span></span> <span data-ttu-id="50407-133">hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt.</span><span class="sxs-lookup"><span data-stu-id="50407-133">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="50407-134">Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="50407-134">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="50407-135">**Plats**</span><span class="sxs-lookup"><span data-stu-id="50407-135">**Location**</span></span>|<span data-ttu-id="50407-136">Västra USA</span><span class="sxs-lookup"><span data-stu-id="50407-136">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="50407-137">hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="50407-137">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="50407-138">hello DNS-zonen plats är alltid ”globala” och visas inte.</span><span class="sxs-lookup"><span data-stu-id="50407-138">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="50407-139">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="50407-139">Create a DNS record</span></span>

<span data-ttu-id="50407-140">hello vägleder följande exempel dig genom hello processen att skapa nya ”A” post.</span><span class="sxs-lookup"><span data-stu-id="50407-140">hello following example walks you through hello process of creating new 'A' record.</span></span> <span data-ttu-id="50407-141">Andra typer av poster och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="50407-141">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span> 

1. <span data-ttu-id="50407-142">Med hello skapade DNS-zonen i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="50407-142">With hello DNS Zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="50407-143">Klicka på hello **contoso.com** DNS-zonen i hello bladet för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="50407-143">Click hello **contoso.com** DNS zone in hello All resources blade.</span></span> <span data-ttu-id="50407-144">Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contoso.com** i hello **filtrera efter namn...**</span><span class="sxs-lookup"><span data-stu-id="50407-144">If hello subscription you selected already has several resources in it, you can enter **contoso.com** in hello **Filter by name…**</span></span> <span data-ttu-id="50407-145">rutan tooeasily åtkomst hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="50407-145">box tooeasily access hello DNS Zone.</span></span>

1. <span data-ttu-id="50407-146">Hello överst i hello **DNS-zonen** bladet väljer **+ postuppsättningen** tooopen hello **lägga till postuppsättning** bladet.</span><span class="sxs-lookup"><span data-stu-id="50407-146">At hello top of hello **DNS zone** blade, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

1. <span data-ttu-id="50407-147">På hello **lägga till postuppsättning** bladet anger hello följande värden och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="50407-147">On hello **Add record set** blade, enter hello following values, and click **OK**.</span></span> <span data-ttu-id="50407-148">I det här exemplet skapar du en A-post.</span><span class="sxs-lookup"><span data-stu-id="50407-148">In this example, you are creating an A record.</span></span>

   |<span data-ttu-id="50407-149">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="50407-149">**Setting**</span></span> | <span data-ttu-id="50407-150">**Värde**</span><span class="sxs-lookup"><span data-stu-id="50407-150">**Value**</span></span> | <span data-ttu-id="50407-151">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="50407-151">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="50407-152">**Namn**</span><span class="sxs-lookup"><span data-stu-id="50407-152">**Name**</span></span>|<span data-ttu-id="50407-153">www</span><span class="sxs-lookup"><span data-stu-id="50407-153">www</span></span>|<span data-ttu-id="50407-154">Namnet på hello-post</span><span class="sxs-lookup"><span data-stu-id="50407-154">Name of hello record</span></span>|
   |<span data-ttu-id="50407-155">**Typ**</span><span class="sxs-lookup"><span data-stu-id="50407-155">**Type**</span></span>|<span data-ttu-id="50407-156">A</span><span class="sxs-lookup"><span data-stu-id="50407-156">A</span></span>| <span data-ttu-id="50407-157">Typ av DNS-poster toocreate, giltiga värden är A, AAAA, CNAME, MX, NS, SRV, TXT och PTR.</span><span class="sxs-lookup"><span data-stu-id="50407-157">Type of DNS record toocreate, acceptable values are A, AAAA, CNAME, MX, NS, SRV, TXT, and PTR.</span></span>  <span data-ttu-id="50407-158">Mer information om posttyper finns i [Översikt över DNS-zoner och poster](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="50407-158">For more information about record types, visit [Overview of DNS zones and records](dns-zones-records.md)</span></span>|
   |<span data-ttu-id="50407-159">**TTL**</span><span class="sxs-lookup"><span data-stu-id="50407-159">**TTL**</span></span>|<span data-ttu-id="50407-160">1</span><span class="sxs-lookup"><span data-stu-id="50407-160">1</span></span>|<span data-ttu-id="50407-161">Time-to-live för hello DNS-begäran.</span><span class="sxs-lookup"><span data-stu-id="50407-161">Time-to-live of hello DNS request.</span></span>|
   |<span data-ttu-id="50407-162">**TTL-enhet**</span><span class="sxs-lookup"><span data-stu-id="50407-162">**TTL unit**</span></span>|<span data-ttu-id="50407-163">Timmar</span><span class="sxs-lookup"><span data-stu-id="50407-163">Hours</span></span>|<span data-ttu-id="50407-164">Tidsmått för TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="50407-164">Measurement of time for TTL value.</span></span>|
   |<span data-ttu-id="50407-165">**IP-adress**</span><span class="sxs-lookup"><span data-stu-id="50407-165">**IP address**</span></span>|<span data-ttu-id="50407-166">ipAddressValue</span><span class="sxs-lookup"><span data-stu-id="50407-166">ipAddressValue</span></span>| <span data-ttu-id="50407-167">Det här värdet är hello IP-adress som löser hello DNS-post.</span><span class="sxs-lookup"><span data-stu-id="50407-167">This value is hello IP address that hello DNS record resolves.</span></span>|

## <a name="view-records"></a><span data-ttu-id="50407-168">Visa poster</span><span class="sxs-lookup"><span data-stu-id="50407-168">View records</span></span>

<span data-ttu-id="50407-169">I hello längst ned på bladet för hello DNS-zonen ser du hello poster för hello DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="50407-169">In hello lower part of hello DNS zone blade, you can see hello records for hello DNS zone.</span></span> <span data-ttu-id="50407-170">Du bör se hello standard DNS- och SOA-poster, som skapas i varje zon, plus eventuella nya poster som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="50407-170">You should see hello default DNS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

![zon](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a><span data-ttu-id="50407-172">Uppdatera namnservrar</span><span class="sxs-lookup"><span data-stu-id="50407-172">Update name servers</span></span>

<span data-ttu-id="50407-173">När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="50407-173">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="50407-174">Detta gör att andra användare i hello Internet toofind DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="50407-174">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="50407-175">hello namnservrar för zonen anges i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="50407-175">hello name servers for your zone are given in hello Azure portal:</span></span>

![zon](./media/dns-getstarted-portal/viewzonens500.png)

<span data-ttu-id="50407-177">Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn).</span><span class="sxs-lookup"><span data-stu-id="50407-177">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="50407-178">Din registrator erbjuder hello alternativet tooset in hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="50407-178">Your registrar offers hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="50407-179">Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="50407-179">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="50407-180">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="50407-180">Delete all resources</span></span>

<span data-ttu-id="50407-181">toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50407-181">toodelete all resources created in this article, complete hello following steps:</span></span>

1. <span data-ttu-id="50407-182">I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="50407-182">In hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="50407-183">Klicka på hello **MyResourceGroup** resursgrupp i hello bladet för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="50407-183">Click hello **MyResourceGroup** resource group in hello All resources blade.</span></span> <span data-ttu-id="50407-184">Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **MyResourceGroup** i hello **filtrera efter namn...**</span><span class="sxs-lookup"><span data-stu-id="50407-184">If hello subscription you selected already has several resources in it, you can enter **MyResourceGroup** in hello **Filter by name…**</span></span> <span data-ttu-id="50407-185">rutan tooeasily åtkomst hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="50407-185">box tooeasily access hello resource group.</span></span>
1. <span data-ttu-id="50407-186">I hello **MyResourceGroup** bladet, klickar du på hello **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="50407-186">In hello **MyResourceGroup** blade, click hello **Delete** button.</span></span>
1. <span data-ttu-id="50407-187">hello portalen måste du tootype hello namnet på hello resurs grupp tooconfirm som du vill toodelete den.</span><span class="sxs-lookup"><span data-stu-id="50407-187">hello portal requires you tootype hello name of hello resource group tooconfirm that you want toodelete it.</span></span> <span data-ttu-id="50407-188">Klicka på **ta bort**, typen *MyResourceGroup* hello resursgruppens namn, sedan klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="50407-188">Click **Delete**, Type *MyResourceGroup* for hello resource group name, then click **Delete**.</span></span> <span data-ttu-id="50407-189">Tar bort en resursgrupp alla resurser inom hello resursgrupp, så alltid att tooconfirm hello innehållet i en resursgrupp innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="50407-189">Deleting a resource group deletes all resources within hello resource group, so always be sure tooconfirm hello contents of a resource group before deleting it.</span></span> <span data-ttu-id="50407-190">hello portal tar bort alla resurser som ingår i hello resursgrupp och sedan tar bort hello resursgruppen sig själv.</span><span class="sxs-lookup"><span data-stu-id="50407-190">hello portal deletes all resources contained within hello resource group, then deletes hello resource group itself.</span></span> <span data-ttu-id="50407-191">Den här processen tar flera minuter.</span><span class="sxs-lookup"><span data-stu-id="50407-191">This process takes several minutes.</span></span>


## <a name="next-steps"></a><span data-ttu-id="50407-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50407-192">Next steps</span></span>

<span data-ttu-id="50407-193">toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="50407-193">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="50407-194">toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och postuppsättningar med hjälp av hello Azure-portalen](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="50407-194">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

