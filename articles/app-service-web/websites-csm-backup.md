---
title: "aaaUse REST tooback upp och återställa Apptjänst-appar"
description: "Lär dig hur toouse RESTful-API-anrop tooback och återställa en app i Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: f94d0aea-edc1-43ab-9b51-ea7375859276
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: f77bdfc7de1626d04d8d0c0e8979231ae1ced81a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a>Använd REST tooback och återställa Apptjänst-appar
> [!div class="op_single_selector"]
> * [PowerShell](../app-service/app-service-powershell-backup.md)
> * [REST-API](websites-csm-backup.md)
> 
> 

[Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/) kan säkerhetskopieras som blobar i Azure-lagring. hello säkerhetskopiering kan också innehålla hello app databaser. Om hello appen någonsin av misstag tas bort, eller om hello app måste toobe återställs tooa tidigare version, den kan återställas från en tidigare säkerhetskopia. Säkerhetskopieringar kan göras när som helst på begäran eller säkerhetskopieringar kan schemaläggas med lämpliga intervall.

Den här artikeln förklarar hur toobackup och återställning av en app med RESTful-API-begäranden. Om du skulle t.ex. toocreate och hantera appen säkerhetskopior grafiskt via hello Azure-portalen, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)

<a name="gettingstarted"></a>

## <a name="getting-started"></a>Komma igång
toosend REST begär, behöver du tooknow appens **namn**, **resursgruppen**, och **prenumerations-id**. Den här informationen kan hittas genom att klicka på din app i hello **Apptjänst** bladet för hello [Azure-portalen](https://portal.azure.com). Vi konfigurerar hello webbplats för hello exemplen i den här artikeln, **backuprestoreapiexamples.azurewebsites.net**. Den lagras i hello standard-webb-WestUS resursgruppen och körs på en prenumeration med hello ID 00001111-2222-3333-4444-555566667777.

![Exempel webbplatsinformation][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a>Säkerhetskopiering och återställning REST API
Nu tar vi upp flera exempel på hur toouse hello REST API toobackup och återställa en app. Varje exempel innehåller en URL och HTTP brödtext i begäran. hello innehåller exempel platshållare kapslas in i klammerparenteser, till exempel {prenumerations-id}. Ersätt platshållarna hello med hello motsvarande information för din app. Här är en förklaring av varje platshållare som visas i hello exempel webbadresser referens.

* prenumerations-id – ID för hello Azure-prenumeration som innehåller hello app
* resursgruppens--namn – namnet på hello resurs grupp som innehåller hello app
* namn – namnet på hello app
* Backup-id – ID för hello app säkerhetskopiering

Hello komplett dokumentation av hello API, inklusive flera valfria parametrar som kan ingå i hello HTTP-begäran finns hello [resursutforskaren Azure](https://resources.azure.com/).

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a>Säkerhetskopiera en app på begäran
tooback in en app direkt, skicka ett **POST** begäran för**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopiering /**.

Det här är vad hello URL ser ut med våra exempel webbplats. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Ange en JSON-objekt i din begäran toospecify vilka storage-konto toouse toostore hello säkerhetskopiering hello brödtext. hello JSON-objekt måste ha en egenskap med namnet **storageAccountUrl**, som har en [SAS-URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) bevilja skrivbehörighet toohello Azure Storage-behållare som innehåller hello säkerhetskopiering blob. Om du vill tooback dina databaser, måste du även ange en lista som innehåller hello namn, typer och anslutningssträngar för hello databaser toobe säkerhetskopieras.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

En säkerhetskopia av hello app börjar omedelbart när hello begäran tas emot. hello säkerhetskopieringen kan ta en lång tid toocomplete. hello HTTP-svaret innehåller ett ID som du kan använda i en annan begäran toosee hello status för hello säkerhetskopiering. Här är ett exempel på hello brödtext hello HTTP-svar tooour säkerhetskopiering begäran.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

> [!NOTE]
> Felmeddelanden kan hittas i hello log-egenskapen för hello HTTP-svar.
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a>Schemalägga automatisk säkerhetskopiering
I toobacking lägga in en app på begäran, kan du också schemalägga säkerhetskopiering toohappen automatiskt.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Ställ in ett nytt schema för automatisk säkerhetskopiering
tooset upp ett schema för säkerhetskopiering, skicka ett **PLACERA** begäran för**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config / backup**.

Här är hello URL ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

hello frågans brödtext måste ha ett JSON-objekt som anger hello konfigurationen för säkerhetskopiering. Här är ett exempel med alla parametrar för hello krävs.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set tootrue tooenable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Det här exemplet konfigureras hello app toobe säkerhetskopieras automatiskt var sjunde dag. Hej parametrar **frequencyInterval** och **frequencyUnit** tillsammans bestämmer hur ofta hello säkerhetskopieringar göras. Giltiga värden för **frequencyUnit** är **timme** och **dag**. Ange frequencyInterval too12 och frequencyUnit toohour exempelvis tooback upp en app var 12: e timme.

Gamla säkerhetskopior tas automatiskt bort från hello storage-konto. Du kan styra hur gammal hello säkerhetskopieringar kan vara genom att ange hello **retentionPeriodInDays** parameter. Om du vill tooalways har minst en säkerhetskopia som sparats, oavsett hur gamla, anges **keepAtLeastOneBackup** tootrue.

### <a name="get-hello-automatic-backup-schedule"></a>Hämta hello schema för automatisk säkerhetskopiering
tooget en app säkerhetskopiera configuration skickar en **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} / config/backup/list**.

hello URL: en för webbplatsen exempel är **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a>Hämta hello status för en säkerhetskopia
Beroende på hur stor hello app är en säkerhetskopia kan ta en stund toocomplete. Säkerhetskopieringar kan också misslyckas timeout, eller delvis lyckas. toosee hello status för alla appens säkerhetskopior, skicka ett **hämta** toohello URL-begäran **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} /providers/ Microsoft.Web/sites/{name}/backups**.

toosee hello status för en specifik säkerhetskopiering, skickar en GET-begäran toohello URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { Backup-id}**.

Här är hello URL ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

hello svarstexten innehåller en liknande toothis exempel för en JSON-objekt.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

hello status för en säkerhetskopia är en uppräknad typ. Här är alla möjliga tillstånd.

* 0 – InProgress: hello säkerhetskopiering har startats men har inte slutförts ännu.
* 1 – misslyckades: hello säkerhetskopiering misslyckades.
* 2 – lyckades: hello säkerhetskopieringen har slutförts.
* 3 – för lång tid: hello säkerhetskopieringen slutfördes inte i tid och avbröts.
* 4 – skapa: hello säkerhetskopiering begäran är i kö men har inte startats.
* 5 – hoppades över: hello säkerhetskopiering inte fortsätta på grund av tooa schema som utlöser för många säkerhetskopior.
* 6 – PartiallySucceeded: hello säkerhetskopiering lyckades, men vissa filer säkerhetskopierats inte eftersom de inte kan läsas. Detta inträffar vanligen eftersom ett exklusivt lås placerades på hello-filer.
* 7 – DeleteInProgress: hello säkerhetskopiering har begärt toobe bort, men har inte tagits bort.
* 8 – DeleteFailed: hello säkerhetskopiering kunde inte tas bort. Detta kan inträffa eftersom hello SAS-URL som har använt toocreate hello backup har gått ut.
* 9 – tas bort: hello säkerhetskopiering har tagits bort.

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a>Återställa en app från en säkerhetskopia
Om din app har tagits bort, eller om du vill toorevert din app tooa tidigare version, kan du återställa hello app från en säkerhetskopia. tooinvoke en återställning, skicka ett **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopieringar / {säkerhetskopiering id} / återställa**.

Här är hello URL ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**

Skicka ett JSON-objekt som innehåller hello egenskaper för hello återställningen i hello begärandetexten. Här är ett exempel som innehåller alla nödvändiga egenskaper:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL toostorage container containing your website backup
    }
}
```

### <a name="restore-tooa-new-app"></a>Återställa tooa ny app
Ibland kanske du vill toocreate en ny app när du återställer en säkerhetskopia, i stället för att skriva över en befintlig app. toodo, ändra hello begäran URL toopoint toohello nya appen toocreate, och ändra hello **över** egenskap i hello JSON för**FALSKT**.

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a>Tar bort en säkerhetskopiering av appen
Om du vill toodelete en säkerhetskopia kan skicka en **ta bort** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} /backups/ {säkerhetskopiering id}**.

Här är hello URL ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a>Hantera en säkerhetskopia SAS-URL
Azure Apptjänst försöker toodelete säkerhetskopiorna från Azure Storage med hjälp av hello SAS-URL som angavs när hello säkerhetskopian skapades. Om den här SAS-URL är inte längre giltig, kan inte hello säkerhetskopieringen tas bort via hello REST API. Men du kan uppdatera hello SAS-URL som är associerade med en säkerhetskopia genom att skicka en **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Här är hello URL ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**

Skicka ett JSON-objekt som innehåller hello nya SAS-URL i hello begärandetexten. Här är ett exempel.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> Av säkerhetsskäl returneras inte hello SAS-URL som är associerad med en säkerhetskopia när du skickar en GET-begäran för en specifik säkerhetskopiering. Om du vill tooview hello SAS-URL som är associerad med en säkerhetskopia kan skicka en POST-begäran toohello samma URL ovan. Inkludera ett tomt JSON-objekt i hello frågans brödtext. hello svar från hello server innehåller alla som säkerhetskopierar information, inklusive dess SAS-URL.
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
