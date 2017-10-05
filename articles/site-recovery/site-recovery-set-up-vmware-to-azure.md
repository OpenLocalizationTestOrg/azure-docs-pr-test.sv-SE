---
title: "Konfigurera källmiljön (VMware till Azure) | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in din lokala miljö för att starta replikera virtuella VMware-datorer till Azure."
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
ms.openlocfilehash: a2fabc56463c8cbf0b8a76b7a84369ed8e535486
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a><span data-ttu-id="c7d41-103">Konfigurera källmiljön (VMware till Azure)</span><span class="sxs-lookup"><span data-stu-id="c7d41-103">Set up the source environment (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7d41-104">VMware till Azure</span><span class="sxs-lookup"><span data-stu-id="c7d41-104">VMware to Azure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="c7d41-105">Fysiska till Azure</span><span class="sxs-lookup"><span data-stu-id="c7d41-105">Physical to Azure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="c7d41-106">Den här artikeln beskriver hur du ställer in din lokala miljö för att starta replikering av virtuella datorer som körs på VMware till Azure.</span><span class="sxs-lookup"><span data-stu-id="c7d41-106">This article describes how to set up your on-premises environment to start replicating virtual machines running on VMware to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7d41-107">Krav</span><span class="sxs-lookup"><span data-stu-id="c7d41-107">Prerequisites</span></span>

<span data-ttu-id="c7d41-108">Artikeln förutsätter att du redan har skapat:</span><span class="sxs-lookup"><span data-stu-id="c7d41-108">The article assumes that you have already created:</span></span>
- <span data-ttu-id="c7d41-109">Recovery Services-ventilen i den [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="c7d41-109">A Recovery Services Vault in the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="c7d41-110">Ett särskilt konto i din VMware vCenter som kan användas för [automatisk identifiering](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c7d41-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="c7d41-111">En virtuell dator som du vill installera konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="c7d41-111">A virtual machine on which to install the configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="c7d41-112">Minimikrav för konfiguration av servern</span><span class="sxs-lookup"><span data-stu-id="c7d41-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="c7d41-113">Configuration server-programvara ska distribueras på en virtuell dator för hög tillgänglighet VMware.</span><span class="sxs-lookup"><span data-stu-id="c7d41-113">The configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="c7d41-114">I följande tabell visar minimikrav för maskinvara, programvara och nätverkskraven för en konfigurationsserver.</span><span class="sxs-lookup"><span data-stu-id="c7d41-114">The following table lists the minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="c7d41-115">HTTPS-baserade proxyservrar stöds inte av konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="c7d41-115">HTTPS-based proxy servers are not supported by the configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="c7d41-116">Välja skyddsmål</span><span class="sxs-lookup"><span data-stu-id="c7d41-116">Choose your protection goals</span></span>

1. <span data-ttu-id="c7d41-117">I Azure-portalen går du till den **återställningstjänster** valvet bladet och välj ditt valv.</span><span class="sxs-lookup"><span data-stu-id="c7d41-117">In the Azure portal, go to the **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="c7d41-118">På menyn resurs på valvet, gå till **komma igång** > **Site Recovery** > **steg 1: Förbered infrastrukturen**  >  **Skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="c7d41-118">On the resource menu of the vault, go to **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="c7d41-120">I **skyddsmål**väljer **till Azure**, och välj **Ja, med VMware vSphere-Hypervisor**.</span><span class="sxs-lookup"><span data-stu-id="c7d41-120">In **Protection goal**, select **To Azure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="c7d41-121">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7d41-121">Then click **OK**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a><span data-ttu-id="c7d41-123">Konfigurera källmiljön</span><span class="sxs-lookup"><span data-stu-id="c7d41-123">Set up the source environment</span></span>
<span data-ttu-id="c7d41-124">Konfigurera källmiljön omfattar två huvudsakliga aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="c7d41-124">Setting up the source environment involves two main activities:</span></span>

- <span data-ttu-id="c7d41-125">Installera och registrera en konfigurationsserver med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c7d41-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="c7d41-126">Identifiera dina lokala virtuella datorer genom att ansluta Site Recovery till din lokala VMware vCenter eller vSphere EXSi värdar.</span><span class="sxs-lookup"><span data-stu-id="c7d41-126">Discover your on-premises virtual machines by connecting Site Recovery to your on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="c7d41-127">Steg 1: Installera och registrera en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="c7d41-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="c7d41-128">Klicka på **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="c7d41-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="c7d41-129">I **Förbered källa**, om du inte har en konfigurationsservern och klicka på **+ konfigurationsservern** att lägga till ett.</span><span class="sxs-lookup"><span data-stu-id="c7d41-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** to add one.</span></span>

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="c7d41-131">På den **Lägg till Server** bladet, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="c7d41-131">On the **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="c7d41-132">Hämta installationsfilen för enhetlig installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c7d41-132">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="c7d41-133">Ladda ned valvregistreringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c7d41-133">Download the vault registration key.</span></span> <span data-ttu-id="c7d41-134">När du kör installationsprogrammet för enhetlig måste nyckeln för tjänstregistrering.</span><span class="sxs-lookup"><span data-stu-id="c7d41-134">You need the registration key when you run Unified Setup.</span></span> <span data-ttu-id="c7d41-135">Nyckeln är giltig i fem dagar efter att du har genererat den.</span><span class="sxs-lookup"><span data-stu-id="c7d41-135">The key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="c7d41-137">På den dator som du använder som konfigurationsservern kör **Unified installationsprogram för Azure Site Recovery** att installera konfigurationsservern, processervern och huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="c7d41-137">On the machine you’re using as the configuration server, run **Azure Site Recovery Unified Setup** to install the configuration server, the process server, and the master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="c7d41-138">Kör Azure Site Recovery enhetlig installation</span><span class="sxs-lookup"><span data-stu-id="c7d41-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="c7d41-139">Registrera för konfiguration av servern misslyckas om datorns system klocka skiljer sig från lokal tid med fler än fem minuter.</span><span class="sxs-lookup"><span data-stu-id="c7d41-139">Configuration server registration fails if the time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="c7d41-140">Synkronisera systemklockan med en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) innan du påbörjar installationen.</span><span class="sxs-lookup"><span data-stu-id="c7d41-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting the installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="c7d41-141">Konfigurationsservern kan installeras via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c7d41-141">The configuration server can be installed via command line.</span></span> <span data-ttu-id="c7d41-142">Mer information finns i [installera konfigurationsservern med hjälp av kommandoradsverktyg](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="c7d41-142">For more information, see [Installing the configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-the-vmware-account-for-automatic-discovery"></a><span data-ttu-id="c7d41-143">Lägg till VMware-konto för automatisk identifiering</span><span class="sxs-lookup"><span data-stu-id="c7d41-143">Add the VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="c7d41-144">Steg 2: Lägg till en vCenter</span><span class="sxs-lookup"><span data-stu-id="c7d41-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="c7d41-145">Du måste ansluta VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery för att tillåta Azure Site Recovery att identifiera virtuella datorer som körs i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="c7d41-145">To allow Azure Site Recovery to discover virtual machines running in your on-premises environment, you need to connect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="c7d41-146">Välj **+ vCenter** att starta ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.</span><span class="sxs-lookup"><span data-stu-id="c7d41-146">Select **+vCenter** to start connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="c7d41-147">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="c7d41-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="c7d41-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7d41-148">Next steps</span></span>
<span data-ttu-id="c7d41-149">[Konfigurera målmiljön](./site-recovery-prepare-target-vmware-to-azure.md) i Azure.</span><span class="sxs-lookup"><span data-stu-id="c7d41-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
