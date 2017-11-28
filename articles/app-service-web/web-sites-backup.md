---
title: aaaBack in din app i Azure
description: "Lär dig hur toocreate säkerhetskopior av dina appar i Azure App Service."
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
ms.openlocfilehash: e41d93d322bbc48b45b28eeaa817928d83c2b9d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="9d153-103">Säkerhetskopiera din app i Azure</span><span class="sxs-lookup"><span data-stu-id="9d153-103">Back up your app in Azure</span></span>
<span data-ttu-id="9d153-104">Hej säkerhetskopiera och återställa funktion i [Azure App Service](../app-service/app-service-value-prop-what-is.md) kan du enkelt skapa app säkerhetskopiering manuellt eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="9d153-104">hello Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="9d153-105">Du kan återställa hello app tooa ögonblicksbild av ett tidigare tillstånd genom överskrivning hello befintliga app eller återställa tooanother app.</span><span class="sxs-lookup"><span data-stu-id="9d153-105">You can restore hello app tooa snapshot of a previous state by overwriting hello existing app or restoring tooanother app.</span></span> 

<span data-ttu-id="9d153-106">Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="9d153-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="9d153-107">Vad säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="9d153-107">What gets backed up</span></span>
<span data-ttu-id="9d153-108">Apptjänst kan säkerhetskopiera hello följande information tooan Azure storage-konto och en behållare som du har konfigurerat din app toouse.</span><span class="sxs-lookup"><span data-stu-id="9d153-108">App Service can backup hello following information tooan Azure storage account and container that you have configured your app toouse.</span></span> 

* <span data-ttu-id="9d153-109">Appkonfiguration</span><span class="sxs-lookup"><span data-stu-id="9d153-109">App configuration</span></span>
* <span data-ttu-id="9d153-110">Filinnehåll</span><span class="sxs-lookup"><span data-stu-id="9d153-110">File content</span></span>
* <span data-ttu-id="9d153-111">Databasen anslutna tooyour app</span><span class="sxs-lookup"><span data-stu-id="9d153-111">Database connected tooyour app</span></span>

