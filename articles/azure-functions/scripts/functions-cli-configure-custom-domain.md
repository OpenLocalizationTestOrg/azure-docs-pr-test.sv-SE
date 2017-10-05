---
title: "Skriptexempel Azure CLI - karta en anpassad domän till en funktionsapp | Microsoft Docs"
description: "Azure CLI skriptexempel - karta en anpassad domän till en funktion i Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 6fcea6d32f9dd25b0fafb4f895f60d8320ac9df8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a><span data-ttu-id="29461-103">Mappa en anpassad domän till en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="29461-103">Map a custom domain to a function app</span></span>

<span data-ttu-id="29461-104">Det här exempelskriptet skapar en funktionsapp med relaterade resurser och sedan mappa `www.<yourdomain>` till den.</span><span class="sxs-lookup"><span data-stu-id="29461-104">This sample script creates a function app with related resources, and then maps `www.<yourdomain>` to it.</span></span> <span data-ttu-id="29461-105">Om du vill mappa till en anpassad domän, måste appen funktionen skapas i en apptjänstplan och inte i en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="29461-105">To map to a custom domain, your function app must be created in an App Service plan and not in a consumption plan.</span></span> <span data-ttu-id="29461-106">Azure Functions stöder bara mappa en anpassad domän med en A-post.</span><span class="sxs-lookup"><span data-stu-id="29461-106">Azure Functions only supports mapping a custom domain using an A record.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="29461-107">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="29461-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="29461-108">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="29461-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="29461-109">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="29461-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="29461-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="29461-110">Sample script</span></span>

<span data-ttu-id="29461-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "mappa en anpassad domän till en funktionsapp")]</span><span class="sxs-lookup"><span data-stu-id="29461-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="29461-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="29461-112">Script explanation</span></span>

<span data-ttu-id="29461-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="29461-113">This script uses the following commands.</span></span> <span data-ttu-id="29461-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="29461-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="29461-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="29461-115">Command</span></span> | <span data-ttu-id="29461-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="29461-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="29461-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="29461-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="29461-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="29461-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="29461-119">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="29461-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="29461-120">Skapar ett lagringskonto som krävs av funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="29461-120">Creates a storage account required by the function app.</span></span> |
| [<span data-ttu-id="29461-121">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="29461-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="29461-122">Skapar en App Service-plan som behövs för att mappa en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="29461-122">Creates an App Service plan required to map a custom domain.</span></span> |
| [<span data-ttu-id="29461-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="29461-123">az functionapp create</span></span>]() | <span data-ttu-id="29461-124">Skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="29461-124">Creates a function app.</span></span> |
| [<span data-ttu-id="29461-125">Lägg till AZ apptjänst web config värdnamn</span><span class="sxs-lookup"><span data-stu-id="29461-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="29461-126">Avbildar en anpassad domän till en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="29461-126">Maps a custom domain to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="29461-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29461-127">Next steps</span></span>

<span data-ttu-id="29461-128">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="29461-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="29461-129">Ytterligare funktioner CLI skriptexempel finns i den [Azure Functions dokumentationen]().</span><span class="sxs-lookup"><span data-stu-id="29461-129">Additional Functions CLI script samples can be found in the [Azure Functions documentation]().</span></span>
