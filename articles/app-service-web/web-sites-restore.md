---
title: aaaRestore en app i Azure
description: "Lär dig hur toorestore appen från en säkerhetskopia."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b54029a9197064f990f29a3c4558c8322668714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-app-in-azure"></a><span data-ttu-id="4974c-103">Återställ en app i Azure</span><span class="sxs-lookup"><span data-stu-id="4974c-103">Restore an app in Azure</span></span>
<span data-ttu-id="4974c-104">Den här artikeln beskrivs hur du toorestore en app i [Azure App Service](../app-service/app-service-value-prop-what-is.md) som du tidigare har säkerhetskopierat (se [säkerhetskopiera din app i Azure](web-sites-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="4974c-104">This article shows you how toorestore an app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) that you have previously backed up (see [Back up your app in Azure](web-sites-backup.md)).</span></span> <span data-ttu-id="4974c-105">Du kan återställa din app med dess tidigare tillstånd på begäran tooa för länkade databaser eller skapa en ny app baserat på en av din ursprungliga app säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4974c-105">You can restore your app with its linked databases on-demand tooa previous state, or create a new app based on one of your original app's backup.</span></span> <span data-ttu-id="4974c-106">Azure Apptjänst stöder hello följande databaser för säkerhetskopiering och återställning:</span><span class="sxs-lookup"><span data-stu-id="4974c-106">Azure App Service supports hello following databases for backup and restore:</span></span>
- [<span data-ttu-id="4974c-107">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4974c-107">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
- [<span data-ttu-id="4974c-108">Azure-databas för MySQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="4974c-108">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
- [<span data-ttu-id="4974c-109">Azure-databas för PostgreSQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="4974c-109">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
- [<span data-ttu-id="4974c-110">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="4974c-110">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [<span data-ttu-id="4974c-111">MySQL i appen</span><span class="sxs-lookup"><span data-stu-id="4974c-111">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

<span data-ttu-id="4974c-112">Återställning från säkerhetskopior är tillgängliga tooapps som körs i **Standard** och **Premium** nivå.</span><span class="sxs-lookup"><span data-stu-id="4974c-112">Restoring from backups is available tooapps running in **Standard** and **Premium** tier.</span></span> <span data-ttu-id="4974c-113">Information om skala upp appar finns [skala upp en app i Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="4974c-113">For information about scaling up your app, see [Scale up an app in Azure](web-sites-scale.md).</span></span> <span data-ttu-id="4974c-114">**Premium** nivå kan ett större antal dagliga säkerhetskopior toobe utförts än **Standard** nivå.</span><span class="sxs-lookup"><span data-stu-id="4974c-114">**Premium** tier allows a greater number of daily backups toobe performed than **Standard** tier.</span></span>

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a><span data-ttu-id="4974c-115">Återställa en app från en befintlig säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="4974c-115">Restore an app from an existing backup</span></span>
1. <span data-ttu-id="4974c-116">På hello **inställningar** bladet för din app i hello Azure-portalen klickar du på **säkerhetskopieringar** toodisplay hello **säkerhetskopieringar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4974c-116">On hello **Settings** blade of your app in hello Azure Portal, click **Backups** toodisplay hello **Backups** blade.</span></span> <span data-ttu-id="4974c-117">Klicka på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="4974c-117">Then click **Restore**.</span></span>
   
    ![Välj Återställ nu][ChooseRestoreNow]
2. <span data-ttu-id="4974c-119">I hello **återställa** bladet, första väljer hello-säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="4974c-119">In hello **Restore** blade, first select hello backup source.</span></span>
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    <span data-ttu-id="4974c-120">Hej **App säkerhetskopiering** alternativet visas alla hello befintliga säkerhetskopior av hello aktuell app och kan du enkelt välja en.</span><span class="sxs-lookup"><span data-stu-id="4974c-120">hello **App backup** option shows you all hello existing backups of hello current app, and you can easily select one.</span></span>
    <span data-ttu-id="4974c-121">Hej **lagring** alternativet kan du välja en säkerhetskopiering ZIP-fil från en befintlig Azure Storage-konto och en behållare i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4974c-121">hello **Storage** option lets you select any backup ZIP file from any existing Azure Storage account and container in your subscription.</span></span>
    <span data-ttu-id="4974c-122">Om du försöker toorestore en säkerhetskopia av en annan app kan du använda hello **lagring** alternativet.</span><span class="sxs-lookup"><span data-stu-id="4974c-122">If you're trying toorestore a backup of another app, use hello **Storage** option.</span></span>
3. <span data-ttu-id="4974c-123">Ange sedan hello mål för återställning av hello-app i **Återställningsmål**.</span><span class="sxs-lookup"><span data-stu-id="4974c-123">Then, specify hello destination for hello app restore in **Restore destination**.</span></span>
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > <span data-ttu-id="4974c-124">Om du väljer **Skriv över**, alla befintliga data i din aktuella appen bort och skrivas över.</span><span class="sxs-lookup"><span data-stu-id="4974c-124">If you choose **Overwrite**, all existing data in your current app is erased and overwritten.</span></span> <span data-ttu-id="4974c-125">Innan du klickar på **OK**, se till att den är exakt vad du vill toodo.</span><span class="sxs-lookup"><span data-stu-id="4974c-125">Before you click **OK**, make sure that it is exactly what you want toodo.</span></span>
   > 
   > 
   
    <span data-ttu-id="4974c-126">Du kan välja **befintlig App** toorestore hello app säkerhetskopiering tooanother app i hello samma resoure grupp.</span><span class="sxs-lookup"><span data-stu-id="4974c-126">You can select **Existing App** toorestore hello app backup tooanother app in hello same resoure group.</span></span> <span data-ttu-id="4974c-127">Innan du använder det här alternativet måste bör du redan har skapat en annan app i resursgruppen med spegling databasen configuration toohello som definieras i hello app säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4974c-127">Before you use this option, you should have already created another app in your resource group with mirroring database configuration toohello one defined in hello app backup.</span></span> <span data-ttu-id="4974c-128">Du kan också skapa en **ny** app toorestore ditt innehåll till.</span><span class="sxs-lookup"><span data-stu-id="4974c-128">You can also Create a **New** app toorestore your content to.</span></span>

4. <span data-ttu-id="4974c-129">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4974c-129">Click **OK**.</span></span>

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a><span data-ttu-id="4974c-130">Hämta eller ta bort en säkerhetskopia från ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="4974c-130">Download or delete a backup from a storage account</span></span>
1. <span data-ttu-id="4974c-131">Från hello huvudsakliga **Bläddra** bladet för hello Azure portal, Välj **lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="4974c-131">From hello main **Browse** blade of hello Azure portal, select **Storage accounts**.</span></span> <span data-ttu-id="4974c-132">En lista över dina befintliga lagringskonton visas.</span><span class="sxs-lookup"><span data-stu-id="4974c-132">A list of your existing storage accounts is displayed.</span></span>
2. <span data-ttu-id="4974c-133">Välj hello storage-konto som innehåller hello-säkerhetskopia som du vill toodownload eller delete.hello bladet för hello storage-konto visas.</span><span class="sxs-lookup"><span data-stu-id="4974c-133">Select hello storage account that contains hello backup that you want toodownload or delete.hello blade for hello storage account is displayed.</span></span>
3. <span data-ttu-id="4974c-134">Välj hello-behållare som du vill i hello lagring-kontoblad</span><span class="sxs-lookup"><span data-stu-id="4974c-134">In hello storage account blade, select hello container you want</span></span>
   
    ![Visa behållare][ViewContainers]
4. <span data-ttu-id="4974c-136">Markera den säkerhetskopierade filen du vill toodownload eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="4974c-136">Select backup file you want toodownload or delete.</span></span>
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. <span data-ttu-id="4974c-138">Klicka på **hämta** eller **ta bort** beroende på vad du vill att toodo.</span><span class="sxs-lookup"><span data-stu-id="4974c-138">Click **Download** or **Delete** depending on what you want toodo.</span></span>  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a><span data-ttu-id="4974c-139">Övervaka en återställning</span><span class="sxs-lookup"><span data-stu-id="4974c-139">Monitor a restore operation</span></span>
<span data-ttu-id="4974c-140">toosee information om hello lyckad eller misslyckad hello appar återställningsåtgärd navigera toohello **aktivitetsloggen** bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4974c-140">toosee details about hello success or failure of hello app restore operation, navigate toohello **Activity Log** blade in hello Azure portal.</span></span>  
 

<span data-ttu-id="4974c-141">Rulla ned toofind hello önskad restore igen och klicka på tooselect den.</span><span class="sxs-lookup"><span data-stu-id="4974c-141">Scroll down toofind hello desired restore operation and click tooselect it.</span></span>

<span data-ttu-id="4974c-142">hello information bladet visar hello tillgänglig information som rör toohello återställningen igen.</span><span class="sxs-lookup"><span data-stu-id="4974c-142">hello details blade displays hello available information related toohello restore operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4974c-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4974c-143">Next Steps</span></span>
<span data-ttu-id="4974c-144">Du kan säkerhetskopiera och återställa Apptjänst-appar med hjälp av REST-API (se [Använd REST tooback upp och återställa Apptjänst-appar](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="4974c-144">You can backup and restore App Service apps using REST API (see [Use REST tooback up and restore App Service apps](websites-csm-backup.md)).</span></span>


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
