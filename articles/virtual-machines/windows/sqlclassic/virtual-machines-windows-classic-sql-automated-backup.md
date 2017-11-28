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
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="4a2a2-103">Automatisk säkerhetskopiering för SQLServer på virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-103">Automated Backup for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4a2a2-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4a2a2-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="4a2a2-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="4a2a2-105">Classic</span></span>](../classic/sql-automated-backup.md)
> 
> 

<span data-ttu-id="4a2a2-106">Automatisk säkerhetskopiering konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure-dator som kör SQL Server 2014 Standard eller Enterprise.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-106">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="4a2a2-107">Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-107">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="4a2a2-108">Automatisk säkerhetskopiering är beroende av hello [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-108">Automated Backup depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="4a2a2-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4a2a2-110">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="4a2a2-111">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="4a2a2-112">tooview hello Resource Manager-versionen av den här artikeln finns [automatisk säkerhetskopiering för SQL Server i Azure virtuella datorer Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-112">tooview hello Resource Manager version of this article, see [Automated Backup for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a2a2-113">Krav</span><span class="sxs-lookup"><span data-stu-id="4a2a2-113">Prerequisites</span></span>
<span data-ttu-id="4a2a2-114">toouse automatisk säkerhetskopiering Tänk hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-114">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="4a2a2-115">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-115">**Operating System**:</span></span>

* <span data-ttu-id="4a2a2-116">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="4a2a2-116">Windows Server 2012</span></span>
* <span data-ttu-id="4a2a2-117">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="4a2a2-117">Windows Server 2012 R2</span></span>
* <span data-ttu-id="4a2a2-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="4a2a2-118">Windows Server 2016</span></span>

<span data-ttu-id="4a2a2-119">**SQL Server-version/utgåva**:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-119">**SQL Server version/edition**:</span></span>

* <span data-ttu-id="4a2a2-120">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="4a2a2-120">SQL Server 2014 Standard</span></span>
* <span data-ttu-id="4a2a2-121">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="4a2a2-121">SQL Server 2014 Enterprise</span></span>

> [!NOTE]
> <span data-ttu-id="4a2a2-122">SQL Server 2016 ännu inte har stöd för automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-122">SQL Server 2016 is not yet supported for Automated Backup.</span></span>
> 
> 

<span data-ttu-id="4a2a2-123">**Databaskonfiguration**:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-123">**Database configuration**:</span></span>

* <span data-ttu-id="4a2a2-124">Måldatabaserna måste använda hello fullständiga återställningsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-124">Target databases must use hello full recovery model.</span></span>

<span data-ttu-id="4a2a2-125">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-125">**Azure PowerShell**:</span></span>

* <span data-ttu-id="4a2a2-126">[Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-126">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="4a2a2-127">**SQL Server IaaS tillägget**:</span><span class="sxs-lookup"><span data-stu-id="4a2a2-127">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="4a2a2-128">[Installera hello SQL Server IaaS tillägget](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-128">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="4a2a2-129">Inställningar</span><span class="sxs-lookup"><span data-stu-id="4a2a2-129">Settings</span></span>
<span data-ttu-id="4a2a2-130">hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-130">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="4a2a2-131">För klassiska virtuella datorer kan använda du PowerShell tooconfigure inställningarna.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-131">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="4a2a2-132">Inställning</span><span class="sxs-lookup"><span data-stu-id="4a2a2-132">Setting</span></span> | <span data-ttu-id="4a2a2-133">Intervall (standard)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-133">Range (Default)</span></span> | <span data-ttu-id="4a2a2-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4a2a2-134">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4a2a2-135">**Automatisk säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-135">**Automated Backup**</span></span> |<span data-ttu-id="4a2a2-136">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-136">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="4a2a2-137">Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2014 Standard eller Enterprise.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-137">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="4a2a2-138">**Kvarhållningsperioden**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-138">**Retention Period**</span></span> |<span data-ttu-id="4a2a2-139">1 – 30 dagar (30 dagar)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-139">1-30 days (30 days)</span></span> |<span data-ttu-id="4a2a2-140">hello antal dagar tooretain en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-140">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="4a2a2-141">**Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-141">**Storage Account**</span></span> |<span data-ttu-id="4a2a2-142">Azure storage-konto (hello-lagringskonto som skapats för hello angivna VM)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-142">Azure storage account (hello storage account created for hello specified VM)</span></span> |<span data-ttu-id="4a2a2-143">Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-143">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="4a2a2-144">En behållare skapas på den här platsen toostore säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-144">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="4a2a2-145">hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och namnet på datorn.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-145">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="4a2a2-146">**Kryptering**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-146">**Encryption**</span></span> |<span data-ttu-id="4a2a2-147">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-147">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="4a2a2-148">Aktiverar eller inaktiverar kryptering.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-148">Enables or disables encryption.</span></span> <span data-ttu-id="4a2a2-149">Om kryptering har aktiverats, hello certifikat används toorestore hello säkerhetskopiering finns i hello ange storage-konto i hello samma automaticbackup behållaren med hjälp av hello samma namngivningskonvention.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-149">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same automaticbackup container using hello same naming convention.</span></span> <span data-ttu-id="4a2a2-150">Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-150">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="4a2a2-151">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-151">**Password**</span></span> |<span data-ttu-id="4a2a2-152">Lösenord text (ingen)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-152">Password text (None)</span></span> |<span data-ttu-id="4a2a2-153">Ett lösenord för krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-153">A password for encryption keys.</span></span> <span data-ttu-id="4a2a2-154">Detta är endast krävs om kryptering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-154">This is only required if encryption is enabled.</span></span> <span data-ttu-id="4a2a2-155">I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-155">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> | <span data-ttu-id="4a2a2-156">**Säkerhetskopieringssystem databaser**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-156">**Backup system databases**</span></span> | <span data-ttu-id="4a2a2-157">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-157">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="4a2a2-158">Ta och fullständiga säkerhetskopieringar av Master, Model och MSDB</span><span class="sxs-lookup"><span data-stu-id="4a2a2-158">Take full backups of Master, Model, and MSDB</span></span> |
| <span data-ttu-id="4a2a2-159">**Konfigurera schemat för säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="4a2a2-159">**Configure backup schedule**</span></span> | <span data-ttu-id="4a2a2-160">Manuell/automatisk (automatisk)</span><span class="sxs-lookup"><span data-stu-id="4a2a2-160">Manual/Automated (Automated)</span></span> | <span data-ttu-id="4a2a2-161">Välj **automatisk** tooautomatically ta fullständig och loggsäkerhetskopior baserat på loggen tillväxt.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-161">Select **Automated** tooautomatically take full and log backups based on log growth.</span></span> <span data-ttu-id="4a2a2-162">Välj **manuell** toospecify hello schemat för fullständig och loggsäkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-162">Select **Manual** toospecify hello schedule for full and log backups.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="4a2a2-163">Med PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a2a2-163">Configuration with PowerShell</span></span>
<span data-ttu-id="4a2a2-164">I följande exempel PowerShell hello, har automatisk säkerhetskopiering konfigurerats för en befintlig SQL Server 2014-VM.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-164">In hello following PowerShell example, Automated Backup is configured for an existing SQL Server 2014 VM.</span></span> <span data-ttu-id="4a2a2-165">Hej **ny AzureVMSqlServerAutoBackupConfig** kommando konfigurerar hello automatisk säkerhetskopiering inställningar toostore säkerhetskopieringar i hello Azure storage-konto som anges av hello $storageaccount variabeln.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-165">hello **New-AzureVMSqlServerAutoBackupConfig** command configures hello Automated Backup settings toostore backups in hello Azure storage account specified by hello $storageaccount variable.</span></span> <span data-ttu-id="4a2a2-166">Dessa säkerhetskopior ska behållas i 10 dagar.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-166">These backups will be retained for 10 days.</span></span> <span data-ttu-id="4a2a2-167">Hej **Set AzureVMSqlServerExtension** kommandot uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-167">hello **Set-AzureVMSqlServerExtension** command updates hello specified Azure VM with these settings.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="4a2a2-168">Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-168">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="4a2a2-169">tooenable kryptering, ändra hello tidigare toopass hello EnableEncryption skriptparameter tillsammans med ett lösenord (säker sträng) för hello CertificatePassword parameter.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-169">tooenable encryption, modify hello previous script toopass hello EnableEncryption parameter along with a password (secure string) for hello CertificatePassword parameter.</span></span> <span data-ttu-id="4a2a2-170">hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-170">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="4a2a2-171">toodisable automatisk säkerhetskopiering, kör hello samma skript utan hello **-aktivera** parametern toohello **ny AzureVMSqlServerAutoBackupConfig**.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-171">toodisable automatic backup, run hello same script without hello **-Enable** parameter toohello **New-AzureVMSqlServerAutoBackupConfig**.</span></span> <span data-ttu-id="4a2a2-172">Det kan ta flera minuter toodisable automatisk säkerhetskopiering som installationen.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-172">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

> [!NOTE]
> <span data-ttu-id="4a2a2-173">Inaktivering och avinstallerar hello SQL Server IaaS-Agent tas inte bort hello som tidigare har konfigurerat inställningarna för hanterad säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-173">Disabling and uninstalling hello SQL Server IaaS Agent does not remove hello previously configured Managed Backup settings.</span></span> <span data-ttu-id="4a2a2-174">Du bör inaktivera automatisk säkerhetskopiering innan du inaktiverar eller avinstallerar hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-174">You should disable Automated Backup before disabling or uninstalling hello SQL Server IaaS Agent.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4a2a2-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a2a2-175">Next steps</span></span>
<span data-ttu-id="4a2a2-176">Automatisk säkerhetskopiering konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-176">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="4a2a2-177">Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="4a2a2-177">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="4a2a2-178">Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-178">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="4a2a2-179">Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-179">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="4a2a2-180">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4a2a2-180">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

