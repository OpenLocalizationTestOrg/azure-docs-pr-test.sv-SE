---
title: aaaSQL Server FCI - Azure Virtual Machines | Microsoft Docs
description: "Den här artikeln förklarar hur toocreate redundanskluster för SQL Server-instans på Azure Virtual Machines."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a><span data-ttu-id="42c34-103">Konfigurera SQL Server-instans för redundanskluster på virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="42c34-103">Configure SQL Server Failover Cluster Instance on Azure Virtual Machines</span></span>

<span data-ttu-id="42c34-104">Den här artikeln förklarar hur toocreate en SQL Server redundanskluster instansen (FCI) på virtuella Azure-datorer i Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="42c34-104">This article explains how toocreate a SQL Server Failover Cluster Instance (FCI) on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="42c34-105">Den här lösningen använder [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) som en programvarubaserad virtuellt SAN-nätverk som synkroniserar hello lagring (datadiskar) mellan hello noder (Azure virtuella datorer) i en Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-105">This solution uses [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) as a software-based virtual SAN that synchronizes hello storage (data disks) between hello nodes (Azure VMs) in a Windows Cluster.</span></span> <span data-ttu-id="42c34-106">S2D är ny i Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="42c34-106">S2D is new in Windows Server 2016.</span></span>

<span data-ttu-id="42c34-107">hello följande diagram visar hello komplett lösning på virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="42c34-107">hello following diagram shows hello complete solution on Azure virtual machines:</span></span>

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

<span data-ttu-id="42c34-109">föregående diagram visar hello:</span><span class="sxs-lookup"><span data-stu-id="42c34-109">hello preceding diagram shows:</span></span>

- <span data-ttu-id="42c34-110">Två virtuella Azure-datorer i ett redundanskluster i Windows.</span><span class="sxs-lookup"><span data-stu-id="42c34-110">Two Azure virtual machines in a Windows Failover Cluster.</span></span> <span data-ttu-id="42c34-111">När en virtuell dator är i ett redundanskluster som också kallas en *klusternod*, eller *noder*.</span><span class="sxs-lookup"><span data-stu-id="42c34-111">When a virtual machine is in a failover cluster it is also called a *cluster node*, or *nodes*.</span></span>
- <span data-ttu-id="42c34-112">Varje virtuell dator har två eller flera datadiskar.</span><span class="sxs-lookup"><span data-stu-id="42c34-112">Each virtual machine has two or more data disks.</span></span>
- <span data-ttu-id="42c34-113">S2D synkroniserar hello data på hello data och visar hello synkroniseras lagring som en lagringspool.</span><span class="sxs-lookup"><span data-stu-id="42c34-113">S2D synchronizes hello data on hello data disk and presents hello synchronized storage as a storage pool.</span></span>
- <span data-ttu-id="42c34-114">hello lagringspoolen visas ett kluster delad volym (CSV) toohello failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-114">hello storage pool presents a cluster shared volume (CSV) toohello failover cluster.</span></span>
- <span data-ttu-id="42c34-115">hello SQL Server FCI-klusterrollen använder hello CSV för hello dataenheter.</span><span class="sxs-lookup"><span data-stu-id="42c34-115">hello SQL Server FCI cluster role uses hello CSV for hello data drives.</span></span>
- <span data-ttu-id="42c34-116">Ett Azure belastningsutjämnare toohold hello IP-adress för hello FCI för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42c34-116">An Azure load balancer toohold hello IP address for hello SQL Server FCI.</span></span>
- <span data-ttu-id="42c34-117">Tillgänglighetsuppsättningen Azure innehåller alla hello-resurser.</span><span class="sxs-lookup"><span data-stu-id="42c34-117">An Azure availability set holds all hello resources.</span></span>

   >[!NOTE]
   ><span data-ttu-id="42c34-118">Alla Azure-resurser finns i hello diagram hello tillhör samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="42c34-118">All Azure resources are in hello diagram are in hello same resource group.</span></span>

