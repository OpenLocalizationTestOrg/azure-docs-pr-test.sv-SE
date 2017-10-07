---
title: "aaaGet igång med Azure DNS använder Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera dina första DNS-zonen och posten med hello Azure CLI 2.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 8a894941e9910d5cc35394a1be9dbca9792613f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-20"></a><span data-ttu-id="1e4f6-104">Komma igång med Azure DNS med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1e4f6-104">Get started with Azure DNS using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e4f6-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1e4f6-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="1e4f6-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e4f6-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="1e4f6-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1e4f6-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="1e4f6-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1e4f6-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="1e4f6-109">Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zonen och posten med hello plattformsoberoende Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-109">This article walks you through hello steps toocreate your first DNS zone and record using hello cross-platform Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="1e4f6-110">Du kan också utföra dessa steg med hello Azure-portalen eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-110">You can also perform these steps using hello Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="1e4f6-111">En DNS-zon är används toohost hello DNS-poster för en viss domän.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="1e4f6-112">toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="1e4f6-113">Varje DNS-post för din domän skapas sedan i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="1e4f6-114">Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="1e4f6-115">Dessa steg beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-115">Each of these steps is described below.</span></span>

<span data-ttu-id="1e4f6-116">Dessa instruktioner förutsätter att du redan har installerat och inloggad tooAzure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-116">These instructions assume you have already installed and signed in tooAzure CLI 2.0.</span></span> <span data-ttu-id="1e4f6-117">Mer information finns [hur toomanage DNS zoner med hjälp av Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-117">For help, see [How toomanage DNS zones using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="1e4f6-118">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="1e4f6-118">Create hello resource group</span></span>

<span data-ttu-id="1e4f6-119">Innan du skapar hello DNS-zonen skapas en resursgrupp toocontain hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="1e4f6-120">hello följande visar hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-120">hello following shows hello command.</span></span>

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="1e4f6-121">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="1e4f6-121">Create a DNS zone</span></span>

<span data-ttu-id="1e4f6-122">En DNS-zon skapas med hjälp av hello `az network dns zone create` kommando.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-122">A DNS zone is created using hello `az network dns zone create` command.</span></span> <span data-ttu-id="1e4f6-123">toosee hjälp för det här kommandot skriver `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-123">toosee help for this command, type `az network dns zone create -h`.</span></span>

<span data-ttu-id="1e4f6-124">hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-124">hello following example creates a DNS zone called *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="1e4f6-125">Använd hello exempel toocreate en DNS-zon ersätter hello värden för din egen.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-125">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="1e4f6-126">Skapa en DNS-post</span><span class="sxs-lookup"><span data-stu-id="1e4f6-126">Create a DNS record</span></span>

<span data-ttu-id="1e4f6-127">toocreate DNS-post och använda hello `az network dns record-set [record type] add-record` kommando.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-127">toocreate a DNS record, use hello `az network dns record-set [record type] add-record` command.</span></span> <span data-ttu-id="1e4f6-128">Mer information, om exempelvis A-poster, finns i `azure network dns record-set A add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-128">For help, for A records for example, see `azure network dns record-set A add-record -h`.</span></span>

<span data-ttu-id="1e4f6-129">hello följande exempel skapas en post med hello relativa namnet ”www” i hello DNS-zonen ”contoso.com” i resursgruppen ”MyResourceGroup”.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-129">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="1e4f6-130">hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-130">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="1e4f6-131">hello-posttypen är ”A”, med IP-adress ”1.2.3.4” och en standard-TTL 3600 sekunder (1 timme) används.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-131">hello record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

<span data-ttu-id="1e4f6-132">För andra posttyper postuppsättningar med fler än en post för den alternativa TTL-värden och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hello Azure CLI 2.0](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-132">For other record types, for record sets with more than one record, for alternative TTL values, and toomodify existing records, see [Manage DNS records and record sets using hello Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="1e4f6-133">Visa poster</span><span class="sxs-lookup"><span data-stu-id="1e4f6-133">View records</span></span>

<span data-ttu-id="1e4f6-134">toolist hello DNS-poster i zonen, Använd:</span><span class="sxs-lookup"><span data-stu-id="1e4f6-134">toolist hello DNS records in your zone, use:</span></span>

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="1e4f6-135">Uppdatera namnservrar</span><span class="sxs-lookup"><span data-stu-id="1e4f6-135">Update name servers</span></span>

<span data-ttu-id="1e4f6-136">När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="1e4f6-137">Detta gör att andra användare i hello Internet toofind DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-137">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="1e4f6-138">hello namnservrar för zonen ges av hello `az network dns zone show` kommando.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-138">hello name servers for your zone are given by hello `az network dns zone show` command.</span></span> <span data-ttu-id="1e4f6-139">servernamn toosee hello namn, Använd JSON-utdata, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-139">toosee hello name server names, use JSON output, as shown in hello following example.</span></span>

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="1e4f6-140">Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-140">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="1e4f6-141">Din registrator kommer att erbjuda hello alternativet tooset in hello namnservrar för hello domän.</span><span class="sxs-lookup"><span data-stu-id="1e4f6-141">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="1e4f6-142">Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-142">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="1e4f6-143">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="1e4f6-143">Delete all resources</span></span>
 
<span data-ttu-id="1e4f6-144">toodelete alla resurser skapas i den här artikeln tar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1e4f6-144">toodelete all resources created in this article, take hello following step:</span></span>

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="1e4f6-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e4f6-145">Next steps</span></span>

<span data-ttu-id="1e4f6-146">toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-146">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="1e4f6-147">toolearn mer information om hur du hanterar DNS-zoner i Azure DNS finns [hantera DNS-zoner i Azure DNS använder Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-147">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

<span data-ttu-id="1e4f6-148">toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och registrera anger i Azure DNS använder Azure CLI 2.0](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1e4f6-148">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>
