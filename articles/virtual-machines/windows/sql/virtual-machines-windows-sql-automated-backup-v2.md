---
title: "aaaAutomated säkerhetskopiering v2 för SQL Server 2016 Azure Virtual Machines | Microsoft Docs"
description: "Förklarar hello automatisk säkerhetskopiering för SQL Server 2016 virtuella datorer som körs i Azure. Den här artikeln är särskilda tooVMs med hello Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: a322792fb22c76bfa74fafb711b8b1927a6e2b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a>Automatisk säkerhetskopiering v2 för SQL Server 2016 Azure virtuella datorer (Resource Manager)

> [!div class="op_single_selector"]
> * [SQLServer 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatisk säkerhetskopiering v2 konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure VM som kör SQL Server 2016 Standard, Enterprise eller Developer-versioner. Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage. Automatisk säkerhetskopiering v2 beror på hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Krav
toouse automatisk säkerhetskopiering v2, granska hello följande krav:

**Operativsystemet**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-version/utgåva**:

- SQL Server 2016 Standard
- SQL Server 2016 Enterprise
- SQL Server 2016 Developer

> [!IMPORTANT]
> Automatisk säkerhetskopiering v2 fungerar med SQL Server 2016. Om du använder SQL Server 2014 kan du använda automatisk säkerhetskopiering v1 tooback in dina databaser. Mer information finns i [automatisk säkerhetskopiering för SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

**Databaskonfiguration**:

- Måldatabaserna måste använda hello fullständiga återställningsmodellen. Mer information om hello effekten av hello fullständiga återställningsmodellen säkerhetskopior finns [säkerhetskopiering Under hello fullständiga återställningsmodellen](https://technet.microsoft.com/library/ms190217.aspx).
- Systemdatabaser inte toouse fullständiga återställningsmodellen. Om du behöver logga säkerhetskopieringar toobe vidtas för modell eller MSDB, måste du använda fullständiga återställningsmodellen.
- Måldatabaserna måste finnas på hello standardinstansen för SQL Server. Hej IaaS-tillägg för SQL Server stöder inte namngivna instanser.

**Azure distributionsmodell**:

- Resource Manager

> [!NOTE]
> Automatisk säkerhetskopiering är beroende av hello **tillägg för SQL Server IaaS Agent**. Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard. Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Inställningar
hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering v2. hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.

### <a name="basic-settings"></a>Grundläggande inställningar

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **Automatisk säkerhetskopiering** | Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2016 Standard eller Enterprise. |
| **Kvarhållningsperioden** | 1 – 30 dagar (30 dagar) | hello antal dagar tooretain säkerhetskopior. |
| **Storage-konto** | Azure Storage-konto | Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage. En behållare skapas på den här platsen toostore säkerhetskopior. hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och databas-GUID. |
| **Kryptering** |Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar kryptering. När kryptering är aktiverat hello certifikat toorestore hello säkerhetskopiering finns i hello angetts storage-konto i hello samma **automaticbackup** behållare med hello samma namngivningskonvention. Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior. |
| **Lösenord** |Text för lösenord | Ett lösenord för krypteringsnycklar. Detta är endast krävs om kryptering är aktiverat. I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades. |

### <a name="advanced-settings"></a>Avancerade inställningar

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **System databassäkerhetskopieringar** | Aktivera/inaktivera (inaktiverat) | När aktiverat funktionen också säkerhetskopierar hello systemdatabaser: Master, MSDB och modell. Verifiera att de är i fullständigt återställningsläge om du vill logga säkerhetskopieringar toobe vidtas för hello MSDB och modellen databaser. Säkerhetskopior av tas aldrig för Master. Och inga säkerhetskopior har tagit för TempDB. |
| **Schemat för säkerhetskopiering** | Manuell/automatisk (automatisk) | Som standard bestäms hello Säkerhetskopieringsschemat automatiskt utifrån hello loggen tillväxt. Manuell Säkerhetskopieringsschemat kan hello användaren toospecify hello tidsfönstret för säkerhetskopiering. I det här fallet säkerhetskopieringar endast tar med hello anges frekvens och under hello anges tidsfönster på en viss dag. |
| **Frekvens för fullständig säkerhetskopiering** | Varje dag/vecka | Frekvens för fullständiga säkerhetskopieringar. I båda fallen börjar fullständiga säkerhetskopieringar under hello nästa schemalagda tid. När varje vecka väljs säkerhetskopieringar kan sträcka sig över flera dagar tills alla databaser har säkerhetskopierats. |
| **Starttid för fullständig säkerhetskopiering** | 00:00 – 23:00 (01:00) | Starttid för en viss dag då fullständiga säkerhetskopieringar kan göras. |
| **Fullständig säkerhetskopiering tidsfönstret** | 1 – 23 timmar (1 timme) | Varaktighet för hello tidsfönstret för en viss dag då fullständiga säkerhetskopieringar kan göras. |
| **Frekvens för säkerhetskopiering av loggen** | 5 – 60 minuter (60 minuter) | Frekvensen för säkerhetskopior. |

## <a name="understanding-full-backup-frequency"></a>Förstå frekvensen för fullständig säkerhetskopiering
Det är viktigt toounderstand hello skillnaden mellan dagliga och veckovisa fullständiga säkerhetskopieringar. I den här aktiviteten kommer vi att gå igenom två scenarier.

### <a name="scenario-1-weekly-backups"></a>Scenario 1: Veckovisa säkerhetskopior
Du har en SQL Server-VM som innehåller ett antal mycket stora databaser.

Måndag aktivera automatisk säkerhetskopiering v2 med hello följande inställningar:

- Schemat för säkerhetskopiering: **manuell**
- Fullständig säkerhetskopiering synkroniseringsfrekvensen: **varje vecka**
- Fullständig säkerhetskopiering starttiden: **01:00**
- Fullständig säkerhetskopiering tidsfönstret: **1 timme**

Det innebär att hello nästa tillgängliga säkerhetskopieringsintervall finns tisdag kl 1 timma. Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget. I det här scenariot är databaserna tillräckligt stor för att slutförs fullständiga säkerhetskopieringar för hello första några databaser. Men efter en timme har inte alla hello databaser säkerhetskopierats.

När detta sker börjar automatisk säkerhetskopiering säkerhetskopiera hello återstående databaser hello nästa dag, onsdag kl 1 timma. Om inte alla databaser har säkerhetskopierats i den tid, görs ett försök igen hello nästa dag på hello samma tid. Detta fortsätter tills alla databaser har säkerhetskopierats.

När den når tisdag, börjar automatisk säkerhetskopiering säkerhetskopiera alla databaser igen.

Det här scenariot visar att automatisk säkerhetskopiering fungerar bara i hello angivna tidsfönstret, och varje databas säkerhetskopieras en gång per vecka. Det visar att det är möjligt för säkerhetskopieringar toospan flera dagar i hello fall där det inte är möjligt toocomplete alla säkerhetskopior i en dag.

### <a name="scenario-2-daily-backups"></a>Scenario 2: Daglig säkerhetskopiering
Du har en SQL Server-VM som innehåller ett antal mycket stora databaser.

Måndag aktivera automatisk säkerhetskopiering v2 med hello följande inställningar:

- Schemat för säkerhetskopiering: manuell
- Fullständig säkerhetskopiering synkroniseringsfrekvensen: varje dag
- Fullständig säkerhetskopiering starttiden: 22:00
- Fullständig säkerhetskopiering tidsfönstret: 6 timmar

Detta innebär att hello nästa tillgängliga säkerhetskopieringsintervall är måndag klockan 10 i 6 timmar. Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget.

Sedan tisdagen 10 i 6 timmar startas och fullständiga säkerhetskopieringar för alla databaser igen.

> [!IMPORTANT]
> När du schemalägger en daglig säkerhetskopiering, rekommenderas att du schemalägger en bred tid fönstret tooensure alla databaser kan säkerhetskopieras i den här gången. Detta är särskilt viktigt i hello fall där du har en stor mängd data tooback upp.

## <a name="configuration-in-hello-portal"></a>Konfigurationen i hello Portal

Du kan använda hello Azure portal tooconfigure automatisk säkerhetskopiering v2 under etableringen eller för befintliga SQL Server 2016 virtuella datorer.

### <a name="new-vms"></a>Nya virtuella datorer

Använd hello Azure portal tooconfigure automatisk säkerhetskopiering v2 när du skapar en ny SQL Server 2016-dator i hello Resource Manager-distributionsmodellen. 

I hello **SQL Server-inställningar** bladet väljer **automatisk säkerhetskopiering**. hello följande Azure portal skärmbild visar hello **SQL automatisk säkerhetskopiering** bladet.

![Automatisk säkerhetskopiering i SQL-konfigurationen i Azure-portalen](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatisk säkerhetskopiering v2 är inaktiverad som standard.

Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Befintliga virtuella datorer

Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer. Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.

![SQL automatisk säkerhetskopiering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk säkerhetskopiering avsnitt.

![Konfigurera automatisk säkerhetskopiering i SQL för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.

Om du aktiverar automatisk säkerhetskopiering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund. Under denna tid kanske hello Azure-portalen inte visar att automatisk säkerhetskopiering har konfigurerats. Vänta några minuter för hello agent toobe installerat kan konfigureras. Efter att hello Azure visar portal hello nya inställningar.

## <a name="configuration-with-powershell"></a>Med PowerShell

Du kan använda PowerShell tooconfigure automatisk säkerhetskopiering v2. Innan du börjar måste du:

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
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Om din utdata visar att **aktivera** har angetts för**FALSKT**, och du har tooenable automatisk säkerhetskopiering. hello bra är att aktivera och konfigurera automatisk säkerhetskopiering i hello samma sätt. Avsnittet hello nästa informationen.

> [!NOTE] 
> Om du har kontrollerat hello inställningarna när du ändrar är det möjligt att du kommer tillbaka hello gamla konfigurationsvärden. Vänta en stund och kontrollera inställningarna för hello igen toomake till att ändringarna har tillämpats.

### <a name="configure-automated-backup-v2"></a>Konfigurera automatisk säkerhetskopiering v2
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

Använd hello **ny AzureRmVMSqlServerAutoBackupConfig** kommando tooenable och konfigurera hello automatisk säkerhetskopiering v2 inställningar toostore säkerhetskopieringar i hello Azure storage-konto. I det här exemplet angetts hello säkerhetskopieringen toobe bevaras under 10 dagar. System databassäkerhetskopiorna är aktiverade. Fullständiga säkerhetskopieringar är schemalagda för varje vecka med ett tidsfönster som börjar vid 20:00 i två timmar. Säkerhetskopieringar är schemalagda för var 30: e minut. Hej andra kommandot **Set AzureRmVMSqlServerExtension**, uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent. 

tooenable kryptering, ändra hello tidigare skriptet toopass hello **EnableEncryption** parametern tillsammans med ett lösenord (säker sträng) för hello **CertificatePassword** parameter. hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

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
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

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
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nästa steg
Automatisk säkerhetskopiering v2 konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer. Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.

Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

