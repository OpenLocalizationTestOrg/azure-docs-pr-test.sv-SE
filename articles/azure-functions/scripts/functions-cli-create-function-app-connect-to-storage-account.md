---
title: aaaCreate en Azure-funktion som ansluter tooan Azure Storage | Microsoft Docs
description: Azure CLI-skript Sample - skapa en Azure-funktion som ansluter tooan Azure Storage
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
ms.openlocfilehash: a51a2c17149478eb2d3d0d4034400ed00cd8416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="ccf94-103">Integrera Funktionsapp i Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="ccf94-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="ccf94-104">Det här exempelskriptet skapar en Funktionsapp och Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ccf94-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ccf94-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ccf94-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ccf94-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="ccf94-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ccf94-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ccf94-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ccf94-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ccf94-108">Sample script</span></span>

<span data-ttu-id="ccf94-109">Det här exemplet skapar en app i Azure-funktion och lägger till hello lagring tooan appinställningen för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="ccf94-109">This sample creates an Azure Function app and adds hello storage connection string tooan app setting.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a><span data-ttu-id="ccf94-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ccf94-110">Clean up deployment</span></span>

<span data-ttu-id="ccf94-111">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, App Service-appen och alla relaterade resurser:</span><span class="sxs-lookup"><span data-stu-id="ccf94-111">After hello script sample has been run, hello following command can be used tooremove hello resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ccf94-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ccf94-112">Script explanation</span></span>

<span data-ttu-id="ccf94-113">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="ccf94-113">This script uses hello following commands.</span></span> <span data-ttu-id="ccf94-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ccf94-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ccf94-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="ccf94-115">Command</span></span> | <span data-ttu-id="ccf94-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ccf94-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ccf94-117">AZ inloggning</span><span class="sxs-lookup"><span data-stu-id="ccf94-117">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="ccf94-118">Inloggningen tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ccf94-118">Login tooAzure.</span></span> |
| [<span data-ttu-id="ccf94-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="ccf94-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ccf94-120">Skapa en resursgrupp med platsen</span><span class="sxs-lookup"><span data-stu-id="ccf94-120">Create a resource group with location</span></span> |
| [<span data-ttu-id="ccf94-121">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="ccf94-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="ccf94-122">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ccf94-122">Create a storage account</span></span> |
| [<span data-ttu-id="ccf94-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="ccf94-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="ccf94-124">Skapa en ny funktionsapp</span><span class="sxs-lookup"><span data-stu-id="ccf94-124">Create a new function app</span></span> |
| [<span data-ttu-id="ccf94-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="ccf94-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="ccf94-126">Rensa</span><span class="sxs-lookup"><span data-stu-id="ccf94-126">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ccf94-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccf94-127">Next steps</span></span>

<span data-ttu-id="ccf94-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ccf94-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ccf94-129">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ccf94-129">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
