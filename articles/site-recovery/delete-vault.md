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
# <a name="delete-a-site-recovery-vault"></a>Ta bort ett Site Recovery-valv
Beroenden kan hindra dig från att ta bort en Azure Site Recovery-valvet. hello åtgärder du måste tootake beror på hello Site Recovery-scenario: VMware tooAzure tooAzure för Hyper-V (med och utan System Center Virtual Machine Manager) och Azure Backup. toodelete ett valv som används i Azure Backup finns [ta bort ett säkerhetskopieringsvalv i Azure](../backup/backup-azure-delete-vault.md).

>[!Important]
>Om du testar hello produkten och inte är orolig för dataförlust, Använd hello Tvingad borttagning metoden toorapidly bort hello valvet och dess beroenden.

> hello PowerShell-kommando tar bort alla hello innehållet i hello valvet och går inte att ångra.

## <a name="use-powershell-tooforce-delete-hello-vault"></a>Använd PowerShell tooforce ta bort hello valv 

toodelete hello Site Recovery-valvet även om det finns skyddade objekt, använda följande kommandon:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a>Ta bort ett Site Recovery-valv 
toodelete hello valvet, följ hello rekommenderade steg för ditt scenario.

### <a name="vmware-vms-tooazure"></a>TooAzure för virtuella VMware-datorer

1. Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Ta bort referenser toovCenter genom följande hello stegen i [ta bort en vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. Ta bort hello konfigurationsservern genom att följa stegen hello i [inaktivera en konfigurationsserver](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).

5. Ta bort hello valv.


### <a name="hyper-v-vms-with-virtual-machine-manager-tooazure"></a>Hyper-V-datorer (med Virtual Machine Manager) tooAzure
1. Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3.  Ta bort refererar till tooVirtual Machine Manager-servrar genom att följa stegen hello i [avregistrera en VMM-server med anslutna](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).

4.  Ta bort hello valv.

### <a name="hyper-v-vms-without-virtual-machine-manager-tooazure"></a>Hyper-V-datorer (utan Virtual Machine Manager) tooAzure
1. Ta bort alla skyddade virtuella datorer med hjälp av hello stegen i [inaktivera skyddet för en VMware-VM eller fysisk server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Ta bort alla replikeringsprinciper genom att följa stegen hello i [ta bort en replikeringsprincip](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Ta bort refererar till tooHyper-V-servrar genom att följa stegen hello i [avregistrera Hyper-V-värd](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Ta bort hello Hyper-V-platsen.

5. Ta bort hello valv.
