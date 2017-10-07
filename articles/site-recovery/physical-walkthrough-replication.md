---
title: "aaaSet upp en replikeringsprincip för fysisk server replication tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp en replikeringsprincip när replikera lokala fysiska servrar tooAzure lagring med hjälp av hello Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: d6bdeeffc24ffc8eaba24311f7c76452edb65648
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-tooazure"></a><span data-ttu-id="24eeb-103">Steg 8: Skapa en replikeringsprincip för fysisk server replication tooAzure</span><span class="sxs-lookup"><span data-stu-id="24eeb-103">Step 8: Set up a replication policy for physical server replication tooAzure</span></span>


<span data-ttu-id="24eeb-104">Den här artikeln beskriver hur tooset upp en replikeringsprincip när du replikerar Windows-/ Linux fysiska servrar tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="24eeb-104">This article describes how tooset up a replication policy when you're replicating Windows/Linux physical servers tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="24eeb-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="24eeb-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="24eeb-106">Konfigurera en princip</span><span class="sxs-lookup"><span data-stu-id="24eeb-106">Configure a policy</span></span>

1. <span data-ttu-id="24eeb-107">Klicka på **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="24eeb-107">Click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="24eeb-108">I **skapa replikeringsprincip**, ange ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="24eeb-108">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="24eeb-109">I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen.</span><span class="sxs-lookup"><span data-stu-id="24eeb-109">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="24eeb-110">Det här värdet anger hur ofta data återställningspunkter skapas.</span><span class="sxs-lookup"><span data-stu-id="24eeb-110">This value indicates how often data recovery points are created.</span></span> <span data-ttu-id="24eeb-111">En avisering genereras om kontinuerlig replikering överskrider den gränsen.</span><span class="sxs-lookup"><span data-stu-id="24eeb-111">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="24eeb-112">I **kvarhållningstid för återställningspunkten**, ange hur länge (i timmar) hello kvarhållningsperiod är för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="24eeb-112">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="24eeb-113">Replikerade virtuella datorer kan återställas tooany punkt i ett fönster.</span><span class="sxs-lookup"><span data-stu-id="24eeb-113">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="24eeb-114">Replikerade toopremium lagring och 72 timmar för standardlagring in too24 timmar kvarhållning har stöd för datorer.</span><span class="sxs-lookup"><span data-stu-id="24eeb-114">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="24eeb-115">I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="24eeb-115">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="24eeb-116">Klicka på **OK** toocreate hello princip.</span><span class="sxs-lookup"><span data-stu-id="24eeb-116">Click **OK** toocreate hello policy.</span></span>

    ![Replikeringsprincip](./media/physical-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="24eeb-118">När du skapar en ny princip associeras den automatiskt med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="24eeb-118">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="24eeb-119">Som standard skapas automatiskt en matchande princip för återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="24eeb-119">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="24eeb-120">Till exempel om hello replikeringsprincip är **rep princip** hello principer ska vara **rep-princip-återställning**.</span><span class="sxs-lookup"><span data-stu-id="24eeb-120">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="24eeb-121">Den här principen används inte förrän du har initierat en återställning från Azure.</span><span class="sxs-lookup"><span data-stu-id="24eeb-121">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24eeb-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24eeb-122">Next steps</span></span>

<span data-ttu-id="24eeb-123">Gå för[steg 9: Installera hello mobilitetstjänsten](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="24eeb-123">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>
