---
title: "Skriptexempel Azure CLI - Övervakare för ett webbprogram med webbserverloggar | Microsoft Docs"
description: "Skriptexempel Azure CLI - Övervakare för ett webbprogram med webbserverloggar"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: df4ca5b1270ada849e231ad9608a5b1d2edda8be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="8af35-103">Övervaka ett webbprogram med webbserverloggar</span><span class="sxs-lookup"><span data-stu-id="8af35-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="8af35-104">I det här scenariot ska du skapa en resursgrupp, apptjänstplan, webbprogram och konfigurera webbappen för att aktivera webbserverloggar.</span><span class="sxs-lookup"><span data-stu-id="8af35-104">In this scenario you will create a resource group, app service plan, web app and configure the web app to enable web server logs.</span></span> <span data-ttu-id="8af35-105">Sedan hämtas loggfilerna för granskning.</span><span class="sxs-lookup"><span data-stu-id="8af35-105">You will then download the log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8af35-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8af35-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8af35-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="8af35-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="8af35-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8af35-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="8af35-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8af35-109">Sample script</span></span>

<span data-ttu-id="8af35-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "övervakaren loggar")]</span><span class="sxs-lookup"><span data-stu-id="8af35-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="8af35-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8af35-111">Script explanation</span></span>

<span data-ttu-id="8af35-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8af35-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="8af35-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8af35-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8af35-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="8af35-114">Command</span></span> | <span data-ttu-id="8af35-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8af35-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8af35-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="8af35-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8af35-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8af35-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8af35-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="8af35-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="8af35-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="8af35-119">Creates an App Service plan.</span></span> <span data-ttu-id="8af35-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="8af35-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="8af35-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="8af35-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="8af35-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="8af35-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="8af35-123">AZ webapp log config</span><span class="sxs-lookup"><span data-stu-id="8af35-123">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="8af35-124">Konfigurerar vilka loggar som finns kvar i en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="8af35-124">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="8af35-125">AZ webapp logga hämtning</span><span class="sxs-lookup"><span data-stu-id="8af35-125">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="8af35-126">Hämtar loggarna för i en Azure-webbapp till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="8af35-126">Downloads the logs of the an Azure web app to your local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8af35-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8af35-127">Next steps</span></span>

<span data-ttu-id="8af35-128">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8af35-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8af35-129">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8af35-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
