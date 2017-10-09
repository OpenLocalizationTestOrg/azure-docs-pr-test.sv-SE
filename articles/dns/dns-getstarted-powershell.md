---
title: "aaaGet igång med Azure DNS med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera din första DNS-zonen och posten med hjälp av PowerShell."
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
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a><span data-ttu-id="b452b-104">Komma igång med Azure DNS med PowerShell</span><span class="sxs-lookup"><span data-stu-id="b452b-104">Get Started with Azure DNS using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b452b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b452b-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="b452b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b452b-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="b452b-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b452b-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="b452b-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b452b-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="b452b-109">Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zon och en post med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b452b-109">This article walks you through hello steps toocreate your first DNS zone and record using Azure PowerShell.</span></span> <span data-ttu-id="b452b-110">Du kan också utföra dessa steg med hello Azure-portalen eller hello plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b452b-110">You can also perform these steps using hello Azure portal or hello cross-platform Azure CLI.</span></span>

<span data-ttu-id="b452b-111">En DNS-zon är används toohost hello DNS-poster för en viss domän.</span><span class="sxs-lookup"><span data-stu-id="b452b-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="b452b-112">toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet.</span><span class="sxs-lookup"><span data-stu-id="b452b-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="b452b-113">Varje DNS-post för din domän skapas sedan i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="b452b-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="b452b-114">Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="b452b-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="b452b-115">Dessa steg beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="b452b-115">Each of these steps is described below.</span></span>

<span data-ttu-id="b452b-116">Dessa instruktioner förutsätter att du redan har installerat och inloggad tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b452b-116">These instructions assume you have already installed and signed in tooAzure PowerShell.</span></span> <span data-ttu-id="b452b-117">Mer information finns [hur toomanage DNS zoner med hjälp av PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-117">For help, see [How toomanage DNS zones using PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="b452b-118">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b452b-118">Create hello resource group</span></span>

<span data-ttu-id="b452b-119">Innan du skapar hello DNS-zonen skapas en resursgrupp toocontain hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="b452b-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="b452b-120">hello följande visar hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="b452b-120">hello following shows hello command.</span></span>

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="b452b-121">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="b452b-121">Create a DNS zone</span></span>

<span data-ttu-id="b452b-122">En DNS-zon skapas med hjälp av hello `New-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b452b-122">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="b452b-123">hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="b452b-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="b452b-124">Använd hello exempel toocreate en DNS-zon ersätter hello värden för din egen.</span><span class="sxs-lookup"><span data-stu-id="b452b-124">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a><span data-ttu-id="b452b-125">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="b452b-125">Create a DNS record</span></span>

<span data-ttu-id="b452b-126">Du skapar postuppsättningar med hello `New-AzureRmDnsRecordSet` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b452b-126">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="b452b-127">hello följande exempel skapas en post med hello relativa namnet ”www” i hello DNS-zonen ”contoso.com” i resursgruppen ”MyResourceGroup”.</span><span class="sxs-lookup"><span data-stu-id="b452b-127">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="b452b-128">hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="b452b-128">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="b452b-129">hello-posttypen är ”A”, med IP-adress ”1.2.3.4” och hello TTL-värde är 3 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b452b-129">hello record type is "A", with IP address "1.2.3.4", and hello TTL is 3600 seconds.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

<span data-ttu-id="b452b-130">För andra posttyper postuppsättningar med fler än en post och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av Azure PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-130">For other record types, for record sets with more than one record, and toomodify existing records, see [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md).</span></span> 


## <a name="view-records"></a><span data-ttu-id="b452b-131">Visa poster</span><span class="sxs-lookup"><span data-stu-id="b452b-131">View records</span></span>

<span data-ttu-id="b452b-132">toolist hello DNS-poster i zonen, Använd:</span><span class="sxs-lookup"><span data-stu-id="b452b-132">toolist hello DNS records in your zone, use:</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a><span data-ttu-id="b452b-133">Uppdatera namnservrar</span><span class="sxs-lookup"><span data-stu-id="b452b-133">Update name servers</span></span>

<span data-ttu-id="b452b-134">När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="b452b-134">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="b452b-135">Detta gör att andra användare i hello Internet toofind DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="b452b-135">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="b452b-136">hello namnservrar för zonen ges av hello `Get-AzureRmDnsZone` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b452b-136">hello name servers for your zone are given by hello `Get-AzureRmDnsZone` cmdlet:</span></span>

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

<span data-ttu-id="b452b-137">Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn).</span><span class="sxs-lookup"><span data-stu-id="b452b-137">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="b452b-138">Din registrator kommer att erbjuda hello alternativet tooset in hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="b452b-138">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="b452b-139">Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-139">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="b452b-140">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="b452b-140">Delete all resources</span></span>

<span data-ttu-id="b452b-141">toodelete alla resurser skapas i den här artikeln tar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b452b-141">toodelete all resources created in this article, take hello following step:</span></span>

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="b452b-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b452b-142">Next steps</span></span>

<span data-ttu-id="b452b-143">toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-143">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="b452b-144">toolearn mer information om hur du hanterar DNS-zoner i Azure DNS finns [hantera DNS-zoner i Azure DNS med hjälp av PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-144">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md).</span></span>

<span data-ttu-id="b452b-145">toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och registrera anger i Azure DNS med hjälp av PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="b452b-145">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md).</span></span>

