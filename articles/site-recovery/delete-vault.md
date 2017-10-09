---
title: aaaDelete Site Recovery-valvet
description: "Lär dig hur toodelete en Azure Site Recovery-valvet, baserat på hello Site Recovery scenario."
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: 36db202d8b790bb5d31d1348fb72f51acb5d559c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="fe617-103">Ta bort ett Site Recovery-valv</span><span class="sxs-lookup"><span data-stu-id="fe617-103">Delete a Site Recovery vault</span></span>
<span data-ttu-id="fe617-104">Beroenden kan hindra dig från att ta bort en Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="fe617-104">Dependencies can prevent you from deleting an Azure Site Recovery vault.</span></span> <span data-ttu-id="fe617-105">hello åtgärder du måste tootake beror på hello Site Recovery-scenario: VMware tooAzure tooAzure för Hyper-V (med och utan System Center Virtual Machine Manager) och Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="fe617-105">hello actions you need tootake vary based on hello Site Recovery scenario: VMware tooAzure, Hyper-V (with and without System Center Virtual Machine Manager) tooAzure, and Azure Backup.</span></span> <span data-ttu-id="fe617-106">toodelete ett valv som används i Azure Backup finns [ta bort ett säkerhetskopieringsvalv i Azure](../backup/backup-azure-delete-vault.md).</span><span class="sxs-lookup"><span data-stu-id="fe617-106">toodelete a vault used in Azure Backup, see [Delete a Backup vault in Azure](../backup/backup-azure-delete-vault.md).</span></span>

>[!Important]
><span data-ttu-id="fe617-107">Om du testar hello produkten och inte är orolig för dataförlust, Använd hello Tvingad borttagning metoden toorapidly bort hello valvet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="fe617-107">If you're testing hello product and aren't concerned about data loss, use hello force delete method toorapidly remove hello vault and all its dependencies.</span></span>

> <span data-ttu-id="fe617-108">hello PowerShell-kommando tar bort alla hello innehållet i hello valvet och går inte att ångra.</span><span class="sxs-lookup"><span data-stu-id="fe617-108">hello PowerShell command deletes all hello contents of hello vault and is not reversible.</span></span>

## <a name="use-powershell-tooforce-delete-hello-vault"></a><span data-ttu-id="fe617-109">Använd PowerShell tooforce ta bort hello valv</span><span class="sxs-lookup"><span data-stu-id="fe617-109">Use PowerShell tooforce delete hello vault</span></span> 

<span data-ttu-id="fe617-110">toodelete hello Site Recovery-valvet även om det finns skyddade objekt, använda följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="fe617-110">toodelete hello Site Recovery vault even if there are protected items, use these commands:</span></span>

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="fe617-111">Ta bort ett Site Recovery-valv</span><span class="sxs-lookup"><span data-stu-id="fe617-111">Delete a Site Recovery vault</span></span> 
<span data-ttu-id="fe617-112">toodelete hello valvet, följ hello rekommenderade steg för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="fe617-112">toodelete hello vault, follow hello recommended steps for your scenario.</span></span>

### <a name="vmware-vms-tooazure"></a><span data-ttu-id="fe617-113">TooAzure för virtuella VMware-datorer</span><span class="sxs-lookup"><span data-stu-id="fe617-113">VMware VMs tooAzure</span></span>

1. <span data-ttu-id="fe617-114">Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="fe617-114">Delete all protected VMs by following hello steps in [Disable protection for a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="fe617-115">Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="fe617-115">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="fe617-116">Ta bort referenser toovCenter genom följande hello stegen i [ta bort en vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span><span class="sxs-lookup"><span data-stu-id="fe617-116">Delete references toovCenter by following hello steps in [Delete a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span></span>

4. <span data-ttu-id="fe617-117">Ta bort hello konfigurationsservern genom att följa stegen hello i [inaktivera en konfigurationsserver](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="fe617-117">Delete hello configuration server by following hello steps in [Decommission a configuration server](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span></span>

5. <span data-ttu-id="fe617-118">Ta bort hello valv.</span><span class="sxs-lookup"><span data-stu-id="fe617-118">Delete hello vault.</span></span>


### <a name="hyper-v-vms-with-virtual-machine-manager-tooazure"></a><span data-ttu-id="fe617-119">Hyper-V-datorer (med Virtual Machine Manager) tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe617-119">Hyper-V VMs (with Virtual Machine Manager) tooAzure</span></span>
1. <span data-ttu-id="fe617-120">Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="fe617-120">Delete all protected VMs by following hello steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="fe617-121">Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="fe617-121">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3.  <span data-ttu-id="fe617-122">Ta bort refererar till tooVirtual Machine Manager-servrar genom att följa stegen hello i [avregistrera en VMM-server med anslutna](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span><span class="sxs-lookup"><span data-stu-id="fe617-122">Delete references tooVirtual Machine Manager servers by following hello steps in [Unregister a connected VMM server](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span></span>

4.  <span data-ttu-id="fe617-123">Ta bort hello valv.</span><span class="sxs-lookup"><span data-stu-id="fe617-123">Delete hello vault.</span></span>

### <a name="hyper-v-vms-without-virtual-machine-manager-tooazure"></a><span data-ttu-id="fe617-124">Hyper-V-datorer (utan Virtual Machine Manager) tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe617-124">Hyper-V VMs (without Virtual Machine Manager) tooAzure</span></span>
1. <span data-ttu-id="fe617-125">Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="fe617-125">Delete all protected VMs by following hello steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="fe617-126">Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="fe617-126">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="fe617-127">Ta bort refererar till tooHyper-V-servrar genom att följa stegen hello i [avregistrera Hyper-V-värd](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span><span class="sxs-lookup"><span data-stu-id="fe617-127">Delete references tooHyper-V servers by following hello steps in [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span></span>

4. <span data-ttu-id="fe617-128">Ta bort hello Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="fe617-128">Delete hello Hyper-V site.</span></span>

5. <span data-ttu-id="fe617-129">Ta bort hello valv.</span><span class="sxs-lookup"><span data-stu-id="fe617-129">Delete hello vault.</span></span>
