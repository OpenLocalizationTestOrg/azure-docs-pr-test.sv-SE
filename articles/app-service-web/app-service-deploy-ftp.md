---
title: aaaDeploy din app tooAzure App Service med FTP/S | Microsoft Docs
description: "Lär dig hur toodeploy din app tooAzure App Service med FTP- eller FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a><span data-ttu-id="a4c43-103">Distribuera din app tooAzure App Service med FTP/S</span><span class="sxs-lookup"><span data-stu-id="a4c43-103">Deploy your app tooAzure App Service using FTP/S</span></span>

<span data-ttu-id="a4c43-104">Den här artikeln beskrivs hur du toouse FTP eller FTPS toodeploy ditt webbprogram, mobilappsserverdel eller API-app för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="a4c43-104">This article shows you how toouse FTP or FTPS toodeploy your web app, mobile app backend, or API app too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="a4c43-105">hello FTP/S-slutpunkten för din app är redan aktiv.</span><span class="sxs-lookup"><span data-stu-id="a4c43-105">hello FTP/S endpoint for your app is already active.</span></span> <span data-ttu-id="a4c43-106">Ingen konfiguration är nödvändiga tooenable FTP/S-distributionen.</span><span class="sxs-lookup"><span data-stu-id="a4c43-106">No configuration is necessary tooenable FTP/S deployment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4c43-107">Vi tar kontinuerligt steg tooimprove säkerhet för Microsoft Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a4c43-107">We are continuously taking steps tooimprove Microsoft Azure Platform security.</span></span> <span data-ttu-id="a4c43-108">Som en del av den här pågående arbete planerad uppgradering av webbprogram för Tyskland Central och Tyskland nordöst regioner.</span><span class="sxs-lookup"><span data-stu-id="a4c43-108">As part of this ongoing effort an upgrade of Web Applications is planned for Germany Central and Germany Northeast regions.</span></span> <span data-ttu-id="a4c43-109">Web Apps inaktiverar under denna hello användning av klartext FTP-protokollet för distributioner.</span><span class="sxs-lookup"><span data-stu-id="a4c43-109">During this Web Apps will be disabling hello use of plain text FTP protocol for deployments.</span></span> <span data-ttu-id="a4c43-110">Vår rekommendation tooour kunder är tooswitch tooFTPS för distributioner.</span><span class="sxs-lookup"><span data-stu-id="a4c43-110">Our recommendation tooour customers is tooswitch tooFTPS for deployments.</span></span> <span data-ttu-id="a4c43-111">Vi förväntar sig inte några avbrott tooyour tjänsten under den här uppgraderingen som är planerat till 9/5.</span><span class="sxs-lookup"><span data-stu-id="a4c43-111">We do not expect any disruption tooyour service during this upgrade which is planned for 9/5.</span></span> <span data-ttu-id="a4c43-112">Vi uppskattar du stöd för i den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a4c43-112">We appreciate you support in this effort.</span></span>

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a><span data-ttu-id="a4c43-113">Steg 1: Ange autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="a4c43-113">Step 1: Set deployment credentials</span></span>

<span data-ttu-id="a4c43-114">tooaccess hello FTP-server för din app, måste du först autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="a4c43-114">tooaccess hello FTP server for your app, you first need deployment credentials.</span></span> 

