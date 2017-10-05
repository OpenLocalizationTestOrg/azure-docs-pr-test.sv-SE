---
title: Skriptexempel Azure CLI - bindning som ett anpassat SSL-certifikatet till en webbapp | Microsoft Docs
description: "Skriptexempel Azure CLI - binda ett SSL-certifikat som är anpassade till en webbapp"
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
ms.openlocfilehash: d4fab3fb2c297bf5f498b63bee46692febb9180b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="536d9-103">Koppla en anpassad SSL-certifikatet till en webbapp</span><span class="sxs-lookup"><span data-stu-id="536d9-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="536d9-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan binda SSL-certifikatet för ett anpassat domännamn till den.</span><span class="sxs-lookup"><span data-stu-id="536d9-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="536d9-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="536d9-105">For this sample, you will need:</span></span>

* <span data-ttu-id="536d9-106">Åtkomst till din domänregistrator DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="536d9-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="536d9-107">En giltig. PFX-filen och lösenordet för SSL-certifikatet som du vill ladda upp och binda.</span><span class="sxs-lookup"><span data-stu-id="536d9-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="536d9-108">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="536d9-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="536d9-109">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="536d9-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="536d9-110">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="536d9-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="536d9-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="536d9-111">Sample script</span></span>

<span data-ttu-id="536d9-112">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "binda ett anpassat SSL-certifikat till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="536d9-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="536d9-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="536d9-113">Script explanation</span></span>

<span data-ttu-id="536d9-114">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="536d9-114">This script uses the following commands.</span></span> <span data-ttu-id="536d9-115">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="536d9-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="536d9-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="536d9-116">Command</span></span> | <span data-ttu-id="536d9-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="536d9-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="536d9-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="536d9-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="536d9-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="536d9-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="536d9-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="536d9-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="536d9-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="536d9-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="536d9-122">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="536d9-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="536d9-123">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="536d9-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="536d9-124">Lägg till AZ webapp config värdnamn</span><span class="sxs-lookup"><span data-stu-id="536d9-124">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="536d9-125">Avbildar en anpassad domän till en webbapp.</span><span class="sxs-lookup"><span data-stu-id="536d9-125">Maps a custom domain to a web app.</span></span> |
| [<span data-ttu-id="536d9-126">AZ webapp config ssl överför</span><span class="sxs-lookup"><span data-stu-id="536d9-126">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="536d9-127">Överför ett SSL-certifikat till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="536d9-127">Uploads an SSL certificate to a web app.</span></span> |
| [<span data-ttu-id="536d9-128">AZ webapp config ssl-bindning</span><span class="sxs-lookup"><span data-stu-id="536d9-128">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="536d9-129">Binder en överförda SSL-certifikat till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="536d9-129">Binds an uploaded SSL certificate to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="536d9-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="536d9-130">Next steps</span></span>

<span data-ttu-id="536d9-131">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="536d9-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="536d9-132">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="536d9-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
