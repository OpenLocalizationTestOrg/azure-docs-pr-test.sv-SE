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
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a><span data-ttu-id="ceed5-104">Automatisk säkerhetskopiering för SQL Server 2014 virtuella datorer (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="ceed5-104">Automated Backup for SQL Server 2014 Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ceed5-105">SQLServer 2014</span><span class="sxs-lookup"><span data-stu-id="ceed5-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="ceed5-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="ceed5-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="ceed5-107">Automatisk säkerhetskopiering konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure-dator som kör SQL Server 2014 Standard eller Enterprise.</span><span class="sxs-lookup"><span data-stu-id="ceed5-107">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="ceed5-108">Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ceed5-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="ceed5-109">Automatisk säkerhetskopiering är beroende av hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-109">Automated Backup depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="ceed5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ceed5-110">Prerequisites</span></span>
<span data-ttu-id="ceed5-111">toouse automatisk säkerhetskopiering Tänk hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="ceed5-111">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="ceed5-112">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="ceed5-112">**Operating System**:</span></span>

- <span data-ttu-id="ceed5-113">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="ceed5-113">Windows Server 2012</span></span>
- <span data-ttu-id="ceed5-114">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ceed5-114">Windows Server 2012 R2</span></span>
- <span data-ttu-id="ceed5-115">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ceed5-115">Windows Server 2016</span></span>

<span data-ttu-id="ceed5-116">**SQL Server-version/utgåva**:</span><span class="sxs-lookup"><span data-stu-id="ceed5-116">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="ceed5-117">SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="ceed5-117">SQL Server 2014 Standard</span></span>
- <span data-ttu-id="ceed5-118">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="ceed5-118">SQL Server 2014 Enterprise</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ceed5-119">Automatisk säkerhetskopiering fungerar med SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="ceed5-119">Automated Backup works with SQL Server 2014.</span></span> <span data-ttu-id="ceed5-120">Om du använder SQL Server 2016 kan du använda automatisk säkerhetskopiering v2 tooback in dina databaser.</span><span class="sxs-lookup"><span data-stu-id="ceed5-120">If you are using SQL Server 2016, you can use Automated Backup v2 tooback up your databases.</span></span> <span data-ttu-id="ceed5-121">Mer information finns i [automatisk säkerhetskopiering v2 för SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-121">For more information, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="ceed5-122">**Databaskonfiguration**:</span><span class="sxs-lookup"><span data-stu-id="ceed5-122">**Database configuration**:</span></span>

- <span data-ttu-id="ceed5-123">Måldatabaserna måste använda hello fullständiga återställningsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="ceed5-124">Mer information om hello effekten av hello fullständiga återställningsmodellen säkerhetskopior finns [säkerhetskopiering Under hello fullständiga återställningsmodellen](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="ceed5-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="ceed5-125">Måldatabaserna måste finnas på hello standardinstansen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ceed5-125">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="ceed5-126">Hej IaaS-tillägg för SQL Server stöder inte namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="ceed5-126">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="ceed5-127">**Azure distributionsmodell**:</span><span class="sxs-lookup"><span data-stu-id="ceed5-127">**Azure deployment model**:</span></span>

- <span data-ttu-id="ceed5-128">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ceed5-128">Resource Manager</span></span>

<span data-ttu-id="ceed5-129">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="ceed5-129">**Azure PowerShell**:</span></span>

- <span data-ttu-id="ceed5-130">[Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview) om du planerar tooconfigure automatisk säkerhetskopiering med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ceed5-130">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Backup with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="ceed5-131">Automatisk säkerhetskopiering är beroende av hello tillägg för SQL Server IaaS Agent.</span><span class="sxs-lookup"><span data-stu-id="ceed5-131">Automated Backup relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="ceed5-132">Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard.</span><span class="sxs-lookup"><span data-stu-id="ceed5-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="ceed5-133">Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="ceed5-134">Inställningar</span><span class="sxs-lookup"><span data-stu-id="ceed5-134">Settings</span></span>

<span data-ttu-id="ceed5-135">hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-135">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="ceed5-136">hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ceed5-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="ceed5-137">Inställning</span><span class="sxs-lookup"><span data-stu-id="ceed5-137">Setting</span></span> | <span data-ttu-id="ceed5-138">Intervall (standard)</span><span class="sxs-lookup"><span data-stu-id="ceed5-138">Range (Default)</span></span> | <span data-ttu-id="ceed5-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ceed5-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ceed5-140">**Automatisk säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="ceed5-140">**Automated Backup**</span></span> | <span data-ttu-id="ceed5-141">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="ceed5-141">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="ceed5-142">Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2014 Standard eller Enterprise.</span><span class="sxs-lookup"><span data-stu-id="ceed5-142">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="ceed5-143">**Kvarhållningsperioden**</span><span class="sxs-lookup"><span data-stu-id="ceed5-143">**Retention Period**</span></span> | <span data-ttu-id="ceed5-144">1 – 30 dagar (30 dagar)</span><span class="sxs-lookup"><span data-stu-id="ceed5-144">1-30 days (30 days)</span></span> | <span data-ttu-id="ceed5-145">hello antal dagar tooretain en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="ceed5-145">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="ceed5-146">**Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="ceed5-146">**Storage Account**</span></span> | <span data-ttu-id="ceed5-147">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="ceed5-147">Azure storage account</span></span> | <span data-ttu-id="ceed5-148">Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage.</span><span class="sxs-lookup"><span data-stu-id="ceed5-148">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="ceed5-149">En behållare skapas på den här platsen toostore säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="ceed5-149">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="ceed5-150">hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och namnet på datorn.</span><span class="sxs-lookup"><span data-stu-id="ceed5-150">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="ceed5-151">**Kryptering**</span><span class="sxs-lookup"><span data-stu-id="ceed5-151">**Encryption**</span></span> | <span data-ttu-id="ceed5-152">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="ceed5-152">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="ceed5-153">Aktiverar eller inaktiverar kryptering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-153">Enables or disables encryption.</span></span> <span data-ttu-id="ceed5-154">När kryptering är aktiverat hello certifikat toorestore hello säkerhetskopiering finns i hello angetts storage-konto i hello samma `automaticbackup` behållare med hello samma namngivningskonvention.</span><span class="sxs-lookup"><span data-stu-id="ceed5-154">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same `automaticbackup` container using hello same naming convention.</span></span> <span data-ttu-id="ceed5-155">Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="ceed5-155">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="ceed5-156">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="ceed5-156">**Password**</span></span> | <span data-ttu-id="ceed5-157">Text för lösenord</span><span class="sxs-lookup"><span data-stu-id="ceed5-157">Password text</span></span> | <span data-ttu-id="ceed5-158">Ett lösenord för krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="ceed5-158">A password for encryption keys.</span></span> <span data-ttu-id="ceed5-159">Detta är endast krävs om kryptering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="ceed5-159">This is only required if encryption is enabled.</span></span> <span data-ttu-id="ceed5-160">I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="ceed5-160">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="ceed5-161">Konfigurationen i hello Portal</span><span class="sxs-lookup"><span data-stu-id="ceed5-161">Configuration in hello Portal</span></span>

<span data-ttu-id="ceed5-162">Du kan använda hello Azure portal tooconfigure automatisk säkerhetskopiering under etableringen eller för befintliga SQL Server 2014 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ceed5-162">You can use hello Azure portal tooconfigure Automated Backup during provisioning or for existing SQL Server 2014 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="ceed5-163">Nya virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ceed5-163">New VMs</span></span>

<span data-ttu-id="ceed5-164">Använd hello Azure portal tooconfigure automatisk säkerhetskopiering när du skapar en ny SQL Server 2014 virtuell dator i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-164">Use hello Azure portal tooconfigure Automated Backup when you create a new SQL Server 2014 Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="ceed5-165">I hello **SQL Server-inställningar** bladet väljer **automatisk säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ceed5-165">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="ceed5-166">hello följande Azure portal skärmbild visar hello **SQL automatisk säkerhetskopiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="ceed5-166">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![Automatisk säkerhetskopiering i SQL-konfigurationen i Azure-portalen](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

<span data-ttu-id="ceed5-168">Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-168">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="ceed5-169">Befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ceed5-169">Existing VMs</span></span>

<span data-ttu-id="ceed5-170">Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ceed5-170">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="ceed5-171">Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="ceed5-171">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatisk säkerhetskopiering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

<span data-ttu-id="ceed5-173">I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk säkerhetskopiering avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ceed5-173">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Konfigurera automatisk säkerhetskopiering i SQL för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

<span data-ttu-id="ceed5-175">När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ceed5-175">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="ceed5-176">Om du aktiverar automatisk säkerhetskopiering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="ceed5-176">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="ceed5-177">Under denna tid kanske hello Azure-portalen inte visar att automatisk säkerhetskopiering har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="ceed5-177">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="ceed5-178">Vänta några minuter för hello agent toobe installerat kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="ceed5-178">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="ceed5-179">Efter att hello Azure visar portal hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="ceed5-179">After that hello Azure portal will reflect hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="ceed5-180">Du kan också konfigurera automatisk säkerhetskopiering med en mall.</span><span class="sxs-lookup"><span data-stu-id="ceed5-180">You can also configure Automated Backup using a template.</span></span> <span data-ttu-id="ceed5-181">Mer information finns i [Azure quickstart-mall för automatisk säkerhetskopiering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span><span class="sxs-lookup"><span data-stu-id="ceed5-181">For more information, see [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="ceed5-182">Med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ceed5-182">Configuration with PowerShell</span></span>

<span data-ttu-id="ceed5-183">Du kan använda PowerShell tooconfigure automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-183">You can use PowerShell tooconfigure Automated Backup.</span></span> <span data-ttu-id="ceed5-184">Innan du börjar måste du:</span><span class="sxs-lookup"><span data-stu-id="ceed5-184">Before you begin, you must:</span></span>

- <span data-ttu-id="ceed5-185">[Hämta och installera den senaste Azure PowerShell hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="ceed5-185">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="ceed5-186">Öppna Windows PowerShell och associera den med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ceed5-186">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="ceed5-187">Du kan göra detta genom att följa stegen hello i hello [konfigurera prenumerationen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) avsnitt i hello etablering avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ceed5-187">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="ceed5-188">Installera hello SQL IaaS-tillägg</span><span class="sxs-lookup"><span data-stu-id="ceed5-188">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="ceed5-189">Om du har etablerat en virtuell dator i SQL Server från hello Azure-portalen bör hello SQL Server IaaS-tillägget vara installerad.</span><span class="sxs-lookup"><span data-stu-id="ceed5-189">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="ceed5-190">Du kan fastställa om det har installerats för den virtuella datorn genom att anropa **Get-AzureRmVM** kommandot och undersöka hello **tillägg** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-190">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

<span data-ttu-id="ceed5-191">Du bör se den visas som ”SqlIaaSAgent” eller ”SQLIaaSExtension” om hello tillägg för SQL Server IaaS-Agent är installerad.</span><span class="sxs-lookup"><span data-stu-id="ceed5-191">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="ceed5-192">**ProvisioningState** för hello-tillägget bör också visa ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="ceed5-192">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span>

<span data-ttu-id="ceed5-193">Om det inte är installerat eller inte fungerar toobe etableras, kan du installera det med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="ceed5-193">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="ceed5-194">I tillägg toohello VM namn och resurs grupp, måste du också ange hello region (**$region**) som den virtuella datorn finns i.</span><span class="sxs-lookup"><span data-stu-id="ceed5-194">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <span data-ttu-id="ceed5-195"><a id="verifysettings"></a>Kontrollera aktuella inställningar</span><span class="sxs-lookup"><span data-stu-id="ceed5-195"><a id="verifysettings"></a> Verify current settings</span></span>

<span data-ttu-id="ceed5-196">Om du har aktiverat automatisk säkerhetskopiering under etablering kan du använda PowerShell toocheck din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ceed5-196">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="ceed5-197">Kör hello **Get-AzureRmVMSqlServerExtension** kommando och granska hello **AutoBackupSettings** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="ceed5-197">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="ceed5-198">Du bör få utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ceed5-198">You should get output similar toohello following:</span></span>

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

<span data-ttu-id="ceed5-199">Om din utdata visar att **aktivera** har angetts för**FALSKT**, och du har tooenable automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-199">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="ceed5-200">hello bra är att aktivera och konfigurera automatisk säkerhetskopiering i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="ceed5-200">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="ceed5-201">Avsnittet hello nästa informationen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-201">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="ceed5-202">Om du har kontrollerat hello inställningarna när du ändrar är det möjligt att du kommer tillbaka hello gamla konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="ceed5-202">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="ceed5-203">Vänta en stund och kontrollera inställningarna för hello igen toomake till att ändringarna har tillämpats.</span><span class="sxs-lookup"><span data-stu-id="ceed5-203">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup"></a><span data-ttu-id="ceed5-204">Konfigurera automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ceed5-204">Configure Automated Backup</span></span>
<span data-ttu-id="ceed5-205">Du kan använda PowerShell tooenable automatisk säkerhetskopiering samt toomodify dess konfiguration och funktionalitet när som helst.</span><span class="sxs-lookup"><span data-stu-id="ceed5-205">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span>

<span data-ttu-id="ceed5-206">Först, Välj eller skapa ett lagringskonto för hello säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="ceed5-206">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="ceed5-207">hello följande skript väljer ett lagringskonto eller skapar den om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ceed5-207">hello following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="ceed5-208">Automatisk säkerhetskopiering stöder inte lagra säkerhetskopior i premium-lagring, men det kan göra säkerhetskopior från Virtuella diskar som använder Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="ceed5-208">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="ceed5-209">Använd hello **ny AzureRmVMSqlServerAutoBackupConfig** kommando tooenable och konfigurera hello automatisk säkerhetskopiering inställningar toostore säkerhetskopieringar i hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ceed5-209">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="ceed5-210">I det här exemplet angetts hello säkerhetskopieringen toobe bevaras under 10 dagar.</span><span class="sxs-lookup"><span data-stu-id="ceed5-210">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="ceed5-211">Hej andra kommandot **Set AzureRmVMSqlServerExtension**, uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ceed5-211">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="ceed5-212">Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="ceed5-212">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="ceed5-213">Det finns andra inställningar för **ny AzureRmVMSqlServerAutoBackupConfig** som gäller endast tooSQL Server 2016 och automatisk säkerhetskopiering v2.</span><span class="sxs-lookup"><span data-stu-id="ceed5-213">There are other settings for **New-AzureRmVMSqlServerAutoBackupConfig** that apply only tooSQL Server 2016 and Automated Backup v2.</span></span> <span data-ttu-id="ceed5-214">SQL Server 2014 stöder inte hello följande inställningar: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, och **LogBackupFrequencyInMinutes**.</span><span class="sxs-lookup"><span data-stu-id="ceed5-214">SQL Server 2014 does not support hello following settings: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, and **LogBackupFrequencyInMinutes**.</span></span> <span data-ttu-id="ceed5-215">Om du försöker tooconfigure inställningarna på en virtuell dator för SQL Server 2014, inga fel, men inte hämta tillämpas hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="ceed5-215">If you attempt tooconfigure these settings on a SQL Server 2014 virtual machine, there is no error, but hello settings do not get applied.</span></span> <span data-ttu-id="ceed5-216">Om du vill toouse inställningarna på en virtuell dator för SQL Server 2016, se [automatisk säkerhetskopiering v2 för SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-216">If you want toouse these settings on a SQL Server 2016 virtual machine, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="ceed5-217">tooenable kryptering, ändra hello tidigare skriptet toopass hello **EnableEncryption** parametern tillsammans med ett lösenord (säker sträng) för hello **CertificatePassword** parameter.</span><span class="sxs-lookup"><span data-stu-id="ceed5-217">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="ceed5-218">hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-218">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

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

<span data-ttu-id="ceed5-219">inställningarna tillämpas tooconfirm [verifiera konfigurationen för automatisk säkerhetskopiering av hello](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="ceed5-219">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="ceed5-220">Inaktivera automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ceed5-220">Disable Automated Backup</span></span>

<span data-ttu-id="ceed5-221">toodisable automatisk säkerhetskopiering, kör hello samma skript utan hello **-aktivera** parametern toohello **ny AzureRmVMSqlServerAutoBackupConfig** kommando.</span><span class="sxs-lookup"><span data-stu-id="ceed5-221">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="ceed5-222">Hej frånvaron av hello **-aktivera** parametern signaler hello kommandot toodisable hello funktionen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-222">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="ceed5-223">Det kan ta flera minuter toodisable automatisk säkerhetskopiering som installationen.</span><span class="sxs-lookup"><span data-stu-id="ceed5-223">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="ceed5-224">Exempelskriptet</span><span class="sxs-lookup"><span data-stu-id="ceed5-224">Example script</span></span>

<span data-ttu-id="ceed5-225">hello följande skript innehåller en uppsättning variabler som du kan anpassa tooenable och konfigurera automatisk säkerhetskopiering för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ceed5-225">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="ceed5-226">Du kan behöva toocustomize hello skript baserat på dina krav i ditt fall.</span><span class="sxs-lookup"><span data-stu-id="ceed5-226">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="ceed5-227">Du kan till exempel har ändrats och toomake om du vill använda toodisable hello säkerhetskopiering av systemdatabaser eller aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="ceed5-227">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ceed5-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ceed5-228">Next steps</span></span>

<span data-ttu-id="ceed5-229">Automatisk säkerhetskopiering konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ceed5-229">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="ceed5-230">Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="ceed5-230">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="ceed5-231">Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-231">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="ceed5-232">Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-232">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="ceed5-233">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ceed5-233">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

