---
title: "Säkerhetskopiera din app i Azure"
description: "Lär dig hur du skapar säkerhetskopior av dina appar i Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 77e983afaaba8e944ab1f337e1c28ced83b63205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="ac163-103">Säkerhetskopiera din app i Azure</span><span class="sxs-lookup"><span data-stu-id="ac163-103">Back up your app in Azure</span></span>
<span data-ttu-id="ac163-104">Baksidan upp och funktionen för återställning i [Azure App Service](../app-service/app-service-value-prop-what-is.md) kan du enkelt skapa app säkerhetskopiering manuellt eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="ac163-104">The Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="ac163-105">Du kan återställa appen till en ögonblicksbild av ett tidigare tillstånd genom att skriva över den befintliga appen eller återställa till en annan app.</span><span class="sxs-lookup"><span data-stu-id="ac163-105">You can restore the app to a snapshot of a previous state by overwriting the existing app or restoring to another app.</span></span> 

<span data-ttu-id="ac163-106">Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="ac163-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="ac163-107">Vad säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="ac163-107">What gets backed up</span></span>
<span data-ttu-id="ac163-108">Apptjänst kan säkerhetskopiera följande information för ett Azure storage-konto och en behållare som du har konfigurerat din app ska använda.</span><span class="sxs-lookup"><span data-stu-id="ac163-108">App Service can backup the following information to an Azure storage account and container that you have configured your app to use.</span></span> 

* <span data-ttu-id="ac163-109">Appkonfiguration</span><span class="sxs-lookup"><span data-stu-id="ac163-109">App configuration</span></span>
* <span data-ttu-id="ac163-110">Filinnehåll</span><span class="sxs-lookup"><span data-stu-id="ac163-110">File content</span></span>
* <span data-ttu-id="ac163-111">Databas som är ansluten till din app</span><span class="sxs-lookup"><span data-stu-id="ac163-111">Database connected to your app</span></span>

