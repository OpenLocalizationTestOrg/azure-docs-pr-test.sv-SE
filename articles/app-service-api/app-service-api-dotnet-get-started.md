---
title: "aaaGet igång med API Apps och ASP.NET i Apptjänst | Microsoft Docs"
description: "Lär dig hur toocreate, distribuera och använda en ASP.NET API-app i Azure App Service med hjälp av Visual Studio 2015."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: ddc028b2-cde0-4567-a6ee-32cb264a830a
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: alkarche
ms.openlocfilehash: d3e90f1585907d183b0435c6cafc5585bc1e29ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a><span data-ttu-id="27198-103">Kom igång med API Apps, ASP.NET och Swagger i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="27198-103">Get started with API Apps, ASP.NET, and Swagger in Azure App Service</span></span>
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="27198-104">Detta är hello först i en serie kurser som visar hur toouse funktioner i Azure App Service som är användbara för att utveckla och vara värd för RESTful-API: er.</span><span class="sxs-lookup"><span data-stu-id="27198-104">This is hello first in a series of tutorials that show how toouse features of Azure App Service that are helpful for developing and hosting RESTful APIs.</span></span>  <span data-ttu-id="27198-105">I den här kursen ingår stöd för API-metadata i Swagger-format.</span><span class="sxs-lookup"><span data-stu-id="27198-105">This tutorial covers support for API metadata in Swagger format.</span></span>

<span data-ttu-id="27198-106">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="27198-106">You'll learn:</span></span>

* <span data-ttu-id="27198-107">Hur toocreate och distribuera [API apps](app-service-api-apps-why-best-platform.md) i Azure App Service med hjälp av verktygen i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="27198-107">How toocreate and deploy [API apps](app-service-api-apps-why-best-platform.md) in Azure App Service by using tools built into Visual Studio 2015.</span></span>
* <span data-ttu-id="27198-108">Hur tooautomate API-identifiering med hjälp av hello Swashbuckle NuGet-paketet toodynamically generera Swagger API-metadata.</span><span class="sxs-lookup"><span data-stu-id="27198-108">How tooautomate API discovery by using hello Swashbuckle NuGet package toodynamically generate Swagger API metadata.</span></span>
* <span data-ttu-id="27198-109">Hur toouse Swagger API-metadata tooautomatically generera klientkod till en API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-109">How toouse Swagger API metadata tooautomatically generate client code for an API app.</span></span>

## <a name="sample-application-overview"></a><span data-ttu-id="27198-110">Översikt över exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="27198-110">Sample application overview</span></span>
<span data-ttu-id="27198-111">I den här kursen får du arbeta med ett enkelt exempelprogram med en att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="27198-111">In this tutorial, you work with a simple to-do list sample application.</span></span> <span data-ttu-id="27198-112">hello-programmet har en klientdel för en sida-program (SPA), ett ASP.NET Web API-mellannivå och en ASP.NET Web API-datanivå.</span><span class="sxs-lookup"><span data-stu-id="27198-112">hello application has a single-page application (SPA) front end, an ASP.NET Web API middle tier, and an ASP.NET Web API data tier.</span></span>

