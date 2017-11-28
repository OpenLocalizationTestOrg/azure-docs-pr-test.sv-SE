---
title: "Konfigurera en replikeringsprincip för Hyper-V-dator (med VMM) replikering till Azure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur du ställer in en replikeringsprincip för Hyper-V-dator (med VMM) replikering till Azure med Azure Site Recovery"
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
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a><span data-ttu-id="ac974-103">Steg 10: Ställ in en replikeringsprincip för Virtuella Hyper-V-replikering (med VMM) till Azure</span><span class="sxs-lookup"><span data-stu-id="ac974-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) to Azure</span></span>


<span data-ttu-id="ac974-104">När du har installerat [nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md), Använd den här artikeln för att konfigurera en replikeringsprincip T\to replikera lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM)-moln till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ac974-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article to configure a replication policy T\to replicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="ac974-105">När du har läst den här artikeln kan du lämna kommentarer längst ned på sidan eller på [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ac974-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="ac974-106">Skapa en princip</span><span class="sxs-lookup"><span data-stu-id="ac974-106">Create a policy</span></span>

1. <span data-ttu-id="ac974-107">Skapa en ny replikeringsprincip genom att klicka på **Förbered infrastruktur** > **Replikeringsinställningar** > **+Skapa och koppla**.</span><span class="sxs-lookup"><span data-stu-id="ac974-107">To create a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Nätverk](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="ac974-109">I **Princip för att skapa och koppla** anger du ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="ac974-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="ac974-110">I **Kopieringsfrekvens** anger du hur ofta du vill replikera förändringsdata (delta) efter den första replikeringen (med 30 sekunders mellanrum, var femte minut eller varje kvart).</span><span class="sxs-lookup"><span data-stu-id="ac974-110">In **Copy frequency**, specify how often you want to replicate delta data after the initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="ac974-111">En frekvens på 30 sekunder stöds inte när du replikerar till premiumlagring.</span><span class="sxs-lookup"><span data-stu-id="ac974-111">A 30 second frequency isn't supported when replicating to premium storage.</span></span> <span data-ttu-id="ac974-112">Begränsningen bestäms av antalet ögonblicksbilder per blob (100) som stöds av premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="ac974-112">The limitation is determined by the number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="ac974-113">Läs mer</span><span class="sxs-lookup"><span data-stu-id="ac974-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="ac974-114">I **Återställningspunkt för kvarhållning** anger du kvarhållningsperioden i antal timmar för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="ac974-114">In **Recovery point retention**, specify in hours how long the retention window will be for each recovery point.</span></span> <span data-ttu-id="ac974-115">Skyddade datorer kan återställas till valfri punkt inom en period.</span><span class="sxs-lookup"><span data-stu-id="ac974-115">Protected machines can be recovered to any point within a window.</span></span>
5. <span data-ttu-id="ac974-116">I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (1–12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="ac974-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="ac974-117">Hyper-V använder två typer av ögonblicksbilder: en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en ögonblicksbild vid en viss tidpunkt av programdata på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ac974-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of the entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of the application data inside the virtual machine.</span></span> <span data-ttu-id="ac974-118">Programkonsekventa ögonblicksbilder använda VSS (Volume Shadow Copy Service) för att säkerställa att programmen är i ett konsekvent tillstånd när ögonblicksbilden tas.</span><span class="sxs-lookup"><span data-stu-id="ac974-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) to ensure that applications are in a consistent state when the snapshot is taken.</span></span> <span data-ttu-id="ac974-119">Observera att om du aktiverar programkonsekventa ögonblicksbilder så påverkar detta prestanda för program som körs på virtuella källdatorer.</span><span class="sxs-lookup"><span data-stu-id="ac974-119">Note that if you enable application-consistent snapshots, it will affect the performance of applications running on source virtual machines.</span></span> <span data-ttu-id="ac974-120">Kontrollera att värdet som du anger är mindre än antalet ytterligare återställningspunkter som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="ac974-120">Ensure that the value you set is less than the number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="ac974-121">I **Starttid för inledande replikering** anger du när den inledande replikeringen ska börja.</span><span class="sxs-lookup"><span data-stu-id="ac974-121">In **Initial replication start time**, indicate when to start the initial replication.</span></span> <span data-ttu-id="ac974-122">Replikeringen sker via Internetbandbredden så du kanske vill schemalägga den utanför kontorstid.</span><span class="sxs-lookup"><span data-stu-id="ac974-122">The replication occurs over your internet bandwidth so you might want to schedule it outside your busy hours.</span></span>
7. <span data-ttu-id="ac974-123">I **Kryptera data lagrade på Azure** anger du om du vill kryptera vilande data i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="ac974-123">In **Encrypt data stored on Azure**, specify whether to encrypt at rest data in Azure storage.</span></span> <span data-ttu-id="ac974-124">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac974-124">Then click **OK**.</span></span>

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="ac974-126">När du skapar en ny princip associeras den automatiskt med VMM-molnet.</span><span class="sxs-lookup"><span data-stu-id="ac974-126">When you create a new policy it's automatically associated with the VMM cloud.</span></span> <span data-ttu-id="ac974-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac974-127">Click **OK**.</span></span> <span data-ttu-id="ac974-128">Du kan associera ytterligare VMM-moln (och de virtuella datorerna i dem) med den här replikeringsprincipen i **Replikering** > principnamn > **Associera VMM-moln**.</span><span class="sxs-lookup"><span data-stu-id="ac974-128">You can associate additional VMM clouds (and the VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="ac974-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac974-130">Next steps</span></span>

<span data-ttu-id="ac974-131">Gå till [steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="ac974-131">Go to [Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
