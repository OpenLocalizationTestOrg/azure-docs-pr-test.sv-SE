---
title: "aaaAzure CLI skriptexempel - binda ett anpassat webbprogram för SSL-certifikat tooa | Microsoft Docs"
description: "Skriptexempel Azure CLI - Bind ett anpassat webbprogram för SSL-certifikat tooa"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2fec2db84a2007fa6b005776c84d4f8cba392b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="3b747-103">Bind ett anpassat webbprogram för SSL-certifikat tooa</span><span class="sxs-lookup"><span data-stu-id="3b747-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="3b747-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och Binder hello SSL-certifikatet för en anpassad domän namnet tooit.</span><span class="sxs-lookup"><span data-stu-id="3b747-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="3b747-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="3b747-105">For this sample, you will need:</span></span>

* <span data-ttu-id="3b747-106">Komma åt tooyour domän domänregistrators DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="3b747-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="3b747-107">En giltig. PFX-filen och lösenordet för hello SSL-certifikat som du vill tooupload och bind.</span><span class="sxs-lookup"><span data-stu-id="3b747-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3b747-108">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3b747-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3b747-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="3b747-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3b747-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3b747-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="3b747-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="3b747-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="3b747-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="3b747-112">Script explanation</span></span>

<span data-ttu-id="3b747-113">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="3b747-113">This script uses hello following commands.</span></span> <span data-ttu-id="3b747-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3b747-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3b747-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="3b747-115">Command</span></span> | <span data-ttu-id="3b747-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3b747-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3b747-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="3b747-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3b747-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="3b747-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3b747-119">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3b747-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="3b747-120">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3b747-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3b747-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="3b747-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="3b747-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="3b747-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="3b747-123">Lägg till AZ webapp config värdnamn</span><span class="sxs-lookup"><span data-stu-id="3b747-123">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="3b747-124">Avbildar en anpassad domän tooa webbapp.</span><span class="sxs-lookup"><span data-stu-id="3b747-124">Maps a custom domain tooa web app.</span></span> |
| [<span data-ttu-id="3b747-125">AZ webapp config ssl överför</span><span class="sxs-lookup"><span data-stu-id="3b747-125">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="3b747-126">Överför en SSL-certifikat tooa webbapp.</span><span class="sxs-lookup"><span data-stu-id="3b747-126">Uploads an SSL certificate tooa web app.</span></span> |
| [<span data-ttu-id="3b747-127">AZ webapp config ssl-bindning</span><span class="sxs-lookup"><span data-stu-id="3b747-127">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="3b747-128">Binder en överförda SSL-certifikat tooa webbapp.</span><span class="sxs-lookup"><span data-stu-id="3b747-128">Binds an uploaded SSL certificate tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3b747-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b747-129">Next steps</span></span>

<span data-ttu-id="3b747-130">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3b747-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3b747-131">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3b747-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
