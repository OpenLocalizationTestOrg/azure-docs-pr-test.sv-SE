---
title: "aaaSet hello källa och mål för fysisk server replication tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av fysiska servrar tooAzure lagring med hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a><span data-ttu-id="62d51-103">Steg 7: Konfigurera hello källa och mål för fysisk server replication tooAzure</span><span class="sxs-lookup"><span data-stu-id="62d51-103">Step 7: Set up hello source and target for physical server replication tooAzure</span></span>

<span data-ttu-id="62d51-104">Den här artikeln beskriver hur tooconfigure käll- och inställningar vid replikering av lokala fysiska servrar tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62d51-104">This article describes how tooconfigure source and target settings when replicating on-premises physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="62d51-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="62d51-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="62d51-106">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="62d51-106">Set up hello source environment</span></span>

<span data-ttu-id="62d51-107">Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera datorer.</span><span class="sxs-lookup"><span data-stu-id="62d51-107">Set up hello configuration server, register it in hello vault, and discover machines.</span></span>

1. <span data-ttu-id="62d51-108">Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="62d51-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="62d51-109">Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.</span><span class="sxs-lookup"><span data-stu-id="62d51-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="62d51-110">I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="62d51-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="62d51-111">Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="62d51-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="62d51-112">Hämta hello valvregistreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="62d51-112">Download hello vault registration key.</span></span> <span data-ttu-id="62d51-113">Du behöver detta när du kör installationsprogrammet för enhetlig.</span><span class="sxs-lookup"><span data-stu-id="62d51-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="62d51-114">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="62d51-114">hello key is valid for five days after you generate it.</span></span>

   ![Konfigurera källan](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="62d51-116">Registrera hello konfigurationsservern i hello-valv</span><span class="sxs-lookup"><span data-stu-id="62d51-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="62d51-117">Hello följande innan du startar och kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern hello processervern och hello huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="62d51-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span> <span data-ttu-id="62d51-118">hello videon Beskriver inställning av hello komponenter för VMware VM tooAzure replikering.</span><span class="sxs-lookup"><span data-stu-id="62d51-118">hello video describes setting up hello components for VMware VM tooAzure replication.</span></span> <span data-ttu-id="62d51-119">Dock är hello samma process giltig för fysisk server tooAzure replikering.</span><span class="sxs-lookup"><span data-stu-id="62d51-119">However, hello same process is valid for physical server tooAzure replication.</span></span>

- <span data-ttu-id="62d51-120">Kontrollera att hello systemklockan är synkroniserad med i hello konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="62d51-120">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="62d51-121">Den måste matcha.</span><span class="sxs-lookup"><span data-stu-id="62d51-121">It should match.</span></span> <span data-ttu-id="62d51-122">Installationen misslyckas om det är 15 minuter framför eller bakom.</span><span class="sxs-lookup"><span data-stu-id="62d51-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="62d51-123">Kör installationsprogrammet som en lokal administratör på hello configuration server-datorn.</span><span class="sxs-lookup"><span data-stu-id="62d51-123">Run setup as a Local Administrator on hello configuration server machine.</span></span>
- <span data-ttu-id="62d51-124">Kontrollera att TLS 1.0 är aktiverat på hello VM.</span><span class="sxs-lookup"><span data-stu-id="62d51-124">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="62d51-125">hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="62d51-125">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-hello-target-environment"></a><span data-ttu-id="62d51-126">Ställ in hello målmiljön</span><span class="sxs-lookup"><span data-stu-id="62d51-126">Set up hello target environment</span></span>

<span data-ttu-id="62d51-127">Innan du konfigurerar hello målmiljön, kontrollera att du har ett Azure storage-konto och virtuella nätverk som har upprättats.</span><span class="sxs-lookup"><span data-stu-id="62d51-127">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="62d51-128">Klicka på **Förbered infrastruktur** > **mål**, och välj hello Azure-prenumeration du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="62d51-128">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="62d51-129">Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.</span><span class="sxs-lookup"><span data-stu-id="62d51-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="62d51-130">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="62d51-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![mål](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="62d51-132">Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, toocreate en Resource Manager-konto eller nätverket infogad.</span><span class="sxs-lookup"><span data-stu-id="62d51-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62d51-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62d51-133">Next steps</span></span>

<span data-ttu-id="62d51-134">Gå för[steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="62d51-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
