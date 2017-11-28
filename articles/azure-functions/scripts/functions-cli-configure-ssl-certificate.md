---
title: "aaaAzure CLI skriptexempel - binda en anpassad funktionsapp för SSL-certifikat tooa | Microsoft Docs"
description: Skriptexempel Azure CLI - bindning en anpassad SSL-certifikat tooa funktionsapp i Azure
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
ms.openlocfilehash: 692dbc03583f2978131823083f1bfd257882664c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-function-app"></a><span data-ttu-id="a4267-103">Binda en anpassad funktionsapp för SSL-certifikat tooa</span><span class="sxs-lookup"><span data-stu-id="a4267-103">Bind a custom SSL certificate tooa function app</span></span>

<span data-ttu-id="a4267-104">Det här exempelskriptet skapar en funktionsapp i App Service med dess relaterade resurser och Binder hello SSL-certifikatet för en anpassad domän namnet tooit.</span><span class="sxs-lookup"><span data-stu-id="a4267-104">This sample script creates a function app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="a4267-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="a4267-105">For this sample, you need:</span></span>

* <span data-ttu-id="a4267-106">Komma åt tooyour domän domänregistrators DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="a4267-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="a4267-107">En giltig. PFX-filen och lösenordet för hello SSL-certifikat som du vill tooupload och bind.</span><span class="sxs-lookup"><span data-stu-id="a4267-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

<span data-ttu-id="a4267-108">toobind ett SSL-certifikat, funktionen appen måste skapas i en apptjänstplan och inte i en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="a4267-108">toobind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a4267-109">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a4267-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a4267-110">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="a4267-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a4267-111">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a4267-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a4267-112">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a4267-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="a4267-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a4267-113">Script explanation</span></span>

<span data-ttu-id="a4267-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="a4267-114">This script uses hello following commands.</span></span> <span data-ttu-id="a4267-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a4267-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a4267-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="a4267-116">Command</span></span> | <span data-ttu-id="a4267-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a4267-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a4267-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="a4267-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a4267-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a4267-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a4267-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="a4267-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="a4267-121">Skapar en App Service krävs toobind SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="a4267-121">Creates an App Service plan required toobind SSL certificates.</span></span> |
| [<span data-ttu-id="a4267-122">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="a4267-122">az functionapp create</span></span>]() | <span data-ttu-id="a4267-123">Skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="a4267-123">Creates a function app.</span></span> |
| [<span data-ttu-id="a4267-124">Lägg till AZ apptjänst web config värdnamn</span><span class="sxs-lookup"><span data-stu-id="a4267-124">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="a4267-125">Avbildar en anpassad domän toohello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="a4267-125">Maps a custom domain toohello function app.</span></span> |
| [<span data-ttu-id="a4267-126">AZ apptjänst web config ssl överför</span><span class="sxs-lookup"><span data-stu-id="a4267-126">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="a4267-127">Överför en SSL-certifikat tooa funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="a4267-127">Uploads an SSL certificate tooa function app.</span></span> |
| [<span data-ttu-id="a4267-128">AZ apptjänst web config ssl-bindning</span><span class="sxs-lookup"><span data-stu-id="a4267-128">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="a4267-129">Binder en överförda funktionsapp för SSL-certifikat tooa.</span><span class="sxs-lookup"><span data-stu-id="a4267-129">Binds an uploaded SSL certificate tooa function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a4267-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4267-130">Next steps</span></span>

<span data-ttu-id="a4267-131">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a4267-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a4267-132">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service]().</span><span class="sxs-lookup"><span data-stu-id="a4267-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation]().</span></span>
