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
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Kom igång med API Apps, ASP.NET och Swagger i Azure Apptjänst
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Detta är hello först i en serie kurser som visar hur toouse funktioner i Azure App Service som är användbara för att utveckla och vara värd för RESTful-API: er.  I den här kursen ingår stöd för API-metadata i Swagger-format.

Du får lära dig:

* Hur toocreate och distribuera [API apps](app-service-api-apps-why-best-platform.md) i Azure App Service med hjälp av verktygen i Visual Studio 2015.
* Hur tooautomate API-identifiering med hjälp av hello Swashbuckle NuGet-paketet toodynamically generera Swagger API-metadata.
* Hur toouse Swagger API-metadata tooautomatically generera klientkod till en API-app.

## <a name="sample-application-overview"></a>Översikt över exempelprogrammet
I den här kursen får du arbeta med ett enkelt exempelprogram med en att göra-lista. hello-programmet har en klientdel för en sida-program (SPA), ett ASP.NET Web API-mellannivå och en ASP.NET Web API-datanivå.

![Diagram över exempelprogram i API Apps](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Här är en skärmbild av hello [AngularJS](https://angularjs.org/) klientdelen.

![API Apps exempel toodo lista](./media/app-service-api-dotnet-get-started/todospa.png)

hello Visual Studio-lösningen innehåller tre projekt:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** -hello klientdelen: en AngularJS SPA som anropar hello mellannivå.
* **ToDoListAPI** -hello mellannivån: ett ASP.NET Web API-projekt som anropar hello datanivå tooperform CRUD-åtgärder på arbetsuppgifter.
* **ToDoListDataAPI** -hello datanivån: ett ASP.NET Web API-projekt som utför CRUD-åtgärder på arbetsuppgifter.

hello arkitekturen med tre nivåer är en av många arkitekturer som du kan implementera med hjälp av API Apps och används här endast i demonstrationssyfte. hello koden i varje nivå är så enkelt som möjligt toodemonstrate API Apps funktioner; hello datanivån använder till exempel serverminne i stället för en databas som persistencemekanism.

På den här kursen har du hello två Web API-projekten och körs i hello moln i App Service API apps.

hello nästa kurs i serien hello distribuerar hello SPA klientdelen toohello moln.

## <a name="prerequisites"></a>Krav
* ASP.NET Web API - hello självstudiekursen förutsätts att du har grundläggande kunskaper om hur toowork med ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) i Visual Studio.
* Azure-konto – Du kan [öppna ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
  
    Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du omedelbart skapa en tillfällig startapp i Apptjänst – **inget kreditkort behövs** och du gör inga åtaganden.
* Visual Studio 2015 med hello [Azure SDK för .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK installerar Visual Studio 2015 automatiskt om det inte redan har.
  
  * I Visual Studio klickar du på Hjälp -> Om Microsoft Visual Studio och ser till att du har "Azure Apptjänst-verktyg v2.9.1" eller senare installerat.
    
    ![Azure App-verktyg version](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > Beroende på hur många av hello SDK beroenden du redan har på din dator, kan installera hello SDK ta lång tid, från flera minuter tooa halvtimme eller flera.
    > 
    > 

## <a name="download-hello-sample-application"></a>Hämta hello exempelprogrammet
1. Hämta hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) databasen.
   
    Du kan klicka på hello **hämta ZIP** knapp eller klona hello-databasen på den lokala datorn.
2. Öppna hello ToDoList-lösningen i Visual Studio 2015 eller 2013.
   
   1. Du behöver tootrust varje lösning.
         ![Säkerhetsvarning](./media/app-service-api-dotnet-get-started/securitywarning.png)
3. Skapa hello-lösning (CTRL + SKIFT + B) toorestore hello NuGet-paket.
   
    Om du vill toosee hello programmet i åtgärd innan du distribuerar det, kan du köra det lokalt. Se till att ToDoListDataAPI är din Startprojekt och kör hello lösning. Du kan förvänta toosee HTTP 403-fel i webbläsaren.

## <a name="use-swagger-api-metadata-and-ui"></a>Använd Swagger API-metadata och -användargränssnitt
Stöd för [Swagger](http://swagger.io/) 2.0 API-metadata är inbyggt i Azure Apptjänst. Varje API-app kan ange en URL-slutpunkt som returnerar metadata för hello API i Swagger JSON-format. hello metadata som returneras från slutpunkten kan vara används toogenerate klientkod.

En ASP.NET Web API-projekt kan dynamiskt generera Swagger-metadata med hjälp av hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketet. Hej Swashbuckle NuGet-paketet har redan installerats i hello ToDoListDataAPI- och ToDoListAPI-projekt som du hämtat.

I det här avsnittet av kursen hello du tittar på hello genererade Swagger 2.0-metadata och försök på ett testanvändargränssnitt som baseras på hello Swagger-metadata.

1. Ange hello ToDoListDataAPI-projektet (**inte** hello ToDoListAPI-projektet) som hello Startprojekt.
   
    ![Ange ToDoDataAPI som startprojekt](./media/app-service-api-dotnet-get-started/startupproject.png)
2. Tryck på F5 eller klicka på **Felsök > Starta felsökning** toorun hello projektet i felsökningsläge.
   
    hello webbläsaren öppnas och visar hello HTTP 403 felsidan.
3. I webbläsarens adressfält, lägger du till `swagger/docs/v1` toohello slutet av hello rad och tryck sedan på RETUR. (URL: en är hello `http://localhost:45914/swagger/docs/v1`.)
   
    Detta är hello standard-URL som används av Swashbuckle tooreturn Swagger 2.0 JSON-metadata för hello API.
   
    Om du använder Internet Explorer uppmanar webbläsaren hello toodownload en *v1.json* fil.
   
    ![Hämta JSON-metadata i IE](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    Om du använder Chrome, Firefox eller Edge visar hello webbläsare hello JSON i webbläsarfönstret hello. Olika webbläsare hanterar JSON på olika sätt och ditt webbläsarfönster ser kanske inte ser ut precis som hello exempel.
   
    ![JSON-metadata i Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    hello följande exempel visar hello första delen av hello Swagger-metadata för hello API, med hello definition för hello Get-metod. Dessa metadata är vilka enheter hello Swagger-användargränssnitt som du använder i hello följande steg och du använder den i ett senare avsnitt av kursen hello-tooautomatically generera klientkod.
   
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
4. Stäng hello webbläsaren och stoppa felsökning i Visual Studio.
5. I hello ToDoListDataAPI-projekt i **Solution Explorer**öppnar hello *App_Start\SwaggerConfig.cs* filen och rullar ned tooline 174 och Avkommentera hello följande kod.
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    Hej *SwaggerConfig.cs* filen skapas när du installerar hello Swashbuckle-paketet i ett projekt. hello-filen innehåller ett antal sätt tooconfigure Swashbuckle.
   
    hello-kod som du har tagit bort kommentarstecken från aktiverar hello Swagger-användargränssnitt som du använder i hello följande steg. När du skapar ett Web API-projekt med hjälp av projektmallen för hello API-app är koden kommenterade som standard som en säkerhetsåtgärd.
6. Kör hello projektet igen.
7. I webbläsarens adressfält, lägger du till `swagger` toohello slutet av hello rad och tryck sedan på RETUR. (URL: en är hello `http://localhost:45914/swagger`.)
8. När hello Swaggers Användargränssnittssida visas klickar du på **ToDoList** toosee hello-metoder som är tillgängliga.
   
    ![Tillgängliga metoder för Swagger-användargränssnittet](./media/app-service-api-dotnet-get-started/methods.png)
9. Klicka på hello först **hämta** knappen i hello lista.
10. I hello **parametrar** ange en asterisk som hello värde för hello `owner` parameter och klicka sedan på **prova**.
    
    När du lägger till autentisering under senare kurser ger hello mellannivån hello faktiska användar-ID toohello datanivå. För tillfället har alla uppgifter asterisk som ägar-ID medan programmet hello körs utan autentisering aktiverad.
    
    ![Prova Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    Hej Swagger-användargränssnitt anropar hello ToDoList Get-metoden och visar hello svarskoden och JSON-resultat.
    
    ![Resultat av att prova Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. Klicka på **Post**, och klicka sedan på hello rutan under **modellschemat**.
    
    Klicka på hello modellen schemat prefills hello textrutan där du kan ange hello parametervärdet för hello Post-metoden. (Om det inte fungerar i Internet Explorer, en annan webbläsare eller ange hello parametervärdet manuellt i hello nästa steg.)  
    
    ![Prova att posta i Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/post.png)
12. Ändra hello JSON i hello `todo` parametern indata så att det ser ut som följande exempel hello eller ersätta din egen beskrivningstext:
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. Klicka på **Prova**.
    
    Hej ToDoList API: N returnerar en HTTP 204-svarskod som indikerar att det lyckades.
14. Klicka på hello först **hämta** knappen och klicka sedan på hello i det avsnittet av sidan hello **prova** knappen.
    
    hello hämta-metodsvaret innehåller nu hello nytt toodo objekt.
15. Valfritt: Prova också hello Put, Delete, och hämta med ID-metoderna.
16. Stäng hello webbläsaren och stoppa felsökning i Visual Studio.

Swashbuckle fungerar med alla ASP.NET Web API-projekt. Om du vill tooadd Swagger-metadata generation tooan befintligt projekt installerar du bara hello Swashbuckle-paketet.

> [!NOTE]
> Swagger-metadata innehåller ett unikt ID för varje API-åtgärd. Som standard kan Swashbuckle generera duplicerade Swagger-åtgärds-ID till dina kontrollantmetoder för Web API.  Det händer om din kontrollant har överbelastade http-metoder, exempelvis `Get()` och `Get(id)`. Information om hur toohandle overloads finns [anpassa Swashbuckle-genererade API-definitioner](app-service-api-dotnet-swashbuckle-customize.md). Om du skapar ett Web API-projekt i Visual Studio med hjälp av mallen för hello Azure API Apps läggs koden som skapar unika åtgärds-ID automatiskt toohello *SwaggerConfig.cs* fil.  
> 
> 

## <a id="createapiapp"></a>Skapa en API-app i Azure och distribuera kod tooit
I det här avsnittet kan du använda Azure-verktyg som är integrerade i hello Visual Studio **Publicera webbplats** guiden toocreate ett nytt API-app i Azure. Du distribuerar sedan hello ToDoListDataAPI-projektet toohello nya API-app och anropa hello API genom att köra hello Swagger-användargränssnitt.

1. I **Solution Explorer**, högerklicka på hello ToDoListDataAPI-projektet och klicka sedan på **publicera**.
   
    ![Klicka på Publicera i Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. I hello **profil** steg i hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.
   
   ![Klicka på Azure Apptjänst i Publicera webbplats](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. Logga in tooyour Azure-konto om du inte redan har gjort det eller uppdatera dina autentiseringsuppgifter om de upphört att gälla.
4. I dialogrutan för Apptjänst hello, Välj hello Azure **prenumeration** du toouse, och klickar sedan på **ny**.
   
    ![Klicka på Ny i dialogrutan för Apptjänst](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    Hej **värd** för hello **skapa App Service** dialogrutan visas.
   
    Eftersom du distribuerar ett Web API-projekt där Swashbuckle är installerat, förutsätter Visual Studio som du vill toocreate en API-App. Detta anges av hello **API App Name** title och av hello faktum att hello **Ändringstypen** listrutan har angetts för**API-App**.
   
    ![Typ av app i dialogrutan för Apptjänst](./media/app-service-api-dotnet-get-started/apptype.png)
5. Ange en **API App Name** som är unikt i hello *azurewebsites.net* domän. Du kan acceptera hello standardnamnet som Visual Studio föreslår.
   
    Om du anger ett namn som någon annan redan har använt visas ett rött utropstecken toohello höger.
   
    hello Webbadressen till hello API-app kommer att `{API app name}.azurewebsites.net`.
6. I hello **resursgruppen** listrutan, klickar du på **ny**, och ange sedan ”ToDoListGroup” eller ett annat namn om du föredrar.
   
    En resursgrupp är en samling Azure-resurser, till exempel API Apps, databaser eller virtuella datorer.    Den här kursen är det bästa toocreate en ny resursgrupp eftersom som gör det enkelt toodelete i ett steg som alla hello Azure-resurser som du skapar för kursen hello.
   
    I den här rutan kan du välja en befintlig [resursgrupp](../azure-resource-manager/resource-group-overview.md) eller skapa en ny genom att skriva in ett namn som skiljer sig från befintliga resursgrupper i din prenumeration.
7. Klicka på hello **ny** knappen Nästa toohello **Apptjänstplan** listrutan.
   
    hello skärmbilden visar exempelvärden för **API App Name**, **prenumeration**, och **resursgruppen** --värdena är olika.
   
    ![Dialogrutan Skapa App Service](./media/app-service-api-dotnet-get-started/createas.png)
   
    I följande hello skapar du en apptjänstplan för hello nya resursgruppen. En apptjänstplan anger hello beräkningsresurserna som API-appen körs på. Till exempel om du väljer hello kostnadsfria nivån API-appen körs på delade virtuella datorer medan den körs på dedikerade virtuella datorer för vissa betalnivåer. Information om apptjänstplaner finns i [Översikt över Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)
8. I hello **konfigurera Apptjänstplan** dialogrutan anger du ”ToDoListPlan” eller ett annat namn om du föredrar.
9. I hello **plats** listrutan väljer du hello-plats som är närmast tooyou.
   
    Den här inställningen anger vilket Azure-datacenter appen ska köras i. Välj en plats Stäng tooyou toominimize [svarstid](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).
10. I hello **storlek** listrutan, klickar du på **lediga**.
    
    I den här självstudien ger hello kostnadsfria prisnivån tillräcklig prestanda.
11. I hello **konfigurera Apptjänstplan** dialogrutan klickar du på **OK**.
    
    ![Klicka på OK i Konfigurera Apptjänstplan](./media/app-service-api-dotnet-get-started/configasp.png)
12. I hello **skapa Apptjänst** dialogrutan klickar du på **skapa**.
    
    ![Klicka på Skapa i dialogrutan Skapa Apptjänst](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    Visual Studio skapar hello API-app och en publiceringsprofil som innehåller alla de hello krävs inställningarna för hello API-appen. Sedan öppnas hello **Publicera webbplats** guiden som du ska använda toodeploy hello projektet.
    
    Hej **Publicera webbplats** öppnas på hello **anslutning** (se nedan).
    
    På hello **anslutning** fliken hello **Server** och **platsnamn** inställningar punkt tooyour API-app. Hej **användarnamn** och **lösenord** är autentiseringsuppgifter för distribution som Azure skapar åt dig. Efter distributionen öppnar Visual Studio en webbläsare toohello **Måladress** (som är hello enda syftet med **Måladress**).  
13. Klicka på **Nästa**.
    
    ![Klicka på Nästa på fliken Anslutning i Publicera webbplats](./media/app-service-api-dotnet-get-started/connnext.png)
    
    hello nästa flik är hello **inställningar** (se nedan). Här kan du ändra hello build configuration fliken toodeploy en felsökningsversion för [fjärrfelsökning](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). hello fliken finns också flera **alternativ för Filpublicering**:
    
    * Ta bort extra filer från destinationen
    * Förkompilera vid publicering
    * Undanta filer från hello App_Data mapp
    
    Under den här kursen behöver du ingen av dem. Utförliga förklaringar av vad de gör finns i [Så här distribuerar du ett webbprojekt med Publicera med ett klick i Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).
14. Klicka på **Nästa**.
    
    ![Klicka på Nästa på fliken Inställningar i Publicera webbplats](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    Nästa är hello **Preview** fliken (se nedan), vilket ger dig en möjlighet toosee vilka filer som ska kopieras från project toohello API-app toobe. När du distribuerar ett projekt tooan API-app som du redan har distribuerat tooearlier, kopieras bara ändrade filer. Om du vill toosee en lista över vad som ska kopieras, kan du klicka på hello **starta förhandsgranskning** knappen.
15. Klicka på **Publicera**.
    
    ![Klicka på Publicera på fliken Förhandsgranska i Publicera webbplats](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    Visual Studio distribuerar hello ToDoListDataAPI-projektet toohello nya API-app. Hej **utdata** loggar slutförd distribution och en ”har skapats” visas i en webbläsare öppnas fönstret toohello URL hello API-appen.
    
    ![Utdatafönster visar slutförd distribution](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Sida som visar att en ny API-app har skapats](./media/app-service-api-dotnet-get-started/appcreated.png)
16. Lägg till ”swagger” toohello URL hello webbläsarens adressfält och tryck sedan på RETUR. (URL: en är hello `http://{apiappname}.azurewebsites.net/swagger`.)
    
    hello webbläsaren visar samma Swagger-användargränssnitt som du såg tidigare, men nu körs i molnet hello hello. Testa hello Get-metoden och du ser att du är tillbaka toohello standard 2 att göra-objekt. hello ändringar du gjort tidigare har sparats i minnet i hello lokal dator.
17. Öppna hello [Azure-portalen](https://portal.azure.com/).
    
    hello Azure-portalen är ett webbgränssnitt för att hantera Azure-resurser, till exempel API apps.
18. Klicka på **Fler tjänster > Apptjänster**.
    
    ![Bläddra i Apptjänster](./media/app-service-api-dotnet-get-started/browseas.png)
19. I hello **Apptjänster** bladet och klicka på din nya API-app. (I hello Azure-portalen kallas fönster som öppnas toohello höger *blad*.)
    
    ![Apptjänstblad](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    Två blad öppna. Det ena bladet visar en översikt över hello API-app och en har en lång lista med inställningar som du kan visa och ändra.
20. I hello **inställningar** bladet, hitta hello **API** avsnittet och klicka på **API-Definition**.
    
    ![API-definition på bladet Inställningar](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    Hej **API-Definition** bladet kan du ange hello-URL som returnerar Swagger 2.0-metadata i JSON-format. När Visual Studio skapar hello API-app, anger hello API definition URL toohello standardvärde för Swashbuckle-genererade metadata som du såg tidigare, vilket är hello API-app grundläggande URL plus `/swagger/docs/v1`.
    
    ![URL för API-definition](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    När du väljer en API-app toogenerate klientkod för den hämtar Visual Studio hello metadata från denna URL.

## <a id="codegen"></a>Generera klientkod för hello datanivå
En av hello fördelarna med att integrera Swagger i Azure API apps är automatisk kodgenerering. Genererade klientklasser gör det enklare toowrite kod som anropar en API-app.

hello ToDoListAPI-projektet har redan hello genereras klientkoden, men i följande hello du ta bort den och återskapa den toosee hur toodo hello kodgenereringen.

1. I Visual Studio **Solution Explorer**, i hello ToDoListAPI-projektet, ta bort hello *ToDoListDataAPI* mapp. **Varning: Ta bort endast hello mappen, inte hello ToDoListDataAPI-projektet.**
   
    ![Ta bort genererad klientkod](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    Den här mappen har skapats med hjälp av hello kodgenereringsprocess som du är om toogo via.
2. Högerklicka på hello ToDoListAPI-projektet och klicka sedan på **Lägg till > REST API-klient**.
   
    ![Lägg till REST API-klienten i Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. I hello **Lägg till REST API-klient** dialogrutan klickar du på **Swagger URL**, och klicka sedan på **Välj Azure-tillgång**.
   
    ![Välj Azure-tillgång](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. I hello **Apptjänst** dialogrutan expanderar hello resursgruppen som du använder för den här kursen väljer din API-app och klicka sedan på **OK**.
   
    ![Välj API-app för kodgenerering](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    Observera att när du går tillbaka toohello **Lägg till REST API-klient** dialogrutan hello textruta har fyllts i med hello API-definition URL-värdet som du såg tidigare i hello-portalen.
   
    ![API-definitions-URL](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > Ett annat sätt tooget metadata för kodgenerering är tooenter hello URL direkt i stället för att gå igenom hello bläddringsdialogrutan. Om du vill toogenerate klientkoden innan du distribuerar tooAzure, kan du köra hello Web API-projekt lokalt, gå toohello URL som innehåller hello Swagger JSON-filen, sparar hello-filen och använda hello **Välj en befintlig Swagger-metadatafil**alternativet.
   > 
   > 
5. I hello **Lägg till REST API-klient** dialogrutan klickar du på **OK**.
   
    Visual Studio skapar en mapp med namnet efter hello API-appen och genererar klientklasser.
   
    ![Koda filer för en genererad klient](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. Öppna i hello ToDoListAPI-projektet *Controllers\ToDoListController.cs* toosee hello koden på rad 40 som anropar API hello med hello genererade klienten.
   
    hello följande utdrag visar hur koden hello instansierar hello objekt och anrop hello Get-metoden.
   
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
   
    Hej konstruktorparametern hämtar slutpunkts-URL för hello från hello `toDoListDataAPIURL` appinställningen. Detta värde är i filen Web.config för hello set toohello lokala IIS Express URL för hello API-projekt så att du kan köra programmet hello lokalt. Om du utelämnar konstruktorparametern hello är hello standardslutpunkten hello-URL som du skapade hello koden från.
7. Klientklassen kommer att skapas med ett annat namn baserat på din API-appnamn; Ändra hello koden i *Controllers\ToDoListController.cs* så att hello typnamnet matchar det som har genererats i projektet. Om du till exempel har gett din API-app namnet ToDoListDataAPI071316 ändrar du den här koden:
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

toothis:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a>Skapa en API-app toohost hello mellannivå
Tidigare du [skapas hello-appen på datanivå-API och distribueras kod tooit](#createapiapp).  Nu du följer hello samma procedur för hello mellannivå-API-app.

1. I **Solution Explorer**, högerklicka på hello mellannivå ToDoListAPI projektet (inte hello ToDoListDataAPI på datanivå) och klicka sedan på **publicera**.
   
    ![Klicka på Publicera i Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. I hello **profil** för hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.
3. I hello **Apptjänst** dialogrutan klickar du på **ny**.
4. I hello **värd** för hello **skapa Apptjänst** dialogrutan godkänner hello standard **API App Name** eller ange ett namn som är unikt i hello * azurewebsites.NET* domän.
5. Välj hello Azure **prenumeration** du har använt.
6. I hello **resursgruppen** listrutan, Välj hello samma resursgrupp som du skapade tidigare.
7. I hello **Apptjänstplan** listrutan, Välj hello samma plan som du skapade tidigare. Som standard den toothat värde.
8. Klicka på **Skapa**.
   
    Visual Studio skapar hello API-appen, skapas en publiceringsprofil för den och visar hello **anslutning** steg i hello **Publicera webbplats** guiden.
9. I hello **anslutning** steg i hello **Publicera webbplats** guiden, klickar du på **publicera**.
   
   Visual Studio distribuerar hello ToDoListAPI-projektet toohello nya API-appen och öppnar en webbläsare toohello Webbadressen för hello API-app. Hej ”har skapats” visas.

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a>Konfigurera hello mellannivå toocall hello datanivå
Om du nu kallas hello mellannivå-API-app, skulle den försöka toocall hello datanivån med hello localhost-URL som finns kvar i hello Web.config-filen. I det här avsnittet anger hello data nivå API-appens URL i en miljöinställning i hello mellannivå-API-app. När hello koden i hello mellannivå-API-appen hämtar inställningen för hello data nivå URL, åsidosätter hello vad som finns i hello Web.config-filen.

1. Gå toohello [Azure-portalen](https://portal.azure.com/), och sedan gå toohello **API-App** bladet för hello API-app som du skapade toohost hello TodoListAPI (mellannivå)-projektet.
2. Hej API-App i **inställningar** bladet, klickar du på **programinställningar**.
3. Hej API-App i **programinställningar** rullar du ned toohello **appinställningar** och lägger till hello följande nyckel och värde. hello värdet kommer att vara hello URL för hello första API App som du publicerade i den här kursen.
   
   | **Nyckel** | toDoListDataAPIURL |
   | --- | --- |
   | **Värde** |https://{namnet på din API på datanivå}.azurewebsites.net |
   | **Exempel** |https://todolistdataapi.azurewebsites.net |
4. Klicka på **Spara**.
   
    ![Klicka på Spara för app-inställningar](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    När hello koden körs i Azure, åsidosätter det här värdet hello localhost-URL som är i hello Web.config-filen.

## <a name="test"></a>Testa
1. I ett webbläsarfönster bläddrar du toohello URL för hello nya mellannivå-API-app som du nyss skapade för ToDoListAPI. Du kan hämta det genom att klicka på hello URL: en i hello API-appens huvudblad i hello-portalen.
2. Lägg till ”swagger” toohello URL hello webbläsarens adressfält och tryck sedan på RETUR. (URL: en är hello `http://{apiappname}.azurewebsites.net/swagger`.)
   
    hello webbläsaren visar hello samma Swagger-användargränssnitt som du tidigare såg för ToDoListDataAPI, men nu `owner` är inte ett obligatoriskt fält för hello hämta-åtgärden eftersom hello mellannivå-API-appen skickar den värdet toohello API-appen på datanivå åt dig. (När du hello autentisering självstudier, skickar hello mellannivån faktiska användar-ID för hello `owner` parametern; för nu den hårdkodar det en asterisk.)
3. Testa hello Get-metoden och hello toovalidate andra metoder som hello API-appen på mellannivå anropar hello datanivå-API-app.
   
    ![Hämta-metoden i Swaggers användargränssnitt](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Felsökning
Om du stöter på problem när du går igenom den här kursen får du några felsökningsidéer här:

* Kontrollera att du använder hello senaste versionen av hello [Azure SDK för .NET](http://go.microsoft.com/fwlink/?linkid=518003).
* Två av projektnamnen hello är liknar varandra (ToDoListAPI, ToDoListDataAPI). Om saker ser inte ut som beskrivs i hello anvisningar när du arbetar med ett projekt, kontrollera att du har öppnat hello rätt projekt.
* Om du arbetar i ett företagsnätverk och försöker toodeploy tooAzure Apptjänst via en brandvägg kan du kontrollera att portarna 443 och 8172 är öppna för webbdistribution. Om du inte kan öppna de portarna kan du använda andra metoder för distribution.  Se [distribuera din app tooAzure Apptjänst](../app-service-web/web-sites-deploy.md).
* Fel ”ruttnamn måste vara unika” – du kan få dem om du av misstag distribuerar hello fel projekt tooan API-appen och sedan distribuera hello rätt en tooit. toocorrect, Omdistributionen hello rätt projekt toohello API appen, och på hello **inställningar** för hello **Publicera webbplats** guiden Välj **ta bort extra filer från destinationen**.

När du har din ASP.NET API-app som körs i Azure App Service kanske toolearn mer om Visual Studio-funktioner som förenklar felsökningen. Information om bland annat loggning och fjärrfelsökning finns i [Felsöka Azure Apptjänst-appar i Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Nästa steg
Du har sett hur toodeploy befintliga Web API-projekt tooAPI apps, genererar klientkod till API apps och använder API apps från .NET-klienter. hello nästa kurs i serien visar hur för[CORS tooconsume API apps från JavaScript-klienter använda](app-service-api-cors-consume-javascript.md).

Mer information om klientkodgenerering finns hello [Azure/AutoRest](https://github.com/azure/autorest) på GitHub.com. Hjälp med problem med att använda hello genererade klienten, öppna ett [problem i hello AutoRest databasen](https://github.com/azure/autorest/issues).

Om du vill toocreate nya API-app-projekt från grunden använder hello **Azure API Apps** mall.

![Mall för API Apps i Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Hej **Azure API Apps** projektmall är likvärdiga toochoosing hello **tom** ASP.NET 4.5.2-mallen mall, klicka på hello kryssrutan tooadd Web API-stöd och installera hello Swashbuckle NuGet-paketet. Hello mallen lägger dessutom till vissa Swashbuckle kod som utformats för tooprevent hello Skapa konfiguration för duplicerade Swagger-åtgärds-ID: N. När du har skapat ett API-App-projekt kan du distribuera den tooan API app hello samma sätt som du såg i den här kursen.

