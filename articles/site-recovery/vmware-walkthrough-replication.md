---
title: "Konfigurera en replikeringsprincip för VMware VM replikering till Azure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste ställa in en replikeringsprincip vid replikering av virtuella VMware-datorer till Azure-lagring"
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
ms.openlocfilehash: 3c4b7ad16e6a03fb605447def18f7475d502fdd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-to-azure"></a><span data-ttu-id="23c1b-103">Steg 9: Konfigurera en replikeringsprincip för VMware VM replikering till Azure</span><span class="sxs-lookup"><span data-stu-id="23c1b-103">Step 9: Set up a replication policy for VMware VM replication to Azure</span></span>


<span data-ttu-id="23c1b-104">Den här artikeln beskriver hur du ställer in en replikeringsprincip när du replikerar virtuella VMware-datorer till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="23c1b-104">This article describes how to set up a replication policy when you're replicating VMware VMs to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="23c1b-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="23c1b-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="23c1b-106">Konfigurera en princip</span><span class="sxs-lookup"><span data-stu-id="23c1b-106">Configure a policy</span></span>

<span data-ttu-id="23c1b-107">Få en snabb överblick av video innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="23c1b-107">Get a quick video overview before you start:</span></span>
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. <span data-ttu-id="23c1b-108">Klicka för att skapa en ny replikeringsprincip **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="23c1b-108">To create a new replication policy, click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="23c1b-109">I **skapa replikeringsprincip**, ange ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="23c1b-109">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="23c1b-110">I **tröskelvärdet för RPO** anger du RPO (mål för återställningspunkt)-gränsen.</span><span class="sxs-lookup"><span data-stu-id="23c1b-110">In **RPO threshold**, specify the RPO limit.</span></span> <span data-ttu-id="23c1b-111">Det här värdet anger hur ofta data återställningspunkter skapas.</span><span class="sxs-lookup"><span data-stu-id="23c1b-111">This value specifies how often data recovery points are created.</span></span> <span data-ttu-id="23c1b-112">En avisering genereras om kontinuerlig replikering överskrider den gränsen.</span><span class="sxs-lookup"><span data-stu-id="23c1b-112">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="23c1b-113">I **kvarhållningstid för återställningspunkten**, ange (i timmar) hur lång kvarhållningsperiod är för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="23c1b-113">In **Recovery point retention**, specify (in hours) how long the retention window is for each recovery point.</span></span> <span data-ttu-id="23c1b-114">Replikerade virtuella datorer kan återställas till valfri punkt i ett fönster.</span><span class="sxs-lookup"><span data-stu-id="23c1b-114">Replicated VMs can be recovered to any point in a window.</span></span> <span data-ttu-id="23c1b-115">Upp till 24 timmar kvarhållning har stöd för datorer som replikeras till premium-lagring och 72 timmar för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="23c1b-115">Up to 24 hours retention is supported for machines replicated to premium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="23c1b-116">I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="23c1b-116">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="23c1b-117">Klicka på **OK** vill skapa principen.</span><span class="sxs-lookup"><span data-stu-id="23c1b-117">Click **OK** to create the policy.</span></span>

    ![Replikeringsprincip](./media/vmware-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="23c1b-119">När du skapar en ny princip associeras den automatiskt med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="23c1b-119">When you create a new policy it's automatically associated with the configuration server.</span></span> <span data-ttu-id="23c1b-120">Som standard skapas automatiskt en matchande princip för återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="23c1b-120">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="23c1b-121">Om exempelvis replikeringsprinciper är **rep-policy** sedan principen för återställning kommer att **rep-princip-återställning**.</span><span class="sxs-lookup"><span data-stu-id="23c1b-121">For example, if the replication policy is **rep-policy** then the failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="23c1b-122">Den här principen används inte förrän du har initierat en återställning från Azure.</span><span class="sxs-lookup"><span data-stu-id="23c1b-122">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23c1b-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23c1b-123">Next steps</span></span>

<span data-ttu-id="23c1b-124">Gå till [steg 10: Installera mobilitetstjänsten](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="23c1b-124">Go to [Step 10: Install the Mobility service](vmware-walkthrough-install-mobility.md)</span></span>
