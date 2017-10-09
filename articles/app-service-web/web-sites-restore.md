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
# <a name="restore-an-app-in-azure"></a>Återställ en app i Azure
Den här artikeln beskrivs hur du toorestore en app i [Azure App Service](../app-service/app-service-value-prop-what-is.md) som du tidigare har säkerhetskopierat (se [säkerhetskopiera din app i Azure](web-sites-backup.md)). Du kan återställa din app med dess tidigare tillstånd på begäran tooa för länkade databaser eller skapa en ny app baserat på en av din ursprungliga app säkerhetskopiering. Azure Apptjänst stöder hello följande databaser för säkerhetskopiering och återställning:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure-databas för MySQL (förhandsgranskning)](https://azure.microsoft.com/en-us/services/mysql)
- [Azure-databas för PostgreSQL (förhandsgranskning)](https://azure.microsoft.com/en-us/services/postgres)
- [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL i appen](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Återställning från säkerhetskopior är tillgängliga tooapps som körs i **Standard** och **Premium** nivå. Information om skala upp appar finns [skala upp en app i Azure](web-sites-scale.md). **Premium** nivå kan ett större antal dagliga säkerhetskopior toobe utförts än **Standard** nivå.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Återställa en app från en befintlig säkerhetskopia
1. På hello **inställningar** bladet för din app i hello Azure-portalen klickar du på **säkerhetskopieringar** toodisplay hello **säkerhetskopieringar** bladet. Klicka på **återställa**.
   
    ![Välj Återställ nu][ChooseRestoreNow]
2. I hello **återställa** bladet, första väljer hello-säkerhetskopia.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    Hej **App säkerhetskopiering** alternativet visas alla hello befintliga säkerhetskopior av hello aktuell app och kan du enkelt välja en.
    Hej **lagring** alternativet kan du välja en säkerhetskopiering ZIP-fil från en befintlig Azure Storage-konto och en behållare i din prenumeration.
    Om du försöker toorestore en säkerhetskopia av en annan app kan du använda hello **lagring** alternativet.
3. Ange sedan hello mål för återställning av hello-app i **Återställningsmål**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Om du väljer **Skriv över**, alla befintliga data i din aktuella appen bort och skrivas över. Innan du klickar på **OK**, se till att den är exakt vad du vill toodo.
   > 
   > 
   
    Du kan välja **befintlig App** toorestore hello app säkerhetskopiering tooanother app i hello samma resoure grupp. Innan du använder det här alternativet måste bör du redan har skapat en annan app i resursgruppen med spegling databasen configuration toohello som definieras i hello app säkerhetskopiering. Du kan också skapa en **ny** app toorestore ditt innehåll till.

4. Klicka på **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Hämta eller ta bort en säkerhetskopia från ett lagringskonto
1. Från hello huvudsakliga **Bläddra** bladet för hello Azure portal, Välj **lagringskonton**. En lista över dina befintliga lagringskonton visas.
2. Välj hello storage-konto som innehåller hello-säkerhetskopia som du vill toodownload eller delete.hello bladet för hello storage-konto visas.
3. Välj hello-behållare som du vill i hello lagring-kontoblad
   
    ![Visa behållare][ViewContainers]
4. Markera den säkerhetskopierade filen du vill toodownload eller ta bort.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klicka på **hämta** eller **ta bort** beroende på vad du vill att toodo.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Övervaka en återställning
toosee information om hello lyckad eller misslyckad hello appar återställningsåtgärd navigera toohello **aktivitetsloggen** bladet i hello Azure-portalen.  
 

Rulla ned toofind hello önskad restore igen och klicka på tooselect den.

hello information bladet visar hello tillgänglig information som rör toohello återställningen igen.

## <a name="next-steps"></a>Nästa steg
Du kan säkerhetskopiera och återställa Apptjänst-appar med hjälp av REST-API (se [Använd REST tooback upp och återställa Apptjänst-appar](websites-csm-backup.md)).


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
