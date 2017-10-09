---
title: "aaaCreate din första funktionen från hello Azure CLI | Microsoft Docs"
description: "Lär dig hur toocreate ditt första Azure fungerar för serverlösa körning med hello Azure CLI."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: erikre
ms.openlocfilehash: 5feed0045d4998b88b0e1bb50996cb7bb42b0822
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-hello-azure-cli"></a>Skapa din första funktion med hjälp av hello Azure CLI

Den här snabbstartsguide går igenom hur toouse Azure Functions toocreate din första funktion. Du kan använda hello Azure CLI toocreate en funktionsapp är hello serverlösa infrastruktur som är värd för din funktion. hello Funktionskoden själva distribueras från en exempel GitHub-databas.    

Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator. 

## <a name="prerequisites"></a>Krav 

Innan du kör det här exemplet måste du ha hello följande:

+ Ett aktivt [GitHub](https://github.com)-konto. 
+ En aktiv Azure-prenumeration.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create). En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. funktionsappar, databaser och lagringskonton) distribueras och hanteras i.

hello följande exempel skapar en resursgrupp med namnet `myResourceGroup`.  
Om du inte använder Cloud Shell måste du först logga in med `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto

Funktioner som använder ett Azure Storage-konto toomaintain tillstånd och annan information om dina funktioner. Skapa ett lagringskonto i hello resursgrupp som du skapat med hjälp av hello [az storage-konto skapar](/cli/azure/storage/account#create) kommando.

I hello följande kommando, ersätta egna globalt unik lagringskontonamnet där du ser hello `<storage_name>` platshållare. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

När hello storage-konto har skapats, visar hello Azure CLI information liknande toohello följande exempel:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Skapa en funktionsapp

Du måste ha en funktion app toohost hello körningen av dina funktioner. Hej funktionsapp är en miljö där serverlösa körningen av funktionskoden. Där kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser. Skapa en funktionsapp med hjälp av hello [az functionapp skapa](/cli/azure/functionapp#create) kommando. 

I hello följande kommando, ersätta dina egna unika funktionen appnamn där du ser hello `<app_name>` platshållare och hello lagringskontonamnet för `<storage_name>`. Hej `<app_name>` används som hello DNS standarddomän för hello funktionsapp och så hello namn måste toobe unikt över alla program i Azure. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Som standard skapas en funktionsapp med hello förbrukning värd plan, vilket innebär att resurser läggs till dynamiskt som krävs av dina funktioner och du betalar endast när funktioner som körs. Mer information finns i [Välj hello rätt värd plan](functions-scale.md). 

När hello funktionsapp har skapats, visar hello Azure CLI information liknande toohello följande exempel:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Nu när du har en funktionsapp, kan du distribuera hello faktiska Funktionskoden från exemplet hello GitHub-lagringsplatsen.

## <a name="deploy-your-function-code"></a>Distribuera din funktionskod  

Det finns flera sätt toocreate Funktionskoden i din nya funktionsapp. Det här avsnittet ansluter tooa exempel lagringsplatsen i GitHub. Som tidigare i hello följande kod ersätter hello `<app_name>` med hello namnet på hello funktionsapp som du skapade. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Efter distributionen hello har källa angetts måste hello Azure CLI visar information liknande toohello följande exempel (null-värden bort för att läsa):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-hello-function"></a>Testa hello-funktionen

Funktionen cURL tootest hello distribueras på en Mac- eller Linux-dator eller med Bash i Windows. Kör följande cURL-kommando och ersätter hello hello `<app_name>` med hello namnet på din funktionsapp. Lägg till hello frågesträngen `&name=<yourname>` toohello URL.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Om du inte har cURL som är tillgängliga i kommandoraden ange hello samma URL i hello-adress i webbläsaren. Igen och Ersätt hello `<app_name>` med hello namnet på appen funktionen och Lägg till hello frågesträngen `&name=<yourname>` toohello URL och utföra hello-begäran. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Rensa resurser

De andra snabbstarterna i den här samlingen bygger på den här snabbstarten. Om du planerar toocontinue på toowork med efterföljande Snabbstart eller hello självstudier inte rensa hello resurser som skapats i denna Snabbstart. Om du inte planerar toocontinue använder du följande kommando toodelete hello alla resurser som skapats av denna Snabbstart:

```azurecli-interactive
az group delete --name myResourceGroup
```
Skriv `y` när du uppmanas till detta.

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
