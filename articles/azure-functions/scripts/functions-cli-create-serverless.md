---
title: "aaaAzure CLI skriptexempel - skapa en Funktionsapp för serverlösa körning | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Funktionsapp för serverlösa körning"
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
ms.openlocfilehash: 7ec872b642e50827896a73a9e43bcc87233e15c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a><span data-ttu-id="14399-103">Skapa en Funktionsapp för serverlösa körning</span><span class="sxs-lookup"><span data-stu-id="14399-103">Create a Function App for serverless execution</span></span>

<span data-ttu-id="14399-104">Det här exempelskriptet skapar en Azure-Funktionsapp, vilket är en behållare för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="14399-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="14399-105">Hej Funktionsapp skapas med hjälp av hello [förbrukning plan](../functions-scale.md#consumption-plan), vilket är idealiskt för händelsedriven serverlösa arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="14399-105">hello Function App is created using hello [consumption plan](../functions-scale.md#consumption-plan), which is ideal for event-driven serverless workloads.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="14399-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="14399-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="14399-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="14399-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="14399-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="14399-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="14399-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="14399-109">Sample script</span></span>

<span data-ttu-id="14399-110">Det här skriptet skapar en Azure-funktion-app med hello [förbrukning plan](../functions-scale.md#consumption-plan).</span><span class="sxs-lookup"><span data-stu-id="14399-110">This script creates an Azure Function app using hello [consumption plan](../functions-scale.md#consumption-plan).</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="14399-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="14399-111">Script explanation</span></span>

<span data-ttu-id="14399-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="14399-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="14399-113">Det här skriptet använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="14399-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="14399-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="14399-114">Command</span></span> | <span data-ttu-id="14399-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="14399-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="14399-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="14399-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="14399-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="14399-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="14399-118">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="14399-118">az storage account create</span></span>](/cli/azure/storage/account#create) | <span data-ttu-id="14399-119">Skapar ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="14399-119">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="14399-120">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="14399-120">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="14399-121">Skapar en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="14399-121">Creates an Azure Function.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="14399-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14399-122">Next steps</span></span>

<span data-ttu-id="14399-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="14399-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="14399-124">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="14399-124">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
