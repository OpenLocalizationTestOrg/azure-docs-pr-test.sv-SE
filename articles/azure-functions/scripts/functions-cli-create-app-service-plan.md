---
title: "Azure CLI-skript Sample - skapa en Funktionsapp i en apptjänstplan | Microsoft Docs"
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
ms.openlocfilehash: 40c3fa6fa6c07d59e4bf55531e116ba50aa92b91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a><span data-ttu-id="37106-103">Skapa en Funktionsapp i en apptjänstplan</span><span class="sxs-lookup"><span data-stu-id="37106-103">Create a Function App in an App Service plan</span></span>

<span data-ttu-id="37106-104">Det här exempelskriptet skapar en Azure-Funktionsapp, vilket är en behållare för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="37106-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="37106-105">Funktionen appen skapas med hjälp av en dedikerad programtjänstplan, vilket innebär att serverresurserna alltid är aktiva.</span><span class="sxs-lookup"><span data-stu-id="37106-105">The Function App is created using a dedicated App Service plan, which means your server resources are always on.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="37106-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="37106-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="37106-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="37106-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="37106-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="37106-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="37106-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="37106-109">Sample script</span></span>

<span data-ttu-id="37106-110">Det här skriptet skapar en Azure-funktion app med en dedikerad [programtjänstplanen](../functions-scale.md#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="37106-110">This script creates an Azure Function app using a dedicated [App Service plan](../functions-scale.md#app-service-plan).</span></span>

<span data-ttu-id="37106-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "skapa en Azure-funktion på en apptjänstplan")]</span><span class="sxs-lookup"><span data-stu-id="37106-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="37106-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="37106-112">Script explanation</span></span>

<span data-ttu-id="37106-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="37106-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="37106-114">Det här skriptet använder följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="37106-114">This script uses the following commands:</span></span>

| <span data-ttu-id="37106-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="37106-115">Command</span></span> | <span data-ttu-id="37106-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="37106-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="37106-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="37106-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="37106-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="37106-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="37106-119">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="37106-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="37106-120">Skapar ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="37106-120">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="37106-121">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="37106-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appserviceplan#create) | <span data-ttu-id="37106-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="37106-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="37106-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="37106-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#delete) | <span data-ttu-id="37106-124">Skapar en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="37106-124">Creates an Azure Function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="37106-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37106-125">Next steps</span></span>

<span data-ttu-id="37106-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="37106-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="37106-127">Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="37106-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
