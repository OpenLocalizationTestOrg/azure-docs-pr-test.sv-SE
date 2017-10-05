---
title: "Använda io.js med Webbappar i Azure Apptjänst"
description: "Lär dig hur du använder en webbapp i Azure App Service med io.js."
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
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="519cd-103">Använda io.js med Webbappar i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="519cd-103">How to use io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="519cd-104">Populära nod förgrening [io.js] funktioner olika skillnader till Joyent's Node.js-projekt, inklusive en mer öppna styrning modell, en snabbare utgivningscykeln och en snabbare antagandet av nya och experiment JavaScript-funktioner.</span><span class="sxs-lookup"><span data-stu-id="519cd-104">The popular Node fork [io.js] features various differences to Joyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="519cd-105">Medan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps har många Node.js-versioner som är förinstallerade, kan också för en som användaren tillhandahållit Node.js binär.</span><span class="sxs-lookup"><span data-stu-id="519cd-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="519cd-106">Den här artikeln beskrivs två metoder som gör att du kan använda io.js på App Service Web Apps: användning av en utökad distributionsskriptet som automatiskt konfigurerar Azure för att använda den senaste tillgängliga io.js versionen, samt en io.js binär manuella överfördes.</span><span class="sxs-lookup"><span data-stu-id="519cd-106">This article discusses two methods enabling the use of io.js on App Service Web Apps: The use of an extended deployment script, which automatically configures Azure to use the latest available io.js version, as well as the manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="519cd-107">Med hjälp av ett skript för distribution</span><span class="sxs-lookup"><span data-stu-id="519cd-107">Using a Deployment Script</span></span>
<span data-ttu-id="519cd-108">Vid distribution av en Node.js-app kör App Service Web Apps ett antal små kommandon för att kontrollera att miljön är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="519cd-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands to ensure that the environment is configured properly.</span></span> <span data-ttu-id="519cd-109">Den här processen innehåller ett skript för distribution, kan anpassas så att ladda ned och konfigurationen av io.js.</span><span class="sxs-lookup"><span data-stu-id="519cd-109">Using a deployment script, this process can be customized to include the download and configuration of io.js.</span></span>

<span data-ttu-id="519cd-110">Den [io.js distributionsskriptet](https://github.com/felixrieseberg/iojs-azure) finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="519cd-110">The [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="519cd-111">Om du vill aktivera io.js på ditt webbprogram, helt enkelt kopiera **.deployment**, **deploy.cmd** och **IISNode.yml** till roten i programmappen och distribuera till Web Apps.</span><span class="sxs-lookup"><span data-stu-id="519cd-111">To enable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** to the root of your application folder and deploy to Web Apps.</span></span>  

<span data-ttu-id="519cd-112">Den första filen **.deployment**, instruerar Web Apps att köra **deploy.cmd** på distribution.</span><span class="sxs-lookup"><span data-stu-id="519cd-112">The first file, **.deployment**, instructs Web Apps to run **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="519cd-113">Det här skriptet körs alla vanliga åtgärder för ett Node.js-program, men även laddar ned den senaste versionen av io.js.</span><span class="sxs-lookup"><span data-stu-id="519cd-113">This script runs all the usual steps for a Node.js application, but also downloads the latest version of io.js.</span></span> <span data-ttu-id="519cd-114">Slutligen **IISNode.yml** konfigurerar webbprogram att använda just den hämtade io.js binära i stället för en förinstallerad Node.js binär.</span><span class="sxs-lookup"><span data-stu-id="519cd-114">Finally, **IISNode.yml** configures Web Apps to use just the downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="519cd-115">Om du vill uppdatera använda io.js binärfilen bara distribuera ditt program - skriptet laddar ned en ny version av io.js varje gång programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="519cd-115">To update the used io.js binary, just redeploy your application - the script will download a new version of io.js every single time the application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="519cd-116">Med manuell Installation</span><span class="sxs-lookup"><span data-stu-id="519cd-116">Using Manual Installation</span></span>
<span data-ttu-id="519cd-117">Manuell installation av en anpassad io.js version omfattar bara två steg.</span><span class="sxs-lookup"><span data-stu-id="519cd-117">The manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="519cd-118">Hämta först den **win-x64** binära direkt från den [io.js distribution].</span><span class="sxs-lookup"><span data-stu-id="519cd-118">First, download the **win-x64** binary directly from the [io.js distribution].</span></span> <span data-ttu-id="519cd-119">Krävs är två filer – **iojs.exe** och **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="519cd-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="519cd-120">Spara filer till en mapp i ditt webbprogram, till exempel i **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="519cd-120">Save both files to a folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="519cd-121">Du konfigurerar webbprogram att använda **iojs.exe** i stället för en förinstallerad nod-version, skapa en **IISNode.yml** filen i roten för ditt program och Lägg till följande rad.</span><span class="sxs-lookup"><span data-stu-id="519cd-121">To configure Web Apps to use **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at the root of your application and add the following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="519cd-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="519cd-122">Next Steps</span></span>
<span data-ttu-id="519cd-123">I den här artikeln som du har lärt dig hur du använder som io.js med App Service Web Apps med hjälp av både distribution skript samt som manuell installation.</span><span class="sxs-lookup"><span data-stu-id="519cd-123">In this article you learned how to use io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="519cd-124">IO.js är under utveckling tunga och uppdateras oftare än Node.js.</span><span class="sxs-lookup"><span data-stu-id="519cd-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="519cd-125">Ett antal Node.js-moduler kanske inte fungerar med io.js - Kontrollera finns [io.js på GitHub] för felsökning.</span><span class="sxs-lookup"><span data-stu-id="519cd-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="519cd-126">Nyheter</span><span class="sxs-lookup"><span data-stu-id="519cd-126">What's changed</span></span>
* <span data-ttu-id="519cd-127">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="519cd-127">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="519cd-128">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="519cd-128">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="519cd-129">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="519cd-129">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="519cd-130">[io.js]: https://iojs.org</span><span class="sxs-lookup"><span data-stu-id="519cd-130">[io.js]: https://iojs.org</span></span>
<span data-ttu-id="519cd-131">[io.js distribution]: https://iojs.org/dist/</span><span class="sxs-lookup"><span data-stu-id="519cd-131">[io.js distribution]: https://iojs.org/dist/</span></span>
<span data-ttu-id="519cd-132">[io.js på GitHub]: https://github.com/iojs/io.js</span><span class="sxs-lookup"><span data-stu-id="519cd-132">[io.js on GitHub]: https://github.com/iojs/io.js</span></span>
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
