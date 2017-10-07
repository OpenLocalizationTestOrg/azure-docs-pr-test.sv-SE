---
title: "aaaSet upp en replikeringsprincip för VMware VM replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: Sammanfattar hello stegen tooset upp en replikeringsprincip vid replikering av virtuella VMware-datorer tooAzure lagring
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 12870f3f98a4dd412221b817581403cd4bf7a76d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-tooazure"></a><span data-ttu-id="f399a-103">Steg 9: Konfigurera en replikeringsprincip för VMware VM replikering tooAzure</span><span class="sxs-lookup"><span data-stu-id="f399a-103">Step 9: Set up a replication policy for VMware VM replication tooAzure</span></span>


<span data-ttu-id="f399a-104">Den här artikeln beskriver hur tooset upp en replikeringsprincip när du replikera virtuella VMware-datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f399a-104">This article describes how tooset up a replication policy when you're replicating VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="f399a-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f399a-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="f399a-106">Konfigurera en princip</span><span class="sxs-lookup"><span data-stu-id="f399a-106">Configure a policy</span></span>

<span data-ttu-id="f399a-107">Få en snabb överblick av video innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="f399a-107">Get a quick video overview before you start:</span></span>
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. <span data-ttu-id="f399a-108">Klicka på toocreate en ny replikeringsprincip **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="f399a-108">toocreate a new replication policy, click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="f399a-109">I **skapa replikeringsprincip**, ange ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="f399a-109">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="f399a-110">I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen.</span><span class="sxs-lookup"><span data-stu-id="f399a-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="f399a-111">Det här värdet anger hur ofta data återställningspunkter skapas.</span><span class="sxs-lookup"><span data-stu-id="f399a-111">This value specifies how often data recovery points are created.</span></span> <span data-ttu-id="f399a-112">En avisering genereras om kontinuerlig replikering överskrider den gränsen.</span><span class="sxs-lookup"><span data-stu-id="f399a-112">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="f399a-113">I **kvarhållningstid för återställningspunkten**, ange hur länge (i timmar) hello kvarhållningsperiod är för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="f399a-113">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="f399a-114">Replikerade virtuella datorer kan återställas tooany punkt i ett fönster.</span><span class="sxs-lookup"><span data-stu-id="f399a-114">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="f399a-115">Replikerade toopremium lagring och 72 timmar för standardlagring in too24 timmar kvarhållning har stöd för datorer.</span><span class="sxs-lookup"><span data-stu-id="f399a-115">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="f399a-116">I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="f399a-116">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="f399a-117">Klicka på **OK** toocreate hello princip.</span><span class="sxs-lookup"><span data-stu-id="f399a-117">Click **OK** toocreate hello policy.</span></span>

    ![Replikeringsprincip](./media/vmware-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="f399a-119">När du skapar en ny princip associeras den automatiskt med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="f399a-119">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="f399a-120">Som standard skapas automatiskt en matchande princip för återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="f399a-120">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="f399a-121">Till exempel om hello replikeringsprincip är **rep princip** hello principer ska vara **rep-princip-återställning**.</span><span class="sxs-lookup"><span data-stu-id="f399a-121">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="f399a-122">Den här principen används inte förrän du har initierat en återställning från Azure.</span><span class="sxs-lookup"><span data-stu-id="f399a-122">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f399a-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f399a-123">Next steps</span></span>

<span data-ttu-id="f399a-124">Gå för[steg 10: Installera hello mobilitetstjänsten](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="f399a-124">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>
