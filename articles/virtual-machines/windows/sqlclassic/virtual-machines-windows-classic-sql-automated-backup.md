---
title: "aaaAutomated säkerhetskopiering för SQL Server-datorer (klassisk) | Microsoft Docs"
description: "Förklarar hello automatisk säkerhetskopiering för SQL Server som körs i Azure Virtual Machines med Resource Manager. "
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 5d8f0412578c2d86edc6e54093a5da4891d3847e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatisk säkerhetskopiering för SQLServer på virtuella Azure-datorer (klassisk)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klassisk](../classic/sql-automated-backup.md)
> 
> 

Automatisk säkerhetskopiering konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure-dator som kör SQL Server 2014 Standard eller Enterprise. Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage. Automatisk säkerhetskopiering är beroende av hello [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. tooview hello Resource Manager-versionen av den här artikeln finns [automatisk säkerhetskopiering för SQL Server i Azure virtuella datorer Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Krav
toouse automatisk säkerhetskopiering Tänk hello följande krav:

**Operativsystemet**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-version/utgåva**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> SQL Server 2016 ännu inte har stöd för automatisk säkerhetskopiering.
> 
> 

**Databaskonfiguration**:

* Måldatabaserna måste använda hello fullständiga återställningsmodellen.

**Azure PowerShell**:

* [Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).

**SQL Server IaaS tillägget**:

* [Installera hello SQL Server IaaS tillägget](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Inställningar
hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering. För klassiska virtuella datorer kan använda du PowerShell tooconfigure inställningarna.

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **Automatisk säkerhetskopiering** |Aktivera/inaktivera (inaktiverat) |Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2014 Standard eller Enterprise. |
| **Kvarhållningsperioden** |1 – 30 dagar (30 dagar) |hello antal dagar tooretain en säkerhetskopia. |
| **Storage-konto** |Azure storage-konto (hello-lagringskonto som skapats för hello angivna VM) |Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage. En behållare skapas på den här platsen toostore säkerhetskopior. hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och namnet på datorn. |
| **Kryptering** |Aktivera/inaktivera (inaktiverat) |Aktiverar eller inaktiverar kryptering. Om kryptering har aktiverats, hello certifikat används toorestore hello säkerhetskopiering finns i hello ange storage-konto i hello samma automaticbackup behållaren med hjälp av hello samma namngivningskonvention. Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior. |
| **Lösenord** |Lösenord text (ingen) |Ett lösenord för krypteringsnycklar. Detta är endast krävs om kryptering är aktiverat. I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades. | **Säkerhetskopieringssystem databaser** | Aktivera/inaktivera (inaktiverat) | Ta och fullständiga säkerhetskopieringar av Master, Model och MSDB |
| **Konfigurera schemat för säkerhetskopiering** | Manuell/automatisk (automatisk) | Välj **automatisk** tooautomatically ta fullständig och loggsäkerhetskopior baserat på loggen tillväxt. Välj **manuell** toospecify hello schemat för fullständig och loggsäkerhetskopior. |

## <a name="configuration-with-powershell"></a>Med PowerShell
I följande exempel PowerShell hello, har automatisk säkerhetskopiering konfigurerats för en befintlig SQL Server 2014-VM. Hej **ny AzureVMSqlServerAutoBackupConfig** kommando konfigurerar hello automatisk säkerhetskopiering inställningar toostore säkerhetskopieringar i hello Azure storage-konto som anges av hello $storageaccount variabeln. Dessa säkerhetskopior ska behållas i 10 dagar. Hej **Set AzureVMSqlServerExtension** kommandot uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.

tooenable kryptering, ändra hello tidigare toopass hello EnableEncryption skriptparameter tillsammans med ett lösenord (säker sträng) för hello CertificatePassword parameter. hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

toodisable automatisk säkerhetskopiering, kör hello samma skript utan hello **-aktivera** parametern toohello **ny AzureVMSqlServerAutoBackupConfig**. Det kan ta flera minuter toodisable automatisk säkerhetskopiering som installationen.

> [!NOTE]
> Inaktivering och avinstallerar hello SQL Server IaaS-Agent tas inte bort hello som tidigare har konfigurerat inställningarna för hanterad säkerhetskopiering. Du bör inaktivera automatisk säkerhetskopiering innan du inaktiverar eller avinstallerar hello SQL Server IaaS-Agent.
> 
> 

## <a name="next-steps"></a>Nästa steg
Automatisk säkerhetskopiering konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer. Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.

Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

