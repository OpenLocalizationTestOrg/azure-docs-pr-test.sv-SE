---
title: "aaaAzure CLI skriptexempel - övervaka ett webbprogram med webbserverloggar | Microsoft Docs"
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
ms.openlocfilehash: 012b800c03af77387bed3d098fae3c96d82aee29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="70463-103">Övervaka ett webbprogram med webbserverloggar</span><span class="sxs-lookup"><span data-stu-id="70463-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="70463-104">I det här scenariot ska du skapa en resursgrupp, apptjänstplan, webbprogram och konfigurerar hello web app tooenable webbserverloggar.</span><span class="sxs-lookup"><span data-stu-id="70463-104">In this scenario you will create a resource group, app service plan, web app and configure hello web app tooenable web server logs.</span></span> <span data-ttu-id="70463-105">Sedan hämtas hello loggfiler för granskning.</span><span class="sxs-lookup"><span data-stu-id="70463-105">You will then download hello log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="70463-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="70463-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="70463-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="70463-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="70463-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="70463-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="70463-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="70463-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="70463-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="70463-110">Script explanation</span></span>

<span data-ttu-id="70463-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="70463-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="70463-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="70463-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="70463-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="70463-113">Command</span></span> | <span data-ttu-id="70463-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="70463-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="70463-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="70463-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="70463-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="70463-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="70463-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="70463-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="70463-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="70463-118">Creates an App Service plan.</span></span> <span data-ttu-id="70463-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="70463-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="70463-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="70463-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="70463-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="70463-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="70463-122">AZ webapp log config</span><span class="sxs-lookup"><span data-stu-id="70463-122">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="70463-123">Konfigurerar vilka loggar som finns kvar i en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="70463-123">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="70463-124">AZ webapp logga hämtning</span><span class="sxs-lookup"><span data-stu-id="70463-124">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="70463-125">Hämtningar hello loggar av hello en Azure web app tooyour lokala dator.</span><span class="sxs-lookup"><span data-stu-id="70463-125">Downloads hello logs of hello an Azure web app tooyour local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="70463-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70463-126">Next steps</span></span>

<span data-ttu-id="70463-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="70463-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="70463-128">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="70463-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
