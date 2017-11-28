---
title: "aaaSet upp en replikeringsprincip för Hyper-V VM replikering (utan VMM för System Center) tooAzure med Azure Site Recovery | Microsoft Docs"
description: Sammanfattar hello stegen tooset upp en replikeringsprincip vid replikering av Hyper-V VMs tooAzure lagring
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 4bd7161f4a0f015da0ecf595fbc6861ede5d68b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a><span data-ttu-id="4e986-103">Steg 9: Konfigurera en replikeringsprincip för Hyper-V VM replikering tooAzure</span><span class="sxs-lookup"><span data-stu-id="4e986-103">Step 9: Set up a replication policy for Hyper-V VM replication tooAzure</span></span>

<span data-ttu-id="4e986-104">Den här artikeln beskriver hur tooset upp en replikeringsprincip när du replikerar virtuella Hyper-V-datorer tooAzure (utan System Center VMM) med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e986-104">This article describes how tooset up a replication policy when you're replicating Hyper-V VMs tooAzure (without System Center VMM) using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="4e986-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4e986-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="about-snapshots"></a><span data-ttu-id="4e986-106">Om ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="4e986-106">About snapshots</span></span>

<span data-ttu-id="4e986-107">Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4e986-107">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span>
    - <span data-ttu-id="4e986-108">Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas.</span><span class="sxs-lookup"><span data-stu-id="4e986-108">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span>
    - <span data-ttu-id="4e986-109">Om du aktiverar programkonsekventa ögonblicksbilder så påverkar hello prestanda för program som körs på virtuella källdatorer.</span><span class="sxs-lookup"><span data-stu-id="4e986-109">If you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="4e986-110">Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="4e986-110">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>

## <a name="set-up-a-replication-policy"></a><span data-ttu-id="4e986-111">Konfigurera en princip för lösenordsreplikering</span><span class="sxs-lookup"><span data-stu-id="4e986-111">Set up a replication policy</span></span>

1. <span data-ttu-id="4e986-112">Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.</span><span class="sxs-lookup"><span data-stu-id="4e986-112">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Nätverk](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="4e986-114">I **Princip för att skapa och koppla** anger du ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="4e986-114">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="4e986-115">I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).</span><span class="sxs-lookup"><span data-stu-id="4e986-115">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    > <span data-ttu-id="4e986-116">En 30 andra frekvens stöds inte vid replikering av toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="4e986-116">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="4e986-117">hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="4e986-117">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> <span data-ttu-id="4e986-118">[Läs mer](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span><span class="sxs-lookup"><span data-stu-id="4e986-118">[Learn more](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span></span>

4. <span data-ttu-id="4e986-119">I **kvarhållningstid för återställningspunkten**, ange i hur länge timmar hello kvarhållningsperiod är för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="4e986-119">In **Recovery point retention**, specify in hours how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="4e986-120">Virtuella datorer kan återställas tooany punkt inom en period.</span><span class="sxs-lookup"><span data-stu-id="4e986-120">VMs can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="4e986-121">I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas.</span><span class="sxs-lookup"><span data-stu-id="4e986-121">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots are created.</span></span>
6. <span data-ttu-id="4e986-122">I **starttid för inledande replikering**, ange när toostart hello inledande replikering.</span><span class="sxs-lookup"><span data-stu-id="4e986-122">In **Initial replication start time**, specify when toostart hello initial replication.</span></span> <span data-ttu-id="4e986-123">hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid.</span><span class="sxs-lookup"><span data-stu-id="4e986-123">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span> <span data-ttu-id="4e986-124">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e986-124">Then click **OK**.</span></span>

    ![Replikeringsprincip](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

<span data-ttu-id="4e986-126">När du skapar en ny princip associeras den automatiskt med hello Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="4e986-126">When you create a new policy, it's automatically associated with hello Hyper-V site.</span></span> <span data-ttu-id="4e986-127">Du kan associera en Hyper-V-platsen (och hello virtuella datorer i den) med flera replikeringsprinciper i **replikering** > principnamn > **associera Hyper-V-platsen**.</span><span class="sxs-lookup"><span data-stu-id="4e986-127">You can associate a Hyper-V site (and hello VMs in it) with multiple replication policies in **Replication** > policy-name > **Associate Hyper-V Site**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="4e986-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e986-128">Next steps</span></span>

<span data-ttu-id="4e986-129">Gå för[steg 10: Aktivera replikering](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4e986-129">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>
