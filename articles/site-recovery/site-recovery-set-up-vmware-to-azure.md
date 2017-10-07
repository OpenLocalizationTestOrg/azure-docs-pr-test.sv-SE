---
title: "Ställ in hello källmiljön (VMware tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera VMware virtuella datorer tooAzure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a><span data-ttu-id="96859-103">Ställ in hello källmiljön (VMware tooAzure)</span><span class="sxs-lookup"><span data-stu-id="96859-103">Set up hello source environment (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="96859-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="96859-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="96859-105">Fysisk tooAzure</span><span class="sxs-lookup"><span data-stu-id="96859-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="96859-106">Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera virtuella datorer som körs på VMware tooAzure.</span><span class="sxs-lookup"><span data-stu-id="96859-106">This article describes how tooset up your on-premises environment toostart replicating virtual machines running on VMware tooAzure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96859-107">Krav</span><span class="sxs-lookup"><span data-stu-id="96859-107">Prerequisites</span></span>

<span data-ttu-id="96859-108">hello artikeln förutsätter att du redan har skapat:</span><span class="sxs-lookup"><span data-stu-id="96859-108">hello article assumes that you have already created:</span></span>
- <span data-ttu-id="96859-109">Recovery Services-ventilen i hello [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="96859-109">A Recovery Services Vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="96859-110">Ett särskilt konto i din VMware vCenter som kan användas för [automatisk identifiering](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="96859-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="96859-111">En virtuell dator på vilken tooinstall hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="96859-111">A virtual machine on which tooinstall hello configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="96859-112">Minimikrav för konfiguration av servern</span><span class="sxs-lookup"><span data-stu-id="96859-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="96859-113">hello configuration server-program ska distribueras på en virtuell dator för hög tillgänglighet VMware.</span><span class="sxs-lookup"><span data-stu-id="96859-113">hello configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="96859-114">hello i den följande tabellen listas hello minimikraven för maskinvara, programvara och nätverkskraven för en konfigurationsserver.</span><span class="sxs-lookup"><span data-stu-id="96859-114">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="96859-115">HTTPS-baserade proxyservrar stöds inte av hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="96859-115">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="96859-116">Välja skyddsmål</span><span class="sxs-lookup"><span data-stu-id="96859-116">Choose your protection goals</span></span>

1. <span data-ttu-id="96859-117">I hello Azure-portalen, går toohello **återställningstjänster** valvet bladet och välj ditt valv.</span><span class="sxs-lookup"><span data-stu-id="96859-117">In hello Azure portal, go toohello **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="96859-118">Hello resurs menyn hello valvet, gå för**komma igång** > **Site Recovery** > **steg 1: Förbered infrastrukturen**  >  **Skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="96859-118">On hello resource menu of hello vault, go too**Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="96859-120">I **skyddsmål**väljer **tooAzure**, och välj **Ja, med VMware vSphere-Hypervisor**.</span><span class="sxs-lookup"><span data-stu-id="96859-120">In **Protection goal**, select **tooAzure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="96859-121">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="96859-121">Then click **OK**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="96859-123">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="96859-123">Set up hello source environment</span></span>
<span data-ttu-id="96859-124">Konfigurera hello källmiljön omfattar två huvudsakliga aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="96859-124">Setting up hello source environment involves two main activities:</span></span>

- <span data-ttu-id="96859-125">Installera och registrera en konfigurationsserver med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="96859-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="96859-126">Identifiera dina lokala virtuella datorer genom att ansluta Site Recovery tooyour lokal VMware vCenter eller vSphere EXSi värdar.</span><span class="sxs-lookup"><span data-stu-id="96859-126">Discover your on-premises virtual machines by connecting Site Recovery tooyour on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="96859-127">Steg 1: Installera och registrera en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="96859-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="96859-128">Klicka på **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="96859-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="96859-129">I **Förbered källa**, om du inte har en konfigurationsservern och klicka på **+ konfigurationsservern** tooadd en.</span><span class="sxs-lookup"><span data-stu-id="96859-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="96859-131">På hello **Lägg till Server** bladet, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="96859-131">On hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="96859-132">Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="96859-132">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="96859-133">Hämta hello valvregistreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="96859-133">Download hello vault registration key.</span></span> <span data-ttu-id="96859-134">Du behöver hello registreringsnyckel när du kör installationsprogrammet för enhetlig.</span><span class="sxs-lookup"><span data-stu-id="96859-134">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="96859-135">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="96859-135">hello key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="96859-137">Kör på hello-dator som du använder som hello konfigurationsservern **Unified installationsprogram för Azure Site Recovery** tooinstall hello konfigurationsservern, hello processervern och hello master mål-servern.</span><span class="sxs-lookup"><span data-stu-id="96859-137">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="96859-138">Kör Azure Site Recovery enhetlig installation</span><span class="sxs-lookup"><span data-stu-id="96859-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="96859-139">Registrera för konfiguration av servern misslyckas om hello tiden på datorns systemklocka skiljer sig från lokal tid med fler än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="96859-139">Configuration server registration fails if hello time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="96859-140">Synkronisera systemklockan med en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) innan du påbörjar installationen hello.</span><span class="sxs-lookup"><span data-stu-id="96859-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="96859-141">hello konfigurationsservern kan installeras via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="96859-141">hello configuration server can be installed via command line.</span></span> <span data-ttu-id="96859-142">Mer information finns i [installerar hello konfigurationsservern med hjälp av kommandoradsverktyg](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="96859-142">For more information, see [Installing hello configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a><span data-ttu-id="96859-143">Lägg till hello VMware-konto för automatisk identifiering</span><span class="sxs-lookup"><span data-stu-id="96859-143">Add hello VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="96859-144">Steg 2: Lägg till en vCenter</span><span class="sxs-lookup"><span data-stu-id="96859-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="96859-145">tooallow Azure Site Recovery toodiscover virtuella datorer som körs i din lokala miljö, behöver du tooconnect din VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="96859-145">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="96859-146">Välj **+ vCenter** toostart ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.</span><span class="sxs-lookup"><span data-stu-id="96859-146">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="96859-147">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="96859-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="96859-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96859-148">Next steps</span></span>
<span data-ttu-id="96859-149">[Konfigurera målmiljön](./site-recovery-prepare-target-vmware-to-azure.md) i Azure.</span><span class="sxs-lookup"><span data-stu-id="96859-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