<span data-ttu-id="ac163-112">Följande databaslösningar stöds med funktionen för säkerhetskopiering:</span><span class="sxs-lookup"><span data-stu-id="ac163-112">The following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="ac163-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="ac163-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="ac163-114">Azure-databas för MySQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="ac163-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="ac163-115">Azure-databas för PostgreSQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="ac163-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="ac163-116">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="ac163-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="ac163-117">MySQL i appen</span><span class="sxs-lookup"><span data-stu-id="ac163-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="ac163-118">Varje säkerhetskopiering är en fullständig offline kopia av din app, inte en inkrementell uppdatering.</span><span class="sxs-lookup"><span data-stu-id="ac163-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="ac163-119">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="ac163-119">Requirements and restrictions</span></span>
* <span data-ttu-id="ac163-120">Baksidan upp och återställa funktionen kräver App Service-plan i den **Standard** nivå eller **Premium** nivå.</span><span class="sxs-lookup"><span data-stu-id="ac163-120">The Back up and Restore feature requires the App Service plan to be in the **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="ac163-121">Läs mer om att skala din programtjänstplan att använda en högre nivå, [skala upp en app i Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="ac163-121">For more information about scaling your App Service plan to use a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="ac163-122">**Premium** nivå kan ett större antal daglig säkerhetskopiering än **Standard** nivå.</span><span class="sxs-lookup"><span data-stu-id="ac163-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="ac163-123">Du behöver ett Azure storage-konto och behållaren i samma prenumeration som den app som du vill säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="ac163-123">You need an Azure storage account and container in the same subscription as the app that you want to backup.</span></span> <span data-ttu-id="ac163-124">Mer information om Azure storage-konton finns i [länkar](#moreaboutstorage) i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ac163-124">For more information on Azure storage accounts, see the [links](#moreaboutstorage) at the end of this article.</span></span>
* <span data-ttu-id="ac163-125">Säkerhetskopieringar kan vara upp till 10 GB app- och innehåll.</span><span class="sxs-lookup"><span data-stu-id="ac163-125">Backups can be up to 10 GB of app and database content.</span></span> <span data-ttu-id="ac163-126">Om säkerhetskopians storlek överskrider denna gräns, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="ac163-126">If the backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="ac163-127">Skapa en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="ac163-127">Create a manual backup</span></span>
1. <span data-ttu-id="ac163-128">I den [Azure Portal](https://portal.azure.com)navigerar du till bladet för din app, Välj **säkerhetskopieringar**.</span><span class="sxs-lookup"><span data-stu-id="ac163-128">In the [Azure Portal](https://portal.azure.com), navigate to your app's blade, select **Backups**.</span></span> <span data-ttu-id="ac163-129">Den **säkerhetskopieringar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="ac163-129">The **Backups** blade will be displayed.</span></span>
   
    ![Säkerhetskopieringar sida][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="ac163-131">Om meddelandet nedan visas klickar du på den för att uppgradera din programtjänstplan innan du kan fortsätta med säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ac163-131">If you see the message below, click it to upgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="ac163-132">Se [skala upp en app i Azure](web-sites-scale.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ac163-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="ac163-133">![Välj lagringskonto](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="ac163-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="ac163-134">I den **säkerhetskopiering** bladet Klicka **konfigurera**
![Klicka på Konfigurera](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="ac163-134">In the **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="ac163-135">I den **konfigurering av säkerhetskopiering** bladet, klickar du på **lagring: inte konfigurerat** så här konfigurerar du ett storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ac163-135">In the **Backup Configuration** blade, click **Storage: Not configured** to configure a storage account.</span></span>
   
    ![Välj lagringskonto][ChooseStorageAccount]
4. <span data-ttu-id="ac163-137">Välj ditt mål för säkerhetskopian genom att välja en **Lagringskonto** och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="ac163-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="ac163-138">Storage-konto måste tillhöra samma prenumeration som den app du vill säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="ac163-138">The storage account must belong to the same subscription as the app you want to back up.</span></span> <span data-ttu-id="ac163-139">Om du vill kan du skapa ett nytt lagringskonto eller en ny behållare i respektive blad.</span><span class="sxs-lookup"><span data-stu-id="ac163-139">If you wish, you can create a new storage account or a new container in the respective blades.</span></span> <span data-ttu-id="ac163-140">När du är klar klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ac163-140">When you're done, click **Select**.</span></span>
   
    ![Välj lagringskonto](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="ac163-142">I den **konfigurering av säkerhetskopiering** bladet som lämnas fortfarande öppen kan du konfigurera **Backup Database**, sedan väljer du databaserna som du vill inkludera i säkerhetskopiering (SQL-databas eller MySQL) och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac163-142">In the **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select the databases you want to include in the backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Välj lagringskonto](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="ac163-144">För en databas ska visas i den här listan sin anslutningssträng måste finnas i den **anslutningssträngar** avsnitt i den **programinställningar** bladet för din app.</span><span class="sxs-lookup"><span data-stu-id="ac163-144">For a database to appear in this list, its connection string must exist in the **Connection strings** section of the **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="ac163-145">I den **konfigurering av säkerhetskopiering** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ac163-145">In the **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="ac163-146">I den **säkerhetskopieringar** bladet, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ac163-146">In the  **Backups** blade, click **Backup**.</span></span>
   
    ![Knappen BackUpNow][BackUpNow]
   
    <span data-ttu-id="ac163-148">Du kan se något pågående meddelande under säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="ac163-148">You see a progress message during the backup process.</span></span>

<span data-ttu-id="ac163-149">När lagringskontot och behållaren har konfigurerats kan du initiera en manuell säkerhetskopiering när som helst.</span><span class="sxs-lookup"><span data-stu-id="ac163-149">Once the storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="ac163-150">Konfigurera automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ac163-150">Configure automated backups</span></span>
1. <span data-ttu-id="ac163-151">I den **Säkerhetskopieringskonfigurationen** bladet ange **schemalagd säkerhetskopiering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="ac163-151">In the **Backup Configuration** blade, set **Scheduled backup** to **On**.</span></span> 
   
    ![Välj lagringskonto](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="ac163-153">Ange schemat för säkerhetskopiering alternativ visas, **schemalagd säkerhetskopiering** till **på**, konfigurerar schemat för säkerhetskopiering enligt önskemål och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac163-153">Backup schedule options will show up, set **Scheduled Backup** to **On**, then configure the backup schedule as desired and click **OK**.</span></span>
   
    ![Aktivera automatisk säkerhetskopiering][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="ac163-155">Konfigurera partiella säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="ac163-155">Configure Partial Backups</span></span>
<span data-ttu-id="ac163-156">Ibland vill du inte säkerhetskopiera allt på din app.</span><span class="sxs-lookup"><span data-stu-id="ac163-156">Sometimes you don't want to backup everything on your app.</span></span> <span data-ttu-id="ac163-157">Några exempel:</span><span class="sxs-lookup"><span data-stu-id="ac163-157">Here are a few examples:</span></span>

* <span data-ttu-id="ac163-158">Du [konfigurera veckovisa säkerhetskopior](web-sites-backup.md#configure-automated-backups) för din app med statiskt innehåll som aldrig ändras, till exempel gamla blogginlägg eller bilder.</span><span class="sxs-lookup"><span data-stu-id="ac163-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="ac163-159">Appen har över 10 GB innehåll (som är den högsta du kan säkerhetskopiera i taget).</span><span class="sxs-lookup"><span data-stu-id="ac163-159">Your app has over 10 GB of content (that's the max amount you can backup at a time).</span></span>
* <span data-ttu-id="ac163-160">Du vill inte säkerhetskopiera filerna.</span><span class="sxs-lookup"><span data-stu-id="ac163-160">You don't want to backup the log files.</span></span>

<span data-ttu-id="ac163-161">Partiella säkerhetskopior kan du ange exakt vilka filer som du vill säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="ac163-161">Partial backups allows you choose exactly which files you want to backup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="ac163-162">Undanta filer från säkerhetskopian</span><span class="sxs-lookup"><span data-stu-id="ac163-162">Exclude files from your backup</span></span>
<span data-ttu-id="ac163-163">Anta att du har en app som innehåller loggfiler och statiska bilder som har säkerhetskopiera en gång och inte kommer att ändras.</span><span class="sxs-lookup"><span data-stu-id="ac163-163">Suppose you have an app that contains log files and static images that have been backup once and are not going to change.</span></span> <span data-ttu-id="ac163-164">I sådana fall kan du utesluta dessa mappar och filer lagras i dina framtida säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="ac163-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="ac163-165">Om du vill undanta filer och mappar från säkerhetskopiorna, skapa en `_backup.filter` filen i den `D:\home\site\wwwroot` mappen för din app.</span><span class="sxs-lookup"><span data-stu-id="ac163-165">To exclude files and folders from your backups, create a `_backup.filter` file in the `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="ac163-166">Ange en lista över filer och mappar som ska läggas till i den här filen.</span><span class="sxs-lookup"><span data-stu-id="ac163-166">Specify the list of files and folders you want to exclude in this file.</span></span> 

<span data-ttu-id="ac163-167">Ett enkelt sätt att komma åt dina filer är att använda Kudu.</span><span class="sxs-lookup"><span data-stu-id="ac163-167">An easy way to access your files is to use Kudu .</span></span> <span data-ttu-id="ac163-168">Klicka på **avancerade verktyg -> Gå** för ditt webbprogram att komma åt Kudu.</span><span class="sxs-lookup"><span data-stu-id="ac163-168">Click **Advanced Tools -> Go** setting for your web app to access Kudu.</span></span>

![Kudu med hjälp av portalen][kudu-portal]

<span data-ttu-id="ac163-170">Identifiera de mappar som du vill undanta från säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="ac163-170">Identify the folders that you want to exclude from your backups.</span></span>  <span data-ttu-id="ac163-171">Till exempel att du vill filtrera bort de markerade kataloger och filer.</span><span class="sxs-lookup"><span data-stu-id="ac163-171">For example , you want to filter out the highlighted folder and files.</span></span>

![Mappen avbildningar][ImagesFolder]

<span data-ttu-id="ac163-173">Skapa en fil med namnet `_backup.filter` och placera listan ovan i filen, men ta bort `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="ac163-173">Create a file called `_backup.filter` and put the list above in the file, but remove `D:\home`.</span></span> <span data-ttu-id="ac163-174">Ange en mapp eller fil per rad.</span><span class="sxs-lookup"><span data-stu-id="ac163-174">List one directory or file per line.</span></span> <span data-ttu-id="ac163-175">Så att innehållet i filen bör vara:</span><span class="sxs-lookup"><span data-stu-id="ac163-175">So the content of the file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="ac163-176">Överför `_backup.filter` filen till den `D:\home\site\wwwroot\` för din webbplats med [ftp](web-sites-deploy.md#ftp) eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="ac163-176">Upload `_backup.filter` file to the `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="ac163-177">Om du vill kan du skapa filen direkt med Kudu `DebugConsole` och infoga det innehållet.</span><span class="sxs-lookup"><span data-stu-id="ac163-177">If you wish, you can create the file directly using Kudu  `DebugConsole` and insert the content there.</span></span>

<span data-ttu-id="ac163-178">Köra säkerhetskopieringar på samma sätt som du skulle göra det, [manuellt](#create-a-manual-backup) eller [automatiskt](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="ac163-178">Run backups the same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="ac163-179">Nu alla filer och mappar som anges i `_backup.filter` är exkluderad från framtida säkerhetskopieringar schemalagd eller manuell start.</span><span class="sxs-lookup"><span data-stu-id="ac163-179">Now, any files and folders that are specified in `_backup.filter` is excluded from the future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="ac163-180">Du kan återställa partiella säkerhetskopior av din webbplats på samma sätt som [återställa en säkerhetskopia av vanliga](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="ac163-180">You restore partial backups of your site the same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="ac163-181">Återställningen har rätta.</span><span class="sxs-lookup"><span data-stu-id="ac163-181">The restore process does the right thing.</span></span>
> 
> <span data-ttu-id="ac163-182">När en fullständig säkerhetskopia återställs ersätts allt innehåll på webbplatsen med säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="ac163-182">When a full backup is restored, all content on the site is replaced with whatever is in the backup.</span></span> <span data-ttu-id="ac163-183">Om en fil på platsen, men inte i säkerhetskopian tas bort.</span><span class="sxs-lookup"><span data-stu-id="ac163-183">If a file is on the site, but not in the backup it gets deleted.</span></span> <span data-ttu-id="ac163-184">Men när en partiell säkerhetskopia återställs allt innehåll som finns i en av de svartlistat katalogerna eller en svartlistat fil är kvar.</span><span class="sxs-lookup"><span data-stu-id="ac163-184">But when a partial backup is restored, any content that is located in one of the blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="ac163-185">Hur säkerhetskopior lagras</span><span class="sxs-lookup"><span data-stu-id="ac163-185">How backups are stored</span></span>
<span data-ttu-id="ac163-186">När du har gjort en eller flera säkerhetskopieringar för din app, säkerhetskopiorna är synliga på den **behållare** bladet för ditt lagringskonto och din app.</span><span class="sxs-lookup"><span data-stu-id="ac163-186">After you have made one or more backups for your app, the backups are visible on the **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="ac163-187">Storage-konto varje säkerhetskopiering består av en`.zip` -fil som innehåller den säkerhetskopiera informationen och en `.xml` -fil som innehåller ett manifest för den `.zip` filinnehåll.</span><span class="sxs-lookup"><span data-stu-id="ac163-187">In the storage account, each backup consists of a`.zip` file that contains the backup data and an `.xml` file that contains a manifest of the `.zip` file contents.</span></span> <span data-ttu-id="ac163-188">Du kan packa och bläddra bland dessa filer om du vill komma åt dina säkerhetskopieringar utan att faktiskt utföra en app-återställning.</span><span class="sxs-lookup"><span data-stu-id="ac163-188">You can unzip and browse these files if you want to access your backups without actually performing an app restore.</span></span>

<span data-ttu-id="ac163-189">Säkerhetskopieringen av databasen för appen lagras i roten av the.zip-filen.</span><span class="sxs-lookup"><span data-stu-id="ac163-189">The database backup for the app is stored in the root of the.zip file.</span></span> <span data-ttu-id="ac163-190">För en SQL-databas är en BACPAC-fil (utan filtillägget) och kan importeras.</span><span class="sxs-lookup"><span data-stu-id="ac163-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="ac163-191">Om du vill skapa en SQL-databas som är baserat på BACPAC export finns [importera en BACPAC-fil för att skapa en ny databas](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac163-191">To create a SQL database based on the BACPAC export, see [Import a BACPAC File to Create a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="ac163-192">Ändra alla filer i din **websitebackups** behållare kan orsaka att säkerhetskopieringen blir ogiltigt och därför icke-återställningsbara.</span><span class="sxs-lookup"><span data-stu-id="ac163-192">Altering any of the files in your **websitebackups** container can cause the backup to become invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="ac163-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac163-193">Next Steps</span></span>
<span data-ttu-id="ac163-194">Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="ac163-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="ac163-195">Du kan också säkerhetskopiera och återställa Apptjänst-appar med hjälp av REST-API (se [Använd REST säkerhetskopiera och återställa Apptjänst-appar](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="ac163-195">You can also backup and restore App Service apps using REST API (see [Use REST to backup and restore App Service apps](websites-csm-backup.md)).</span></span>


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

