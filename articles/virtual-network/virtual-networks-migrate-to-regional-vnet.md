---
title: "aaaMigrate virtuellt Azure-nätverk (klassiskt) från en tillhörighetsgrupp tooa region | Microsoft Docs"
description: "Lär dig hur toomigrate ett virtuellt nätverk (klassiskt) från en tillhörighet gruppera tooa region."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a><span data-ttu-id="2a142-103">Migrera ett virtuellt nätverk (klassiska) från en tillhörighetsgrupp tooa region</span><span class="sxs-lookup"><span data-stu-id="2a142-103">Migrate a virtual network (classic) from an affinity group tooa region</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a142-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a142-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="2a142-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2a142-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="2a142-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="2a142-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="2a142-107">Tillhörighetsgrupper se till att resurserna skapas i samma tillhörighetsgrupp fysiskt finns på servrar som är nära varandra att aktivera dessa resurser toocommunicate snabbare hello.</span><span class="sxs-lookup"><span data-stu-id="2a142-107">Affinity groups ensure that resources created within hello same affinity group are physically hosted by servers that are close together, enabling these resources toocommunicate quicker.</span></span> <span data-ttu-id="2a142-108">I hello tidigare var tillhörighetsgrupper ett krav för att skapa virtuella nätverk (klassiska).</span><span class="sxs-lookup"><span data-stu-id="2a142-108">In hello past, affinity groups were a requirement for creating virtual networks (classic).</span></span> <span data-ttu-id="2a142-109">Då gick hello network manager-tjänsten som hanteras av virtuellt nätverk (klassiskt) fungerar bara i en uppsättning fysiska servrar eller skalningsenhet.</span><span class="sxs-lookup"><span data-stu-id="2a142-109">At that time, hello network manager service that managed virtual networks (classic) could only work within a set of physical servers or scale unit.</span></span> <span data-ttu-id="2a142-110">Arkitektur förbättringar har ökat hello omfattning nätverket management tooa region.</span><span class="sxs-lookup"><span data-stu-id="2a142-110">Architectural improvements have increased hello scope of network management tooa region.</span></span>

<span data-ttu-id="2a142-111">På grund av förbättringarna arkitektur finns tillhörighetsgrupper inte längre rekommenderas eller krävs för virtuella nätverk (klassiska).</span><span class="sxs-lookup"><span data-stu-id="2a142-111">As a result of these architectural improvements, affinity groups are no longer recommended, or required for virtual networks (classic).</span></span> <span data-ttu-id="2a142-112">hello användningen av tillhörighetsgrupper för virtuella nätverk (klassiskt) ersätts av regioner.</span><span class="sxs-lookup"><span data-stu-id="2a142-112">hello use of affinity groups for virtual networks (classic) is replaced by regions.</span></span> <span data-ttu-id="2a142-113">Virtuella nätverk (klassiskt) som är associerade med regioner kallas regionala virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2a142-113">Virtual networks (classic) that are associated with regions are called regional virtual networks.</span></span>

<span data-ttu-id="2a142-114">Vi rekommenderar att du inte använder tillhörighetsgrupper i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="2a142-114">We recommend that you don't use affinity groups in general.</span></span> <span data-ttu-id="2a142-115">Utöver hello virtuella nätverket kravet tillhörighetsgrupper har också viktig toouse tooensure resurser, till exempel beräkning (klassisk) och lagring (klassisk), placerades nära varandra.</span><span class="sxs-lookup"><span data-stu-id="2a142-115">Aside from hello virtual network requirement, affinity groups were also important toouse tooensure resources, such as compute (classic) and storage (classic), were placed near each other.</span></span> <span data-ttu-id="2a142-116">Men med hello aktuella Azure-nätverksarkitekturen behövs kraven placering inte längre.</span><span class="sxs-lookup"><span data-stu-id="2a142-116">However, with hello current Azure network architecture, these placement requirements are no longer necessary.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a142-117">Det är tekniskt möjligt toocreate ett virtuellt nätverk som är associerad med en tillhörighetsgrupp, men det finns inga tvingande orsak toodo så.</span><span class="sxs-lookup"><span data-stu-id="2a142-117">Although it is still technically possible toocreate a virtual network that is associated with an affinity group, there is no compelling reason toodo so.</span></span> <span data-ttu-id="2a142-118">Många funktioner för virtuella nätverk, till exempel nätverkssäkerhetsgrupper, är bara tillgängliga när du använder ett regionalt virtuellt nätverk och är inte tillgängliga för virtuella nätverk som är associerade med tillhörighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="2a142-118">Many virtual network features, such as network security groups, are only available when using a regional virtual network, and are not available for virtual networks that are associated with affinity groups.</span></span>
> 
> 

## <a name="edit-hello-network-configuration-file"></a><span data-ttu-id="2a142-119">Redigera konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="2a142-119">Edit hello network configuration file</span></span>

