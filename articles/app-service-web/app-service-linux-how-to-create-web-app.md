---
title: "aaaCreate en Azure web app som körs på Linux | Microsoft Docs"
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
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="edae9-104">Skapa en Azure-webbapp som körs på Linux</span><span class="sxs-lookup"><span data-stu-id="edae9-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a><span data-ttu-id="edae9-105">Använd hello Azure portal toocreate ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="edae9-105">Use hello Azure portal toocreate your web app</span></span>
<span data-ttu-id="edae9-106">Du kan börja skapa ditt webbprogram på Linux från hello [Azure-portalen](https://portal.azure.com) som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="edae9-106">You can start creating your web app on Linux from hello [Azure portal](https://portal.azure.com) as shown in hello following image:</span></span>

![Skapa en webbapp på hello Azure-portalen][1]

<span data-ttu-id="edae9-108">Därefter hello **skapa-bladet** öppnas som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="edae9-108">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![hello skapa-bladet][2]

1. <span data-ttu-id="edae9-110">Namnge ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="edae9-110">Give your web app a name.</span></span>
2. <span data-ttu-id="edae9-111">Välj en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="edae9-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="edae9-112">(Se tillgängliga regioner i hello [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="edae9-112">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="edae9-113">Välj en befintlig Azure App Service-plan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="edae9-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="edae9-114">(Se App Service-plan kommentarer i hello [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="edae9-114">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="edae9-115">Välj programmet hello stack som du avser toouse.</span><span class="sxs-lookup"><span data-stu-id="edae9-115">Choose hello application stack that you intend toouse.</span></span> <span data-ttu-id="edae9-116">Du kan välja mellan flera versioner av Node.js, PHP, .net kärn och Ruby.</span><span class="sxs-lookup"><span data-stu-id="edae9-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="edae9-117">När du har skapat hello app, kan du ändra hello programstack från hello programinställningar som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="edae9-117">Once you have created hello app, you can change hello application stack from hello application settings as shown in hello following image:</span></span>

![Programinställningar][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="edae9-119">Distribuera ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="edae9-119">Deploy your web app</span></span>
<span data-ttu-id="edae9-120">Om du väljer **distributionsalternativ** från hello management portal ger du hello alternativet toouse lokala Git eller GitHub-lagringsplatsen toodeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="edae9-120">Choosing **deployment options** from hello management portal gives you hello option toouse local Git or GitHub repository toodeploy your application.</span></span> <span data-ttu-id="edae9-121">hello resten av hello instruktioner är liknande toothose för en icke-Linux-webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="edae9-121">hello rest of hello instructions are similar toothose for a non-Linux web app.</span></span> <span data-ttu-id="edae9-122">Du kan följa hello instruktionerna i [lokal Git-distribution](app-service-deploy-local-git.md) eller [kontinuerlig distribution](app-service-continuous-deployment.md) toodeploy din app.</span><span class="sxs-lookup"><span data-stu-id="edae9-122">You can follow hello instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) toodeploy your app.</span></span>

<span data-ttu-id="edae9-123">Du kan också använda FTP tooupload webbplatsen tooyour program.</span><span class="sxs-lookup"><span data-stu-id="edae9-123">You can also use FTP tooupload your application tooyour site.</span></span> <span data-ttu-id="edae9-124">Du kan hämta hello FTP-slutpunkten för ditt webbprogram från hello diagnostik loggar avsnittet som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="edae9-124">You can get hello FTP endpoint for your web app from hello diagnostics logs section as shown in hello following image:</span></span>

![Diagnostik-loggar][4]

## <a name="next-steps"></a><span data-ttu-id="edae9-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="edae9-126">Next steps</span></span>
* [<span data-ttu-id="edae9-127">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="edae9-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="edae9-128">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="edae9-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="edae9-129">Med hjälp av Ruby i Azure App Service Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="edae9-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="edae9-130">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="edae9-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
