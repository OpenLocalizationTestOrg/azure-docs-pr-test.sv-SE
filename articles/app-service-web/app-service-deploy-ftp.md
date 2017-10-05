---
title: Distribuera din app till Azure App Service med FTP/S | Microsoft Docs
description: "Lär dig mer om att distribuera din app till Azure App Service med hjälp av FTP- eller FTPS."
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
ms.openlocfilehash: 9078abbc4ed7eff6975201443992f7bbb84bf57c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a><span data-ttu-id="53c38-103">Distribuera din app till Azure App Service med FTP/S</span><span class="sxs-lookup"><span data-stu-id="53c38-103">Deploy your app to Azure App Service using FTP/S</span></span>

<span data-ttu-id="53c38-104">Den här artikeln visar hur du använder FTP eller FTPS för att distribuera ditt webbprogram, mobilappsserverdel eller API-appen [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="53c38-104">This article shows you how to use FTP or FTPS to deploy your web app, mobile app backend, or API app to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="53c38-105">FTP-/ S-slutpunkten för din app är redan aktiv.</span><span class="sxs-lookup"><span data-stu-id="53c38-105">The FTP/S endpoint for your app is already active.</span></span> <span data-ttu-id="53c38-106">Ingen konfiguration krävs för att aktivera FTP/S-distribution.</span><span class="sxs-lookup"><span data-stu-id="53c38-106">No configuration is necessary to enable FTP/S deployment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53c38-107">Vi vidtar kontinuerligt åtgärder för att förbättra säkerheten för Microsoft Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="53c38-107">We are continuously taking steps to improve Microsoft Azure Platform security.</span></span> <span data-ttu-id="53c38-108">Som en del av den här pågående arbete planerad uppgradering av webbprogram för Tyskland Central och Tyskland nordöst regioner.</span><span class="sxs-lookup"><span data-stu-id="53c38-108">As part of this ongoing effort an upgrade of Web Applications is planned for Germany Central and Germany Northeast regions.</span></span> <span data-ttu-id="53c38-109">Web Apps inaktiverar under denna användning av klartext FTP-protokollet för distributioner.</span><span class="sxs-lookup"><span data-stu-id="53c38-109">During this Web Apps will be disabling the use of plain text FTP protocol for deployments.</span></span> <span data-ttu-id="53c38-110">Vår rekommendation för våra kunder är att växla till FTPS för distributioner.</span><span class="sxs-lookup"><span data-stu-id="53c38-110">Our recommendation to our customers is to switch to FTPS for deployments.</span></span> <span data-ttu-id="53c38-111">Vi förväntar sig inte några avbrott i tjänsten under den här uppgraderingen som är planerat till 9/5.</span><span class="sxs-lookup"><span data-stu-id="53c38-111">We do not expect any disruption to your service during this upgrade which is planned for 9/5.</span></span> <span data-ttu-id="53c38-112">Vi uppskattar du stöd för i den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="53c38-112">We appreciate you support in this effort.</span></span>

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a><span data-ttu-id="53c38-113">Steg 1: Ange autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="53c38-113">Step 1: Set deployment credentials</span></span>

<span data-ttu-id="53c38-114">För att komma åt FTP-servern för din app måste du först autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="53c38-114">To access the FTP server for your app, you first need deployment credentials.</span></span> 

<span data-ttu-id="53c38-115">Om du vill ange eller återställa dina autentiseringsuppgifter för distribution, se [autentiseringsuppgifter för distribution av Azure App Service](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="53c38-115">To set or reset your deployment credentials, see [Azure App Service Deployment Credentials](app-service-deployment-credentials.md).</span></span> <span data-ttu-id="53c38-116">Den här kursen visar hur du använder användarnivå autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="53c38-116">This tutorial demonstrates the use of user-level credentials.</span></span>

## <a name="step-2-get-ftp-connection-information"></a><span data-ttu-id="53c38-117">Steg 2: Hämta information om FTP-anslutning</span><span class="sxs-lookup"><span data-stu-id="53c38-117">Step 2: Get FTP connection information</span></span>

1. <span data-ttu-id="53c38-118">I den [Azure-portalen](https://portal.azure.com), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="53c38-118">In the [Azure portal](https://portal.azure.com), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="53c38-119">Välj **översikt** i den vänstra menyn anteckna värdena för **FTP/distribution användaren**, **värdnamn för FTP-**, och **FTPS värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="53c38-119">Select **Overview** in the left menu, then note the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span> 

    ![Information om FTP-anslutning](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > <span data-ttu-id="53c38-121">Den **FTP/distribution användaren** användaren värdet som visas i Azure Portal inklusive namnet på appen för att ge rätt kontext för FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="53c38-121">The **FTP/Deployment User** user value as displayed by the Azure Portal including the app name in order to provide proper context for the FTP server.</span></span>
    > <span data-ttu-id="53c38-122">Du kan hitta samma information när du väljer **egenskaper** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="53c38-122">You can find the same information when you select **Properties** in the left menu.</span></span> 
    >
    > <span data-ttu-id="53c38-123">Dessutom visas aldrig lösenord för distribution.</span><span class="sxs-lookup"><span data-stu-id="53c38-123">Also, the deployment password is never shown.</span></span> <span data-ttu-id="53c38-124">Om du glömmer lösenordet distribution kan gå tillbaka till [steg 1](#step1) och återställa lösenordet distribution.</span><span class="sxs-lookup"><span data-stu-id="53c38-124">If you forget your deployment password, go back to [step 1](#step1) and reset your deployment password.</span></span>
    >
    >

## <a name="step-3-deploy-files-to-azure"></a><span data-ttu-id="53c38-125">Steg 3: Distribuera filer till Azure</span><span class="sxs-lookup"><span data-stu-id="53c38-125">Step 3: Deploy files to Azure</span></span>

1. <span data-ttu-id="53c38-126">Från FTP-klient ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), osv.), använder informationen som samlats in för att ansluta till din app.</span><span class="sxs-lookup"><span data-stu-id="53c38-126">From your FTP client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), etc), use the connection information you gathered to connect to your app.</span></span>
3. <span data-ttu-id="53c38-127">Kopiera filerna och deras respektive katalogstrukturen till den [ **/platsen/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) i Azure (eller **/platsen/wwwroot/App_Data/jobb/** katalogen för WebJobs).</span><span class="sxs-lookup"><span data-stu-id="53c38-127">Copy your files and their respective directory structure to the [**/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (or the **/site/wwwroot/App_Data/Jobs/** directory for WebJobs).</span></span>
4. <span data-ttu-id="53c38-128">Gå till din app-URL att verifiera appen körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="53c38-128">Browse to your app's URL to verify the app is running properly.</span></span> 

> [!NOTE] 
> <span data-ttu-id="53c38-129">Till skillnad från [Git-baserade distributioner](app-service-deploy-local-git.md), FTP-distributionen inte stöder följande distribution automatiseringar:</span><span class="sxs-lookup"><span data-stu-id="53c38-129">Unlike [Git-based deployments](app-service-deploy-local-git.md), FTP deployment doesn't support the following deployment automations:</span></span> 
>
> - <span data-ttu-id="53c38-130">beroende återställning (till exempel NuGet och NPM, PIP. Composer automatiseringar)</span><span class="sxs-lookup"><span data-stu-id="53c38-130">dependency restore (such as NuGet, NPM, PIP, and Composer automations)</span></span>
> - <span data-ttu-id="53c38-131">.NET-binärfiler för</span><span class="sxs-lookup"><span data-stu-id="53c38-131">compilation of .NET binaries</span></span>
> - <span data-ttu-id="53c38-132">generering av web.config (det här är en [Node.js-exempel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span><span class="sxs-lookup"><span data-stu-id="53c38-132">generation of web.config (here is a [Node.js example](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span></span>
> 
> <span data-ttu-id="53c38-133">Du måste återställa, skapa, skapa dessa nödvändiga filer manuellt på den lokala datorn och distribuera dem tillsammans med din app.</span><span class="sxs-lookup"><span data-stu-id="53c38-133">You must restore, build, and generate these necessary files manually on your local machine and deploy them together with your app.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="53c38-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53c38-134">Next steps</span></span>

<span data-ttu-id="53c38-135">Mer avancerade scenarier för distribution, försök [distribution till Azure med Git](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="53c38-135">For more advanced deployment scenarios, try [deploying to Azure with Git](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="53c38-136">Git-baserad distribution till Azure kan versionskontroll, paketet återställning, MSBuild och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="53c38-136">Git-based deployment to Azure enables version control, package restore, MSBuild, and more.</span></span>

## <a name="more-resources"></a><span data-ttu-id="53c38-137">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="53c38-137">More Resources</span></span>

* <span data-ttu-id="53c38-138">[Skapa en PHP-MySQL-webbapp och distribuera med FTP](web-sites-php-mysql-deploy-use-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="53c38-138">[Create a PHP-MySQL web app and deploy using FTP](web-sites-php-mysql-deploy-use-ftp.md).</span></span>
* [<span data-ttu-id="53c38-139">Autentiseringsuppgifter för distribution av Azure App Service</span><span class="sxs-lookup"><span data-stu-id="53c38-139">Azure App Service Deployment Credentials</span></span>](app-service-deploy-ftp.md)
