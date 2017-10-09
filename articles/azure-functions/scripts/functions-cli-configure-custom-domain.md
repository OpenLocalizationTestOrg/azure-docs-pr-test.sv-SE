---
title: "aaaAzure CLI skriptexempel - mappa en anpassad domän tooa funktionsapp | Microsoft Docs"
description: "Azure CLI skriptexempel - karta en anpassad domän tooa funktionsapp i Azure."
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
ms.openlocfilehash: c7cb0a3e132b491250623b945aecf6aea4f57c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-function-app"></a><span data-ttu-id="1844c-103">Mappa en anpassad domän tooa funktionsapp</span><span class="sxs-lookup"><span data-stu-id="1844c-103">Map a custom domain tooa function app</span></span>

<span data-ttu-id="1844c-104">Det här exempelskriptet skapar en funktionsapp med relaterade resurser och sedan mappa `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="1844c-104">This sample script creates a function app with related resources, and then maps `www.<yourdomain>` tooit.</span></span> <span data-ttu-id="1844c-105">toomap tooa anpassad domän, appen funktionen måste skapas i en apptjänstplan och inte i en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="1844c-105">toomap tooa custom domain, your function app must be created in an App Service plan and not in a consumption plan.</span></span> <span data-ttu-id="1844c-106">Azure Functions stöder bara mappa en anpassad domän med en A-post.</span><span class="sxs-lookup"><span data-stu-id="1844c-106">Azure Functions only supports mapping a custom domain using an A record.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1844c-107">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1844c-107">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1844c-108">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="1844c-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1844c-109">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1844c-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="1844c-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1844c-110">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="1844c-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1844c-111">Script explanation</span></span>

<span data-ttu-id="1844c-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="1844c-112">This script uses hello following commands.</span></span> <span data-ttu-id="1844c-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1844c-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1844c-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="1844c-114">Command</span></span> | <span data-ttu-id="1844c-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1844c-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1844c-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="1844c-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1844c-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="1844c-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1844c-118">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="1844c-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="1844c-119">Skapar ett lagringskonto som krävs av hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="1844c-119">Creates a storage account required by hello function app.</span></span> |
| [<span data-ttu-id="1844c-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="1844c-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="1844c-121">Skapar en anpassad domän för toomap en Apptjänst-planen som krävs.</span><span class="sxs-lookup"><span data-stu-id="1844c-121">Creates an App Service plan required toomap a custom domain.</span></span> |
| [<span data-ttu-id="1844c-122">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="1844c-122">az functionapp create</span></span>]() | <span data-ttu-id="1844c-123">Skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="1844c-123">Creates a function app.</span></span> |
| [<span data-ttu-id="1844c-124">Lägg till AZ apptjänst web config värdnamn</span><span class="sxs-lookup"><span data-stu-id="1844c-124">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="1844c-125">Avbildar en anpassad domän tooa funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="1844c-125">Maps a custom domain tooa function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1844c-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1844c-126">Next steps</span></span>

<span data-ttu-id="1844c-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1844c-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1844c-128">Ytterligare funktioner CLI skriptexempel finns i hello [Azure Functions dokumentationen]().</span><span class="sxs-lookup"><span data-stu-id="1844c-128">Additional Functions CLI script samples can be found in hello [Azure Functions documentation]().</span></span>
