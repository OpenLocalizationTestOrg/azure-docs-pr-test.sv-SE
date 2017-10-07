---
title: "aaaAutomated säkerhetskopiering för SQL Server 2014 Azure Virtual Machines | Microsoft Docs"
description: "Förklarar hello automatisk säkerhetskopiering för SQL Server 2014 virtuella datorer som körs i Azure. Den här artikeln är särskilda tooVMs med hello Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: c6803d8ef9f80e44a2f87918d87e099f1b562483
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatisk säkerhetskopiering för SQL Server 2014 virtuella datorer (Resource Manager)

> [!div class="op_single_selector"]
> * [SQLServer 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatisk säkerhetskopiering konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure-dator som kör SQL Server 2014 Standard eller Enterprise. Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage. Automatisk säkerhetskopiering är beroende av hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Krav
toouse automatisk säkerhetskopiering Tänk hello följande krav:

**Operativsystemet**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-version/utgåva**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatisk säkerhetskopiering fungerar med SQL Server 2014. Om du använder SQL Server 2016 kan du använda automatisk säkerhetskopiering v2 tooback in dina databaser. Mer information finns i [automatisk säkerhetskopiering v2 för SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

**Databaskonfiguration**:

- Måldatabaserna måste använda hello fullständiga återställningsmodellen. Mer information om hello effekten av hello fullständiga återställningsmodellen säkerhetskopior finns [säkerhetskopiering Under hello fullständiga återställningsmodellen](https://technet.microsoft.com/library/ms190217.aspx).
- Måldatabaserna måste finnas på hello standardinstansen för SQL Server. Hej IaaS-tillägg för SQL Server stöder inte namngivna instanser.

**Azure distributionsmodell**:

- Resource Manager

**Azure PowerShell**:

- [Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview) om du planerar tooconfigure automatisk säkerhetskopiering med PowerShell.

> [!NOTE]
> Automatisk säkerhetskopiering är beroende av hello tillägg för SQL Server IaaS Agent. Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard. Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Inställningar

hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering. hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **Automatisk säkerhetskopiering** | Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2014 Standard eller Enterprise. |
| **Kvarhållningsperioden** | 1 – 30 dagar (30 dagar) | hello antal dagar tooretain en säkerhetskopia. |
| **Storage-konto** | Azure Storage-konto | Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage. En behållare skapas på den här platsen toostore säkerhetskopior. hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och namnet på datorn. |
| **Kryptering** | Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar kryptering. När kryptering är aktiverat hello certifikat toorestore hello säkerhetskopiering finns i hello angetts storage-konto i hello samma `automaticbackup` behållare med hello samma namngivningskonvention. Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior. |
| **Lösenord** | Text för lösenord | Ett lösenord för krypteringsnycklar. Detta är endast krävs om kryptering är aktiverat. I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades. |

## <a name="configuration-in-hello-portal"></a>Konfigurationen i hello Portal

Du kan använda hello Azure portal tooconfigure automatisk säkerhetskopiering under etableringen eller för befintliga SQL Server 2014 virtuella datorer.

### <a name="new-vms"></a>Nya virtuella datorer

Använd hello Azure portal tooconfigure automatisk säkerhetskopiering när du skapar en ny SQL Server 2014 virtuell dator i hello Resource Manager-distributionsmodellen.

I hello **SQL Server-inställningar** bladet väljer **automatisk säkerhetskopiering**. hello följande Azure portal skärmbild visar hello **SQL automatisk säkerhetskopiering** bladet.

![Automatisk säkerhetskopiering i SQL-konfigurationen i Azure-portalen](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Befintliga virtuella datorer

Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer. Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.

![SQL automatisk säkerhetskopiering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk säkerhetskopiering avsnitt.

![Konfigurera automatisk säkerhetskopiering i SQL för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.

Om du aktiverar automatisk säkerhetskopiering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund. Under denna tid kanske hello Azure-portalen inte visar att automatisk säkerhetskopiering har konfigurerats. Vänta några minuter för hello agent toobe installerat kan konfigureras. Efter att hello Azure visar portal hello nya inställningar.

> [!NOTE]
> Du kan också konfigurera automatisk säkerhetskopiering med en mall. Mer information finns i [Azure quickstart-mall för automatisk säkerhetskopiering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Med PowerShell

Du kan använda PowerShell tooconfigure automatisk säkerhetskopiering. Innan du börjar måste du:

- [Hämta och installera den senaste Azure PowerShell hello](http://aka.ms/webpi-azps).
- Öppna Windows PowerShell och associera den med ditt konto. Du kan göra detta genom att följa stegen hello i hello [konfigurera prenumerationen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) avsnitt i hello etablering avsnittet.

### <a name="install-hello-sql-iaas-extension"></a>Installera hello SQL IaaS-tillägg
Om du har etablerat en virtuell dator i SQL Server från hello Azure-portalen bör hello SQL Server IaaS-tillägget vara installerad. Du kan fastställa om det har installerats för den virtuella datorn genom att anropa **Get-AzureRmVM** kommandot och undersöka hello **tillägg** egenskapen.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Du bör se den visas som ”SqlIaaSAgent” eller ”SQLIaaSExtension” om hello tillägg för SQL Server IaaS-Agent är installerad. **ProvisioningState** för hello-tillägget bör också visa ”lyckades”.

Om det inte är installerat eller inte fungerar toobe etableras, kan du installera det med följande kommando hello. I tillägg toohello VM namn och resurs grupp, måste du också ange hello region (**$region**) som den virtuella datorn finns i.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a>Kontrollera aktuella inställningar

Om du har aktiverat automatisk säkerhetskopiering under etablering kan du använda PowerShell toocheck din aktuella konfiguration. Kör hello **Get-AzureRmVMSqlServerExtension** kommando och granska hello **AutoBackupSettings** egenskapen:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Du bör få utdata liknande toohello följande:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Om din utdata visar att **aktivera** har angetts för**FALSKT**, och du har tooenable automatisk säkerhetskopiering. hello bra är att aktivera och konfigurera automatisk säkerhetskopiering i hello samma sätt. Avsnittet hello nästa informationen.

> [!NOTE] 
> Om du har kontrollerat hello inställningarna när du ändrar är det möjligt att du kommer tillbaka hello gamla konfigurationsvärden. Vänta en stund och kontrollera inställningarna för hello igen toomake till att ändringarna har tillämpats.

### <a name="configure-automated-backup"></a>Konfigurera automatisk säkerhetskopiering
Du kan använda PowerShell tooenable automatisk säkerhetskopiering samt toomodify dess konfiguration och funktionalitet när som helst.

Först, Välj eller skapa ett lagringskonto för hello säkerhetskopior. hello följande skript väljer ett lagringskonto eller skapar den om den inte finns.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Automatisk säkerhetskopiering stöder inte lagra säkerhetskopior i premium-lagring, men det kan göra säkerhetskopior från Virtuella diskar som använder Premium-lagring.

Använd hello **ny AzureRmVMSqlServerAutoBackupConfig** kommando tooenable och konfigurera hello automatisk säkerhetskopiering inställningar toostore säkerhetskopieringar i hello Azure storage-konto. I det här exemplet angetts hello säkerhetskopieringen toobe bevaras under 10 dagar. Hej andra kommandot **Set AzureRmVMSqlServerExtension**, uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.

> [!NOTE]
> Det finns andra inställningar för **ny AzureRmVMSqlServerAutoBackupConfig** som gäller endast tooSQL Server 2016 och automatisk säkerhetskopiering v2. SQL Server 2014 stöder inte hello följande inställningar: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, och **LogBackupFrequencyInMinutes**. Om du försöker tooconfigure inställningarna på en virtuell dator för SQL Server 2014, inga fel, men inte hämta tillämpas hello inställningar. Om du vill toouse inställningarna på en virtuell dator för SQL Server 2016, se [automatisk säkerhetskopiering v2 för SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

tooenable kryptering, ändra hello tidigare skriptet toopass hello **EnableEncryption** parametern tillsammans med ett lösenord (säker sträng) för hello **CertificatePassword** parameter. hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

inställningarna tillämpas tooconfirm [verifiera konfigurationen för automatisk säkerhetskopiering av hello](#verifysettings).

### <a name="disable-automated-backup"></a>Inaktivera automatisk säkerhetskopiering

toodisable automatisk säkerhetskopiering, kör hello samma skript utan hello **-aktivera** parametern toohello **ny AzureRmVMSqlServerAutoBackupConfig** kommando. Hej frånvaron av hello **-aktivera** parametern signaler hello kommandot toodisable hello funktionen. Det kan ta flera minuter toodisable automatisk säkerhetskopiering som installationen.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Exempelskriptet

hello följande skript innehåller en uppsättning variabler som du kan anpassa tooenable och konfigurera automatisk säkerhetskopiering för den virtuella datorn. Du kan behöva toocustomize hello skript baserat på dina krav i ditt fall. Du kan till exempel har ändrats och toomake om du vill använda toodisable hello säkerhetskopiering av systemdatabaser eller aktivera kryptering.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is hello resource group which is hosting hello VM where you are deploying hello SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account toostore hello backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nästa steg

Automatisk säkerhetskopiering konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer. Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.

Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

