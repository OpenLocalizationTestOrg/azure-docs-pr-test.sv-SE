---
title: "aaaAzure CLI skriptexempel - ansluta ett lagringskonto för web app tooa | Microsoft Docs"
description: Azure CLI-skript Sample - Anslut en web app tooa storage-konto
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: affee2d39ef3f98c6043010850e08b67fb9ce54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a><span data-ttu-id="0bfef-103">Ansluta en web app tooa storage-konto</span><span class="sxs-lookup"><span data-stu-id="0bfef-103">Connect a web app tooa storage account</span></span>

<span data-ttu-id="0bfef-104">I det här scenariot du lära dig hur toocreate ett Azure storage-konto och ett Azure web app.</span><span class="sxs-lookup"><span data-stu-id="0bfef-104">In this scenario you will learn how toocreate an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="0bfef-105">Du kan länka hello storage-konto toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="0bfef-105">Then you will link hello storage account toohello web app using app settings.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0bfef-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0bfef-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="0bfef-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="0bfef-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0bfef-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0bfef-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="0bfef-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0bfef-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="0bfef-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0bfef-110">Script explanation</span></span>

<span data-ttu-id="0bfef-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, storage-konto och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="0bfef-111">This script uses hello following commands toocreate a resource group, web app, storage account and all related resources.</span></span> <span data-ttu-id="0bfef-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0bfef-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0bfef-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="0bfef-113">Command</span></span> | <span data-ttu-id="0bfef-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0bfef-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0bfef-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="0bfef-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0bfef-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="0bfef-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0bfef-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="0bfef-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="0bfef-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0bfef-118">Creates an App Service plan.</span></span> <span data-ttu-id="0bfef-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="0bfef-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="0bfef-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="0bfef-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="0bfef-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="0bfef-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="0bfef-122">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="0bfef-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="0bfef-123">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bfef-123">Creates a storage account.</span></span> <span data-ttu-id="0bfef-124">Detta är där hello statiska tillgångar lagras.</span><span class="sxs-lookup"><span data-stu-id="0bfef-124">This is where hello static assets will be stored.</span></span> |
| [<span data-ttu-id="0bfef-125">AZ konto visa--anslutningssträngen för lagring</span><span class="sxs-lookup"><span data-stu-id="0bfef-125">az storage account show-connection-string</span></span>](https://docs.microsoft.com/cli/azure/storage/account#show-connection-string) | |
| [<span data-ttu-id="0bfef-126">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="0bfef-126">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="0bfef-127">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="0bfef-127">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="0bfef-128">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="0bfef-128">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0bfef-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0bfef-129">Next steps</span></span>

<span data-ttu-id="0bfef-130">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0bfef-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0bfef-131">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0bfef-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