![Diagram över exempelprogram i API Apps](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

<span data-ttu-id="27198-114">Här är en skärmbild av hello [AngularJS](https://angularjs.org/) klientdelen.</span><span class="sxs-lookup"><span data-stu-id="27198-114">Here's a screen shot of hello [AngularJS](https://angularjs.org/) front end.</span></span>

![API Apps exempel toodo lista](./media/app-service-api-dotnet-get-started/todospa.png)

<span data-ttu-id="27198-116">hello Visual Studio-lösningen innehåller tre projekt:</span><span class="sxs-lookup"><span data-stu-id="27198-116">hello Visual Studio solution includes three projects:</span></span>

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* <span data-ttu-id="27198-117">**ToDoListAngular** -hello klientdelen: en AngularJS SPA som anropar hello mellannivå.</span><span class="sxs-lookup"><span data-stu-id="27198-117">**ToDoListAngular** - hello front end: an AngularJS SPA that calls hello middle tier.</span></span>
* <span data-ttu-id="27198-118">**ToDoListAPI** -hello mellannivån: ett ASP.NET Web API-projekt som anropar hello datanivå tooperform CRUD-åtgärder på arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="27198-118">**ToDoListAPI** - hello middle tier: an ASP.NET Web API project that calls hello data tier tooperform CRUD operations on to-do items.</span></span>
* <span data-ttu-id="27198-119">**ToDoListDataAPI** -hello datanivån: ett ASP.NET Web API-projekt som utför CRUD-åtgärder på arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="27198-119">**ToDoListDataAPI** - hello data tier:  an ASP.NET Web API project that performs CRUD operations on to-do items.</span></span>

<span data-ttu-id="27198-120">hello arkitekturen med tre nivåer är en av många arkitekturer som du kan implementera med hjälp av API Apps och används här endast i demonstrationssyfte.</span><span class="sxs-lookup"><span data-stu-id="27198-120">hello three-tier architecture is one of many architectures that you can implement by using API Apps and is used here only for demonstration purposes.</span></span> <span data-ttu-id="27198-121">hello koden i varje nivå är så enkelt som möjligt toodemonstrate API Apps funktioner; hello datanivån använder till exempel serverminne i stället för en databas som persistencemekanism.</span><span class="sxs-lookup"><span data-stu-id="27198-121">hello code in each tier is as simple as possible toodemonstrate API Apps features; for example, hello data tier uses server memory rather than a database as its persistence mechanism.</span></span>

<span data-ttu-id="27198-122">På den här kursen har du hello två Web API-projekten och körs i hello moln i App Service API apps.</span><span class="sxs-lookup"><span data-stu-id="27198-122">On completing this tutorial, you'll have hello two Web API projects up and running in hello cloud in App Service API apps.</span></span>

<span data-ttu-id="27198-123">hello nästa kurs i serien hello distribuerar hello SPA klientdelen toohello moln.</span><span class="sxs-lookup"><span data-stu-id="27198-123">hello next tutorial in hello series deploys hello SPA front end toohello cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27198-124">Krav</span><span class="sxs-lookup"><span data-stu-id="27198-124">Prerequisites</span></span>
* <span data-ttu-id="27198-125">ASP.NET Web API - hello självstudiekursen förutsätts att du har grundläggande kunskaper om hur toowork med ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27198-125">ASP.NET Web API - hello tutorial instructions assume you have a basic knowledge of how toowork with ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.</span></span>
* <span data-ttu-id="27198-126">Azure-konto – Du kan [öppna ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="27198-126">Azure account - You can [Open an Azure account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
  
    <span data-ttu-id="27198-127">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="27198-127">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="27198-128">Där kan du omedelbart skapa en tillfällig startapp i Apptjänst – **inget kreditkort behövs** och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="27198-128">There, you can immediately create a short-lived starter app in App Service — **no credit card required**, and no commitments.</span></span>
* <span data-ttu-id="27198-129">Visual Studio 2015 med hello [Azure SDK för .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK installerar Visual Studio 2015 automatiskt om det inte redan har.</span><span class="sxs-lookup"><span data-stu-id="27198-129">Visual Studio 2015 with hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - hello SDK installs Visual Studio 2015 automatically if you don't already have it.</span></span>
  
  * <span data-ttu-id="27198-130">I Visual Studio klickar du på Hjälp -> Om Microsoft Visual Studio och ser till att du har "Azure Apptjänst-verktyg v2.9.1" eller senare installerat.</span><span class="sxs-lookup"><span data-stu-id="27198-130">In Visual Studio, click Help -> About Microsoft Visual Studio and ensure that you have "Azure App Service Tools v2.9.1" or higher installed.</span></span>
    
    ![Azure App-verktyg version](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > <span data-ttu-id="27198-132">Beroende på hur många av hello SDK beroenden du redan har på din dator, kan installera hello SDK ta lång tid, från flera minuter tooa halvtimme eller flera.</span><span class="sxs-lookup"><span data-stu-id="27198-132">Depending on how many of hello SDK dependencies you already have on your machine, installing hello SDK could take a long time, from several minutes tooa half hour or more.</span></span>
    > 
    > 

## <a name="download-hello-sample-application"></a><span data-ttu-id="27198-133">Hämta hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="27198-133">Download hello sample application</span></span>
1. <span data-ttu-id="27198-134">Hämta hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) databasen.</span><span class="sxs-lookup"><span data-stu-id="27198-134">Download hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) repository.</span></span>
   
    <span data-ttu-id="27198-135">Du kan klicka på hello **hämta ZIP** knapp eller klona hello-databasen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="27198-135">You can click hello **Download ZIP** button or clone hello repository on your local machine.</span></span>
2. <span data-ttu-id="27198-136">Öppna hello ToDoList-lösningen i Visual Studio 2015 eller 2013.</span><span class="sxs-lookup"><span data-stu-id="27198-136">Open hello ToDoList solution in Visual Studio 2015 or 2013.</span></span>
   
   1. <span data-ttu-id="27198-137">Du behöver tootrust varje lösning.</span><span class="sxs-lookup"><span data-stu-id="27198-137">You will need tootrust each solution.</span></span>
         <span data-ttu-id="27198-138">![Säkerhetsvarning](./media/app-service-api-dotnet-get-started/securitywarning.png)</span><span class="sxs-lookup"><span data-stu-id="27198-138">![Security Warning](./media/app-service-api-dotnet-get-started/securitywarning.png)</span></span>
3. <span data-ttu-id="27198-139">Skapa hello-lösning (CTRL + SKIFT + B) toorestore hello NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="27198-139">Build hello solution (CTRL + SHIFT + B)  toorestore hello NuGet packages.</span></span>
   
    <span data-ttu-id="27198-140">Om du vill toosee hello programmet i åtgärd innan du distribuerar det, kan du köra det lokalt.</span><span class="sxs-lookup"><span data-stu-id="27198-140">If you want toosee hello application in operation before you deploy it, you can run it locally.</span></span> <span data-ttu-id="27198-141">Se till att ToDoListDataAPI är din Startprojekt och kör hello lösning.</span><span class="sxs-lookup"><span data-stu-id="27198-141">Make sure that ToDoListDataAPI is your startup project and run hello solution.</span></span> <span data-ttu-id="27198-142">Du kan förvänta toosee HTTP 403-fel i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="27198-142">You should expect toosee a HTTP 403 error in your browser.</span></span>

## <a name="use-swagger-api-metadata-and-ui"></a><span data-ttu-id="27198-143">Använd Swagger API-metadata och -användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="27198-143">Use Swagger API metadata and UI</span></span>
<span data-ttu-id="27198-144">Stöd för [Swagger](http://swagger.io/) 2.0 API-metadata är inbyggt i Azure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="27198-144">Support for [Swagger](http://swagger.io/) 2.0 API metadata is built into Azure App Service.</span></span> <span data-ttu-id="27198-145">Varje API-app kan ange en URL-slutpunkt som returnerar metadata för hello API i Swagger JSON-format.</span><span class="sxs-lookup"><span data-stu-id="27198-145">Each API app can specify a URL endpoint that returns metadata for hello API in Swagger JSON format.</span></span> <span data-ttu-id="27198-146">hello metadata som returneras från slutpunkten kan vara används toogenerate klientkod.</span><span class="sxs-lookup"><span data-stu-id="27198-146">hello metadata returned from that endpoint can be used toogenerate client code.</span></span>

<span data-ttu-id="27198-147">En ASP.NET Web API-projekt kan dynamiskt generera Swagger-metadata med hjälp av hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="27198-147">An ASP.NET Web API project can dynamically generate Swagger metadata by using hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet package.</span></span> <span data-ttu-id="27198-148">Hej Swashbuckle NuGet-paketet har redan installerats i hello ToDoListDataAPI- och ToDoListAPI-projekt som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="27198-148">hello Swashbuckle NuGet package is already installed in hello ToDoListDataAPI and ToDoListAPI projects that you downloaded.</span></span>

<span data-ttu-id="27198-149">I det här avsnittet av kursen hello du tittar på hello genererade Swagger 2.0-metadata och försök på ett testanvändargränssnitt som baseras på hello Swagger-metadata.</span><span class="sxs-lookup"><span data-stu-id="27198-149">In this section of hello tutorial, you look at hello generated Swagger 2.0 metadata, and then you try out a test UI that is based on hello Swagger metadata.</span></span>

1. <span data-ttu-id="27198-150">Ange hello ToDoListDataAPI-projektet (**inte** hello ToDoListAPI-projektet) som hello Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="27198-150">Set hello ToDoListDataAPI project (**not** hello ToDoListAPI project) as hello startup project.</span></span>
   
    ![Ange ToDoDataAPI som startprojekt](./media/app-service-api-dotnet-get-started/startupproject.png)
2. <span data-ttu-id="27198-152">Tryck på F5 eller klicka på **Felsök > Starta felsökning** toorun hello projektet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="27198-152">Press F5 or click **Debug > Start Debugging** toorun hello project in debug mode.</span></span>
   
    <span data-ttu-id="27198-153">hello webbläsaren öppnas och visar hello HTTP 403 felsidan.</span><span class="sxs-lookup"><span data-stu-id="27198-153">hello browser opens and shows hello HTTP 403 error page.</span></span>
3. <span data-ttu-id="27198-154">I webbläsarens adressfält, lägger du till `swagger/docs/v1` toohello slutet av hello rad och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="27198-154">In your browser address bar, add `swagger/docs/v1` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="27198-155">(URL: en är hello `http://localhost:45914/swagger/docs/v1`.)</span><span class="sxs-lookup"><span data-stu-id="27198-155">(hello URL is `http://localhost:45914/swagger/docs/v1`.)</span></span>
   
    <span data-ttu-id="27198-156">Detta är hello standard-URL som används av Swashbuckle tooreturn Swagger 2.0 JSON-metadata för hello API.</span><span class="sxs-lookup"><span data-stu-id="27198-156">This is hello default URL used by Swashbuckle tooreturn Swagger 2.0 JSON metadata for hello API.</span></span>
   
    <span data-ttu-id="27198-157">Om du använder Internet Explorer uppmanar webbläsaren hello toodownload en *v1.json* fil.</span><span class="sxs-lookup"><span data-stu-id="27198-157">If you're using Internet Explorer, hello browser prompts you toodownload a *v1.json* file.</span></span>
   
    ![Hämta JSON-metadata i IE](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    <span data-ttu-id="27198-159">Om du använder Chrome, Firefox eller Edge visar hello webbläsare hello JSON i webbläsarfönstret hello.</span><span class="sxs-lookup"><span data-stu-id="27198-159">If you're using Chrome, Firefox, or Edge, hello browser displays hello JSON in hello browser window.</span></span> <span data-ttu-id="27198-160">Olika webbläsare hanterar JSON på olika sätt och ditt webbläsarfönster ser kanske inte ser ut precis som hello exempel.</span><span class="sxs-lookup"><span data-stu-id="27198-160">Different browsers handle JSON differently, and your browser window may not look exactly like hello example.</span></span>
   
    ![JSON-metadata i Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    <span data-ttu-id="27198-162">hello följande exempel visar hello första delen av hello Swagger-metadata för hello API, med hello definition för hello Get-metod.</span><span class="sxs-lookup"><span data-stu-id="27198-162">hello following sample shows hello first section of hello Swagger metadata for hello API, with hello definition for hello Get method.</span></span> <span data-ttu-id="27198-163">Dessa metadata är vilka enheter hello Swagger-användargränssnitt som du använder i hello följande steg och du använder den i ett senare avsnitt av kursen hello-tooautomatically generera klientkod.</span><span class="sxs-lookup"><span data-stu-id="27198-163">This metadata is what drives hello Swagger UI that you use in hello following steps, and you use it in a later section of hello tutorial tooautomatically generate client code.</span></span>
   
        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },
4. <span data-ttu-id="27198-164">Stäng hello webbläsaren och stoppa felsökning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27198-164">Close hello browser and stop Visual Studio debugging.</span></span>
5. <span data-ttu-id="27198-165">I hello ToDoListDataAPI-projekt i **Solution Explorer**öppnar hello *App_Start\SwaggerConfig.cs* filen och rullar ned tooline 174 och Avkommentera hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="27198-165">In hello ToDoListDataAPI project in **Solution Explorer**, open hello *App_Start\SwaggerConfig.cs* file, then scroll down tooline 174 and uncomment hello following code.</span></span>
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    <span data-ttu-id="27198-166">Hej *SwaggerConfig.cs* filen skapas när du installerar hello Swashbuckle-paketet i ett projekt.</span><span class="sxs-lookup"><span data-stu-id="27198-166">hello *SwaggerConfig.cs* file is created when you install hello Swashbuckle package in a project.</span></span> <span data-ttu-id="27198-167">hello-filen innehåller ett antal sätt tooconfigure Swashbuckle.</span><span class="sxs-lookup"><span data-stu-id="27198-167">hello file provides a number of ways tooconfigure Swashbuckle.</span></span>
   
    <span data-ttu-id="27198-168">hello-kod som du har tagit bort kommentarstecken från aktiverar hello Swagger-användargränssnitt som du använder i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="27198-168">hello code you've uncommented enables hello Swagger UI that you use in hello following steps.</span></span> <span data-ttu-id="27198-169">När du skapar ett Web API-projekt med hjälp av projektmallen för hello API-app är koden kommenterade som standard som en säkerhetsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="27198-169">When you create a Web API project by using hello API app project template, this code is commented out by default as a security measure.</span></span>
6. <span data-ttu-id="27198-170">Kör hello projektet igen.</span><span class="sxs-lookup"><span data-stu-id="27198-170">Run hello project again.</span></span>
7. <span data-ttu-id="27198-171">I webbläsarens adressfält, lägger du till `swagger` toohello slutet av hello rad och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="27198-171">In your browser address bar, add `swagger` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="27198-172">(URL: en är hello `http://localhost:45914/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="27198-172">(hello URL is `http://localhost:45914/swagger`.)</span></span>
8. <span data-ttu-id="27198-173">När hello Swaggers Användargränssnittssida visas klickar du på **ToDoList** toosee hello-metoder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="27198-173">When hello Swagger UI page appears, click **ToDoList** toosee hello methods available.</span></span>
   
    ![Tillgängliga metoder för Swagger-användargränssnittet](./media/app-service-api-dotnet-get-started/methods.png)
9. <span data-ttu-id="27198-175">Klicka på hello först **hämta** knappen i hello lista.</span><span class="sxs-lookup"><span data-stu-id="27198-175">Click hello first **Get** button in hello list.</span></span>
10. <span data-ttu-id="27198-176">I hello **parametrar** ange en asterisk som hello värde för hello `owner` parameter och klicka sedan på **prova**.</span><span class="sxs-lookup"><span data-stu-id="27198-176">In hello **Parameters** section, enter an asterisk as hello value of hello `owner` parameter, and then click **Try it out**.</span></span>
    
    <span data-ttu-id="27198-177">När du lägger till autentisering under senare kurser ger hello mellannivån hello faktiska användar-ID toohello datanivå.</span><span class="sxs-lookup"><span data-stu-id="27198-177">When you add authentication in later tutorials, hello middle tier will provide hello actual user ID toohello data tier.</span></span> <span data-ttu-id="27198-178">För tillfället har alla uppgifter asterisk som ägar-ID medan programmet hello körs utan autentisering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="27198-178">For now, all tasks will have asterisk as their owner ID while hello application runs without authentication enabled.</span></span>
    
    ![Prova Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    <span data-ttu-id="27198-180">Hej Swagger-användargränssnitt anropar hello ToDoList Get-metoden och visar hello svarskoden och JSON-resultat.</span><span class="sxs-lookup"><span data-stu-id="27198-180">hello Swagger UI calls hello ToDoList Get method and displays hello response code and JSON results.</span></span>
    
    ![Resultat av att prova Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. <span data-ttu-id="27198-182">Klicka på **Post**, och klicka sedan på hello rutan under **modellschemat**.</span><span class="sxs-lookup"><span data-stu-id="27198-182">Click **Post**, and then click hello box under **Model Schema**.</span></span>
    
    <span data-ttu-id="27198-183">Klicka på hello modellen schemat prefills hello textrutan där du kan ange hello parametervärdet för hello Post-metoden.</span><span class="sxs-lookup"><span data-stu-id="27198-183">Clicking hello model schema prefills hello input box where you can specify hello parameter value for hello Post method.</span></span> <span data-ttu-id="27198-184">(Om det inte fungerar i Internet Explorer, en annan webbläsare eller ange hello parametervärdet manuellt i hello nästa steg.)</span><span class="sxs-lookup"><span data-stu-id="27198-184">(If this doesn't work in Internet Explorer, use a different browser or enter hello parameter value manually in hello next step.)</span></span>  
    
    ![Prova att posta i Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/post.png)
12. <span data-ttu-id="27198-186">Ändra hello JSON i hello `todo` parametern indata så att det ser ut som följande exempel hello eller ersätta din egen beskrivningstext:</span><span class="sxs-lookup"><span data-stu-id="27198-186">Change hello JSON in hello `todo` parameter input box so that it looks like hello following example, or substitute your own description text:</span></span>
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. <span data-ttu-id="27198-187">Klicka på **Prova**.</span><span class="sxs-lookup"><span data-stu-id="27198-187">Click **Try it out**.</span></span>
    
    <span data-ttu-id="27198-188">Hej ToDoList API: N returnerar en HTTP 204-svarskod som indikerar att det lyckades.</span><span class="sxs-lookup"><span data-stu-id="27198-188">hello ToDoList API returns an HTTP 204 response code that indicates success.</span></span>
14. <span data-ttu-id="27198-189">Klicka på hello först **hämta** knappen och klicka sedan på hello i det avsnittet av sidan hello **prova** knappen.</span><span class="sxs-lookup"><span data-stu-id="27198-189">Click hello first **Get** button, and then in that section of hello page click hello **Try it out** button.</span></span>
    
    <span data-ttu-id="27198-190">hello hämta-metodsvaret innehåller nu hello nytt toodo objekt.</span><span class="sxs-lookup"><span data-stu-id="27198-190">hello Get method response now includes hello new toodo item.</span></span>
15. <span data-ttu-id="27198-191">Valfritt: Prova också hello Put, Delete, och hämta med ID-metoderna.</span><span class="sxs-lookup"><span data-stu-id="27198-191">Optional: Try also hello Put, Delete, and Get by ID methods.</span></span>
16. <span data-ttu-id="27198-192">Stäng hello webbläsaren och stoppa felsökning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27198-192">Close hello browser and stop Visual Studio debugging.</span></span>

<span data-ttu-id="27198-193">Swashbuckle fungerar med alla ASP.NET Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="27198-193">Swashbuckle works with any ASP.NET Web API project.</span></span> <span data-ttu-id="27198-194">Om du vill tooadd Swagger-metadata generation tooan befintligt projekt installerar du bara hello Swashbuckle-paketet.</span><span class="sxs-lookup"><span data-stu-id="27198-194">If you want tooadd Swagger metadata generation tooan existing project, just install hello Swashbuckle package.</span></span>

> [!NOTE]
> <span data-ttu-id="27198-195">Swagger-metadata innehåller ett unikt ID för varje API-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="27198-195">Swagger metadata includes a unique ID for each API operation.</span></span> <span data-ttu-id="27198-196">Som standard kan Swashbuckle generera duplicerade Swagger-åtgärds-ID till dina kontrollantmetoder för Web API. </span><span class="sxs-lookup"><span data-stu-id="27198-196">By default, Swashbuckle may generate duplicate Swagger operation IDs for your Web API controller methods.</span></span> <span data-ttu-id="27198-197">Det händer om din kontrollant har överbelastade http-metoder, exempelvis `Get()` och `Get(id)`.</span><span class="sxs-lookup"><span data-stu-id="27198-197">This happens if your controller has overloaded HTTP methods, such as `Get()` and `Get(id)`.</span></span> <span data-ttu-id="27198-198">Information om hur toohandle overloads finns [anpassa Swashbuckle-genererade API-definitioner](app-service-api-dotnet-swashbuckle-customize.md).</span><span class="sxs-lookup"><span data-stu-id="27198-198">For information about how toohandle overloads, see [Customize Swashbuckle-generated API definitions](app-service-api-dotnet-swashbuckle-customize.md).</span></span> <span data-ttu-id="27198-199">Om du skapar ett Web API-projekt i Visual Studio med hjälp av mallen för hello Azure API Apps läggs koden som skapar unika åtgärds-ID automatiskt toohello *SwaggerConfig.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="27198-199">If you create a Web API project in Visual Studio by using hello Azure API App template, code that generates unique operation IDs is automatically added toohello *SwaggerConfig.cs* file.</span></span>  
> 
> 

## <span data-ttu-id="27198-200"><a id="createapiapp"></a>Skapa en API-app i Azure och distribuera kod tooit</span><span class="sxs-lookup"><span data-stu-id="27198-200"><a id="createapiapp"></a> Create an API app in Azure and deploy code tooit</span></span>
<span data-ttu-id="27198-201">I det här avsnittet kan du använda Azure-verktyg som är integrerade i hello Visual Studio **Publicera webbplats** guiden toocreate ett nytt API-app i Azure.</span><span class="sxs-lookup"><span data-stu-id="27198-201">In this section, you use Azure tools that are integrated into hello Visual Studio **Publish Web** wizard toocreate a new API app in Azure.</span></span> <span data-ttu-id="27198-202">Du distribuerar sedan hello ToDoListDataAPI-projektet toohello nya API-app och anropa hello API genom att köra hello Swagger-användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="27198-202">Then you deploy hello ToDoListDataAPI project toohello new API app and call hello API by running hello Swagger UI.</span></span>

1. <span data-ttu-id="27198-203">I **Solution Explorer**, högerklicka på hello ToDoListDataAPI-projektet och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="27198-203">In **Solution Explorer**, right-click hello ToDoListDataAPI project, and then click **Publish**.</span></span>
   
    ![Klicka på Publicera i Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. <span data-ttu-id="27198-205">I hello **profil** steg i hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="27198-205">In hello **Profile** step of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
   
   ![Klicka på Azure Apptjänst i Publicera webbplats](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. <span data-ttu-id="27198-207">Logga in tooyour Azure-konto om du inte redan har gjort det eller uppdatera dina autentiseringsuppgifter om de upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="27198-207">Sign in tooyour Azure account if you have not already done so, or refresh your credentials if they're expired.</span></span>
4. <span data-ttu-id="27198-208">I dialogrutan för Apptjänst hello, Välj hello Azure **prenumeration** du toouse, och klickar sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="27198-208">In hello App Service dialog box, choose hello Azure **Subscription** you want toouse, and then click **New**.</span></span>
   
    ![Klicka på Ny i dialogrutan för Apptjänst](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    <span data-ttu-id="27198-210">Hej **värd** för hello **skapa App Service** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="27198-210">hello **Hosting** tab of hello **Create App Service** dialog box appears.</span></span>
   
    <span data-ttu-id="27198-211">Eftersom du distribuerar ett Web API-projekt där Swashbuckle är installerat, förutsätter Visual Studio som du vill toocreate en API-App.</span><span class="sxs-lookup"><span data-stu-id="27198-211">Because you're deploying a Web API project that has Swashbuckle installed, Visual Studio assumes that you want toocreate an API App.</span></span> <span data-ttu-id="27198-212">Detta anges av hello **API App Name** title och av hello faktum att hello **Ändringstypen** listrutan har angetts för**API-App**.</span><span class="sxs-lookup"><span data-stu-id="27198-212">This is indicated by hello **API App Name** title and by hello fact that hello **Change Type** drop-down list is set too**API App**.</span></span>
   
    ![Typ av app i dialogrutan för Apptjänst](./media/app-service-api-dotnet-get-started/apptype.png)
5. <span data-ttu-id="27198-214">Ange en **API App Name** som är unikt i hello *azurewebsites.net* domän.</span><span class="sxs-lookup"><span data-stu-id="27198-214">Enter an **API App Name** that is unique in hello *azurewebsites.net* domain.</span></span> <span data-ttu-id="27198-215">Du kan acceptera hello standardnamnet som Visual Studio föreslår.</span><span class="sxs-lookup"><span data-stu-id="27198-215">You can accept hello default name that Visual Studio proposes.</span></span>
   
    <span data-ttu-id="27198-216">Om du anger ett namn som någon annan redan har använt visas ett rött utropstecken toohello höger.</span><span class="sxs-lookup"><span data-stu-id="27198-216">If you enter a name that someone else has already used, you see a red exclamation mark toohello right.</span></span>
   
    <span data-ttu-id="27198-217">hello Webbadressen till hello API-app kommer att `{API app name}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="27198-217">hello URL of hello API app will be `{API app name}.azurewebsites.net`.</span></span>
6. <span data-ttu-id="27198-218">I hello **resursgruppen** listrutan, klickar du på **ny**, och ange sedan ”ToDoListGroup” eller ett annat namn om du föredrar.</span><span class="sxs-lookup"><span data-stu-id="27198-218">In hello **Resource Group** drop-down, click **New**, and then enter "ToDoListGroup" or another name if you prefer.</span></span>
   
    <span data-ttu-id="27198-219">En resursgrupp är en samling Azure-resurser, till exempel API Apps, databaser eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="27198-219">A resource group is a collection of Azure resources such as API apps, databases, VMs, and so forth.</span></span>    <span data-ttu-id="27198-220">Den här kursen är det bästa toocreate en ny resursgrupp eftersom som gör det enkelt toodelete i ett steg som alla hello Azure-resurser som du skapar för kursen hello.</span><span class="sxs-lookup"><span data-stu-id="27198-220">For this tutorial, it's best toocreate a new resource group because that makes it easy toodelete in one step all hello Azure resources that you create for hello tutorial.</span></span>
   
    <span data-ttu-id="27198-221">I den här rutan kan du välja en befintlig [resursgrupp](../azure-resource-manager/resource-group-overview.md) eller skapa en ny genom att skriva in ett namn som skiljer sig från befintliga resursgrupper i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="27198-221">This box lets you select an existing [resource group](../azure-resource-manager/resource-group-overview.md) or create a new one by typing in a name that is different from any existing resource group in your subscription.</span></span>
7. <span data-ttu-id="27198-222">Klicka på hello **ny** knappen Nästa toohello **Apptjänstplan** listrutan.</span><span class="sxs-lookup"><span data-stu-id="27198-222">Click hello **New** button next toohello **App Service Plan** drop-down.</span></span>
   
    <span data-ttu-id="27198-223">hello skärmbilden visar exempelvärden för **API App Name**, **prenumeration**, och **resursgruppen** --värdena är olika.</span><span class="sxs-lookup"><span data-stu-id="27198-223">hello screen shot shows sample values for **API App Name**, **Subscription**, and **Resource Group** -- your values will be different.</span></span>
   
    ![Dialogrutan Skapa App Service](./media/app-service-api-dotnet-get-started/createas.png)
   
    <span data-ttu-id="27198-225">I följande hello skapar du en apptjänstplan för hello nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="27198-225">In hello following steps you create an App Service plan for hello new resource group.</span></span> <span data-ttu-id="27198-226">En apptjänstplan anger hello beräkningsresurserna som API-appen körs på.</span><span class="sxs-lookup"><span data-stu-id="27198-226">An App Service plan specifies hello compute resources that your API app runs on.</span></span> <span data-ttu-id="27198-227">Till exempel om du väljer hello kostnadsfria nivån API-appen körs på delade virtuella datorer medan den körs på dedikerade virtuella datorer för vissa betalnivåer.</span><span class="sxs-lookup"><span data-stu-id="27198-227">For example, if you choose hello free tier, your API app runs on shared VMs, while for some paid tiers it runs on dedicated VMs.</span></span> <span data-ttu-id="27198-228">Information om apptjänstplaner finns i [Översikt över Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span><span class="sxs-lookup"><span data-stu-id="27198-228">For information about App Service plans, see [App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
8. <span data-ttu-id="27198-229">I hello **konfigurera Apptjänstplan** dialogrutan anger du ”ToDoListPlan” eller ett annat namn om du föredrar.</span><span class="sxs-lookup"><span data-stu-id="27198-229">In hello **Configure App Service Plan** dialog, enter "ToDoListPlan" or another name if you prefer.</span></span>
9. <span data-ttu-id="27198-230">I hello **plats** listrutan väljer du hello-plats som är närmast tooyou.</span><span class="sxs-lookup"><span data-stu-id="27198-230">In hello **Location** drop-down list, choose hello location that is closest tooyou.</span></span>
   
    <span data-ttu-id="27198-231">Den här inställningen anger vilket Azure-datacenter appen ska köras i.</span><span class="sxs-lookup"><span data-stu-id="27198-231">This setting specifies which Azure datacenter your app will run in.</span></span> <span data-ttu-id="27198-232">Välj en plats Stäng tooyou toominimize [svarstid](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span><span class="sxs-lookup"><span data-stu-id="27198-232">Choose a location close tooyou toominimize [latency](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span></span>
10. <span data-ttu-id="27198-233">I hello **storlek** listrutan, klickar du på **lediga**.</span><span class="sxs-lookup"><span data-stu-id="27198-233">In hello **Size** drop-down, click **Free**.</span></span>
    
    <span data-ttu-id="27198-234">I den här självstudien ger hello kostnadsfria prisnivån tillräcklig prestanda.</span><span class="sxs-lookup"><span data-stu-id="27198-234">For this tutorial, hello free pricing tier will provide sufficient performance.</span></span>
11. <span data-ttu-id="27198-235">I hello **konfigurera Apptjänstplan** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="27198-235">In hello **Configure App Service Plan** dialog, click **OK**.</span></span>
    
    ![Klicka på OK i Konfigurera Apptjänstplan](./media/app-service-api-dotnet-get-started/configasp.png)
12. <span data-ttu-id="27198-237">I hello **skapa Apptjänst** dialogrutan klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="27198-237">In hello **Create App Service** dialog box, click **Create**.</span></span>
    
    ![Klicka på Skapa i dialogrutan Skapa Apptjänst](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    <span data-ttu-id="27198-239">Visual Studio skapar hello API-app och en publiceringsprofil som innehåller alla de hello krävs inställningarna för hello API-appen.</span><span class="sxs-lookup"><span data-stu-id="27198-239">Visual Studio creates hello API app and a publish profile that has all of hello required settings for hello API app.</span></span> <span data-ttu-id="27198-240">Sedan öppnas hello **Publicera webbplats** guiden som du ska använda toodeploy hello projektet.</span><span class="sxs-lookup"><span data-stu-id="27198-240">Then it opens hello **Publish Web** wizard, which you'll use toodeploy hello project.</span></span>
    
    <span data-ttu-id="27198-241">Hej **Publicera webbplats** öppnas på hello **anslutning** (se nedan).</span><span class="sxs-lookup"><span data-stu-id="27198-241">hello **Publish Web** wizard opens on hello **Connection** tab (shown below).</span></span>
    
    <span data-ttu-id="27198-242">På hello **anslutning** fliken hello **Server** och **platsnamn** inställningar punkt tooyour API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-242">On hello **Connection** tab, hello **Server** and **Site name** settings point tooyour API app.</span></span> <span data-ttu-id="27198-243">Hej **användarnamn** och **lösenord** är autentiseringsuppgifter för distribution som Azure skapar åt dig.</span><span class="sxs-lookup"><span data-stu-id="27198-243">hello **User name** and **Password** are deployment credentials that Azure creates for you.</span></span> <span data-ttu-id="27198-244">Efter distributionen öppnar Visual Studio en webbläsare toohello **Måladress** (som är hello enda syftet med **Måladress**).</span><span class="sxs-lookup"><span data-stu-id="27198-244">After deployment, Visual Studio opens a browser toohello **Destination URL** (that's hello only purpose for **Destination URL**).</span></span>  
13. <span data-ttu-id="27198-245">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27198-245">Click **Next**.</span></span>
    
    ![Klicka på Nästa på fliken Anslutning i Publicera webbplats](./media/app-service-api-dotnet-get-started/connnext.png)
    
    <span data-ttu-id="27198-247">hello nästa flik är hello **inställningar** (se nedan).</span><span class="sxs-lookup"><span data-stu-id="27198-247">hello next tab is hello **Settings** tab (shown below).</span></span> <span data-ttu-id="27198-248">Här kan du ändra hello build configuration fliken toodeploy en felsökningsversion för [fjärrfelsökning](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span><span class="sxs-lookup"><span data-stu-id="27198-248">Here you can change hello build configuration tab toodeploy a debug build for [remote debugging](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span></span> <span data-ttu-id="27198-249">hello fliken finns också flera **alternativ för Filpublicering**:</span><span class="sxs-lookup"><span data-stu-id="27198-249">hello tab also offers several **File Publish Options**:</span></span>
    
    * <span data-ttu-id="27198-250">Ta bort extra filer från destinationen</span><span class="sxs-lookup"><span data-stu-id="27198-250">Remove additional files at destination</span></span>
    * <span data-ttu-id="27198-251">Förkompilera vid publicering</span><span class="sxs-lookup"><span data-stu-id="27198-251">Precompile during publishing</span></span>
    * <span data-ttu-id="27198-252">Undanta filer från hello App_Data mapp</span><span class="sxs-lookup"><span data-stu-id="27198-252">Exclude files from hello App_Data folder</span></span>
    
    <span data-ttu-id="27198-253">Under den här kursen behöver du ingen av dem.</span><span class="sxs-lookup"><span data-stu-id="27198-253">For this tutorial you don't need any of these.</span></span> <span data-ttu-id="27198-254">Utförliga förklaringar av vad de gör finns i [Så här distribuerar du ett webbprojekt med Publicera med ett klick i Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).</span><span class="sxs-lookup"><span data-stu-id="27198-254">For detailed explanations of what they do, see [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).</span></span>
14. <span data-ttu-id="27198-255">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27198-255">Click **Next**.</span></span>
    
    ![Klicka på Nästa på fliken Inställningar i Publicera webbplats](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    <span data-ttu-id="27198-257">Nästa är hello **Preview** fliken (se nedan), vilket ger dig en möjlighet toosee vilka filer som ska kopieras från project toohello API-app toobe.</span><span class="sxs-lookup"><span data-stu-id="27198-257">Next is hello **Preview** tab (shown below), which gives you an opportunity toosee what files are going toobe copied from your project toohello API app.</span></span> <span data-ttu-id="27198-258">När du distribuerar ett projekt tooan API-app som du redan har distribuerat tooearlier, kopieras bara ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="27198-258">When you're deploying a project tooan API app that you already deployed tooearlier, only changed files are copied.</span></span> <span data-ttu-id="27198-259">Om du vill toosee en lista över vad som ska kopieras, kan du klicka på hello **starta förhandsgranskning** knappen.</span><span class="sxs-lookup"><span data-stu-id="27198-259">If you want toosee a list of what will be copied, you can click hello **Start Preview** button.</span></span>
15. <span data-ttu-id="27198-260">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="27198-260">Click **Publish**.</span></span>
    
    ![Klicka på Publicera på fliken Förhandsgranska i Publicera webbplats](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    <span data-ttu-id="27198-262">Visual Studio distribuerar hello ToDoListDataAPI-projektet toohello nya API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-262">Visual Studio deploys hello ToDoListDataAPI project toohello new API app.</span></span> <span data-ttu-id="27198-263">Hej **utdata** loggar slutförd distribution och en ”har skapats” visas i en webbläsare öppnas fönstret toohello URL hello API-appen.</span><span class="sxs-lookup"><span data-stu-id="27198-263">hello **Output** window logs successful deployment, and a "successfully created" page appears in a browser window opened toohello URL of hello API app.</span></span>
    
    ![Utdatafönster visar slutförd distribution](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Sida som visar att en ny API-app har skapats](./media/app-service-api-dotnet-get-started/appcreated.png)
16. <span data-ttu-id="27198-266">Lägg till ”swagger” toohello URL hello webbläsarens adressfält och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="27198-266">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="27198-267">(URL: en är hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="27198-267">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
    
    <span data-ttu-id="27198-268">hello webbläsaren visar samma Swagger-användargränssnitt som du såg tidigare, men nu körs i molnet hello hello.</span><span class="sxs-lookup"><span data-stu-id="27198-268">hello browser displays hello same Swagger UI that you saw earlier, but now it's running in hello cloud.</span></span> <span data-ttu-id="27198-269">Testa hello Get-metoden och du ser att du är tillbaka toohello standard 2 att göra-objekt.</span><span class="sxs-lookup"><span data-stu-id="27198-269">Try out hello Get method, and you see that you're back toohello default 2 to-do items.</span></span> <span data-ttu-id="27198-270">hello ändringar du gjort tidigare har sparats i minnet i hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="27198-270">hello changes you made earlier were saved in memory in hello local machine.</span></span>
17. <span data-ttu-id="27198-271">Öppna hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="27198-271">Open hello [Azure portal](https://portal.azure.com/).</span></span>
    
    <span data-ttu-id="27198-272">hello Azure-portalen är ett webbgränssnitt för att hantera Azure-resurser, till exempel API apps.</span><span class="sxs-lookup"><span data-stu-id="27198-272">hello Azure portal is a web interface for managing Azure resources such as API apps.</span></span>
18. <span data-ttu-id="27198-273">Klicka på **Fler tjänster > Apptjänster**.</span><span class="sxs-lookup"><span data-stu-id="27198-273">Click **More Services > App Services**.</span></span>
    
    ![Bläddra i Apptjänster](./media/app-service-api-dotnet-get-started/browseas.png)
19. <span data-ttu-id="27198-275">I hello **Apptjänster** bladet och klicka på din nya API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-275">In hello **App Services** blade, find and click your new API app.</span></span> <span data-ttu-id="27198-276">(I hello Azure-portalen kallas fönster som öppnas toohello höger *blad*.)</span><span class="sxs-lookup"><span data-stu-id="27198-276">(In hello Azure portal, windows that open toohello right are called *blades*.)</span></span>
    
    ![Apptjänstblad](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    <span data-ttu-id="27198-278">Två blad öppna.</span><span class="sxs-lookup"><span data-stu-id="27198-278">Two blades open.</span></span> <span data-ttu-id="27198-279">Det ena bladet visar en översikt över hello API-app och en har en lång lista med inställningar som du kan visa och ändra.</span><span class="sxs-lookup"><span data-stu-id="27198-279">One blade has an overview of hello API app, and one has a long list of settings that you can view and change.</span></span>
20. <span data-ttu-id="27198-280">I hello **inställningar** bladet, hitta hello **API** avsnittet och klicka på **API-Definition**.</span><span class="sxs-lookup"><span data-stu-id="27198-280">In hello **Settings** blade, find hello **API** section and click **API Definition**.</span></span>
    
    ![API-definition på bladet Inställningar](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    <span data-ttu-id="27198-282">Hej **API-Definition** bladet kan du ange hello-URL som returnerar Swagger 2.0-metadata i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="27198-282">hello **API Definition** blade lets you specify hello URL that returns Swagger 2.0 metadata in JSON format.</span></span> <span data-ttu-id="27198-283">När Visual Studio skapar hello API-app, anger hello API definition URL toohello standardvärde för Swashbuckle-genererade metadata som du såg tidigare, vilket är hello API-app grundläggande URL plus `/swagger/docs/v1`.</span><span class="sxs-lookup"><span data-stu-id="27198-283">When Visual Studio creates hello API app, it sets hello API definition URL toohello default value for Swashbuckle-generated metadata that you saw earlier, which is hello API app's base URL plus `/swagger/docs/v1`.</span></span>
    
    ![URL för API-definition](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    <span data-ttu-id="27198-285">När du väljer en API-app toogenerate klientkod för den hämtar Visual Studio hello metadata från denna URL.</span><span class="sxs-lookup"><span data-stu-id="27198-285">When you select an API app toogenerate client code for it, Visual Studio retrieves hello metadata from this URL.</span></span>

## <span data-ttu-id="27198-286"><a id="codegen"></a>Generera klientkod för hello datanivå</span><span class="sxs-lookup"><span data-stu-id="27198-286"><a id="codegen"></a> Generate client code for hello data tier</span></span>
<span data-ttu-id="27198-287">En av hello fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering.</span><span class="sxs-lookup"><span data-stu-id="27198-287">One of hello advantages of integrating Swagger into Azure API apps is automatic code generation.</span></span> <span data-ttu-id="27198-288">Genererade klientklasser gör det enklare toowrite kod som anropar en API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-288">Generated client classes make it easier toowrite code that calls an API app.</span></span>

<span data-ttu-id="27198-289">hello ToDoListAPI-projektet har redan hello genereras klientkoden, men i följande hello du ta bort den och återskapa den toosee hur toodo hello kodgenereringen.</span><span class="sxs-lookup"><span data-stu-id="27198-289">hello ToDoListAPI project already has hello generated client code, but in hello following steps you'll delete it and regenerate it toosee how toodo hello code generation.</span></span>

1. <span data-ttu-id="27198-290">I Visual Studio **Solution Explorer**, i hello ToDoListAPI-projektet, ta bort hello *ToDoListDataAPI* mapp.</span><span class="sxs-lookup"><span data-stu-id="27198-290">In Visual Studio **Solution Explorer**, in hello ToDoListAPI project, delete hello *ToDoListDataAPI* folder.</span></span> <span data-ttu-id="27198-291">**Varning: Ta bort endast hello mappen, inte hello ToDoListDataAPI-projektet.**</span><span class="sxs-lookup"><span data-stu-id="27198-291">**Caution: Delete only hello folder, not hello ToDoListDataAPI project.**</span></span>
   
    ![Ta bort genererad klientkod](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    <span data-ttu-id="27198-293">Den här mappen har skapats med hjälp av hello kodgenereringsprocess som du är om toogo via.</span><span class="sxs-lookup"><span data-stu-id="27198-293">This folder was created by using hello code generation process that you're about toogo through.</span></span>
2. <span data-ttu-id="27198-294">Högerklicka på hello ToDoListAPI-projektet och klicka sedan på **Lägg till > REST API-klient**.</span><span class="sxs-lookup"><span data-stu-id="27198-294">Right-click hello ToDoListAPI project, and then click **Add > REST API Client**.</span></span>
   
    ![Lägg till REST API-klienten i Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. <span data-ttu-id="27198-296">I hello **Lägg till REST API-klient** dialogrutan klickar du på **Swagger URL**, och klicka sedan på **Välj Azure-tillgång**.</span><span class="sxs-lookup"><span data-stu-id="27198-296">In hello **Add REST API Client** dialog box, click **Swagger URL**, and then click **Select Azure Asset**.</span></span>
   
    ![Välj Azure-tillgång](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. <span data-ttu-id="27198-298">I hello **Apptjänst** dialogrutan expanderar hello resursgruppen som du använder för den här kursen väljer din API-app och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="27198-298">In hello **App Service** dialog box, expand hello resource group you're using for this tutorial and select your API app, and then click **OK**.</span></span>
   
    ![Välj API-app för kodgenerering](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    <span data-ttu-id="27198-300">Observera att när du går tillbaka toohello **Lägg till REST API-klient** dialogrutan hello textruta har fyllts i med hello API-definition URL-värdet som du såg tidigare i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="27198-300">Notice that when you return toohello **Add REST API Client** dialog, hello text box has been filled in with hello API definition URL value that you saw earlier in hello portal.</span></span>
   
    ![API-definitions-URL](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > <span data-ttu-id="27198-302">Ett annat sätt tooget metadata för kodgenerering är tooenter hello URL direkt i stället för att gå igenom hello bläddringsdialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27198-302">An alternative way tooget metadata for code generation is tooenter hello URL directly instead of going through hello browse dialog.</span></span> <span data-ttu-id="27198-303">Om du vill toogenerate klientkoden innan du distribuerar tooAzure, kan du köra hello Web API-projekt lokalt, gå toohello URL som innehåller hello Swagger JSON-filen, sparar hello-filen och använda hello **Välj en befintlig Swagger-metadatafil**alternativet.</span><span class="sxs-lookup"><span data-stu-id="27198-303">Or if you want toogenerate client code before deploying tooAzure, you could run hello Web API project locally, go toohello URL that provides hello Swagger JSON file, save hello file, and use hello **Select an existing Swagger metadata file** option.</span></span>
   > 
   > 
5. <span data-ttu-id="27198-304">I hello **Lägg till REST API-klient** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="27198-304">In hello **Add REST API Client** dialog box, click **OK**.</span></span>
   
    <span data-ttu-id="27198-305">Visual Studio skapar en mapp med namnet efter hello API-appen och genererar klientklasser.</span><span class="sxs-lookup"><span data-stu-id="27198-305">Visual Studio creates a folder named after hello API app and generates client classes.</span></span>
   
    ![Koda filer för en genererad klient](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. <span data-ttu-id="27198-307">Öppna i hello ToDoListAPI-projektet *Controllers\ToDoListController.cs* toosee hello koden på rad 40 som anropar API hello med hello genererade klienten.</span><span class="sxs-lookup"><span data-stu-id="27198-307">In hello ToDoListAPI project, open *Controllers\ToDoListController.cs* toosee hello code at line 40  that calls hello API by using hello generated client.</span></span>
   
    <span data-ttu-id="27198-308">hello följande utdrag visar hur koden hello instansierar hello objekt och anrop hello Get-metoden.</span><span class="sxs-lookup"><span data-stu-id="27198-308">hello following snippet shows how hello code instantiates hello client object and calls hello Get method.</span></span>
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
   
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }
   
    <span data-ttu-id="27198-309">Hej konstruktorparametern hämtar slutpunkts-URL för hello från hello `toDoListDataAPIURL` appinställningen.</span><span class="sxs-lookup"><span data-stu-id="27198-309">hello constructor parameter gets hello endpoint URL from  hello `toDoListDataAPIURL` app setting.</span></span> <span data-ttu-id="27198-310">Detta värde är i filen Web.config för hello set toohello lokala IIS Express URL för hello API-projekt så att du kan köra programmet hello lokalt.</span><span class="sxs-lookup"><span data-stu-id="27198-310">In hello Web.config file, that value is set toohello local IIS Express URL of hello API project so that you can run hello application locally.</span></span> <span data-ttu-id="27198-311">Om du utelämnar konstruktorparametern hello är hello standardslutpunkten hello-URL som du skapade hello koden från.</span><span class="sxs-lookup"><span data-stu-id="27198-311">If you omit hello constructor parameter, hello default endpoint is hello URL that you generated hello code from.</span></span>
7. <span data-ttu-id="27198-312">Klientklassen kommer att skapas med ett annat namn baserat på din API-appnamn; Ändra hello koden i *Controllers\ToDoListController.cs* så att hello typnamnet matchar det som har genererats i projektet.</span><span class="sxs-lookup"><span data-stu-id="27198-312">Your client class will be generated with a different name based on your API app name; change hello code in *Controllers\ToDoListController.cs* so that hello type name matches what was generated in your project.</span></span> <span data-ttu-id="27198-313">Om du till exempel har gett din API-app namnet ToDoListDataAPI071316 ändrar du den här koden:</span><span class="sxs-lookup"><span data-stu-id="27198-313">For example, if you named your API App ToDoListDataAPI071316, you would change this code:</span></span>
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

<span data-ttu-id="27198-314">toothis:</span><span class="sxs-lookup"><span data-stu-id="27198-314">toothis:</span></span>

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a><span data-ttu-id="27198-315">Skapa en API-app toohost hello mellannivå</span><span class="sxs-lookup"><span data-stu-id="27198-315">Create an API app toohost hello middle tier</span></span>
<span data-ttu-id="27198-316">Tidigare du [skapas hello-appen på datanivå-API och distribueras kod tooit](#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="27198-316">Earlier you [created hello data tier API app and deployed code tooit](#createapiapp).</span></span>  <span data-ttu-id="27198-317">Nu du följer hello samma procedur för hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-317">Now you follow hello same procedure for hello middle tier API app.</span></span>

1. <span data-ttu-id="27198-318">I **Solution Explorer**, högerklicka på hello mellannivå ToDoListAPI projektet (inte hello ToDoListDataAPI på datanivå) och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="27198-318">In **Solution Explorer**, right-click hello middle tier ToDoListAPI  project (not hello data tier ToDoListDataAPI), and then click **Publish**.</span></span>
   
    ![Klicka på Publicera i Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. <span data-ttu-id="27198-320">I hello **profil** för hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="27198-320">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="27198-321">I hello **Apptjänst** dialogrutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="27198-321">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="27198-322">I hello **värd** för hello **skapa Apptjänst** dialogrutan godkänner hello standard **API App Name** eller ange ett namn som är unikt i hello * azurewebsites.NET* domän.</span><span class="sxs-lookup"><span data-stu-id="27198-322">In hello **Hosting** tab of hello **Create App Service** dialog box, accept hello default **API App Name** or enter a name that is unique in hello *azurewebsites.net* domain.</span></span>
5. <span data-ttu-id="27198-323">Välj hello Azure **prenumeration** du har använt.</span><span class="sxs-lookup"><span data-stu-id="27198-323">Choose hello Azure **Subscription** you have been using.</span></span>
6. <span data-ttu-id="27198-324">I hello **resursgruppen** listrutan, Välj hello samma resursgrupp som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="27198-324">In hello **Resource Group** drop-down, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="27198-325">I hello **Apptjänstplan** listrutan, Välj hello samma plan som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="27198-325">In hello **App Service Plan** drop-down, choose hello same plan you created earlier.</span></span> <span data-ttu-id="27198-326">Som standard den toothat värde.</span><span class="sxs-lookup"><span data-stu-id="27198-326">It will default toothat value.</span></span>
8. <span data-ttu-id="27198-327">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="27198-327">Click **Create**.</span></span>
   
    <span data-ttu-id="27198-328">Visual Studio skapar hello API-appen, skapas en publiceringsprofil för den och visar hello **anslutning** steg i hello **Publicera webbplats** guiden.</span><span class="sxs-lookup"><span data-stu-id="27198-328">Visual Studio creates hello API app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
9. <span data-ttu-id="27198-329">I hello **anslutning** steg i hello **Publicera webbplats** guiden, klickar du på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="27198-329">In hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
   
   <span data-ttu-id="27198-330">Visual Studio distribuerar hello ToDoListAPI-projektet toohello nya API-appen och öppnar en webbläsare toohello Webbadressen för hello API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-330">Visual Studio deploys hello ToDoListAPI project toohello new API app and opens a browser toohello URL of hello API app.</span></span> <span data-ttu-id="27198-331">Hej ”har skapats” visas.</span><span class="sxs-lookup"><span data-stu-id="27198-331">hello "successfully created" page appears.</span></span>

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a><span data-ttu-id="27198-332">Konfigurera hello mellannivå toocall hello datanivå</span><span class="sxs-lookup"><span data-stu-id="27198-332">Configure hello middle tier toocall hello data tier</span></span>
<span data-ttu-id="27198-333">Om du nu kallas hello mellannivå-API-app, skulle den försöka toocall hello datanivån med hello localhost-URL som finns kvar i hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="27198-333">If you called hello middle tier API app now, it would try toocall hello data tier using hello localhost URL that is still in hello Web.config file.</span></span> <span data-ttu-id="27198-334">I det här avsnittet anger hello data nivå API-appens URL i en miljöinställning i hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-334">In this section you enter hello data tier API app URL into an environment setting in hello middle tier API app.</span></span> <span data-ttu-id="27198-335">När hello koden i hello mellannivå-API-appen hämtar inställningen för hello data nivå URL, åsidosätter hello vad som finns i hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="27198-335">When hello code in hello middle tier API app retrieves hello data tier URL setting, hello environment setting will override what's in hello Web.config file.</span></span>

1. <span data-ttu-id="27198-336">Gå toohello [Azure-portalen](https://portal.azure.com/), och sedan gå toohello **API-App** bladet för hello API-app som du skapade toohost hello TodoListAPI (mellannivå)-projektet.</span><span class="sxs-lookup"><span data-stu-id="27198-336">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **API App** blade for hello API app that you created toohost hello TodoListAPI (middle tier) project.</span></span>
2. <span data-ttu-id="27198-337">Hej API-App i **inställningar** bladet, klickar du på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="27198-337">In hello API App's **Settings** blade, click **Application settings**.</span></span>
3. <span data-ttu-id="27198-338">Hej API-App i **programinställningar** rullar du ned toohello **appinställningar** och lägger till hello följande nyckel och värde.</span><span class="sxs-lookup"><span data-stu-id="27198-338">In hello API App's **Application Settings** blade, scroll down toohello **App settings** section and add hello following key and value.</span></span> <span data-ttu-id="27198-339">hello värdet kommer att vara hello URL för hello första API App som du publicerade i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="27198-339">hello value will be hello URL of hello first API App you published in this tutorial.</span></span>
   
   | <span data-ttu-id="27198-340">**Nyckel**</span><span class="sxs-lookup"><span data-stu-id="27198-340">**Key**</span></span> | <span data-ttu-id="27198-341">toDoListDataAPIURL</span><span class="sxs-lookup"><span data-stu-id="27198-341">toDoListDataAPIURL</span></span> |
   | --- | --- |
   | <span data-ttu-id="27198-342">**Värde**</span><span class="sxs-lookup"><span data-stu-id="27198-342">**Value**</span></span> |<span data-ttu-id="27198-343">https://{namnet på din API på datanivå}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="27198-343">https://{your data tier API app name}.azurewebsites.net</span></span> |
   | <span data-ttu-id="27198-344">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="27198-344">**Example**</span></span> |<span data-ttu-id="27198-345">https://todolistdataapi.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="27198-345">https://todolistdataapi.azurewebsites.net</span></span> |
4. <span data-ttu-id="27198-346">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="27198-346">Click **Save**.</span></span>
   
    ![Klicka på Spara för app-inställningar](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    <span data-ttu-id="27198-348">När hello koden körs i Azure, åsidosätter det här värdet hello localhost-URL som är i hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="27198-348">When hello code runs in Azure, this value will now override hello localhost URL that is in hello Web.config file.</span></span>

## <a name="test"></a><span data-ttu-id="27198-349">Testa</span><span class="sxs-lookup"><span data-stu-id="27198-349">Test</span></span>
1. <span data-ttu-id="27198-350">I ett webbläsarfönster bläddrar du toohello URL för hello nya mellannivå-API-app som du nyss skapade för ToDoListAPI.</span><span class="sxs-lookup"><span data-stu-id="27198-350">In a browser window, browse toohello URL of hello new middle tier API app that you just created for ToDoListAPI.</span></span> <span data-ttu-id="27198-351">Du kan hämta det genom att klicka på hello URL: en i hello API-appens huvudblad i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="27198-351">You can get there by clicking hello URL in hello API app's main blade in hello portal.</span></span>
2. <span data-ttu-id="27198-352">Lägg till ”swagger” toohello URL hello webbläsarens adressfält och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="27198-352">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="27198-353">(URL: en är hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="27198-353">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
   
    <span data-ttu-id="27198-354">hello webbläsaren visar hello samma Swagger-användargränssnitt som du tidigare såg för ToDoListDataAPI, men nu `owner` är inte ett obligatoriskt fält för hello hämta-åtgärden eftersom hello mellannivå-API-appen skickar den värdet toohello API-appen på datanivå åt dig.</span><span class="sxs-lookup"><span data-stu-id="27198-354">hello browser displays hello same Swagger UI that you saw earlier for ToDoListDataAPI, but now `owner` is not a required field for hello Get operation, because hello middle tier API app is sending that value toohello data tier API app for you.</span></span> <span data-ttu-id="27198-355">(När du hello autentisering självstudier, skickar hello mellannivån faktiska användar-ID för hello `owner` parametern; för nu den hårdkodar det en asterisk.)</span><span class="sxs-lookup"><span data-stu-id="27198-355">(When you do hello authentication tutorials, hello middle tier will send actual user IDs for hello `owner` parameter; for now it is hard-coding an asterisk.)</span></span>
3. <span data-ttu-id="27198-356">Testa hello Get-metoden och hello toovalidate andra metoder som hello API-appen på mellannivå anropar hello datanivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="27198-356">Try out hello Get method and hello other methods toovalidate that hello middle tier API app is successfully calling hello data tier API app.</span></span>
   
    ![Hämta-metoden i Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a><span data-ttu-id="27198-358">Felsökning</span><span class="sxs-lookup"><span data-stu-id="27198-358">Troubleshooting</span></span>
<span data-ttu-id="27198-359">Om du stöter på problem när du går igenom den här kursen får du några felsökningsidéer här:</span><span class="sxs-lookup"><span data-stu-id="27198-359">In case you run into a problem as you go through this tutorial here are some troubleshooting ideas:</span></span>

* <span data-ttu-id="27198-360">Kontrollera att du använder hello senaste versionen av hello [Azure SDK för .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="27198-360">Make sure that you're using hello latest version of hello [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="27198-361">Två av projektnamnen hello är liknar varandra (ToDoListAPI, ToDoListDataAPI).</span><span class="sxs-lookup"><span data-stu-id="27198-361">Two of hello project names are similar (ToDoListAPI, ToDoListDataAPI).</span></span> <span data-ttu-id="27198-362">Om saker ser inte ut som beskrivs i hello anvisningar när du arbetar med ett projekt, kontrollera att du har öppnat hello rätt projekt.</span><span class="sxs-lookup"><span data-stu-id="27198-362">If things don't look as described in hello instructions when you are working with a project, make sure you have opened hello correct project.</span></span>
* <span data-ttu-id="27198-363">Om du arbetar i ett företagsnätverk och försöker toodeploy tooAzure Apptjänst via en brandvägg kan du kontrollera att portarna 443 och 8172 är öppna för webbdistribution.</span><span class="sxs-lookup"><span data-stu-id="27198-363">If you're on a corporate network and are trying toodeploy tooAzure App Service through a firewall, make sure that ports 443 and 8172 are open for Web Deploy.</span></span> <span data-ttu-id="27198-364">Om du inte kan öppna de portarna kan du använda andra metoder för distribution.</span><span class="sxs-lookup"><span data-stu-id="27198-364">If you can't open those ports, you can use other deployment methods.</span></span>  <span data-ttu-id="27198-365">Se [distribuera din app tooAzure Apptjänst](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="27198-365">See [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md).</span></span>
* <span data-ttu-id="27198-366">Fel ”ruttnamn måste vara unika” – du kan få dem om du av misstag distribuerar hello fel projekt tooan API-appen och sedan distribuera hello rätt en tooit.</span><span class="sxs-lookup"><span data-stu-id="27198-366">"Route names must be unique" errors -- you could get these if you accidentally deploy hello wrong project tooan API app and then later deploy hello correct one tooit.</span></span> <span data-ttu-id="27198-367">toocorrect, Omdistributionen hello rätt projekt toohello API appen, och på hello **inställningar** för hello **Publicera webbplats** guiden Välj **ta bort extra filer från destinationen**.</span><span class="sxs-lookup"><span data-stu-id="27198-367">toocorrect this, redeploy hello correct project toohello API app, and on hello **Settings** tab of hello **Publish Web** wizard select **Remove additional files at destination**.</span></span>

<span data-ttu-id="27198-368">När du har din ASP.NET API-app som körs i Azure App Service kanske toolearn mer om Visual Studio-funktioner som förenklar felsökningen.</span><span class="sxs-lookup"><span data-stu-id="27198-368">After you have your ASP.NET API app running in Azure App Service, you may want toolearn more about Visual Studio features that simplify troubleshooting.</span></span> <span data-ttu-id="27198-369">Information om bland annat loggning och fjärrfelsökning finns i [Felsöka Azure Apptjänst-appar i Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="27198-369">For information about logging, remote debugging, and more, see  [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="27198-370">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27198-370">Next steps</span></span>
<span data-ttu-id="27198-371">Du har sett hur toodeploy befintliga Web API-projekt tooAPI apps, genererar klientkod till API apps och använder API apps från .NET-klienter.</span><span class="sxs-lookup"><span data-stu-id="27198-371">You've seen how toodeploy existing Web API projects tooAPI apps, generate client code for API apps, and consume API apps from .NET clients.</span></span> <span data-ttu-id="27198-372">hello nästa kurs i serien visar hur för[CORS tooconsume API apps från JavaScript-klienter använda](app-service-api-cors-consume-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="27198-372">hello next tutorial in this series shows how too[use CORS tooconsume API apps from JavaScript clients](app-service-api-cors-consume-javascript.md).</span></span>

<span data-ttu-id="27198-373">Mer information om klientkodgenerering finns hello [Azure/AutoRest](https://github.com/azure/autorest) på GitHub.com. Hjälp med problem med att använda hello genererade klienten, öppna ett [problem i hello AutoRest databasen](https://github.com/azure/autorest/issues).</span><span class="sxs-lookup"><span data-stu-id="27198-373">For more information about client code generation, see hello [Azure/AutoRest](https://github.com/azure/autorest) repository on GitHub.com. For help with problems using hello generated client, open an [issue in hello AutoRest repository](https://github.com/azure/autorest/issues).</span></span>

<span data-ttu-id="27198-374">Om du vill toocreate nya API-app-projekt från grunden använder hello **Azure API Apps** mall.</span><span class="sxs-lookup"><span data-stu-id="27198-374">If you want toocreate new API app projects from scratch, use hello **Azure API App** template.</span></span>

![Mall för API Apps i Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

<span data-ttu-id="27198-376">Hej **Azure API Apps** projektmall är likvärdiga toochoosing hello **tom** ASP.NET 4.5.2-mallen mall, klicka på hello kryssrutan tooadd Web API-stöd och installera hello Swashbuckle NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="27198-376">hello **Azure API App** project template is equivalent toochoosing hello **Empty** ASP.NET 4.5.2 template, clicking hello check box tooadd Web API support, and installing hello Swashbuckle NuGet package.</span></span> <span data-ttu-id="27198-377">Hello mallen lägger dessutom till vissa Swashbuckle kod som utformats för tooprevent hello Skapa konfiguration för duplicerade Swagger-åtgärds-ID: N.</span><span class="sxs-lookup"><span data-stu-id="27198-377">In addition, hello template adds some Swashbuckle configuration code designed tooprevent hello creation of duplicate Swagger operation IDs.</span></span> <span data-ttu-id="27198-378">När du har skapat ett API-App-projekt kan du distribuera den tooan API app hello samma sätt som du såg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="27198-378">Once you've created an API App project, you can deploy it tooan API app hello same way you saw in this tutorial.</span></span>

