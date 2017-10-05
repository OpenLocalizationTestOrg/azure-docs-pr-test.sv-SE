---
title: Skapa en Azure-funktion som ansluter till en Azure Storage | Microsoft Docs
description: Azure CLI-skript Sample - skapa en Azure-funktion som ansluter till en Azure Storage
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: 36dbc2c181c9991a27163e3194800f63c6c0e01e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="4cdca-103">Integrera Funktionsapp i Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="4cdca-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="4cdca-104">Det här exempelskriptet skapar en Funktionsapp och Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4cdca-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4cdca-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4cdca-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4cdca-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="4cdca-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="4cdca-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4cdca-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4cdca-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4cdca-108">Sample script</span></span>

<span data-ttu-id="4cdca-109">Det här exemplet skapar en app i Azure-funktion och lägger till anslutningssträngen för lagring till en appinställning.</span><span class="sxs-lookup"><span data-stu-id="4cdca-109">This sample creates an Azure Function app and adds the storage connection string to an app setting.</span></span>

<span data-ttu-id="4cdca-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "integrera funktionen appen till Azure Storage-konto")]</span><span class="sxs-lookup"><span data-stu-id="4cdca-110">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="4cdca-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4cdca-111">Clean up deployment</span></span>

<span data-ttu-id="4cdca-112">När skriptexempel har körts följande kommando kan användas för att ta bort resursgruppen, App Service-appen och alla relaterade resurser:</span><span class="sxs-lookup"><span data-stu-id="4cdca-112">After the script sample has been run, the following command can be used to remove the resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4cdca-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4cdca-113">Script explanation</span></span>

<span data-ttu-id="4cdca-114">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="4cdca-114">This script uses the following commands.</span></span> <span data-ttu-id="4cdca-115">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4cdca-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4cdca-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="4cdca-116">Command</span></span> | <span data-ttu-id="4cdca-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4cdca-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4cdca-118">AZ inloggning</span><span class="sxs-lookup"><span data-stu-id="4cdca-118">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="4cdca-119">Logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="4cdca-119">Login to Azure.</span></span> |
| [<span data-ttu-id="4cdca-120">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4cdca-120">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4cdca-121">Skapa en resursgrupp med platsen</span><span class="sxs-lookup"><span data-stu-id="4cdca-121">Create a resource group with location</span></span> |
| [<span data-ttu-id="4cdca-122">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="4cdca-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="4cdca-123">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="4cdca-123">Create a storage account</span></span> |
| [<span data-ttu-id="4cdca-124">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="4cdca-124">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="4cdca-125">Skapa en ny funktionsapp</span><span class="sxs-lookup"><span data-stu-id="4cdca-125">Create a new function app</span></span> |
| [<span data-ttu-id="4cdca-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="4cdca-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="4cdca-127">Rensa</span><span class="sxs-lookup"><span data-stu-id="4cdca-127">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4cdca-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cdca-128">Next steps</span></span>

<span data-ttu-id="4cdca-129">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4cdca-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4cdca-130">Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4cdca-130">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
