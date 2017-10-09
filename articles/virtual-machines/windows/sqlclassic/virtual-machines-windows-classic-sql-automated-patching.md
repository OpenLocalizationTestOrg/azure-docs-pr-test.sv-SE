---
title: "aaaAutomated korrigering för SQL Server-datorer (klassisk) | Microsoft Docs"
description: "Beskriver funktionen för automatisk uppdatering av hello för SQL Server virtuella datorer som körs i Azure med hjälp av hello klassisk distribution läge."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatisk uppdatering för SQLServer på virtuella Azure-datorer (klassisk)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klassisk](../classic/sql-automated-patching.md)
> 
> 

Automatisk korrigering upprättar en underhållsperiod för en Azure-dator som kör SQL Server. Automatiska uppdateringar kan endast installeras under den här underhållsperioden. För SQL Server säkerställer detta att uppdateringar och eventuella tillhörande omstarter inträffar på hello bästa möjliga tid för hello-databasen. Automatisk korrigering beror på hello [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. tooview hello Resource Manager-versionen av den här artikeln finns [automatisk uppdatering för SQL Server i Azure virtuella datorer Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Krav
toouse automatisk uppdatering, Överväg hello följande krav:

**Operativsystemet**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-version**:

* SQLServer 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).

**SQL Server IaaS tillägget**:

* [Installera hello SQL Server IaaS tillägget](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Inställningar
hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk uppdatering. För klassiska virtuella datorer kan använda du PowerShell tooconfigure inställningarna.

| Inställning | Möjliga värden | Beskrivning |
| --- | --- | --- |
| **Automatisk uppdatering** |Aktivera/inaktivera (inaktiverat) |Aktiverar eller inaktiverar automatisk uppdatering för en virtuell Azure-dator. |
| **Underhållsschema** |Varje dag, måndag, tisdag, onsdag, torsdag, fredag, lördag, söndag |hello schema för att hämta och installera Windows, SQL Server och Microsoft-uppdateringar för den virtuella datorn. |
| **Starttimme för underhåll** |0-24 |hello lokala start tid tooupdate hello virtuella datorn. |
| **Underhåll fönstervaraktigheten** |30-180 |hello antal minuter som tillåts toocomplete hello hämtning och installation av uppdateringar. |
| **Patch-kategori** |Viktigt |hello kategori uppdateringar toodownload och installation. |

## <a name="configuration-with-powershell"></a>Med PowerShell
I följande exempel hello, PowerShell är används tooconfigure automatisk uppdatering på en befintlig SQL Server-VM. Hej **ny AzureVMSqlServerAutoPatchingConfig** kommando konfigurerar en ny underhållsperiod för automatiska uppdateringar.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Baserat på det här exemplet hello följande tabell beskrivs hello praktiken på hello målet virtuella Azure-datorn:

| Parameter | Verkan |
| --- | --- |
| **DayOfWeek** |Korrigeringsprogram installerade varje torsdag. |
| **MaintenanceWindowStartingHour** |Begin uppdateringar på 11:00:00. |
| **MaintenanceWindowsDuration** |Korrigeringsfiler måste installeras inom 120 minuter. Baserat på hello starttid, måste de utföra av 1:00 pm. |
| **PatchCategory** |hello endast är möjligt för den här parametern ”viktigt”. |

Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.

toodisable automatisk uppdatering kör hello samma skript utan hello - aktivera parametern toohello ny AzureVMSqlServerAutoPatchingConfig. Som med installation, det kan ta flera minuter toodisable automatisk uppdatering.

## <a name="next-steps"></a>Nästa steg
Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

