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
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a><span data-ttu-id="76cbe-103">Använd REST för att säkerhetskopiera och återställa Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="76cbe-103">Use REST to back up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="76cbe-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="76cbe-104">PowerShell</span></span>](../app-service/app-service-powershell-backup.md)
> * [<span data-ttu-id="76cbe-105">REST-API</span><span class="sxs-lookup"><span data-stu-id="76cbe-105">REST API</span></span>](websites-csm-backup.md)
> 
> 

<span data-ttu-id="76cbe-106">[Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/) kan säkerhetskopieras som blobar i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="76cbe-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) can be backed up as blobs in Azure storage.</span></span> <span data-ttu-id="76cbe-107">Säkerhetskopieringen kan också innehålla appens databaser.</span><span class="sxs-lookup"><span data-stu-id="76cbe-107">The backup can also contain the app’s databases.</span></span> <span data-ttu-id="76cbe-108">Om appen någonsin av misstag tas bort, eller om appen behöver återställas till en tidigare version, kan du återställa den från en tidigare säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="76cbe-108">If the app is ever accidentally deleted, or if the app needs to be reverted to a previous version, it can be restored from any previous backup.</span></span> <span data-ttu-id="76cbe-109">Säkerhetskopieringar kan göras när som helst på begäran eller säkerhetskopieringar kan schemaläggas med lämpliga intervall.</span><span class="sxs-lookup"><span data-stu-id="76cbe-109">Backups can be done at any time on demand, or backups can be scheduled at suitable intervals.</span></span>

<span data-ttu-id="76cbe-110">Den här artikeln beskriver hur du säkerhetskopierar och återställer en app med RESTful-API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="76cbe-110">This article explains how to backup and restore an app with RESTful API requests.</span></span> <span data-ttu-id="76cbe-111">Om du vill skapa och hantera appen säkerhetskopior grafiskt via Azure portal, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)</span><span class="sxs-lookup"><span data-stu-id="76cbe-111">If you would like to create and manage app backups graphically through the Azure portal, see [Back up a web app in Azure App Service](web-sites-backup.md)</span></span>

<a name="gettingstarted"></a>

## <a name="getting-started"></a><span data-ttu-id="76cbe-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="76cbe-112">Getting Started</span></span>
<span data-ttu-id="76cbe-113">Om du vill skicka REST-begäranden som du behöver veta din app **namn**, **resursgruppen**, och **prenumerations-id**. Den här informationen kan hittas genom att klicka på din app i den **Apptjänst** bladet för den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76cbe-113">To send REST requests, you need to know your app’s **name**, **resource group**, and **subscription id**. This information can be found by clicking your app in the **App Service** blade of the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="76cbe-114">Vi konfigurerar webbplatsen för exemplen i den här artikeln **backuprestoreapiexamples.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-114">For the examples in this article, we are configuring the website **backuprestoreapiexamples.azurewebsites.net**.</span></span> <span data-ttu-id="76cbe-115">Den lagras i standard-webb-WestUS resursgruppen och körs på en prenumeration med det ID 00001111-2222-3333-4444-555566667777.</span><span class="sxs-lookup"><span data-stu-id="76cbe-115">It is stored in the Default-Web-WestUS resource group and is running on a subscription with the ID 00001111-2222-3333-4444-555566667777.</span></span>

