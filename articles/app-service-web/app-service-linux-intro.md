---
title: "aaaIntroduction tooAzure webbprogrammet på Linux | Microsoft Docs"
description: "Läs mer om Azure-Webbapp på Linux."
keywords: "Azure apptjänst, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a><span data-ttu-id="eda07-104">Introduktion tooAzure webbprogrammet på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-104">Introduction tooAzure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="eda07-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="eda07-105">Overview</span></span>
<span data-ttu-id="eda07-106">Kunder kan använda Web App i Linux toohost webbprogram internt på Linux för stackar för program som stöds.</span><span class="sxs-lookup"><span data-stu-id="eda07-106">Customers can use Web App on Linux toohost web apps natively on Linux for supported application stacks.</span></span> <span data-ttu-id="eda07-107">hello innehåller följande avsnitt hello programmet stackar som stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="eda07-107">hello following section lists hello application stacks that are currently supported.</span></span> 

## <a name="features"></a><span data-ttu-id="eda07-108">Funktioner</span><span class="sxs-lookup"><span data-stu-id="eda07-108">Features</span></span>
<span data-ttu-id="eda07-109">Web App på Linux stöder för närvarande följande program stackar hello:</span><span class="sxs-lookup"><span data-stu-id="eda07-109">Web App on Linux currently supports hello following application stacks:</span></span>

* <span data-ttu-id="eda07-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="eda07-110">Node.js</span></span>
    * <span data-ttu-id="eda07-111">4.4</span><span class="sxs-lookup"><span data-stu-id="eda07-111">4.4</span></span>
    * <span data-ttu-id="eda07-112">4.5</span><span class="sxs-lookup"><span data-stu-id="eda07-112">4.5</span></span>
    * <span data-ttu-id="eda07-113">6.2</span><span class="sxs-lookup"><span data-stu-id="eda07-113">6.2</span></span>
    * <span data-ttu-id="eda07-114">6.6</span><span class="sxs-lookup"><span data-stu-id="eda07-114">6.6</span></span>
    * <span data-ttu-id="eda07-115">6.9</span><span class="sxs-lookup"><span data-stu-id="eda07-115">6.9</span></span>
    * <span data-ttu-id="eda07-116">6.10</span><span class="sxs-lookup"><span data-stu-id="eda07-116">6.10</span></span>
    * <span data-ttu-id="eda07-117">6.11</span><span class="sxs-lookup"><span data-stu-id="eda07-117">6.11</span></span>
    * <span data-ttu-id="eda07-118">8.0</span><span class="sxs-lookup"><span data-stu-id="eda07-118">8.0</span></span>
    * <span data-ttu-id="eda07-119">8.1</span><span class="sxs-lookup"><span data-stu-id="eda07-119">8.1</span></span>
* <span data-ttu-id="eda07-120">PHP</span><span class="sxs-lookup"><span data-stu-id="eda07-120">PHP</span></span>
    * <span data-ttu-id="eda07-121">5.6</span><span class="sxs-lookup"><span data-stu-id="eda07-121">5.6</span></span>
    * <span data-ttu-id="eda07-122">7.0</span><span class="sxs-lookup"><span data-stu-id="eda07-122">7.0</span></span>
* <span data-ttu-id="eda07-123">.NET core</span><span class="sxs-lookup"><span data-stu-id="eda07-123">.Net Core</span></span>
    * <span data-ttu-id="eda07-124">1.0</span><span class="sxs-lookup"><span data-stu-id="eda07-124">1.0</span></span>
    * <span data-ttu-id="eda07-125">1.1</span><span class="sxs-lookup"><span data-stu-id="eda07-125">1.1</span></span>
* <span data-ttu-id="eda07-126">Ruby</span><span class="sxs-lookup"><span data-stu-id="eda07-126">Ruby</span></span>
    * <span data-ttu-id="eda07-127">2.3</span><span class="sxs-lookup"><span data-stu-id="eda07-127">2.3</span></span>

<span data-ttu-id="eda07-128">Kunder kan distribuera sina program med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="eda07-128">Customers can deploy their applications by using:</span></span>

