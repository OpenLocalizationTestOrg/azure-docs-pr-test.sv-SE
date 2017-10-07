---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooAzure (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av Hyper-V VMs tooAzure lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a><span data-ttu-id="f8c61-103">Steg 8: Ställ in hello källa och mål för tooAzure för Hyper-V-replikering</span><span class="sxs-lookup"><span data-stu-id="f8c61-103">Step 8: Set up hello source and target for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="f8c61-104">Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt Hyper-V virtuella datorer (utan VMM för System Center) tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8c61-104">This article describes how tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines (without System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="f8c61-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f8c61-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="f8c61-106">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="f8c61-106">Set up hello source environment</span></span>

<span data-ttu-id="f8c61-107">Ställ in hello Hyper-V-platsen, installera hello Azure Site Recovery-providern och hello Azure Recovery Services-agenten på Hyper-V-värdar och registrera hello-webbplats i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="f8c61-107">Set up hello Hyper-V site, install hello Azure Site Recovery Provider and hello Azure Recovery Services agent on Hyper-V hosts, and register hello site in hello vault.</span></span>

1. <span data-ttu-id="f8c61-108">I **Förbered infrastrukturen**, klickar du på **källa**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-108">In **Prepare Infrastructure**, click **Source**.</span></span> <span data-ttu-id="f8c61-109">tooadd en ny Hyper-V-plats som en behållare för dina Hyper-V-värdar eller kluster, klicka på **+ Hyper-V-platsen**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-109">tooadd a new Hyper-V site as a container for your Hyper-V hosts or clusters, click **+Hyper-V Site**.</span></span>

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. <span data-ttu-id="f8c61-111">I **skapa Hyper-V-platsen**, ange ett namn för hello-platsen.</span><span class="sxs-lookup"><span data-stu-id="f8c61-111">In **Create Hyper-V site**, specify a name for hello site.</span></span> <span data-ttu-id="f8c61-112">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-112">Then click **OK**.</span></span> <span data-ttu-id="f8c61-113">Nu väljer hello plats du har skapat och klicka på **+ Hyper-V Server** tooadd en server toohello plats.</span><span class="sxs-lookup"><span data-stu-id="f8c61-113">Now, select hello site you created, and click **+Hyper-V Server** tooadd a server toohello site.</span></span>

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="f8c61-115">I **Lägg till Server** > **servertyp**, kontrollera att **Hyper-V server** visas.</span><span class="sxs-lookup"><span data-stu-id="f8c61-115">In **Add Server** > **Server type**, check that **Hyper-V server** is displayed.</span></span>

    - <span data-ttu-id="f8c61-116">Kontrollera att hello Hyper-V-server som du vill tooadd uppfyller hello [krav](#on-premises-prerequisites), och är kan tooaccess hello angivna URL: er.</span><span class="sxs-lookup"><span data-stu-id="f8c61-116">Make sure that hello Hyper-V server you want tooadd complies with hello [prerequisites](#on-premises-prerequisites), and is able tooaccess hello specified URLs.</span></span>
    - <span data-ttu-id="f8c61-117">Hämta installationsfilen för hello Azure Site Recovery-providern.</span><span class="sxs-lookup"><span data-stu-id="f8c61-117">Download hello Azure Site Recovery Provider installation file.</span></span> <span data-ttu-id="f8c61-118">Du kör den här filen tooinstall hello providern och hello Recovery Services-agenten på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="f8c61-118">You run this file tooinstall hello Provider and hello Recovery Services agent on each Hyper-V host.</span></span>

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a><span data-ttu-id="f8c61-120">Installera hello Provider och agent</span><span class="sxs-lookup"><span data-stu-id="f8c61-120">Install hello Provider and agent</span></span>

1. <span data-ttu-id="f8c61-121">Kör installationsfilen för hello-providern på varje värd du lagt till toohello Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="f8c61-121">Run hello Provider setup file on each host you added toohello Hyper-V site.</span></span> <span data-ttu-id="f8c61-122">Om du installerar på en Hyper-V-kluster, kan du köra installationsprogrammet på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="f8c61-122">If you're installing on a Hyper-V cluster, run setup on each cluster node.</span></span> <span data-ttu-id="f8c61-123">Installera och registrera varje klusternod för Hyper-V säkerställer att virtuella datorer är skyddade, även om de migrera mellan noder.</span><span class="sxs-lookup"><span data-stu-id="f8c61-123">Installing and registering each Hyper-V cluster node ensures that VMs are protected, even if they migrate across nodes.</span></span>
2. <span data-ttu-id="f8c61-124">I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.</span><span class="sxs-lookup"><span data-stu-id="f8c61-124">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="f8c61-125">I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-125">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>
4. <span data-ttu-id="f8c61-126">I **Valvinställningar**, klickar du på **Bläddra** tooselect hello valvnyckelfilen som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="f8c61-126">In **Vault Settings**, click **Browse** tooselect hello vault key file that you downloaded.</span></span> <span data-ttu-id="f8c61-127">Ange hello Azure Site Recovery-prenumerationen, hello valvnamnet och hello Hyper-V platsservern toowhich hello Hyper-V tillhör.</span><span class="sxs-lookup"><span data-stu-id="f8c61-127">Specify hello Azure Site Recovery subscription, hello vault name, and hello Hyper-V site toowhich hello Hyper-V server belongs.</span></span>

    ![Serverregistrering](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. <span data-ttu-id="f8c61-129">I **proxyinställningar**, ange hur providern som körs på Hyper-V-värdar som ansluter tooAzure Site Recovery via hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="f8c61-129">In **Proxy Settings**, specify how hello Provider running on Hyper-V hosts connects tooAzure Site Recovery over hello internet.</span></span>

    * <span data-ttu-id="f8c61-130">Om du vill att hello providern tooconnect direkt väljer **ansluter direkt tooAzure Site Recovery utan en proxy**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-130">If you want hello Provider tooconnect directly select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
    * <span data-ttu-id="f8c61-131">Om den befintliga proxyservern kräver autentisering eller om du vill använda toouse en anpassad proxyserver för hello leverantörsanslutning väljer **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-131">If your existing proxy requires authentication, or you want toouse a custom proxy for hello Provider connection, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
    * <span data-ttu-id="f8c61-132">Om du använder en proxyserver:</span><span class="sxs-lookup"><span data-stu-id="f8c61-132">If you use a proxy:</span></span>
        - <span data-ttu-id="f8c61-133">Ange hello-adress, port och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f8c61-133">Specify hello address, port, and credentials</span></span>
        - <span data-ttu-id="f8c61-134">Kontrollera att hello webbadresser som beskrivs i hello [krav](#prerequisites) tillåts via hello proxy.</span><span class="sxs-lookup"><span data-stu-id="f8c61-134">Make sure hello URLs described in hello [prerequisites](#prerequisites) are allowed through hello proxy.</span></span>

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. <span data-ttu-id="f8c61-136">När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="f8c61-136">After installation finishes, click **Register** tooregister hello server in hello vault.</span></span>

    ![Installationsplats](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. <span data-ttu-id="f8c61-138">När registreringen är klar, hämtas metadata från hello Hyper-V-servern av Azure Site Recovery och hello server visas i **Site Recovery-infrastruktur** > **Hyper-V-värdar**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-138">After registration finishes, metadata from hello Hyper-V server is retrieved by Azure Site Recovery, and hello server is displayed in **Site Recovery Infrastructure** > **Hyper-V Hosts**.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="f8c61-139">Ställ in hello målmiljön</span><span class="sxs-lookup"><span data-stu-id="f8c61-139">Set up hello target environment</span></span>

<span data-ttu-id="f8c61-140">Ange hello Azure storage-konto för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.</span><span class="sxs-lookup"><span data-stu-id="f8c61-140">Specify hello Azure storage account for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="f8c61-141">Klicka på **Förbered infrastruktur** > **mål**.</span><span class="sxs-lookup"><span data-stu-id="f8c61-141">Click **Prepare infrastructure** > **Target**.</span></span>
2. <span data-ttu-id="f8c61-142">Välj hello prenumeration och hello resursgrupp som du vill toocreate hello virtuella Azure-datorer efter redundans.</span><span class="sxs-lookup"><span data-stu-id="f8c61-142">Select hello subscription and hello resource group in which you want toocreate hello Azure VMs after failover.</span></span> <span data-ttu-id="f8c61-143">Välj hello distribution modell som du vill toouse i Azure (klassiska eller resource management) för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f8c61-143">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello VMs.</span></span>

3. <span data-ttu-id="f8c61-144">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f8c61-144">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    - <span data-ttu-id="f8c61-145">Om du inte har ett lagringskonto, klickar du på **+ lagring** toocreate en Resource Manager-baserade konto infogad.</span><span class="sxs-lookup"><span data-stu-id="f8c61-145">If you don't have a storage account, click **+Storage** toocreate a Resource Manager-based account inline.</span></span> 
    - <span data-ttu-id="f8c61-146">Om du inte har ett Azure-nätverk, klickar du på **+ nätverk** toocreate en infogad Resource Manager-baserat nätverk.</span><span class="sxs-lookup"><span data-stu-id="f8c61-146">If you don't have a Azure network, click **+Network** toocreate a Resource Manager-based network inline.</span></span>






## <a name="next-steps"></a><span data-ttu-id="f8c61-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8c61-147">Next steps</span></span>

<span data-ttu-id="f8c61-148">Gå för[steg 9: Konfigurera en replikeringsprincip](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f8c61-148">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>
