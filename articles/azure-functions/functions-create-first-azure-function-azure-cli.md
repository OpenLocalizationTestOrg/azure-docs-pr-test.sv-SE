---
title: "Skapa din första funktion med Azure CLI | Microsoft Docs"
description: "Lär dig hur du skapar din första Azure-funktion för serverfri körning med Azure CLI."
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
ms.openlocfilehash: 8bd3e4bb7423db44c48b04f25edcf1074e6ea0bd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a><span data-ttu-id="d0dcb-103">Skapa din första funktion med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d0dcb-103">Create your first function using the Azure CLI</span></span>

<span data-ttu-id="d0dcb-104">I den här snabbstartsguiden får du hjälp att skapa din första funktion i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-104">This quickstart tutorial walks through how to use Azure Functions to create your first function.</span></span> <span data-ttu-id="d0dcb-105">Du kan använda Azure CLI till att skapa en funktionsapp, som är den serverfria infrastruktur som är värd för funktionen.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-105">You use the Azure CLI to create a function app, which is the serverless infrastructure that hosts your function.</span></span> <span data-ttu-id="d0dcb-106">Själva funktionskoden distribueras från en GitHub-exempellagringsplats.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-106">The function code itself is deployed from a GitHub sample repository.</span></span>    

<span data-ttu-id="d0dcb-107">Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-107">You can follow the steps below using a Mac, Windows, or Linux computer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d0dcb-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d0dcb-108">Prerequisites</span></span> 

<span data-ttu-id="d0dcb-109">Innan du kör exemplet måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="d0dcb-109">Before running this sample, you must have the following:</span></span>

