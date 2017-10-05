---
title: "Komma igång med Azure DNS med hjälp av Azure Portal | Microsoft Docs"
description: "Läs om hur du skapar en DNS-zon och en DNS-post i Azure DNS. Detta är en steg-för-steg-guide om hur du skapar och hanterar din första DNS-zon och DNS-post på Azure Portal."
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
ms.openlocfilehash: 93b24e3d9fbb3fbb3ea995271fd63d1e82eb9c9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-the-azure-portal"></a><span data-ttu-id="ff4d1-104">Komma igång med Azure DNS med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ff4d1-104">Get started with Azure DNS using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ff4d1-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ff4d1-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="ff4d1-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ff4d1-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="ff4d1-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ff4d1-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="ff4d1-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ff4d1-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="ff4d1-109">Den här artikeln visar hur du skapa din första DNS-zon och DNS-post med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-109">This article walks you through the steps to create your first DNS zone and record using the Azure portal.</span></span> <span data-ttu-id="ff4d1-110">Du kan också utföra dessa steg med Azure PowerShell eller plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-110">You can also perform these steps using Azure PowerShell or the cross-platform Azure CLI.</span></span>

<span data-ttu-id="ff4d1-111">En DNS-zon används som värd åt DNS-posterna för en viss domän.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="ff4d1-112">Om du vill låta Azure DNS vara värd för din domän så måste du skapa en DNS-zon för det domännamnet.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="ff4d1-113">Varje DNS-post för din domän skapas sedan i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="ff4d1-114">Om du vill publicera din DNS-zon på Internet måste du konfigurera namnservrarna för domänen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="ff4d1-115">Vart och ett av dessa steg beskrivs i följande steg.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-115">Each of these steps is described in the following steps.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="ff4d1-116">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="ff4d1-116">Create a DNS zone</span></span>

1. <span data-ttu-id="ff4d1-117">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ff4d1-117">Sign in to the Azure portal</span></span>
2. <span data-ttu-id="ff4d1-118">Klicka på **Nytt > Nätverk >** på navmenyn och klicka sedan på **DNS-zon** för att öppna bladet Skapa DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-118">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-getstarted-portal/openzone650.png)

4. <span data-ttu-id="ff4d1-120">På bladet **Skapa DNS-zon** anger du följande värden och klickar sedan på **Skapa**:</span><span class="sxs-lookup"><span data-stu-id="ff4d1-120">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>


   | <span data-ttu-id="ff4d1-121">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-121">**Setting**</span></span> | <span data-ttu-id="ff4d1-122">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-122">**Value**</span></span> | <span data-ttu-id="ff4d1-123">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-123">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="ff4d1-124">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-124">**Name**</span></span>|<span data-ttu-id="ff4d1-125">contoso.com</span><span class="sxs-lookup"><span data-stu-id="ff4d1-125">contoso.com</span></span>|<span data-ttu-id="ff4d1-126">Namnet på DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="ff4d1-126">The name of the DNS zone</span></span>|
   |<span data-ttu-id="ff4d1-127">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-127">**Subscription**</span></span>|<span data-ttu-id="ff4d1-128">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="ff4d1-128">[Your subscription]</span></span>|<span data-ttu-id="ff4d1-129">Välj en prenumeration att skapa DNS-zonen i.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-129">Select a subscription to create the DNS zone in.</span></span>|
   |<span data-ttu-id="ff4d1-130">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-130">**Resource group**</span></span>|<span data-ttu-id="ff4d1-131">**Skapa ny:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="ff4d1-131">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="ff4d1-132">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-132">Create a resource group.</span></span> <span data-ttu-id="ff4d1-133">Resursgruppens namn måste vara unikt inom den prenumeration du valde.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-133">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="ff4d1-134">Mer information om resursgrupper finns i [översikten över Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-134">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="ff4d1-135">**Plats**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-135">**Location**</span></span>|<span data-ttu-id="ff4d1-136">Västra USA</span><span class="sxs-lookup"><span data-stu-id="ff4d1-136">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="ff4d1-137">Resursgruppen refererar till platsen för resursgruppen och har ingen inverkan på DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-137">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="ff4d1-138">Platsen för DNS-zonen är alltid "global" och visas inte.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-138">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="ff4d1-139">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="ff4d1-139">Create a DNS record</span></span>