<span data-ttu-id="9d153-112">hello följande databaslösningar för stöds med funktionen för säkerhetskopiering:</span><span class="sxs-lookup"><span data-stu-id="9d153-112">hello following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="9d153-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="9d153-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="9d153-114">Azure-databas för MySQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9d153-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="9d153-115">Azure-databas för PostgreSQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9d153-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="9d153-116">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="9d153-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="9d153-117">MySQL i appen</span><span class="sxs-lookup"><span data-stu-id="9d153-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="9d153-118">Varje säkerhetskopiering är en fullständig offline kopia av din app, inte en inkrementell uppdatering.</span><span class="sxs-lookup"><span data-stu-id="9d153-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="9d153-119">Krav och begränsningar</span><span class="sxs-lookup"><span data-stu-id="9d153-119">Requirements and restrictions</span></span>
* <span data-ttu-id="9d153-120">Hej säkerhetskopiera och återställa funktionen kräver hello App Service-plan toobe i hello **Standard** nivå eller **Premium** nivå.</span><span class="sxs-lookup"><span data-stu-id="9d153-120">hello Back up and Restore feature requires hello App Service plan toobe in hello **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="9d153-121">Läs mer om att skala din App Service-plan toouse en högre nivå, [skala upp en app i Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="9d153-121">For more information about scaling your App Service plan toouse a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="9d153-122">**Premium** nivå kan ett större antal daglig säkerhetskopiering än **Standard** nivå.</span><span class="sxs-lookup"><span data-stu-id="9d153-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="9d153-123">Du behöver ett Azure storage-konto och en behållare i hello samma prenumeration som hello app som du vill toobackup.</span><span class="sxs-lookup"><span data-stu-id="9d153-123">You need an Azure storage account and container in hello same subscription as hello app that you want toobackup.</span></span> <span data-ttu-id="9d153-124">Mer information om Azure storage-konton finns hello [länkar](#moreaboutstorage) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9d153-124">For more information on Azure storage accounts, see hello [links](#moreaboutstorage) at hello end of this article.</span></span>
* <span data-ttu-id="9d153-125">Säkerhetskopieringar kan vara upp too10 GB app- och innehåll.</span><span class="sxs-lookup"><span data-stu-id="9d153-125">Backups can be up too10 GB of app and database content.</span></span> <span data-ttu-id="9d153-126">Hej säkerhetskopiestorlek överskrider denna gräns, får du ett fel.</span><span class="sxs-lookup"><span data-stu-id="9d153-126">If hello backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="9d153-127">Skapa en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="9d153-127">Create a manual backup</span></span>
1. <span data-ttu-id="9d153-128">I hello [Azure Portal](https://portal.azure.com)navigerar tooyour appens blad, Välj **säkerhetskopieringar**.</span><span class="sxs-lookup"><span data-stu-id="9d153-128">In hello [Azure Portal](https://portal.azure.com), navigate tooyour app's blade, select **Backups**.</span></span> <span data-ttu-id="9d153-129">Hej **säkerhetskopieringar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="9d153-129">hello **Backups** blade will be displayed.</span></span>
   
    ![Säkerhetskopieringar sida][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="9d153-131">Om du ser nedan hello-meddelande, klicka på den tooupgrade din programtjänstplan innan du kan fortsätta med säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="9d153-131">If you see hello message below, click it tooupgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="9d153-132">Se [skala upp en app i Azure](web-sites-scale.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9d153-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="9d153-133">![Välj lagringskonto](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="9d153-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="9d153-134">I hello **säkerhetskopiering** bladet Klicka **konfigurera**
![Klicka på Konfigurera](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="9d153-134">In hello **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="9d153-135">I hello **konfigurering av säkerhetskopiering** bladet, klickar du på **lagring: inte konfigurerat** tooconfigure ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9d153-135">In hello **Backup Configuration** blade, click **Storage: Not configured** tooconfigure a storage account.</span></span>
   
    ![Välj lagringskonto][ChooseStorageAccount]
4. <span data-ttu-id="9d153-137">Välj ditt mål för säkerhetskopian genom att välja en **Lagringskonto** och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="9d153-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="9d153-138">hello storage-kontot måste höra toohello samma prenumeration som hello-app som du vill att tooback.</span><span class="sxs-lookup"><span data-stu-id="9d153-138">hello storage account must belong toohello same subscription as hello app you want tooback up.</span></span> <span data-ttu-id="9d153-139">Om du vill kan skapa du ett nytt lagringskonto eller en ny behållare i respektive hello-blad.</span><span class="sxs-lookup"><span data-stu-id="9d153-139">If you wish, you can create a new storage account or a new container in hello respective blades.</span></span> <span data-ttu-id="9d153-140">När du är klar klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="9d153-140">When you're done, click **Select**.</span></span>
   
    ![Välj lagringskonto](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="9d153-142">I hello **konfigurering av säkerhetskopiering** bladet som lämnas fortfarande öppen kan du konfigurera **Backup Database**, Välj hello databaser du vill tooinclude hello säkerhetskopior (SQL-databas eller MySQL), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d153-142">In hello **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select hello databases you want tooinclude in hello backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Välj lagringskonto](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="9d153-144">För en databas tooappear i den här listan sin anslutningssträng måste finnas i hello **anslutningssträngar** avsnitt i hello **programinställningar** bladet för din app.</span><span class="sxs-lookup"><span data-stu-id="9d153-144">For a database tooappear in this list, its connection string must exist in hello **Connection strings** section of hello **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="9d153-145">I hello **konfigurering av säkerhetskopiering** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9d153-145">In hello **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="9d153-146">I hello **säkerhetskopieringar** bladet, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="9d153-146">In hello  **Backups** blade, click **Backup**.</span></span>
   
    ![Knappen BackUpNow][BackUpNow]
   
    <span data-ttu-id="9d153-148">Du ser något pågående meddelande under hello säkerhetskopieringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="9d153-148">You see a progress message during hello backup process.</span></span>

<span data-ttu-id="9d153-149">När hello storage-konto och behållaren har konfigurerats kan du initiera en manuell säkerhetskopiering när som helst.</span><span class="sxs-lookup"><span data-stu-id="9d153-149">Once hello storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="9d153-150">Konfigurera automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="9d153-150">Configure automated backups</span></span>
1. <span data-ttu-id="9d153-151">I hello **Säkerhetskopieringskonfigurationen** bladet ange **schemalagd säkerhetskopiering** för**på**.</span><span class="sxs-lookup"><span data-stu-id="9d153-151">In hello **Backup Configuration** blade, set **Scheduled backup** too**On**.</span></span> 
   
    ![Välj lagringskonto](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="9d153-153">Ange schemat för säkerhetskopiering alternativ visas, **schemalagd säkerhetskopiering** för**på**, konfigurerar schemat för säkerhetskopiering av hello enligt önskemål och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d153-153">Backup schedule options will show up, set **Scheduled Backup** too**On**, then configure hello backup schedule as desired and click **OK**.</span></span>
   
    ![Aktivera automatisk säkerhetskopiering][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="9d153-155">Konfigurera partiella säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="9d153-155">Configure Partial Backups</span></span>
<span data-ttu-id="9d153-156">Ibland vill du inte toobackup allt på din app.</span><span class="sxs-lookup"><span data-stu-id="9d153-156">Sometimes you don't want toobackup everything on your app.</span></span> <span data-ttu-id="9d153-157">Några exempel:</span><span class="sxs-lookup"><span data-stu-id="9d153-157">Here are a few examples:</span></span>

* <span data-ttu-id="9d153-158">Du [konfigurera veckovisa säkerhetskopior](web-sites-backup.md#configure-automated-backups) för din app med statiskt innehåll som aldrig ändras, till exempel gamla blogginlägg eller bilder.</span><span class="sxs-lookup"><span data-stu-id="9d153-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="9d153-159">Appen har över 10 GB innehåll (som är Hej max du kan säkerhetskopiera i taget).</span><span class="sxs-lookup"><span data-stu-id="9d153-159">Your app has over 10 GB of content (that's hello max amount you can backup at a time).</span></span>
* <span data-ttu-id="9d153-160">Vill du inte toobackup hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="9d153-160">You don't want toobackup hello log files.</span></span>

<span data-ttu-id="9d153-161">Partiella säkerhetskopior kan du ange exakt vilka filer som du vill toobackup.</span><span class="sxs-lookup"><span data-stu-id="9d153-161">Partial backups allows you choose exactly which files you want toobackup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="9d153-162">Undanta filer från säkerhetskopian</span><span class="sxs-lookup"><span data-stu-id="9d153-162">Exclude files from your backup</span></span>
<span data-ttu-id="9d153-163">Anta att du har en app som innehåller loggfiler och statiska bilder som har säkerhetskopiera en gång och inte kommer toochange.</span><span class="sxs-lookup"><span data-stu-id="9d153-163">Suppose you have an app that contains log files and static images that have been backup once and are not going toochange.</span></span> <span data-ttu-id="9d153-164">I sådana fall kan du utesluta dessa mappar och filer lagras i dina framtida säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="9d153-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="9d153-165">tooexclude filer och mappar från säkerhetskopiorna, skapa en `_backup.filter` filen i hello `D:\home\site\wwwroot` mappen för din app.</span><span class="sxs-lookup"><span data-stu-id="9d153-165">tooexclude files and folders from your backups, create a `_backup.filter` file in hello `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="9d153-166">Ange hello lista över filer och mappar som du vill tooexclude i den här filen.</span><span class="sxs-lookup"><span data-stu-id="9d153-166">Specify hello list of files and folders you want tooexclude in this file.</span></span> 

<span data-ttu-id="9d153-167">Ett enkelt sätt tooaccess dina filer är toouse Kudu.</span><span class="sxs-lookup"><span data-stu-id="9d153-167">An easy way tooaccess your files is toouse Kudu .</span></span> <span data-ttu-id="9d153-168">Klicka på **avancerade verktyg -> Gå** för din web app tooaccess Kudu.</span><span class="sxs-lookup"><span data-stu-id="9d153-168">Click **Advanced Tools -> Go** setting for your web app tooaccess Kudu.</span></span>

![Kudu med hjälp av portalen][kudu-portal]

<span data-ttu-id="9d153-170">Identifiera hello mappar som du vill tooexclude från säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="9d153-170">Identify hello folders that you want tooexclude from your backups.</span></span>  <span data-ttu-id="9d153-171">Till exempel vill du toofilter ut hello markerade mappen och filer.</span><span class="sxs-lookup"><span data-stu-id="9d153-171">For example , you want toofilter out hello highlighted folder and files.</span></span>

![Mappen avbildningar][ImagesFolder]

<span data-ttu-id="9d153-173">Skapa en fil med namnet `_backup.filter` och placera hello listan ovan i hello-fil, men ta bort `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="9d153-173">Create a file called `_backup.filter` and put hello list above in hello file, but remove `D:\home`.</span></span> <span data-ttu-id="9d153-174">Ange en mapp eller fil per rad.</span><span class="sxs-lookup"><span data-stu-id="9d153-174">List one directory or file per line.</span></span> <span data-ttu-id="9d153-175">Så bör hello innehållet hello filen vara:</span><span class="sxs-lookup"><span data-stu-id="9d153-175">So hello content of hello file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="9d153-176">Överför `_backup.filter` filen toohello `D:\home\site\wwwroot\` för din webbplats med [ftp](web-sites-deploy.md#ftp) eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="9d153-176">Upload `_backup.filter` file toohello `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="9d153-177">Om du vill kan du skapa hello-filen direkt med Kudu `DebugConsole` och infoga det hello-innehåll.</span><span class="sxs-lookup"><span data-stu-id="9d153-177">If you wish, you can create hello file directly using Kudu  `DebugConsole` and insert hello content there.</span></span>

<span data-ttu-id="9d153-178">Köra säkerhetskopieringar hello samma sätt som du skulle göra det, [manuellt](#create-a-manual-backup) eller [automatiskt](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="9d153-178">Run backups hello same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="9d153-179">Nu alla filer och mappar som anges i `_backup.filter` är exkluderad från hello framtida säkerhetskopieringar schemalagd eller manuell start.</span><span class="sxs-lookup"><span data-stu-id="9d153-179">Now, any files and folders that are specified in `_backup.filter` is excluded from hello future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="9d153-180">Du återställer partiella säkerhetskopior av din webbplats hello samma sätt som [återställa en säkerhetskopia av vanliga](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="9d153-180">You restore partial backups of your site hello same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="9d153-181">hello återställningsprocessen hello rätta.</span><span class="sxs-lookup"><span data-stu-id="9d153-181">hello restore process does hello right thing.</span></span>
> 
> <span data-ttu-id="9d153-182">När en fullständig säkerhetskopia återställs ersätts allt innehåll på hello plats med hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="9d153-182">When a full backup is restored, all content on hello site is replaced with whatever is in hello backup.</span></span> <span data-ttu-id="9d153-183">Om en fil på hello plats, men inte i hello säkerhetskopiering tas bort.</span><span class="sxs-lookup"><span data-stu-id="9d153-183">If a file is on hello site, but not in hello backup it gets deleted.</span></span> <span data-ttu-id="9d153-184">Men när en partiell säkerhetskopia återställs allt innehåll som finns i en av hello övriga kataloger eller en svartlistat fil är kvar.</span><span class="sxs-lookup"><span data-stu-id="9d153-184">But when a partial backup is restored, any content that is located in one of hello blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="9d153-185">Hur säkerhetskopior lagras</span><span class="sxs-lookup"><span data-stu-id="9d153-185">How backups are stored</span></span>
<span data-ttu-id="9d153-186">När du har gjort en eller flera säkerhetskopieringar för din app, hello säkerhetskopior är synliga på hello **behållare** bladet för ditt lagringskonto och din app.</span><span class="sxs-lookup"><span data-stu-id="9d153-186">After you have made one or more backups for your app, hello backups are visible on hello **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="9d153-187">I hello lagringskonto varje säkerhetskopiering består av en`.zip` -fil som innehåller hello säkerhetskopierade data och en `.xml` -fil som innehåller ett manifest för hello `.zip` filinnehåll.</span><span class="sxs-lookup"><span data-stu-id="9d153-187">In hello storage account, each backup consists of a`.zip` file that contains hello backup data and an `.xml` file that contains a manifest of hello `.zip` file contents.</span></span> <span data-ttu-id="9d153-188">Du kan packa och bläddra bland dessa filer om du vill tooaccess dina säkerhetskopieringar utan att faktiskt utföra en app-återställning.</span><span class="sxs-lookup"><span data-stu-id="9d153-188">You can unzip and browse these files if you want tooaccess your backups without actually performing an app restore.</span></span>

<span data-ttu-id="9d153-189">hello säkerhetskopiering av databasen för hello app lagras i hello rot the.zip filen.</span><span class="sxs-lookup"><span data-stu-id="9d153-189">hello database backup for hello app is stored in hello root of the.zip file.</span></span> <span data-ttu-id="9d153-190">För en SQL-databas är en BACPAC-fil (utan filtillägget) och kan importeras.</span><span class="sxs-lookup"><span data-stu-id="9d153-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="9d153-191">toocreate en SQL-databas utifrån hello BACPAC export, se [importera en BACPAC filen tooCreate en ny databas](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d153-191">toocreate a SQL database based on hello BACPAC export, see [Import a BACPAC File tooCreate a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="9d153-192">Ändra någon av hello filer i din **websitebackups** behållare kan orsaka hello säkerhetskopiering toobecome ogiltiga och därför icke-återställningsbara.</span><span class="sxs-lookup"><span data-stu-id="9d153-192">Altering any of hello files in your **websitebackups** container can cause hello backup toobecome invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="9d153-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d153-193">Next Steps</span></span>
<span data-ttu-id="9d153-194">Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="9d153-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="9d153-195">Du kan också säkerhetskopiera och återställa Apptjänst-appar med hjälp av REST-API (se [Använd REST toobackup och återställning av Apptjänst-appar](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="9d153-195">You can also backup and restore App Service apps using REST API (see [Use REST toobackup and restore App Service apps](websites-csm-backup.md)).</span></span>


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

