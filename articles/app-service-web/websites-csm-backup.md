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
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="04128-103">Använd REST tooback och återställa Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="04128-103">Use REST tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="04128-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="04128-104">PowerShell</span></span>](../app-service/app-service-powershell-backup.md)
> * [<span data-ttu-id="04128-105">REST-API</span><span class="sxs-lookup"><span data-stu-id="04128-105">REST API</span></span>](websites-csm-backup.md)
> 
> 

<span data-ttu-id="04128-106">[Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/) kan säkerhetskopieras som blobar i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="04128-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) can be backed up as blobs in Azure storage.</span></span> <span data-ttu-id="04128-107">hello säkerhetskopiering kan också innehålla hello app databaser.</span><span class="sxs-lookup"><span data-stu-id="04128-107">hello backup can also contain hello app’s databases.</span></span> <span data-ttu-id="04128-108">Om hello appen någonsin av misstag tas bort, eller om hello app måste toobe återställs tooa tidigare version, den kan återställas från en tidigare säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="04128-108">If hello app is ever accidentally deleted, or if hello app needs toobe reverted tooa previous version, it can be restored from any previous backup.</span></span> <span data-ttu-id="04128-109">Säkerhetskopieringar kan göras när som helst på begäran eller säkerhetskopieringar kan schemaläggas med lämpliga intervall.</span><span class="sxs-lookup"><span data-stu-id="04128-109">Backups can be done at any time on demand, or backups can be scheduled at suitable intervals.</span></span>

<span data-ttu-id="04128-110">Den här artikeln förklarar hur toobackup och återställning av en app med RESTful-API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="04128-110">This article explains how toobackup and restore an app with RESTful API requests.</span></span> <span data-ttu-id="04128-111">Om du skulle t.ex. toocreate och hantera appen säkerhetskopior grafiskt via hello Azure-portalen, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)</span><span class="sxs-lookup"><span data-stu-id="04128-111">If you would like toocreate and manage app backups graphically through hello Azure portal, see [Back up a web app in Azure App Service](web-sites-backup.md)</span></span>

<a name="gettingstarted"></a>

## <a name="getting-started"></a><span data-ttu-id="04128-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="04128-112">Getting Started</span></span>
<span data-ttu-id="04128-113">toosend REST begär, behöver du tooknow appens **namn**, **resursgruppen**, och **prenumerations-id**. Den här informationen kan hittas genom att klicka på din app i hello **Apptjänst** bladet för hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="04128-113">toosend REST requests, you need tooknow your app’s **name**, **resource group**, and **subscription id**. This information can be found by clicking your app in hello **App Service** blade of hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="04128-114">Vi konfigurerar hello webbplats för hello exemplen i den här artikeln, **backuprestoreapiexamples.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="04128-114">For hello examples in this article, we are configuring hello website **backuprestoreapiexamples.azurewebsites.net**.</span></span> <span data-ttu-id="04128-115">Den lagras i hello standard-webb-WestUS resursgruppen och körs på en prenumeration med hello ID 00001111-2222-3333-4444-555566667777.</span><span class="sxs-lookup"><span data-stu-id="04128-115">It is stored in hello Default-Web-WestUS resource group and is running on a subscription with hello ID 00001111-2222-3333-4444-555566667777.</span></span>