* <span data-ttu-id="eda07-129">FTP</span><span class="sxs-lookup"><span data-stu-id="eda07-129">FTP</span></span>
* <span data-ttu-id="eda07-130">Lokal Git</span><span class="sxs-lookup"><span data-stu-id="eda07-130">Local Git</span></span>
* <span data-ttu-id="eda07-131">GitHub</span><span class="sxs-lookup"><span data-stu-id="eda07-131">GitHub</span></span>
* <span data-ttu-id="eda07-132">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="eda07-132">Bitbucket</span></span>

<span data-ttu-id="eda07-133">För att skala program:</span><span class="sxs-lookup"><span data-stu-id="eda07-133">For application scaling:</span></span>

* <span data-ttu-id="eda07-134">Kunder kan skala webbprogram uppåt och nedåt genom att ändra hello nivå av deras programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="eda07-134">Customers can scale web apps up and down by changing hello tier of their App Service plan</span></span>
* <span data-ttu-id="eda07-135">Kunder kan skala ut program och kör appen instanser inom gränserna för hello av deras SKU</span><span class="sxs-lookup"><span data-stu-id="eda07-135">Customers can scale out applications and run multiple app instances within hello confines of their SKU</span></span>

<span data-ttu-id="eda07-136">För Kudu del av hello grundläggande funktioner:</span><span class="sxs-lookup"><span data-stu-id="eda07-136">For Kudu, some of hello basic functionality:</span></span>

* <span data-ttu-id="eda07-137">Miljöer</span><span class="sxs-lookup"><span data-stu-id="eda07-137">Environments</span></span>
* <span data-ttu-id="eda07-138">Distributioner</span><span class="sxs-lookup"><span data-stu-id="eda07-138">Deployments</span></span>
* <span data-ttu-id="eda07-139">Basic-konsolen</span><span class="sxs-lookup"><span data-stu-id="eda07-139">Basic console</span></span>
* <span data-ttu-id="eda07-140">SSH</span><span class="sxs-lookup"><span data-stu-id="eda07-140">SSH</span></span>

<span data-ttu-id="eda07-141">För devops:</span><span class="sxs-lookup"><span data-stu-id="eda07-141">For devops:</span></span>

* <span data-ttu-id="eda07-142">Mellanlagringsmiljöer</span><span class="sxs-lookup"><span data-stu-id="eda07-142">Staging environments</span></span>
* <span data-ttu-id="eda07-143">ACR och DockerHub CI/CD</span><span class="sxs-lookup"><span data-stu-id="eda07-143">ACR and DockerHub CI/CD</span></span>

## <a name="limitations"></a><span data-ttu-id="eda07-144">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="eda07-144">Limitations</span></span>
<span data-ttu-id="eda07-145">hello Azure-portalen visar endast funktioner som för närvarande stöds för webbprogrammet på Linux och döljer hello rest.</span><span class="sxs-lookup"><span data-stu-id="eda07-145">hello Azure portal shows only features that currently work for Web App on Linux and hides hello rest.</span></span> <span data-ttu-id="eda07-146">Som vi aktivera flera funktioner som visas de på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="eda07-146">As we enable more features, they will be visible on hello portal.</span></span>

<span data-ttu-id="eda07-147">Vissa funktioner, till exempel virtuell nätverksintegration, Azure Active Directory/autentiseringsmetoder från tredje part eller Kudu webbplatstillägg är inte tillgängliga ännu.</span><span class="sxs-lookup"><span data-stu-id="eda07-147">Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, are not available yet.</span></span> <span data-ttu-id="eda07-148">När dessa funktioner är tillgängliga, kommer vi uppdatera våra dokumentationen och bloggen om hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="eda07-148">Once these features are available, we will update our documentation and blog about hello changes.</span></span>

<span data-ttu-id="eda07-149">Den här förhandsversion finns för närvarande bara i hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="eda07-149">This public preview is currently only available in hello following regions:</span></span>

