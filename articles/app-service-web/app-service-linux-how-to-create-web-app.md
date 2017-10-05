---
title: "Skapa en Azure-webbapp som körs på Linux | Microsoft Docs"
description: "Web app skapa arbetsflöde för Azure Web App på Linux."
keywords: "Azure apptjänst, webbprogram, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="8f1c3-104">Skapa en Azure-webbapp som körs på Linux</span><span class="sxs-lookup"><span data-stu-id="8f1c3-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a><span data-ttu-id="8f1c3-105">Använda Azure portal för att skapa ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="8f1c3-105">Use the Azure portal to create your web app</span></span>
<span data-ttu-id="8f1c3-106">Du kan börja skapa ditt webbprogram på Linux från den [Azure-portalen](https://portal.azure.com) som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="8f1c3-106">You can start creating your web app on Linux from the [Azure portal](https://portal.azure.com) as shown in the following image:</span></span>

![Skapa en webbapp på Azure portal][1]

<span data-ttu-id="8f1c3-108">Sedan den **skapa-bladet** öppnas som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="8f1c3-108">Next, the **Create blade** opens as shown in the following image:</span></span>

![Skapa-bladet][2]

1. <span data-ttu-id="8f1c3-110">Namnge ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-110">Give your web app a name.</span></span>
2. <span data-ttu-id="8f1c3-111">Välj en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="8f1c3-112">(Se tillgängliga regioner i den [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="8f1c3-112">(See available regions in the [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="8f1c3-113">Välj en befintlig Azure App Service-plan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="8f1c3-114">(Se App Service-plan kommentarer i den [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="8f1c3-114">(See App Service plan notes in the [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="8f1c3-115">Välj programstack som du tänker använda.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-115">Choose the application stack that you intend to use.</span></span> <span data-ttu-id="8f1c3-116">Du kan välja mellan flera versioner av Node.js, PHP, .net kärn och Ruby.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="8f1c3-117">När du har skapat appen, kan du ändra programstack från programinställningarna enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="8f1c3-117">Once you have created the app, you can change the application stack from the application settings as shown in the following image:</span></span>

![Programinställningar][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="8f1c3-119">Distribuera ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="8f1c3-119">Deploy your web app</span></span>
<span data-ttu-id="8f1c3-120">Om du väljer **distributionsalternativ** från management portal kan du välja att använda lokal Git eller GitHub-lagringsplats för att distribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-120">Choosing **deployment options** from the management portal gives you the option to use local Git or GitHub repository to deploy your application.</span></span> <span data-ttu-id="8f1c3-121">Resten av instruktionerna är samma som för en icke-Linux-webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-121">The rest of the instructions are similar to those for a non-Linux web app.</span></span> <span data-ttu-id="8f1c3-122">Du kan följa anvisningarna i [lokal Git-distribution](app-service-deploy-local-git.md) eller [kontinuerlig distribution](app-service-continuous-deployment.md) att distribuera din app.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-122">You can follow the instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) to deploy your app.</span></span>

<span data-ttu-id="8f1c3-123">Du kan också använda FTP för att överföra ditt program på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="8f1c3-123">You can also use FTP to upload your application to your site.</span></span> <span data-ttu-id="8f1c3-124">Du kan hämta FTP-slutpunkten för din webbapp från avsnittet diagnostics loggar som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="8f1c3-124">You can get the FTP endpoint for your web app from the diagnostics logs section as shown in the following image:</span></span>

![Diagnostik-loggar][4]

## <a name="next-steps"></a><span data-ttu-id="8f1c3-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f1c3-126">Next steps</span></span>
* [<span data-ttu-id="8f1c3-127">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="8f1c3-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="8f1c3-128">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="8f1c3-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="8f1c3-129">Med hjälp av Ruby i Azure App Service Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="8f1c3-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="8f1c3-130">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8f1c3-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
