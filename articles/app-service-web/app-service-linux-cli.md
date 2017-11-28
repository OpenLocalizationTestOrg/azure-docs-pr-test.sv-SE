---
title: "Hantera webbprogrammet på Linux med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Hantera webbprogrammet på Linux med hjälp av Azure CLI."
keywords: "Azure apptjänst, webbprogram, cli, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 04aceecf0cb4cad5c838b7254bf7079a36bbd0d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a><span data-ttu-id="3a1e5-104">Hantera webbprogrammet på Linux med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3a1e5-104">Manage Web App on Linux using Azure CLI</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="3a1e5-105">Kommandona i den här artikeln ska du kunna skapa och hantera ett webbprogram i Linux med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-105">Using the commands in this article you are able to create and manage a Web App on Linux using Azure CLI 2.0.</span></span>
<span data-ttu-id="3a1e5-106">Du kan börja använda den nya versionen av CLI på två sätt:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-106">You can start using the new version of the CLI in two ways:</span></span>

* <span data-ttu-id="3a1e5-107">[Installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-107">[Installing Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span>
* <span data-ttu-id="3a1e5-108">Med hjälp av [Azure-molnet Shell (förhandsgranskning)](../cloud-shell/overview.md)</span><span class="sxs-lookup"><span data-stu-id="3a1e5-108">Using [Azure Cloud Shell (Preview)](../cloud-shell/overview.md)</span></span>


## <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="3a1e5-109">Skapa en Linux-Programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3a1e5-109">Create a Linux App Service Plan</span></span>

<span data-ttu-id="3a1e5-110">Om du vill skapa en Linux App Service-Plan kan du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-110">To create a Linux App Service Plan, you can use the following command:</span></span>

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a><span data-ttu-id="3a1e5-111">Skapa en anpassad dockerbehållare Web App</span><span class="sxs-lookup"><span data-stu-id="3a1e5-111">Create a custom Docker container Web App</span></span>

<span data-ttu-id="3a1e5-112">För att skapa en webbapp och konfigurerar den för att köra en anpassad dockerbehållare, kan du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-112">To create a web app and configuring it to run a custom Docker container, you can use the following command:</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-the-docker-container-logging"></a><span data-ttu-id="3a1e5-113">Aktivera loggning för Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="3a1e5-113">Activate the Docker container logging</span></span>

<span data-ttu-id="3a1e5-114">Du kan använda följande kommando för att aktivera loggning för Docker-behållare:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-114">To activate the Docker container logging, you can use the following command:</span></span>

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-the-custom-docker-container-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="3a1e5-115">Ändra den anpassade dockerbehållare för en befintlig Webbapp på Linux-App</span><span class="sxs-lookup"><span data-stu-id="3a1e5-115">Change the custom Docker container for an existing Web App on Linux App</span></span>

<span data-ttu-id="3a1e5-116">Om du vill ändra en tidigare skapad app från den aktuella Docker-avbildningen till en ny avbildning kan du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-116">To change a previously created app, from the current Docker image to a new image, you can use the following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a><span data-ttu-id="3a1e5-117">Använda Docker-avbildningar från ett privat register</span><span class="sxs-lookup"><span data-stu-id="3a1e5-117">Using Docker images from a private registry</span></span>

<span data-ttu-id="3a1e5-118">Du kan konfigurera din app att använda bilder från ett privat register.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-118">You can configure your app to use images from a private registry.</span></span> <span data-ttu-id="3a1e5-119">Du måste ange en url för ditt registret, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-119">You need to provide the url for your registry, user name, and password.</span></span> <span data-ttu-id="3a1e5-120">Göra du kan detta med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-120">This can be achieved using the following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a><span data-ttu-id="3a1e5-121">Aktivera kontinuerlig distribution för anpassade Docker-avbildningar</span><span class="sxs-lookup"><span data-stu-id="3a1e5-121">Enable continuous deployments for custom Docker images</span></span>

<span data-ttu-id="3a1e5-122">Med det här kommandot kan du aktivera funktionen för CD och få webhooksadressen.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-122">With the following command you can enable the CD functionality, and get the webhook url.</span></span> <span data-ttu-id="3a1e5-123">URL: en kan användas för att konfigurera DockerHub eller Azure-behållare registret repor.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-123">This url can be used to configure you DockerHub or Azure Container Registry repos.</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a><span data-ttu-id="3a1e5-124">Skapa ett webbprogram på Linux-App med en av våra inbyggda runtime-ramverk</span><span class="sxs-lookup"><span data-stu-id="3a1e5-124">Create a Web App on Linux App using one of our built-in runtime frameworks</span></span>

<span data-ttu-id="3a1e5-125">Du kan använda följande kommando för att skapa ett webbprogram för PHP 5.6 på Linux-App som.</span><span class="sxs-lookup"><span data-stu-id="3a1e5-125">To create a PHP 5.6 Web App on Linux App that, you can use the following command.</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="3a1e5-126">Ändra framework-version för en befintlig Webbapp på Linux-App</span><span class="sxs-lookup"><span data-stu-id="3a1e5-126">Change framework version for an existing Web App on Linux App</span></span>

<span data-ttu-id="3a1e5-127">Om du vill ändra en tidigare skapad app från den aktuella framework-versionen till Node.js 6.11 kan du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-127">To change a previously created app, from the current framework version to Node.js 6.11, you can use the following command:</span></span>

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a><span data-ttu-id="3a1e5-128">Konfigurera Git-distributioner för Webbappen</span><span class="sxs-lookup"><span data-stu-id="3a1e5-128">Set up Git deployments for your Web App</span></span>

<span data-ttu-id="3a1e5-129">Om du vill konfigurera Git-distributioner för din app kan du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1e5-129">To set up Git deployments for your app, you can use the following command:</span></span>

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a><span data-ttu-id="3a1e5-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a1e5-130">Next steps</span></span>
* [<span data-ttu-id="3a1e5-131">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="3a1e5-131">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="3a1e5-132">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3a1e5-132">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [<span data-ttu-id="3a1e5-133">Azure-molnet Shell (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="3a1e5-133">Azure Cloud Shell (Preview)</span></span>](../cloud-shell/overview.md)
* [<span data-ttu-id="3a1e5-134">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3a1e5-134">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="3a1e5-135">Kontinuerlig distribution med Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="3a1e5-135">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
