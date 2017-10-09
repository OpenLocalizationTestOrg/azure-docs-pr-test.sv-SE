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
# <a name="create-your-first-function-using-hello-azure-cli"></a><span data-ttu-id="944b0-103">Skapa din första funktion med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="944b0-103">Create your first function using hello Azure CLI</span></span>

<span data-ttu-id="944b0-104">Den här snabbstartsguide går igenom hur toouse Azure Functions toocreate din första funktion.</span><span class="sxs-lookup"><span data-stu-id="944b0-104">This quickstart tutorial walks through how toouse Azure Functions toocreate your first function.</span></span> <span data-ttu-id="944b0-105">Du kan använda hello Azure CLI toocreate en funktionsapp är hello serverlösa infrastruktur som är värd för din funktion.</span><span class="sxs-lookup"><span data-stu-id="944b0-105">You use hello Azure CLI toocreate a function app, which is hello serverless infrastructure that hosts your function.</span></span> <span data-ttu-id="944b0-106">hello Funktionskoden själva distribueras från en exempel GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="944b0-106">hello function code itself is deployed from a GitHub sample repository.</span></span>    

<span data-ttu-id="944b0-107">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="944b0-107">You can follow hello steps below using a Mac, Windows, or Linux computer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="944b0-108">Krav</span><span class="sxs-lookup"><span data-stu-id="944b0-108">Prerequisites</span></span> 

<span data-ttu-id="944b0-109">Innan du kör det här exemplet måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="944b0-109">Before running this sample, you must have hello following:</span></span>