<span data-ttu-id="42c34-119">Mer information om S2D finns [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span><span class="sxs-lookup"><span data-stu-id="42c34-119">For details about S2D, see [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span></span>

<span data-ttu-id="42c34-120">S2D stöder två typer av arkitekturer - konvergerade och hyperkonvergerad.</span><span class="sxs-lookup"><span data-stu-id="42c34-120">S2D supports two types of architectures - converged and hyper-converged.</span></span> <span data-ttu-id="42c34-121">hello-arkitekturen i det här dokumentet är hyperkonvergerad.</span><span class="sxs-lookup"><span data-stu-id="42c34-121">hello architecture in this document is hyper-converged.</span></span> <span data-ttu-id="42c34-122">Hyperkonvergerad infrastruktur platser hello lagring på hello samma servrar värden hello klustrade programmet.</span><span class="sxs-lookup"><span data-stu-id="42c34-122">A hyper-converged infrastructure places hello storage on hello same servers that host hello clustered application.</span></span> <span data-ttu-id="42c34-123">I den här arkitekturen är hello lagring på varje nod i SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="42c34-123">In this architecture, hello storage is on each SQL Server FCI node.</span></span>

### <a name="example-azure-template"></a><span data-ttu-id="42c34-124">Exemplet Azure-mall</span><span class="sxs-lookup"><span data-stu-id="42c34-124">Example Azure template</span></span>

<span data-ttu-id="42c34-125">Du kan skapa hello hela lösningen från en mall i Azure.</span><span class="sxs-lookup"><span data-stu-id="42c34-125">You can create hello entire solution in Azure from a template.</span></span> <span data-ttu-id="42c34-126">Ett exempel på en mall finns i hello GitHub [Azure Quickstart mallar](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span><span class="sxs-lookup"><span data-stu-id="42c34-126">An example of a template is available in hello GitHub [Azure Quickstart Templates](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span></span> <span data-ttu-id="42c34-127">Det här exemplet är inte utformad eller testas för en viss arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="42c34-127">This example is not designed or tested for any specific workload.</span></span> <span data-ttu-id="42c34-128">Du kan köra hello mallen toocreate en SQL Server-FCI med S2D lagring anslutna tooyour domän.</span><span class="sxs-lookup"><span data-stu-id="42c34-128">You can run hello template toocreate a SQL Server FCI with S2D storage connected tooyour domain.</span></span> <span data-ttu-id="42c34-129">Du kan utvärdera hello mall och ändra den efter dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="42c34-129">You can evaluate hello template, and modify it for your purposes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="42c34-130">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="42c34-130">Before you begin</span></span>

<span data-ttu-id="42c34-131">Det finns några saker du behöver tooknow och några saker som du behöver på plats innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="42c34-131">There are a few things you need tooknow and a couple of things that you need in place before you proceed.</span></span>

### <a name="what-tooknow"></a><span data-ttu-id="42c34-132">Vilka tooknow</span><span class="sxs-lookup"><span data-stu-id="42c34-132">What tooknow</span></span>
<span data-ttu-id="42c34-133">Du bör ha en operativ tolkning av hello följande tekniker:</span><span class="sxs-lookup"><span data-stu-id="42c34-133">You should have an operational understanding of hello following technologies:</span></span>

- [<span data-ttu-id="42c34-134">Tekniker för Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-134">Windows cluster technologies</span></span>](http://technet.microsoft.com/library/hh831579.aspx)
-  <span data-ttu-id="42c34-135">[Instanser av SQL Server-redundanskluster](http://msdn.microsoft.com/library/ms189134.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c34-135">[SQL Server Failover Cluster Instances](http://msdn.microsoft.com/library/ms189134.aspx).</span></span>

<span data-ttu-id="42c34-136">Du bör också ha en förståelse av hello följande tekniker:</span><span class="sxs-lookup"><span data-stu-id="42c34-136">Also, you should have a general understanding of hello following technologies:</span></span>

- [<span data-ttu-id="42c34-137">Hyper-Konvergerad lösning med lagringsutrymmen i Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="42c34-137">Hyper-converged solution using Storage Spaces Direct in Windows Server 2016</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [<span data-ttu-id="42c34-138">Azure-resursgrupper</span><span class="sxs-lookup"><span data-stu-id="42c34-138">Azure resource groups</span></span>](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a><span data-ttu-id="42c34-139">Vilka toohave</span><span class="sxs-lookup"><span data-stu-id="42c34-139">What toohave</span></span>

<span data-ttu-id="42c34-140">Innan du följer hello anvisningarna i den här artikeln bör bör du redan ha:</span><span class="sxs-lookup"><span data-stu-id="42c34-140">Before following hello instructions in this article, you should already have:</span></span>

- <span data-ttu-id="42c34-141">En Microsoft Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42c34-141">A Microsoft Azure subscription.</span></span>
- <span data-ttu-id="42c34-142">En Windows-domän på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-142">A Windows domain on Azure virtual machines.</span></span>
- <span data-ttu-id="42c34-143">Ett konto med behörighet toocreate objekt i hello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="42c34-143">An account with permission toocreate objects in hello Azure virtual machine.</span></span>
- <span data-ttu-id="42c34-144">Ett virtuellt Azure-nätverk och undernät med tillräckligt utrymme för IP-adresser för hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="42c34-144">An Azure virtual network and subnet with sufficient IP address space for hello following components:</span></span>
   - <span data-ttu-id="42c34-145">Både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-145">Both virtual machines.</span></span>
   - <span data-ttu-id="42c34-146">hello redundans klustrets IP-adress.</span><span class="sxs-lookup"><span data-stu-id="42c34-146">hello failover cluster IP address.</span></span>
   - <span data-ttu-id="42c34-147">En IP-adress för varje FCI.</span><span class="sxs-lookup"><span data-stu-id="42c34-147">An IP address for each FCI.</span></span>
- <span data-ttu-id="42c34-148">DNS har konfigurerats på hello Azure Network pekar toohello domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="42c34-148">DNS configured on hello Azure Network, pointing toohello domain controllers.</span></span>

<span data-ttu-id="42c34-149">Med dessa krav är på plats, kan du fortsätta med att skapa klustret för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="42c34-149">With these prerequisites in place, you can proceed with building your failover cluster.</span></span> <span data-ttu-id="42c34-150">hello första steget är toocreate hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-150">hello first step is toocreate hello virtual machines.</span></span>

## <a name="step-1-create-virtual-machines"></a><span data-ttu-id="42c34-151">Steg 1: Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="42c34-151">Step 1: Create virtual machines</span></span>

1. <span data-ttu-id="42c34-152">Logga in toohello [Azure-portalen](http://portal.azure.com) med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42c34-152">Log in toohello [Azure portal](http://portal.azure.com) with your subscription.</span></span>

1. <span data-ttu-id="42c34-153">[Skapa en Azure tillgänglighetsuppsättning](../tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="42c34-153">[Create an Azure availability set](../tutorial-availability-sets.md).</span></span>

   <span data-ttu-id="42c34-154">hello tillgänglighet in grupper virtuella datorer över feldomäner och uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="42c34-154">hello availability set groups virtual machines across fault domains and update domains.</span></span> <span data-ttu-id="42c34-155">Hej tillgänglighetsuppsättning säkerställer att programmet inte påverkas av enskilda felpunkter som hello nätverks-switch eller hello power enhet av en samling servrar.</span><span class="sxs-lookup"><span data-stu-id="42c34-155">hello availability set makes sure that your application is not affected by single points of failure, like hello network switch or hello power unit of a rack of servers.</span></span>

   <span data-ttu-id="42c34-156">Om du inte har skapat hello resursgruppen för dina virtuella datorer kan du göra det när du skapar en Azure tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-156">If you have not created hello resource group for your virtual machines, do it when you create an Azure availability set.</span></span> <span data-ttu-id="42c34-157">Om du använder hello Azure portal toocreate hello tillgänglighetsuppsättning hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="42c34-157">If you're using hello Azure portal toocreate hello availability set, do hello following steps:</span></span>

   - <span data-ttu-id="42c34-158">I hello Azure-portalen klickar du på  **+**  tooopen hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="42c34-158">In hello Azure portal, click **+** tooopen hello Azure Marketplace.</span></span> <span data-ttu-id="42c34-159">Sök efter **tillgänglighetsuppsättning**.</span><span class="sxs-lookup"><span data-stu-id="42c34-159">Search for **Availability set**.</span></span>
   - <span data-ttu-id="42c34-160">Klicka på **tillgänglighetsuppsättning**.</span><span class="sxs-lookup"><span data-stu-id="42c34-160">Click **Availability set**.</span></span>
   - <span data-ttu-id="42c34-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-161">Click **Create**.</span></span>
   - <span data-ttu-id="42c34-162">På hello **skapa tillgänglighetsuppsättning** bladet set hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="42c34-162">On hello **Create availability set** blade, set hello following values:</span></span>
      - <span data-ttu-id="42c34-163">**Namnet**: ett namn för hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-163">**Name**: A name for hello availability set.</span></span>
      - <span data-ttu-id="42c34-164">**Prenumerationen**: din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42c34-164">**Subscription**: Your Azure subscription.</span></span>
      - <span data-ttu-id="42c34-165">**Resursgruppen**: Om du vill toouse en befintlig grupp **använda befintliga** och välj hello hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="42c34-165">**Resource group**: If you want toouse an existing group, click **Use existing** and select hello group from hello drop-down list.</span></span> <span data-ttu-id="42c34-166">Annars väljer **Skapa nytt** och Skriv ett namn för hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="42c34-166">Otherwise choose **Create New** and type a name for hello group.</span></span>
      - <span data-ttu-id="42c34-167">**Plats**: Ange hello plats där du planerar att toocreate dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-167">**Location**: Set hello location where you plan toocreate your virtual machines.</span></span>
      - <span data-ttu-id="42c34-168">**Fault domäner**: Använd hello standard (3).</span><span class="sxs-lookup"><span data-stu-id="42c34-168">**Fault domains**: Use hello default (3).</span></span>
      - <span data-ttu-id="42c34-169">**Uppdatera domäner**: Använd hello standard (5).</span><span class="sxs-lookup"><span data-stu-id="42c34-169">**Update domains**: Use hello default (5).</span></span>
   - <span data-ttu-id="42c34-170">Klicka på **skapa** toocreate hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-170">Click **Create** toocreate hello availability set.</span></span>

1. <span data-ttu-id="42c34-171">Skapa hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-171">Create hello virtual machines in hello availability set.</span></span>

   <span data-ttu-id="42c34-172">Etablera två virtuella SQL Server-datorer i hello Azure tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-172">Provision two SQL Server virtual machines in hello Azure availability set.</span></span> <span data-ttu-id="42c34-173">Instruktioner finns i [etablera en virtuell dator med SQL Server i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="42c34-173">For instructions, see [Provision a SQL Server virtual machine in hello Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

   <span data-ttu-id="42c34-174">Placera både virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="42c34-174">Place both virtual machines:</span></span>

   - <span data-ttu-id="42c34-175">I hello är samma Azure-resursgrupp som ditt tillgänglighetsuppsättning i.</span><span class="sxs-lookup"><span data-stu-id="42c34-175">In hello same Azure resource group that your availability set is in.</span></span>
   - <span data-ttu-id="42c34-176">På hello samma nätverk som en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="42c34-176">On hello same network as your domain controller.</span></span>
   - <span data-ttu-id="42c34-177">I ett undernät med tillräckligt utrymme för IP-adresser för både virtuella datorer och alla FCIs som du kan använda förr eller senare i det här klustret.</span><span class="sxs-lookup"><span data-stu-id="42c34-177">On a subnet with sufficient IP address space for both virtual machines, and all FCIs that you may eventually use on this cluster.</span></span>
   - <span data-ttu-id="42c34-178">I hello Azure tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c34-178">In hello Azure availability set.</span></span>   

      >[!IMPORTANT]
      ><span data-ttu-id="42c34-179">Du kan inte ange eller ändra tillgänglighetsuppsättning när en virtuell dator har skapats.</span><span class="sxs-lookup"><span data-stu-id="42c34-179">You cannot set or change availability set after a virtual machine has been created.</span></span>

   <span data-ttu-id="42c34-180">Välj en bild från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="42c34-180">Choose an image from hello Azure Marketplace.</span></span> <span data-ttu-id="42c34-181">Du kan använda en Marketplace-avbildning med som omfattar Windows Server och SQL Server eller bara hello Windows Server.</span><span class="sxs-lookup"><span data-stu-id="42c34-181">You can use a Marketplace image with that includes Windows Server and SQL Server, or just hello Windows Server.</span></span> <span data-ttu-id="42c34-182">Mer information finns i [översikt över SQL Server på Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)</span><span class="sxs-lookup"><span data-stu-id="42c34-182">For details, see [Overview of SQL Server on Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)</span></span>

   <span data-ttu-id="42c34-183">hello officiella SQL Server-avbildningar i hello Azure-galleriet innehåller installerade SQL Server-instansen plus hello installationsprogrammet för SQL Server och hello nödvändig nyckel.</span><span class="sxs-lookup"><span data-stu-id="42c34-183">hello official SQL Server images in hello Azure Gallery include an installed SQL Server instance, plus hello SQL Server installation software, and hello required key.</span></span>

   <span data-ttu-id="42c34-184">Välj hello höger bild enligt toohow som du vill toopay för hello SQL Server-licens:</span><span class="sxs-lookup"><span data-stu-id="42c34-184">Choose hello right image according toohow you want toopay for hello SQL Server license:</span></span>

   - <span data-ttu-id="42c34-185">**Betala per användning licensiering**: hello per minut kostnaden för dessa avbildningar innehåller hello SQL Server-licensiering:</span><span class="sxs-lookup"><span data-stu-id="42c34-185">**Pay per usage licensing**: hello per-minute cost of these images includes hello SQL Server licensing:</span></span>
      - <span data-ttu-id="42c34-186">**SQL Server 2016 Enterprise på Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="42c34-186">**SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="42c34-187">**SQL Server 2016 Standard på Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="42c34-187">**SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="42c34-188">**SQL Server 2016 utvecklare i Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="42c34-188">**SQL Server 2016 Developer on Windows Server Datacenter 2016**</span></span>

   - <span data-ttu-id="42c34-189">**Bring-your-äger-licens (BYOL)**</span><span class="sxs-lookup"><span data-stu-id="42c34-189">**Bring-your-own-license (BYOL)**</span></span>

      - <span data-ttu-id="42c34-190">**{BYOL} SQL Server 2016 Enterprise på Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="42c34-190">**{BYOL} SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="42c34-191">**{BYOL} SQL Server 2016 Standard på Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="42c34-191">**{BYOL} SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>

   >[!IMPORTANT]
   ><span data-ttu-id="42c34-192">När du skapar hello virtuell dator tar du bort hello förinstallerade fristående SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="42c34-192">After you create hello virtual machine, remove hello pre-installed standalone SQL Server instance.</span></span> <span data-ttu-id="42c34-193">Du använder hello förinstallerade SQL Server media toocreate hello SQL Server FCI när du har konfigurerat hello failover-kluster och S2D.</span><span class="sxs-lookup"><span data-stu-id="42c34-193">You will use hello pre-installed SQL Server media toocreate hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span>

   <span data-ttu-id="42c34-194">Alternativt kan du använda Azure Marketplace-bilder med bara hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="42c34-194">Alternatively, you can use Azure Marketplace images with just hello operating system.</span></span> <span data-ttu-id="42c34-195">Välj en **Windows Server 2016 Datacenter** avbildningen och installera hello SQL Server FCI när du har konfigurerat hello failover-kluster och S2D.</span><span class="sxs-lookup"><span data-stu-id="42c34-195">Choose a **Windows Server 2016 Datacenter** image and install hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span> <span data-ttu-id="42c34-196">Den här avbildningen innehåller inte SQL Server-installationsmedia.</span><span class="sxs-lookup"><span data-stu-id="42c34-196">This image does not contain SQL Server installation media.</span></span> <span data-ttu-id="42c34-197">Placera hello installationsmediet på en plats där du kan köra hello SQL Server-installation för varje server.</span><span class="sxs-lookup"><span data-stu-id="42c34-197">Place hello installation media in a location where you can run hello SQL Server installation for each server.</span></span>

1. <span data-ttu-id="42c34-198">När Azure skapar virtuella datorer kan ansluta tooeach virtuell dator med RDP.</span><span class="sxs-lookup"><span data-stu-id="42c34-198">After Azure creates your virtual machines, connect tooeach virtual machine with RDP.</span></span>

   <span data-ttu-id="42c34-199">När du först ansluta tooa virtuell dator med RDP frågan hello datorn om du vill tooallow toobe för den här datorn kan identifieras på hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="42c34-199">When you first connect tooa virtual machine with RDP, hello computer asks if you want tooallow this PC toobe discoverable on hello network.</span></span> <span data-ttu-id="42c34-200">Klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="42c34-200">Click **Yes**.</span></span>

1. <span data-ttu-id="42c34-201">Om du använder en hello SQL Server-baserade virtuella avbildningar kan du ta bort hello SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="42c34-201">If you are using one of hello SQL Server-based virtual machine images, remove hello SQL Server instance.</span></span>

   - <span data-ttu-id="42c34-202">I **program och funktioner**, högerklicka på **Microsoft SQL Server 2016 (64-bitars)** och på **Avinstallera/ändra**.</span><span class="sxs-lookup"><span data-stu-id="42c34-202">In **Programs and Features**, right-click **Microsoft SQL Server 2016 (64-bit)** and click **Uninstall/Change**.</span></span>
   - <span data-ttu-id="42c34-203">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="42c34-203">Click **Remove**.</span></span>
   - <span data-ttu-id="42c34-204">Välj hello standardinstansen.</span><span class="sxs-lookup"><span data-stu-id="42c34-204">Select hello default instance.</span></span>
   - <span data-ttu-id="42c34-205">Ta bort alla funktioner under **Databasmotortjänster**.</span><span class="sxs-lookup"><span data-stu-id="42c34-205">Remove all features under **Database Engine Services**.</span></span> <span data-ttu-id="42c34-206">Ta inte bort **Delade funktioner i**.</span><span class="sxs-lookup"><span data-stu-id="42c34-206">Do not remove **Shared Features**.</span></span> <span data-ttu-id="42c34-207">Se hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="42c34-207">See hello following picture:</span></span>

      ![Ta bort funktioner](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - <span data-ttu-id="42c34-209">Klicka på **nästa**, och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="42c34-209">Click **Next**, and then click **Remove**.</span></span>

1. <span data-ttu-id="42c34-210"><a name="ports"></a>Öppna portar i brandväggen hello.</span><span class="sxs-lookup"><span data-stu-id="42c34-210"><a name="ports"></a>Open hello firewall ports.</span></span>

   <span data-ttu-id="42c34-211">Öppna hello följande portar på hello Windows-brandväggen på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="42c34-211">On each virtual machine, open hello following ports on hello Windows Firewall.</span></span>

   | <span data-ttu-id="42c34-212">Syfte</span><span class="sxs-lookup"><span data-stu-id="42c34-212">Purpose</span></span> | <span data-ttu-id="42c34-213">TCP-Port</span><span class="sxs-lookup"><span data-stu-id="42c34-213">TCP Port</span></span> | <span data-ttu-id="42c34-214">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="42c34-214">Notes</span></span>
   | ------ | ------ | ------
   | <span data-ttu-id="42c34-215">SQL Server</span><span class="sxs-lookup"><span data-stu-id="42c34-215">SQL Server</span></span> | <span data-ttu-id="42c34-216">1433</span><span class="sxs-lookup"><span data-stu-id="42c34-216">1433</span></span> | <span data-ttu-id="42c34-217">Normal port för standardinstanser av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42c34-217">Normal port for default instances of SQL Server.</span></span> <span data-ttu-id="42c34-218">Om du har använt en avbildning från galleriet hello öppnas automatiskt den här porten.</span><span class="sxs-lookup"><span data-stu-id="42c34-218">If you used an image from hello gallery, this port is automatically opened.</span></span>
   | <span data-ttu-id="42c34-219">Hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="42c34-219">Health probe</span></span> | <span data-ttu-id="42c34-220">59999</span><span class="sxs-lookup"><span data-stu-id="42c34-220">59999</span></span> | <span data-ttu-id="42c34-221">Alla öppna TCP-port.</span><span class="sxs-lookup"><span data-stu-id="42c34-221">Any open TCP port.</span></span> <span data-ttu-id="42c34-222">I ett senare steg, konfigurera hello belastningsutjämnare [hälsoavsökningen](#probe) och hello klustret toouse denna port.</span><span class="sxs-lookup"><span data-stu-id="42c34-222">In a later step, configure hello load balancer [health probe](#probe) and hello cluster toouse this port.</span></span>  

1. <span data-ttu-id="42c34-223">Lägg till lagring toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42c34-223">Add storage toohello virtual machine.</span></span> <span data-ttu-id="42c34-224">Detaljerad information finns i [lägga till lagringsenheter](../../../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="42c34-224">For detailed information, see [add storage](../../../storage/common/storage-premium-storage.md).</span></span>

   <span data-ttu-id="42c34-225">Både virtuella datorer måste ha minst två hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="42c34-225">Both virtual machines need at least two data disks.</span></span>

   <span data-ttu-id="42c34-226">Koppla rådata diskar - inte NTFS-formaterade diskar.</span><span class="sxs-lookup"><span data-stu-id="42c34-226">Attach raw disks - not NTFS formatted disks.</span></span>
      >[!NOTE]
      ><span data-ttu-id="42c34-227">Du kan bara aktivera S2D med ingen kontroll av behörighet om du kopplar NTFS-formaterade diskar.</span><span class="sxs-lookup"><span data-stu-id="42c34-227">If you attach NTFS-formatted disks, you can only enable S2D with no disk eligibility check.</span></span>  

   <span data-ttu-id="42c34-228">Koppla minst två Premium-lagring (SSD-diskar) tooeach VM.</span><span class="sxs-lookup"><span data-stu-id="42c34-228">Attach a minimum of two Premium Storage (SSD disks) tooeach VM.</span></span> <span data-ttu-id="42c34-229">Vi rekommenderar minst P30 diskar (1 TB).</span><span class="sxs-lookup"><span data-stu-id="42c34-229">We recommend at least P30 (1 TB) disks.</span></span>

   <span data-ttu-id="42c34-230">Ange värden för cachelagring**skrivskyddad**.</span><span class="sxs-lookup"><span data-stu-id="42c34-230">Set host caching too**Read-only**.</span></span>

   <span data-ttu-id="42c34-231">hello lagringskapacitet som du använder i produktionsmiljöer beror på din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="42c34-231">hello storage capacity you use in production environments depends on your workload.</span></span> <span data-ttu-id="42c34-232">hello värdena beskrivs i den här artikeln för demonstration och testning.</span><span class="sxs-lookup"><span data-stu-id="42c34-232">hello values described in this article are for demonstration and testing.</span></span>

1. <span data-ttu-id="42c34-233">[Lägg till hello virtuella datorer tooyour befintlig domän](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="42c34-233">[Add hello virtual machines tooyour pre-existing domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

<span data-ttu-id="42c34-234">När hello virtuella datorer skapas och konfigureras, kan du konfigurera hello failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-234">After hello virtual machines are created and configured, you can configure hello failover cluster.</span></span>

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a><span data-ttu-id="42c34-235">Steg 2: Konfigurera hello Windows-redundanskluster med S2D</span><span class="sxs-lookup"><span data-stu-id="42c34-235">Step 2: Configure hello Windows Failover Cluster with S2D</span></span>

<span data-ttu-id="42c34-236">hello nästa steg är tooconfigure hello redundanskluster med S2D.</span><span class="sxs-lookup"><span data-stu-id="42c34-236">hello next step is tooconfigure hello failover cluster with S2D.</span></span> <span data-ttu-id="42c34-237">I det här steget ska du göra hello följande stegen:</span><span class="sxs-lookup"><span data-stu-id="42c34-237">In this step, you will do hello following substeps:</span></span>

1. <span data-ttu-id="42c34-238">Lägger till funktionen för redundanskluster i Windows</span><span class="sxs-lookup"><span data-stu-id="42c34-238">Add Windows Failover Clustering feature</span></span>
1. <span data-ttu-id="42c34-239">Verifiera hello-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-239">Validate hello cluster</span></span>
1. <span data-ttu-id="42c34-240">Skapa hello failover-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-240">Create hello failover cluster</span></span>
1. <span data-ttu-id="42c34-241">Skapa hello molnet vittne</span><span class="sxs-lookup"><span data-stu-id="42c34-241">Create hello cloud witness</span></span>
1. <span data-ttu-id="42c34-242">Lägga till lagringsenheter</span><span class="sxs-lookup"><span data-stu-id="42c34-242">Add storage</span></span>

### <a name="add-windows-failover-clustering-feature"></a><span data-ttu-id="42c34-243">Lägger till funktionen för redundanskluster i Windows</span><span class="sxs-lookup"><span data-stu-id="42c34-243">Add Windows Failover Clustering feature</span></span>

1. <span data-ttu-id="42c34-244">toobegin, ansluta toohello första virtuella datorn med RDP med ett domänkonto som är medlem i lokala administratörer och har behörighet toocreate objekt i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="42c34-244">toobegin, connect toohello first virtual machine with RDP using a domain account that is a member of local administrators, and has permissions toocreate objects in Active Directory.</span></span> <span data-ttu-id="42c34-245">Använd det här kontot för hello resten av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="42c34-245">Use this account for hello rest of hello configuration.</span></span>

1. <span data-ttu-id="42c34-246">[Lägg till redundanskluster funktionen tooeach virtuell dator](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="42c34-246">[Add Failover Clustering feature tooeach virtual machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

   <span data-ttu-id="42c34-247">tooinstall redundansklusterfunktionen från hello-Gränssnittet hello följande steg på både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-247">tooinstall Failover Clustering feature from hello UI, do hello following steps on both virtual machines.</span></span>
   - <span data-ttu-id="42c34-248">I **Serverhanteraren**, klickar du på **hantera**, och klicka sedan på **Lägg till roller och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="42c34-248">In **Server Manager**, click **Manage**, and then click **Add Roles and Features**.</span></span>
   - <span data-ttu-id="42c34-249">I **guiden Lägg till roller och funktioner**, klickar du på **nästa** förrän du får för**Välj funktioner**.</span><span class="sxs-lookup"><span data-stu-id="42c34-249">In **Add Roles and Features Wizard**, click **Next** until you get too**Select Features**.</span></span>
   - <span data-ttu-id="42c34-250">I **Välj funktioner**, klickar du på **redundanskluster**.</span><span class="sxs-lookup"><span data-stu-id="42c34-250">In **Select Features**, click **Failover Clustering**.</span></span> <span data-ttu-id="42c34-251">Inkludera alla nödvändiga funktioner och hello-hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="42c34-251">Include all required features and hello management tools.</span></span> <span data-ttu-id="42c34-252">Klicka på **lägga till funktioner**.</span><span class="sxs-lookup"><span data-stu-id="42c34-252">Click **Add Features**.</span></span>
   - <span data-ttu-id="42c34-253">Klicka på **nästa** och klicka sedan på **Slutför** tooinstall hello funktioner.</span><span class="sxs-lookup"><span data-stu-id="42c34-253">Click **Next** and then click **Finish** tooinstall hello features.</span></span>

   <span data-ttu-id="42c34-254">tooinstall hello funktionen för redundanskluster med PowerShell kan köra hello följande skript från en administratör PowerShell-session på en av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-254">tooinstall hello Failover Clustering feature with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

<span data-ttu-id="42c34-255">För referens, hello nästa steg gör hello anvisningarna under steg3 i [Hyper-Konvergerad lösning med lagringsutrymmen i Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="42c34-255">For reference, hello next steps follow hello instructions under Step 3 of [Hyper-converged solution using Storage Spaces Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span></span>

### <a name="validate-hello-cluster"></a><span data-ttu-id="42c34-256">Verifiera hello-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-256">Validate hello cluster</span></span>

<span data-ttu-id="42c34-257">Den här guiden refererar tooinstructions under [verifiera kluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span><span class="sxs-lookup"><span data-stu-id="42c34-257">This guide refers tooinstructions under [validate cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span></span>

<span data-ttu-id="42c34-258">Verifiera hello-kluster i hello Användargränssnittet eller med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="42c34-258">Validate hello cluster in hello UI or with PowerShell.</span></span>

<span data-ttu-id="42c34-259">toovalidate hello kluster med hello-Gränssnittet hello följande från någon av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-259">toovalidate hello cluster with hello UI, do hello following steps from one of hello virtual machines.</span></span>

1. <span data-ttu-id="42c34-260">I **Serverhanteraren**, klickar du på **verktyg**, klicka på **Klusterhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="42c34-260">In **Server Manager**, click **Tools**, then click **Failover Cluster Manager**.</span></span>
1. <span data-ttu-id="42c34-261">I **Klusterhanteraren**, klickar du på **åtgärd**, klicka på **verifiera konfiguration...** .</span><span class="sxs-lookup"><span data-stu-id="42c34-261">In **Failover Cluster Manager**, click **Action**, then click **Validate Configuration...**.</span></span>
1. <span data-ttu-id="42c34-262">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-262">Click **Next**.</span></span>
1. <span data-ttu-id="42c34-263">På **Välj servrar eller ett kluster**, hello-typnamn för både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-263">On **Select Servers or a Cluster**, type hello name of both virtual machines.</span></span>
1. <span data-ttu-id="42c34-264">På **test,**, Välj **kör bara valda test**.</span><span class="sxs-lookup"><span data-stu-id="42c34-264">On **Testing options**, choose **Run only tests I select**.</span></span> <span data-ttu-id="42c34-265">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-265">Click **Next**.</span></span>
1. <span data-ttu-id="42c34-266">På **testa markeringen**, inkluderar alla test med undantag **lagring**.</span><span class="sxs-lookup"><span data-stu-id="42c34-266">On **Test selection**, include all tests except **Storage**.</span></span> <span data-ttu-id="42c34-267">Se hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="42c34-267">See hello following picture:</span></span>

   ![Verifiera test](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. <span data-ttu-id="42c34-269">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-269">Click **Next**.</span></span>
1. <span data-ttu-id="42c34-270">På **bekräftelse**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-270">On **Confirmation**, click **Next**.</span></span>

<span data-ttu-id="42c34-271">Hej **guiden Verifiera en konfiguration** körs hello verifieringstester.</span><span class="sxs-lookup"><span data-stu-id="42c34-271">hello **Validate a Configuration Wizard** runs hello validation tests.</span></span>

<span data-ttu-id="42c34-272">toovalidate hello kluster med PowerShell kan köra hello följande skript från en administratör PowerShell-session på en av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-272">toovalidate hello cluster with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

<span data-ttu-id="42c34-273">När du validerar hello kluster skapa hello failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-273">After you validate hello cluster, create hello failover cluster.</span></span>

### <a name="create-hello-failover-cluster"></a><span data-ttu-id="42c34-274">Skapa hello failover-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-274">Create hello failover cluster</span></span>

<span data-ttu-id="42c34-275">Den här guiden refererar för[skapa hello redundanskluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span><span class="sxs-lookup"><span data-stu-id="42c34-275">This guide refers too[Create hello failover cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span></span>

<span data-ttu-id="42c34-276">toocreate hello failover-kluster, behöver du:</span><span class="sxs-lookup"><span data-stu-id="42c34-276">toocreate hello failover cluster, you need:</span></span>
- <span data-ttu-id="42c34-277">hello namnen på hello virtuella datorer som blivit hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="42c34-277">hello names of hello virtual machines that become hello cluster nodes.</span></span>
- <span data-ttu-id="42c34-278">Ett namn för hello failover-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-278">A name for hello failover cluster</span></span>
- <span data-ttu-id="42c34-279">En IP-adress för hello failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-279">An IP address for hello failover cluster.</span></span> <span data-ttu-id="42c34-280">Du kan använda en IP-adress som inte används på hello samma Azure-nätverk och undernät som hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="42c34-280">You can use an IP address that is not used on hello same Azure virtual network and subnet as hello cluster nodes.</span></span>

<span data-ttu-id="42c34-281">hello följande PowerShell skapar ett redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-281">hello following PowerShell creates a failover cluster.</span></span> <span data-ttu-id="42c34-282">Uppdatera hello skriptet med hello namnen på hello noder (hello namn på virtuella datorer) och en tillgänglig IP-adress från hello Azure VNET:</span><span class="sxs-lookup"><span data-stu-id="42c34-282">Update hello script with hello names of hello nodes (hello virtual machine names) and an available IP address from hello Azure VNET:</span></span>

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a><span data-ttu-id="42c34-283">Skapa ett moln vittne</span><span class="sxs-lookup"><span data-stu-id="42c34-283">Create a cloud witness</span></span>

<span data-ttu-id="42c34-284">Molnet vittne är en ny typ av kluster kvorumvittne lagras i en Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="42c34-284">Cloud Witness is a new type of cluster quorum witness stored in an Azure Storage Blob.</span></span> <span data-ttu-id="42c34-285">Detta tar bort hello behovet av en separat virtuell dator som värd för en resurs som vittne.</span><span class="sxs-lookup"><span data-stu-id="42c34-285">This removes hello need of a separate VM hosting a witness share.</span></span>

1. <span data-ttu-id="42c34-286">[Skapa ett moln vittne för hello redundanskluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="42c34-286">[Create a cloud witness for hello failover cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span>

1. <span data-ttu-id="42c34-287">Skapa en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="42c34-287">Create a blob container.</span></span>

1. <span data-ttu-id="42c34-288">Spara hello snabbtangenter och hello behållar-URL.</span><span class="sxs-lookup"><span data-stu-id="42c34-288">Save hello access keys and hello container URL.</span></span>

1. <span data-ttu-id="42c34-289">Konfigurera hello failover cluster klustret kvorumvittne.</span><span class="sxs-lookup"><span data-stu-id="42c34-289">Configure hello failover cluster cluster quorum witness.</span></span> <span data-ttu-id="42c34-290">Se [konfigurera hello kvorumvittne i användargränssnittet för hello]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) i hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="42c34-290">See, [Configure hello quorum witness in hello user interface].(http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in hello UI.</span></span>

### <a name="add-storage"></a><span data-ttu-id="42c34-291">Lägga till lagringsenheter</span><span class="sxs-lookup"><span data-stu-id="42c34-291">Add storage</span></span>

<span data-ttu-id="42c34-292">hello diskarna för S2D måste toobe tomt och utan partitioner eller andra data.</span><span class="sxs-lookup"><span data-stu-id="42c34-292">hello disks for S2D need toobe empty and without partitions or other data.</span></span> <span data-ttu-id="42c34-293">Följ tooclean diskar [hello stegen i den här guiden](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span><span class="sxs-lookup"><span data-stu-id="42c34-293">tooclean disks follow [hello steps in this guide](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span></span>

1. <span data-ttu-id="42c34-294">[Aktivera Store lagringsutrymmen Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="42c34-294">[Enable Store Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span></span>

   <span data-ttu-id="42c34-295">följande PowerShell hello kan lagringsutrymmen direkt.</span><span class="sxs-lookup"><span data-stu-id="42c34-295">hello following PowerShell enables storage spaces direct.</span></span>  

   ```PowerShell
   Enable-ClusterS2D
   ```

   <span data-ttu-id="42c34-296">I **Klusterhanteraren**, du kan nu se hello lagringspoolen.</span><span class="sxs-lookup"><span data-stu-id="42c34-296">In **Failover Cluster Manager**, you can now see hello storage pool.</span></span>

1. <span data-ttu-id="42c34-297">[Skapa en volym](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span><span class="sxs-lookup"><span data-stu-id="42c34-297">[Create a volume](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span></span>

   <span data-ttu-id="42c34-298">En av hello funktionerna i S2D är att det automatiskt skapas en lagringspool när du aktiverar den.</span><span class="sxs-lookup"><span data-stu-id="42c34-298">One of hello features of S2D is that it automatically creates a storage pool when you enable it.</span></span> <span data-ttu-id="42c34-299">Du är nu redo toocreate en volym.</span><span class="sxs-lookup"><span data-stu-id="42c34-299">You are now ready toocreate a volume.</span></span> <span data-ttu-id="42c34-300">Hej PowerShell-kommandot `New-Volume` automatiserar hello volym skapas, inklusive formatering, lägga till toohello klustret och skapa en klusterdelad volym (CSV).</span><span class="sxs-lookup"><span data-stu-id="42c34-300">hello PowerShell commandlet `New-Volume` automates hello volume creation process, including formatting, adding toohello cluster, and creating a cluster shared volume (CSV).</span></span> <span data-ttu-id="42c34-301">hello följande exempel skapar en 800 GB (Gigabyte) CSV.</span><span class="sxs-lookup"><span data-stu-id="42c34-301">hello following example creates an 800 gigabyte (GB) CSV.</span></span>

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   <span data-ttu-id="42c34-302">När det här kommandot har slutförts, är en 800 GB-volym monterad som en klusterresurs.</span><span class="sxs-lookup"><span data-stu-id="42c34-302">After this command completes, an 800 GB volume is mounted as a cluster resource.</span></span> <span data-ttu-id="42c34-303">hello volymen är på `C:\ClusterStorage\Volume1\`.</span><span class="sxs-lookup"><span data-stu-id="42c34-303">hello volume is at `C:\ClusterStorage\Volume1\`.</span></span>

   <span data-ttu-id="42c34-304">hello följande diagram visar en delad klustervolym med S2D:</span><span class="sxs-lookup"><span data-stu-id="42c34-304">hello following diagram shows a cluster shared volume with S2D:</span></span>

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a><span data-ttu-id="42c34-306">Steg 3: Testa redundans-kluster</span><span class="sxs-lookup"><span data-stu-id="42c34-306">Step 3: Test failover cluster failover</span></span>

<span data-ttu-id="42c34-307">I hanteraren för redundanskluster, kontrollera att du kan flytta hello storage resource toohello annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="42c34-307">In Failover Cluster Manager, verify that you can move hello storage resource toohello other cluster node.</span></span> <span data-ttu-id="42c34-308">Om du kan ansluta toohello redundanskluster med **Klusterhanteraren** och flytta hello lagringsutrymme från en nod toohello andra, är du redo tooconfigure hello FCI.</span><span class="sxs-lookup"><span data-stu-id="42c34-308">If you can connect toohello failover cluster with **Failover Cluster Manager** and move hello storage from one node toohello other, you are ready tooconfigure hello FCI.</span></span>

## <a name="step-4-create-sql-server-fci"></a><span data-ttu-id="42c34-309">Steg 4: Skapa SQLServer FCI</span><span class="sxs-lookup"><span data-stu-id="42c34-309">Step 4: Create SQL Server FCI</span></span>

<span data-ttu-id="42c34-310">När du har konfigurerat hello failover-kluster och alla komponenter i serverkluster inklusive lagring, kan du skapa hello SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="42c34-310">After you have configured hello failover cluster and all cluster components including storage, you can create hello SQL Server FCI.</span></span>

1. <span data-ttu-id="42c34-311">Ansluta toohello första virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="42c34-311">Connect toohello first virtual machine with RDP.</span></span>

1. <span data-ttu-id="42c34-312">I **Klusterhanteraren**, se till att alla klustrets kärnresurser är hello första virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42c34-312">In **Failover Cluster Manager**, make sure all cluster core resources are on hello first virtual machine.</span></span> <span data-ttu-id="42c34-313">Flytta alla resurser toothis virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42c34-313">If necessary, move all resources toothis virtual machine.</span></span>

1. <span data-ttu-id="42c34-314">Leta upp hello-installationsmediet.</span><span class="sxs-lookup"><span data-stu-id="42c34-314">Locate hello installation media.</span></span> <span data-ttu-id="42c34-315">Om hello virtuell dator använder en hello Azure Marketplace-bilder, hello media finns i `C:\SQLServer_<version number>_Full`.</span><span class="sxs-lookup"><span data-stu-id="42c34-315">If hello virtual machine uses one of hello Azure Marketplace images, hello media is located at `C:\SQLServer_<version number>_Full`.</span></span> <span data-ttu-id="42c34-316">Klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="42c34-316">Click **Setup**.</span></span>

1. <span data-ttu-id="42c34-317">I hello **SQL Server Installationscenter**, klickar du på **Installation**.</span><span class="sxs-lookup"><span data-stu-id="42c34-317">In hello **SQL Server Installation Center**, click **Installation**.</span></span>

1. <span data-ttu-id="42c34-318">Klicka på **nya SQL-servern failover cluster installation**.</span><span class="sxs-lookup"><span data-stu-id="42c34-318">Click **New SQL Server failover cluster installation**.</span></span> <span data-ttu-id="42c34-319">Följ instruktionerna för hello i hello guiden tooinstall hello FCI för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42c34-319">Follow hello instructions in hello wizard tooinstall hello SQL Server FCI.</span></span>

   <span data-ttu-id="42c34-320">Hej FCI datakataloger måste toobe på klustrad lagring.</span><span class="sxs-lookup"><span data-stu-id="42c34-320">hello FCI data directories need toobe on clustered storage.</span></span> <span data-ttu-id="42c34-321">Med S2D är det inte en delad disk utan en monteringspunkt punkt tooa volym på varje server.</span><span class="sxs-lookup"><span data-stu-id="42c34-321">With S2D, it's not a shared disk, but a mount point tooa volume on each server.</span></span> <span data-ttu-id="42c34-322">S2D synkroniserar hello volym mellan båda noderna.</span><span class="sxs-lookup"><span data-stu-id="42c34-322">S2D synchronizes hello volume between both nodes.</span></span> <span data-ttu-id="42c34-323">hello-volymen visas toohello klustret som en klusterdelad volym.</span><span class="sxs-lookup"><span data-stu-id="42c34-323">hello volume is presented toohello cluster as a cluster shared volume.</span></span> <span data-ttu-id="42c34-324">Använd hello CSV monteringspunkt för hello datakataloger.</span><span class="sxs-lookup"><span data-stu-id="42c34-324">Use hello CSV mount point for hello data directories.</span></span>

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. <span data-ttu-id="42c34-326">När du har slutfört guiden hello installeras en SQL Server FCI på hello första noden.</span><span class="sxs-lookup"><span data-stu-id="42c34-326">After you complete hello wizard, Setup will install a SQL Server FCI on hello first node.</span></span>

1. <span data-ttu-id="42c34-327">När installationen har installerats hello FCI på hello första noden ansluta toohello andra noden med RDP.</span><span class="sxs-lookup"><span data-stu-id="42c34-327">After Setup successfully installs hello FCI on hello first node, connect toohello second node with RDP.</span></span>

1. <span data-ttu-id="42c34-328">Öppna hello **SQL Server Installationscenter**.</span><span class="sxs-lookup"><span data-stu-id="42c34-328">Open hello **SQL Server Installation Center**.</span></span> <span data-ttu-id="42c34-329">Klicka på **Installation**.</span><span class="sxs-lookup"><span data-stu-id="42c34-329">Click **Installation**.</span></span>

1. <span data-ttu-id="42c34-330">Klicka på **Lägg till nod tooa SQL Server-redundanskluster**.</span><span class="sxs-lookup"><span data-stu-id="42c34-330">Click **Add node tooa SQL Server failover cluster**.</span></span> <span data-ttu-id="42c34-331">Följ instruktionerna för hello i hello guiden tooinstall SQLServer och Lägg till den här servern toohello FCI.</span><span class="sxs-lookup"><span data-stu-id="42c34-331">Follow hello instructions in hello wizard tooinstall SQL server and add this server toohello FCI.</span></span>

   >[!NOTE]
   ><span data-ttu-id="42c34-332">Om du använde en galleriet Azure Marketplace-avbildning med SQL Server, ingick SQL Server-verktyg i hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="42c34-332">If you used an Azure Marketplace gallery image with SQL Server, SQL Server tools were included with hello image.</span></span> <span data-ttu-id="42c34-333">Om du inte använde den här avbildningen, installera SQL Server-verktyg för hello separat.</span><span class="sxs-lookup"><span data-stu-id="42c34-333">If you did not use this image, install hello SQL Server tools separately.</span></span> <span data-ttu-id="42c34-334">Se [Hämta SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c34-334">See [Download SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="step-5-create-azure-load-balancer"></a><span data-ttu-id="42c34-335">Steg 5: Skapa Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="42c34-335">Step 5: Create Azure load balancer</span></span>

<span data-ttu-id="42c34-336">På Azure virtual machines använder kluster en belastningen belastningsutjämnaren toohold en IP-adress som behöver toobe på en klusternod åt gången.</span><span class="sxs-lookup"><span data-stu-id="42c34-336">On Azure virtual machines, clusters use a load balancer toohold an IP address that needs toobe on one cluster node at a time.</span></span> <span data-ttu-id="42c34-337">I den här lösningen innehåller hello belastningsutjämnaren hello IP-adress för hello FCI för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42c34-337">In this solution, hello load balancer holds hello IP address for hello SQL Server FCI.</span></span>

<span data-ttu-id="42c34-338">[Skapa och konfigurera en Azure belastningsutjämnare](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="42c34-338">[Create and configure an Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="42c34-339">Skapa hello belastningsutjämnare i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="42c34-339">Create hello load balancer in hello Azure portal</span></span>

<span data-ttu-id="42c34-340">toocreate hello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="42c34-340">toocreate hello load balancer:</span></span>

1. <span data-ttu-id="42c34-341">Gå toohello resursgrupp med hello virtuella datorer i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="42c34-341">In hello Azure portal, go toohello Resource Group with hello virtual machines.</span></span>

1. <span data-ttu-id="42c34-342">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="42c34-342">Click **+ Add**.</span></span> <span data-ttu-id="42c34-343">Sök hello Marketplace för **belastningsutjämnaren**.</span><span class="sxs-lookup"><span data-stu-id="42c34-343">Search hello Marketplace for **Load Balancer**.</span></span> <span data-ttu-id="42c34-344">Klicka på **belastningsutjämnare**.</span><span class="sxs-lookup"><span data-stu-id="42c34-344">Click **Load Balancer**.</span></span>

1. <span data-ttu-id="42c34-345">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="42c34-345">Click **Create**.</span></span>

1. <span data-ttu-id="42c34-346">Konfigurera hello belastningsutjämnare med:</span><span class="sxs-lookup"><span data-stu-id="42c34-346">Configure hello load balancer with:</span></span>

   - <span data-ttu-id="42c34-347">**Namnet**: ett namn som identifierar hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="42c34-347">**Name**: A name that identifies hello load balancer.</span></span>
   - <span data-ttu-id="42c34-348">**Typen**: hello belastningsutjämnare kan vara offentligt eller privat.</span><span class="sxs-lookup"><span data-stu-id="42c34-348">**Type**: hello load balancer can be either public or private.</span></span> <span data-ttu-id="42c34-349">En privat belastningsutjämnare kan nås inifrån hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="42c34-349">A private load balancer can be accessed from within hello same VNET.</span></span> <span data-ttu-id="42c34-350">De flesta Azure-program kan använda en privat belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="42c34-350">Most Azure applications can use a private load balancer.</span></span> <span data-ttu-id="42c34-351">Om ditt program måste åtkomst tooSQL Server direkt via hello Internet, kan du använda en offentlig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="42c34-351">If your application needs access tooSQL Server directly over hello Internet, use a public load balancer.</span></span>
   - <span data-ttu-id="42c34-352">**Virtuellt nätverk**: hello samma nätverk som hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-352">**Virtual Network**: hello same network as hello virtual machines.</span></span>
   - <span data-ttu-id="42c34-353">**Undernät**: hello samma undernät som hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="42c34-353">**Subnet**: hello same subnet as hello virtual machines.</span></span>
   - <span data-ttu-id="42c34-354">**Privata IP-adressen**: hello samma IP-adress som du tilldelade toohello SQL Server FCI-klusterresursen nätverk.</span><span class="sxs-lookup"><span data-stu-id="42c34-354">**Private IP address**: hello same IP address that you assigned toohello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="42c34-355">**prenumerationen**: din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42c34-355">**subscription**: Your Azure subscription.</span></span>
   - <span data-ttu-id="42c34-356">**Resursgruppen**: Använd hello samma resursgrupp som de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="42c34-356">**Resource Group**: Use hello same resource group as your virtual machines.</span></span>
   - <span data-ttu-id="42c34-357">**Plats**: Använd hello samma Azure-plats som de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="42c34-357">**Location**: Use hello same Azure location as your virtual machines.</span></span>
   <span data-ttu-id="42c34-358">Se hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="42c34-358">See hello following picture:</span></span>

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a><span data-ttu-id="42c34-360">Konfigurera hello belastningsutjämnarens serverdelspool</span><span class="sxs-lookup"><span data-stu-id="42c34-360">Configure hello load balancer backend pool</span></span>

1. <span data-ttu-id="42c34-361">Returnera toohello Azure-resursgrupp med hello virtuella datorer och leta upp hello nya belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="42c34-361">Return toohello Azure Resource Group with hello virtual machines and locate hello new load balancer.</span></span> <span data-ttu-id="42c34-362">Du kan ha toorefresh hello vyn på hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="42c34-362">You may have toorefresh hello view on hello Resource Group.</span></span> <span data-ttu-id="42c34-363">Klicka på hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="42c34-363">Click hello load balancer.</span></span>

1. <span data-ttu-id="42c34-364">På hello belastningen belastningsutjämnaren bladet, klickar du på **serverdelspooler**.</span><span class="sxs-lookup"><span data-stu-id="42c34-364">On hello load balancer blade, click **Backend pools**.</span></span>

1. <span data-ttu-id="42c34-365">Klicka på **+ Lägg till** tooadd en serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="42c34-365">Click **+ Add** tooadd a backend pool.</span></span>

1. <span data-ttu-id="42c34-366">Ange ett namn för hello serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="42c34-366">Type a name for hello backend pool.</span></span>

1. <span data-ttu-id="42c34-367">Klicka på **lägga till en virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="42c34-367">Click **Add a virtual machine**.</span></span>

1. <span data-ttu-id="42c34-368">På hello **Välj virtuella datorer** bladet, klickar du på **Välj en tillgänglighetsuppsättning**.</span><span class="sxs-lookup"><span data-stu-id="42c34-368">On hello **Choose virtual machines** blade, click **Choose an availability set**.</span></span>

1. <span data-ttu-id="42c34-369">Välj hello tillgänglighetsuppsättning du placerade hello SQL Server-datorer i.</span><span class="sxs-lookup"><span data-stu-id="42c34-369">Choose hello availability set that you placed hello SQL Server virtual machines in.</span></span>

1. <span data-ttu-id="42c34-370">På hello **Välj virtuella datorer** bladet, klickar du på **Välj hello virtuella datorerna**.</span><span class="sxs-lookup"><span data-stu-id="42c34-370">On hello **Choose virtual machines** blade, click **Choose hello virtual machines**.</span></span>

   <span data-ttu-id="42c34-371">Azure-portalen ska se ut så hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="42c34-371">Your Azure portal should look like hello following picture:</span></span>

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. <span data-ttu-id="42c34-373">Klicka på **Välj** på hello **Välj virtuella datorer** bladet.</span><span class="sxs-lookup"><span data-stu-id="42c34-373">Click **Select** on hello **Choose virtual machines** blade.</span></span>

1. <span data-ttu-id="42c34-374">Klicka på **OK** två gånger.</span><span class="sxs-lookup"><span data-stu-id="42c34-374">Click **OK** twice.</span></span>

### <a name="configure-a-load-balancer-health-probe"></a><span data-ttu-id="42c34-375">Konfigurera en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="42c34-375">Configure a load balancer health probe</span></span>

1. <span data-ttu-id="42c34-376">På hello belastningen belastningsutjämnaren bladet, klickar du på **hälsoavsökning**.</span><span class="sxs-lookup"><span data-stu-id="42c34-376">On hello load balancer blade, click **Health probes**.</span></span>

1. <span data-ttu-id="42c34-377">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="42c34-377">Click **+ Add**.</span></span>

1. <span data-ttu-id="42c34-378">På hello **Lägg till hälsoavsökningen** bladet <a name="probe"> </a>ange hello hälsa avsökningen parametrar:</span><span class="sxs-lookup"><span data-stu-id="42c34-378">On hello **Add health probe** blade, <a name="probe"></a>Set hello health probe parameters:</span></span>

   - <span data-ttu-id="42c34-379">**Namnet**: ett namn för hello hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="42c34-379">**Name**: A name for hello health probe.</span></span>
   - <span data-ttu-id="42c34-380">**Protokollet**: TCP.</span><span class="sxs-lookup"><span data-stu-id="42c34-380">**Protocol**: TCP.</span></span>
   - <span data-ttu-id="42c34-381">**Port**: Ange tooan tillgängliga TCP-port.</span><span class="sxs-lookup"><span data-stu-id="42c34-381">**Port**: Set tooan available TCP port.</span></span> <span data-ttu-id="42c34-382">Den här porten kräver en öppen brandväggsport.</span><span class="sxs-lookup"><span data-stu-id="42c34-382">This port requires an open firewall port.</span></span> <span data-ttu-id="42c34-383">Använd hello [samma port](#ports) du angiven för hello hälsoavsökningen hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="42c34-383">Use hello [same port](#ports) you set for hello health probe at hello firewall.</span></span>
   - <span data-ttu-id="42c34-384">**Intervallet**: 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="42c34-384">**Interval**: 5 Seconds.</span></span>
   - <span data-ttu-id="42c34-385">**Tröskelvärde för ohälsosamt värde**: 2 upprepade fel.</span><span class="sxs-lookup"><span data-stu-id="42c34-385">**Unhealthy threshold**: 2 consecutive failures.</span></span>

1. <span data-ttu-id="42c34-386">Klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="42c34-386">Click OK.</span></span>

### <a name="set-load-balancing-rules"></a><span data-ttu-id="42c34-387">Ange regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="42c34-387">Set load balancing rules</span></span>

1. <span data-ttu-id="42c34-388">På hello belastningen belastningsutjämnaren bladet, klickar du på **belastningsutjämningsregler**.</span><span class="sxs-lookup"><span data-stu-id="42c34-388">On hello load balancer blade, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="42c34-389">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="42c34-389">Click **+ Add**.</span></span>

1. <span data-ttu-id="42c34-390">Ange hello belastningsutjämning regler Startparametrar:</span><span class="sxs-lookup"><span data-stu-id="42c34-390">Set hello load balancing rules parameters:</span></span>

   - <span data-ttu-id="42c34-391">**Namnet**: ett namn för hello regler för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="42c34-391">**Name**: A name for hello load balancing rules.</span></span>
   - <span data-ttu-id="42c34-392">**Frontend-IP-adressen**: använda hello IP-adress för hello nätverksresurs för SQL Server FCI-kluster.</span><span class="sxs-lookup"><span data-stu-id="42c34-392">**Frontend IP address**: Use hello IP address for hello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="42c34-393">**Port**: för hello SQL Server FCI TCP-port.</span><span class="sxs-lookup"><span data-stu-id="42c34-393">**Port**: Set for hello SQL Server FCI TCP port.</span></span> <span data-ttu-id="42c34-394">hello instans standardporten är 1433.</span><span class="sxs-lookup"><span data-stu-id="42c34-394">hello default instance port is 1433.</span></span>
   - <span data-ttu-id="42c34-395">**Serverdelsport**: det här värdet används hello samma port som hello **Port** värdet när du aktiverar **flytande IP (direkt serverreturnering)**.</span><span class="sxs-lookup"><span data-stu-id="42c34-395">**Backend port**: This value uses hello same port as hello **Port** value when you enable **Floating IP (direct server return)**.</span></span>
   - <span data-ttu-id="42c34-396">**Serverdelspool**: Använd hello backend namn som du tidigare har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="42c34-396">**Backend pool**: Use hello backend pool name that you configured earlier.</span></span>
   - <span data-ttu-id="42c34-397">**Hälsoavsökningen**: Använd hello hälsoavsökningen som du tidigare har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="42c34-397">**Health probe**: Use hello health probe that you configured earlier.</span></span>
   - <span data-ttu-id="42c34-398">**Sessionspersistence**: Ingen.</span><span class="sxs-lookup"><span data-stu-id="42c34-398">**Session persistence**: None.</span></span>
   - <span data-ttu-id="42c34-399">**Inaktivitetstid (minuter)**: 4.</span><span class="sxs-lookup"><span data-stu-id="42c34-399">**Idle timeout (minutes)**: 4.</span></span>
   - <span data-ttu-id="42c34-400">**Flytande IP (direkt serverreturnering)**: aktiverat</span><span class="sxs-lookup"><span data-stu-id="42c34-400">**Floating IP (direct server return)**: Enabled</span></span>

1. <span data-ttu-id="42c34-401">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="42c34-401">Click **OK**.</span></span>

## <a name="step-6-configure-cluster-for-probe"></a><span data-ttu-id="42c34-402">Steg 6: Konfigurera kluster för avsökningen</span><span class="sxs-lookup"><span data-stu-id="42c34-402">Step 6: Configure cluster for probe</span></span>

<span data-ttu-id="42c34-403">Ange hello klustret avsökningen Portparametern i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="42c34-403">Set hello cluster probe port parameter in PowerShell.</span></span>

<span data-ttu-id="42c34-404">tooset Hej klustret avsökningen Portparametern, uppdatera variabler i hello följande skript från din miljö.</span><span class="sxs-lookup"><span data-stu-id="42c34-404">tooset hello cluster probe port parameter, update variables in hello following script from your environment.</span></span>

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a><span data-ttu-id="42c34-405">Steg 7: Testa redundans för FCI</span><span class="sxs-lookup"><span data-stu-id="42c34-405">Step 7: Test FCI failover</span></span>

<span data-ttu-id="42c34-406">Testa redundans för hello FCI toovalidate klusterfunktionaliteten.</span><span class="sxs-lookup"><span data-stu-id="42c34-406">Test failover of hello FCI toovalidate cluster functionality.</span></span> <span data-ttu-id="42c34-407">Hej gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="42c34-407">Do hello following steps:</span></span>

1. <span data-ttu-id="42c34-408">Ansluta tooone hello SQL Server FCI klusternoder med RDP.</span><span class="sxs-lookup"><span data-stu-id="42c34-408">Connect tooone of hello SQL Server FCI cluster nodes with RDP.</span></span>

1. <span data-ttu-id="42c34-409">Öppna **Klusterhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="42c34-409">Open **Failover Cluster Manager**.</span></span> <span data-ttu-id="42c34-410">Klicka på **roller**.</span><span class="sxs-lookup"><span data-stu-id="42c34-410">Click **Roles**.</span></span> <span data-ttu-id="42c34-411">Observera vilken nod som äger hello FCI för SQL Server-rollen.</span><span class="sxs-lookup"><span data-stu-id="42c34-411">Notice which node owns hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="42c34-412">Högerklicka på hello FCI för SQL Server-rollen.</span><span class="sxs-lookup"><span data-stu-id="42c34-412">Right-click hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="42c34-413">Klicka på **flytta** och på **bästa möjliga nod**.</span><span class="sxs-lookup"><span data-stu-id="42c34-413">Click **Move** and click **Best Possible Node**.</span></span>

<span data-ttu-id="42c34-414">**Hanteraren för redundanskluster** visar hello rollen och dess resurser i offlineläge.</span><span class="sxs-lookup"><span data-stu-id="42c34-414">**Failover Cluster Manager** shows hello role and its resources go offline.</span></span> <span data-ttu-id="42c34-415">hello resurser sedan flytta och anslutas på hello andra noden.</span><span class="sxs-lookup"><span data-stu-id="42c34-415">hello resources then move and come online on hello other node.</span></span>

### <a name="test-connectivity"></a><span data-ttu-id="42c34-416">Testa anslutning</span><span class="sxs-lookup"><span data-stu-id="42c34-416">Test connectivity</span></span>

<span data-ttu-id="42c34-417">logga in tooanother virtuell dator i hello-tootest anslutning samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="42c34-417">tootest connectivity, log in tooanother virtual machine in hello same virtual network.</span></span> <span data-ttu-id="42c34-418">Öppna **SQL Server Management Studio** och ansluta toohello SQL Server FCI namn.</span><span class="sxs-lookup"><span data-stu-id="42c34-418">Open **SQL Server Management Studio** and connect toohello SQL Server FCI name.</span></span>

>[!NOTE]
><span data-ttu-id="42c34-419">Om nödvändigt, kan du [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c34-419">If necessary, you can [download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="limitations"></a><span data-ttu-id="42c34-420">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="42c34-420">Limitations</span></span>
<span data-ttu-id="42c34-421">På Azure virtual machines stöds inte Microsoft Distributed Transaction Coordinator (DTC) på FCIs eftersom hello RPC-porten inte stöds av hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="42c34-421">On Azure virtual machines, Microsoft Distributed Transaction Coordinator (DTC) is not supported on FCIs because hello RPC port is not supported by hello load balancer.</span></span>

## <a name="see-also"></a><span data-ttu-id="42c34-422">Se även</span><span class="sxs-lookup"><span data-stu-id="42c34-422">See Also</span></span>

[<span data-ttu-id="42c34-423">Konfigurera S2D med fjärrskrivbord (Azure)</span><span class="sxs-lookup"><span data-stu-id="42c34-423">Setup S2D with remote desktop (Azure)</span></span>](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

<span data-ttu-id="42c34-424">[Hyper-Konvergerad lösning med lagringsutrymmen direkt](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="42c34-424">[Hyper-converged solution with storage spaces direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span></span>

[<span data-ttu-id="42c34-425">Direkt översikt över lagringsutrymmen i</span><span class="sxs-lookup"><span data-stu-id="42c34-425">Storage Space Direct Overview</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[<span data-ttu-id="42c34-426">SQL Server-stöd för S2D</span><span class="sxs-lookup"><span data-stu-id="42c34-426">SQL Server support for S2D</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
