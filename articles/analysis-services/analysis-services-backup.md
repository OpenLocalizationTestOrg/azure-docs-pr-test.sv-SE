---
title: "aaaAzure Analysis Services-databas säkerhetskopierar och återställer | Microsoft Docs"
description: "Beskriver hur toobackup och återställning av en Azure Analysis Services-databasen."
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
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a><span data-ttu-id="21b0b-103">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="21b0b-103">Backup and restore</span></span>

<span data-ttu-id="21b0b-104">Säkerhetskopiera tabellmodell databaser i Azure Analysis Services är mycket hello samma som för lokala Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="21b0b-104">Backing up tabular model databases in Azure Analysis Services is much hello same as for on-premises Analysis Services.</span></span> <span data-ttu-id="21b0b-105">hello viktigaste skillnaden är där du vill lagra säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="21b0b-105">hello primary difference is where you store your backup files.</span></span> <span data-ttu-id="21b0b-106">Säkerhetskopieringsfilerna måste sparas tooa behållare i en [Azure storage-konto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="21b0b-106">Backup files must be saved tooa container in an [Azure storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="21b0b-107">Du kan använda ett lagringskonto och behållare som du redan har eller de kan skapas när du konfigurerar inställningarna för servern.</span><span class="sxs-lookup"><span data-stu-id="21b0b-107">You can use a storage account and container you already have, or they can be created when configuring storage settings for your server.</span></span>

> [!NOTE]
> <span data-ttu-id="21b0b-108">Skapa ett lagringskonto kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="21b0b-108">Creating a storage account can result in a new billable service.</span></span> <span data-ttu-id="21b0b-109">Det finns fler toolearn [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).</span><span class="sxs-lookup"><span data-stu-id="21b0b-109">toolearn more, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).</span></span>
> 
> 

<span data-ttu-id="21b0b-110">Säkerhetskopieringar sparas med tillägget abf.</span><span class="sxs-lookup"><span data-stu-id="21b0b-110">Backups are saved with an abf extension.</span></span> <span data-ttu-id="21b0b-111">För InMemory-tabellmodeller lagras både modelldata och metadata.</span><span class="sxs-lookup"><span data-stu-id="21b0b-111">For in-memory tabular models, both model data and metadata are stored.</span></span> <span data-ttu-id="21b0b-112">Endast modellmetadata lagras för DirectQuery-tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="21b0b-112">For DirectQuery tabular models, only model metadata is stored.</span></span> <span data-ttu-id="21b0b-113">Säkerhetskopieringar kan komprimeras och krypteras, beroende på hello du väljer.</span><span class="sxs-lookup"><span data-stu-id="21b0b-113">Backups can be compressed and encrypted, depending on hello options you choose.</span></span> 



## <a name="configure-storage-settings"></a><span data-ttu-id="21b0b-114">Konfigurera inställningar för lagring</span><span class="sxs-lookup"><span data-stu-id="21b0b-114">Configure storage settings</span></span>
<span data-ttu-id="21b0b-115">Innan du säkerhetskopierar, måste tooconfigure lagringsinställningarna för servern.</span><span class="sxs-lookup"><span data-stu-id="21b0b-115">Before backing up, you need tooconfigure storage settings for your server.</span></span>


### <a name="tooconfigure-storage-settings"></a><span data-ttu-id="21b0b-116">tooconfigure Lagringsinställningar</span><span class="sxs-lookup"><span data-stu-id="21b0b-116">tooconfigure storage settings</span></span>
1.  <span data-ttu-id="21b0b-117">I Azure-portalen > **inställningar**, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-117">In Azure portal > **Settings**, click **Backup**.</span></span>

    ![Säkerhetskopieringar i inställningarna](./media/analysis-services-backup/aas-backup-backups.png)

2.  <span data-ttu-id="21b0b-119">Klicka på **aktiverad**, klicka på **Lagringsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-119">Click **Enabled**, then click **Storage Settings**.</span></span>

    ![Aktivera](./media/analysis-services-backup/aas-backup-enable.png)

3. <span data-ttu-id="21b0b-121">Välj ditt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="21b0b-121">Select your storage account or create a new one.</span></span>

4. <span data-ttu-id="21b0b-122">Välj en behållare eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="21b0b-122">Select a container or create a new one.</span></span>

    ![Välj behållare](./media/analysis-services-backup/aas-backup-container.png)

5. <span data-ttu-id="21b0b-124">Spara inställningarna för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="21b0b-124">Save your backup settings.</span></span>

    ![Spara inställningar för säkerhetskopiering](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a><span data-ttu-id="21b0b-126">Säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="21b0b-126">Backup</span></span>

### <a name="toobackup-by-using-ssms"></a><span data-ttu-id="21b0b-127">toobackup med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="21b0b-127">toobackup by using SSMS</span></span>

1. <span data-ttu-id="21b0b-128">Högerklicka på en databas i SSMS, > **säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-128">In SSMS, right-click a database > **Back Up**.</span></span>

2. <span data-ttu-id="21b0b-129">I **Backup Database** > **säkerhetskopian**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-129">In **Backup Database** > **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="21b0b-130">I hello **spara fil som** dialogrutan verifiera hello mappsökvägen och skriv sedan ett namn för hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="21b0b-130">In hello **Save file as** dialog, verify hello folder path, and then type a name for hello backup file.</span></span> 

4. <span data-ttu-id="21b0b-131">I hello **Backup Database** dialogrutan, Välj alternativ.</span><span class="sxs-lookup"><span data-stu-id="21b0b-131">In hello **Backup Database** dialog, select options.</span></span>

    <span data-ttu-id="21b0b-132">**Tillåt skriva över** – Välj det här alternativet toooverwrite säkerhetskopior av hello samma namn.</span><span class="sxs-lookup"><span data-stu-id="21b0b-132">**Allow file overwrite** - Select this option toooverwrite backup files of hello same name.</span></span> <span data-ttu-id="21b0b-133">Om inte det här alternativet väljs hello-filen som du sparar kan inte ha hello samma namn som en fil som redan finns i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="21b0b-133">If this option is not selected, hello file you are saving cannot have hello same name as a file that already exists in hello same location.</span></span>

    <span data-ttu-id="21b0b-134">**Tillämpa komprimering** – Välj det här alternativet toocompress hello säkerhetskopierade filen.</span><span class="sxs-lookup"><span data-stu-id="21b0b-134">**Apply compression** - Select this option toocompress hello backup file.</span></span> <span data-ttu-id="21b0b-135">Komprimerade säkerhetskopieringsfiler spara diskutrymme, men kräver något högre CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="21b0b-135">Compressed backup files save disk space, but require slightly higher CPU utilization.</span></span> 

    <span data-ttu-id="21b0b-136">**Kryptera säkerhetskopian** – Välj det här alternativet tooencrypt hello säkerhetskopierade filen.</span><span class="sxs-lookup"><span data-stu-id="21b0b-136">**Encrypt backup file** - Select this option tooencrypt hello backup file.</span></span> <span data-ttu-id="21b0b-137">Det här alternativet kräver en användardefinierat lösenord toosecure hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="21b0b-137">This option requires a user-supplied password toosecure hello backup file.</span></span> <span data-ttu-id="21b0b-138">hello lösenordet går inte att läsa av hello säkerhetskopieringsdata något annat sätt än en återställning.</span><span class="sxs-lookup"><span data-stu-id="21b0b-138">hello password prevents reading of hello backup data any other means than a restore operation.</span></span> <span data-ttu-id="21b0b-139">Om du väljer tooencrypt säkerhetskopieringar hello lösenordet lagras på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="21b0b-139">If you choose tooencrypt backups, store hello password in a safe location.</span></span>

5. <span data-ttu-id="21b0b-140">Klicka på **OK** toocreate och spara hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="21b0b-140">Click **OK** toocreate and save hello backup file.</span></span>


### <a name="powershell"></a><span data-ttu-id="21b0b-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21b0b-141">PowerShell</span></span>
<span data-ttu-id="21b0b-142">Använd [säkerhetskopiering ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="21b0b-142">Use [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span></span>

## <a name="restore"></a><span data-ttu-id="21b0b-143">Återställ</span><span class="sxs-lookup"><span data-stu-id="21b0b-143">Restore</span></span>
<span data-ttu-id="21b0b-144">När du återställer, måste din säkerhetskopia vara i hello storage-konto som du har konfigurerat för din server.</span><span class="sxs-lookup"><span data-stu-id="21b0b-144">When restoring, your backup file must be in hello storage account you've configured for your server.</span></span> <span data-ttu-id="21b0b-145">Om du behöver toomove säkerhetskopian från en lokal plats tooyour storage-konto, använder [Microsoft Azure Lagringsutforskaren](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) eller hello [AzCopy](../storage/common/storage-use-azcopy.md) kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="21b0b-145">If you need toomove a backup file from an on-premises location tooyour storage account, use [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) or hello [AzCopy](../storage/common/storage-use-azcopy.md) command-line utility.</span></span> 



> [!NOTE]
> <span data-ttu-id="21b0b-146">Om du återställa från en lokal server, måste du ta bort alla hello domänanvändare från hello modellen roller och Lägg till dem tillbaka toohello roller som Azure Active Directory-användare.</span><span class="sxs-lookup"><span data-stu-id="21b0b-146">If you're restoring from an on-premises server, you must remove all hello domain users from hello model's roles and add them back toohello roles as Azure Active Directory users.</span></span>
> 
> 

### <a name="toorestore-by-using-ssms"></a><span data-ttu-id="21b0b-147">toorestore med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="21b0b-147">toorestore by using SSMS</span></span>

1. <span data-ttu-id="21b0b-148">Högerklicka på en databas i SSMS, > **återställa**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-148">In SSMS, right-click a database > **Restore**.</span></span>

2. <span data-ttu-id="21b0b-149">I hello **Backup Database** dialogrutan i **säkerhetskopian**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="21b0b-149">In hello **Backup Database** dialog, in **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="21b0b-150">I hello **Sök efter databasfiler** dialogrutan, Välj hello-fil som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="21b0b-150">In hello **Locate Database Files** dialog, select hello file you want toorestore.</span></span>

4. <span data-ttu-id="21b0b-151">I **återställningsdatabasen**väljer hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="21b0b-151">In **Restore database**, select hello database.</span></span>

5. <span data-ttu-id="21b0b-152">Ange alternativ.</span><span class="sxs-lookup"><span data-stu-id="21b0b-152">Specify options.</span></span> <span data-ttu-id="21b0b-153">Säkerhetsalternativ måste matcha hello säkerhetskopieringsalternativ som du använde när du säkerhetskopierar.</span><span class="sxs-lookup"><span data-stu-id="21b0b-153">Security options must match hello backup options you used when backing up.</span></span>


### <a name="powershell"></a><span data-ttu-id="21b0b-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21b0b-154">PowerShell</span></span>

<span data-ttu-id="21b0b-155">Använd [återställning ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="21b0b-155">Use [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span></span>


## <a name="related-information"></a><span data-ttu-id="21b0b-156">Relaterad information</span><span class="sxs-lookup"><span data-stu-id="21b0b-156">Related information</span></span>

[<span data-ttu-id="21b0b-157">Azure storage-konton</span><span class="sxs-lookup"><span data-stu-id="21b0b-157">Azure storage accounts</span></span>](../storage/common/storage-create-storage-account.md)  
<span data-ttu-id="21b0b-158">[Hög tillgänglighet](analysis-services-bcdr.md)   </span><span class="sxs-lookup"><span data-stu-id="21b0b-158">[High availability](analysis-services-bcdr.md)   </span></span>  
[<span data-ttu-id="21b0b-159">Hantera Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="21b0b-159">Manage Azure Analysis Services</span></span>](analysis-services-manage.md)
