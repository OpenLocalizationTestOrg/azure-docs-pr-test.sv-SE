---
title: "aaaSet upp en replikeringsprincip för Hyper-V-dator (med VMM) replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset upp en replikeringsprincip för Hyper-V-dator (med VMM) replikering tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: e1579fde559ca34eca19a01e740fec28a0df2f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a><span data-ttu-id="26480-103">Steg 10: Ställ in en replikeringsprincip för Hyper-V VM replikering (med VMM) tooAzure</span><span class="sxs-lookup"><span data-stu-id="26480-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) tooAzure</span></span>


<span data-ttu-id="26480-104">När du har installerat [nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md), Använd den här artikeln tooconfigure en replikeringsprincip T\tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [ Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="26480-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article tooconfigure a replication policy T\tooreplicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="26480-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="26480-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="26480-106">Skapa en princip</span><span class="sxs-lookup"><span data-stu-id="26480-106">Create a policy</span></span>

1. <span data-ttu-id="26480-107">Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.</span><span class="sxs-lookup"><span data-stu-id="26480-107">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Nätverk](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="26480-109">I **Princip för att skapa och koppla** anger du ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="26480-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="26480-110">I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).</span><span class="sxs-lookup"><span data-stu-id="26480-110">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="26480-111">En 30 andra frekvens stöds inte vid replikering av toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="26480-111">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="26480-112">hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="26480-112">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="26480-113">Läs mer</span><span class="sxs-lookup"><span data-stu-id="26480-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="26480-114">I **kvarhållningstid för återställningspunkten**, ange i timmar hur länge hello kvarhållningsperiod ska vara för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="26480-114">In **Recovery point retention**, specify in hours how long hello retention window will be for each recovery point.</span></span> <span data-ttu-id="26480-115">Skyddade datorer kan återställas tooany punkt inom en period.</span><span class="sxs-lookup"><span data-stu-id="26480-115">Protected machines can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="26480-116">I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (1–12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="26480-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="26480-117">Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="26480-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span> <span data-ttu-id="26480-118">Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas.</span><span class="sxs-lookup"><span data-stu-id="26480-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span> <span data-ttu-id="26480-119">Observera att om du aktiverar programkonsekventa ögonblicksbilder påverkas hello prestanda för program som körs på virtuella källdatorer.</span><span class="sxs-lookup"><span data-stu-id="26480-119">Note that if you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="26480-120">Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="26480-120">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="26480-121">I **starttid för inledande replikering**, ange när toostart hello inledande replikering.</span><span class="sxs-lookup"><span data-stu-id="26480-121">In **Initial replication start time**, indicate when toostart hello initial replication.</span></span> <span data-ttu-id="26480-122">hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid.</span><span class="sxs-lookup"><span data-stu-id="26480-122">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span>
7. <span data-ttu-id="26480-123">I **kryptera data lagrade på Azure**, ange om tooencrypt vilande data i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="26480-123">In **Encrypt data stored on Azure**, specify whether tooencrypt at rest data in Azure storage.</span></span> <span data-ttu-id="26480-124">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="26480-124">Then click **OK**.</span></span>

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="26480-126">När du skapar en ny princip associeras den automatiskt med hello VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="26480-126">When you create a new policy it's automatically associated with hello VMM cloud.</span></span> <span data-ttu-id="26480-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="26480-127">Click **OK**.</span></span> <span data-ttu-id="26480-128">Du kan associera ytterligare VMM-moln (och hello virtuella datorer i dem) med den här replikeringsprincipen i **replikering** > principnamn > **associera VMM-moln**.</span><span class="sxs-lookup"><span data-stu-id="26480-128">You can associate additional VMM clouds (and hello VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="26480-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="26480-130">Next steps</span></span>

<span data-ttu-id="26480-131">Gå för[steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="26480-131">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
