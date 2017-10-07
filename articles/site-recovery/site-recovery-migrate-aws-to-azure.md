---
title: "aaaMigrate virtuella datorer från AWS tooAzure | Microsoft Docs"
description: "Den här artikeln beskriver hur toomigrate virtuella datorer som körs i Amazon Web Services (AWS) tooAzure med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a><span data-ttu-id="64bce-103">Migrera virtuella datorer i Amazon Web Services (AWS) tooAzure med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="64bce-103">Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery</span></span>

<span data-ttu-id="64bce-104">Den här artikeln beskriver hur toomigrate AWS Windows instanser tooAzure virtuella datorer med hello [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="64bce-104">This article describes how toomigrate AWS Windows instances tooAzure virtual machines with hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="64bce-105">Migrering är en växling från AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64bce-105">Migration is effectively a failover from AWS tooAzure.</span></span> <span data-ttu-id="64bce-106">Det går inte att återställning av datorer tooAWS och det finns ingen pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="64bce-106">You can't failback machines tooAWS, and there's no ongoing replication.</span></span> <span data-ttu-id="64bce-107">Den här artikeln beskriver hello steg för migrering i hello Azure-portalen och baseras på hello instruktioner för [replikerar en fysisk dator tooAzure](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="64bce-107">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for [replicating a physical machine tooAzure](site-recovery-vmware-to-azure.md).</span></span>

<span data-ttu-id="64bce-108">Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="64bce-108">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="64bce-109">Operativsystem som stöds</span><span class="sxs-lookup"><span data-stu-id="64bce-109">Supported operating systems</span></span>

<span data-ttu-id="64bce-110">Site Recovery kan vara används toomigrate EC2 instanser som kör något av följande operativsystem hello:</span><span class="sxs-lookup"><span data-stu-id="64bce-110">Site Recovery can be used toomigrate EC2 instances running any of hello following operating systems:</span></span>

- <span data-ttu-id="64bce-111">Windows (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="64bce-111">Windows(64 bit only)</span></span>
    - <span data-ttu-id="64bce-112">Windows Server 2008 R2 SP1 + (Citrix NV drivrutiner eller AWS NV-drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="64bce-112">Windows Server 2008 R2 SP1+ (Citrix PV drivers or AWS PV drivers only.</span></span> <span data-ttu-id="64bce-113">**Kör RedHat NV drivrutiner instanser stöds inte**) Windows Server 2012 Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="64bce-113">**Instances running RedHat PV drivers are not supported**) Windows Server 2012 Windows Server 2012 R2</span></span>
- <span data-ttu-id="64bce-114">Linux (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="64bce-114">Linux (64 bit only)</span></span>
    - <span data-ttu-id="64bce-115">Red Hat Enterprise Linux 6.7 (endast HVM virtualiserade instanser)</span><span class="sxs-lookup"><span data-stu-id="64bce-115">Red Hat Enterprise Linux 6.7 (HVM virtualized instances only)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64bce-116">Krav</span><span class="sxs-lookup"><span data-stu-id="64bce-116">Prerequisites</span></span>

<span data-ttu-id="64bce-117">Här är vad du behöver för den här distributionen:</span><span class="sxs-lookup"><span data-stu-id="64bce-117">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="64bce-118">**Konfigurationsservern**: en Amazon EC2 virtuell dator som kör Windows Server 2012 R2 distribueras som hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="64bce-118">**Configuration server**: An Amazon EC2 VM running Windows Server 2012 R2 is deployed as hello configuration server.</span></span> <span data-ttu-id="64bce-119">Som standard installeras hello andra Azure Site Recovery-komponenter (processervern och huvudmålservern) när du distribuerar hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="64bce-119">By default, hello other Azure Site Recovery components (process server and master target server) are installed when you deploy hello configuration server.</span></span> <span data-ttu-id="64bce-120">Den här artikeln beskriver hello steg för migrering i hello Azure-portalen och baseras på hello instruktioner för [Läs mer](site-recovery-components.md)</span><span class="sxs-lookup"><span data-stu-id="64bce-120">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for  [Learn more](site-recovery-components.md)</span></span>

* <span data-ttu-id="64bce-121">**EC2 instanser**: hello Amazon EC2 virtuella datorer instanser som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="64bce-121">**EC2 instances**: hello Amazon EC2 virtual machines instances that you want toomigrate.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="64bce-122">Distributionssteg</span><span class="sxs-lookup"><span data-stu-id="64bce-122">Deployment steps</span></span>

1. <span data-ttu-id="64bce-123">Skapa ett Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="64bce-123">Create a Recovery Services vault.</span></span>
2. <span data-ttu-id="64bce-124">hello säkerhetsgruppen för dina EC2 instanser ska ha regler har konfigurerat tooallow kommunikation mellan hello EC2-instans som du vill toomigrate och hello-instans som du planerar toodeploy hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="64bce-124">hello Security Group of your EC2 instances should have rules configured tooallow communication between hello EC2 instance that you want toomigrate, and hello instance on which you plan toodeploy hello Configuration Server.</span></span>

3. <span data-ttu-id="64bce-125">Distribuera en ASR konfigurationsservern på hello samma Amazon virtuella privata moln som dina EC2-instanser.</span><span class="sxs-lookup"><span data-stu-id="64bce-125">On hello same Amazon Virtual Private Cloud as your EC2 instances, deploy an ASR configuration server.</span></span> <span data-ttu-id="64bce-126">Se hello VMware / fysiskt tooAzure krav för konfigurationskrav för server-distributionen.</span><span class="sxs-lookup"><span data-stu-id="64bce-126">Refer hello VMware / Physical tooAzure prerequisites for configuration server deployment requirements.</span></span>

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  <span data-ttu-id="64bce-128">När din konfigurationsservern distribueras i AWS och registreras Recovery Services-valvet, bör du se hello konfigurationsservern och processervern under Site Recovery-infrastruktur som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="64bce-128">Once your configuration server is deployed in AWS and registered with your Recovery Services vault, you should see hello configuration server and process server under Site Recovery infrastructure as shown below:</span></span>

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. <span data-ttu-id="64bce-130">När du har distribuerat hello konfigurationsservern (det kan ta upp too15 minustes för den tooappear), verifiera att den kan kommunicera med hello virtuella datorer som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="64bce-130">After you've deployed hello configuration server (it might take up too15 minustes for it tooappear), validate that it can communicate with hello VMs that you want toomigrate.</span></span>

6. <span data-ttu-id="64bce-131">[Konfigurera replikeringsinställningar](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="64bce-131">[Set up replication settings](site-recovery-setup-replication-settings-vmware.md).</span></span>

7. <span data-ttu-id="64bce-132">Aktivera replikering: Aktivera replikering för hello virtuella datorer du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="64bce-132">Enable replication: Enable replication for hello VMs you want toomigrate.</span></span> <span data-ttu-id="64bce-133">Du kan identifiera hello EC2 instanser som använder hello privata IP-adresser, som du kan hämta från hello EC2-konsolen.</span><span class="sxs-lookup"><span data-stu-id="64bce-133">You can discover hello EC2 instances using hello private IP addresses, which you can get from hello EC2 console.</span></span>

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. <span data-ttu-id="64bce-135">När hello EC2 instanser som har skyddats och hello replikering tooAzure är klar, [köra ett Redundanstest](site-recovery-test-failover-to-azure.md) toovalidate programprestanda i Azure.</span><span class="sxs-lookup"><span data-stu-id="64bce-135">Once hello EC2 instances have been protected and hello replication tooAzure is complete, [run a Test Failover](site-recovery-test-failover-to-azure.md) toovalidate your application's performance in Azure.</span></span>

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. <span data-ttu-id="64bce-137">Kör en växling vid fel från AWS tooAzure för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="64bce-137">Run a Failover from AWS tooAzure for each VM.</span></span> <span data-ttu-id="64bce-138">Alternativt kan du skapa en återställningsplan kör en Redundansväxling toomigrate flera virtuella datorer från AWS tooAzure</span><span class="sxs-lookup"><span data-stu-id="64bce-138">Optionally, you can create a recovery plan and run a Failover, toomigrate multiple virtual machines from AWS tooAzure.</span></span> <span data-ttu-id="64bce-139">[Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="64bce-139">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

10. <span data-ttu-id="64bce-140">För migrering behöver du inte toocommit en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="64bce-140">For migration, you don't need toocommit a failover.</span></span> <span data-ttu-id="64bce-141">I stället du väljer alternativet för fullständig migrering av hello för varje dator som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="64bce-141">Instead, you select hello Complete Migration option for each machine you want toomigrate.</span></span> <span data-ttu-id="64bce-142">hello slutföra migreringen åtgärden är klar in hello migreringsprocessen, tar du bort replikeringen av hello datorn och stoppar Site Recovery fakturering för hello datorn.</span><span class="sxs-lookup"><span data-stu-id="64bce-142">hello Complete Migration action finishes up hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

    ![Migrera](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a><span data-ttu-id="64bce-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64bce-144">Next steps</span></span>

- <span data-ttu-id="64bce-145">[Förbereda migrerade datorer tooenable replikering](site-recovery-azure-to-azure-after-migration.md) tooanother region för disaster recovery behov.</span><span class="sxs-lookup"><span data-stu-id="64bce-145">[Prepare migrated machines tooenable replication](site-recovery-azure-to-azure-after-migration.md) tooanother region for disaster recovery needs.</span></span>
- <span data-ttu-id="64bce-146">Börja skydda dina arbetsbelastningar genom att [replikera virtuella Azure datorer.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="64bce-146">Start protecting your workloads by [replicating Azure virtual machines.](site-recovery-azure-to-azure.md)</span></span>
