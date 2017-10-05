---
title: Skriptexempel Azure CLI - bindning som ett anpassat SSL-certifikatet till en funktionsapp | Microsoft Docs
description: Skriptexempel Azure CLI - Bind ett anpassat SSL-certifikatet till en funktionsapp i Azure
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ddabb701d7d5615232d1f6163aa6fb166efe5cb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a><span data-ttu-id="c1d45-103">Bind ett anpassade SSL-certifikat till en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="c1d45-103">Bind a custom SSL certificate to a function app</span></span>

<span data-ttu-id="c1d45-104">Det här exempelskriptet skapar en funktionsapp i App Service med dess relaterade resurser och sedan binda SSL-certifikatet för ett anpassat domännamn till den.</span><span class="sxs-lookup"><span data-stu-id="c1d45-104">This sample script creates a function app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="c1d45-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="c1d45-105">For this sample, you need:</span></span>

* <span data-ttu-id="c1d45-106">Åtkomst till din domänregistrator DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="c1d45-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="c1d45-107">En giltig. PFX-filen och lösenordet för SSL-certifikatet som du vill ladda upp och binda.</span><span class="sxs-lookup"><span data-stu-id="c1d45-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

<span data-ttu-id="c1d45-108">Om du vill binda ett SSL-certifikat, måste funktionen appen skapas i en apptjänstplan och inte i en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="c1d45-108">To bind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c1d45-109">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c1d45-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c1d45-110">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c1d45-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="c1d45-111">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c1d45-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c1d45-112">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c1d45-112">Sample script</span></span>

<span data-ttu-id="c1d45-113">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "binda ett anpassat SSL-certifikat till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="c1d45-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c1d45-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c1d45-114">Script explanation</span></span>

<span data-ttu-id="c1d45-115">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c1d45-115">This script uses the following commands.</span></span> <span data-ttu-id="c1d45-116">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c1d45-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c1d45-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="c1d45-117">Command</span></span> | <span data-ttu-id="c1d45-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c1d45-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c1d45-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="c1d45-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c1d45-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c1d45-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c1d45-121">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="c1d45-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c1d45-122">Skapar en App Service-plan som krävs för att binda SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="c1d45-122">Creates an App Service plan required to bind SSL certificates.</span></span> |
| [<span data-ttu-id="c1d45-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="c1d45-123">az functionapp create</span></span>]() | <span data-ttu-id="c1d45-124">Skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c1d45-124">Creates a function app.</span></span> |
| [<span data-ttu-id="c1d45-125">Lägg till AZ apptjänst web config värdnamn</span><span class="sxs-lookup"><span data-stu-id="c1d45-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="c1d45-126">Mappar en anpassad domän för funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="c1d45-126">Maps a custom domain to the function app.</span></span> |
| [<span data-ttu-id="c1d45-127">AZ apptjänst web config ssl överför</span><span class="sxs-lookup"><span data-stu-id="c1d45-127">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="c1d45-128">Överför ett SSL-certifikat till en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c1d45-128">Uploads an SSL certificate to a function app.</span></span> |
| [<span data-ttu-id="c1d45-129">AZ apptjänst web config ssl-bindning</span><span class="sxs-lookup"><span data-stu-id="c1d45-129">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="c1d45-130">Binder en överförda SSL-certifikatet till en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c1d45-130">Binds an uploaded SSL certificate to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c1d45-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1d45-131">Next steps</span></span>

<span data-ttu-id="c1d45-132">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1d45-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c1d45-133">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service]().</span><span class="sxs-lookup"><span data-stu-id="c1d45-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation]().</span></span>