+ <span data-ttu-id="d0dcb-110">Ett aktivt [GitHub](https://github.com)-konto.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-110">An active [GitHub](https://github.com) account.</span></span> 
+ <span data-ttu-id="d0dcb-111">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-111">An active Azure subscription.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d0dcb-112">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-112">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d0dcb-113">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="d0dcb-114">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d0dcb-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="d0dcb-115">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d0dcb-115">Create a resource group</span></span>

<span data-ttu-id="d0dcb-116">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="d0dcb-116">Create a resource group with the [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="d0dcb-117">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. funktionsappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-117">An Azure resource group is a logical container into which Azure resources like function apps, databases, and storage accounts are deployed and managed.</span></span>

<span data-ttu-id="d0dcb-118">I följande exempel skapas en resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-118">The following example creates a resource group named `myResourceGroup`.</span></span>  
<span data-ttu-id="d0dcb-119">Om du inte använder Cloud Shell måste du först logga in med `az login`.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-119">If you are not using Cloud Shell, you must first sign in using `az login`.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a><span data-ttu-id="d0dcb-120">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="d0dcb-120">Create an Azure Storage account</span></span>

<span data-ttu-id="d0dcb-121">I funktioner används ett Azure Storage-konto till att lagra status och annan information om dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-121">Functions uses an Azure Storage account to maintain state and other information about your functions.</span></span> <span data-ttu-id="d0dcb-122">Skapa ett lagringskonto i resursgruppen du skapade med hjälp av kommandot [az storage account create](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="d0dcb-122">Create a storage account in the resource group you created by using the [az storage account create](/cli/azure/storage/account#create) command.</span></span>

<span data-ttu-id="d0dcb-123">I följande kommando infogar du ditt globalt unika lagringskontonamn istället för platshållaren `<storage_name>`.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-123">In the following command, substitute your own globally unique storage account name where you see the `<storage_name>` placeholder.</span></span> <span data-ttu-id="d0dcb-124">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-124">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

<span data-ttu-id="d0dcb-125">När lagringskontot har skapats visas information som liknar följande exempel i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="d0dcb-125">After the storage account has been created, the Azure CLI shows information similar to the following example:</span></span>

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

## <a name="create-a-function-app"></a><span data-ttu-id="d0dcb-126">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="d0dcb-126">Create a function app</span></span>

<span data-ttu-id="d0dcb-127">Du måste ha en funktionsapp som värd för körning av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-127">You must have a function app to host the execution of your functions.</span></span> <span data-ttu-id="d0dcb-128">Funktionsappen är en miljö för serverfri körning av funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-128">The function app provides an environment for serverless execution of your function code.</span></span> <span data-ttu-id="d0dcb-129">Där kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-129">It lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> <span data-ttu-id="d0dcb-130">Skapa en funktionsapp med kommandot [az functionapp create](/cli/azure/functionapp#create).</span><span class="sxs-lookup"><span data-stu-id="d0dcb-130">Create a function app by using the [az functionapp create](/cli/azure/functionapp#create) command.</span></span> 

<span data-ttu-id="d0dcb-131">I följande kommando infogar du ditt unika funktionsappnamn istället för platshållaren `<app_name>` och lagringskontonamnet istället för `<storage_name>`.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-131">In the following command, substitute your own unique function app name where you see the `<app_name>` placeholder and the storage account name for  `<storage_name>`.</span></span> <span data-ttu-id="d0dcb-132">`<app_name>` används som DNS-standarddomän för funktionsappen. Därför måste namnet vara unikt bland alla appar i Azure.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-132">The `<app_name>` is used as the default DNS domain for the function app, and so the name needs to be unique across all apps in Azure.</span></span> 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
<span data-ttu-id="d0dcb-133">Som standard skapas en funktionsapp med värdplanen Consumption, vilket innebär att resurser läggs till dynamiskt när de behövs i dina funktioner och att du bara betalar när funktionerna körs.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-133">By default, a function app is created with the Consumption hosting plan, which means that resources are added dynamically as required by your functions and you only pay when functions are running.</span></span> <span data-ttu-id="d0dcb-134">Mer information finns i [Välja rätt värdplan](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="d0dcb-134">For more information, see [Choose the correct hosting plan](functions-scale.md).</span></span> 

<span data-ttu-id="d0dcb-135">När funktionsappen har skapats visas information som liknar följande exempel i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="d0dcb-135">After the function app has been created, the Azure CLI shows information similar to the following example:</span></span>

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

<span data-ttu-id="d0dcb-136">Nu när du har en funktionsapp kan du distribuera den faktiska funktionskoden från GitHub-exempellagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-136">Now that you have a function app, you can deploy the actual function code from the GitHub sample repository.</span></span>

## <a name="deploy-your-function-code"></a><span data-ttu-id="d0dcb-137">Distribuera din funktionskod</span><span class="sxs-lookup"><span data-stu-id="d0dcb-137">Deploy your function code</span></span>  

<span data-ttu-id="d0dcb-138">Det finns flera sätt att skapa funktionskoden i din nya funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-138">There are several ways to create your function code in your new function app.</span></span> <span data-ttu-id="d0dcb-139">I det här ämnet ansluter vi till en exempellagringsplats i GitHub.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-139">This topic connects to a sample repository in GitHub.</span></span> <span data-ttu-id="d0dcb-140">Precis som tidigare ersätter du platshållaren `<app_name>` med namnet på den funktionsapp du skapade.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-140">As before, in the following code replace the `<app_name>` placeholder with the name of the function app you created.</span></span> 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
<span data-ttu-id="d0dcb-141">När distributionskällan har angetts visas information som liknar följande exempel i Azure CLI (nullvärden är borttagna för att öka läsbarheten):</span><span class="sxs-lookup"><span data-stu-id="d0dcb-141">After the deployment source been set, the Azure CLI shows information similar to the following example (null values removed for readability):</span></span>

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

## <a name="test-the-function"></a><span data-ttu-id="d0dcb-142">Testa funktionen</span><span class="sxs-lookup"><span data-stu-id="d0dcb-142">Test the function</span></span>

<span data-ttu-id="d0dcb-143">Använd cURL till att testa den distribuerade funktionen på en Mac- eller Linux-dator, eller Bash i Windows.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-143">Use cURL to test the deployed function on a Mac or Linux computer or using Bash on Windows.</span></span> <span data-ttu-id="d0dcb-144">Kör följande cURL-kommando och ersätt platshållaren `<app_name>` med namnet på din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-144">Execute the following cURL command, replacing the `<app_name>` placeholder with the name of your function app.</span></span> <span data-ttu-id="d0dcb-145">Lägg till frågesträngen `&name=<yourname>` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-145">Append the query string `&name=<yourname>` to the URL.</span></span>

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

<span data-ttu-id="d0dcb-147">Om du inte har cURL tillgängligt på kommandoraden anger du samma webbadress i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-147">If you don't have cURL available in your command line, enter the same URL in the address of your web browser.</span></span> <span data-ttu-id="d0dcb-148">På samma sätt ersätter du platshållaren `<app_name>` med namnet på funktionsappen och lägger till frågesträngen `&name=<yourname>` i webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-148">Again, replace the `<app_name>` placeholder with the name of your function app, and append the query string `&name=<yourname>` to the URL and execute the request.</span></span> 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Funktionssvaret visas i en webbläsare.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a><span data-ttu-id="d0dcb-150">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="d0dcb-150">Clean up resources</span></span>

<span data-ttu-id="d0dcb-151">De andra snabbstarterna i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-151">Other quickstarts in this collection build upon this quickstart.</span></span> <span data-ttu-id="d0dcb-152">Om du planerar att fortsätta att arbeta med efterföljande snabbstarter eller med självstudierna ska du inte rensa resurserna som skapas i denna snabbstart.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-152">If you plan to continue on to work with subsequent quickstarts or with the tutorials, do not clean up the resources created in this quickstart.</span></span> <span data-ttu-id="d0dcb-153">Om du inte planerar att fortsätta kan du använda kommandona nedan för att ta bort alla resurser som har skapats i den här snabbstarten:</span><span class="sxs-lookup"><span data-stu-id="d0dcb-153">If you do not plan to continue, use the following command to delete all resources created by this quickstart:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```
<span data-ttu-id="d0dcb-154">Skriv `y` när du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="d0dcb-154">Type `y` when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0dcb-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0dcb-155">Next steps</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
