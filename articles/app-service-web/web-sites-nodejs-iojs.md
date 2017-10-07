---
title: aaaHow toouse io.js med Azure App Service Web Apps
description: "Lär dig hur toouse en webbapp i Azure App Service med io.js."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="dd43b-103">Hur toouse io.js med Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="dd43b-103">How toouse io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="dd43b-104">hello populära nod förgrening [io.js] funktioner olika skillnader tooJoyent Node.js-projekt, inklusive en mer öppna styrning modell, en snabbare utgivningscykeln och en snabbare antagandet av nya och experiment JavaScript-funktioner.</span><span class="sxs-lookup"><span data-stu-id="dd43b-104">hello popular Node fork [io.js] features various differences tooJoyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="dd43b-105">Medan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps har många Node.js-versioner som är förinstallerade, kan också för en som användaren tillhandahållit Node.js binär.</span><span class="sxs-lookup"><span data-stu-id="dd43b-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="dd43b-106">Den här artikeln beskrivs två metoder som gör att hello använda io.js på App Service Web Apps: hello användning av en utökad distributionsskriptet som automatiskt konfigurerar Azure toouse hello senaste tillgängliga io.js versionen, samt hello manuell överföring av en io.js binär.</span><span class="sxs-lookup"><span data-stu-id="dd43b-106">This article discusses two methods enabling hello use of io.js on App Service Web Apps: hello use of an extended deployment script, which automatically configures Azure toouse hello latest available io.js version, as well as hello manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="dd43b-107">Med hjälp av ett skript för distribution</span><span class="sxs-lookup"><span data-stu-id="dd43b-107">Using a Deployment Script</span></span>
<span data-ttu-id="dd43b-108">Vid distribution av en Node.js-app kör App Service Web Apps ett antal små kommandon tooensure som hello miljö har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="dd43b-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands tooensure that hello environment is configured properly.</span></span> <span data-ttu-id="dd43b-109">Med en distribution med skriptet för kan den här processen vara anpassade tooinclude hello hämtningen och konfigurationen av io.js.</span><span class="sxs-lookup"><span data-stu-id="dd43b-109">Using a deployment script, this process can be customized tooinclude hello download and configuration of io.js.</span></span>

<span data-ttu-id="dd43b-110">Hej [io.js distributionsskriptet](https://github.com/felixrieseberg/iojs-azure) finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="dd43b-110">hello [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="dd43b-111">tooenable io.js på ditt webbprogram helt enkelt kopiera **.deployment**, **deploy.cmd** och **IISNode.yml** toohello rot i programmappen och distribuera tooWeb appar.</span><span class="sxs-lookup"><span data-stu-id="dd43b-111">tooenable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** toohello root of your application folder and deploy tooWeb Apps.</span></span>  

<span data-ttu-id="dd43b-112">hello första filen, **.deployment**, instruerar Web Apps toorun **deploy.cmd** på distribution.</span><span class="sxs-lookup"><span data-stu-id="dd43b-112">hello first file, **.deployment**, instructs Web Apps toorun **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="dd43b-113">Det här skriptet körs alla hello vanliga åtgärder för ett Node.js-program, men också hämtar hello senaste versionen av io.js.</span><span class="sxs-lookup"><span data-stu-id="dd43b-113">This script runs all hello usual steps for a Node.js application, but also downloads hello latest version of io.js.</span></span> <span data-ttu-id="dd43b-114">Slutligen **IISNode.yml** konfigurerar Web Apps toouse bara hello hämtas io.js binära i stället för en förinstallerad Node.js binär.</span><span class="sxs-lookup"><span data-stu-id="dd43b-114">Finally, **IISNode.yml** configures Web Apps toouse just hello downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="dd43b-115">tooupdate hello används io.js binary, distribuera bara om programmet - hello skript hämtar en ny version av io.js varje gång hello programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="dd43b-115">tooupdate hello used io.js binary, just redeploy your application - hello script will download a new version of io.js every single time hello application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="dd43b-116">Med manuell Installation</span><span class="sxs-lookup"><span data-stu-id="dd43b-116">Using Manual Installation</span></span>
<span data-ttu-id="dd43b-117">hello manuell installation av en anpassad io.js version omfattar bara två steg.</span><span class="sxs-lookup"><span data-stu-id="dd43b-117">hello manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="dd43b-118">Hämta först hello **win-x64** binära direkt från hello [io.js distribution].</span><span class="sxs-lookup"><span data-stu-id="dd43b-118">First, download hello **win-x64** binary directly from hello [io.js distribution].</span></span> <span data-ttu-id="dd43b-119">Krävs är två filer – **iojs.exe** och **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="dd43b-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="dd43b-120">Spara båda filer tooa mapp inuti ditt webbprogram, till exempel i **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="dd43b-120">Save both files tooa folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="dd43b-121">tooconfigure Web Apps toouse **iojs.exe** i stället för en förinstallerad nod-version, skapa en **IISNode.yml** hello roten i ditt program och Lägg till följande rad hello.</span><span class="sxs-lookup"><span data-stu-id="dd43b-121">tooconfigure Web Apps toouse **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at hello root of your application and add hello following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="dd43b-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd43b-122">Next Steps</span></span>
<span data-ttu-id="dd43b-123">I den här artikeln du lärt dig hur toouse io.js med App Service Web Apps med hjälp av både tillhandahålls distribution skript samt manuell installation.</span><span class="sxs-lookup"><span data-stu-id="dd43b-123">In this article you learned how toouse io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="dd43b-124">IO.js är under utveckling tunga och uppdateras oftare än Node.js.</span><span class="sxs-lookup"><span data-stu-id="dd43b-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="dd43b-125">Ett antal Node.js-moduler kanske inte fungerar med io.js - Kontrollera finns [io.js på GitHub] för felsökning.</span><span class="sxs-lookup"><span data-stu-id="dd43b-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="dd43b-126">Nyheter</span><span class="sxs-lookup"><span data-stu-id="dd43b-126">What's changed</span></span>
* <span data-ttu-id="dd43b-127">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="dd43b-127">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="dd43b-128">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="dd43b-128">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="dd43b-129">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="dd43b-129">No credit cards required; no commitments.</span></span>
> 
> 

[io.js]: https://iojs.org
[io.js distribution]: https://iojs.org/dist/
[io.js på GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
