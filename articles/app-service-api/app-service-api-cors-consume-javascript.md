---
title: "stöd för aaaCORS i App Service | Microsoft Docs"
description: "Lär dig hur toouse CORS stöd i Azure Apptjänst."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: alkarche
ms.openlocfilehash: c229378b75840bc0f7b2eefc3df3031233f9b494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a>Använda en API-app från JavaScript med CORS
Apptjänst har inbyggt stöd för [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), vilket gör att JavaScript-klienter toomake mellan domäner anropar tooAPIs som finns i API apps. Apptjänst kan du konfigurera CORS åtkomst tooyour API utan att skriva någon kod i ditt API.

Den här artikeln innehåller två avsnitt:

* Hej [hur tooconfigure CORS](#corsconfig) förklaras i allmänna hur tooconfigure CORS för API-app, webbapp eller mobilapp. Det gäller även tooall ramverk som stöds av Apptjänsten, inklusive .NET, Node.js och Java. 
* Från och med hello [fortsätter hello .NET komma igång-Självstudier](#tutorialstart) avsnittet hello artikeln är en genomgång som visar CORS-stöd genom att bygga vidare på vad du gjorde i [hello första API Apps komma igång-kursen ](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Hur tooconfigure CORS i Azure App Service
Du kan konfigurera CORS i hello Azure-portalen eller med hjälp av [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) verktyg.

#### <a name="configure-cors-in-hello-azure-portal"></a>Konfigurera CORS i hello Azure-portalen
1. I en webbläsare går toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **Apptjänster**, och klicka sedan på hello namnet på din API-app.
   
    ![Välj API-app i portalen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. I hello **inställningar** bladet som öppnas toohello höger om hello **API-app** bladet, hitta hello **API** avsnittet och klicka sedan på **CORS**.
   
   ![Välj CORS i bladet Inställningar](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Ange hello URL eller URL: er som du vill tooallow JavaScript-anrop toocome från i hello textruta.

    Om du har distribuerat JavaScript programmet tooa webbappen med namnet todolistangular anger du ”https://todolistangular.azurewebsites.net”. Alternativt kan ange du en asterisk (*) toospecify att alla ursprungsdomäner accepteras.


1. Klicka på **Spara**.
   
   ![Klicka på Spara](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   När du klickar på **spara**, hello API-appen att acceptera JavaScript-anrop från hello angivna URL: er.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurera CORS med hjälp av Azure Resource Manager-verktyg
Du kan också konfigurera CORS för API-app med hjälp av [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs) och hello [Azure CLI](../cli-install-nodejs.md). 

Ett exempel på en Azure Resource Manager-mall som anger CORS-egenskapen för hello öppna hello [azuredeploy.JSON i hello databasen för den här kursens exempelprogram](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Hitta hello avsnitt i hello-mall som ser ut som följande exempel hello:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Fortsätter hello .NET komma igång-kursen
Om du följer hello Node.js eller Java Kom igång-serien för API apps har du slutfört hello komma igång-serien. Hoppa över toohello [nästa steg](#next-steps) avsnittet toofind förslag på ytterligare utbildning om API Apps.

hello resten av den här artikeln är en förlängning av hello Kom igång-serien för .NET och förutsätter att du har slutfört [hello första självstudierna](app-service-api-dotnet-get-started.md).

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a>Distribuera hello ToDoListAngular-projektet tooa nytt webbprogram
I [hello första självstudierna](app-service-api-dotnet-get-started.md), du har skapat en mellannivå-API-app och en API-appen på datanivå. I den här självstudiekursen skapar du en enda sida program (SPA) webbapp som anrop hello mellannivå-API-app. Hello SPA toowork har tooenable CORS på hello mellannivå-API-app. 

I hello [exempelappen todolist](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular-projektet är en enkel AngularJS-klient som anropar hello mellannivå ToDoListAPI Web API-projekt. Hej JavaScript-kod i hello *app/scripts/todoListSvc.js* anropar hello-API med hjälp av hello AngularJS HTTP-leverantören. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a>Skapa ett nytt webbprogram för hello ToDoListAngular-projektet
Hej proceduren toocreate en ny App Service webbapp och distribuera ett projekt tooit är liknande toowhat som du såg för [att skapa och distribuera en API-app i hello första kursen i den här serien](app-service-api-dotnet-get-started.md#createapiapp). hello bara skillnaden är den typ av app hello är **Webbapp** i stället för **API-App**.  Skärmdumpar av dialogrutorna hello finns 

1. I **Solution Explorer**, högerklicka på hello ToDoListAngular-projektet och klicka sedan på **publicera**.
2. I hello **profil** för hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.
3. I hello **Apptjänst** dialogrutan klickar du på **ny**.
4. I hello **värd** för hello **skapa Apptjänst** dialogrutan Ange en **Webbprogramnamnet** som är unikt i hello *azurewebsites.net* domän. 
5. Välj hello Azure **prenumeration** du vill toowork med.
6. I hello **resursgruppen** listrutan Välj hello samma resursgrupp som du skapade tidigare.
7. I hello **Apptjänstplan** listrutan Välj hello samma plan som du skapade tidigare. 
8. Klicka på **Skapa**.
   
    Visual Studio skapar hello webbprogram, skapas en publiceringsprofil för den och visar hello **anslutning** steg i hello **Publicera webbplats** guiden.
   
    Klicka inte på **Publicera** ännu. I följande avsnitt hello, konfigurerar du hello ny web app toocall hello mellannivå-API-app som körs i Apptjänst. 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a>Ange URL för hello mellannivå i inställningarna för webbappen
1. Gå toohello [Azure-portalen](https://portal.azure.com/), och sedan gå toohello **Web App** bladet för hello webbprogrammet som du skapade toohost hello TodoListAngular (klientdelen)-projektet.
2. Klicka på **Inställningar > Programinställningar**.
3. I hello **appinställningar** lägger du till hello följande nyckel och värde:
   
   | Nyckel | Värde | Exempel |
   | --- | --- | --- |
   | toDoListAPIURL |https://{namnet på ditt API på mellannivå}.azurewebsites.net |https://todolistapi0121.azurewebsites.net |
4. Klicka på **Spara**.
   
    När hello koden körs i Azure åsidosätter det här värdet hello localhost-URL som är i hello *Web.config* fil. 
   
    hello-koden som hämtar Inställningsvärdet hello finns i *index.cshtml*:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    Hej koden i *todoListSvc.js* använder hello inställningen:
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a>Distribuera hello ToDoListAngular web project toohello nytt webbprogram
* I Visual Studio i hello **anslutning** steg i hello **Publicera webbplats** guiden, klickar du på **publicera**.
  
   Visual Studio distribuerar hello ToDoListAngular-projektet toohello ny webbapp och öppnar en webbläsare toohello URL av hello webbprogram. 

### <a name="test-hello-application-without-cors-enabled"></a>Testa programmet hello utan att CORS är aktiverat
1. Öppna hello konsolfönstret i webbläsarens utvecklingsverktyg.
2. I hello webbläsarfönster som visar hello AngularJS UI, klickar du på hello **tooDo lista** länk.
   
    hello JavaScript-kod försöker toocall hello mellannivå-API-app, men hello anropet misslyckas eftersom hello klientdelen körs i en annan domän än hello tillbaka slutet. konsolfönster för hello webbläsarens utvecklingsverktyg visar ett felmeddelande för cross-origin.
   
    ![Felmeddelande för cross-origin](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a>Konfigurera CORS för API-appen för hello mellannivå
I det här avsnittet kan du konfigurera hello CORS-inställningen i Azure för hello mellannivå API-appen ToDoListAPI. Den här inställningen tillåter hello mellannivå API app tooreceive JavaScript-anrop från hello webbapp som du skapade för hello ToDoListAngular-projektet.

1. I en webbläsare går du toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **Apptjänster**, och klicka sedan på hello ToDoListAPI (mellannivå) API-app.
   
    ![Välj API-app i portalen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. I hello **inställningar** bladet som öppnas toohello höger om hello **API-app** bladet, hitta hello **API** avsnittet och klicka sedan på **CORS**.
   
   ![Välj CORS i portalen](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Ange hello URL för webbappen ToDoListAngular (klientdelen) för hello hello i textrutan. Till exempel tillåta anrop från hello URL Om du har distribuerat hello ToDoListAngular-projektet tooa webbprogram med namnet todolistangular0121 `https://todolistangular0121.azurewebsites.net`.
   
   Alternativt kan ange du en asterisk (*) toospecify att alla ursprungsdomäner accepteras.
5. Klicka på **Spara**.
   
   ![Klicka på Spara](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   När du klickar på **spara**, hello API-appen att acceptera JavaScript-anrop från hello specificerat URL: en. I den här skärmdumpen kommer hello API-appen ToDoListAPI0223 acceptera JavaScript-klientanrop från hello webbappen todolistangular.

### <a name="test-hello-application-with-cors-enabled"></a>Testa programmet hello med CORS aktiverat
* Öppna en webbläsare toohello HTTPS-URL för hello webbprogrammet. 
  
    Det här programmet hello kan du visa, lägga till, redigera och ta bort arbetsuppgifter. 
  
    ![sidan för tooDo lista för exempelapp](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Apptjänst-CORS kontra Web API CORS
I Web API-projekt kan du installera hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketet toospecify i koden vilka domäner som din API ska ta emot JavaScript-anrop.

Stöd för Web API CORS är mer flexibelt än stöd för Apptjänst-CORS. Till exempel kan du i koden ange olika accepterade ursprung för olika åtgärdsmetoder, medan du anger en uppsättning accepterade ursprung för alla metoder i en API-app när det gäller Apptjänst-CORS.

> [!NOTE]
> Försök inte toouse både Web API CORS och Apptjänst CORS i en API-app. Apptjänst-CORS har företräde och Web API CORS har ingen effekt. Till exempel om du aktiverar en ursprungsdomän i Apptjänst och aktiverar alla ursprungsdomäner i Web API-koden, Azure API-app bara ska ta emot samtal från hello domän du angav i Azure.
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a>Hur tooenable CORS i Web API-koden
hello följande steg sammanfattar hello process för att aktivera stöd för Web API CORS. Mer information finns i [Aktivera cross-origin-begäranden i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. I ett Web API-projekt installerar hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketet.
2. Inkludera en `config.EnableCors()` kodrad i hello **registrera** metod för hello **WebApiConfig** klassen som hello följande exempel. 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // hello following line enables you toocontrol CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. I Web API-kontrollanten lägger du till en `using` -instruktion för hello `System.Web.Http.Cors` namnområde och Lägg till hello `EnableCors` attributet toohello klass eller tooindividual åtgärden kontrollantmetoder. I följande exempel hello, gäller CORS-stödet toohello hela kontrollanten.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>Använda Azure API Management med API Apps
Om du använder Azure API Management med en API-app måste du konfigurera CORS i API Management i stället för i hello API-app. Mer information finns i hello följande resurser:

* [Översikt över Azure API Management (video: CORS börjar vid 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Korsdomänprinciper för API Management](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Felsökning
Om du stöter på problem när du går igenom den här kursen får du några felsökningsidéer här.

* Kontrollera att du använder hello senaste versionen av hello [Azure SDK för .NET för Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).
* Kontrollera att du har angett `https` i hello CORS-inställningen och se till att du använder `https` toorun hello frontend-webbprogram.
* Kontrollera att du har angett hello CORS-inställningen i API-appen för hello mellannivå, inte i hello frontend-webbprogram.
* Om du konfigurerar CORS i både programkoden och Azure App Service, Observera att hello Apptjänst-CORS-inställning åsidosätter vad du gör i programkoden. 

toolearn mer om Visual Studio-funktioner som förenklar felsökningen, se [felsöka Azure Apptjänst-appar i Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Nästa steg
I den här artikeln har sett du hur tooenable Apptjänst-CORS stöder så att klientens JavaScript-kod kan anropa ett API i en annan domän. Mer om API apps, läsa hello toolearn [introduktion tooauthentication i App Service](../app-service/app-service-authentication-overview.md), och sedan gå toohello [användarautentisering för API apps](app-service-api-dotnet-user-principal-auth.md) kursen.

