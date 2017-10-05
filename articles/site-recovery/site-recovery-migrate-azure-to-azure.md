---
title: Migrera Azure IaaS-VM mellan Azure-regioner | Microsoft Docs
description: "Använd Azure Site Recovery för att migrera Azure IaaS-virtuella datorer från en Azure-region till en annan."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: ef2972c077a2b1dd2b2fd6ce53cc6560520ea870
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a><span data-ttu-id="de37a-103">Migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="de37a-103">Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery</span></span>
## <a name="overview"></a><span data-ttu-id="de37a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="de37a-104">Overview</span></span>
<span data-ttu-id="de37a-105">Välkommen till Azure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="de37a-105">Welcome to Azure Site Recovery!</span></span> <span data-ttu-id="de37a-106">Använd den här artikeln om du vill migrera virtuella datorer i Azure mellan Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="de37a-106">Use this article if you want to migrate Azure VMs between Azure regions.</span></span> <span data-ttu-id="de37a-107">Observera att innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="de37a-107">Before you start, note that:</span></span>

* <span data-ttu-id="de37a-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="de37a-108">Azure has two different deployment models for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="de37a-109">Azure har också två portaler – den klassiska Azure-portalen som stöder den klassiska distributionsmodellen och Azure-portalen som stöder båda distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="de37a-109">Azure also has two portals – the Azure classic portal that supports the classic deployment model, and the Azure portal with support for both deployment models.</span></span> <span data-ttu-id="de37a-110">De grundläggande stegen för migrering är samma oavsett om du konfigurerar Site Recovery i Resource Manager eller i klassiskt.</span><span class="sxs-lookup"><span data-stu-id="de37a-110">The basic steps for migration are the same whether you're configuring Site Recovery in Resource Manager or in classic.</span></span> <span data-ttu-id="de37a-111">Men Användargränssnittet anvisningar och skärmdumpar i den här artikeln gäller för Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de37a-111">However the UI instructions and screenshots in this article are relevant for the Azure portal.</span></span>
* <span data-ttu-id="de37a-112">**För närvarande kan du bara migrera från en region till en annan. Du kan växla över virtuella datorer från en Azure-region till en annan, men du kan inte återställas dem igen.**</span><span class="sxs-lookup"><span data-stu-id="de37a-112">**Currently you can only migrate from one region to another. You can fail over VMs from one Azure region to another, but you can't fail them back again.**</span></span>

<span data-ttu-id="de37a-113">Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="de37a-113">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de37a-114">Krav</span><span class="sxs-lookup"><span data-stu-id="de37a-114">Prerequisites</span></span>
<span data-ttu-id="de37a-115">Här är vad du behöver för den här distributionen:</span><span class="sxs-lookup"><span data-stu-id="de37a-115">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="de37a-116">**IaaS-virtuella datorer**: på virtuella datorer som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="de37a-116">**IaaS virtual machines**: The VMs you want to migrate.</span></span> <span data-ttu-id="de37a-117">Du kan migrera dessa virtuella datorer genom att behandla dem som fysiska datorer.</span><span class="sxs-lookup"><span data-stu-id="de37a-117">You migrate these VMs by treating them as physical machines.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="de37a-118">Distributionssteg</span><span class="sxs-lookup"><span data-stu-id="de37a-118">Deployment steps</span></span>
<span data-ttu-id="de37a-119">Det här avsnittet beskrivs stegen för distributionen i den nya Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de37a-119">This section describes the deployment steps in the new Azure portal.</span></span>

1. <span data-ttu-id="de37a-120">[Skapa ett valv](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="de37a-120">[Create a vault](site-recovery-vmware-to-azure.md).</span></span>
2. <span data-ttu-id="de37a-121">[Aktivera replikering](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="de37a-121">[Enable replication](site-recovery-vmware-to-azure.md).</span></span> <span data-ttu-id="de37a-122">Aktivera replikering för virtuella datorer du vill migrera och välja Azure som källa.</span><span class="sxs-lookup"><span data-stu-id="de37a-122">Enable replication for the VMs you want to migrate, and choose Azure as source.</span></span> 
3. <span data-ttu-id="de37a-123">[Kör en oplanerad redundansväxling](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="de37a-123">[ Run an unplanned failover](site-recovery-failover.md).</span></span> <span data-ttu-id="de37a-124">När den inledande replikeringen är klar kan köra du en oplanerad redundans från en Azure-region till en annan.</span><span class="sxs-lookup"><span data-stu-id="de37a-124">After initial replication is complete, you can run an unplanned failover from one Azure region to another.</span></span> <span data-ttu-id="de37a-125">Alternativt kan du skapa en återställningsplan och kör en oplanerad redundans för att migrera flera virtuella datorer mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="de37a-125">Optionally, you can create a recovery plan and run an unplanned failover, to migrate multiple virtual machines between regions.</span></span> <span data-ttu-id="de37a-126">[Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="de37a-126">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de37a-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de37a-127">Next steps</span></span>
<span data-ttu-id="de37a-128">Mer information om andra replikeringarna i [vad är Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="de37a-128">Learn more about other replication scenarios in [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>
