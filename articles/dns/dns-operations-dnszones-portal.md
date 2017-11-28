---
title: aaaManage DNS-zoner i Azure DNS - Azure-portalen | Microsoft Docs
description: "Du kan hantera DNS-zoner med hello Azure-portalen. Den här artikeln beskriver hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a><span data-ttu-id="bcc35-104">Hur toomanage DNS-zoner i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bcc35-104">How toomanage DNS Zones in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bcc35-105">Portal</span><span class="sxs-lookup"><span data-stu-id="bcc35-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="bcc35-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bcc35-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="bcc35-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bcc35-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="bcc35-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bcc35-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="bcc35-109">Den här artikeln visar hur toomanage DNS-zoner med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bcc35-109">This article shows you how toomanage your DNS zones by using hello Azure portal.</span></span> <span data-ttu-id="bcc35-110">Du kan också hantera DNS-zoner med hello plattformsoberoende [Azure CLI](dns-operations-dnszones-cli.md) eller hello Azure [PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="bcc35-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure [PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="bcc35-111">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="bcc35-111">Create a DNS zone</span></span>

1. <span data-ttu-id="bcc35-112">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bcc35-112">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="bcc35-113">Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.</span><span class="sxs-lookup"><span data-stu-id="bcc35-113">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zon](./media/dns-operations-dnszones-portal/openzone650.png)

4. <span data-ttu-id="bcc35-115">På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="bcc35-115">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>


   | <span data-ttu-id="bcc35-116">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="bcc35-116">**Setting**</span></span> | <span data-ttu-id="bcc35-117">**Värde**</span><span class="sxs-lookup"><span data-stu-id="bcc35-117">**Value**</span></span> | <span data-ttu-id="bcc35-118">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="bcc35-118">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="bcc35-119">**Namn**</span><span class="sxs-lookup"><span data-stu-id="bcc35-119">**Name**</span></span>|<span data-ttu-id="bcc35-120">contoso.com</span><span class="sxs-lookup"><span data-stu-id="bcc35-120">contoso.com</span></span>|<span data-ttu-id="bcc35-121">hello namnet på hello DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="bcc35-121">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="bcc35-122">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bcc35-122">**Subscription**</span></span>|<span data-ttu-id="bcc35-123">[Din prenumeration]</span><span class="sxs-lookup"><span data-stu-id="bcc35-123">[Your subscription]</span></span>|<span data-ttu-id="bcc35-124">Välj en prenumeration toocreate hello DNS-zonen i.</span><span class="sxs-lookup"><span data-stu-id="bcc35-124">Select a subscription toocreate hello DNS zone in.</span></span>|
   |<span data-ttu-id="bcc35-125">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bcc35-125">**Resource group**</span></span>|<span data-ttu-id="bcc35-126">**Skapa ny:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="bcc35-126">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="bcc35-127">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bcc35-127">Create a resource group.</span></span> <span data-ttu-id="bcc35-128">hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt.</span><span class="sxs-lookup"><span data-stu-id="bcc35-128">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="bcc35-129">Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="bcc35-129">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="bcc35-130">**Plats**</span><span class="sxs-lookup"><span data-stu-id="bcc35-130">**Location**</span></span>|<span data-ttu-id="bcc35-131">Västra USA</span><span class="sxs-lookup"><span data-stu-id="bcc35-131">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="bcc35-132">hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="bcc35-132">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="bcc35-133">hello DNS-zonen plats är alltid ”globala” och visas inte.</span><span class="sxs-lookup"><span data-stu-id="bcc35-133">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="list-dns-zones"></a><span data-ttu-id="bcc35-134">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="bcc35-134">List DNS zones</span></span>

<span data-ttu-id="bcc35-135">I hello Azure portal, navigerar för**fler tjänster** > **nätverk** > **DNS-zoner**.</span><span class="sxs-lookup"><span data-stu-id="bcc35-135">In hello Azure portal, navigate too**More services** > **Networking** > **DNS zones**.</span></span> <span data-ttu-id="bcc35-136">Varje DNS-zon är det egna resurs, information, till exempel antal post och namnservrar kan visas i den här vyn.</span><span class="sxs-lookup"><span data-stu-id="bcc35-136">Each DNS zone is it's own resource, information such as number of record-sets and name servers are viewable from this view.</span></span> <span data-ttu-id="bcc35-137">hello kolumnen **NAMNSERVRAR** finns inte i hello standardvyn, tooadd det klickar du på **kolumner**väljer **namnservrar** och på **klar**.</span><span class="sxs-lookup"><span data-stu-id="bcc35-137">hello column **NAME SERVERS** is not in hello default view, tooadd it click **Columns**, select **Name servers** and click **Done**.</span></span>

![Visar en lista över DNS-zoner](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a><span data-ttu-id="bcc35-139">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="bcc35-139">Delete a DNS zone</span></span>

<span data-ttu-id="bcc35-140">Navigera tooa DNS-zonen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bcc35-140">Navigate tooa DNS zone in hello portal.</span></span> <span data-ttu-id="bcc35-141">På hello **DNS-zonen** bladet, klickar du på **ta bort zon**.</span><span class="sxs-lookup"><span data-stu-id="bcc35-141">On hello **DNS zone** blade, click **Delete zone**.</span></span> <span data-ttu-id="bcc35-142">Du kan ange tooconfirm du förskjutning toodelete hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="bcc35-142">You are prompted tooconfirm you are wanting toodelete hello DNS zone.</span></span> <span data-ttu-id="bcc35-143">En DNS-zon också tar du bort alla hello-poster som ingår i hello zon.</span><span class="sxs-lookup"><span data-stu-id="bcc35-143">Deleting a DNS zone also deletes all hello records that are contained in hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcc35-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcc35-144">Next steps</span></span>

<span data-ttu-id="bcc35-145">Lär dig hur toowork med DNS-zon och poster genom att besöka [Kom igång med Azure DNS med hello Azure-portalen](dns-getstarted-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bcc35-145">Learn how toowork with your DNS Zone and records by visiting [Get started with Azure DNS using hello Azure portal](dns-getstarted-portal.md).</span></span>
