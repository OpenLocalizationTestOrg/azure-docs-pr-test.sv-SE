---
title: Ta bort ett Site Recovery-valv
description: "Lär dig hur du tar bort en Azure Site Recovery-valvet, baserat på scenariot Site Recovery."
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
ms.openlocfilehash: b95b9defa0a037f7d7d3ef36b99bc7c53c751050
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="01532-103">Ta bort ett Site Recovery-valv</span><span class="sxs-lookup"><span data-stu-id="01532-103">Delete a Site Recovery vault</span></span>
<span data-ttu-id="01532-104">Beroenden kan hindra dig från att ta bort en Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="01532-104">Dependencies can prevent you from deleting an Azure Site Recovery vault.</span></span> <span data-ttu-id="01532-105">Du behöver vidta åtgärder kan variera, beroende på Site Recovery-scenario: VMware till Azure, Hyper-V (med och utan System Center Virtual Machine Manager) till Azure och Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="01532-105">The actions you need to take vary based on the Site Recovery scenario: VMware to Azure, Hyper-V (with and without System Center Virtual Machine Manager) to Azure, and Azure Backup.</span></span> <span data-ttu-id="01532-106">Om du vill ta bort ett valv som används i Azure Backup finns [ta bort ett säkerhetskopieringsvalv i Azure](../backup/backup-azure-delete-vault.md).</span><span class="sxs-lookup"><span data-stu-id="01532-106">To delete a vault used in Azure Backup, see [Delete a Backup vault in Azure](../backup/backup-azure-delete-vault.md).</span></span>

>[!Important]
><span data-ttu-id="01532-107">Om du testar produkten och inte är orolig för dataförlust, Använd kraft delete-metoden snabbt ta bort valvet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="01532-107">If you're testing the product and aren't concerned about data loss, use the force delete method to rapidly remove the vault and all its dependencies.</span></span>

> <span data-ttu-id="01532-108">PowerShell-kommandot tar bort allt innehåll på valvet och går inte att ångra.</span><span class="sxs-lookup"><span data-stu-id="01532-108">The PowerShell command deletes all the contents of the vault and is not reversible.</span></span>

## <a name="use-powershell-to-force-delete-the-vault"></a><span data-ttu-id="01532-109">Använd PowerShell för att tvinga ta bort valvet</span><span class="sxs-lookup"><span data-stu-id="01532-109">Use PowerShell to force delete the vault</span></span> 

<span data-ttu-id="01532-110">Ta bort Site Recovery-valvet även om det finns skyddade objekt genom att använda dessa kommandon:</span><span class="sxs-lookup"><span data-stu-id="01532-110">To delete the Site Recovery vault even if there are protected items, use these commands:</span></span>

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="01532-111">Ta bort ett Site Recovery-valv</span><span class="sxs-lookup"><span data-stu-id="01532-111">Delete a Site Recovery vault</span></span> 
<span data-ttu-id="01532-112">Följ rekommenderade åtgärder för ditt scenario om du vill ta bort valvet.</span><span class="sxs-lookup"><span data-stu-id="01532-112">To delete the vault, follow the recommended steps for your scenario.</span></span>

### <a name="vmware-vms-to-azure"></a><span data-ttu-id="01532-113">Virtuella VMware-datorer till Azure</span><span class="sxs-lookup"><span data-stu-id="01532-113">VMware VMs to Azure</span></span>

1. <span data-ttu-id="01532-114">Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="01532-114">Delete all protected VMs by following the steps in [Disable protection for a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="01532-115">Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="01532-115">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="01532-116">Ta bort referenser till vCenter genom att följa stegen i [ta bort en vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span><span class="sxs-lookup"><span data-stu-id="01532-116">Delete references to vCenter by following the steps in [Delete a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span></span>

4. <span data-ttu-id="01532-117">Ta bort konfigurationsservern genom att följa stegen i [inaktivera en konfigurationsserver](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="01532-117">Delete the configuration server by following the steps in [Decommission a configuration server](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span></span>

5. <span data-ttu-id="01532-118">Ta bort valvet.</span><span class="sxs-lookup"><span data-stu-id="01532-118">Delete the vault.</span></span>


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a><span data-ttu-id="01532-119">Hyper-V-datorer (med Virtual Machine Manager) till Azure</span><span class="sxs-lookup"><span data-stu-id="01532-119">Hyper-V VMs (with Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="01532-120">Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="01532-120">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="01532-121">Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="01532-121">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3.  <span data-ttu-id="01532-122">Ta bort referenser till Virtual Machine Manager-servrar genom att följa stegen i [avregistrera en VMM-server med anslutna](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span><span class="sxs-lookup"><span data-stu-id="01532-122">Delete references to Virtual Machine Manager servers by following the steps in [Unregister a connected VMM server](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span></span>

4.  <span data-ttu-id="01532-123">Ta bort valvet.</span><span class="sxs-lookup"><span data-stu-id="01532-123">Delete the vault.</span></span>

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a><span data-ttu-id="01532-124">Hyper-V-datorer (utan Virtual Machine Manager) till Azure</span><span class="sxs-lookup"><span data-stu-id="01532-124">Hyper-V VMs (without Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="01532-125">Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="01532-125">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="01532-126">Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="01532-126">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="01532-127">Ta bort referenser till Hyper-V-servrar genom att följa stegen i [avregistrera Hyper-V-värd](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span><span class="sxs-lookup"><span data-stu-id="01532-127">Delete references to Hyper-V servers by following the steps in [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span></span>

4. <span data-ttu-id="01532-128">Ta bort Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="01532-128">Delete the Hyper-V site.</span></span>

5. <span data-ttu-id="01532-129">Ta bort valvet.</span><span class="sxs-lookup"><span data-stu-id="01532-129">Delete the vault.</span></span>