<span data-ttu-id="ff4d1-140">Följande exempel visar hur du skapar en ny "A"-post.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-140">The following example walks you through the process of creating new 'A' record.</span></span> <span data-ttu-id="ff4d1-141">Information om andra posttyper och hur du ändrar befintliga poster finns i [Hantera DNS-poster och postuppsättningar med Azure Portal](dns-operations-recordsets-portal.md) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-141">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span> 

1. <span data-ttu-id="ff4d1-142">I Azure Portal klickar du på **Alla resurser** i rutan **Favoriter** för den DNS-zon du skapade.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-142">With the DNS Zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="ff4d1-143">Klicka på DNS-zonen **contoso.com** på bladet Alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-143">Click the **contoso.com** DNS zone in the All resources blade.</span></span> <span data-ttu-id="ff4d1-144">Om den prenumeration du valde redan har flera resurser kan du ange **contoso.com** i rutan **Filtrera efter namn...**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-144">If the subscription you selected already has several resources in it, you can enter **contoso.com** in the **Filter by name…**</span></span> <span data-ttu-id="ff4d1-145">när du ska hitta din DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-145">box to easily access the DNS Zone.</span></span>

1. <span data-ttu-id="ff4d1-146">Välj **+ Postuppsättning** längst upp på bladet **DNS-zon** för att öppna bladet **Lägg till uppsättning av poster**.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-146">At the top of the **DNS zone** blade, select **+ Record set** to open the **Add record set** blade.</span></span>

