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
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a><span data-ttu-id="32468-104">Automatisk säkerhetskopiering v2 för SQL Server 2016 Azure virtuella datorer (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="32468-104">Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="32468-105">SQLServer 2014</span><span class="sxs-lookup"><span data-stu-id="32468-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="32468-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="32468-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="32468-107">Automatisk säkerhetskopiering v2 konfigurerar automatiskt [hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure VM som kör SQL Server 2016 Standard, Enterprise eller Developer-versioner.</span><span class="sxs-lookup"><span data-stu-id="32468-107">Automated Backup v2 automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2016 Standard, Enterprise, or Developer editions.</span></span> <span data-ttu-id="32468-108">Detta gör att du tooconfigure regelbundna säkerhetskopieringar som använder beständiga Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="32468-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="32468-109">Automatisk säkerhetskopiering v2 beror på hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="32468-109">Automated Backup v2 depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="32468-110">Krav</span><span class="sxs-lookup"><span data-stu-id="32468-110">Prerequisites</span></span>
<span data-ttu-id="32468-111">toouse automatisk säkerhetskopiering v2, granska hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="32468-111">toouse Automated Backup v2, review hello following prerequisites:</span></span>

<span data-ttu-id="32468-112">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="32468-112">**Operating System**:</span></span>

- <span data-ttu-id="32468-113">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="32468-113">Windows Server 2012 R2</span></span>
- <span data-ttu-id="32468-114">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="32468-114">Windows Server 2016</span></span>

<span data-ttu-id="32468-115">**SQL Server-version/utgåva**:</span><span class="sxs-lookup"><span data-stu-id="32468-115">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="32468-116">SQL Server 2016 Standard</span><span class="sxs-lookup"><span data-stu-id="32468-116">SQL Server 2016 Standard</span></span>
- <span data-ttu-id="32468-117">SQL Server 2016 Enterprise</span><span class="sxs-lookup"><span data-stu-id="32468-117">SQL Server 2016 Enterprise</span></span>
- <span data-ttu-id="32468-118">SQL Server 2016 Developer</span><span class="sxs-lookup"><span data-stu-id="32468-118">SQL Server 2016 Developer</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32468-119">Automatisk säkerhetskopiering v2 fungerar med SQL Server 2016.</span><span class="sxs-lookup"><span data-stu-id="32468-119">Automated Backup v2 works with SQL Server 2016.</span></span> <span data-ttu-id="32468-120">Om du använder SQL Server 2014 kan du använda automatisk säkerhetskopiering v1 tooback in dina databaser.</span><span class="sxs-lookup"><span data-stu-id="32468-120">If you are using SQL Server 2014, you can use Automated Backup v1 tooback up your databases.</span></span> <span data-ttu-id="32468-121">Mer information finns i [automatisk säkerhetskopiering för SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="32468-121">For more information, see [Automated Backup for SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span></span>

<span data-ttu-id="32468-122">**Databaskonfiguration**:</span><span class="sxs-lookup"><span data-stu-id="32468-122">**Database configuration**:</span></span>

- <span data-ttu-id="32468-123">Måldatabaserna måste använda hello fullständiga återställningsmodellen.</span><span class="sxs-lookup"><span data-stu-id="32468-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="32468-124">Mer information om hello effekten av hello fullständiga återställningsmodellen säkerhetskopior finns [säkerhetskopiering Under hello fullständiga återställningsmodellen](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="32468-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="32468-125">Systemdatabaser inte toouse fullständiga återställningsmodellen.</span><span class="sxs-lookup"><span data-stu-id="32468-125">System databases do not have toouse full recovery model.</span></span> <span data-ttu-id="32468-126">Om du behöver logga säkerhetskopieringar toobe vidtas för modell eller MSDB, måste du använda fullständiga återställningsmodellen.</span><span class="sxs-lookup"><span data-stu-id="32468-126">However, if you require log backups toobe taken for Model or MSDB, you must use full recovery model.</span></span>
- <span data-ttu-id="32468-127">Måldatabaserna måste finnas på hello standardinstansen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="32468-127">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="32468-128">Hej IaaS-tillägg för SQL Server stöder inte namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="32468-128">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="32468-129">**Azure distributionsmodell**:</span><span class="sxs-lookup"><span data-stu-id="32468-129">**Azure deployment model**:</span></span>

- <span data-ttu-id="32468-130">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="32468-130">Resource Manager</span></span>

> [!NOTE]
> <span data-ttu-id="32468-131">Automatisk säkerhetskopiering är beroende av hello **tillägg för SQL Server IaaS Agent**.</span><span class="sxs-lookup"><span data-stu-id="32468-131">Automated Backup relies on hello **SQL Server IaaS Agent Extension**.</span></span> <span data-ttu-id="32468-132">Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard.</span><span class="sxs-lookup"><span data-stu-id="32468-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="32468-133">Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="32468-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="32468-134">Inställningar</span><span class="sxs-lookup"><span data-stu-id="32468-134">Settings</span></span>
<span data-ttu-id="32468-135">hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk säkerhetskopiering v2.</span><span class="sxs-lookup"><span data-stu-id="32468-135">hello following table describes hello options that can be configured for Automated Backup v2.</span></span> <span data-ttu-id="32468-136">hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="32468-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

### <a name="basic-settings"></a><span data-ttu-id="32468-137">Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="32468-137">Basic Settings</span></span>

| <span data-ttu-id="32468-138">Inställning</span><span class="sxs-lookup"><span data-stu-id="32468-138">Setting</span></span> | <span data-ttu-id="32468-139">Intervall (standard)</span><span class="sxs-lookup"><span data-stu-id="32468-139">Range (Default)</span></span> | <span data-ttu-id="32468-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32468-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="32468-141">**Automatisk säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="32468-141">**Automated Backup**</span></span> | <span data-ttu-id="32468-142">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="32468-142">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="32468-143">Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2016 Standard eller Enterprise.</span><span class="sxs-lookup"><span data-stu-id="32468-143">Enables or disables Automated Backup for an Azure VM running SQL Server 2016 Standard or Enterprise.</span></span> |
| <span data-ttu-id="32468-144">**Kvarhållningsperioden**</span><span class="sxs-lookup"><span data-stu-id="32468-144">**Retention Period**</span></span> | <span data-ttu-id="32468-145">1 – 30 dagar (30 dagar)</span><span class="sxs-lookup"><span data-stu-id="32468-145">1-30 days (30 days)</span></span> | <span data-ttu-id="32468-146">hello antal dagar tooretain säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="32468-146">hello number of days tooretain backups.</span></span> |
| <span data-ttu-id="32468-147">**Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="32468-147">**Storage Account**</span></span> | <span data-ttu-id="32468-148">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="32468-148">Azure storage account</span></span> | <span data-ttu-id="32468-149">Toouse ett Azure storage-konto för att lagra filer för automatisk säkerhetskopiering i blob storage.</span><span class="sxs-lookup"><span data-stu-id="32468-149">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="32468-150">En behållare skapas på den här platsen toostore säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="32468-150">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="32468-151">hello namngivningskonvention för säkerhetskopian innehåller hello datum, tid och databas-GUID.</span><span class="sxs-lookup"><span data-stu-id="32468-151">hello backup file naming convention includes hello date, time, and database GUID.</span></span> |
| <span data-ttu-id="32468-152">**Kryptering**</span><span class="sxs-lookup"><span data-stu-id="32468-152">**Encryption**</span></span> |<span data-ttu-id="32468-153">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="32468-153">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="32468-154">Aktiverar eller inaktiverar kryptering.</span><span class="sxs-lookup"><span data-stu-id="32468-154">Enables or disables encryption.</span></span> <span data-ttu-id="32468-155">När kryptering är aktiverat hello certifikat toorestore hello säkerhetskopiering finns i hello angetts storage-konto i hello samma **automaticbackup** behållare med hello samma namngivningskonvention.</span><span class="sxs-lookup"><span data-stu-id="32468-155">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same **automaticbackup** container using hello same naming convention.</span></span> <span data-ttu-id="32468-156">Om hello lösenordet ändras, skapas ett nytt certifikat med lösenordet, men hello gammalt certifikat förblir toorestore tidigare säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="32468-156">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="32468-157">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="32468-157">**Password**</span></span> |<span data-ttu-id="32468-158">Text för lösenord</span><span class="sxs-lookup"><span data-stu-id="32468-158">Password text</span></span> | <span data-ttu-id="32468-159">Ett lösenord för krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="32468-159">A password for encryption keys.</span></span> <span data-ttu-id="32468-160">Detta är endast krävs om kryptering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="32468-160">This is only required if encryption is enabled.</span></span> <span data-ttu-id="32468-161">I ordning toorestore en krypterad säkerhetskopiering, du måste ha hello rätt lösenord och relaterade certifikatet som användes för närvarande hello hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="32468-161">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

### <a name="advanced-settings"></a><span data-ttu-id="32468-162">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="32468-162">Advanced Settings</span></span>

| <span data-ttu-id="32468-163">Inställning</span><span class="sxs-lookup"><span data-stu-id="32468-163">Setting</span></span> | <span data-ttu-id="32468-164">Intervall (standard)</span><span class="sxs-lookup"><span data-stu-id="32468-164">Range (Default)</span></span> | <span data-ttu-id="32468-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="32468-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="32468-166">**System databassäkerhetskopieringar**</span><span class="sxs-lookup"><span data-stu-id="32468-166">**System Database Backups**</span></span> | <span data-ttu-id="32468-167">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="32468-167">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="32468-168">När aktiverat funktionen också säkerhetskopierar hello systemdatabaser: Master, MSDB och modell.</span><span class="sxs-lookup"><span data-stu-id="32468-168">When enabled, this feature will also back up hello system databases: Master, MSDB, and Model.</span></span> <span data-ttu-id="32468-169">Verifiera att de är i fullständigt återställningsläge om du vill logga säkerhetskopieringar toobe vidtas för hello MSDB och modellen databaser.</span><span class="sxs-lookup"><span data-stu-id="32468-169">For hello MSDB and Model databases, verify that they are in full recovery mode if you want log backups toobe taken.</span></span> <span data-ttu-id="32468-170">Säkerhetskopior av tas aldrig för Master.</span><span class="sxs-lookup"><span data-stu-id="32468-170">Log backups are never taken for Master.</span></span> <span data-ttu-id="32468-171">Och inga säkerhetskopior har tagit för TempDB.</span><span class="sxs-lookup"><span data-stu-id="32468-171">And no backups are taken for TempDB.</span></span> |
| <span data-ttu-id="32468-172">**Schemat för säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="32468-172">**Backup Schedule**</span></span> | <span data-ttu-id="32468-173">Manuell/automatisk (automatisk)</span><span class="sxs-lookup"><span data-stu-id="32468-173">Manual/Automated (Automated)</span></span> | <span data-ttu-id="32468-174">Som standard bestäms hello Säkerhetskopieringsschemat automatiskt utifrån hello loggen tillväxt.</span><span class="sxs-lookup"><span data-stu-id="32468-174">By default, hello backup schedule will be automatically determined based on hello log growth.</span></span> <span data-ttu-id="32468-175">Manuell Säkerhetskopieringsschemat kan hello användaren toospecify hello tidsfönstret för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="32468-175">Manual backup schedule allows hello user toospecify hello time window for backups.</span></span> <span data-ttu-id="32468-176">I det här fallet säkerhetskopieringar endast tar med hello anges frekvens och under hello anges tidsfönster på en viss dag.</span><span class="sxs-lookup"><span data-stu-id="32468-176">In this case, backups will only ever take place at hello specified frequency and during hello specified time window of a given day.</span></span> |
| <span data-ttu-id="32468-177">**Frekvens för fullständig säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="32468-177">**Full backup frequency**</span></span> | <span data-ttu-id="32468-178">Varje dag/vecka</span><span class="sxs-lookup"><span data-stu-id="32468-178">Daily/Weekly</span></span> | <span data-ttu-id="32468-179">Frekvens för fullständiga säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="32468-179">Frequency of full backups.</span></span> <span data-ttu-id="32468-180">I båda fallen börjar fullständiga säkerhetskopieringar under hello nästa schemalagda tid.</span><span class="sxs-lookup"><span data-stu-id="32468-180">In both cases, full backups will begin during hello next scheduled time window.</span></span> <span data-ttu-id="32468-181">När varje vecka väljs säkerhetskopieringar kan sträcka sig över flera dagar tills alla databaser har säkerhetskopierats.</span><span class="sxs-lookup"><span data-stu-id="32468-181">When weekly is selected, backups could span multiple days until all databases have successfully backed up.</span></span> |
| <span data-ttu-id="32468-182">**Starttid för fullständig säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="32468-182">**Full backup start time**</span></span> | <span data-ttu-id="32468-183">00:00 – 23:00 (01:00)</span><span class="sxs-lookup"><span data-stu-id="32468-183">00:00 – 23:00 (01:00)</span></span> | <span data-ttu-id="32468-184">Starttid för en viss dag då fullständiga säkerhetskopieringar kan göras.</span><span class="sxs-lookup"><span data-stu-id="32468-184">Start time of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="32468-185">**Fullständig säkerhetskopiering tidsfönstret**</span><span class="sxs-lookup"><span data-stu-id="32468-185">**Full backup time window**</span></span> | <span data-ttu-id="32468-186">1 – 23 timmar (1 timme)</span><span class="sxs-lookup"><span data-stu-id="32468-186">1 – 23 hours (1 hour)</span></span> | <span data-ttu-id="32468-187">Varaktighet för hello tidsfönstret för en viss dag då fullständiga säkerhetskopieringar kan göras.</span><span class="sxs-lookup"><span data-stu-id="32468-187">Duration of hello time window of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="32468-188">**Frekvens för säkerhetskopiering av loggen**</span><span class="sxs-lookup"><span data-stu-id="32468-188">**Log backup frequency**</span></span> | <span data-ttu-id="32468-189">5 – 60 minuter (60 minuter)</span><span class="sxs-lookup"><span data-stu-id="32468-189">5 – 60 minutes (60 minutes)</span></span> | <span data-ttu-id="32468-190">Frekvensen för säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="32468-190">Frequency of log backups.</span></span> |

## <a name="understanding-full-backup-frequency"></a><span data-ttu-id="32468-191">Förstå frekvensen för fullständig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="32468-191">Understanding full backup frequency</span></span>
<span data-ttu-id="32468-192">Det är viktigt toounderstand hello skillnaden mellan dagliga och veckovisa fullständiga säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="32468-192">It is important toounderstand hello difference between daily and weekly full backups.</span></span> <span data-ttu-id="32468-193">I den här aktiviteten kommer vi att gå igenom två scenarier.</span><span class="sxs-lookup"><span data-stu-id="32468-193">In this effort, we will walk through two example scenarios.</span></span>

### <a name="scenario-1-weekly-backups"></a><span data-ttu-id="32468-194">Scenario 1: Veckovisa säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="32468-194">Scenario 1: Weekly backups</span></span>
<span data-ttu-id="32468-195">Du har en SQL Server-VM som innehåller ett antal mycket stora databaser.</span><span class="sxs-lookup"><span data-stu-id="32468-195">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="32468-196">Måndag aktivera automatisk säkerhetskopiering v2 med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="32468-196">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="32468-197">Schemat för säkerhetskopiering: **manuell**</span><span class="sxs-lookup"><span data-stu-id="32468-197">Backup schedule: **Manual**</span></span>
- <span data-ttu-id="32468-198">Fullständig säkerhetskopiering synkroniseringsfrekvensen: **varje vecka**</span><span class="sxs-lookup"><span data-stu-id="32468-198">Full backup frequency: **Weekly**</span></span>
- <span data-ttu-id="32468-199">Fullständig säkerhetskopiering starttiden: **01:00**</span><span class="sxs-lookup"><span data-stu-id="32468-199">Full backup start time: **01:00**</span></span>
- <span data-ttu-id="32468-200">Fullständig säkerhetskopiering tidsfönstret: **1 timme**</span><span class="sxs-lookup"><span data-stu-id="32468-200">Full backup time window: **1 hour**</span></span>

<span data-ttu-id="32468-201">Det innebär att hello nästa tillgängliga säkerhetskopieringsintervall finns tisdag kl 1 timma.</span><span class="sxs-lookup"><span data-stu-id="32468-201">This means that hello next available backup window is Tuesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="32468-202">Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget.</span><span class="sxs-lookup"><span data-stu-id="32468-202">At that time, Automated Backup will begin backing up your databases one at a time.</span></span> <span data-ttu-id="32468-203">I det här scenariot är databaserna tillräckligt stor för att slutförs fullständiga säkerhetskopieringar för hello första några databaser.</span><span class="sxs-lookup"><span data-stu-id="32468-203">In this scenario, your databases are large enough that full backups will complete for hello first couple databases.</span></span> <span data-ttu-id="32468-204">Men efter en timme har inte alla hello databaser säkerhetskopierats.</span><span class="sxs-lookup"><span data-stu-id="32468-204">However, after one hour not all of hello databases have been backed up.</span></span>

<span data-ttu-id="32468-205">När detta sker börjar automatisk säkerhetskopiering säkerhetskopiera hello återstående databaser hello nästa dag, onsdag kl 1 timma.</span><span class="sxs-lookup"><span data-stu-id="32468-205">When this happens, Automated Backup will begin backing up hello remaining databases hello next day, Wednesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="32468-206">Om inte alla databaser har säkerhetskopierats i den tid, görs ett försök igen hello nästa dag på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="32468-206">If not all databases have been backed up in that time, it will try again hello next day at hello same time.</span></span> <span data-ttu-id="32468-207">Detta fortsätter tills alla databaser har säkerhetskopierats.</span><span class="sxs-lookup"><span data-stu-id="32468-207">This will continue until all databases have been successfully backed up.</span></span>

<span data-ttu-id="32468-208">När den når tisdag, börjar automatisk säkerhetskopiering säkerhetskopiera alla databaser igen.</span><span class="sxs-lookup"><span data-stu-id="32468-208">Once it reaches Tuesday again, Automated Backup will begin backing up all databases once again.</span></span>

<span data-ttu-id="32468-209">Det här scenariot visar att automatisk säkerhetskopiering fungerar bara i hello angivna tidsfönstret, och varje databas säkerhetskopieras en gång per vecka.</span><span class="sxs-lookup"><span data-stu-id="32468-209">This scenario shows that Automated Backup will only operate within hello specified time window, and each database will be backed up once per week.</span></span> <span data-ttu-id="32468-210">Det visar att det är möjligt för säkerhetskopieringar toospan flera dagar i hello fall där det inte är möjligt toocomplete alla säkerhetskopior i en dag.</span><span class="sxs-lookup"><span data-stu-id="32468-210">This also shows that it is possible for backups toospan multiple days in hello case where it is not possible toocomplete all backups in a single day.</span></span>

### <a name="scenario-2-daily-backups"></a><span data-ttu-id="32468-211">Scenario 2: Daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="32468-211">Scenario 2: Daily backups</span></span>
<span data-ttu-id="32468-212">Du har en SQL Server-VM som innehåller ett antal mycket stora databaser.</span><span class="sxs-lookup"><span data-stu-id="32468-212">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="32468-213">Måndag aktivera automatisk säkerhetskopiering v2 med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="32468-213">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="32468-214">Schemat för säkerhetskopiering: manuell</span><span class="sxs-lookup"><span data-stu-id="32468-214">Backup schedule: Manual</span></span>
- <span data-ttu-id="32468-215">Fullständig säkerhetskopiering synkroniseringsfrekvensen: varje dag</span><span class="sxs-lookup"><span data-stu-id="32468-215">Full backup frequency: Daily</span></span>
- <span data-ttu-id="32468-216">Fullständig säkerhetskopiering starttiden: 22:00</span><span class="sxs-lookup"><span data-stu-id="32468-216">Full backup start time: 22:00</span></span>
- <span data-ttu-id="32468-217">Fullständig säkerhetskopiering tidsfönstret: 6 timmar</span><span class="sxs-lookup"><span data-stu-id="32468-217">Full backup time window: 6 hours</span></span>

<span data-ttu-id="32468-218">Detta innebär att hello nästa tillgängliga säkerhetskopieringsintervall är måndag klockan 10 i 6 timmar.</span><span class="sxs-lookup"><span data-stu-id="32468-218">This means that hello next available backup window is Monday at 10 PM for 6 hours.</span></span> <span data-ttu-id="32468-219">Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget.</span><span class="sxs-lookup"><span data-stu-id="32468-219">At that time, Automated Backup will begin backing up your databases one at a time.</span></span>

<span data-ttu-id="32468-220">Sedan tisdagen 10 i 6 timmar startas och fullständiga säkerhetskopieringar för alla databaser igen.</span><span class="sxs-lookup"><span data-stu-id="32468-220">Then, on Tuesday at 10 for 6 hours, full backups of all databases will start again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32468-221">När du schemalägger en daglig säkerhetskopiering, rekommenderas att du schemalägger en bred tid fönstret tooensure alla databaser kan säkerhetskopieras i den här gången.</span><span class="sxs-lookup"><span data-stu-id="32468-221">When scheduling daily backups, it is recommended that you schedule a wide time window tooensure all databases can be backed up within this time.</span></span> <span data-ttu-id="32468-222">Detta är särskilt viktigt i hello fall där du har en stor mängd data tooback upp.</span><span class="sxs-lookup"><span data-stu-id="32468-222">This is especially important in hello case where you have a large amount of data tooback up.</span></span>

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="32468-223">Konfigurationen i hello Portal</span><span class="sxs-lookup"><span data-stu-id="32468-223">Configuration in hello Portal</span></span>

<span data-ttu-id="32468-224">Du kan använda hello Azure portal tooconfigure automatisk säkerhetskopiering v2 under etableringen eller för befintliga SQL Server 2016 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="32468-224">You can use hello Azure portal tooconfigure Automated Backup v2 during provisioning or for existing SQL Server 2016 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="32468-225">Nya virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="32468-225">New VMs</span></span>

<span data-ttu-id="32468-226">Använd hello Azure portal tooconfigure automatisk säkerhetskopiering v2 när du skapar en ny SQL Server 2016-dator i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="32468-226">Use hello Azure portal tooconfigure Automated Backup v2 when you create a new SQL Server 2016 Virtual Machine in hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="32468-227">I hello **SQL Server-inställningar** bladet väljer **automatisk säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="32468-227">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="32468-228">hello följande Azure portal skärmbild visar hello **SQL automatisk säkerhetskopiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="32468-228">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![Automatisk säkerhetskopiering i SQL-konfigurationen i Azure-portalen](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> <span data-ttu-id="32468-230">Automatisk säkerhetskopiering v2 är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="32468-230">Automated Backup v2 is disabled by default.</span></span>

<span data-ttu-id="32468-231">Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="32468-231">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="32468-232">Befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="32468-232">Existing VMs</span></span>

<span data-ttu-id="32468-233">Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="32468-233">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="32468-234">Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="32468-234">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatisk säkerhetskopiering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

<span data-ttu-id="32468-236">I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk säkerhetskopiering avsnitt.</span><span class="sxs-lookup"><span data-stu-id="32468-236">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Konfigurera automatisk säkerhetskopiering i SQL för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

<span data-ttu-id="32468-238">När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="32468-238">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="32468-239">Om du aktiverar automatisk säkerhetskopiering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="32468-239">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="32468-240">Under denna tid kanske hello Azure-portalen inte visar att automatisk säkerhetskopiering har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="32468-240">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="32468-241">Vänta några minuter för hello agent toobe installerat kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="32468-241">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="32468-242">Efter att hello Azure visar portal hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="32468-242">After that hello Azure portal will reflect hello new settings.</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="32468-243">Med PowerShell</span><span class="sxs-lookup"><span data-stu-id="32468-243">Configuration with PowerShell</span></span>

<span data-ttu-id="32468-244">Du kan använda PowerShell tooconfigure automatisk säkerhetskopiering v2.</span><span class="sxs-lookup"><span data-stu-id="32468-244">You can use PowerShell tooconfigure Automated Backup v2.</span></span> <span data-ttu-id="32468-245">Innan du börjar måste du:</span><span class="sxs-lookup"><span data-stu-id="32468-245">Before you begin, you must:</span></span>

- <span data-ttu-id="32468-246">[Hämta och installera den senaste Azure PowerShell hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="32468-246">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="32468-247">Öppna Windows PowerShell och associera den med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="32468-247">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="32468-248">Du kan göra detta genom att följa stegen hello i hello [konfigurera prenumerationen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) avsnitt i hello etablering avsnittet.</span><span class="sxs-lookup"><span data-stu-id="32468-248">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="32468-249">Installera hello SQL IaaS-tillägg</span><span class="sxs-lookup"><span data-stu-id="32468-249">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="32468-250">Om du har etablerat en virtuell dator i SQL Server från hello Azure-portalen bör hello SQL Server IaaS-tillägget vara installerad.</span><span class="sxs-lookup"><span data-stu-id="32468-250">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="32468-251">Du kan fastställa om det har installerats för den virtuella datorn genom att anropa **Get-AzureRmVM** kommandot och undersöka hello **tillägg** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="32468-251">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

<span data-ttu-id="32468-252">Du bör se den visas som ”SqlIaaSAgent” eller ”SQLIaaSExtension” om hello tillägg för SQL Server IaaS-Agent är installerad.</span><span class="sxs-lookup"><span data-stu-id="32468-252">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="32468-253">**ProvisioningState** för hello-tillägget bör också visa ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="32468-253">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span> 

<span data-ttu-id="32468-254">Om det inte är installerat eller inte fungerar toobe etableras, kan du installera det med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="32468-254">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="32468-255">I tillägg toohello VM namn och resurs grupp, måste du också ange hello region (**$region**) som den virtuella datorn finns i.</span><span class="sxs-lookup"><span data-stu-id="32468-255">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <span data-ttu-id="32468-256"><a id="verifysettings"></a>Kontrollera aktuella inställningar</span><span class="sxs-lookup"><span data-stu-id="32468-256"><a id="verifysettings"></a> Verify current settings</span></span>
<span data-ttu-id="32468-257">Om du har aktiverat automatisk säkerhetskopiering under etablering kan du använda PowerShell toocheck din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="32468-257">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="32468-258">Kör hello **Get-AzureRmVMSqlServerExtension** kommando och granska hello **AutoBackupSettings** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="32468-258">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="32468-259">Du bör få utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="32468-259">You should get output similar toohello following:</span></span>

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

<span data-ttu-id="32468-260">Om din utdata visar att **aktivera** har angetts för**FALSKT**, och du har tooenable automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="32468-260">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="32468-261">hello bra är att aktivera och konfigurera automatisk säkerhetskopiering i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="32468-261">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="32468-262">Avsnittet hello nästa informationen.</span><span class="sxs-lookup"><span data-stu-id="32468-262">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="32468-263">Om du har kontrollerat hello inställningarna när du ändrar är det möjligt att du kommer tillbaka hello gamla konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="32468-263">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="32468-264">Vänta en stund och kontrollera inställningarna för hello igen toomake till att ändringarna har tillämpats.</span><span class="sxs-lookup"><span data-stu-id="32468-264">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup-v2"></a><span data-ttu-id="32468-265">Konfigurera automatisk säkerhetskopiering v2</span><span class="sxs-lookup"><span data-stu-id="32468-265">Configure Automated Backup v2</span></span>
<span data-ttu-id="32468-266">Du kan använda PowerShell tooenable automatisk säkerhetskopiering samt toomodify dess konfiguration och funktionalitet när som helst.</span><span class="sxs-lookup"><span data-stu-id="32468-266">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span> 

<span data-ttu-id="32468-267">Först, Välj eller skapa ett lagringskonto för hello säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="32468-267">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="32468-268">hello följande skript väljer ett lagringskonto eller skapar den om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="32468-268">hello following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="32468-269">Automatisk säkerhetskopiering stöder inte lagra säkerhetskopior i premium-lagring, men det kan göra säkerhetskopior från Virtuella diskar som använder Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="32468-269">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="32468-270">Använd hello **ny AzureRmVMSqlServerAutoBackupConfig** kommando tooenable och konfigurera hello automatisk säkerhetskopiering v2 inställningar toostore säkerhetskopieringar i hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32468-270">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup v2 settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="32468-271">I det här exemplet angetts hello säkerhetskopieringen toobe bevaras under 10 dagar.</span><span class="sxs-lookup"><span data-stu-id="32468-271">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="32468-272">System databassäkerhetskopiorna är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="32468-272">System database backups are enabled.</span></span> <span data-ttu-id="32468-273">Fullständiga säkerhetskopieringar är schemalagda för varje vecka med ett tidsfönster som börjar vid 20:00 i två timmar.</span><span class="sxs-lookup"><span data-stu-id="32468-273">Full backups are scheduled for weekly with a time window starting at 20:00 for two hours.</span></span> <span data-ttu-id="32468-274">Säkerhetskopieringar är schemalagda för var 30: e minut.</span><span class="sxs-lookup"><span data-stu-id="32468-274">Log backups are scheduled for every 30 minutes.</span></span> <span data-ttu-id="32468-275">Hej andra kommandot **Set AzureRmVMSqlServerExtension**, uppdateringar hello angivna virtuella Azure-datorn med de här inställningarna.</span><span class="sxs-lookup"><span data-stu-id="32468-275">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

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

<span data-ttu-id="32468-276">Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="32468-276">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span> 

<span data-ttu-id="32468-277">tooenable kryptering, ändra hello tidigare skriptet toopass hello **EnableEncryption** parametern tillsammans med ett lösenord (säker sträng) för hello **CertificatePassword** parameter.</span><span class="sxs-lookup"><span data-stu-id="32468-277">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="32468-278">hello följande skript kan hello inställningar för automatisk säkerhetskopiering i hello föregående exempel och lägger till kryptering.</span><span class="sxs-lookup"><span data-stu-id="32468-278">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

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

<span data-ttu-id="32468-279">inställningarna tillämpas tooconfirm [verifiera konfigurationen för automatisk säkerhetskopiering av hello](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="32468-279">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="32468-280">Inaktivera automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="32468-280">Disable Automated Backup</span></span>
<span data-ttu-id="32468-281">toodisable automatisk säkerhetskopiering, kör hello samma skript utan hello **-aktivera** parametern toohello **ny AzureRmVMSqlServerAutoBackupConfig** kommando.</span><span class="sxs-lookup"><span data-stu-id="32468-281">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="32468-282">Hej frånvaron av hello **-aktivera** parametern signaler hello kommandot toodisable hello funktionen.</span><span class="sxs-lookup"><span data-stu-id="32468-282">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="32468-283">Det kan ta flera minuter toodisable automatisk säkerhetskopiering som installationen.</span><span class="sxs-lookup"><span data-stu-id="32468-283">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="32468-284">Exempelskriptet</span><span class="sxs-lookup"><span data-stu-id="32468-284">Example script</span></span>
<span data-ttu-id="32468-285">hello följande skript innehåller en uppsättning variabler som du kan anpassa tooenable och konfigurera automatisk säkerhetskopiering för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="32468-285">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="32468-286">Du kan behöva toocustomize hello skript baserat på dina krav i ditt fall.</span><span class="sxs-lookup"><span data-stu-id="32468-286">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="32468-287">Du kan till exempel har ändrats och toomake om du vill använda toodisable hello säkerhetskopiering av systemdatabaser eller aktivera kryptering.</span><span class="sxs-lookup"><span data-stu-id="32468-287">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="32468-288">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32468-288">Next steps</span></span>
<span data-ttu-id="32468-289">Automatisk säkerhetskopiering v2 konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="32468-289">Automated Backup v2 configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="32468-290">Så det är viktigt för[hello i dokumentationen för hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello beteende och konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="32468-290">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="32468-291">Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande ämne hello: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="32468-291">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="32468-292">Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="32468-292">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="32468-293">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32468-293">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

