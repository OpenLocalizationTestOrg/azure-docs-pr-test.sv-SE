---
title: "aaaVM startar om eller ändrar storlek på problem i Azure | Microsoft Docs"
description: "Felsökning av Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 01/10/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d9ff76b64b6646b04565eb93110759c4ebc858e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a><span data-ttu-id="a33ef-103">Felsöka distributionsproblem med att starta om eller ändrar storlek på en befintlig Linux VM i Azure</span><span class="sxs-lookup"><span data-stu-id="a33ef-103">Troubleshoot deployment issues with restarting or resizing an existing Linux VM in Azure</span></span>
<span data-ttu-id="a33ef-104">När du försöker toostart en stoppad Azure virtuell dator (VM) eller ändra storlek på en befintlig virtuell Azure-dator, är hello vanliga fel du stöter på ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="a33ef-104">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="a33ef-105">Det här felet uppstår när hello kluster eller den region antingen inte har resurser som är tillgängliga eller inte support hello begärda VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="a33ef-105">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="a33ef-106">Samla in aktiviteten loggar</span><span class="sxs-lookup"><span data-stu-id="a33ef-106">Collect activity logs</span></span>
<span data-ttu-id="a33ef-107">toostart felsökning, samla in hello aktivitet loggar tooidentify hello fel som är associerade med hello problemet.</span><span class="sxs-lookup"><span data-stu-id="a33ef-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="a33ef-108">hello följande länkar innehåller detaljerad information om hello processen:</span><span class="sxs-lookup"><span data-stu-id="a33ef-108">hello following links contain detailed information on hello process:</span></span>

[<span data-ttu-id="a33ef-109">Visa distributionsåtgärder</span><span class="sxs-lookup"><span data-stu-id="a33ef-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="a33ef-110">Visa aktiviteten loggar toomanage Azure resurser</span><span class="sxs-lookup"><span data-stu-id="a33ef-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="a33ef-111">Problem: Fel när du startar en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a33ef-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="a33ef-112">Du försöker toostart en stoppad virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="a33ef-112">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="a33ef-113">Orsak</span><span class="sxs-lookup"><span data-stu-id="a33ef-113">Cause</span></span>
<span data-ttu-id="a33ef-114">hello begäran toostart hello stoppats VM har toobe med hello ursprungliga klustret som är värd för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a33ef-114">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="a33ef-115">Hello-klustret har dock inte ledigt utrymme tillgängligt toofulfill hello begäran.</span><span class="sxs-lookup"><span data-stu-id="a33ef-115">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="a33ef-116">Lösning</span><span class="sxs-lookup"><span data-stu-id="a33ef-116">Resolution</span></span>
* <span data-ttu-id="a33ef-117">Stoppa alla hello virtuella datorer i hello tillgänglighet ange och starta sedan om varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a33ef-117">Stop all hello VMs in hello availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="a33ef-118">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="a33ef-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="a33ef-119">När alla Hej stoppa virtuella datorer, Välj av hello stoppade virtuella datorer och klicka på Start.</span><span class="sxs-lookup"><span data-stu-id="a33ef-119">After all hello VMs stop, select each of hello stopped VMs and click Start.</span></span>
* <span data-ttu-id="a33ef-120">Försök igen med begäran för hello omstart vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="a33ef-120">Retry hello restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="a33ef-121">Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a33ef-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="a33ef-122">Du försöker tooresize en befintlig virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="a33ef-122">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="a33ef-123">Orsak</span><span class="sxs-lookup"><span data-stu-id="a33ef-123">Cause</span></span>
<span data-ttu-id="a33ef-124">hello begäran tooresize hello VM har toobe försökt på hello ursprungliga klustret värdar hello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a33ef-124">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="a33ef-125">Dock hello kluster stöder inte hello begärda VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="a33ef-125">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="a33ef-126">Lösning</span><span class="sxs-lookup"><span data-stu-id="a33ef-126">Resolution</span></span>
* <span data-ttu-id="a33ef-127">Försök hello förfrågan med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="a33ef-127">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="a33ef-128">Om hello efterfrågade storleken hello VM inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="a33ef-128">If hello size of hello requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="a33ef-129">Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="a33ef-129">Stop all hello VMs in hello availability set.</span></span>
     
     * <span data-ttu-id="a33ef-130">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="a33ef-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="a33ef-131">När alla Hej stoppa virtuella datorer, ändra storlek på hello önskad VM tooa större storlek.</span><span class="sxs-lookup"><span data-stu-id="a33ef-131">After all hello VMs stop, resize hello desired VM tooa larger size.</span></span>
  3. <span data-ttu-id="a33ef-132">Välj hello storlek VM och klicka på **starta**, och sedan början av hello stoppas virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a33ef-132">Select hello resized VM and click **Start**, and then start each of hello stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a33ef-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a33ef-133">Next steps</span></span>
<span data-ttu-id="a33ef-134">Om du får problem när du skapar en ny Linux VM i Azure, se [felsöka distributionsproblem med att skapa en ny virtuell Linux-dator i Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a33ef-134">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

