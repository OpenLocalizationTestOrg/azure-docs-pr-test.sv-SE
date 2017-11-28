---
title: "aaaSet in hello källan och målet för VMware replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av virtuella VMware-datorer tooAzure lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a><span data-ttu-id="ed6a6-103">Steg 8: Ställ in hello källan och målet för VMware replikering tooAzure</span><span class="sxs-lookup"><span data-stu-id="ed6a6-103">Step 8: Set up hello source and target for VMware replication tooAzure</span></span>

<span data-ttu-id="ed6a6-104">Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt VMware virtuella datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="ed6a6-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ed6a6-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="ed6a6-106">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="ed6a6-106">Set up hello source environment</span></span>

<span data-ttu-id="ed6a6-107">Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-107">Set up hello configuration server, register it in hello vault, and discover VMs.</span></span>

1. <span data-ttu-id="ed6a6-108">Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="ed6a6-109">Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="ed6a6-110">I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="ed6a6-111">Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="ed6a6-112">Hämta hello valvregistreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-112">Download hello vault registration key.</span></span> <span data-ttu-id="ed6a6-113">Du behöver detta när du kör installationsprogrammet för enhetlig.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="ed6a6-114">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-114">hello key is valid for five days after you generate it.</span></span>

   ![Konfigurera källan](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="ed6a6-116">Registrera hello konfigurationsservern i hello-valv</span><span class="sxs-lookup"><span data-stu-id="ed6a6-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="ed6a6-117">Hello följande innan du startar och kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern hello processervern och hello huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span>
    - <span data-ttu-id="ed6a6-118">Få en snabb överblick av video</span><span class="sxs-lookup"><span data-stu-id="ed6a6-118">Get a quick video overview</span></span>

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - <span data-ttu-id="ed6a6-119">Kontrollera att hello systemklockan är synkroniserad med i hello konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="ed6a6-119">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="ed6a6-120">Den måste matcha.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-120">It should match.</span></span> <span data-ttu-id="ed6a6-121">Installationen misslyckas om det är 15 minuter framför eller bakom.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-121">If it's 15 minutes in front or behind, setup might fail.</span></span>
    - <span data-ttu-id="ed6a6-122">Kör installationsprogrammet som en lokal administratör på hello konfigurationsservern VM.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-122">Run setup as a Local Administrator on hello configuration server VM.</span></span>
    - <span data-ttu-id="ed6a6-123">Kontrollera att TLS 1.0 är aktiverat på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-123">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="ed6a6-124">hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="ed6a6-124">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>



## <a name="connect-toovmware-servers"></a><span data-ttu-id="ed6a6-125">Anslut tooVMware servrar</span><span class="sxs-lookup"><span data-stu-id="ed6a6-125">Connect tooVMware servers</span></span>

<span data-ttu-id="ed6a6-126">tooallow Azure Site Recovery toodiscover virtuella datorer som körs i din lokala miljö, behöver du tooconnect din VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-126">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span> <span data-ttu-id="ed6a6-127">Observera följande hello innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="ed6a6-127">Note hello following before you start:</span></span>

- <span data-ttu-id="ed6a6-128">Om du lägger till hello vCenter-servern eller vSphere värdar tooSite återställning med ett konto utan administratörsbehörighet på servern hello måste hello kontot dessa behörigheter aktiveras:</span><span class="sxs-lookup"><span data-stu-id="ed6a6-128">If you add hello vCenter server or vSphere hosts tooSite Recovery with an account without administrator privileges on hello server, hello account needs these privileges enabled:</span></span>
    - <span data-ttu-id="ed6a6-129">Datacenter, datalagret, mapp, värd, nätverk, resurs, virtuella datorn vSphere distribuerade växel.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-129">Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, vSphere Distributed Switch.</span></span>
    - <span data-ttu-id="ed6a6-130">Hej vCenter-servern måste lagring vyer behörigheter.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-130">hello vCenter server needs Storage views permissions.</span></span>
- <span data-ttu-id="ed6a6-131">När du lägger till VMware-servrar tooSite återställning kan det ta 15 minuter eller längre för dessa tooappear hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-131">When you add VMware servers tooSite Recovery, it can take 15 minutes or longer for them tooappear in hello portal.</span></span>

### <a name="add-hello-account-for-automatic-discovery"></a><span data-ttu-id="ed6a6-132">Lägg till hello konto för automatisk identifiering</span><span class="sxs-lookup"><span data-stu-id="ed6a6-132">Add hello account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a><span data-ttu-id="ed6a6-133">Upprätta en anslutning</span><span class="sxs-lookup"><span data-stu-id="ed6a6-133">Set up a connection</span></span>

<span data-ttu-id="ed6a6-134">Anslut tooservers på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ed6a6-134">Connect tooservers as follows:</span></span>

1. <span data-ttu-id="ed6a6-135">Välj **+ vCenter** toostart ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-135">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>
2. <span data-ttu-id="ed6a6-136">I **lägga till vCenter**, ange ett eget namn för hello vSphere-värd eller vCenter server och sedan ange hello IP-adress eller FQDN för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-136">In **Add vCenter**, specify a friendly name for hello vSphere host or vCenter server, and then specify hello IP address or FQDN of hello server.</span></span>
3. <span data-ttu-id="ed6a6-137">Lämna hello port som 443 om VMware-servrar är konfigurerade toolisten för begäranden på en annan port.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-137">Leave hello port as 443 unless your VMware servers are configured toolisten for requests on a different port.</span></span> <span data-ttu-id="ed6a6-138">Välj hello-konto som är tooconnect toohello VMware vCenter eller vSphere ESXi-servern.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-138">Select hello account that is tooconnect toohello VMware vCenter or vSphere ESXi server.</span></span> <span data-ttu-id="ed6a6-139">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-139">Click **OK**.</span></span>
4. <span data-ttu-id="ed6a6-140">Site Recovery ansluter tooVMware servrar med hjälp av hello angett inställningar och identifierar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-140">Site Recovery connects tooVMware servers using hello specified settings, and discovers VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="ed6a6-141">Om du vill lägga till en server eller en värd med ett konto som inte har administratörsbehörighet på hello vCenter eller värden, kontrollera att hello-tjänstkontot har dessa behörigheter aktiveras: Datacenter, datalagret, mapp, värd, nätverk, resurs, virtuell dator, och vSphere distribuerade växel.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-141">If you're adding a server or host with an account that doesn't have administrator privileges on hello vCenter or host server, make sure that hello account has these privileges enabled: Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, and vSphere Distributed Switch.</span></span> <span data-ttu-id="ed6a6-142">Hello VMware vCenter-servern måste dessutom hello lagring vyer privilegiet aktiverat.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-142">In addition, hello VMware vCenter server needs hello Storage Views privilege enabled.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="ed6a6-143">Ställ in hello målmiljön</span><span class="sxs-lookup"><span data-stu-id="ed6a6-143">Set up hello target environment</span></span>

<span data-ttu-id="ed6a6-144">Innan du konfigurerar hello målmiljön, kontrollera att du har ett Azure storage-konto och virtuella nätverk som har upprättats.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-144">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="ed6a6-145">Klicka på **Förbered infrastruktur** > **mål**, och välj hello Azure-prenumeration du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-145">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="ed6a6-146">Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-146">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="ed6a6-147">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-147">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![mål](./media/vmware-walkthrough-source-target/gs-target.png)
4. <span data-ttu-id="ed6a6-149">Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, toocreate en Resource Manager-konto eller nätverket infogad.</span><span class="sxs-lookup"><span data-stu-id="ed6a6-149">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed6a6-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed6a6-150">Next steps</span></span>

<span data-ttu-id="ed6a6-151">Gå för[steg 9: Konfigurera en replikeringsprincip](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="ed6a6-151">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>
