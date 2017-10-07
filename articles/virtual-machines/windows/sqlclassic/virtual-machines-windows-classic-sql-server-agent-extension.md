---
title: "aaaAutomate hanteringsuppgifter på SQL virtuella datorer (klassisk) | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toomanage hello SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder hello klassisk distribution läge."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a>Automatisera hanteringsuppgifter på Azure Virtual Machines med hello tillägg för SQL Server Agent (klassisk)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisk](../classic/sql-server-agent-extension.md)
> 
>
 
hello tillägg för SQL Server IaaS Agent (SQLIaaSAgent) körs på virtuella Azure-datorer tooautomate administrationsuppgifter. Det här avsnittet innehåller en översikt över hello-tjänster som stöds av hello tillägget samt anvisningar för installation, status och borttagning.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. tooview hello Resource Manager-versionen av den här artikeln finns [SQL Server Agent-tillägg för SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Tjänster som stöds
hello tillägg för SQL Server IaaS Agent stöder hello följande uppgifter:

| Funktionen för administration | Beskrivning |
| --- | --- |
| **Automatisk SQL-säkerhetskopiering** |Automatiserar hello schemaläggning av säkerhetskopieringar för alla databaser för hello standardinstansen av SQL Server i hello VM. Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-backup.md). |
| **Automatisk SQL-uppdatering** |Konfigurerar en underhållsperiod tooyour VM ta placera under vilka uppdateringar, så att du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning. Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-patching.md). |
| **Azure Key Vault-integrering** |Aktiverar du tooautomatically installera och konfigurera Azure Key Vault på SQL Server-VM. Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (klassisk)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Krav
Krav för toouse hello SQL Server IaaS Agent tillägget på den virtuella datorn:

### <a name="operating-system"></a>Operativsystem:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server-versioner:
* SQLServer 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[Hämta och konfigurera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).

Starta Windows PowerShell och ansluta tooyour Azure-prenumeration med hello **Add-AzureAccount** kommando.

    Add-AzureAccount

Om du har flera prenumerationer använder **Välj AzureSubscription** tooselect hello prenumeration som innehåller mål-klassiska VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Nu kan du hämta en lista över hello klassiska virtuella datorer och deras associerade tjänstnamn med hello **Get-AzureVM** kommando.

    Get-AzureVM

## <a name="installation"></a>Installation
Klassiska virtuella datorer, måste du använda PowerShell tooinstall hello tillägg för SQL Server IaaS Agent och konfigurera tillhörande tjänster. Använd hello **Set AzureVMSqlServerExtension** PowerShell cmdlet tooinstall hello tillägg. Följande kommando hello installerar hello tillägg på en Windows Server-VM (klassisk) och kallar det ”SQLIaaSExtension”.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Om du uppdaterar toohello senaste versionen av hello SQL IaaS Agent tillägget måste du starta om den virtuella datorn när du har uppdaterat hello-tillägget.

> [!NOTE]
> Klassiska virtuella datorer inte har en alternativet tooinstall och konfigurera hello SQL IaaS Agent tillägget hello-portalen.
> 
> 

## <a name="status"></a>Status
Enkelriktade tooverify att hello tillägg har installerats är tooview hello agentens status hello Azure-portalen. Välj en virtuell dator som anges i hello virtuella bladet och klicka sedan på **tillägg**. Du bör se hello **SQLIaaSAgent** tillägg i listan.

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Du kan också använda hello **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Borttagning
I hello Azure-portalen, du kan avinstallera hello tillägg genom att klicka på hello ellips på hello **tillägg** bladet för de virtuella datorn. Klicka på **avinstallera**.

![Avinstallera hello SQL Server IaaS Agent tillägget i Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Du kan också använda hello **ta bort AzureVMSqlServerExtension** Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Nästa steg
Börja använda någon av hello-tjänster som stöds av hello extension. Mer information finns i hello information som refereras i hello [-tjänster som stöds](#supported-services) i den här artikeln.

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

