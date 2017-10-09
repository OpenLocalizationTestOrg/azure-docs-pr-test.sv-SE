---
title: "aaaAzure CLI skriptexempel - mappa en anpassad domän tooa webbapp | Microsoft Docs"
description: "Skriptexempel Azure CLI - karta en anpassad domän tooa webbapp"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 49d6be092e438a63c0a43e3207080ca4cd5ff3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-web-app"></a><span data-ttu-id="d851e-103">Mappa en anpassad domän tooa webbapp</span><span class="sxs-lookup"><span data-stu-id="d851e-103">Map a custom domain tooa web app</span></span>

<span data-ttu-id="d851e-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan mappa `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="d851e-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d851e-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d851e-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d851e-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="d851e-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d851e-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d851e-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d851e-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d851e-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="d851e-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d851e-109">Script explanation</span></span>

<span data-ttu-id="d851e-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="d851e-110">This script uses hello following commands.</span></span> <span data-ttu-id="d851e-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d851e-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d851e-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="d851e-112">Command</span></span> | <span data-ttu-id="d851e-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d851e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d851e-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="d851e-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d851e-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d851e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d851e-116">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="d851e-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="d851e-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="d851e-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="d851e-118">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="d851e-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="d851e-119">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="d851e-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="d851e-120">Lägg till AZ webapp config värdnamn</span><span class="sxs-lookup"><span data-stu-id="d851e-120">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="d851e-121">Avbildar en anpassad domän tooa webbapp.</span><span class="sxs-lookup"><span data-stu-id="d851e-121">Maps a custom domain tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d851e-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d851e-122">Next steps</span></span>

<span data-ttu-id="d851e-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d851e-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d851e-124">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d851e-124">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