1. <span data-ttu-id="ff4d1-147">Ange följande värden på bladet **Lägg till uppsättning av poster** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-147">On the **Add record set** blade, enter the following values, and click **OK**.</span></span> <span data-ttu-id="ff4d1-148">I det här exemplet skapar du en A-post.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-148">In this example, you are creating an A record.</span></span>

   |<span data-ttu-id="ff4d1-149">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-149">**Setting**</span></span> | <span data-ttu-id="ff4d1-150">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-150">**Value**</span></span> | <span data-ttu-id="ff4d1-151">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-151">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="ff4d1-152">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-152">**Name**</span></span>|<span data-ttu-id="ff4d1-153">www</span><span class="sxs-lookup"><span data-stu-id="ff4d1-153">www</span></span>|<span data-ttu-id="ff4d1-154">Namnet på posten</span><span class="sxs-lookup"><span data-stu-id="ff4d1-154">Name of the record</span></span>|
   |<span data-ttu-id="ff4d1-155">**Typ**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-155">**Type**</span></span>|<span data-ttu-id="ff4d1-156">A</span><span class="sxs-lookup"><span data-stu-id="ff4d1-156">A</span></span>| <span data-ttu-id="ff4d1-157">Den typ av DNS-post som du vill skapa. Godkända värden är A, AAAA, CNAME, MX, NS, SRV, TXT och PTR.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-157">Type of DNS record to create, acceptable values are A, AAAA, CNAME, MX, NS, SRV, TXT, and PTR.</span></span>  <span data-ttu-id="ff4d1-158">Mer information om posttyper finns i [Översikt över DNS-zoner och poster](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="ff4d1-158">For more information about record types, visit [Overview of DNS zones and records](dns-zones-records.md)</span></span>|
   |<span data-ttu-id="ff4d1-159">**TTL**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-159">**TTL**</span></span>|<span data-ttu-id="ff4d1-160">1</span><span class="sxs-lookup"><span data-stu-id="ff4d1-160">1</span></span>|<span data-ttu-id="ff4d1-161">Time-to-live för DNS-begäran.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-161">Time-to-live of the DNS request.</span></span>|
   |<span data-ttu-id="ff4d1-162">**TTL-enhet**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-162">**TTL unit**</span></span>|<span data-ttu-id="ff4d1-163">Timmar</span><span class="sxs-lookup"><span data-stu-id="ff4d1-163">Hours</span></span>|<span data-ttu-id="ff4d1-164">Tidsmått för TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-164">Measurement of time for TTL value.</span></span>|
   |<span data-ttu-id="ff4d1-165">**IP-adress**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-165">**IP address**</span></span>|<span data-ttu-id="ff4d1-166">ipAddressValue</span><span class="sxs-lookup"><span data-stu-id="ff4d1-166">ipAddressValue</span></span>| <span data-ttu-id="ff4d1-167">Det här värdet är den IP-adress som DNS-posten matchar till.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-167">This value is the IP address that the DNS record resolves.</span></span>|

## <a name="view-records"></a><span data-ttu-id="ff4d1-168">Visa poster</span><span class="sxs-lookup"><span data-stu-id="ff4d1-168">View records</span></span>

<span data-ttu-id="ff4d1-169">På den nedre delen av bladet DNS-zon visas posterna för DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-169">In the lower part of the DNS zone blade, you can see the records for the DNS zone.</span></span> <span data-ttu-id="ff4d1-170">Du bör se DNS- och SOA-standardposterna (dessa skapas i varje zon) plus eventuella nya poster som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-170">You should see the default DNS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

![zon](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a><span data-ttu-id="ff4d1-172">Uppdatera namnservrar</span><span class="sxs-lookup"><span data-stu-id="ff4d1-172">Update name servers</span></span>

<span data-ttu-id="ff4d1-173">När du är nöjd med konfigurationen av DNS-zonen och DNS-posterna måste du konfigurera ditt domännamn för användning med Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-173">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="ff4d1-174">Det gör att andra användare på Internet kan hitta dina DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-174">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="ff4d1-175">Namnservrarna för din zon anges på Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="ff4d1-175">The name servers for your zone are given in the Azure portal:</span></span>

![zon](./media/dns-getstarted-portal/viewzonens500.png)

<span data-ttu-id="ff4d1-177">Dessa namnservrar ska konfigureras med domännamnsregistratorn (där du köpte domännamnet).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-177">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="ff4d1-178">Registratorn erbjuder möjligheten att konfigurera namnservrar för domänen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-178">Your registrar offers the option to set up the name servers for the domain.</span></span> <span data-ttu-id="ff4d1-179">Mer information finns i [Delegera en domän till Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-179">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="ff4d1-180">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="ff4d1-180">Delete all resources</span></span>

<span data-ttu-id="ff4d1-181">Så här tar du bort alla resurser som skapats i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="ff4d1-181">To delete all resources created in this article, complete the following steps:</span></span>

1. <span data-ttu-id="ff4d1-182">Klicka på **Alla resurser** i rutan **Favoriter** i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-182">In the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="ff4d1-183">Klicka på resursgruppen **MyResourceGroup** på bladet Alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-183">Click the **MyResourceGroup** resource group in the All resources blade.</span></span> <span data-ttu-id="ff4d1-184">Om den prenumeration du valde redan har flera resurser kan du ange **MyResourceGroup** i rutan **Filtrera efter namn**</span><span class="sxs-lookup"><span data-stu-id="ff4d1-184">If the subscription you selected already has several resources in it, you can enter **MyResourceGroup** in the **Filter by name…**</span></span> <span data-ttu-id="ff4d1-185">när du ska hitta resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-185">box to easily access the resource group.</span></span>
1. <span data-ttu-id="ff4d1-186">Klicka på knappen **Ta bort** på bladet **MyResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-186">In the **MyResourceGroup** blade, click the **Delete** button.</span></span>
1. <span data-ttu-id="ff4d1-187">Du måste ange namnet på resursgruppen i portalen som bekräftelse på att du vill ta bort den.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-187">The portal requires you to type the name of the resource group to confirm that you want to delete it.</span></span> <span data-ttu-id="ff4d1-188">Klicka på **Ta bort**, skriv *MyResourceGroup* som resursgruppnamn och klicka sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-188">Click **Delete**, Type *MyResourceGroup* for the resource group name, then click **Delete**.</span></span> <span data-ttu-id="ff4d1-189">När du tar bort en resursgrupp så tas alla resurser i resursgruppen bort, så du måste alltid kontrollera innehållet i en resursgrupp innan du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-189">Deleting a resource group deletes all resources within the resource group, so always be sure to confirm the contents of a resource group before deleting it.</span></span> <span data-ttu-id="ff4d1-190">Portalen tar bort alla resurser som finns i resursgruppen och sedan tas själva resursgruppen bort.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-190">The portal deletes all resources contained within the resource group, then deletes the resource group itself.</span></span> <span data-ttu-id="ff4d1-191">Den här processen tar flera minuter.</span><span class="sxs-lookup"><span data-stu-id="ff4d1-191">This process takes several minutes.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ff4d1-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff4d1-192">Next steps</span></span>

<span data-ttu-id="ff4d1-193">Läs mer om Azure DNS i [Översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-193">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="ff4d1-194">Mer information om hur du hanterar DNS-poster i Azure DNS finns i [Hantera DNS-poster och postuppsättningar med Azure Portal](dns-operations-recordsets-portal.md) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="ff4d1-194">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

