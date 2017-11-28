---
title: "Ställ in hello källmiljön (fysiska servrar tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera fysiska servrar som kör Windows eller Linux till Azure."
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
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a><span data-ttu-id="86b3a-103">Ställ in hello källmiljön (fysisk server tooAzure)</span><span class="sxs-lookup"><span data-stu-id="86b3a-103">Set up hello source environment (physical server tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="86b3a-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="86b3a-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="86b3a-105">Fysisk tooAzure</span><span class="sxs-lookup"><span data-stu-id="86b3a-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="86b3a-106">Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera fysiska servrar som kör Windows eller Linux till Azure.</span><span class="sxs-lookup"><span data-stu-id="86b3a-106">This article describes how tooset up your on-premises environment toostart replicating physical servers running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86b3a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="86b3a-107">Prerequisites</span></span>

<span data-ttu-id="86b3a-108">hello artikeln förutsätter att du redan har:</span><span class="sxs-lookup"><span data-stu-id="86b3a-108">hello article assumes that you already have:</span></span>
1. <span data-ttu-id="86b3a-109">Recovery Services-valvet i hello [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="86b3a-109">A Recovery Services vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
3. <span data-ttu-id="86b3a-110">En fysisk dator på vilken tooinstall hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="86b3a-110">A physical computer on which tooinstall hello configuration server.</span></span>

### <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="86b3a-111">Minimikrav för konfiguration av servern</span><span class="sxs-lookup"><span data-stu-id="86b3a-111">Configuration server minimum requirements</span></span>
<span data-ttu-id="86b3a-112">hello i den följande tabellen listas hello minimikraven för maskinvara, programvara och nätverkskraven för en konfigurationsserver.</span><span class="sxs-lookup"><span data-stu-id="86b3a-112">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="86b3a-113">HTTPS-baserade proxyservrar stöds inte av hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="86b3a-113">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="86b3a-114">Välja skyddsmål</span><span class="sxs-lookup"><span data-stu-id="86b3a-114">Choose your protection goals</span></span>

1. <span data-ttu-id="86b3a-115">I hello Azure-portalen, går toohello **återställningstjänster** valv bladet och välj ditt valv.</span><span class="sxs-lookup"><span data-stu-id="86b3a-115">In hello Azure portal, go toohello **Recovery Services** vaults blade and select your vault.</span></span>
2. <span data-ttu-id="86b3a-116">I hello **resurs** menyn hello valvet, klickar du på **komma igång** > **Site Recovery** > **steg 1: Förbered Infrastruktur** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="86b3a-116">In hello **Resource** menu of hello vault, click **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. <span data-ttu-id="86b3a-118">I **skyddsmål**väljer **tooAzure** och **inte virtualiserade/andra**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="86b3a-118">In **Protection goal**, select **tooAzure** and **Not virtualized/Other**, and then click **OK**.</span></span>

    ![Välja mål](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="86b3a-120">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="86b3a-120">Set up hello source environment</span></span>

1. <span data-ttu-id="86b3a-121">I **Förbered källa**, om du inte har en konfigurationsservern och klicka på **+ konfigurationsservern** tooadd en.</span><span class="sxs-lookup"><span data-stu-id="86b3a-121">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

  ![Konfigurera källan](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. <span data-ttu-id="86b3a-123">I hello **Lägg till Server** bladet, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="86b3a-123">In hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="86b3a-124">Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="86b3a-124">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="86b3a-125">Hämta hello valvregistreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="86b3a-125">Download hello vault registration key.</span></span> <span data-ttu-id="86b3a-126">Du behöver hello registreringsnyckel när du kör installationsprogrammet för enhetlig.</span><span class="sxs-lookup"><span data-stu-id="86b3a-126">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="86b3a-127">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="86b3a-127">hello key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. <span data-ttu-id="86b3a-129">Kör på hello-dator som du använder som hello konfigurationsservern **Unified installationsprogram för Azure Site Recovery** tooinstall hello konfigurationsservern, hello processervern och hello master mål-servern.</span><span class="sxs-lookup"><span data-stu-id="86b3a-129">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="86b3a-130">Kör Azure Site Recovery enhetlig installation</span><span class="sxs-lookup"><span data-stu-id="86b3a-130">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="86b3a-131">Registrera för konfiguration av servern misslyckas om hello tiden på datorns systemklockan är mer än fem minuter från lokal tid.</span><span class="sxs-lookup"><span data-stu-id="86b3a-131">Configuration server registration fails if hello time on your computer's system clock is more than five minutes off of local time.</span></span> <span data-ttu-id="86b3a-132">Synkronisera systemklockan med en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) innan du påbörjar installationen hello.</span><span class="sxs-lookup"><span data-stu-id="86b3a-132">Synchronize your system clock with a [time server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="86b3a-133">hello konfigurationsservern kan installeras via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="86b3a-133">hello configuration server can be installed via a command line.</span></span> <span data-ttu-id="86b3a-134">Mer information finns i [installera konfigurationsservern med hjälp av kommandoradsverktyg](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="86b3a-134">For more information, see [Installing configuration server using command-line tools](http://aka.ms/installconfigsrv).</span></span>


## <a name="common-issues"></a><span data-ttu-id="86b3a-135">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="86b3a-135">Common issues</span></span>

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="86b3a-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86b3a-136">Next steps</span></span>

<span data-ttu-id="86b3a-137">Nästa steg omfattar [konfigurera målmiljön](./site-recovery-prepare-target-physical-to-azure.md) i Azure.</span><span class="sxs-lookup"><span data-stu-id="86b3a-137">Next step involves [setting up your target environment](./site-recovery-prepare-target-physical-to-azure.md) in Azure.</span></span>