* <span data-ttu-id="eda07-150">Västra USA</span><span class="sxs-lookup"><span data-stu-id="eda07-150">West US</span></span>
* <span data-ttu-id="eda07-151">Östra USA</span><span class="sxs-lookup"><span data-stu-id="eda07-151">East US</span></span>
* <span data-ttu-id="eda07-152">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="eda07-152">West Europe</span></span>
* <span data-ttu-id="eda07-153">Norra Europa</span><span class="sxs-lookup"><span data-stu-id="eda07-153">North Europe</span></span>
* <span data-ttu-id="eda07-154">Södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="eda07-154">South Central US</span></span>
* <span data-ttu-id="eda07-155">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="eda07-155">North Central US</span></span>
* <span data-ttu-id="eda07-156">Sydostasien</span><span class="sxs-lookup"><span data-stu-id="eda07-156">Southeast Asia</span></span>
* <span data-ttu-id="eda07-157">Östasien</span><span class="sxs-lookup"><span data-stu-id="eda07-157">East Asia</span></span>
* <span data-ttu-id="eda07-158">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="eda07-158">Australia East</span></span>
* <span data-ttu-id="eda07-159">Östra Japan</span><span class="sxs-lookup"><span data-stu-id="eda07-159">Japan East</span></span>
* <span data-ttu-id="eda07-160">Södra Brasilien</span><span class="sxs-lookup"><span data-stu-id="eda07-160">Brazil South</span></span>
* <span data-ttu-id="eda07-161">Södra Indien</span><span class="sxs-lookup"><span data-stu-id="eda07-161">South India</span></span>

<span data-ttu-id="eda07-162">Web Apps i Linux stöds endast i hello dedikerade apptjänstplaner och har inte en kostnadsfri eller delad nivå.</span><span class="sxs-lookup"><span data-stu-id="eda07-162">Web Apps on Linux is only supported in hello Dedicated app service plans and does not have a Free or Shared tier.</span></span> <span data-ttu-id="eda07-163">Dessutom är App Service-planer för vanliga och Linux-webbprogram ömsesidigt uteslutande, så du inte kan skapa ett Linux-webbprogram i en icke-Linux app service-plan.</span><span class="sxs-lookup"><span data-stu-id="eda07-163">Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.</span></span>

<span data-ttu-id="eda07-164">Web Apps i Linux måste skapas i en resursgrupp som inte innehåller icke-Linux web apps i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="eda07-164">Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in hello same region.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="eda07-165">Felsökning</span><span class="sxs-lookup"><span data-stu-id="eda07-165">Troubleshooting</span></span> ##

<span data-ttu-id="eda07-166">När programmet inte toostart eller du toocheck hello loggning från din app, kontrollera hello Docker loggar i hello loggfilerna directory.</span><span class="sxs-lookup"><span data-stu-id="eda07-166">When your application fails toostart or you want toocheck hello logging from your app, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="eda07-167">Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.</span><span class="sxs-lookup"><span data-stu-id="eda07-167">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="eda07-168">toolog hello `stdout` och `stderr` från din behållaren måste tooenable **Dockerbehållare loggning** under **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="eda07-168">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Aktivera loggning][2]

![Med Kudu tooview Docker-loggar][1]

<span data-ttu-id="eda07-171">Du kan komma åt hello SCM plats från **avancerade verktyg** i hello **utvecklingsverktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="eda07-171">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eda07-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eda07-172">Next steps</span></span>
<span data-ttu-id="eda07-173">Se följande länkar tooget igång med Apptjänst i Linux hello.</span><span class="sxs-lookup"><span data-stu-id="eda07-173">See hello following links tooget started with App Service on Linux.</span></span> <span data-ttu-id="eda07-174">Du kan publicera frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="eda07-174">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="eda07-175">Hur toouse anpassade Docker bild för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-175">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="eda07-176">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-176">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="eda07-177">Med .NET Core i Azure App Service Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-177">Using .NET Core in Azure App Service Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="eda07-178">Med hjälp av Ruby i Azure App Service Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-178">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="eda07-179">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="eda07-179">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="eda07-180">SSH-stöd för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-180">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="eda07-181">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eda07-181">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="eda07-182">Docker-hubb kontinuerlig distribution med Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="eda07-182">Docker Hub Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png