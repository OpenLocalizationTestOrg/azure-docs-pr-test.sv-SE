---
title: "aaaAutomated korrigering för SQL Server-datorer (Resource Manager) | Microsoft Docs"
description: "Beskriver funktionen för automatisk uppdatering av hello för SQL Server virtuella datorer som körs i Azure med hjälp av hanteraren för filserverresurser."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatisk uppdatering av SQL Server i Azure Virtual Machines (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klassisk](../classic/sql-automated-patching.md)
> 
> 

Automatisk korrigering upprättar en underhållsperiod för en Azure-dator som kör SQL Server. Automatiska uppdateringar kan endast installeras under den här underhållsperioden. För SQL Server säkerställer den här rescriction att uppdateringar och eventuella tillhörande omstarter inträffar på hello bästa möjliga tid för hello-databasen. Automatisk korrigering beror på hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello klassiska versionen av den här artikeln finns [automatisk uppdatering för SQL Server i Azure virtuella datorer klassiska](../classic/sql-automated-patching.md).

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

* [Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview) om du planerar tooconfigure automatisk uppdatering med PowerShell.

> [!NOTE]
> Automatisk korrigering beroende hello tillägg för SQL Server IaaS Agent. Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard. Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Inställningar
hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk uppdatering. hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.

| Inställning | Möjliga värden | Beskrivning |
| --- | --- | --- |
| **Automatisk uppdatering** |Aktivera/inaktivera (inaktiverat) |Aktiverar eller inaktiverar automatisk uppdatering för en virtuell Azure-dator. |
| **Underhållsschema** |Varje dag, måndag, tisdag, onsdag, torsdag, fredag, lördag, söndag |hello schema för att hämta och installera Windows, SQL Server och Microsoft-uppdateringar för den virtuella datorn. |
| **Starttimme för underhåll** |0-24 |hello lokala start tid tooupdate hello virtuella datorn. |
| **Underhåll fönstervaraktigheten** |30-180 |hello antal minuter som tillåts toocomplete hello hämtning och installation av uppdateringar. |
| **Patch-kategori** |Viktigt |hello kategori uppdateringar toodownload och installation. |

## <a name="configuration-in-hello-portal"></a>Konfigurationen i hello Portal
Du kan använda hello Azure portal tooconfigure automatisk uppdatering under etableringen eller för befintliga virtuella datorer.

### <a name="new-vms"></a>Nya virtuella datorer
Använd hello Azure portal tooconfigure automatisk uppdatering när du skapar en ny virtuell dator för SQL Server i hello Resource Manager-distributionsmodellen.

I hello **SQL Server-inställningar** bladet väljer **automatisk uppdatering**. hello följande Azure portal skärmbild visar hello **SQL automatisk uppdatering** bladet.

![SQL automatisk uppdatering i Azure-portalen](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Befintliga virtuella datorer
Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer. Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.

![SQL automatisk uppdatering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk uppdatering avsnitt.

![Konfigurera SQL automatisk uppdatering för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.

Om du aktiverar automatisk uppdatering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund. Under denna tid kanske hello Azure-portalen inte visar att automatisk uppdatering har konfigurerats. Vänta några minuter för hello agent toobe installerat kan konfigureras. Efter att hello Azure visar portal hello nya inställningar.

> [!NOTE]
> Du kan också konfigurera automatisk uppdatering med hjälp av en mall. Mer information finns i [Azure quickstart-mall för automatisk uppdatering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>Med PowerShell
När du har etablerat din SQL-VM, använder du PowerShell tooconfigure automatisk uppdatering.

I följande exempel hello, PowerShell är används tooconfigure automatisk uppdatering på en befintlig SQL Server-VM. Hej **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** kommando konfigurerar en ny underhållsperiod för automatiska uppdateringar.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Baserat på det här exemplet hello följande tabell beskrivs hello praktiken på hello målet virtuella Azure-datorn:

| Parameter | Verkan |
| --- | --- |
| **DayOfWeek** |Korrigeringsprogram installerade varje torsdag. |
| **MaintenanceWindowStartingHour** |Begin uppdateringar på 11:00:00. |
| **MaintenanceWindowsDuration** |Korrigeringsfiler måste installeras inom 120 minuter. Baserat på hello starttid, måste de utföra av 1:00 pm. |
| **PatchCategory** |Hej bara möjligt inställningen för den här parametern är **viktigt**. |

Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.

toodisable automatisk uppdatering kör hello samma skript utan hello **-aktivera** parametern toohello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Hej frånvaron av hello **-aktivera** parametern signaler hello kommandot toodisable hello funktionen.

## <a name="next-steps"></a>Nästa steg
Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