![Exempel webbplatsinformation][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a><span data-ttu-id="76cbe-117">Säkerhetskopiering och återställning REST API</span><span class="sxs-lookup"><span data-stu-id="76cbe-117">Backup and restore REST API</span></span>
<span data-ttu-id="76cbe-118">Nu tar vi upp flera exempel på hur du använder REST API för att säkerhetskopiera och återställa en app.</span><span class="sxs-lookup"><span data-stu-id="76cbe-118">We will now cover several examples of how to use the REST API to backup and restore an app.</span></span> <span data-ttu-id="76cbe-119">Varje exempel innehåller en URL och HTTP brödtext i begäran.</span><span class="sxs-lookup"><span data-stu-id="76cbe-119">Each example includes a URL and HTTP request body.</span></span> <span data-ttu-id="76cbe-120">Exempel-URL: en innehåller platshållare kapslas in i klammerparenteser, till exempel {prenumerations-id}.</span><span class="sxs-lookup"><span data-stu-id="76cbe-120">The sample URL contains placeholders wrapped in curly braces, such as {subscription-id}.</span></span> <span data-ttu-id="76cbe-121">Ersätt platshållarna med motsvarande information för din app.</span><span class="sxs-lookup"><span data-stu-id="76cbe-121">Replace the placeholders with the corresponding information for your app.</span></span> <span data-ttu-id="76cbe-122">Här är en förklaring av varje platshållare som visas i exempel-URL: er för referens.</span><span class="sxs-lookup"><span data-stu-id="76cbe-122">For reference, here is an explanation of each placeholder that appears in the example URLs.</span></span>

* <span data-ttu-id="76cbe-123">prenumerations-id – ID för Azure-prenumeration som innehåller appen</span><span class="sxs-lookup"><span data-stu-id="76cbe-123">subscription-id – ID of the Azure subscription containing the app</span></span>
* <span data-ttu-id="76cbe-124">resursgruppens--namn – namnet på resursgruppen som innehåller appen</span><span class="sxs-lookup"><span data-stu-id="76cbe-124">resource-group-name – Name of the resource group containing the app</span></span>
* <span data-ttu-id="76cbe-125">namn – namnet på appen</span><span class="sxs-lookup"><span data-stu-id="76cbe-125">name – Name of the app</span></span>
* <span data-ttu-id="76cbe-126">Backup-id – ID för app-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="76cbe-126">backup-id – ID of the app backup</span></span>

<span data-ttu-id="76cbe-127">Komplett dokumentation av API, inklusive flera valfria parametrar som kan tas med i HTTP-begäran finns i [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="76cbe-127">For the complete documentation of the API, including several optional parameters that can be included in the HTTP request, see the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a><span data-ttu-id="76cbe-128">Säkerhetskopiera en app på begäran</span><span class="sxs-lookup"><span data-stu-id="76cbe-128">Backup an app on demand</span></span>
<span data-ttu-id="76cbe-129">Om du vill säkerhetskopiera en app direkt, skicka ett **POST** begäran om att **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ säkerhetskopiering /**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-129">To back up an app immediately, send a **POST** request to **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.</span></span>

<span data-ttu-id="76cbe-130">Här är hur URL: en ser ut med våra exempel webbplats.</span><span class="sxs-lookup"><span data-stu-id="76cbe-130">Here is what the URL looks like using our example website.</span></span> <span data-ttu-id="76cbe-131">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**</span><span class="sxs-lookup"><span data-stu-id="76cbe-131">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**</span></span>

<span data-ttu-id="76cbe-132">Ange en JSON-objekt i brödtexten för din begäran om att ange vilken lagringskontotyp du använder för att lagra säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="76cbe-132">Supply a JSON object in the body of your request to specify which storage account to use to store the backup.</span></span> <span data-ttu-id="76cbe-133">JSON-objekt måste ha en egenskap med namnet **storageAccountUrl**, som har en [SAS-URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) bevilja åtkomst till Azure Storage-behållare som innehåller den säkerhetskopiera blobben.</span><span class="sxs-lookup"><span data-stu-id="76cbe-133">The JSON object must have a property named **storageAccountUrl**, which holds a [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) granting write access to the Azure Storage container that holds the backup blob.</span></span> <span data-ttu-id="76cbe-134">Om du vill säkerhetskopiera databaser, måste du även ange en lista med namn, typer och anslutningssträngar för databaser som ska säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="76cbe-134">If you want to back up your databases, you must also supply a list containing the names, types, and connection strings of the databases to be backed up.</span></span>

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

<span data-ttu-id="76cbe-135">En säkerhetskopia av appen börjar omedelbart när begäran tas emot.</span><span class="sxs-lookup"><span data-stu-id="76cbe-135">A backup of the app begins immediately when the request is received.</span></span> <span data-ttu-id="76cbe-136">Säkerhetskopieringen kan ta lång tid att slutföra.</span><span class="sxs-lookup"><span data-stu-id="76cbe-136">The backup process may take a long time to complete.</span></span> <span data-ttu-id="76cbe-137">HTTP-svaret innehåller ett ID som du kan använda i en annan begäran om att se status för säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="76cbe-137">The HTTP response contains an ID that you can use in another request to see the status of the backup.</span></span> <span data-ttu-id="76cbe-138">Här är ett exempel på innehållet i HTTP-svar på vår säkerhetskopiering begäran.</span><span class="sxs-lookup"><span data-stu-id="76cbe-138">Here is an example of the body of the HTTP response to our backup request.</span></span>

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
> <span data-ttu-id="76cbe-139">Felmeddelanden kan hittas i egenskapen av HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="76cbe-139">Error messages can be found in the log property of the HTTP response.</span></span>
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a><span data-ttu-id="76cbe-140">Schemalägga automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="76cbe-140">Schedule automatic backups</span></span>
<span data-ttu-id="76cbe-141">Förutom att säkerhetskopiera en app på begäran, kan du också schemalägga en säkerhetskopiering sker automatiskt.</span><span class="sxs-lookup"><span data-stu-id="76cbe-141">In addition to backing up an app on demand, you can also schedule a backup to happen automatically.</span></span>

### <a name="set-up-a-new-automatic-backup-schedule"></a><span data-ttu-id="76cbe-142">Ställ in ett nytt schema för automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="76cbe-142">Set up a new automatic backup schedule</span></span>
<span data-ttu-id="76cbe-143">Om du vill konfigurera ett schema för säkerhetskopiering, skicka ett **PLACERA** begäran om att **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/ säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-143">To set up a backup schedule, send a **PUT** request to **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.</span></span>

<span data-ttu-id="76cbe-144">Det här är URL-Adressen ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-144">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="76cbe-145">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**</span><span class="sxs-lookup"><span data-stu-id="76cbe-145">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**</span></span>

<span data-ttu-id="76cbe-146">Begärandetexten måste ha ett JSON-objekt som anger konfigurationen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="76cbe-146">The request body must have a JSON object that specifies the backup configuration.</span></span> <span data-ttu-id="76cbe-147">Här är ett exempel med de obligatoriska parametrarna.</span><span class="sxs-lookup"><span data-stu-id="76cbe-147">Here is an example with all the required parameters.</span></span>

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

<span data-ttu-id="76cbe-148">Det här exemplet konfigurerar appen säkerhetskopieras automatiskt var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="76cbe-148">This example configures the app to be automatically backed up every seven days.</span></span> <span data-ttu-id="76cbe-149">Parametrarna **frequencyInterval** och **frequencyUnit** tillsammans bestämmer hur ofta säkerhetskopieringarna ska inträffa.</span><span class="sxs-lookup"><span data-stu-id="76cbe-149">The parameters **frequencyInterval** and **frequencyUnit** together determine how often the backups happen.</span></span> <span data-ttu-id="76cbe-150">Giltiga värden för **frequencyUnit** är **timme** och **dag**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-150">Valid values for **frequencyUnit** are **hour** and **day**.</span></span> <span data-ttu-id="76cbe-151">Exempelvis om du vill säkerhetskopiera en app var 12: e timme, ställer du in frequencyInterval-12 och frequencyUnit timme.</span><span class="sxs-lookup"><span data-stu-id="76cbe-151">For example, to back up an app every 12 hours, set frequencyInterval to 12 and frequencyUnit to hour.</span></span>

<span data-ttu-id="76cbe-152">Gamla säkerhetskopior tas bort automatiskt från lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="76cbe-152">Old backups are automatically removed from the storage account.</span></span> <span data-ttu-id="76cbe-153">Du kan styra hur gammal säkerhetskopieringar kan vara genom att ange den **retentionPeriodInDays** parameter.</span><span class="sxs-lookup"><span data-stu-id="76cbe-153">You can control how old the backups can be by setting the **retentionPeriodInDays** parameter.</span></span> <span data-ttu-id="76cbe-154">Om du vill att alltid ska ha minst en säkerhetskopia som sparats, oavsett hur gamla, anges **keepAtLeastOneBackup** till true.</span><span class="sxs-lookup"><span data-stu-id="76cbe-154">If you want to always have at least one backup saved, regardless of how old it is, set **keepAtLeastOneBackup** to true.</span></span>

### <a name="get-the-automatic-backup-schedule"></a><span data-ttu-id="76cbe-155">Hämta schemat för automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="76cbe-155">Get the automatic backup schedule</span></span>
<span data-ttu-id="76cbe-156">För att hämta konfigurationen för säkerhetskopiering av en app, skicka ett **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} / config/säkerhetskopiera/listan**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-156">To get an app’s backup configuration, send a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.</span></span>

<span data-ttu-id="76cbe-157">URL: en för webbplatsen exempel är **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-157">The URL for our example site is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span></span>

<a name="get-backup-status"></a>

## <a name="get-the-status-of-a-backup"></a><span data-ttu-id="76cbe-158">Hämta status för en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="76cbe-158">Get the status of a backup</span></span>
<span data-ttu-id="76cbe-159">Beroende på hur stor appen är kan en säkerhetskopia ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="76cbe-159">Depending on how large the app is, a backup may take a while to complete.</span></span> <span data-ttu-id="76cbe-160">Säkerhetskopieringar kan också misslyckas timeout, eller delvis lyckas.</span><span class="sxs-lookup"><span data-stu-id="76cbe-160">Backups might also fail, time out, or partially succeed.</span></span> <span data-ttu-id="76cbe-161">Om du vill se status för alla appens säkerhetskopieringar skicka en **hämta** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ platser / {name} / säkerhetskopieringar**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-161">To see the status of all an app’s backups, send a **GET** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.</span></span>

<span data-ttu-id="76cbe-162">Om du vill se status för en specifik säkerhetskopia skickar en GET-begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}** .</span><span class="sxs-lookup"><span data-stu-id="76cbe-162">To see the status of a specific backup, send a GET request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="76cbe-163">Det här är URL-Adressen ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-163">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="76cbe-164">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**</span><span class="sxs-lookup"><span data-stu-id="76cbe-164">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<span data-ttu-id="76cbe-165">Svarstexten innehåller ett JSON-objekt som liknar det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="76cbe-165">The response body contains a JSON object similar to this example.</span></span>

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

<span data-ttu-id="76cbe-166">Status för en säkerhetskopia är en uppräknad typ.</span><span class="sxs-lookup"><span data-stu-id="76cbe-166">The status of a backup is an enumerated type.</span></span> <span data-ttu-id="76cbe-167">Här är alla möjliga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="76cbe-167">Here is every possible state.</span></span>

* <span data-ttu-id="76cbe-168">0 – Pågår: Säkerhetskopieringen har startats men ännu inte har slutförts.</span><span class="sxs-lookup"><span data-stu-id="76cbe-168">0 – InProgress: The backup has been started but has not yet completed.</span></span>
* <span data-ttu-id="76cbe-169">1 – Misslyckades: Säkerhetskopieringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="76cbe-169">1 – Failed: The backup was unsuccessful.</span></span>
* <span data-ttu-id="76cbe-170">2 – Lyckades: Säkerhetskopieringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="76cbe-170">2 – Succeeded: The backup completed successfully.</span></span>
* <span data-ttu-id="76cbe-171">3 – för lång tid: Säkerhetskopieringen slutfördes inte i tid och avbröts.</span><span class="sxs-lookup"><span data-stu-id="76cbe-171">3 – TimedOut: The backup did not finish in time and was canceled.</span></span>
* <span data-ttu-id="76cbe-172">4 – Skapad: Säkerhetskopieringsbegäran köas men har inte startats.</span><span class="sxs-lookup"><span data-stu-id="76cbe-172">4 – Created: The backup request is queued but has not been started.</span></span>
* <span data-ttu-id="76cbe-173">5 – Hoppades över: Säkerhetskopieringen har inte utförts på grund av ett schema som utlöst för många säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="76cbe-173">5 – Skipped: The backup did not proceed due to a schedule triggering too many backups.</span></span>
* <span data-ttu-id="76cbe-174">6 – Lyckades delvis: Säkerhetskopieringen slutfördes, men vissa filer säkerhetskopierades inte eftersom de inte kunde läsas.</span><span class="sxs-lookup"><span data-stu-id="76cbe-174">6 – PartiallySucceeded: The backup succeeded, but some files were not backed up because they could not be read.</span></span> <span data-ttu-id="76cbe-175">Detta inträffar vanligen eftersom filerna låsts exklusivt.</span><span class="sxs-lookup"><span data-stu-id="76cbe-175">This usually happens because an exclusive lock was placed on the files.</span></span>
* <span data-ttu-id="76cbe-176">7 – Borttagning sker: Säkerhetskopieringen har begärts att tas bort, men har inte tagits bort ännu.</span><span class="sxs-lookup"><span data-stu-id="76cbe-176">7 – DeleteInProgress: The backup has been requested to be deleted, but has not yet been deleted.</span></span>
* <span data-ttu-id="76cbe-177">8 – Borttagning misslyckades: Det gick inte att ta bort säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="76cbe-177">8 – DeleteFailed: The backup could not be deleted.</span></span> <span data-ttu-id="76cbe-178">Detta kan inträffa om SAS-webbadressen som användes för att skapa säkerhetskopian har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="76cbe-178">This might happen because the SAS URL that was used to create the backup has expired.</span></span>
* <span data-ttu-id="76cbe-179">9 – Borttagen: Säkerhetskopian har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="76cbe-179">9 – Deleted: The backup was deleted successfully.</span></span>

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a><span data-ttu-id="76cbe-180">Återställa en app från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="76cbe-180">Restore an app from a backup</span></span>
<span data-ttu-id="76cbe-181">Om din app har tagits bort, eller om du vill återställa din app till en tidigare version, kan du återställa appen från en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="76cbe-181">If your app has been deleted, or if you want to revert your app to a previous version, you can restore the app from a backup.</span></span> <span data-ttu-id="76cbe-182">Om du vill starta en återställning, skicka ett **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups / {säkerhetskopiering id} / återställa**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-182">To invoke a restore, send a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.</span></span>

<span data-ttu-id="76cbe-183">Det här är URL-Adressen ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-183">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="76cbe-184">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**</span><span class="sxs-lookup"><span data-stu-id="76cbe-184">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**</span></span>

<span data-ttu-id="76cbe-185">Skicka ett JSON-objekt som innehåller egenskaper för återställningen i frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="76cbe-185">In the request body, send a JSON object that contains the properties for the restore operation.</span></span> <span data-ttu-id="76cbe-186">Här är ett exempel som innehåller alla nödvändiga egenskaper:</span><span class="sxs-lookup"><span data-stu-id="76cbe-186">Here is an example containing all required properties:</span></span>

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

### <a name="restore-to-a-new-app"></a><span data-ttu-id="76cbe-187">Återställ till en ny app</span><span class="sxs-lookup"><span data-stu-id="76cbe-187">Restore to a new app</span></span>
<span data-ttu-id="76cbe-188">Ibland kanske du vill skapa en ny app när du återställer en säkerhetskopia, i stället för att skriva över en befintlig app.</span><span class="sxs-lookup"><span data-stu-id="76cbe-188">Sometimes you might want to create a new app when you restore a backup, instead of overwriting an already existing app.</span></span> <span data-ttu-id="76cbe-189">Gör detta genom att ändra URL: en så att den pekar till den nya appen som du vill skapa och ändra den **över** egenskap i JSON till **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-189">To do this, change the request URL to point to the new app you want to create, and change the **overwrite** property in the JSON to **false**.</span></span>

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a><span data-ttu-id="76cbe-190">Tar bort en säkerhetskopiering av appen</span><span class="sxs-lookup"><span data-stu-id="76cbe-190">Delete an app backup</span></span>
<span data-ttu-id="76cbe-191">Om du vill ta bort en säkerhetskopia kan skicka en **ta bort** begäran till URL: en **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} /backups/ {säkerhetskopiering id}**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-191">If you would like to delete a backup, send a **DELETE** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="76cbe-192">Det här är URL-Adressen ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-192">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="76cbe-193">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**</span><span class="sxs-lookup"><span data-stu-id="76cbe-193">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a><span data-ttu-id="76cbe-194">Hantera en säkerhetskopia SAS-URL</span><span class="sxs-lookup"><span data-stu-id="76cbe-194">Manage a backup’s SAS URL</span></span>
<span data-ttu-id="76cbe-195">Azure Apptjänst försöker ta bort säkerhetskopiorna från Azure Storage med hjälp av SAS-URL som angavs när säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="76cbe-195">Azure App Service will attempt to delete your backup from Azure Storage using the SAS URL that was provided when the backup was created.</span></span> <span data-ttu-id="76cbe-196">Om den här SAS-URL är inte längre giltig, kan inte säkerhetskopieringen tas bort via REST API.</span><span class="sxs-lookup"><span data-stu-id="76cbe-196">If this SAS URL is no longer valid, the backup cannot be deleted through the REST API.</span></span> <span data-ttu-id="76cbe-197">Men du kan uppdatera SAS-URL som är associerad med en säkerhetskopia genom att skicka en **POST** begäran till URL: en **https://management.azure.com/subscriptions/ {prenumerations-id} /resourceGroups/ {resurs-gruppnamn} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span><span class="sxs-lookup"><span data-stu-id="76cbe-197">However, you can update the SAS URL associated with a backup by sending a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span></span>

<span data-ttu-id="76cbe-198">Det här är URL-Adressen ser ut för webbplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-198">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="76cbe-199">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**</span><span class="sxs-lookup"><span data-stu-id="76cbe-199">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**</span></span>

<span data-ttu-id="76cbe-200">Skicka ett JSON-objekt som innehåller den nya SAS-URL i frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="76cbe-200">In the request body, send a JSON object that contains the new SAS URL.</span></span> <span data-ttu-id="76cbe-201">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="76cbe-201">Here is an example.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> <span data-ttu-id="76cbe-202">Av säkerhetsskäl returneras inte SAS-URL som är associerad med en säkerhetskopia när du skickar en GET-begäran för en specifik säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="76cbe-202">For security reasons, the SAS URL associated with a backup is not returned when sending a GET request for a specific backup.</span></span> <span data-ttu-id="76cbe-203">Skicka en POST-begäran till samma URL ovan om du vill visa SAS-URL som är associerad med en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="76cbe-203">If you want to view the SAS URL associated with a backup, send a POST request to the same URL above.</span></span> <span data-ttu-id="76cbe-204">Innehåller ett tomt JSON-objekt i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="76cbe-204">Include an empty JSON object in the request body.</span></span> <span data-ttu-id="76cbe-205">Svaret från servern innehåller all information om att säkerhetskopiering, inklusive dess SAS-URL.</span><span class="sxs-lookup"><span data-stu-id="76cbe-205">The response from the server contains all of that backup’s information, including its SAS URL.</span></span>
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