<span data-ttu-id="a4c43-115">tooset eller återställa dina autentiseringsuppgifter för distribution finns [autentiseringsuppgifter för distribution av Azure App Service](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="a4c43-115">tooset or reset your deployment credentials, see [Azure App Service Deployment Credentials](app-service-deployment-credentials.md).</span></span> <span data-ttu-id="a4c43-116">Den här kursen visar hello använder användarnivå autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a4c43-116">This tutorial demonstrates hello use of user-level credentials.</span></span>

## <a name="step-2-get-ftp-connection-information"></a><span data-ttu-id="a4c43-117">Steg 2: Hämta information om FTP-anslutning</span><span class="sxs-lookup"><span data-stu-id="a4c43-117">Step 2: Get FTP connection information</span></span>

1. <span data-ttu-id="a4c43-118">I hello [Azure-portalen](https://portal.azure.com), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="a4c43-118">In hello [Azure portal](https://portal.azure.com), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="a4c43-119">Välj **översikt** hello vänstra menyn anteckna hello värden för **FTP/distribution användaren**, **värdnamn för FTP-**, och **FTPS värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="a4c43-119">Select **Overview** in hello left menu, then note hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span> 

    ![Information om FTP-anslutning](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > <span data-ttu-id="a4c43-121">Hej **FTP/distribution användaren** värde som användaren som visas av hello Azure Portal, inklusive appnamn hälsningspaket i ordning tooprovide rätt kontext för hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="a4c43-121">hello **FTP/Deployment User** user value as displayed by hello Azure Portal including hello app name in order tooprovide proper context for hello FTP server.</span></span>
    > <span data-ttu-id="a4c43-122">Du kan hitta hello samma information när du väljer **egenskaper** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="a4c43-122">You can find hello same information when you select **Properties** in hello left menu.</span></span> 
    >
    > <span data-ttu-id="a4c43-123">Dessutom visas aldrig hello distribution lösenord.</span><span class="sxs-lookup"><span data-stu-id="a4c43-123">Also, hello deployment password is never shown.</span></span> <span data-ttu-id="a4c43-124">Om du glömmer lösenordet distribution går du tillbaka för[steg 1](#step1) och återställa lösenordet distribution.</span><span class="sxs-lookup"><span data-stu-id="a4c43-124">If you forget your deployment password, go back too[step 1](#step1) and reset your deployment password.</span></span>
    >
    >

## <a name="step-3-deploy-files-tooazure"></a><span data-ttu-id="a4c43-125">Steg 3: Distribuera filer tooAzure</span><span class="sxs-lookup"><span data-stu-id="a4c43-125">Step 3: Deploy files tooAzure</span></span>

1. <span data-ttu-id="a4c43-126">Från FTP-klient ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), osv.), Använd hello anslutningsinformation du samlade in tooconnect tooyour app.</span><span class="sxs-lookup"><span data-stu-id="a4c43-126">From your FTP client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), etc), use hello connection information you gathered tooconnect tooyour app.</span></span>
3. <span data-ttu-id="a4c43-127">Kopiera filerna och deras respektive directory strukturen toohello [ **/platsen/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) i Azure (eller hello **/platsen/wwwroot/App_Data/jobb/** katalogen för WebJobs).</span><span class="sxs-lookup"><span data-stu-id="a4c43-127">Copy your files and their respective directory structure toohello [**/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (or hello **/site/wwwroot/App_Data/Jobs/** directory for WebJobs).</span></span>
4. <span data-ttu-id="a4c43-128">Bläddra tooyour appens URL tooverify hello appen körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="a4c43-128">Browse tooyour app's URL tooverify hello app is running properly.</span></span> 

> [!NOTE] 
> <span data-ttu-id="a4c43-129">Till skillnad från [Git-baserade distributioner](app-service-deploy-local-git.md), FTP-distributionen inte stöder hello efter distributionen automatiseringar:</span><span class="sxs-lookup"><span data-stu-id="a4c43-129">Unlike [Git-based deployments](app-service-deploy-local-git.md), FTP deployment doesn't support hello following deployment automations:</span></span> 
>
> - <span data-ttu-id="a4c43-130">beroende återställning (till exempel NuGet och NPM, PIP. Composer automatiseringar)</span><span class="sxs-lookup"><span data-stu-id="a4c43-130">dependency restore (such as NuGet, NPM, PIP, and Composer automations)</span></span>
> - <span data-ttu-id="a4c43-131">.NET-binärfiler för</span><span class="sxs-lookup"><span data-stu-id="a4c43-131">compilation of .NET binaries</span></span>
> - <span data-ttu-id="a4c43-132">generering av web.config (det här är en [Node.js-exempel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span><span class="sxs-lookup"><span data-stu-id="a4c43-132">generation of web.config (here is a [Node.js example](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span></span>
> 
> <span data-ttu-id="a4c43-133">Du måste återställa, skapa, skapa dessa nödvändiga filer manuellt på den lokala datorn och distribuera dem tillsammans med din app.</span><span class="sxs-lookup"><span data-stu-id="a4c43-133">You must restore, build, and generate these necessary files manually on your local machine and deploy them together with your app.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="a4c43-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4c43-134">Next steps</span></span>

<span data-ttu-id="a4c43-135">Mer avancerade scenarier för distribution, försök [distribuera tooAzure med Git](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="a4c43-135">For more advanced deployment scenarios, try [deploying tooAzure with Git](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="a4c43-136">Distribution av Git-baserade tooAzure kan versionskontroll, paketet återställning, MSBuild och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a4c43-136">Git-based deployment tooAzure enables version control, package restore, MSBuild, and more.</span></span>

## <a name="more-resources"></a><span data-ttu-id="a4c43-137">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="a4c43-137">More Resources</span></span>

* <span data-ttu-id="a4c43-138">[Skapa en PHP-MySQL-webbapp och distribuera med FTP](web-sites-php-mysql-deploy-use-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="a4c43-138">[Create a PHP-MySQL web app and deploy using FTP](web-sites-php-mysql-deploy-use-ftp.md).</span></span>
* [<span data-ttu-id="a4c43-139">Autentiseringsuppgifter för distribution av Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a4c43-139">Azure App Service Deployment Credentials</span></span>](app-service-deploy-ftp.md)
