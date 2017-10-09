---
title: "aaaAutomate hanteringsuppgifter på SQL virtuella datorer (Resource Manager) | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toomanage hello SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder läget för hello Resource Manager distribution."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a>Automatisera hanteringsuppgifter på Azure Virtual Machines med hello tillägg för SQL Server Agent (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisk](../classic/sql-server-agent-extension.md)
> 
> 

hello tillägg för SQL Server IaaS Agent (SQLIaaSExtension) körs på virtuella Azure-datorer tooautomate administrationsuppgifter. Det här avsnittet innehåller en översikt över hello-tjänster som stöds av hello tillägget samt anvisningar för installation, status och borttagning.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello klassiska versionen av den här artikeln finns [SQL Server Agent-tillägget för SQL Server VMs klassiska](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Tjänster som stöds
hello tillägg för SQL Server IaaS Agent stöder hello följande uppgifter:

| Funktionen för administration | Beskrivning |
| --- | --- |
| **Automatisk SQL-säkerhetskopiering** |Automatiserar hello schemaläggning av säkerhetskopieringar för alla databaser för hello standardinstansen av SQL Server i hello VM. Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisk SQL-uppdatering** |Konfigurerar en underhållsperiod tooyour VM ta placera under vilka uppdateringar, så att du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning. Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Key Vault-integrering** |Aktiverar du tooautomatically installera och konfigurera Azure Key Vault på SQL Server-VM. Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

När installerad och körs, tillgängliggör hello tillägg för SQL Server IaaS Agent dessa funktioner för administration på hello SQL Server-panelen på hello virtuell dator i hello Azure-portalen och via Azure PowerShell för SQL Server marketplace-bilder och via Azure PowerShell för manuella installationer hello-tillägget. 

## <a name="prerequisites"></a>Krav
Krav för toouse hello SQL Server IaaS Agent tillägget på den virtuella datorn:

**Operativsystemet**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versioner**:

* SQLServer 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Hämta och konfigurera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview)

## <a name="installation"></a>Installation
hello tillägg för SQL Server IaaS Agent installeras automatiskt när du etablerar en hello SQL Server virtuella galleriavbildningar. Om du behöver tooreinstall hello tillägget manuellt på något av dessa SQL Server-datorer kan använda hello följande PowerShell-kommando:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

Det är också möjligt tooinstall hello tillägg för SQL Server IaaS Agent på en virtuell dator bara Operativsystemet Windows Server. Detta stöds endast om du har installerat SQL Server manuellt på den datorn. Och sedan installera hello tillägget manuellt med hjälp av hello samma **Set AzureVMSqlServerExtension** PowerShell-cmdlet.

> [!NOTE]
> Om du manuellt installera hello tillägg för SQL Server IaaS Agent på en OS-endast Windows Server VM, kan du inte hantera hello SQL Server-konfigurationsinställningar via hello Azure-portalen. I det här scenariot måste du göra alla ändringar med PowerShell.

## <a name="status"></a>Status
Enkelriktade tooverify att hello tillägg har installerats är tooview hello agentens status hello Azure-portalen. Välj **alla inställningar** i hello virtuella bladet och klicka sedan på **tillägg**. Du bör se hello **SQLIaaSExtension** tillägg i listan.

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Du kan också använda hello **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

hello föregående kommando bekräftar hello-agenten är installerad och ger allmän statusinformation. Du kan också få statusinformation om automatisk säkerhetskopiering och korrigering med hello följande kommandon.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Borttagning
I hello Azure-portalen, du kan avinstallera hello tillägg genom att klicka på hello ellips på hello **tillägg** bladet för de virtuella datorn. Klicka på **ta bort**.

![Avinstallera hello SQL Server IaaS Agent tillägget i Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Du kan också använda hello **ta bort AzureRmVMSqlServerExtension** Powershell-cmdlet.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Nästa steg
Börja använda någon av hello-tjänster som stöds av hello extension. Mer information finns i hello information som refereras i hello [-tjänster som stöds](#supported-services) i den här artikeln.

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

