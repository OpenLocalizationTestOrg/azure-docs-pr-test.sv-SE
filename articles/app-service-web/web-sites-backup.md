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
# <a name="back-up-your-app-in-azure"></a>Säkerhetskopiera din app i Azure
Hej säkerhetskopiera och återställa funktion i [Azure App Service](../app-service/app-service-value-prop-what-is.md) kan du enkelt skapa app säkerhetskopiering manuellt eller enligt ett schema. Du kan återställa hello app tooa ögonblicksbild av ett tidigare tillstånd genom överskrivning hello befintliga app eller återställa tooanother app. 

Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Vad säkerhetskopieras
Apptjänst kan säkerhetskopiera hello följande information tooan Azure storage-konto och en behållare som du har konfigurerat din app toouse. 

* Appkonfiguration
* Filinnehåll
* Databasen anslutna tooyour app

hello följande databaslösningar för stöds med funktionen för säkerhetskopiering: 
   - [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
   - [Azure-databas för MySQL (förhandsgranskning)](https://azure.microsoft.com/en-us/services/mysql)
   - [Azure-databas för PostgreSQL (förhandsgranskning)](https://azure.microsoft.com/en-us/services/postgres)
   - [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [MySQL i appen](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Varje säkerhetskopiering är en fullständig offline kopia av din app, inte en inkrementell uppdatering.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Krav och begränsningar
* Hej säkerhetskopiera och återställa funktionen kräver hello App Service-plan toobe i hello **Standard** nivå eller **Premium** nivå. Läs mer om att skala din App Service-plan toouse en högre nivå, [skala upp en app i Azure](web-sites-scale.md).  
  **Premium** nivå kan ett större antal daglig säkerhetskopiering än **Standard** nivå.
* Du behöver ett Azure storage-konto och en behållare i hello samma prenumeration som hello app som du vill toobackup. Mer information om Azure storage-konton finns hello [länkar](#moreaboutstorage) hello slutet av den här artikeln.
* Säkerhetskopieringar kan vara upp too10 GB app- och innehåll. Hej säkerhetskopiestorlek överskrider denna gräns, får du ett fel.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Skapa en manuell säkerhetskopia
1. I hello [Azure Portal](https://portal.azure.com)navigerar tooyour appens blad, Välj **säkerhetskopieringar**. Hej **säkerhetskopieringar** bladet visas.
   
    ![Säkerhetskopieringar sida][ChooseBackupsPage]
   
   > [!NOTE]
   > Om du ser nedan hello-meddelande, klicka på den tooupgrade din programtjänstplan innan du kan fortsätta med säkerhetskopiering.
   > Se [skala upp en app i Azure](web-sites-scale.md) för mer information.  
   > ![Välj lagringskonto](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. I hello **säkerhetskopiering** bladet Klicka **konfigurera**
![Klicka på Konfigurera](./media/web-sites-backup/ClickConfigure1.png)
3. I hello **konfigurering av säkerhetskopiering** bladet, klickar du på **lagring: inte konfigurerat** tooconfigure ett lagringskonto.
   
    ![Välj lagringskonto][ChooseStorageAccount]
4. Välj ditt mål för säkerhetskopian genom att välja en **Lagringskonto** och **behållare**. hello storage-kontot måste höra toohello samma prenumeration som hello-app som du vill att tooback. Om du vill kan skapa du ett nytt lagringskonto eller en ny behållare i respektive hello-blad. När du är klar klickar du på **Välj**.
   
    ![Välj lagringskonto](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. I hello **konfigurering av säkerhetskopiering** bladet som lämnas fortfarande öppen kan du konfigurera **Backup Database**, Välj hello databaser du vill tooinclude hello säkerhetskopior (SQL-databas eller MySQL), och klicka sedan på **OK**.  
   
    ![Välj lagringskonto](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > För en databas tooappear i den här listan sin anslutningssträng måste finnas i hello **anslutningssträngar** avsnitt i hello **programinställningar** bladet för din app.
   > 
   > 
6. I hello **konfigurering av säkerhetskopiering** bladet, klickar du på **spara**.    
7. I hello **säkerhetskopieringar** bladet, klickar du på **säkerhetskopiering**.
   
    ![Knappen BackUpNow][BackUpNow]
   
    Du ser något pågående meddelande under hello säkerhetskopieringsprocessen.

När hello storage-konto och behållaren har konfigurerats kan du initiera en manuell säkerhetskopiering när som helst.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Konfigurera automatisk säkerhetskopiering
1. I hello **Säkerhetskopieringskonfigurationen** bladet ange **schemalagd säkerhetskopiering** för**på**. 
   
    ![Välj lagringskonto](./media/web-sites-backup/05ScheduleBackup1.png)
2. Ange schemat för säkerhetskopiering alternativ visas, **schemalagd säkerhetskopiering** för**på**, konfigurerar schemat för säkerhetskopiering av hello enligt önskemål och klickar på **OK**.
   
    ![Aktivera automatisk säkerhetskopiering][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Konfigurera partiella säkerhetskopieringar
Ibland vill du inte toobackup allt på din app. Några exempel:

* Du [konfigurera veckovisa säkerhetskopior](web-sites-backup.md#configure-automated-backups) för din app med statiskt innehåll som aldrig ändras, till exempel gamla blogginlägg eller bilder.
* Appen har över 10 GB innehåll (som är Hej max du kan säkerhetskopiera i taget).
* Vill du inte toobackup hello loggfiler.

Partiella säkerhetskopior kan du ange exakt vilka filer som du vill toobackup.

### <a name="exclude-files-from-your-backup"></a>Undanta filer från säkerhetskopian
Anta att du har en app som innehåller loggfiler och statiska bilder som har säkerhetskopiera en gång och inte kommer toochange. I sådana fall kan du utesluta dessa mappar och filer lagras i dina framtida säkerhetskopieringar. tooexclude filer och mappar från säkerhetskopiorna, skapa en `_backup.filter` filen i hello `D:\home\site\wwwroot` mappen för din app. Ange hello lista över filer och mappar som du vill tooexclude i den här filen. 

Ett enkelt sätt tooaccess dina filer är toouse Kudu. Klicka på **avancerade verktyg -> Gå** för din web app tooaccess Kudu.

![Kudu med hjälp av portalen][kudu-portal]

Identifiera hello mappar som du vill tooexclude från säkerhetskopiorna.  Till exempel vill du toofilter ut hello markerade mappen och filer.

![Mappen avbildningar][ImagesFolder]

Skapa en fil med namnet `_backup.filter` och placera hello listan ovan i hello-fil, men ta bort `D:\home`. Ange en mapp eller fil per rad. Så bör hello innehållet hello filen vara:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Överför `_backup.filter` filen toohello `D:\home\site\wwwroot\` för din webbplats med [ftp](web-sites-deploy.md#ftp) eller någon annan metod. Om du vill kan du skapa hello-filen direkt med Kudu `DebugConsole` och infoga det hello-innehåll.

Köra säkerhetskopieringar hello samma sätt som du skulle göra det, [manuellt](#create-a-manual-backup) eller [automatiskt](#configure-automated-backups). Nu alla filer och mappar som anges i `_backup.filter` är exkluderad från hello framtida säkerhetskopieringar schemalagd eller manuell start. 

> [!NOTE]
> Du återställer partiella säkerhetskopior av din webbplats hello samma sätt som [återställa en säkerhetskopia av vanliga](web-sites-restore.md). hello återställningsprocessen hello rätta.
> 
> När en fullständig säkerhetskopia återställs ersätts allt innehåll på hello plats med hello säkerhetskopiering. Om en fil på hello plats, men inte i hello säkerhetskopiering tas bort. Men när en partiell säkerhetskopia återställs allt innehåll som finns i en av hello övriga kataloger eller en svartlistat fil är kvar.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Hur säkerhetskopior lagras
När du har gjort en eller flera säkerhetskopieringar för din app, hello säkerhetskopior är synliga på hello **behållare** bladet för ditt lagringskonto och din app. I hello lagringskonto varje säkerhetskopiering består av en`.zip` -fil som innehåller hello säkerhetskopierade data och en `.xml` -fil som innehåller ett manifest för hello `.zip` filinnehåll. Du kan packa och bläddra bland dessa filer om du vill tooaccess dina säkerhetskopieringar utan att faktiskt utföra en app-återställning.

hello säkerhetskopiering av databasen för hello app lagras i hello rot the.zip filen. För en SQL-databas är en BACPAC-fil (utan filtillägget) och kan importeras. toocreate en SQL-databas utifrån hello BACPAC export, se [importera en BACPAC filen tooCreate en ny databas](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Ändra någon av hello filer i din **websitebackups** behållare kan orsaka hello säkerhetskopiering toobecome ogiltiga och därför icke-återställningsbara.
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a>Nästa steg
Information om hur du återställer en app från en säkerhetskopia finns [återställa en app i Azure](web-sites-restore.md). Du kan också säkerhetskopiera och återställa Apptjänst-appar med hjälp av REST-API (se [Använd REST toobackup och återställning av Apptjänst-appar](websites-csm-backup.md)).


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

