---
title: "Konfigurera en replikeringsprincip för fysisk serverreplikering till Azure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste ställa in en replikeringsprincip vid replikering av lokala fysiska servrar till Azure-lagring med hjälp av Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: 9ce23382001b54e7b9b7a58b8dd9aa61b400826d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-to-azure"></a><span data-ttu-id="5fcd2-103">Steg 8: Skapa en replikeringsprincip för fysisk serverreplikering till Azure</span><span class="sxs-lookup"><span data-stu-id="5fcd2-103">Step 8: Set up a replication policy for physical server replication to Azure</span></span>


<span data-ttu-id="5fcd2-104">Den här artikeln beskriver hur du ställer in en replikeringsprincip när du replikerar fysiska Windows-/ Linux-servrar till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-104">This article describes how to set up a replication policy when you're replicating Windows/Linux physical servers to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="5fcd2-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5fcd2-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="5fcd2-106">Konfigurera en princip</span><span class="sxs-lookup"><span data-stu-id="5fcd2-106">Configure a policy</span></span>

1. <span data-ttu-id="5fcd2-107">Klicka på **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-107">Click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="5fcd2-108">I **skapa replikeringsprincip**, ange ett principnamn.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-108">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="5fcd2-109">I **tröskelvärdet för RPO** anger du RPO (mål för återställningspunkt)-gränsen.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-109">In **RPO threshold**, specify the RPO limit.</span></span> <span data-ttu-id="5fcd2-110">Det här värdet anger hur ofta data återställningspunkter skapas.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-110">This value indicates how often data recovery points are created.</span></span> <span data-ttu-id="5fcd2-111">En avisering genereras om kontinuerlig replikering överskrider den gränsen.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-111">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="5fcd2-112">I **kvarhållningstid för återställningspunkten**, ange (i timmar) hur lång kvarhållningsperiod är för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-112">In **Recovery point retention**, specify (in hours) how long the retention window is for each recovery point.</span></span> <span data-ttu-id="5fcd2-113">Replikerade virtuella datorer kan återställas till valfri punkt i ett fönster.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-113">Replicated VMs can be recovered to any point in a window.</span></span> <span data-ttu-id="5fcd2-114">Upp till 24 timmar kvarhållning har stöd för datorer som replikeras till premium-lagring och 72 timmar för standardlagring.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-114">Up to 24 hours retention is supported for machines replicated to premium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="5fcd2-115">I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-115">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="5fcd2-116">Klicka på **OK** vill skapa principen.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-116">Click **OK** to create the policy.</span></span>

    ![Replikeringsprincip](./media/physical-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="5fcd2-118">När du skapar en ny princip associeras den automatiskt med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-118">When you create a new policy it's automatically associated with the configuration server.</span></span> <span data-ttu-id="5fcd2-119">Som standard skapas automatiskt en matchande princip för återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-119">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="5fcd2-120">Om exempelvis replikeringsprinciper är **rep-policy** sedan principen för återställning kommer att **rep-princip-återställning**.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-120">For example, if the replication policy is **rep-policy** then the failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="5fcd2-121">Den här principen används inte förrän du har initierat en återställning från Azure.</span><span class="sxs-lookup"><span data-stu-id="5fcd2-121">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5fcd2-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5fcd2-122">Next steps</span></span>

<span data-ttu-id="5fcd2-123">Gå till [steg 9: Installera mobilitetstjänsten](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="5fcd2-123">Go to [Step 9: Install the Mobility service](physical-walkthrough-install-mobility.md)</span></span>