+ <span data-ttu-id="944b0-110">Ett aktivt [GitHub](https://github.com)-konto.</span><span class="sxs-lookup"><span data-stu-id="944b0-110">An active [GitHub](https://github.com) account.</span></span> 
+ <span data-ttu-id="944b0-111">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="944b0-111">An active Azure subscription.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="944b0-112">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="944b0-112">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="944b0-113">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="944b0-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="944b0-114">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="944b0-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="944b0-115">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="944b0-115">Create a resource group</span></span>

<span data-ttu-id="944b0-116">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="944b0-116">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="944b0-117">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. funktionsappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="944b0-117">An Azure resource group is a logical container into which Azure resources like function apps, databases, and storage accounts are deployed and managed.</span></span>

<span data-ttu-id="944b0-118">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="944b0-118">hello following example creates a resource group named `myResourceGroup`.</span></span>  
<span data-ttu-id="944b0-119">Om du inte använder Cloud Shell måste du först logga in med `az login`.</span><span class="sxs-lookup"><span data-stu-id="944b0-119">If you are not using Cloud Shell, you must first sign in using `az login`.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a><span data-ttu-id="944b0-120">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="944b0-120">Create an Azure Storage account</span></span>

<span data-ttu-id="944b0-121">Funktioner som använder ett Azure Storage-konto toomaintain tillstånd och annan information om dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="944b0-121">Functions uses an Azure Storage account toomaintain state and other information about your functions.</span></span> <span data-ttu-id="944b0-122">Skapa ett lagringskonto i hello resursgrupp som du skapat med hjälp av hello [az storage-konto skapar](/cli/azure/storage/account#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="944b0-122">Create a storage account in hello resource group you created by using hello [az storage account create](/cli/azure/storage/account#create) command.</span></span>

<span data-ttu-id="944b0-123">I hello följande kommando, ersätta egna globalt unik lagringskontonamnet där du ser hello `<storage_name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="944b0-123">In hello following command, substitute your own globally unique storage account name where you see hello `<storage_name>` placeholder.</span></span> <span data-ttu-id="944b0-124">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="944b0-124">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

<span data-ttu-id="944b0-125">När hello storage-konto har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="944b0-125">After hello storage account has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

## <a name="create-a-function-app"></a><span data-ttu-id="944b0-126">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="944b0-126">Create a function app</span></span>

<span data-ttu-id="944b0-127">Du måste ha en funktion app toohost hello körningen av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="944b0-127">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="944b0-128">Hej funktionsapp är en miljö där serverlösa körningen av funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="944b0-128">hello function app provides an environment for serverless execution of your function code.</span></span> <span data-ttu-id="944b0-129">Där kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser.</span><span class="sxs-lookup"><span data-stu-id="944b0-129">It lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> <span data-ttu-id="944b0-130">Skapa en funktionsapp med hjälp av hello [az functionapp skapa](/cli/azure/functionapp#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="944b0-130">Create a function app by using hello [az functionapp create](/cli/azure/functionapp#create) command.</span></span> 

<span data-ttu-id="944b0-131">I hello följande kommando, ersätta dina egna unika funktionen appnamn där du ser hello `<app_name>` platshållare och hello lagringskontonamnet för `<storage_name>`.</span><span class="sxs-lookup"><span data-stu-id="944b0-131">In hello following command, substitute your own unique function app name where you see hello `<app_name>` placeholder and hello storage account name for  `<storage_name>`.</span></span> <span data-ttu-id="944b0-132">Hej `<app_name>` används som hello DNS standarddomän för hello funktionsapp och så hello namn måste toobe unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="944b0-132">hello `<app_name>` is used as hello default DNS domain for hello function app, and so hello name needs toobe unique across all apps in Azure.</span></span> 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
<span data-ttu-id="944b0-133">Som standard skapas en funktionsapp med hello förbrukning värd plan, vilket innebär att resurser läggs till dynamiskt som krävs av dina funktioner och du betalar endast när funktioner som körs.</span><span class="sxs-lookup"><span data-stu-id="944b0-133">By default, a function app is created with hello Consumption hosting plan, which means that resources are added dynamically as required by your functions and you only pay when functions are running.</span></span> <span data-ttu-id="944b0-134">Mer information finns i [Välj hello rätt värd plan](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="944b0-134">For more information, see [Choose hello correct hosting plan](functions-scale.md).</span></span> 

<span data-ttu-id="944b0-135">När hello funktionsapp har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="944b0-135">After hello function app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="944b0-136">Nu när du har en funktionsapp, kan du distribuera hello faktiska Funktionskoden från exemplet hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="944b0-136">Now that you have a function app, you can deploy hello actual function code from hello GitHub sample repository.</span></span>

## <a name="deploy-your-function-code"></a><span data-ttu-id="944b0-137">Distribuera din funktionskod</span><span class="sxs-lookup"><span data-stu-id="944b0-137">Deploy your function code</span></span>  

<span data-ttu-id="944b0-138">Det finns flera sätt toocreate Funktionskoden i din nya funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="944b0-138">There are several ways toocreate your function code in your new function app.</span></span> <span data-ttu-id="944b0-139">Det här avsnittet ansluter tooa exempel lagringsplatsen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="944b0-139">This topic connects tooa sample repository in GitHub.</span></span> <span data-ttu-id="944b0-140">Som tidigare i hello följande kod ersätter hello `<app_name>` med hello namnet på hello funktionsapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="944b0-140">As before, in hello following code replace hello `<app_name>` placeholder with hello name of hello function app you created.</span></span> 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
<span data-ttu-id="944b0-141">Efter distributionen hello har källa angetts måste hello Azure CLI visar information liknande toohello följande exempel (null-värden bort för att läsa):</span><span class="sxs-lookup"><span data-stu-id="944b0-141">After hello deployment source been set, hello Azure CLI shows information similar toohello following example (null values removed for readability):</span></span>

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

## <a name="test-hello-function"></a><span data-ttu-id="944b0-142">Testa hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="944b0-142">Test hello function</span></span>

<span data-ttu-id="944b0-143">Funktionen cURL tootest hello distribueras på en Mac- eller Linux-dator eller med Bash i Windows.</span><span class="sxs-lookup"><span data-stu-id="944b0-143">Use cURL tootest hello deployed function on a Mac or Linux computer or using Bash on Windows.</span></span> <span data-ttu-id="944b0-144">Kör följande cURL-kommando och ersätter hello hello `<app_name>` med hello namnet på din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="944b0-144">Execute hello following cURL command, replacing hello `<app_name>` placeholder with hello name of your function app.</span></span> <span data-ttu-id="944b0-145">Lägg till hello frågesträngen `&name=<yourname>` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="944b0-145">Append hello query string `&name=<yourname>` toohello URL.</span></span>

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

<span data-ttu-id="944b0-147">Om du inte har cURL som är tillgängliga i kommandoraden ange hello samma URL i hello-adress i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="944b0-147">If you don't have cURL available in your command line, enter hello same URL in hello address of your web browser.</span></span> <span data-ttu-id="944b0-148">Igen och Ersätt hello `<app_name>` med hello namnet på appen funktionen och Lägg till hello frågesträngen `&name=<yourname>` toohello URL och utföra hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="944b0-148">Again, replace hello `<app_name>` placeholder with hello name of your function app, and append hello query string `&name=<yourname>` toohello URL and execute hello request.</span></span> 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a><span data-ttu-id="944b0-150">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="944b0-150">Clean up resources</span></span>

<span data-ttu-id="944b0-151">De andra snabbstarterna i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="944b0-151">Other quickstarts in this collection build upon this quickstart.</span></span> <span data-ttu-id="944b0-152">Om du planerar toocontinue på toowork med efterföljande Snabbstart eller hello självstudier inte rensa hello resurser som skapats i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="944b0-152">If you plan toocontinue on toowork with subsequent quickstarts or with hello tutorials, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="944b0-153">Om du inte planerar toocontinue använder du följande kommando toodelete hello alla resurser som skapats av denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="944b0-153">If you do not plan toocontinue, use hello following command toodelete all resources created by this quickstart:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```
<span data-ttu-id="944b0-154">Skriv `y` när du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="944b0-154">Type `y` when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="944b0-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="944b0-155">Next steps</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
