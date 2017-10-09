---
title: "aaaManage webbprogrammet på Linux med hjälp av Azure CLI 2.0 | Microsoft Docs"
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
ms.openlocfilehash: 5e8e0da8a362450c56d2e87e087f77598ec874ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a><span data-ttu-id="a21da-104">Hantera webbprogrammet på Linux med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a21da-104">Manage Web App on Linux using Azure CLI</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="a21da-105">Hello kommandona i den här artikeln du kan toocreate och hantera ett webbprogram i Linux med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="a21da-105">Using hello commands in this article you are able toocreate and manage a Web App on Linux using Azure CLI 2.0.</span></span>
<span data-ttu-id="a21da-106">Du kan börja använda hello ny version av hello CLI på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a21da-106">You can start using hello new version of hello CLI in two ways:</span></span>

* <span data-ttu-id="a21da-107">[Installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator.</span><span class="sxs-lookup"><span data-stu-id="a21da-107">[Installing Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span>
* <span data-ttu-id="a21da-108">Med hjälp av [Azure-molnet Shell (förhandsgranskning)](../cloud-shell/overview.md)</span><span class="sxs-lookup"><span data-stu-id="a21da-108">Using [Azure Cloud Shell (Preview)](../cloud-shell/overview.md)</span></span>


## <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="a21da-109">Skapa en Linux-Programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="a21da-109">Create a Linux App Service Plan</span></span>

<span data-ttu-id="a21da-110">toocreate en Linux App Service-Plan, kan du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-110">toocreate a Linux App Service Plan, you can use hello following command:</span></span>

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a><span data-ttu-id="a21da-111">Skapa en anpassad dockerbehållare Web App</span><span class="sxs-lookup"><span data-stu-id="a21da-111">Create a custom Docker container Web App</span></span>

<span data-ttu-id="a21da-112">toocreate en webbapp och konfigurerar den toorun anpassade dockerbehållare, du kan använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-112">toocreate a web app and configuring it toorun a custom Docker container, you can use hello following command:</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a><span data-ttu-id="a21da-113">Aktivera loggning för hello Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="a21da-113">Activate hello Docker container logging</span></span>

<span data-ttu-id="a21da-114">tooactivate hello Docker behållare loggning kan du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-114">tooactivate hello Docker container logging, you can use hello following command:</span></span>

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="a21da-115">Ändra hello anpassade dockerbehållare för en befintlig Webbapp på Linux-App</span><span class="sxs-lookup"><span data-stu-id="a21da-115">Change hello custom Docker container for an existing Web App on Linux App</span></span>

<span data-ttu-id="a21da-116">toochange en tidigare skapad app från hello aktuella Docker bild tooa ny avbildning, kan du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-116">toochange a previously created app, from hello current Docker image tooa new image, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a><span data-ttu-id="a21da-117">Använda Docker-avbildningar från ett privat register</span><span class="sxs-lookup"><span data-stu-id="a21da-117">Using Docker images from a private registry</span></span>

<span data-ttu-id="a21da-118">Du kan konfigurera din app toouse bilder från ett privat register.</span><span class="sxs-lookup"><span data-stu-id="a21da-118">You can configure your app toouse images from a private registry.</span></span> <span data-ttu-id="a21da-119">Du behöver tooprovide hello url för ditt registret, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a21da-119">You need tooprovide hello url for your registry, user name, and password.</span></span> <span data-ttu-id="a21da-120">Göra du kan detta med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-120">This can be achieved using hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a><span data-ttu-id="a21da-121">Aktivera kontinuerlig distribution för anpassade Docker-avbildningar</span><span class="sxs-lookup"><span data-stu-id="a21da-121">Enable continuous deployments for custom Docker images</span></span>

<span data-ttu-id="a21da-122">Med följande kommando som du kan aktivera hello hello CD-funktioner och få hello webhooksadressen.</span><span class="sxs-lookup"><span data-stu-id="a21da-122">With hello following command you can enable hello CD functionality, and get hello webhook url.</span></span> <span data-ttu-id="a21da-123">URL: en kan vara används tooconfigure du DockerHub eller Azure-behållare registret repor.</span><span class="sxs-lookup"><span data-stu-id="a21da-123">This url can be used tooconfigure you DockerHub or Azure Container Registry repos.</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a><span data-ttu-id="a21da-124">Skapa ett webbprogram på Linux-App med en av våra inbyggda runtime-ramverk</span><span class="sxs-lookup"><span data-stu-id="a21da-124">Create a Web App on Linux App using one of our built-in runtime frameworks</span></span>

<span data-ttu-id="a21da-125">toocreate ett webbprogram för PHP-5.6 på Linux-App som du kan använda följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="a21da-125">toocreate a PHP 5.6 Web App on Linux App that, you can use hello following command.</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="a21da-126">Ändra framework-version för en befintlig Webbapp på Linux-App</span><span class="sxs-lookup"><span data-stu-id="a21da-126">Change framework version for an existing Web App on Linux App</span></span>

<span data-ttu-id="a21da-127">toochange en tidigare skapad app från hello aktuella framework version tooNode.js 6.11, kan du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-127">toochange a previously created app, from hello current framework version tooNode.js 6.11, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a><span data-ttu-id="a21da-128">Konfigurera Git-distributioner för Webbappen</span><span class="sxs-lookup"><span data-stu-id="a21da-128">Set up Git deployments for your Web App</span></span>

<span data-ttu-id="a21da-129">tooset Git-distributioner för din app kan du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a21da-129">tooset up Git deployments for your app, you can use hello following command:</span></span>

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a><span data-ttu-id="a21da-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a21da-130">Next steps</span></span>
* [<span data-ttu-id="a21da-131">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="a21da-131">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="a21da-132">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a21da-132">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [<span data-ttu-id="a21da-133">Azure-molnet Shell (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="a21da-133">Azure Cloud Shell (Preview)</span></span>](../cloud-shell/overview.md)
* [<span data-ttu-id="a21da-134">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a21da-134">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="a21da-135">Kontinuerlig distribution med Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="a21da-135">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
