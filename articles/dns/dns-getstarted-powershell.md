---
title: "Komma igång med Azure DNS med PowerShell | Microsoft Docs"
description: "Läs om hur du skapar en DNS-zon och en DNS-post i Azure DNS. Detta är en steg-för-steg-guide om hur du skapar och hanterar din första DNS-zon och DNS-post med PowerShell."
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
ms.openlocfilehash: 48f7ba325f61b4a91c0208b4c99058da801bee19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a><span data-ttu-id="bce7b-104">Komma igång med Azure DNS med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bce7b-104">Get Started with Azure DNS using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bce7b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bce7b-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="bce7b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bce7b-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="bce7b-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bce7b-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="bce7b-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bce7b-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="bce7b-109">Den här artikeln visar hur du skapar din första DNS-zon och DNS-post med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bce7b-109">This article walks you through the steps to create your first DNS zone and record using Azure PowerShell.</span></span> <span data-ttu-id="bce7b-110">Du kan också utföra dessa steg med Azure Portal eller plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bce7b-110">You can also perform these steps using the Azure portal or the cross-platform Azure CLI.</span></span>

<span data-ttu-id="bce7b-111">En DNS-zon används som värd åt DNS-posterna för en viss domän.</span><span class="sxs-lookup"><span data-stu-id="bce7b-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="bce7b-112">Om du vill låta Azure DNS vara värd för din domän så måste du skapa en DNS-zon för det domännamnet.</span><span class="sxs-lookup"><span data-stu-id="bce7b-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="bce7b-113">Varje DNS-post för din domän skapas sedan i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="bce7b-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="bce7b-114">Om du vill publicera din DNS-zon på Internet måste du konfigurera namnservrarna för domänen.</span><span class="sxs-lookup"><span data-stu-id="bce7b-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="bce7b-115">Dessa steg beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="bce7b-115">Each of these steps is described below.</span></span>

<span data-ttu-id="bce7b-116">Anvisningarna förutsätter att du redan har installerat och loggat in på Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bce7b-116">These instructions assume you have already installed and signed in to Azure PowerShell.</span></span> <span data-ttu-id="bce7b-117">Mer information finns i [Hantera DNS-zoner med PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-117">For help, see [How to manage DNS zones using PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="bce7b-118">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bce7b-118">Create the resource group</span></span>

<span data-ttu-id="bce7b-119">Skapa en resursgrupp som ska innehålla DNS-zonen innan du skapar DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="bce7b-119">Before creating the DNS zone, a resource group is created to contain the DNS Zone.</span></span> <span data-ttu-id="bce7b-120">Nedan visas kommandot.</span><span class="sxs-lookup"><span data-stu-id="bce7b-120">The following shows the command.</span></span>

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="bce7b-121">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="bce7b-121">Create a DNS zone</span></span>

<span data-ttu-id="bce7b-122">En DNS-zon skapas med hjälp av cmdleten `New-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="bce7b-122">A DNS zone is created by using the `New-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="bce7b-123">Exemplet nedan skapar en DNS-zon som heter *contoso.com* i resursgruppen med namnet *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="bce7b-123">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="bce7b-124">Använd exemplet när du vill skapa en DNS-zon, och ersätt värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="bce7b-124">Use the example to create a DNS zone, substituting the values for your own.</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a><span data-ttu-id="bce7b-125">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="bce7b-125">Create a DNS record</span></span>

<span data-ttu-id="bce7b-126">Du skapar postuppsättningar med hjälp av cmdleten `New-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="bce7b-126">You create record sets by using the `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="bce7b-127">I följande exempel skapas en post med det relativa namnet "www" i resursgruppen "MyResourceGroup" i DNS-zonen "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="bce7b-127">The following example creates a record with the relative name "www" in the DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="bce7b-128">Postuppsättningens fullständigt kvalificerade namn är ”www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="bce7b-128">The fully-qualified name of the record set is "www.contoso.com".</span></span> <span data-ttu-id="bce7b-129">Posttypen är ”A” , IP-adressen är 1.2.3.4 och TTL är 3 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="bce7b-129">The record type is "A", with IP address "1.2.3.4", and the TTL is 3600 seconds.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

<span data-ttu-id="bce7b-130">Information om andra posttyper, postuppsättningar med fler än en post och ändring av befintliga poster finns i [Hantera DNS-poster och postuppsättningar med Azure PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-130">For other record types, for record sets with more than one record, and to modify existing records, see [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md).</span></span> 


## <a name="view-records"></a><span data-ttu-id="bce7b-131">Visa poster</span><span class="sxs-lookup"><span data-stu-id="bce7b-131">View records</span></span>

<span data-ttu-id="bce7b-132">Om du vill visa en lista med DNS-poster i din zon använder du:</span><span class="sxs-lookup"><span data-stu-id="bce7b-132">To list the DNS records in your zone, use:</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a><span data-ttu-id="bce7b-133">Uppdatera namnservrar</span><span class="sxs-lookup"><span data-stu-id="bce7b-133">Update name servers</span></span>

<span data-ttu-id="bce7b-134">När du är nöjd med konfigurationen av DNS-zonen och DNS-posterna måste du konfigurera ditt domännamn för användning med Azure DNS-namnservrarna.</span><span class="sxs-lookup"><span data-stu-id="bce7b-134">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="bce7b-135">Det gör att andra användare på Internet kan hitta dina DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="bce7b-135">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="bce7b-136">Namnservrarna för din zon anges av cmdleten `Get-AzureRmDnsZone`:</span><span class="sxs-lookup"><span data-stu-id="bce7b-136">The name servers for your zone are given by the `Get-AzureRmDnsZone` cmdlet:</span></span>

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

<span data-ttu-id="bce7b-137">Dessa namnservrar ska konfigureras med domännamnsregistratorn (där du köpte domännamnet).</span><span class="sxs-lookup"><span data-stu-id="bce7b-137">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="bce7b-138">Registratorn erbjuder möjligheten att konfigurera namnservrar för domänen.</span><span class="sxs-lookup"><span data-stu-id="bce7b-138">Your registrar will offer the option to set up the name servers for the domain.</span></span> <span data-ttu-id="bce7b-139">Mer information finns i [Delegera en domän till Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-139">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="bce7b-140">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="bce7b-140">Delete all resources</span></span>

<span data-ttu-id="bce7b-141">Så här tar du bort alla resurser som skapats i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="bce7b-141">To delete all resources created in this article, take the following step:</span></span>

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="bce7b-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bce7b-142">Next steps</span></span>

<span data-ttu-id="bce7b-143">Läs mer om Azure DNS i [Översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-143">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="bce7b-144">Mer information om hur du hanterar DNS-zoner i Azure DNS finns i [Hantera DNS-zoner i Azure DNS med PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-144">To learn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md).</span></span>

<span data-ttu-id="bce7b-145">Mer information om hur du hanterar DNS-poster i Azure DNS finns i [Hantera DNS-poster och postuppsättningar i Azure DNS med PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="bce7b-145">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md).</span></span>

