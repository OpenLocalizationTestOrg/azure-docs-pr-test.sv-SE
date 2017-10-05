---
title: "Azure Analysis Services-databassäkerhetskopiering och återställning | Microsoft Docs"
description: "Beskriver hur du säkerhetskopierar och återställer en Azure Analysis Services-databas."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: bffa481a498b130ef1f2388a5ba856da5d164ee0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="backup-and-restore"></a><span data-ttu-id="a9574-103">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="a9574-103">Backup and restore</span></span>

<span data-ttu-id="a9574-104">Säkerhetskopiera tabellmodell databaser i Azure Analysis Services är ungefär desamma som för lokala Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="a9574-104">Backing up tabular model databases in Azure Analysis Services is much the same as for on-premises Analysis Services.</span></span> <span data-ttu-id="a9574-105">Den viktigaste skillnaden är där du vill lagra säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="a9574-105">The primary difference is where you store your backup files.</span></span> <span data-ttu-id="a9574-106">Säkerhetskopieringsfilerna måste sparas till en behållare i en [Azure storage-konto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="a9574-106">Backup files must be saved to a container in an [Azure storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="a9574-107">Du kan använda ett lagringskonto och behållare som du redan har eller de kan skapas när du konfigurerar inställningarna för servern.</span><span class="sxs-lookup"><span data-stu-id="a9574-107">You can use a storage account and container you already have, or they can be created when configuring storage settings for your server.</span></span>

> [!NOTE]
> <span data-ttu-id="a9574-108">Skapa ett lagringskonto kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="a9574-108">Creating a storage account can result in a new billable service.</span></span> <span data-ttu-id="a9574-109">Läs mer i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).</span><span class="sxs-lookup"><span data-stu-id="a9574-109">To learn more, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).</span></span>
> 
> 

<span data-ttu-id="a9574-110">Säkerhetskopieringar sparas med tillägget abf.</span><span class="sxs-lookup"><span data-stu-id="a9574-110">Backups are saved with an abf extension.</span></span> <span data-ttu-id="a9574-111">För InMemory-tabellmodeller lagras både modelldata och metadata.</span><span class="sxs-lookup"><span data-stu-id="a9574-111">For in-memory tabular models, both model data and metadata are stored.</span></span> <span data-ttu-id="a9574-112">Endast modellmetadata lagras för DirectQuery-tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="a9574-112">For DirectQuery tabular models, only model metadata is stored.</span></span> <span data-ttu-id="a9574-113">Säkerhetskopieringar kan komprimeras och krypteras, beroende på vilka alternativ du väljer.</span><span class="sxs-lookup"><span data-stu-id="a9574-113">Backups can be compressed and encrypted, depending on the options you choose.</span></span> 



## <a name="configure-storage-settings"></a><span data-ttu-id="a9574-114">Konfigurera inställningar för lagring</span><span class="sxs-lookup"><span data-stu-id="a9574-114">Configure storage settings</span></span>
<span data-ttu-id="a9574-115">Innan du säkerhetskopierar, måste du konfigurera inställningarna för servern.</span><span class="sxs-lookup"><span data-stu-id="a9574-115">Before backing up, you need to configure storage settings for your server.</span></span>


### <a name="to-configure-storage-settings"></a><span data-ttu-id="a9574-116">Så här konfigurerar du inställningar för lagring</span><span class="sxs-lookup"><span data-stu-id="a9574-116">To configure storage settings</span></span>
1.  <span data-ttu-id="a9574-117">I Azure-portalen > **inställningar**, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="a9574-117">In Azure portal > **Settings**, click **Backup**.</span></span>

    ![Säkerhetskopieringar i inställningarna](./media/analysis-services-backup/aas-backup-backups.png)

2.  <span data-ttu-id="a9574-119">Klicka på **aktiverad**, klicka på **Lagringsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="a9574-119">Click **Enabled**, then click **Storage Settings**.</span></span>

    ![Aktivera](./media/analysis-services-backup/aas-backup-enable.png)

3. <span data-ttu-id="a9574-121">Välj ditt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a9574-121">Select your storage account or create a new one.</span></span>

4. <span data-ttu-id="a9574-122">Välj en behållare eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a9574-122">Select a container or create a new one.</span></span>

    ![Välj behållare](./media/analysis-services-backup/aas-backup-container.png)