![Exempel webbplatsinformation][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a><span data-ttu-id="04128-117">Säkerhetskopiering och återställning REST API</span><span class="sxs-lookup"><span data-stu-id="04128-117">Backup and restore REST API</span></span>
<span data-ttu-id="04128-118">Nu tar vi upp flera exempel på hur toouse hello REST API toobackup och återställa en app.</span><span class="sxs-lookup"><span data-stu-id="04128-118">We will now cover several examples of how toouse hello REST API toobackup and restore an app.</span></span> <span data-ttu-id="04128-119">Varje exempel innehåller en URL och HTTP brödtext i begäran.</span><span class="sxs-lookup"><span data-stu-id="04128-119">Each example includes a URL and HTTP request body.</span></span> <span data-ttu-id="04128-120">hello innehåller exempel platshållare kapslas in i klammerparenteser, till exempel {prenumerations-id}.</span><span class="sxs-lookup"><span data-stu-id="04128-120">hello sample URL contains placeholders wrapped in curly braces, such as {subscription-id}.</span></span> <span data-ttu-id="04128-121">Ersätt platshållarna hello med hello motsvarande information för din app.</span><span class="sxs-lookup"><span data-stu-id="04128-121">Replace hello placeholders with hello corresponding information for your app.</span></span> <span data-ttu-id="04128-122">Här är en förklaring av varje platshållare som visas i hello exempel webbadresser referens.</span><span class="sxs-lookup"><span data-stu-id="04128-122">For reference, here is an explanation of each placeholder that appears in hello example URLs.</span></span>

* <span data-ttu-id="04128-123">prenumerations-id – ID för hello Azure-prenumeration som innehåller hello app</span><span class="sxs-lookup"><span data-stu-id="04128-123">subscription-id – ID of hello Azure subscription containing hello app</span></span>
* <span data-ttu-id="04128-124">resursgruppens--namn – namnet på hello resurs grupp som innehåller hello app</span><span class="sxs-lookup"><span data-stu-id="04128-124">resource-group-name – Name of hello resource group containing hello app</span></span>
* <span data-ttu-id="04128-125">namn – namnet på hello app</span><span class="sxs-lookup"><span data-stu-id="04128-125">name – Name of hello app</span></span>
* <span data-ttu-id="04128-126">Backup-id – ID för hello app säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="04128-126">backup-id – ID of hello app backup</span></span>

<span data-ttu-id="04128-127">Hello komplett dokumentation av hello API, inklusive flera valfria parametrar som kan ingå i hello HTTP-begäran finns hello [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="04128-127">For hello complete documentation of hello API, including several optional parameters that can be included in hello HTTP request, see hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a><span data-ttu-id="04128-128">Säkerhetskopiera en app på begäran</span><span class="sxs-lookup"><span data-stu-id="04128-128">Backup an app on demand</span></span>
<span data-ttu-id="04128-129">tooback in en app direkt, skicka ett **POST** begäran för**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopiering /**.</span><span class="sxs-lookup"><span data-stu-id="04128-129">tooback up an app immediately, send a **POST** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.</span></span>

<span data-ttu-id="04128-130">Det här är vad hello URL ser ut med våra exempel webbplats.</span><span class="sxs-lookup"><span data-stu-id="04128-130">Here is what hello URL looks like using our example website.</span></span> <span data-ttu-id="04128-131">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**</span><span class="sxs-lookup"><span data-stu-id="04128-131">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**</span></span>

<span data-ttu-id="04128-132">Ange en JSON-objekt i din begäran toospecify vilka storage-konto toouse toostore hello säkerhetskopiering hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="04128-132">Supply a JSON object in hello body of your request toospecify which storage account toouse toostore hello backup.</span></span> <span data-ttu-id="04128-133">hello JSON-objekt måste ha en egenskap med namnet **storageAccountUrl**, som har en [SAS-URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) bevilja skrivbehörighet toohello Azure Storage-behållare som innehåller hello säkerhetskopiering blob.</span><span class="sxs-lookup"><span data-stu-id="04128-133">hello JSON object must have a property named **storageAccountUrl**, which holds a [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) granting write access toohello Azure Storage container that holds hello backup blob.</span></span> <span data-ttu-id="04128-134">Om du vill tooback dina databaser, måste du även ange en lista som innehåller hello namn, typer och anslutningssträngar för hello databaser toobe säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="04128-134">If you want tooback up your databases, you must also supply a list containing hello names, types, and connection strings of hello databases toobe backed up.</span></span>

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

<span data-ttu-id="04128-135">En säkerhetskopia av hello app börjar omedelbart när hello begäran tas emot.</span><span class="sxs-lookup"><span data-stu-id="04128-135">A backup of hello app begins immediately when hello request is received.</span></span> <span data-ttu-id="04128-136">hello säkerhetskopieringen kan ta en lång tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="04128-136">hello backup process may take a long time toocomplete.</span></span> <span data-ttu-id="04128-137">hello HTTP-svaret innehåller ett ID som du kan använda i en annan begäran toosee hello status för hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="04128-137">hello HTTP response contains an ID that you can use in another request toosee hello status of hello backup.</span></span> <span data-ttu-id="04128-138">Här är ett exempel på hello brödtext hello HTTP-svar tooour säkerhetskopiering begäran.</span><span class="sxs-lookup"><span data-stu-id="04128-138">Here is an example of hello body of hello HTTP response tooour backup request.</span></span>

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
> <span data-ttu-id="04128-139">Felmeddelanden kan hittas i hello log-egenskapen för hello HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="04128-139">Error messages can be found in hello log property of hello HTTP response.</span></span>
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a><span data-ttu-id="04128-140">Schemalägga automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="04128-140">Schedule automatic backups</span></span>
<span data-ttu-id="04128-141">I toobacking lägga in en app på begäran, kan du också schemalägga säkerhetskopiering toohappen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="04128-141">In addition toobacking up an app on demand, you can also schedule a backup toohappen automatically.</span></span>

### <a name="set-up-a-new-automatic-backup-schedule"></a><span data-ttu-id="04128-142">Ställ in ett nytt schema för automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="04128-142">Set up a new automatic backup schedule</span></span>
<span data-ttu-id="04128-143">tooset upp ett schema för säkerhetskopiering, skicka ett **PLACERA** begäran för**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config / backup**.</span><span class="sxs-lookup"><span data-stu-id="04128-143">tooset up a backup schedule, send a **PUT** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.</span></span>

<span data-ttu-id="04128-144">Här är hello URL ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-144">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="04128-145">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**</span><span class="sxs-lookup"><span data-stu-id="04128-145">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**</span></span>

<span data-ttu-id="04128-146">hello frågans brödtext måste ha ett JSON-objekt som anger hello konfigurationen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="04128-146">hello request body must have a JSON object that specifies hello backup configuration.</span></span> <span data-ttu-id="04128-147">Här är ett exempel med alla parametrar för hello krävs.</span><span class="sxs-lookup"><span data-stu-id="04128-147">Here is an example with all hello required parameters.</span></span>

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

<span data-ttu-id="04128-148">Det här exemplet konfigureras hello app toobe säkerhetskopieras automatiskt var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="04128-148">This example configures hello app toobe automatically backed up every seven days.</span></span> <span data-ttu-id="04128-149">Hej parametrar **frequencyInterval** och **frequencyUnit** tillsammans bestämmer hur ofta hello säkerhetskopieringar göras.</span><span class="sxs-lookup"><span data-stu-id="04128-149">hello parameters **frequencyInterval** and **frequencyUnit** together determine how often hello backups happen.</span></span> <span data-ttu-id="04128-150">Giltiga värden för **frequencyUnit** är **timme** och **dag**.</span><span class="sxs-lookup"><span data-stu-id="04128-150">Valid values for **frequencyUnit** are **hour** and **day**.</span></span> <span data-ttu-id="04128-151">Ange frequencyInterval too12 och frequencyUnit toohour exempelvis tooback upp en app var 12: e timme.</span><span class="sxs-lookup"><span data-stu-id="04128-151">For example, tooback up an app every 12 hours, set frequencyInterval too12 and frequencyUnit toohour.</span></span>

<span data-ttu-id="04128-152">Gamla säkerhetskopior tas automatiskt bort från hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="04128-152">Old backups are automatically removed from hello storage account.</span></span> <span data-ttu-id="04128-153">Du kan styra hur gammal hello säkerhetskopieringar kan vara genom att ange hello **retentionPeriodInDays** parameter.</span><span class="sxs-lookup"><span data-stu-id="04128-153">You can control how old hello backups can be by setting hello **retentionPeriodInDays** parameter.</span></span> <span data-ttu-id="04128-154">Om du vill tooalways har minst en säkerhetskopia som sparats, oavsett hur gamla, anges **keepAtLeastOneBackup** tootrue.</span><span class="sxs-lookup"><span data-stu-id="04128-154">If you want tooalways have at least one backup saved, regardless of how old it is, set **keepAtLeastOneBackup** tootrue.</span></span>

### <a name="get-hello-automatic-backup-schedule"></a><span data-ttu-id="04128-155">Hämta hello schema för automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="04128-155">Get hello automatic backup schedule</span></span>
<span data-ttu-id="04128-156">tooget en app säkerhetskopiera configuration skickar en **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} / config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="04128-156">tooget an app’s backup configuration, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.</span></span>

<span data-ttu-id="04128-157">hello URL: en för webbplatsen exempel är **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="04128-157">hello URL for our example site is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span></span>

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a><span data-ttu-id="04128-158">Hämta hello status för en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="04128-158">Get hello status of a backup</span></span>
<span data-ttu-id="04128-159">Beroende på hur stor hello app är en säkerhetskopia kan ta en stund toocomplete.</span><span class="sxs-lookup"><span data-stu-id="04128-159">Depending on how large hello app is, a backup may take a while toocomplete.</span></span> <span data-ttu-id="04128-160">Säkerhetskopieringar kan också misslyckas timeout, eller delvis lyckas.</span><span class="sxs-lookup"><span data-stu-id="04128-160">Backups might also fail, time out, or partially succeed.</span></span> <span data-ttu-id="04128-161">toosee hello status för alla appens säkerhetskopior, skicka ett **hämta** toohello URL-begäran **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} /providers/ Microsoft.Web/sites/{name}/backups**.</span><span class="sxs-lookup"><span data-stu-id="04128-161">toosee hello status of all an app’s backups, send a **GET** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.</span></span>

<span data-ttu-id="04128-162">toosee hello status för en specifik säkerhetskopiering, skickar en GET-begäran toohello URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { Backup-id}**.</span><span class="sxs-lookup"><span data-stu-id="04128-162">toosee hello status of a specific backup, send a GET request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="04128-163">Här är hello URL ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-163">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="04128-164">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**</span><span class="sxs-lookup"><span data-stu-id="04128-164">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<span data-ttu-id="04128-165">hello svarstexten innehåller en liknande toothis exempel för en JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="04128-165">hello response body contains a JSON object similar toothis example.</span></span>

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

<span data-ttu-id="04128-166">hello status för en säkerhetskopia är en uppräknad typ.</span><span class="sxs-lookup"><span data-stu-id="04128-166">hello status of a backup is an enumerated type.</span></span> <span data-ttu-id="04128-167">Här är alla möjliga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="04128-167">Here is every possible state.</span></span>

* <span data-ttu-id="04128-168">0 – InProgress: hello säkerhetskopiering har startats men har inte slutförts ännu.</span><span class="sxs-lookup"><span data-stu-id="04128-168">0 – InProgress: hello backup has been started but has not yet completed.</span></span>
* <span data-ttu-id="04128-169">1 – misslyckades: hello säkerhetskopiering misslyckades.</span><span class="sxs-lookup"><span data-stu-id="04128-169">1 – Failed: hello backup was unsuccessful.</span></span>
* <span data-ttu-id="04128-170">2 – lyckades: hello säkerhetskopieringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="04128-170">2 – Succeeded: hello backup completed successfully.</span></span>
* <span data-ttu-id="04128-171">3 – för lång tid: hello säkerhetskopieringen slutfördes inte i tid och avbröts.</span><span class="sxs-lookup"><span data-stu-id="04128-171">3 – TimedOut: hello backup did not finish in time and was canceled.</span></span>
* <span data-ttu-id="04128-172">4 – skapa: hello säkerhetskopiering begäran är i kö men har inte startats.</span><span class="sxs-lookup"><span data-stu-id="04128-172">4 – Created: hello backup request is queued but has not been started.</span></span>
* <span data-ttu-id="04128-173">5 – hoppades över: hello säkerhetskopiering inte fortsätta på grund av tooa schema som utlöser för många säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="04128-173">5 – Skipped: hello backup did not proceed due tooa schedule triggering too many backups.</span></span>
* <span data-ttu-id="04128-174">6 – PartiallySucceeded: hello säkerhetskopiering lyckades, men vissa filer säkerhetskopierats inte eftersom de inte kan läsas.</span><span class="sxs-lookup"><span data-stu-id="04128-174">6 – PartiallySucceeded: hello backup succeeded, but some files were not backed up because they could not be read.</span></span> <span data-ttu-id="04128-175">Detta inträffar vanligen eftersom ett exklusivt lås placerades på hello-filer.</span><span class="sxs-lookup"><span data-stu-id="04128-175">This usually happens because an exclusive lock was placed on hello files.</span></span>
* <span data-ttu-id="04128-176">7 – DeleteInProgress: hello säkerhetskopiering har begärt toobe bort, men har inte tagits bort.</span><span class="sxs-lookup"><span data-stu-id="04128-176">7 – DeleteInProgress: hello backup has been requested toobe deleted, but has not yet been deleted.</span></span>
* <span data-ttu-id="04128-177">8 – DeleteFailed: hello säkerhetskopiering kunde inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="04128-177">8 – DeleteFailed: hello backup could not be deleted.</span></span> <span data-ttu-id="04128-178">Detta kan inträffa eftersom hello SAS-URL som har använt toocreate hello backup har gått ut.</span><span class="sxs-lookup"><span data-stu-id="04128-178">This might happen because hello SAS URL that was used toocreate hello backup has expired.</span></span>
* <span data-ttu-id="04128-179">9 – tas bort: hello säkerhetskopiering har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="04128-179">9 – Deleted: hello backup was deleted successfully.</span></span>

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a><span data-ttu-id="04128-180">Återställa en app från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="04128-180">Restore an app from a backup</span></span>
<span data-ttu-id="04128-181">Om din app har tagits bort, eller om du vill toorevert din app tooa tidigare version, kan du återställa hello app från en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="04128-181">If your app has been deleted, or if you want toorevert your app tooa previous version, you can restore hello app from a backup.</span></span> <span data-ttu-id="04128-182">tooinvoke en återställning, skicka ett **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopieringar / {säkerhetskopiering id} / återställa**.</span><span class="sxs-lookup"><span data-stu-id="04128-182">tooinvoke a restore, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.</span></span>

<span data-ttu-id="04128-183">Här är hello URL ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-183">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="04128-184">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**</span><span class="sxs-lookup"><span data-stu-id="04128-184">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**</span></span>

<span data-ttu-id="04128-185">Skicka ett JSON-objekt som innehåller hello egenskaper för hello återställningen i hello begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="04128-185">In hello request body, send a JSON object that contains hello properties for hello restore operation.</span></span> <span data-ttu-id="04128-186">Här är ett exempel som innehåller alla nödvändiga egenskaper:</span><span class="sxs-lookup"><span data-stu-id="04128-186">Here is an example containing all required properties:</span></span>

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

### <a name="restore-tooa-new-app"></a><span data-ttu-id="04128-187">Återställa tooa ny app</span><span class="sxs-lookup"><span data-stu-id="04128-187">Restore tooa new app</span></span>
<span data-ttu-id="04128-188">Ibland kanske du vill toocreate en ny app när du återställer en säkerhetskopia, i stället för att skriva över en befintlig app.</span><span class="sxs-lookup"><span data-stu-id="04128-188">Sometimes you might want toocreate a new app when you restore a backup, instead of overwriting an already existing app.</span></span> <span data-ttu-id="04128-189">toodo, ändra hello begäran URL toopoint toohello nya appen toocreate, och ändra hello **över** egenskap i hello JSON för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="04128-189">toodo this, change hello request URL toopoint toohello new app you want toocreate, and change hello **overwrite** property in hello JSON too**false**.</span></span>

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a><span data-ttu-id="04128-190">Tar bort en säkerhetskopiering av appen</span><span class="sxs-lookup"><span data-stu-id="04128-190">Delete an app backup</span></span>
<span data-ttu-id="04128-191">Om du vill toodelete en säkerhetskopia kan skicka en **ta bort** toohello URL-begäran **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} /backups/ {säkerhetskopiering id}**.</span><span class="sxs-lookup"><span data-stu-id="04128-191">If you would like toodelete a backup, send a **DELETE** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="04128-192">Här är hello URL ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-192">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="04128-193">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**</span><span class="sxs-lookup"><span data-stu-id="04128-193">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a><span data-ttu-id="04128-194">Hantera en säkerhetskopia SAS-URL</span><span class="sxs-lookup"><span data-stu-id="04128-194">Manage a backup’s SAS URL</span></span>
<span data-ttu-id="04128-195">Azure Apptjänst försöker toodelete säkerhetskopiorna från Azure Storage med hjälp av hello SAS-URL som angavs när hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="04128-195">Azure App Service will attempt toodelete your backup from Azure Storage using hello SAS URL that was provided when hello backup was created.</span></span> <span data-ttu-id="04128-196">Om den här SAS-URL är inte längre giltig, kan inte hello säkerhetskopieringen tas bort via hello REST API.</span><span class="sxs-lookup"><span data-stu-id="04128-196">If this SAS URL is no longer valid, hello backup cannot be deleted through hello REST API.</span></span> <span data-ttu-id="04128-197">Men du kan uppdatera hello SAS-URL som är associerade med en säkerhetskopia genom att skicka en **POST** toohello URL-begäran **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span><span class="sxs-lookup"><span data-stu-id="04128-197">However, you can update hello SAS URL associated with a backup by sending a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span></span>

<span data-ttu-id="04128-198">Här är hello URL ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-198">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="04128-199">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**</span><span class="sxs-lookup"><span data-stu-id="04128-199">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**</span></span>

<span data-ttu-id="04128-200">Skicka ett JSON-objekt som innehåller hello nya SAS-URL i hello begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="04128-200">In hello request body, send a JSON object that contains hello new SAS URL.</span></span> <span data-ttu-id="04128-201">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="04128-201">Here is an example.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> <span data-ttu-id="04128-202">Av säkerhetsskäl returneras inte hello SAS-URL som är associerad med en säkerhetskopia när du skickar en GET-begäran för en specifik säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="04128-202">For security reasons, hello SAS URL associated with a backup is not returned when sending a GET request for a specific backup.</span></span> <span data-ttu-id="04128-203">Om du vill tooview hello SAS-URL som är associerad med en säkerhetskopia kan skicka en POST-begäran toohello samma URL ovan.</span><span class="sxs-lookup"><span data-stu-id="04128-203">If you want tooview hello SAS URL associated with a backup, send a POST request toohello same URL above.</span></span> <span data-ttu-id="04128-204">Inkludera ett tomt JSON-objekt i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="04128-204">Include an empty JSON object in hello request body.</span></span> <span data-ttu-id="04128-205">hello svar från hello server innehåller alla som säkerhetskopierar information, inklusive dess SAS-URL.</span><span class="sxs-lookup"><span data-stu-id="04128-205">hello response from hello server contains all of that backup’s information, including its SAS URL.</span></span>
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
