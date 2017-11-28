---
title: "aaaAzure CLI skriptexempel - skapa en Funktionsapp i en apptjänstplan | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Funktionsapp i en apptjänstplan"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c0ffbbbf022e5680e5ae3141e784e7c7bced0bc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a><span data-ttu-id="fdc32-103">Skapa en Funktionsapp i en apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="fdc32-103">Create a Function App in an App Service plan</span></span>

<span data-ttu-id="fdc32-104">Det här exempelskriptet skapar en Azure-Funktionsapp, vilket är en behållare för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="fdc32-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="fdc32-105">Hej Funktionsapp skapas med hjälp av en dedikerad programtjänstplan, vilket innebär att serverresurserna alltid är aktiva.</span><span class="sxs-lookup"><span data-stu-id="fdc32-105">hello Function App is created using a dedicated App Service plan, which means your server resources are always on.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fdc32-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fdc32-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fdc32-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="fdc32-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fdc32-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fdc32-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="fdc32-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="fdc32-109">Sample script</span></span>

<span data-ttu-id="fdc32-110">Det här skriptet skapar en Azure-funktion app med en dedikerad [programtjänstplanen](../functions-scale.md#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="fdc32-110">This script creates an Azure Function app using a dedicated [App Service plan](../functions-scale.md#app-service-plan).</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="fdc32-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="fdc32-111">Script explanation</span></span>

<span data-ttu-id="fdc32-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="fdc32-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="fdc32-113">Det här skriptet använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="fdc32-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="fdc32-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="fdc32-114">Command</span></span> | <span data-ttu-id="fdc32-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="fdc32-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fdc32-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="fdc32-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="fdc32-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="fdc32-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fdc32-118">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="fdc32-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="fdc32-119">Skapar ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fdc32-119">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="fdc32-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="fdc32-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appserviceplan#create) | <span data-ttu-id="fdc32-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="fdc32-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="fdc32-122">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="fdc32-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#delete) | <span data-ttu-id="fdc32-123">Skapar en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc32-123">Creates an Azure Function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fdc32-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fdc32-124">Next steps</span></span>

<span data-ttu-id="fdc32-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fdc32-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="fdc32-126">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fdc32-126">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
