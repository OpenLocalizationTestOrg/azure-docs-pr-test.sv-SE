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
# <a name="delete-a-site-recovery-vault"></a>Ta bort ett Site Recovery-valv
Beroenden kan hindra dig från att ta bort en Azure Site Recovery-valvet. Du behöver vidta åtgärder kan variera, beroende på Site Recovery-scenario: VMware till Azure, Hyper-V (med och utan System Center Virtual Machine Manager) till Azure och Azure Backup. Om du vill ta bort ett valv som används i Azure Backup finns [ta bort ett säkerhetskopieringsvalv i Azure](../backup/backup-azure-delete-vault.md).

>[!Important]
>Om du testar produkten och inte är orolig för dataförlust, Använd kraft delete-metoden snabbt ta bort valvet och dess beroenden.

> PowerShell-kommandot tar bort allt innehåll på valvet och går inte att ångra.

## <a name="use-powershell-to-force-delete-the-vault"></a>Använd PowerShell för att tvinga ta bort valvet 

Ta bort Site Recovery-valvet även om det finns skyddade objekt genom att använda dessa kommandon:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a>Ta bort ett Site Recovery-valv 
Följ rekommenderade åtgärder för ditt scenario om du vill ta bort valvet.

### <a name="vmware-vms-to-azure"></a>Virtuella VMware-datorer till Azure

1. Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Ta bort referenser till vCenter genom att följa stegen i [ta bort en vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. Ta bort konfigurationsservern genom att följa stegen i [inaktivera en konfigurationsserver](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).

5. Ta bort valvet.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Hyper-V-datorer (med Virtual Machine Manager) till Azure
1. Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3.  Ta bort referenser till Virtual Machine Manager-servrar genom att följa stegen i [avregistrera en VMM-server med anslutna](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).

4.  Ta bort valvet.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Hyper-V-datorer (utan Virtual Machine Manager) till Azure
1. Ta bort alla skyddade virtuella datorer genom att följa stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Ta bort referenser till Hyper-V-servrar genom att följa stegen i [avregistrera Hyper-V-värd](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Ta bort Hyper-V-platsen.

5. Ta bort valvet.
