---
title: "Använd REST för att säkerhetskopiera och återställa Apptjänst-appar"
description: "Lär dig hur du använder RESTful-API-anrop för att säkerhetskopiera och återställa en app i Azure App Service"
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
ms.openlocfilehash: c1b8fc3be3af46279bf35bddbc82acf1827b9eb9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Använd REST för att säkerhetskopiera och återställa Apptjänst-appar
> [!div class="op_single_selector"]
> * [PowerShell](../app-service/app-service-powershell-backup.md)
> * [REST-API](websites-csm-backup.md)
> 
> 

[Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/) kan säkerhetskopieras som blobar i Azure-lagring. Säkerhetskopieringen kan också innehålla appens databaser. Om appen någonsin av misstag tas bort, eller om appen behöver återställas till en tidigare version, kan du återställa den från en tidigare säkerhetskopia. Säkerhetskopieringar kan göras när som helst på begäran eller säkerhetskopieringar kan schemaläggas med lämpliga intervall.

Den här artikeln beskriver hur du säkerhetskopierar och återställer en app med RESTful-API-begäranden. Om du vill skapa och hantera appen säkerhetskopior grafiskt via Azure portal, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)

<a name="gettingstarted"></a>

## <a name="getting-started"></a>Komma igång
Om du vill skicka REST-begäranden som du behöver veta din app **namn**, **resursgruppen**, och **prenumerations-id**. Den här informationen kan hittas genom att klicka på din app i den **Apptjänst** bladet för den [Azure-portalen](https://portal.azure.com). Vi konfigurerar webbplatsen för exemplen i den här artikeln **backuprestoreapiexamples.azurewebsites.net**. Den lagras i standard-webb-WestUS resursgruppen och körs på en prenumeration med det ID 00001111-2222-3333-4444-555566667777.

![Exempel webbplatsinformation][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a>Säkerhetskopiering och återställning REST API
Nu tar vi upp flera exempel på hur du använder REST API för att säkerhetskopiera och återställa en app. Varje exempel innehåller en URL och HTTP brödtext i begäran. Exempel-URL: en innehåller platshållare kapslas in i klammerparenteser, till exempel {prenumerations-id}. Ersätt platshållarna med motsvarande information för din app. Här är en förklaring av varje platshållare som visas i exempel-URL: er för referens.

* prenumerations-id – ID för Azure-prenumeration som innehåller appen
* resursgruppens--namn – namnet på resursgruppen som innehåller appen
* namn – namnet på appen
* Backup-id – ID för app-säkerhetskopiering

Komplett dokumentation av API, inklusive flera valfria parametrar som kan tas med i HTTP-begäran finns i [resursutforskaren Azure](https://resources.azure.com/).

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a>Säkerhetskopiera en app på begäran
Om du vill säkerhetskopiera en app direkt, skicka ett **POST** begäran om att **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopiering /**.

Här är hur URL: en ser ut med våra exempel webbplats. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Ange en JSON-objekt i brödtexten för din begäran om att ange vilken lagringskontotyp du använder för att lagra säkerhetskopian. JSON-objekt måste ha en egenskap med namnet **storageAccountUrl**, som har en [SAS-URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) bevilja åtkomst till Azure Storage-behållare som innehåller den säkerhetskopiera blobben. Om du vill säkerhetskopiera databaser, måste du även ange en lista med namn, typer och anslutningssträngar för databaser som ska säkerhetskopieras.

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

En säkerhetskopia av appen börjar omedelbart när begäran tas emot. Säkerhetskopieringen kan ta lång tid att slutföra. HTTP-svaret innehåller ett ID som du kan använda i en annan begäran om att se status för säkerhetskopieringen. Här är ett exempel på innehållet i HTTP-svar på vår säkerhetskopiering begäran.

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
> Felmeddelanden kan hittas i egenskapen av HTTP-svar.
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a>Schemalägga automatisk säkerhetskopiering
Förutom att säkerhetskopiera en app på begäran, kan du också schemalägga en säkerhetskopiering sker automatiskt.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Ställ in ett nytt schema för automatisk säkerhetskopiering
Om du vill konfigurera ett schema för säkerhetskopiering, skicka ett **PLACERA** begäran om att **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/ säkerhetskopiering**.

Det här är URL-Adressen ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Begärandetexten måste ha ett JSON-objekt som anger konfigurationen för säkerhetskopiering. Här är ett exempel med de obligatoriska parametrarna.

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
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Det här exemplet konfigurerar appen säkerhetskopieras automatiskt var sjunde dag. Parametrarna **frequencyInterval** och **frequencyUnit** tillsammans bestämmer hur ofta säkerhetskopieringarna ska inträffa. Giltiga värden för **frequencyUnit** är **timme** och **dag**. Exempelvis om du vill säkerhetskopiera en app var 12: e timme, ställer du in frequencyInterval-12 och frequencyUnit timme.

Gamla säkerhetskopior tas bort automatiskt från lagringskontot. Du kan styra hur gammal säkerhetskopieringar kan vara genom att ange den **retentionPeriodInDays** parameter. Om du vill att alltid ska ha minst en säkerhetskopia som sparats, oavsett hur gamla, anges **keepAtLeastOneBackup** till true.

### <a name="get-the-automatic-backup-schedule"></a>Hämta schemat för automatisk säkerhetskopiering
För att hämta konfigurationen för säkerhetskopiering av en app, skicka ett **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} / config/säkerhetskopiera/listan**.

URL: en för webbplatsen exempel är **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>

## <a name="get-the-status-of-a-backup"></a>Hämta status för en säkerhetskopia
Beroende på hur stor appen är kan en säkerhetskopia ta en stund att slutföra. Säkerhetskopieringar kan också misslyckas timeout, eller delvis lyckas. Om du vill se status för alla appens säkerhetskopieringar skicka en **hämta** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} / säkerhetskopieringar**.

Om du vill se status för en specifik säkerhetskopia skickar en GET-begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}** .

Det här är URL-Adressen ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

Svarstexten innehåller ett JSON-objekt som liknar det här exemplet.

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

Status för en säkerhetskopia är en uppräknad typ. Här är alla möjliga tillstånd.

* 0 – Pågår: Säkerhetskopieringen har startats men ännu inte har slutförts.
* 1 – Misslyckades: Säkerhetskopieringen misslyckades.
* 2 – Lyckades: Säkerhetskopieringen har slutförts.
* 3 – för lång tid: Säkerhetskopieringen slutfördes inte i tid och avbröts.
* 4 – Skapad: Säkerhetskopieringsbegäran köas men har inte startats.
* 5 – Hoppades över: Säkerhetskopieringen har inte utförts på grund av ett schema som utlöst för många säkerhetskopieringar.
* 6 – Lyckades delvis: Säkerhetskopieringen slutfördes, men vissa filer säkerhetskopierades inte eftersom de inte kunde läsas. Detta inträffar vanligen eftersom filerna låsts exklusivt.
* 7 – Borttagning sker: Säkerhetskopieringen har begärts att tas bort, men har inte tagits bort ännu.
* 8 – Borttagning misslyckades: Det gick inte att ta bort säkerhetskopian. Detta kan inträffa om SAS-webbadressen som användes för att skapa säkerhetskopian har upphört att gälla.
* 9 – Borttagen: Säkerhetskopian har tagits bort.

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a>Återställa en app från en säkerhetskopia
Om din app har tagits bort, eller om du vill återställa din app till en tidigare version, kan du återställa appen från en säkerhetskopia. Om du vill starta en återställning, skicka ett **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups / {säkerhetskopiering id} / återställa**.

Det här är URL-Adressen ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**

Skicka ett JSON-objekt som innehåller egenskaper för återställningen i frågans brödtext. Här är ett exempel som innehåller alla nödvändiga egenskaper:

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
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Återställ till en ny app
Ibland kanske du vill skapa en ny app när du återställer en säkerhetskopia, i stället för att skriva över en befintlig app. Gör detta genom att ändra URL: en så att den pekar till den nya appen som du vill skapa och ändra den **över** egenskap i JSON till **FALSKT**.

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a>Tar bort en säkerhetskopiering av appen
Om du vill ta bort en säkerhetskopia kan skicka en **ta bort** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} /backups/ {säkerhetskopiering id}**.

Det här är URL-Adressen ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a>Hantera en säkerhetskopia SAS-URL
Azure Apptjänst försöker ta bort säkerhetskopiorna från Azure Storage med hjälp av SAS-URL som angavs när säkerhetskopian skapades. Om den här SAS-URL är inte längre giltig, kan inte säkerhetskopieringen tas bort via REST API. Men du kan uppdatera SAS-URL som är associerad med en säkerhetskopia genom att skicka en **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Det här är URL-Adressen ser ut för webbplatsen exempel. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**

Skicka ett JSON-objekt som innehåller den nya SAS-URL i frågans brödtext. Här är ett exempel.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> Av säkerhetsskäl returneras inte SAS-URL som är associerad med en säkerhetskopia när du skickar en GET-begäran för en specifik säkerhetskopiering. Skicka en POST-begäran till samma URL ovan om du vill visa SAS-URL som är associerad med en säkerhetskopia. Innehåller ett tomt JSON-objekt i begärandetexten. Svaret från servern innehåller all information om att säkerhetskopiering, inklusive dess SAS-URL.
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
