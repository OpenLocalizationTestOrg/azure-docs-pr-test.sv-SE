---
title: "aaaCreate ett .NET-program för Service Fabric | Microsoft Docs"
description: "Lär dig hur toocreate ett program med en ASP.NET Core frontend och en tillförlitlig tillståndskänslig backend-tjänst och distribuera hello programmet tooa kluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: bab331b9f8616c50a2794b6c048aace15579c8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a><span data-ttu-id="8d619-103">Skapa och distribuera ett program med en webb-API för ASP.NET Core frontend-tjänst och en tillståndskänslig backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-103">Create and deploy an application with an ASP.NET Core Web API front-end service and a stateful back-end service</span></span>
<span data-ttu-id="8d619-104">Den här kursen ingår i en serie.</span><span class="sxs-lookup"><span data-stu-id="8d619-104">This tutorial is part one of a series.</span></span>  <span data-ttu-id="8d619-105">Du kommer lära dig hur toocreate ett Azure Service Fabric-program med en webb-API för ASP.NET Core framför slut och en tillståndskänslig backend-tjänst toostore dina data.</span><span class="sxs-lookup"><span data-stu-id="8d619-105">You will learn how toocreate an Azure Service Fabric application with an ASP.NET Core Web API front end and a stateful back-end service toostore your data.</span></span> <span data-ttu-id="8d619-106">När du är klar har röstningsapp med en ASP.NET Core webbklientdelen som sparar röstning resultat i en tillståndskänslig backend-tjänst i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8d619-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in hello cluster.</span></span> <span data-ttu-id="8d619-107">Om du inte vill toomanually skapa hello röstning program, kan du [hämta hello källkoden](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello slutfört i programmet och gå vidare för[igenom hello röstning exempelprogrammet](#walkthrough_anchor).</span><span class="sxs-lookup"><span data-stu-id="8d619-107">If you don't want toomanually create hello voting application, you can [download hello source code](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) for hello completed application and skip ahead too[Walk through hello voting sample application](#walkthrough_anchor).</span></span>

![Diagram över](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="8d619-109">I delen i hello serie kan du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="8d619-109">In part one of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d619-110">Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-110">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="8d619-111">Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-111">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="8d619-112">Använda hello omvänd proxy toocommunicate med hello tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="8d619-112">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="8d619-113">I den här självstudiekursen serien lär du dig hur du:</span><span class="sxs-lookup"><span data-stu-id="8d619-113">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="8d619-114">Skapa ett .NET Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="8d619-114">Build a .NET Service Fabric application</span></span>
> * [<span data-ttu-id="8d619-115">Distribuera programmet hello tooa fjärrkluster</span><span class="sxs-lookup"><span data-stu-id="8d619-115">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [<span data-ttu-id="8d619-116">Konfigurera CI/CD: N med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8d619-116">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="8d619-117">Krav</span><span class="sxs-lookup"><span data-stu-id="8d619-117">Prerequisites</span></span>
<span data-ttu-id="8d619-118">Innan du börjar den här kursen:</span><span class="sxs-lookup"><span data-stu-id="8d619-118">Before you begin this tutorial:</span></span>
- <span data-ttu-id="8d619-119">Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="8d619-119">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="8d619-120">[Installera Visual Studio 2017](https://www.visualstudio.com/) och installera hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="8d619-120">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="8d619-121">Installera hello Service Fabric-SDK</span><span class="sxs-lookup"><span data-stu-id="8d619-121">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a><span data-ttu-id="8d619-122">Skapa en ASP.NET Web API-tjänst som en tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-122">Create an ASP.NET Web API service as a reliable service</span></span>
<span data-ttu-id="8d619-123">Först skapa hello web front-end av hello röstning program med hjälp av ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d619-123">First, create hello web front-end of hello voting application using ASP.NET Core.</span></span> <span data-ttu-id="8d619-124">ASP.NET Core är ett lätt, plattformsoberoende utveckling att du kan använda toocreate moderna webbgränssnittet och webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="8d619-124">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> <span data-ttu-id="8d619-125">tooget en klar förståelse för hur ASP.NET Core kan integreras med Service Fabric, rekommenderar vi starkt läsa igenom hello [ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="8d619-125">tooget a complete understanding of how ASP.NET Core integrates with Service Fabric, we strongly recommend reading through hello [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) article.</span></span> <span data-ttu-id="8d619-126">Nu kan du följa den här självstudiekursen tooget igång snabbt.</span><span class="sxs-lookup"><span data-stu-id="8d619-126">For now, you can follow this tutorial tooget started quickly.</span></span> <span data-ttu-id="8d619-127">toolearn mer om ASP.NET Core finns hello [ASP.NET Core-dokumentation](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="8d619-127">toolearn more about ASP.NET Core, see hello [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

> [!NOTE]
> <span data-ttu-id="8d619-128">Den här kursen är baserad på hello [ASP.NET Core tools för Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="8d619-128">This tutorial is based on hello [ASP.NET Core tools for Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span></span> <span data-ttu-id="8d619-129">hello .NET Core verktyg för Visual Studio 2015 uppdateras inte längre.</span><span class="sxs-lookup"><span data-stu-id="8d619-129">hello .NET Core tools for Visual Studio 2015 are no longer being updated.</span></span>

1. <span data-ttu-id="8d619-130">Starta Visual Studio som **administratör**.</span><span class="sxs-lookup"><span data-stu-id="8d619-130">Launch Visual Studio as an **administrator**.</span></span>

2. <span data-ttu-id="8d619-131">Skapa ett projekt med **filen**->**ny**->**projekt**</span><span class="sxs-lookup"><span data-stu-id="8d619-131">Create a project with **File**->**New**->**Project**</span></span>

3. <span data-ttu-id="8d619-132">I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="8d619-132">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

4. <span data-ttu-id="8d619-133">Namnge programmet hello **Röstningsdatabasen** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8d619-133">Name hello application **Voting** and press **OK**.</span></span>

   ![Dialogrutan Nytt projekt i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. <span data-ttu-id="8d619-135">På hello **nya Service Fabric Service** väljer **tillståndslös ASP.NET Core**, och namnge din tjänst **VotingWeb**.</span><span class="sxs-lookup"><span data-stu-id="8d619-135">On hello **New Service Fabric Service** page, choose **Stateless ASP.NET Core**, and name your service **VotingWeb**.</span></span>
   
   ![Välja ASP.NET-webbtjänst i hello nya service dialog](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. <span data-ttu-id="8d619-137">hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar.</span><span class="sxs-lookup"><span data-stu-id="8d619-137">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="8d619-138">Den här kursen väljer **webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="8d619-138">For this tutorial, choose **Web Application**.</span></span> 
   
   ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   <span data-ttu-id="8d619-140">Visual Studio skapar ett program och ett tjänstprojekt och visar dem i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="8d619-140">Visual Studio creates an application and a service project and displays them in Solution Explorer.</span></span>

   ![Solution Explorer efter skapandet av program med ASP.NET core Web API-tjänsten]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a><span data-ttu-id="8d619-142">Lägga till AngularJS toohello VotingWeb tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-142">Add AngularJS toohello VotingWeb service</span></span>
<span data-ttu-id="8d619-143">Lägg till [AngularJS](http://angularjs.org/) tooyour tjänst med hello inbyggda [Bower support](/aspnet/core/client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="8d619-143">Add [AngularJS](http://angularjs.org/) tooyour service using hello built-in [Bower support](/aspnet/core/client-side/bower).</span></span> <span data-ttu-id="8d619-144">Öppna *bower.json* och lägga till poster för vinkel och vinkel bootstrap och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-144">Open *bower.json* and add entries for angular and angular-bootstrap, then save your changes.</span></span>

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
<span data-ttu-id="8d619-145">När du sparar hello *bower.json* filen Angular har installerats i ditt projekt *wwwroot/lib* mapp.</span><span class="sxs-lookup"><span data-stu-id="8d619-145">Upon saving hello *bower.json* file, Angular is installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="8d619-146">Dessutom visas inom hello *beroenden/Bower* mapp.</span><span class="sxs-lookup"><span data-stu-id="8d619-146">Additionally, it is listed within hello *Dependencies/Bower* folder.</span></span>

### <a name="update-hello-sitejs-file"></a><span data-ttu-id="8d619-147">Uppdatera hello site.js-filen</span><span class="sxs-lookup"><span data-stu-id="8d619-147">Update hello site.js file</span></span>
<span data-ttu-id="8d619-148">Öppna hello *wwwroot/js/site.js* fil.</span><span class="sxs-lookup"><span data-stu-id="8d619-148">Open hello *wwwroot/js/site.js* file.</span></span>  <span data-ttu-id="8d619-149">Ersätt innehållet med hello JavaScript som används av hello Home vyer:</span><span class="sxs-lookup"><span data-stu-id="8d619-149">Replace its contents with hello JavaScript used by hello Home views:</span></span>

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-hello-indexcshtml-file"></a><span data-ttu-id="8d619-150">Uppdatera hello Index.cshtml-filen</span><span class="sxs-lookup"><span data-stu-id="8d619-150">Update hello Index.cshtml file</span></span>
<span data-ttu-id="8d619-151">Öppna hello *Views/Home/Index.cshtml* fil, hello visa specifika toohello Home-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="8d619-151">Open hello *Views/Home/Index.cshtml* file, hello view specific toohello Home controller.</span></span>  <span data-ttu-id="8d619-152">Ersätt innehållet med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-152">Replace its contents with hello following, then save your changes.</span></span>

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click toovote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-hello-layoutcshtml-file"></a><span data-ttu-id="8d619-153">Uppdatera hello _Layout.cshtml-filen</span><span class="sxs-lookup"><span data-stu-id="8d619-153">Update hello _Layout.cshtml file</span></span>
<span data-ttu-id="8d619-154">Öppna hello *Views/Shared/_Layout.cshtml* fil, hello standardlayout för hello ASP.NET-app.</span><span class="sxs-lookup"><span data-stu-id="8d619-154">Open hello *Views/Shared/_Layout.cshtml* file, hello default layout for hello ASP.NET app.</span></span>  <span data-ttu-id="8d619-155">Ersätt innehållet med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-155">Replace its contents with hello following, then save your changes.</span></span>

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-hello-votingwebcs-file"></a><span data-ttu-id="8d619-156">Uppdatera hello VotingWeb.cs-filen</span><span class="sxs-lookup"><span data-stu-id="8d619-156">Update hello VotingWeb.cs file</span></span>
<span data-ttu-id="8d619-157">Öppna hello *VotingWeb.cs* filen, vilket skapar hello ASP.NET Core Webbvärden inuti hello tillståndslösa tjänsten hello WebListener webbservern.</span><span class="sxs-lookup"><span data-stu-id="8d619-157">Open hello *VotingWeb.cs* file, which creates hello ASP.NET Core WebHost inside hello stateless service using hello WebListener web server.</span></span>  <span data-ttu-id="8d619-158">Lägg till hello `using System.Net.Http;` direktivet toohello överkant hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8d619-158">Add hello `using System.Net.Http;` directive toohello top of hello file.</span></span>  <span data-ttu-id="8d619-159">Ersätt hello `CreateServiceInstanceListeners()` fungerar med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-159">Replace hello `CreateServiceInstanceListeners()` function with hello following, then save your changes.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-hello-votescontrollercs-file"></a><span data-ttu-id="8d619-160">Lägg till hello VotesController.cs fil</span><span class="sxs-lookup"><span data-stu-id="8d619-160">Add hello VotesController.cs file</span></span>
<span data-ttu-id="8d619-161">Lägg till en domänkontrollant som definierar röstning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8d619-161">Add a controller which defines voting actions.</span></span> <span data-ttu-id="8d619-162">Högerklicka på hello **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klass**.</span><span class="sxs-lookup"><span data-stu-id="8d619-162">Right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span>  <span data-ttu-id="8d619-163">Namnge hello filen ”VotesController.cs” och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8d619-163">Name hello file "VotesController.cs" and click **Add**.</span></span>  <span data-ttu-id="8d619-164">Ersätt hello-filens innehåll med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-164">Replace hello file contents with hello following, then save your changes.</span></span>  <span data-ttu-id="8d619-165">Senare i [uppdateringsfil hello VotesController.cs](#updatevotecontroller_anchor), den här filen tas ändrade tooread och skriva röstning data från hello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d619-165">Later, in [Update hello VotesController.cs file](#updatevotecontroller_anchor), this file will be modified tooread and write voting data from hello back-end service.</span></span>  <span data-ttu-id="8d619-166">För tillfället returnerar hello domänkontrollanten statiska sträng toohello datavy.</span><span class="sxs-lookup"><span data-stu-id="8d619-166">For now, hello controller returns static string data toohello view.</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-hello-application-locally"></a><span data-ttu-id="8d619-167">Distribuera och köra programmet hello lokalt</span><span class="sxs-lookup"><span data-stu-id="8d619-167">Deploy and run hello application locally</span></span>
<span data-ttu-id="8d619-168">Du kan nu gå vidare och köra programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8d619-168">You can now go ahead and run hello application.</span></span> <span data-ttu-id="8d619-169">I Visual Studio trycker du på `F5` toodeploy hello program för felsökning.</span><span class="sxs-lookup"><span data-stu-id="8d619-169">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span> <span data-ttu-id="8d619-170">`F5`misslyckas om du inte tidigare öppna Visual Studio som **administratör**.</span><span class="sxs-lookup"><span data-stu-id="8d619-170">`F5` fails if you didn't previously open Visual Studio as **administrator**.</span></span>

> [!NOTE]
> <span data-ttu-id="8d619-171">hello skapar första gången du kör och distribuera hello programmet lokalt, Visual Studio ett lokala kluster för felsökning.</span><span class="sxs-lookup"><span data-stu-id="8d619-171">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span>  <span data-ttu-id="8d619-172">Skapa ett kluster kan ta en stund.</span><span class="sxs-lookup"><span data-stu-id="8d619-172">Cluster creation may take some time.</span></span> <span data-ttu-id="8d619-173">hello klustret Skapandestatus visas i utdatafönstret för hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d619-173">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="8d619-174">Ditt webbprogram bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="8d619-174">At this point, your web app should look like this:</span></span>

![ASP.NET Core frontend](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

<span data-ttu-id="8d619-176">toostop felsöker hello program går du tillbaka tooVisual Studio och tryck på **SKIFT + F5**.</span><span class="sxs-lookup"><span data-stu-id="8d619-176">toostop debugging hello application, go back tooVisual Studio and press **Shift+F5**.</span></span>

## <a name="add-a-stateful-back-end-service-tooyour-application"></a><span data-ttu-id="8d619-177">Lägga till ett program för tooyour tillståndskänslig backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-177">Add a stateful back-end service tooyour application</span></span>
<span data-ttu-id="8d619-178">Nu när vi har ett ASP.NET Web API-tjänsten som körs i vårt program kan vi gå vidare och Lägg till en tillståndskänslig tillförlitlig tjänst toostore vissa data i vårt program.</span><span class="sxs-lookup"><span data-stu-id="8d619-178">Now that we have an ASP.NET Web API service running in our application, let's go ahead and add a stateful reliable service toostore some data in our application.</span></span>

<span data-ttu-id="8d619-179">Service Fabric kan du tooconsistently och på ett tillförlitligt sätt lagra dina data direkt i din tjänst med hjälp av tillförlitliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="8d619-179">Service Fabric allows you tooconsistently and reliably store your data right inside your service by using reliable collections.</span></span> <span data-ttu-id="8d619-180">Tillförlitliga samlingar är en uppsättning med hög tillgänglighet och tillförlitlig Samlingsklasser som är bekant tooanyone som har använt C#-samlingar.</span><span class="sxs-lookup"><span data-stu-id="8d619-180">Reliable collections are a set of highly available and reliable collection classes that are familiar tooanyone who has used C# collections.</span></span>

<span data-ttu-id="8d619-181">I den här självstudiekursen skapar du en tjänst som lagrar counter-värdet i en tillförlitlig samling.</span><span class="sxs-lookup"><span data-stu-id="8d619-181">In this tutorial, you create a service which stores a counter value in a reliable collection.</span></span>

1. <span data-ttu-id="8d619-182">I Solution Explorer högerklickar du på **Services** inom hello projektet och välj **Lägg till > nya Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="8d619-182">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Lägga till ett nytt tooan befintliga tjänstprogram](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. <span data-ttu-id="8d619-184">I hello **nya Service Fabric Service** dialogrutan Välj **Stateful ASP.NET Core**, och name hello service **VotingData** och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8d619-184">In hello **New Service Fabric Service** dialog, choose **Stateful ASP.NET Core**, and name hello service **VotingData** and press **OK**.</span></span>

    ![Dialogrutan Ny tjänst i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    <span data-ttu-id="8d619-186">När service-projekt har skapats har du två tjänster i ditt program.</span><span class="sxs-lookup"><span data-stu-id="8d619-186">Once your service project is created, you have two services in your application.</span></span> <span data-ttu-id="8d619-187">När du fortsätter toobuild programmet, du kan lägga till fler tjänster i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="8d619-187">As you continue toobuild your application, you can add more services in hello same way.</span></span> <span data-ttu-id="8d619-188">Båda kan vara oberoende versionsnummer och uppgraderade.</span><span class="sxs-lookup"><span data-stu-id="8d619-188">Each can be independently versioned and upgraded.</span></span>

3. <span data-ttu-id="8d619-189">hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar.</span><span class="sxs-lookup"><span data-stu-id="8d619-189">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="8d619-190">Den här kursen väljer **Web API**.</span><span class="sxs-lookup"><span data-stu-id="8d619-190">For this tutorial, choose **Web API**.</span></span>

    ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    <span data-ttu-id="8d619-192">Visual Studio skapar ett service-projekt och visar dem i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="8d619-192">Visual Studio creates a service project and displays them in Solution Explorer.</span></span>

    ![Solution Explorer](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a><span data-ttu-id="8d619-194">Lägg till hello VoteDataController.cs fil</span><span class="sxs-lookup"><span data-stu-id="8d619-194">Add hello VoteDataController.cs file</span></span>

<span data-ttu-id="8d619-195">I hello **VotingData** projektet genom att högerklicka på hello **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klassen**.</span><span class="sxs-lookup"><span data-stu-id="8d619-195">In hello **VotingData** project right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span> <span data-ttu-id="8d619-196">Namnge hello filen ”VoteDataController.cs” och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8d619-196">Name hello file "VoteDataController.cs" and click **Add**.</span></span> <span data-ttu-id="8d619-197">Ersätt hello-filens innehåll med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-197">Replace hello file contents with hello following, then save your changes.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-hello-services"></a><span data-ttu-id="8d619-198">Ansluta hello-tjänster</span><span class="sxs-lookup"><span data-stu-id="8d619-198">Connect hello services</span></span>
<span data-ttu-id="8d619-199">I nästa steg, vi ansluta hello två tjänster och se hello frontwebbservern programmet get och set röstning information från hello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d619-199">In this next step, we will connect hello two services and make hello front-end Web application get and set voting information from hello back-end service.</span></span>

<span data-ttu-id="8d619-200">Service Fabric ger fullständig flexibilitet i hur du kommunicerar med tillförlitlig tjänster.</span><span class="sxs-lookup"><span data-stu-id="8d619-200">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="8d619-201">I ett enskilt program kanske tjänster som är tillgängliga via TCP.</span><span class="sxs-lookup"><span data-stu-id="8d619-201">Within a single application, you might have services that are accessible via TCP.</span></span> <span data-ttu-id="8d619-202">Andra tjänster som kan nås via en HTTP-REST-API och fortfarande andra tjänster kan vara tillgänglig via webben sockets.</span><span class="sxs-lookup"><span data-stu-id="8d619-202">Other services that might be accessible via an HTTP REST API and still other services could be accessible via web sockets.</span></span> <span data-ttu-id="8d619-203">Bakgrund hello alternativen och hello kompromisser ingår, se [kommunicerar med tjänster](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="8d619-203">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

<span data-ttu-id="8d619-204">I den här kursen använder [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="8d619-204">In this tutorial, we are using [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a><span data-ttu-id="8d619-205">Uppdatera hello VotesController.cs-filen</span><span class="sxs-lookup"><span data-stu-id="8d619-205">Update hello VotesController.cs file</span></span>
<span data-ttu-id="8d619-206">I hello **VotingWeb** projekt, öppna hello *Controllers/VotesController.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="8d619-206">In hello **VotingWeb** project, open hello *Controllers/VotesController.cs* file.</span></span>  <span data-ttu-id="8d619-207">Ersätt hello `VotesController` klassen definition innehållet med hello följande och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8d619-207">Replace hello `VotesController` class definition contents with hello following, then save your changes.</span></span>

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-hello-voting-sample-application"></a><span data-ttu-id="8d619-208">Gå igenom hello röstning exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="8d619-208">Walk through hello voting sample application</span></span>
<span data-ttu-id="8d619-209">hello röstning program består av två tjänster:</span><span class="sxs-lookup"><span data-stu-id="8d619-209">hello voting application consists of two services:</span></span>
- <span data-ttu-id="8d619-210">Web frontend-tjänst (VotingWeb) – en ASP.NET Core webbtjänsten frontend, vilket fungerar webbsidan hello och visar webb-API: er toocommunicate med hello backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8d619-210">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves hello web page and exposes web APIs toocommunicate with hello backend service.</span></span>
- <span data-ttu-id="8d619-211">Backend-tjänst (VotingData)-ett ASP.NET Core webbtjänsten, som visar ett API toostore hello rösten resulterar i en tillförlitlig ordlista kvar på disken.</span><span class="sxs-lookup"><span data-stu-id="8d619-211">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API toostore hello vote results in a reliable dictionary persisted on disk.</span></span>

![Diagram över](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="8d619-213">När du rösta i hello programmet hello följande inträffar händelser:</span><span class="sxs-lookup"><span data-stu-id="8d619-213">When you vote in hello application hello following events occur:</span></span>
1. <span data-ttu-id="8d619-214">En JavaScript skickar hello rösten begäran toohello webb-API i hello Frontend-webbtjänsten som ett HTTP PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="8d619-214">A JavaScript sends hello vote request toohello web API in hello web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="8d619-215">hello Frontend-webbtjänsten använder en proxy-toolocate och vidarebefordra en HTTP PUT-begäran toohello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d619-215">hello web front-end service uses a proxy toolocate and forward an HTTP PUT request toohello back-end service.</span></span>

3. <span data-ttu-id="8d619-216">hello backend-tjänst tar hello inkommande begäran och lagrar hello uppdateras resultera i en tillförlitlig ordlista som hämtar replikerade toomultiple noder i klustret hello och kvar på disken.</span><span class="sxs-lookup"><span data-stu-id="8d619-216">hello back-end service takes hello incoming request, and stores hello updated result in a reliable dictionary, which gets replicated toomultiple nodes within hello cluster and persisted on disk.</span></span> <span data-ttu-id="8d619-217">Alla hello programdata lagras i hello klustret, så det behövs ingen databas.</span><span class="sxs-lookup"><span data-stu-id="8d619-217">All hello application's data is stored in hello cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="8d619-218">Felsökning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d619-218">Debug in Visual Studio</span></span>
<span data-ttu-id="8d619-219">När du felsöker programmet i Visual Studio använder du en lokal utveckling Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="8d619-219">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="8d619-220">Du har hello alternativet tooadjust felsökning upplevelse tooyour scenariot.</span><span class="sxs-lookup"><span data-stu-id="8d619-220">You have hello option tooadjust your debugging experience tooyour scenario.</span></span> <span data-ttu-id="8d619-221">I det här programmet lagrar vi data i vår backend-tjänst med hjälp av en tillförlitlig ordlista.</span><span class="sxs-lookup"><span data-stu-id="8d619-221">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="8d619-222">Visual Studio tar bort hello program per standard när du stoppar hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="8d619-222">Visual Studio removes hello application per default when you stop hello debugger.</span></span> <span data-ttu-id="8d619-223">Ta bort programmet hello medför hello data i hello backend-tjänst tooalso tas bort.</span><span class="sxs-lookup"><span data-stu-id="8d619-223">Removing hello application causes hello data in hello back-end service tooalso be removed.</span></span> <span data-ttu-id="8d619-224">toopersist hello data mellan felsökning sessioner, kan du ändra hello **programmet felsökningsläge** som en egenskap på hello **Röstningsdatabasen** projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d619-224">toopersist hello data between debugging sessions, you can change hello **Application Debug Mode** as a property on hello **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="8d619-225">toolook på vad som händer i hello kod, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8d619-225">toolook at what happens in hello code, complete hello following steps:</span></span>
1. <span data-ttu-id="8d619-226">Öppna hello **VotesController.cs** fil och anger en brytpunkt i hello webb-API: er **placera** metod (rad 47) – du kan söka efter hello-filen i hello Solution Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d619-226">Open hello **VotesController.cs** file and set a breakpoint in hello web API's **Put** method (line 47) - You can search for hello file in hello Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="8d619-227">Öppna hello **VoteDataController.cs** fil och anger en brytpunkt i den här web API **placera** metod (rad 50).</span><span class="sxs-lookup"><span data-stu-id="8d619-227">Open hello **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="8d619-228">Gå tillbaka toohello webbläsare och klicka på ett alternativ för röstning eller lägga till ett nytt alternativ för röstning.</span><span class="sxs-lookup"><span data-stu-id="8d619-228">Go back toohello browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="8d619-229">Du kan träffa hello första brytpunkt i hello web front slutpunkts api-kontrollanten.</span><span class="sxs-lookup"><span data-stu-id="8d619-229">You hit hello first breakpoint in hello web front-end's api controller.</span></span>
    
    1. <span data-ttu-id="8d619-230">Detta är där hello JavaScript i hello webbläsaren skickar en begäran toohello web API-kontrollanten i hello frontend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d619-230">This is where hello JavaScript in hello browser sends a request toohello web API controller in hello front-end service.</span></span>
    
    ![Lägg till röst frontend-tjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. <span data-ttu-id="8d619-232">Först skapar vi hello URL toohello ReverseProxy för vår backend-tjänst **(1)**.</span><span class="sxs-lookup"><span data-stu-id="8d619-232">First we construct hello URL toohello ReverseProxy for our back-end service **(1)**.</span></span>
    3. <span data-ttu-id="8d619-233">Vi skickar hello HTTP PUT-begäran om toohello ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="8d619-233">Then we send hello HTTP PUT Request toohello ReverseProxy **(2)**.</span></span>
    4. <span data-ttu-id="8d619-234">Slutligen hello returnerar vi hello svar från hello backend-tjänst toohello klient **(3)**.</span><span class="sxs-lookup"><span data-stu-id="8d619-234">Finally hello we return hello response from hello back-end service toohello client **(3)**.</span></span>

4. <span data-ttu-id="8d619-235">Tryck på **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="8d619-235">Press **F5** toocontinue</span></span>
    1. <span data-ttu-id="8d619-236">Du är nu vid hello break i hello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d619-236">You are now at hello break point in hello back-end service.</span></span>
    
    ![Lägga till röst backend-tjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. <span data-ttu-id="8d619-238">I hello första raden i hello metoden **(1)** vi använder hello `StateManager` tooget eller Lägg till en tillförlitlig ordlista som heter `counts`.</span><span class="sxs-lookup"><span data-stu-id="8d619-238">In hello first line in hello method **(1)** we are using hello `StateManager` tooget or add a reliable dictionary called `counts`.</span></span>
    3. <span data-ttu-id="8d619-239">All interaktion med värden i en tillförlitlig ordlista kräver en transaktion detta med hjälp av instruktionen **(2)** skapar den aktuella transaktionen.</span><span class="sxs-lookup"><span data-stu-id="8d619-239">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    4. <span data-ttu-id="8d619-240">I hello transaktion vi sedan uppdatera hello värdet för hello relevanta nyckeln för hello röstning alternativet och incheckningar hello åtgärden **(3)**.</span><span class="sxs-lookup"><span data-stu-id="8d619-240">In hello transaction, we then update hello value of hello relevant key for hello voting option and commits hello operation **(3)**.</span></span> <span data-ttu-id="8d619-241">När hello checkat returnerar metoden hello data uppdateras i hello ordlista och replikeras tooother noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="8d619-241">Once hello commit method returns, hello data is updated in hello dictionary and replicated tooother nodes in hello cluster.</span></span> <span data-ttu-id="8d619-242">hello data lagras nu på ett säkert sätt i hello kluster och hello backend-tjänst kan växlas tooother noder och fortfarande har hello data är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="8d619-242">hello data is now safely stored in hello cluster, and hello back-end service can fail over tooother nodes, still having hello data available.</span></span>
5. <span data-ttu-id="8d619-243">Tryck på **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="8d619-243">Press **F5** toocontinue</span></span>

<span data-ttu-id="8d619-244">toostop hello felsökningssessionen, tryck på **SKIFT + F5**.</span><span class="sxs-lookup"><span data-stu-id="8d619-244">toostop hello debugging session, press **Shift+F5**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8d619-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d619-245">Next steps</span></span>
<span data-ttu-id="8d619-246">I denna del av hello kursen får du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="8d619-246">In this part of hello tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d619-247">Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-247">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="8d619-248">Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst</span><span class="sxs-lookup"><span data-stu-id="8d619-248">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="8d619-249">Använda hello omvänd proxy toocommunicate med hello tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="8d619-249">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="8d619-250">Avancerade toohello nästa kurs:</span><span class="sxs-lookup"><span data-stu-id="8d619-250">Advance toohello next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8d619-251">Distribuera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="8d619-251">Deploy hello application tooAzure</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
