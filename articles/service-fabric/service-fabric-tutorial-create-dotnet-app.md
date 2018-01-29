---
title: "Skapa ett .NET-program för Service Fabric | Microsoft Docs"
description: "Lär dig hur du skapar ett program med en ASP.NET Core frontend och en tillförlitlig tjänst tillståndskänslig backend- och distribuera programmet till ett kluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/17/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f4b3c766ee46233cd4ec2d195e39d0b68516952f
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Skapa och distribuera ett program med en webb-API för ASP.NET Core frontend-tjänst och en tillståndskänslig backend-tjänst
Den här kursen ingår i en serie.  Du får lära dig att skapa ett Azure Service Fabric-program med en ASP.NET Core Web API-klient och en tillståndskänslig backend-tjänst för att lagra data. När du är klar har du ett röstningsprogam med ASP.NET Core-webbklient som sparar röstningsresultat i en tillståndskänslig backend-tjänst i klustret. Om du inte vill skapa röstning programmet manuellt, kan du [ladda ned källkoden](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) för det färdiga programmet och gå vidare till [igenom röstning exempelprogrammet](#walkthrough_anchor).

![Diagram över programmet](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

I delen en av serierna kan du lära dig hur du:

> [!div class="checklist"]
> * Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst
> * Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst
> * Använd omvänd proxy för att kommunicera med tjänsten tillståndskänslig

I den här självstudiekursen serien lär du dig hur du:
> [!div class="checklist"]
> * Skapa ett .NET Service Fabric-program
> * [Distribuera programmet till ett kluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurera CI/CD: N med hjälp av Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Konfigurera övervakning och diagnostik för programmet](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Förutsättningar
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installera Visual Studio 2017](https://www.visualstudio.com/) 15.3 eller senare med den **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.
- [Installera Service Fabric SDK](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Skapa en ASP.NET Web API-tjänst som en tillförlitlig tjänst
Först skapa webben frontend röstning programkonfigurationen med ASP.NET Core. ASP.NET Core är ett lätt, plattformsoberoende utveckling som du kan använda för att skapa moderna webbgränssnittet och webb-API: er. För att få en förståelse av hur ASP.NET Core kan integreras med Service Fabric, rekommenderar vi starkt läsa igenom den [ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) artikel. Nu kan du följa den här kursen och kom igång snabbt. Läs mer om ASP.NET Core i den [ASP.NET Core-dokumentation](https://docs.microsoft.com/aspnet/core/).

1. Starta Visual Studio som **administratör**.

2. Skapa ett projekt med **filen**->**ny**->**projekt**

3. Klicka på **Moln > Service Fabric-program** i dialogrutan **Nytt projekt**.

4. Ge programmet namnet **Röstningsdatabasen** och tryck på **OK**.

   ![Dialogrutan Nytt projekt i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. På den **nya Service Fabric Service** väljer **tillståndslös ASP.NET Core**, och namnge din tjänst **VotingWeb**.
   
   ![Välja ASP.NET-webbtjänst i dialogrutan Ny tjänst](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Nästa sida innehåller en uppsättning av ASP.NET Core projektmallar. Den här kursen väljer **webbprogram (Model-View-Controller)**. 
   
   ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio skapar ett program och ett tjänstprojekt och visar dem i Solution Explorer.

   ![Solution Explorer efter skapandet av program med ASP.NET core Web API-tjänsten]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>Lägg till AngularJS i VotingWeb-tjänst
Lägg till [AngularJS](http://angularjs.org/) till tjänsten med hjälp av [Bower support](/aspnet/core/client-side/bower). Lägg först till en konfigurationsfil för Bower i projektet.  I Solution Explorer högerklickar du på **VotingWeb** och välj **Lägg till nytt objekt ->**. Välj **Web** och sedan **Bower konfigurationsfilen**.  Den *bower.json* filen har skapats.

Öppna *bower.json* och lägga till poster för vinkel och vinkel bootstrap och sedan spara ändringarna.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "3.2.1",
    "jquery-validation": "1.16.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.8",
    "angular-bootstrap": "v1.1.0"
  }
}
```
När det sparas den *bower.json* filen Angular har installerats i ditt projekt *wwwroot/lib* mapp. Dessutom visas inom den *beroenden/Bower* mapp.

### <a name="update-the-sitejs-file"></a>Uppdatera site.js-filen
Öppna den *wwwroot/js/site.js* fil.  Ersätt innehållet med JavaScript som används av hem-vyer:

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

### <a name="update-the-indexcshtml-file"></a>Uppdatera Index.cshtml-filen
Öppna den *Views/Home/Index.cshtml* -fil, vyn specifika för Home-styrenhet.  Ersätt innehållet med följande och sedan spara ändringarna.

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
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item"/>
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr/>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
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

### <a name="update-the-layoutcshtml-file"></a>Uppdatera _Layout.cshtml-filen
Öppna den *Views/Shared/_Layout.cshtml* -fil, standardlayout för ASP.NET-app.  Ersätt innehållet med följande och sedan spara ändringarna.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet"/>
    <link href="~/css/site.css" rel="stylesheet"/>

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

### <a name="update-the-votingwebcs-file"></a>Uppdatera VotingWeb.cs-filen
Öppna den *VotingWeb.cs* filen, vilket skapar ASP.NET Core Webbvärden inuti tillståndslösa tjänsten WebListener webbservern.  

Lägg till den `using System.Net.Http;` direktivet överst i filen.  

Ersätt den `CreateServiceInstanceListeners()` fungerar med följande och sedan spara ändringarna.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(
            serviceContext =>
                new KestrelCommunicationListener(
                    serviceContext,
                    "ServiceEndpoint",
                    (url, listener) =>
                    {
                        ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

                        return new WebHostBuilder()
                            .UseKestrel()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<HttpClient>(new HttpClient())
                                    .AddSingleton<FabricClient>(new FabricClient())
                                    .AddSingleton<StatelessServiceContext>(serviceContext))
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
                    }))
    };
}
```

Också lägga till den `GetVotingDataServiceName` metod som returnerar namnet på tjänsten när avsökas:

```csharp
internal static Uri GetVotingDataServiceName(ServiceContext context)
{
    return new Uri($"{context.CodePackageActivationContext.ApplicationName}/VotingData");
}
```

### <a name="add-the-votescontrollercs-file"></a>Lägga till filen VotesController.cs
Lägg till en domänkontrollant som definierar röstning åtgärder. Högerklickar du på den **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klassen**.  Namn på filen ”VotesController.cs” och klicka på **Lägg till**.  

Ersätt filens innehåll med följande och sedan spara ändringarna.  Senare i [uppdateringsfil VotesController.cs](#updatevotecontroller_anchor), den här filen ändras för att läsa och skriva röstning data från backend-tjänst.  För tillfället returnerar styrenheten statiska strängdata till vyn.

```csharp
namespace VotingWeb.Controllers
{
    using System;
    using System.Collections.Generic;
    using System.Fabric;
    using System.Fabric.Query;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Newtonsoft.Json;

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

### <a name="configure-the-listening-port"></a>Konfigurera lyssningsporten
När VotingWeb frontend-tjänst skapas väljer slumpmässigt en port för tjänsten för att lyssna på i Visual Studio.  Tjänsten VotingWeb fungerar som klientdel för det här programmet och godkänner externa trafiken, så vi binda tjänsten till ett fast och vet port korrekt.  Den [tjänstmanifestet](service-fabric-application-and-service-manifests.md) deklarerar Tjänsteslutpunkter. I Solution Explorer öppnar *VotingWeb/PackageRoot/ServiceManifest.xml*.  Hitta de **Endpoint** resurs i den **resurser** avsnittet och ändra den **Port** värde till mellan 80, eller till en annan port. För att distribuera och köra programmet lokalt, måste programmet lyssningsporten vara öppen och tillgänglig på datorn.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Uppdatera också egenskapsvärdet programmets URL i röst-projektet så att en webbläsare öppnas till rätt port när du felsöker med 'F5'.  I Solution Explorer, väljer du den **Röstningsdatabasen** projekt och uppdatera den **programmets URL** egenskapen.

![Programmets URL](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

### <a name="deploy-and-run-the-application-locally"></a>Distribuera och köra programmet lokalt
Du kan nu gå vidare och köra programmet. Tryck på `F5` i Visual Studio för att distribuera programmet för felsökning. `F5`misslyckas om du inte tidigare öppna Visual Studio som **administratör**.

> [!NOTE]
> Första gången du kör och distribuerar programmet lokalt, skapar Visual Studio ett lokalt kluster för felsökning.  Skapa ett kluster kan ta en stund. Statusen för klustergenereringen visas i utdatafönstret i Visual Studio.

Ditt webbprogram bör nu se ut så här:

![ASP.NET Core frontend](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Om du vill stoppa felsökningen av programmet, gå tillbaka till Visual Studio och tryck på **SKIFT + F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Lägg till en tillståndskänslig backend-tjänst i ditt program
Nu när ett ASP.NET Web API-tjänsten körs i programmet, gå vidare och lägga till en tillståndskänslig tillförlitlig tjänst för att lagra vissa data i programmet.

Service Fabric kan du konsekvent och tillförlitligt sätt lagra dina data direkt i din tjänst med hjälp av tillförlitliga samlingar. Tillförlitliga samlingar är en uppsättning med hög tillgänglighet och tillförlitlig Samlingsklasser som känner till alla som har använt C#-samlingar.

I den här självstudiekursen skapar du en tjänst som lagrar counter-värdet i en tillförlitlig samling.

1. I Solution Explorer högerklickar du på **Services** i programmet projektet och välj **Lägg till > nya Service Fabric Service**.
    
2. I den **nya Service Fabric Service** dialogrutan Välj **Stateful ASP.NET Core**, och ger den namnet tjänsten **VotingData** och tryck på **OK**.

    ![Dialogrutan Ny tjänst i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    När service-projekt har skapats har du två tjänster i ditt program. Du kan lägga till fler tjänster på samma sätt som du fortsätta att skapa ditt program. Båda kan vara oberoende versionsnummer och uppgraderade.

3. Nästa sida innehåller en uppsättning av ASP.NET Core projektmallar. Den här kursen väljer **Web API**.

    ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio skapar ett service-projekt och visar dem i Solution Explorer.

    ![Solution Explorer](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-webapi-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>Lägga till filen VoteDataController.cs

I den **VotingData** projektet genom att högerklicka på den **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klassen**. Namn på filen ”VoteDataController.cs” och klicka på **Lägg till**. Ersätt filens innehåll med följande och sedan spara ändringarna.

```csharp
// ------------------------------------------------------------
//  Copyright (c) Microsoft Corporation.  All rights reserved.
//  Licensed under the MIT License (MIT). See License.txt in the repo root for license information.
// ------------------------------------------------------------

namespace VotingData.Controllers
{
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.ServiceFabric.Data;
    using Microsoft.ServiceFabric.Data.Collections;

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
            CancellationToken ct = new CancellationToken();

            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                IAsyncEnumerable<KeyValuePair<string, int>> list = await votesDictionary.CreateEnumerableAsync(tx);

                IAsyncEnumerator<KeyValuePair<string, int>> enumerator = list.GetAsyncEnumerator();

                List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return this.Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

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
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

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


## <a name="connect-the-services"></a>Ansluta tjänsterna
I nästa steg, ansluta de två tjänsterna och gör frontwebbservern programmet get och set röstning information från backend-tjänsten.

Service Fabric ger fullständig flexibilitet i hur du kommunicerar med tillförlitlig tjänster. I ett enskilt program kanske tjänster som är tillgängliga via TCP. Andra tjänster som kan nås via en HTTP-REST-API och fortfarande andra tjänster kan vara tillgänglig via webben sockets. Bakgrund alternativen och kompromisser ingår, se [kommunicerar med tjänster](service-fabric-connect-and-communicate-with-services.md).

I den här kursen använder [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>Uppdatera VotesController.cs-filen
I den **VotingWeb** projektet öppnar den *Controllers/VotesController.cs* fil.  Ersätt den `VotesController` klassen definition innehållet med följande och sedan spara ändringarna.

```csharp
public class VotesController : Controller
{
    private readonly HttpClient httpClient;
    private readonly FabricClient fabricClient;
    private readonly StatelessServiceContext serviceContext;

    public VotesController(HttpClient httpClient, StatelessServiceContext context, FabricClient fabricClient)
    {
        this.fabricClient = fabricClient;
        this.httpClient = httpClient;
        this.serviceContext = context;
    }

    // GET: api/Votes
    [HttpGet("")]
    public async Task<IActionResult> Get()
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);

        ServicePartitionList partitions = await this.fabricClient.QueryManager.GetPartitionListAsync(serviceName);

        List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

        foreach (Partition partition in partitions)
        {
            string proxyUrl =
                $"{proxyAddress}/api/VoteData?PartitionKey={((Int64RangePartitionInformation) partition.PartitionInformation).LowKey}&PartitionKind=Int64Range";

            using (HttpResponseMessage response = await this.httpClient.GetAsync(proxyUrl))
            {
                if (response.StatusCode != System.Net.HttpStatusCode.OK)
                {
                    continue;
                }

                result.AddRange(JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync()));
            }
        }

        return this.Json(result);
    }

    // PUT: api/Votes/name
    [HttpPut("{name}")]
    public async Task<IActionResult> Put(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        StringContent putContent = new StringContent($"{{ 'name' : '{name}' }}", Encoding.UTF8, "application/json");
        putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

        using (HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent))
        {
            return new ContentResult()
            {
                StatusCode = (int) response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }
    }

    // DELETE: api/Votes/name
    [HttpDelete("{name}")]
    public async Task<IActionResult> Delete(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        using (HttpResponseMessage response = await this.httpClient.DeleteAsync(proxyUrl))
        {
            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int) response.StatusCode);
            }
        }

        return new OkResult();
    }


    /// <summary>
    /// Constructs a reverse proxy URL for a given service.
    /// Example: http://localhost:19081/VotingApplication/VotingData/
    /// </summary>
    /// <param name="serviceName"></param>
    /// <returns></returns>
    private Uri GetProxyAddress(Uri serviceName)
    {
        return new Uri($"http://localhost:19081{serviceName.AbsolutePath}");
    }

    /// <summary>
    /// Creates a partition key from the given name.
    /// Uses the zero-based numeric position in the alphabet of the first letter of the name (0-25).
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    private long GetPartitionKey(string name)
    {
        return Char.ToUpper(name.First()) - 'A';
    }
}
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Gå igenom exempelprogrammet för röstning
Röstningsprogrammet består av två tjänster:
- Webbklienttjänst (VotingWeb) – En webbklienttjänst för ASP.NET Core som används av webbsidan och visar webb-API:er för att kommunicera med serverdelstjänsten.
- Serverdelstjänst (VotingData) – En webbtjänst för ASP.NET Core som visar en API för att lagra röstningsresultat i en tillförlitlig ordlista på disken.

![Diagram över programmet](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

När du röstar i programmet händer följande:
1. Ett JavaScript skickar röstningsbegäran till webb-API i webbklienttjänsten som en HTTP PUT-begäran.

2. Webbklienttjänsten använder en proxy för att hitta och vidarebefordra en HTTP PUT-begäran till serverdelstjänsten.

3. Serverdelstjänsten tar den inkommande begäran och lagrar det uppdaterade resultatet i en tillförlitlig ordlista, som replikeras till flera noder i klustret och sparas på disken. Alla programdata lagras i klustret, så det behövs ingen databas.

## <a name="debug-in-visual-studio"></a>Felsökning i Visual Studio
När du felsöker programmet i Visual Studio använder du ett lokalt utvecklingskluster för Service Fabric. Du kan välja att anpassa felsökningen så att det passar ditt scenario. Lagra data i backend-tjänsten med hjälp av en tillförlitlig ordlista i det här programmet. Visual Studio tar som standard bort programmet när du stoppar felsökningsprogrammet. När programmet tas bort kommer även data i serverdelstjänsten att tas bort. Om du vill spara data mellan felsökningssessionerna kan du ändra **programmets felsökningsläge** som en egenskap i projektet **Voting** i Visual Studio.

Gör så här om du vill se vad som händer i koden:
1. Öppna den **VotesController.cs** fil och anger en brytpunkt i webb-API: er **placera** metod (rad 63) – du kan söka efter filen i Solution Explorer i Visual Studio.

2. Öppna den **VoteDataController.cs** fil och anger en brytpunkt i den här web API **placera** metod (rad 53).

3. Gå tillbaka till webbläsaren och klicka på ett röstningsalternativ eller lägg till ett nytt röstningsalternativ. Du kommer till den första brytpunkten i webbklientens api-kontroll.
    
    1. Här skickar JavaScript i webbläsaren en begäran till webb-API-kontrollen i frontwebbtjänsten.
    
    ![Lägg till röst för frontwebbtjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Först skapar Webbadressen till ReverseProxy för backend-tjänst **(1)**.
    3. Skicka den HTTP PUT-begäran till ReverseProxy **(2)**.
    4. Slutligen returnera svar från backend-tjänsten på klienten **(3)**.

4. Tryck på **F5** för att fortsätta
    1. Du befinner dig nu på brytpunkten i serverdelstjänsten.
    
    ![Lägg till röst för serverdelstjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. I den första raden i metoden **(1)** använder den `StateManager` att hämta eller lägga till en tillförlitlig ordlista som heter `counts`.
    3. All interaktion med värden i en tillförlitlig ordlista kräver en transaktion, den här använder instruktionen **(2)** som skapar den transaktionen.
    4. Uppdatera värdet för den aktuella nyckeln för alternativet röstning i transaktionen, och genomför åtgärden **(3)**. När utförandemetoden returneras uppdateras data i ordlistan och replikeras till andra noder i klustret. Data har nu lagrats i klustret och serverdelstjänsten kan redundansväxla till andra noder och fortfarande ha data tillgängliga.
5. Tryck på **F5** för att fortsätta

Stoppa felsökningssessionen genom att trycka på **Skift + F5**.


## <a name="next-steps"></a>Nästa steg
I denna del av kursen får du lära dig hur du:

> [!div class="checklist"]
> * Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst
> * Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst
> * Använd omvänd proxy för att kommunicera med tjänsten tillståndskänslig

Gå vidare till nästa kurs:
> [!div class="nextstepaction"]
> [Distribuera programmet till Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)
