---
title: "aaaContinuous distribution med Azure Web App på Linux | Microsoft Docs"
description: "Hur toosetup kontinuerlig distribution i Azure Web App på Linux."
keywords: "Azure apptjänst, linux, oss, acr"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f94d837e27605da58428f507ab2b0eb3af3297e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a><span data-ttu-id="2a7a1-104">Kontinuerlig distribution med Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="2a7a1-104">Continuous deployment with Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="2a7a1-105">I kursen får du konfigurera kontinuerlig distribution för en avbildning för anpassade container från hanterade [Azure Container registret](https://azure.microsoft.com/en-us/services/container-registry/) databaser eller [Docker-hubb](https://hub.docker.com).</span><span class="sxs-lookup"><span data-stu-id="2a7a1-105">In this tutorial, you configure continuous deployment for a custom container image from Managed [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) repositories or [Docker Hub](https://hub.docker.com).</span></span>

## <a name="step-1---sign-in-tooazure"></a><span data-ttu-id="2a7a1-106">Steg 1 – Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="2a7a1-106">Step 1 - Sign in tooAzure</span></span>

<span data-ttu-id="2a7a1-107">Logga in toohello Azure-portalen på http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="2a7a1-107">Sign in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---enable-container-continuous-deployment-feature"></a><span data-ttu-id="2a7a1-108">Steg 2 – aktivera funktionen för behållaren kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2a7a1-108">Step 2 - Enable container continuous deployment feature</span></span>

<span data-ttu-id="2a7a1-109">Du kan aktivera funktionen-hello kontinuerlig distribution med [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) och köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="2a7a1-109">You can enable hello continuous deployment feature using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

<span data-ttu-id="2a7a1-110">I hello  **[Azure-portalen](https://portal.azure.com/)**, klicka på hello **Apptjänst** alternativet hello vänster hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-110">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="2a7a1-111">Klicka på hello namnet på din app som du vill tooconfigure Docker-hubb kontinuerlig distribution för.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-111">Click on hello name of your app that you want tooconfigure Docker Hub continuous deployment for.</span></span>

<span data-ttu-id="2a7a1-112">I hello **appinställningar**, Lägg till en app inställningen kallas `DOCKER_ENABLE_CI` med hello värdet `true`.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-112">In hello **App settings**, add an app setting called `DOCKER_ENABLE_CI` with hello value `true`.</span></span>

![infoga bilden app-inställning](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a><span data-ttu-id="2a7a1-114">Steg 3 – Förbereda Webhooksadressen</span><span class="sxs-lookup"><span data-stu-id="2a7a1-114">Step 3 - Prepare Webhook URL</span></span>

<span data-ttu-id="2a7a1-115">Du kan hämta hello Webhooksadressen med [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) och köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="2a7a1-115">You can obtain hello Webhook URL using [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) and executing hello following command</span></span>

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

<span data-ttu-id="2a7a1-116">För hello Webhooksadressen, behöver du toohave hello följande slutpunkt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-116">For hello Webhook URL, you need toohave hello following endpoint: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.</span></span>

<span data-ttu-id="2a7a1-117">Du kan hämta din `publishingusername` och `publishingpwd` genom att hämta hello webbprogrammet publiceringsprofil med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-117">You can obtain your `publishingusername` and `publishingpwd` by downloading hello web app publish profile using hello Azure portal.</span></span>

![infoga bilden för att lägga till webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a><span data-ttu-id="2a7a1-119">Steg 4 – Lägg till en webbtjänst hook</span><span class="sxs-lookup"><span data-stu-id="2a7a1-119">Step 4 - Add a web hook</span></span>

### <a name="azure-container-registry"></a><span data-ttu-id="2a7a1-120">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="2a7a1-120">Azure Container Registry</span></span>

<span data-ttu-id="2a7a1-121">I din registret portalbladet klickar du på **Webhooks**, klicka på Skapa en ny webhook **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-121">In your registry portal blade, click **Webhooks**, create a new webhook by clicking **Add**.</span></span> <span data-ttu-id="2a7a1-122">I hello **skapa webhook** bladet namnge din webhooken.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-122">In hello **Create webhook** blade, give your webhook a name.</span></span> <span data-ttu-id="2a7a1-123">För hello Webhook URI, behöver du tooprovide hello URL från **steg3**</span><span class="sxs-lookup"><span data-stu-id="2a7a1-123">For hello Webhook URI, you need tooprovide hello URL obtained from **Step 3**</span></span>

<span data-ttu-id="2a7a1-124">Kontrollera att du har definierat hello scope som hello lagringsplatsen som innehåller bilden behållare.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-124">Make sure that you define hello scope as hello repo that contains your container image.</span></span>

![Infoga bild av webhooken](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

<span data-ttu-id="2a7a1-126">När hello avbildningen uppdateras hämta hello webbprogrammet uppdateras automatiskt med hello ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-126">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

### <a name="docker-hub"></a><span data-ttu-id="2a7a1-127">Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="2a7a1-127">Docker Hub</span></span>

<span data-ttu-id="2a7a1-128">På din hubb Docker klickar du på **Webhooks**, sedan **skapa en WEBHOOK**.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-128">In your Docker Hub page, click **Webhooks**, then **CREATE A WEBHOOK**.</span></span>

![infoga bilden för att lägga till webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

<span data-ttu-id="2a7a1-130">För hello Webhooksadressen, behöver du tooprovide hello URL från **steg3**</span><span class="sxs-lookup"><span data-stu-id="2a7a1-130">For hello Webhook URL, you need tooprovide hello URL obtained from **Step 3**</span></span>

![infoga bilden för att lägga till webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

<span data-ttu-id="2a7a1-132">När hello avbildningen uppdateras hämta hello webbprogrammet uppdateras automatiskt med hello ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="2a7a1-132">When hello image gets updated, hello web app get updated automatically with hello new image.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a7a1-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a7a1-133">Next steps</span></span>
* [<span data-ttu-id="2a7a1-134">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="2a7a1-134">What is Azure Web App on Linux?</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="2a7a1-135">Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="2a7a1-135">Azure Container Registry</span></span>](https://azure.microsoft.com/en-us/services/container-registry/)
* [<span data-ttu-id="2a7a1-136">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="2a7a1-136">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="2a7a1-137">Med .NET Core i Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="2a7a1-137">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="2a7a1-138">Med hjälp av Ruby i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="2a7a1-138">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="2a7a1-139">Hur toouse anpassade Docker bild för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="2a7a1-139">How toouse a custom Docker image for Azure Web App on Linux</span></span>](./app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="2a7a1-140">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="2a7a1-140">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md) 
* [<span data-ttu-id="2a7a1-141">Hantera webbprogrammet på Linux med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2a7a1-141">Manage Web App on Linux using Azure CLI 2.0</span></span>](./app-service-linux-cli.md)



