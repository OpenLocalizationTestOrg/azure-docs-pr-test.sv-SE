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
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Skapa och distribuera ett program med en webb-API för ASP.NET Core frontend-tjänst och en tillståndskänslig backend-tjänst
Den här kursen ingår i en serie.  Du kommer lära dig hur toocreate ett Azure Service Fabric-program med en webb-API för ASP.NET Core framför slut och en tillståndskänslig backend-tjänst toostore dina data. När du är klar har röstningsapp med en ASP.NET Core webbklientdelen som sparar röstning resultat i en tillståndskänslig backend-tjänst i hello klustret. Om du inte vill toomanually skapa hello röstning program, kan du [hämta hello källkoden](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello slutfört i programmet och gå vidare för[igenom hello röstning exempelprogrammet](#walkthrough_anchor).

![Diagram över](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

I delen i hello serie kan du lära dig hur du:

> [!div class="checklist"]
> * Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst
> * Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst
> * Använda hello omvänd proxy toocommunicate med hello tillståndskänslig service

I den här självstudiekursen serien lär du dig hur du:
> [!div class="checklist"]
> * Skapa ett .NET Service Fabric-program
> * [Distribuera programmet hello tooa fjärrkluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurera CI/CD: N med hjälp av Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installera Visual Studio 2017](https://www.visualstudio.com/) och installera hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.
- [Installera hello Service Fabric-SDK](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Skapa en ASP.NET Web API-tjänst som en tillförlitlig tjänst
Först skapa hello web front-end av hello röstning program med hjälp av ASP.NET Core. ASP.NET Core är ett lätt, plattformsoberoende utveckling att du kan använda toocreate moderna webbgränssnittet och webb-API: er. tooget en klar förståelse för hur ASP.NET Core kan integreras med Service Fabric, rekommenderar vi starkt läsa igenom hello [ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) artikel. Nu kan du följa den här självstudiekursen tooget igång snabbt. toolearn mer om ASP.NET Core finns hello [ASP.NET Core-dokumentation](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Den här kursen är baserad på hello [ASP.NET Core tools för Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). hello .NET Core verktyg för Visual Studio 2015 uppdateras inte längre.

1. Starta Visual Studio som **administratör**.

2. Skapa ett projekt med **filen**->**ny**->**projekt**

3. I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.

4. Namnge programmet hello **Röstningsdatabasen** och tryck på **OK**.

   ![Dialogrutan Nytt projekt i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. På hello **nya Service Fabric Service** väljer **tillståndslös ASP.NET Core**, och namnge din tjänst **VotingWeb**.
   
   ![Välja ASP.NET-webbtjänst i hello nya service dialog](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar. Den här kursen väljer **webbprogram**. 
   
   ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio skapar ett program och ett tjänstprojekt och visar dem i Solution Explorer.

   ![Solution Explorer efter skapandet av program med ASP.NET core Web API-tjänsten]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a>Lägga till AngularJS toohello VotingWeb tjänst
Lägg till [AngularJS](http://angularjs.org/) tooyour tjänst med hello inbyggda [Bower support](/aspnet/core/client-side/bower). Öppna *bower.json* och lägga till poster för vinkel och vinkel bootstrap och sedan spara ändringarna.

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
När du sparar hello *bower.json* filen Angular har installerats i ditt projekt *wwwroot/lib* mapp. Dessutom visas inom hello *beroenden/Bower* mapp.

### <a name="update-hello-sitejs-file"></a>Uppdatera hello site.js-filen
Öppna hello *wwwroot/js/site.js* fil.  Ersätt innehållet med hello JavaScript som används av hello Home vyer:

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

### <a name="update-hello-indexcshtml-file"></a>Uppdatera hello Index.cshtml-filen
Öppna hello *Views/Home/Index.cshtml* fil, hello visa specifika toohello Home-styrenhet.  Ersätt innehållet med hello följande och sedan spara ändringarna.

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

### <a name="update-hello-layoutcshtml-file"></a>Uppdatera hello _Layout.cshtml-filen
Öppna hello *Views/Shared/_Layout.cshtml* fil, hello standardlayout för hello ASP.NET-app.  Ersätt innehållet med hello följande och sedan spara ändringarna.

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

### <a name="update-hello-votingwebcs-file"></a>Uppdatera hello VotingWeb.cs-filen
Öppna hello *VotingWeb.cs* filen, vilket skapar hello ASP.NET Core Webbvärden inuti hello tillståndslösa tjänsten hello WebListener webbservern.  Lägg till hello `using System.Net.Http;` direktivet toohello överkant hello-filen.  Ersätt hello `CreateServiceInstanceListeners()` fungerar med hello följande och sedan spara ändringarna.

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

### <a name="add-hello-votescontrollercs-file"></a>Lägg till hello VotesController.cs fil
Lägg till en domänkontrollant som definierar röstning åtgärder. Högerklicka på hello **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klass**.  Namnge hello filen ”VotesController.cs” och klicka på **Lägg till**.  Ersätt hello-filens innehåll med hello följande och sedan spara ändringarna.  Senare i [uppdateringsfil hello VotesController.cs](#updatevotecontroller_anchor), den här filen tas ändrade tooread och skriva röstning data från hello backend-tjänst.  För tillfället returnerar hello domänkontrollanten statiska sträng toohello datavy.

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



### <a name="deploy-and-run-hello-application-locally"></a>Distribuera och köra programmet hello lokalt
Du kan nu gå vidare och köra programmet hello. I Visual Studio trycker du på `F5` toodeploy hello program för felsökning. `F5`misslyckas om du inte tidigare öppna Visual Studio som **administratör**.

> [!NOTE]
> hello skapar första gången du kör och distribuera hello programmet lokalt, Visual Studio ett lokala kluster för felsökning.  Skapa ett kluster kan ta en stund. hello klustret Skapandestatus visas i utdatafönstret för hello Visual Studio.

Ditt webbprogram bör nu se ut så här:

![ASP.NET Core frontend](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

toostop felsöker hello program går du tillbaka tooVisual Studio och tryck på **SKIFT + F5**.

## <a name="add-a-stateful-back-end-service-tooyour-application"></a>Lägga till ett program för tooyour tillståndskänslig backend-tjänst
Nu när vi har ett ASP.NET Web API-tjänsten som körs i vårt program kan vi gå vidare och Lägg till en tillståndskänslig tillförlitlig tjänst toostore vissa data i vårt program.

Service Fabric kan du tooconsistently och på ett tillförlitligt sätt lagra dina data direkt i din tjänst med hjälp av tillförlitliga samlingar. Tillförlitliga samlingar är en uppsättning med hög tillgänglighet och tillförlitlig Samlingsklasser som är bekant tooanyone som har använt C#-samlingar.

I den här självstudiekursen skapar du en tjänst som lagrar counter-värdet i en tillförlitlig samling.

1. I Solution Explorer högerklickar du på **Services** inom hello projektet och välj **Lägg till > nya Service Fabric Service**.
   
    ![Lägga till ett nytt tooan befintliga tjänstprogram](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. I hello **nya Service Fabric Service** dialogrutan Välj **Stateful ASP.NET Core**, och name hello service **VotingData** och tryck på **OK**.

    ![Dialogrutan Ny tjänst i Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    När service-projekt har skapats har du två tjänster i ditt program. När du fortsätter toobuild programmet, du kan lägga till fler tjänster i hello samma sätt. Båda kan vara oberoende versionsnummer och uppgraderade.

3. hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar. Den här kursen väljer **Web API**.

    ![Välj typ av ASP.NET-projekt](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio skapar ett service-projekt och visar dem i Solution Explorer.

    ![Solution Explorer](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a>Lägg till hello VoteDataController.cs fil

I hello **VotingData** projektet genom att högerklicka på hello **domänkontrollanter** mapp, välj sedan **Lägg till -> Ny-objekt > klassen**. Namnge hello filen ”VoteDataController.cs” och klicka på **Lägg till**. Ersätt hello-filens innehåll med hello följande och sedan spara ändringarna.

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


## <a name="connect-hello-services"></a>Ansluta hello-tjänster
I nästa steg, vi ansluta hello två tjänster och se hello frontwebbservern programmet get och set röstning information från hello backend-tjänst.

Service Fabric ger fullständig flexibilitet i hur du kommunicerar med tillförlitlig tjänster. I ett enskilt program kanske tjänster som är tillgängliga via TCP. Andra tjänster som kan nås via en HTTP-REST-API och fortfarande andra tjänster kan vara tillgänglig via webben sockets. Bakgrund hello alternativen och hello kompromisser ingår, se [kommunicerar med tjänster](service-fabric-connect-and-communicate-with-services.md).

I den här kursen använder [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a>Uppdatera hello VotesController.cs-filen
I hello **VotingWeb** projekt, öppna hello *Controllers/VotesController.cs* fil.  Ersätt hello `VotesController` klassen definition innehållet med hello följande och sedan spara ändringarna.

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

## <a name="walk-through-hello-voting-sample-application"></a>Gå igenom hello röstning exempelprogrammet
hello röstning program består av två tjänster:
- Web frontend-tjänst (VotingWeb) – en ASP.NET Core webbtjänsten frontend, vilket fungerar webbsidan hello och visar webb-API: er toocommunicate med hello backend-tjänsten.
- Backend-tjänst (VotingData)-ett ASP.NET Core webbtjänsten, som visar ett API toostore hello rösten resulterar i en tillförlitlig ordlista kvar på disken.

![Diagram över](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

När du rösta i hello programmet hello följande inträffar händelser:
1. En JavaScript skickar hello rösten begäran toohello webb-API i hello Frontend-webbtjänsten som ett HTTP PUT-begäran.

2. hello Frontend-webbtjänsten använder en proxy-toolocate och vidarebefordra en HTTP PUT-begäran toohello backend-tjänst.

3. hello backend-tjänst tar hello inkommande begäran och lagrar hello uppdateras resultera i en tillförlitlig ordlista som hämtar replikerade toomultiple noder i klustret hello och kvar på disken. Alla hello programdata lagras i hello klustret, så det behövs ingen databas.

## <a name="debug-in-visual-studio"></a>Felsökning i Visual Studio
När du felsöker programmet i Visual Studio använder du en lokal utveckling Service Fabric-klustret. Du har hello alternativet tooadjust felsökning upplevelse tooyour scenariot. I det här programmet lagrar vi data i vår backend-tjänst med hjälp av en tillförlitlig ordlista. Visual Studio tar bort hello program per standard när du stoppar hello felsökare. Ta bort programmet hello medför hello data i hello backend-tjänst tooalso tas bort. toopersist hello data mellan felsökning sessioner, kan du ändra hello **programmet felsökningsläge** som en egenskap på hello **Röstningsdatabasen** projekt i Visual Studio.

toolook på vad som händer i hello kod, fullständig hello följande steg:
1. Öppna hello **VotesController.cs** fil och anger en brytpunkt i hello webb-API: er **placera** metod (rad 47) – du kan söka efter hello-filen i hello Solution Explorer i Visual Studio.

2. Öppna hello **VoteDataController.cs** fil och anger en brytpunkt i den här web API **placera** metod (rad 50).

3. Gå tillbaka toohello webbläsare och klicka på ett alternativ för röstning eller lägga till ett nytt alternativ för röstning. Du kan träffa hello första brytpunkt i hello web front slutpunkts api-kontrollanten.
    
    1. Detta är där hello JavaScript i hello webbläsaren skickar en begäran toohello web API-kontrollanten i hello frontend-tjänst.
    
    ![Lägg till röst frontend-tjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Först skapar vi hello URL toohello ReverseProxy för vår backend-tjänst **(1)**.
    3. Vi skickar hello HTTP PUT-begäran om toohello ReverseProxy **(2)**.
    4. Slutligen hello returnerar vi hello svar från hello backend-tjänst toohello klient **(3)**.

4. Tryck på **F5** toocontinue
    1. Du är nu vid hello break i hello backend-tjänst.
    
    ![Lägga till röst backend-tjänst](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. I hello första raden i hello metoden **(1)** vi använder hello `StateManager` tooget eller Lägg till en tillförlitlig ordlista som heter `counts`.
    3. All interaktion med värden i en tillförlitlig ordlista kräver en transaktion detta med hjälp av instruktionen **(2)** skapar den aktuella transaktionen.
    4. I hello transaktion vi sedan uppdatera hello värdet för hello relevanta nyckeln för hello röstning alternativet och incheckningar hello åtgärden **(3)**. När hello checkat returnerar metoden hello data uppdateras i hello ordlista och replikeras tooother noder i klustret hello. hello data lagras nu på ett säkert sätt i hello kluster och hello backend-tjänst kan växlas tooother noder och fortfarande har hello data är tillgängliga.
5. Tryck på **F5** toocontinue

toostop hello felsökningssessionen, tryck på **SKIFT + F5**.


## <a name="next-steps"></a>Nästa steg
I denna del av hello kursen får du lära dig hur du:

> [!div class="checklist"]
> * Skapa en ASP.NET Core Web API-tjänst som en tillståndskänslig tillförlitlig tjänst
> * Skapa ett webbprogram för ASP.NET Core-tjänst som en tillståndslös webbtjänst
> * Använda hello omvänd proxy toocommunicate med hello tillståndskänslig service

Avancerade toohello nästa kurs:
> [!div class="nextstepaction"]
> [Distribuera hello programmet tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md)
