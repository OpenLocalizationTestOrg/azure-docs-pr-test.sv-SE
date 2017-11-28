---
title: aaaAzure CLI skriptexempel - ansluta en web app tooCosmos DB | Microsoft Docs
description: Azure CLI-skript Sample - Anslut en web app tooCosmos DB
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 1f2123378b9d5812fa793730f7fa5a5bc9ab63c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-toocosmos-db"></a><span data-ttu-id="4f5ef-103">Ansluta en web app tooCosmos DB</span><span class="sxs-lookup"><span data-stu-id="4f5ef-103">Connect a web app tooCosmos DB</span></span>

<span data-ttu-id="4f5ef-104">I det här scenariot du lära dig hur toocreate ett Azure DB som Cosmos-konto och ett Azure web app.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-104">In this scenario you will learn how toocreate an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="4f5ef-105">Du kan länka hello Cosmos DB toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-105">Then you will link hello Cosmos DB toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4f5ef-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4f5ef-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4f5ef-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4f5ef-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4f5ef-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4f5ef-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4f5ef-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4f5ef-110">Script explanation</span></span>

<span data-ttu-id="4f5ef-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, Cosmos-DB och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-111">This script uses hello following commands toocreate a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="4f5ef-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4f5ef-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="4f5ef-113">Command</span></span> | <span data-ttu-id="4f5ef-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4f5ef-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4f5ef-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4f5ef-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4f5ef-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4f5ef-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="4f5ef-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4f5ef-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-118">Creates an App Service plan.</span></span> <span data-ttu-id="4f5ef-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="4f5ef-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="4f5ef-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="4f5ef-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="4f5ef-122">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="4f5ef-122">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="4f5ef-123">Skapar en Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-123">Creates a Cosmos DB account.</span></span> <span data-ttu-id="4f5ef-124">Detta är där hello data ska lagras.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-124">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="4f5ef-125">AZ cosmosdb lista nycklar</span><span class="sxs-lookup"><span data-stu-id="4f5ef-125">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="4f5ef-126">Visar hello snabbtangenter för hello angett Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-126">Lists hello access keys for hello specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="4f5ef-127">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="4f5ef-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="4f5ef-128">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="4f5ef-129">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="4f5ef-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4f5ef-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f5ef-130">Next steps</span></span>

<span data-ttu-id="4f5ef-131">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4f5ef-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4f5ef-132">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4f5ef-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