5. <span data-ttu-id="a9574-124">Spara inställningarna för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="a9574-124">Save your backup settings.</span></span>

    ![Spara inställningar för säkerhetskopiering](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a><span data-ttu-id="a9574-126">Säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="a9574-126">Backup</span></span>

### <a name="to-backup-by-using-ssms"></a><span data-ttu-id="a9574-127">Säkerhetskopiera med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="a9574-127">To backup by using SSMS</span></span>

1. <span data-ttu-id="a9574-128">Högerklicka på en databas i SSMS, > **säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="a9574-128">In SSMS, right-click a database > **Back Up**.</span></span>

2. <span data-ttu-id="a9574-129">I **Backup Database** > **säkerhetskopian**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="a9574-129">In **Backup Database** > **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="a9574-130">I den **spara fil som** dialogrutan verifiera mappsökvägen och skriv sedan ett namn för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="a9574-130">In the **Save file as** dialog, verify the folder path, and then type a name for the backup file.</span></span> 

4. <span data-ttu-id="a9574-131">I den **Backup Database** dialogrutan, Välj alternativ.</span><span class="sxs-lookup"><span data-stu-id="a9574-131">In the **Backup Database** dialog, select options.</span></span>

    <span data-ttu-id="a9574-132">**Tillåt skriva över** – Välj det här alternativet för att skriva över den säkerhetskopierade filer med samma namn.</span><span class="sxs-lookup"><span data-stu-id="a9574-132">**Allow file overwrite** - Select this option to overwrite backup files of the same name.</span></span> <span data-ttu-id="a9574-133">Om inte det här alternativet väljs kan inte du sparar filen ha samma namn som en fil som redan finns på samma plats.</span><span class="sxs-lookup"><span data-stu-id="a9574-133">If this option is not selected, the file you are saving cannot have the same name as a file that already exists in the same location.</span></span>

    <span data-ttu-id="a9574-134">**Tillämpa komprimering** – Välj det här alternativet om du vill komprimera säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="a9574-134">**Apply compression** - Select this option to compress the backup file.</span></span> <span data-ttu-id="a9574-135">Komprimerade säkerhetskopieringsfiler spara diskutrymme, men kräver något högre CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="a9574-135">Compressed backup files save disk space, but require slightly higher CPU utilization.</span></span> 

    <span data-ttu-id="a9574-136">**Kryptera säkerhetskopian** – Välj det här alternativet för att kryptera säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="a9574-136">**Encrypt backup file** - Select this option to encrypt the backup file.</span></span> <span data-ttu-id="a9574-137">Det här alternativet kräver ett användardefinierat lösenord för att skydda filen med säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="a9574-137">This option requires a user-supplied password to secure the backup file.</span></span> <span data-ttu-id="a9574-138">Lösenordet går inte att läsa av säkerhetskopierade data för något annat sätt än en återställning.</span><span class="sxs-lookup"><span data-stu-id="a9574-138">The password prevents reading of the backup data any other means than a restore operation.</span></span> <span data-ttu-id="a9574-139">Om du väljer att kryptera säkerhetskopiorna lagra lösenordet på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="a9574-139">If you choose to encrypt backups, store the password in a safe location.</span></span>

5. <span data-ttu-id="a9574-140">Klicka på **OK** att skapa och spara säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="a9574-140">Click **OK** to create and save the backup file.</span></span>


### <a name="powershell"></a><span data-ttu-id="a9574-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9574-141">PowerShell</span></span>
<span data-ttu-id="a9574-142">Använd [säkerhetskopiering ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9574-142">Use [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span></span>

## <a name="restore"></a><span data-ttu-id="a9574-143">Återställ</span><span class="sxs-lookup"><span data-stu-id="a9574-143">Restore</span></span>
<span data-ttu-id="a9574-144">När du återställer, måste din säkerhetskopia vara i storage-konto som du har konfigurerat för din server.</span><span class="sxs-lookup"><span data-stu-id="a9574-144">When restoring, your backup file must be in the storage account you've configured for your server.</span></span> <span data-ttu-id="a9574-145">Om du behöver flytta en säkerhetskopia från en lokal plats till ditt lagringskonto använder [Microsoft Azure Lagringsutforskaren](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) eller [AzCopy](../storage/common/storage-use-azcopy.md) kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="a9574-145">If you need to move a backup file from an on-premises location to your storage account, use [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) or the [AzCopy](../storage/common/storage-use-azcopy.md) command-line utility.</span></span> 



> [!NOTE]
> <span data-ttu-id="a9574-146">Om du återställa från en lokal server, måste du ta bort alla domänanvändare från modellens roller och lägga till dem igen rollerna som Azure Active Directory-användare.</span><span class="sxs-lookup"><span data-stu-id="a9574-146">If you're restoring from an on-premises server, you must remove all the domain users from the model's roles and add them back to the roles as Azure Active Directory users.</span></span>
> 
> 

### <a name="to-restore-by-using-ssms"></a><span data-ttu-id="a9574-147">Återställa med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="a9574-147">To restore by using SSMS</span></span>

1. <span data-ttu-id="a9574-148">Högerklicka på en databas i SSMS, > **återställa**.</span><span class="sxs-lookup"><span data-stu-id="a9574-148">In SSMS, right-click a database > **Restore**.</span></span>

2. <span data-ttu-id="a9574-149">I den **Backup Database** dialogrutan i **säkerhetskopian**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="a9574-149">In the **Backup Database** dialog, in **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="a9574-150">I den **Sök efter databasfiler** dialogrutan, Välj den fil som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="a9574-150">In the **Locate Database Files** dialog, select the file you want to restore.</span></span>

4. <span data-ttu-id="a9574-151">I **återställningsdatabasen**, välj sedan databasen.</span><span class="sxs-lookup"><span data-stu-id="a9574-151">In **Restore database**, select the database.</span></span>

5. <span data-ttu-id="a9574-152">Ange alternativ.</span><span class="sxs-lookup"><span data-stu-id="a9574-152">Specify options.</span></span> <span data-ttu-id="a9574-153">Säkerhetsalternativ måste matcha de alternativ som du använde när du säkerhetskopierar.</span><span class="sxs-lookup"><span data-stu-id="a9574-153">Security options must match the backup options you used when backing up.</span></span>


### <a name="powershell"></a><span data-ttu-id="a9574-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9574-154">PowerShell</span></span>

<span data-ttu-id="a9574-155">Använd [återställning ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a9574-155">Use [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span></span>


## <a name="related-information"></a><span data-ttu-id="a9574-156">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="a9574-156">Related information</span></span>

[<span data-ttu-id="a9574-157">Azure storage-konton</span><span class="sxs-lookup"><span data-stu-id="a9574-157">Azure storage accounts</span></span>](../storage/common/storage-create-storage-account.md)  
<span data-ttu-id="a9574-158">[Hög tillgänglighet](analysis-services-bcdr.md)   </span><span class="sxs-lookup"><span data-stu-id="a9574-158">[High availability](analysis-services-bcdr.md)   </span></span>  
[<span data-ttu-id="a9574-159">Hantera Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="a9574-159">Manage Azure Analysis Services</span></span>](analysis-services-manage.md)
