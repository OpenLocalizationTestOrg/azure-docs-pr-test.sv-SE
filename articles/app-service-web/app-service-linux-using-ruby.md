---
title: "aaaUsing Ruby i Azure App Service Web App på Linux | Microsoft Docs"
description: "Med hjälp av Ruby i Azure App Service Webbapp på Linux."
keywords: "Azure apptjänst, webbprogram, vanliga frågor och svar, linux, oss, ruby"
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
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 45692cb3bf1da9ff65b9466055029bfaef8b7d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a><span data-ttu-id="683ae-104">Använda Ruby i webbprogrammet på Linux</span><span class="sxs-lookup"><span data-stu-id="683ae-104">Using Ruby in Web App on Linux</span></span> #

<span data-ttu-id="683ae-105">Vi har fört stöd för Ruby v.2.3 med hello senaste uppdatering tooour serverdel.</span><span class="sxs-lookup"><span data-stu-id="683ae-105">With hello latest update tooour backend, we introduced support for Ruby v.2.3.</span></span> <span data-ttu-id="683ae-106">Genom att ange hello konfiguration av ditt Linux-webbprogram, kan du ändra hello programstack.</span><span class="sxs-lookup"><span data-stu-id="683ae-106">By setting hello configuration of your Linux web app, you can change hello application stack.</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="683ae-107">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="683ae-107">Using hello Azure portal</span></span> ##

<span data-ttu-id="683ae-108">Nya hello-menyn i hello [Azure-portalen](https://portal.azure.com), du kan välja toocreate ett webbprogram på Linux hello webb + mobilt alternativ som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="683ae-108">From hello new menu in hello [Azure portal](https://portal.azure.com), you can choose toocreate a Web App on Linux from hello Web + Mobile option as shown in hello following image:</span></span>

![Skapa en webbapp på hello Azure-portalen][1]

<span data-ttu-id="683ae-110">Därefter hello **skapa-bladet** öppnas som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="683ae-110">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![hello skapa-bladet][2]

1. <span data-ttu-id="683ae-112">Namnge ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="683ae-112">Give your web app a name.</span></span>
2. <span data-ttu-id="683ae-113">Välj en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="683ae-113">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="683ae-114">(Se tillgängliga regioner i hello [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="683ae-114">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="683ae-115">Välj en befintlig Azure App Service-plan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="683ae-115">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="683ae-116">(Se App Service-plan kommentarer i hello [avsnittet](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="683ae-116">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="683ae-117">Välj hello Ruby hello inbyggda Runtime stackar.</span><span class="sxs-lookup"><span data-stu-id="683ae-117">Choose hello Ruby from hello Built-in Runtime stacks.</span></span>

<span data-ttu-id="683ae-118">När ditt Ruby webbprogram skapas, kan du distribuera tooit med Git- eller FTP.</span><span class="sxs-lookup"><span data-stu-id="683ae-118">After your Ruby web app gets created, you can deploy tooit using Git or FTP.</span></span>

<span data-ttu-id="683ae-119">toolearn mer om att skapa en Ruby app Kontrollera hello [get igång-guide](app-service-linux-ruby-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="683ae-119">toolearn more about creating a Ruby app, check hello [get started guide](app-service-linux-ruby-get-started.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="683ae-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="683ae-120">Next steps</span></span>
* [<span data-ttu-id="683ae-121">Vad är Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="683ae-121">What is Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="683ae-122">Lokal Git-distribution tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="683ae-122">Local Git Deployment tooAzure App Service</span></span>](app-service-deploy-local-git.md)
* [<span data-ttu-id="683ae-123">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="683ae-123">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="683ae-124">Skapa en Ruby App med Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="683ae-124">Create a Ruby App with Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png