1. <span data-ttu-id="2a142-120">Exportera konfigurationsfilen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="2a142-120">Export hello network configuration file.</span></span> <span data-ttu-id="2a142-121">toolearn hur tooexport en nätverkskonfiguration med PowerShell eller hello Azure-kommandoradsgränssnittet (CLI) version 1.0, se [konfigurera ett virtuellt nätverk med en konfigurationsfil för nätverket](virtual-networks-using-network-configuration-file.md#export).</span><span class="sxs-lookup"><span data-stu-id="2a142-121">toolearn how tooexport a network configuration file using PowerShell or hello Azure command-line interface (CLI) 1.0, see [Configure a virtual network using a network configuration file](virtual-networks-using-network-configuration-file.md#export).</span></span>
2. <span data-ttu-id="2a142-122">Redigera konfigurationsfilen för hello nätverk, ersätter **AffinityGroup** med **plats**.</span><span class="sxs-lookup"><span data-stu-id="2a142-122">Edit hello network configuration file, replacing **AffinityGroup** with **Location**.</span></span> <span data-ttu-id="2a142-123">Du anger en Azure [region](https://azure.microsoft.com/regions) för **plats**.</span><span class="sxs-lookup"><span data-stu-id="2a142-123">You specify an Azure [region](https://azure.microsoft.com/regions) for **Location**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2a142-124">Hej **plats** hello region som du angav för hello tillhörighetsgrupp som är kopplad till ditt virtuella nätverk (klassiska).</span><span class="sxs-lookup"><span data-stu-id="2a142-124">hello **Location** is hello region that you specified for hello affinity group that is associated with your virtual network (classic).</span></span> <span data-ttu-id="2a142-125">Om ditt virtuella nätverk (klassiska) som är associerad med en tillhörighetsgrupp som finns i västra USA när du migrerar, till exempel din **plats** måste peka tooWest USA.</span><span class="sxs-lookup"><span data-stu-id="2a142-125">For example, if your virtual network (classic) is associated with an affinity group that is located in West US, when you migrate, your **Location** must point tooWest US.</span></span> 
   > 
   > 
   
    <span data-ttu-id="2a142-126">Redigera hello följande rader i konfigurationsfilen nätverk, ersätter hello värden med dina egna:</span><span class="sxs-lookup"><span data-stu-id="2a142-126">Edit hello following lines in your network configuration file, replacing hello values with your own:</span></span> 
   
    <span data-ttu-id="2a142-127">**Gammalt värde:** \<VirtualNetworkSitename = ”VNetUSWest” AffinityGroup = ”VNetDemoAG”\></span><span class="sxs-lookup"><span data-stu-id="2a142-127">**Old value:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\></span></span> 
   
    <span data-ttu-id="2a142-128">**Nytt värde:** \<VirtualNetworkSitename = ”VNetUSWest” sökväg = ”USA, västra”\></span><span class="sxs-lookup"><span data-stu-id="2a142-128">**New value:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\></span></span>
3. <span data-ttu-id="2a142-129">Spara dina ändringar och [importera](virtual-networks-using-network-configuration-file.md#import) hello tooAzure för konfiguration av nätverk.</span><span class="sxs-lookup"><span data-stu-id="2a142-129">Save your changes and [import](virtual-networks-using-network-configuration-file.md#import) hello network configuration tooAzure.</span></span>

> [!NOTE]
> <span data-ttu-id="2a142-130">Migreringen medför några driftavbrott tooyour tjänster.</span><span class="sxs-lookup"><span data-stu-id="2a142-130">This migration does NOT cause any downtime tooyour services.</span></span>
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a><span data-ttu-id="2a142-131">Vilka toodo om du har en virtuell dator (klassisk) i en tillhörighetsgrupp</span><span class="sxs-lookup"><span data-stu-id="2a142-131">What toodo if you have a VM (classic) in an affinity group</span></span>
<span data-ttu-id="2a142-132">Virtuella datorer (klassiskt) som används för närvarande i en tillhörighetsgrupp behöver inte toobe tas bort från hello tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="2a142-132">VMs (classic) that are currently in an affinity group do not need toobe removed from hello affinity group.</span></span> <span data-ttu-id="2a142-133">När en virtuell dator har distribuerats, är det enda skalningsenhet för distribuerade tooa.</span><span class="sxs-lookup"><span data-stu-id="2a142-133">Once a VM is deployed, it is deployed tooa single scale unit.</span></span> <span data-ttu-id="2a142-134">Tillhörighetsgrupper kan begränsa hello uppsättning tillgängliga VM-storlekar för en ny VM-distribution, men en befintlig virtuell dator som har distribuerats redan är begränsad toohello uppsättning VM storlekar i hello skalningsenhet i vilka hello VM distribueras.</span><span class="sxs-lookup"><span data-stu-id="2a142-134">Affinity groups can restrict hello set of available VM sizes for a new VM deployment, but any existing VM that is deployed is already restricted toohello set of VM sizes available in hello scale unit in which hello VM is deployed.</span></span> <span data-ttu-id="2a142-135">Eftersom hello VM är redan distribuerad tooa skalningsenhet, har ta bort en virtuell dator från en tillhörighetsgrupp ingen effekt på hello VM.</span><span class="sxs-lookup"><span data-stu-id="2a142-135">Because hello VM is already deployed tooa scale unit, removing a VM from an affinity group has no effect on hello VM.</span></span>
