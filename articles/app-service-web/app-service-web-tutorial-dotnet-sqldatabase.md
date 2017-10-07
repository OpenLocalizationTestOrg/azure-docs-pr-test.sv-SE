---
title: aaaBuild en ASP.NET-app i Azure med SQL-databas | Microsoft Docs
description: "Lär dig hur tooget en ASP.NET app arbetar i Azure, med anslutning tooa SQL-databas."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d21c2bc404bfe038608c17e5a94d96847153002c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Skapa en ASP.NET-app i Azure med SQL-databas

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst. Den här kursen visar hur toodeploy en datadrivna ASP.NET webbapp i Azure och koppla den för[Azure SQL Database](../sql-database/sql-database-technical-overview.md). När du är klar har du en ASP.NET-app som körs i [Azure App Service](../app-service/app-service-value-prop-what-is.md) och anslutna tooSQL databas.

![Publicerade ASP.NET-program i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en SQL-databas i Azure
> * Ansluta en ASP.NET-app tooSQL databas
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen loggas från Azure tooyour terminal
> * Hantera hello appen i hello Azure-portalen

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

* Installera [Visual Studio 2017](https://www.visualstudio.com/downloads/) med hello följande arbetsbelastningar:
  - **ASP.NET och webbutveckling**
  - **Azure Development**

  ![ASP.NET och webbutveckling och Azure Development (under webb och moln)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Hämta hello-exempel

[Hämta hello exempelprojektet](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Extrahera (packa) hello *dotnet-sqldb-kursen-master.zip* fil.

hello exempelprojektet innehåller en grundläggande [ASP.NET MVC](https://www.asp.net/mvc) CRUD (skapa-Läs-Uppdatera-ta bort) app med hjälp av [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-hello-app"></a>Kör hello-appen

Öppna hello *dotnet-sqldb-kursen-master/DotNetAppSqlDb.sln* filen i Visual Studio. 

Typen `Ctrl+F5` toorun hello app utan felsökning. hello appen visas i din standardwebbläsare. Välj hello **Skapa nytt** länka och skapa ett par *uppgiften* objekt. 

![Dialogrutan Nytt ASP.NET-projekt](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Testa hello **redigera**, **information**, och **ta bort** länkar.

hello appen använder en databas kontexten tooconnect med hello-databas. I det här exemplet hello databaskontexten använder en anslutningssträng som heter `MyDbConnection`. hello anslutningssträngen har angetts i hello *Web.config* fil- och refereras till i hello *Models/MyDatabaseContext.cs* fil. hello namn för anslutningssträngen används senare i hello självstudiekursen tooconnect hello Azure web app tooan Azure SQL Database. 

## <a name="publish-tooazure-with-sql-database"></a>Publicera tooAzure med SQL-databas

I hello **Solution Explorer**, högerklicka på din **DotNetAppSqlDb** projektet och välj **publicera**.

![Publicera från Solution Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Se till att **Microsoft Azure App Service** är markerat och klicka på **Publicera**.

![Publicera från projektöversiktssidan](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Publicering öppnar hello **skapa App Service** dialogrutan, vilket hjälper dig att skapa alla hello Azure-resurser du behöver toorun ASP.NET-webbapp i Azure.

### <a name="sign-in-tooazure"></a>Logga in tooAzure

I hello **skapa Apptjänst** dialogrutan klickar du på **Lägg till ett konto**, och logga sedan in tooyour Azure-prenumeration. Om du redan är inloggad på ett Microsoft-konto kontrollerar du att kontot tillhör din Azure-prenumeration. Om hello inloggade Microsoft-konto inte har din Azure-prenumeration, klicka på den tooadd hello rätt konto.
   
![Logga in tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

När du är inloggad är du redo toocreate alla hello resurser som du behöver för din Azure-webbapp i den här dialogrutan.

### <a name="configure-hello-web-app-name"></a>Konfigurera hello webbprogrammets namn

Du kan behålla hello genereras webbprogrammets namn eller ändra det tooanother unikt namn (giltiga tecken är `a-z`, `0-9`, och `-`). hello webbprogrammets namn används som en del av hello standard-URL för din app (`<app_name>.azurewebsites.net`, där `<app_name>` är din webbprogrammets namn). hello webbprogrammets namn måste toobe unikt över alla program i Azure. 

![Skapa app service dialog](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Nästa för**resursgruppen**, klickar du på **ny**.

![Nästa tooResource gruppen, klicka på nytt.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Namnet hello resursgruppen **myResourceGroup**.

> [!NOTE]
> Klicka inte på **skapa**. Du måste först tooset upp en SQL-databas i ett senare steg.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Nästa för**Apptjänstplan**, klickar du på **ny**. 

I hello **konfigurera Apptjänstplan** dialogrutan Konfigurera hello nya App Service-plan med hello följande inställningar:

![Skapa apptjänstplan](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Inställning  | Föreslaget värde | Mer information |
| ----------------- | ------------ | ----|
|**App Service-Plan**| myAppServicePlan | [App Service-planer](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Plats**| Västra Europa | [Azure-regioner](https://azure.microsoft.com/regions/) |
|**Storlek**| Kostnadsfri | [Prisnivåer](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a>Skapa en SQL Server-instans

Innan du skapar en databas måste en [logisk Azure SQL Database-server](../sql-database/sql-database-features.md). En logisk server innehåller en uppsättning databaser som hanteras som en grupp.

Välj **utforska ytterligare Azure-tjänster**.

![Ange webbappnamn](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

I hello **Services** klickar du på hello  **+**  ikonen nästa för**SQL-databas**. 

![I fliken hello tjänster klickar du på hello + ikonen nästa tooSQL databas.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

I hello **Konfigurera SQL-databas** dialogrutan klickar du på **ny** nästa för**SQL Server**. 

Ett unikt servernamn genereras. Det här namnet används som en del av hello standard-URL för din logiska server `<server_name>.database.windows.net`. Det måste vara unikt över alla logiska serverinstanser i Azure. Du kan ändra hello servernamn, men behålla hello genereras värdet för den här självstudiekursen.

Lägga till en administratörsanvändarnamn och lösenord och välj sedan **OK**. Kraven på lösenordskomplexitet, se [lösenordsprincip](/sql/relational-databases/security/password-policy).

Kom ihåg detta användarnamn och lösenord. Du behöver toomanage hello logisk server-instansen senare.

![Skapa SQL Server-instans](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a>Skapa en SQL Database

I hello **Konfigurera SQL-databas** dialogrutan: 

* Behåll hello standardgenererade **databasnamnet**.
* I **namn för anslutningssträngen**, typen *MyDbConnection*. Det här namnet måste matcha hello anslutningssträngen som refereras i *Models/MyDatabaseContext.cs*.
* Välj **OK**.

![Konfigurera SQL-databas](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

Hej **skapa App Service** visar hello resurser du har skapat. Klicka på **Skapa**. 

![hello-resurser som du har skapat](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

När hello guiden är klar att skapa hello Azure-resurser, publicerar tooAzure din ASP.NET-app. Standardwebbläsaren startas med hello URL toohello distribuerade appen. 

Lägg till några arbetsuppgifter.

![Publicerade ASP.NET-program i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Grattis! ASP.NET-programmet datadrivna körs live i Azure App Service.

## <a name="access-hello-sql-database-locally"></a>Komma åt lokalt hello SQL-databas

Visual Studio kan du hantera din nya SQL-databas enkelt i hello och utforska **SQL Server Object Explorer**.

### <a name="create-a-database-connection"></a>Skapa en databasanslutning

Från hello **visa** väljer du **SQL Server Object Explorer**.

Hello överst i **SQL Server Object Explorer**, klicka på hello **Lägg till SQL Server** knappen.

### <a name="configure-hello-database-connection"></a>Konfigurera hello databasanslutning

I hello **Anslut** dialogrutan Expandera hello **Azure** nod. Alla SQL-databas instanser i Azure anges här.

Välj hello `DotNetAppSqlDb` SQL-databas. hello-anslutning som du skapade tidigare fylls automatiskt längst ned hello.

Ange hello databasadministratörens lösenord du skapade tidigare och klicka på **Anslut**.

![Konfigurera databasanslutningen från Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Tillåta klientanslutning från datorn

Hej **skapa en ny brandväggsregel** dialogrutan öppnas. Som standard kan bara anslutningar från Azure-tjänster, till exempel Azure-webbapp i SQL Database-instans. tooconnect tooyour databasen, skapa en brandväggsregel i hello SQL Database-instans. hello brandväggsregel tillåter hello offentliga IP-adressen för den lokala datorn.

hello dialogrutan är redan fylld med datorns offentliga IP-adressen.

Se till att **lägga till klientens IP-Adressen** är markerad och klicka på **OK**. 

![Ställa in brandväggen för SQL Database-instans](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

När Visual Studio har skapat hello brandväggsinställningen för din instans av SQL-databas, din anslutning visas i **SQL Server Object Explorer**.

Här kan du utföra hello vanligaste databasåtgärder, till exempel kör frågor, skapa vyer och lagrade procedurer och mycket mer. 

Högerklicka på hello `Todoes` tabell och välj **visa Data**. 

![Utforska SQL Database-objekt](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Uppdatera app med Code First Migrations

Du kan använda hello välbekanta verktyg i Visual Studio tooupdate din databas och webb-app i Azure. I det här steget använder Code First Migrations i Entity Framework toomake ett ändra tooyour databasschema och publicerar den tooAzure.

Läs mer om hur du använder Entity Framework Code First Migrations [komma igång med Entity Framework 6 Code First med MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Uppdatera datamodellen

Öppna _Models\Todo.cs_ i hello redigerare. Lägg till följande egenskapen toohello hello `ToDo` klass:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Kör Code First Migrations lokalt

Kör några kommandon toomake uppdateringar tooyour lokal databas. 

Från hello **verktyg** -menyn klickar du på **NuGet Package Manager** > **Pakethanterarkonsolen**.

Aktivera Code First Migrations i hello fönstret Package Manager-konsolen:

```PowerShell
Enable-Migrations
```

Lägg till en migrering:

```PowerShell
Add-Migration AddProperty
```

Uppdatera hello lokal databas:

```PowerShell
Update-Database
```

Typen `Ctrl+F5` toorun hello app. Testa hello redigera detaljer, och skapa länkar.

Om programmet hello läser in utan fel, har Code First Migrations slutförts. Dock hello sidan fortfarande ser ut samma eftersom applogiken inte är ännu använder den här nya egenskapen. 

### <a name="use-hello-new-property"></a>Använd hello ny egenskap

Göra några ändringar i din kod toouse hello `Done` egenskapen. För enkelhetens i den här självstudiekursen ska endast toochange hello `Index` och `Create` toosee hello egenskapen-vyer i åtgärden.

Öppna _Controllers\TodosController.cs_.

Hitta hello `Create()` metod och Lägg till `Done` toohello lista över egenskaper i hello `Bind` attribut. När du är klar din `Create()` Metodsignaturen ser ut som följande kod hello:

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

Öppna _Views\Todos\Create.cshtml_.

I hello Razor kod, bör du se en `<div class="form-group">` element som använder `model.Description`, och sedan en annan `<div class="form-group">` element som använder `model.CreatedDate`. Direkt efter dessa två element, lägga till en annan `<div class="form-group">` element som använder `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Öppna _Views\Todos\Index.cshtml_.

Sök efter hello tom `<th></th>` element. Lägg till hello följande Razor kod ovanför det här elementet:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Hitta hello `<td>` element som innehåller hello `Html.ActionLink()` hjälpmetoder. Lägg till hello följande Razor kod ovanför det här elementet:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Det är allt du behöver toosee hello ändringar i hello `Index` och `Create` vyer. 

Typen `Ctrl+F5` toorun hello app.

Du kan nu lägga till en att göra-objekt och kontrollera **klar**. Sedan ska den visas i din startsida som en slutförd artikel. Kom ihåg att hello `Edit` vyn inte visar hello `Done` fältet, eftersom du inte ändra hello `Edit` vyn.

### <a name="enable-code-first-migrations-in-azure"></a>Aktivera Code First Migrations i Azure

Nu när koden ändra fungerar, inklusive Databasmigrering publicera den tooyour Azure webbapp och uppdatera din SQL-databas med Code First Migrations för.

Precis innan, högerklicka på projektet och välj **publicera**.

Klicka på **inställningar** tooopen hello Publiceringsguiden.

![Öppna Publiceringsinställningar](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

Klicka på hello guiden **nästa**.

Kontrollera att den hello-anslutningssträngen för SQL-databasen fylls i **MyDatabaseContext (MyDbConnection)**. Du kan behöva tooselect hello **myToDoAppDb** databasen hello listrutan. 

Välj **köra Code First Migrations (körs vid programstart)**, klicka på **spara**.

![Aktivera Code First Migrations i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Publicera ändringarna

Nu när du har aktiverat Code First Migrations i ditt Azure webbapp publicera kodändringarna.

I hello publicera sida klickar du på **publicera**.

Försök att lägga till arbetsuppgifter igen och välj **klar**, och de visas i din startsida som en slutförd artikel.

![Azure-webbapp efter koden första migrering](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Alla befintliga arbetsuppgifter fortfarande visas. Befintliga data i SQL-databasen är inte förlorade när du publicerar ASP.NET-programmet. Dessutom Code First Migrations bara ändras hello dataschemat och lämnar företaget dina befintliga data.


## <a name="stream-application-logs"></a>Dataströmmen programloggar

Du kan strömma spårning av meddelanden direkt från din Azure web app tooVisual Studio.

Öppna _Controllers\TodosController.cs_.

Varje åtgärd som börjar med en `Trace.WriteLine()` metod. Den här koden läggs tooshow du hur tooadd trace meddelanden tooyour Azure webbapp.

### <a name="open-server-explorer"></a>Öppna Server Explorer

Från hello **visa** väljer du **Server Explorer**. Du kan konfigurera loggning för din Azure-webbapp i **Server Explorer**. 

### <a name="enable-log-streaming"></a>Strömning logg

I **Server Explorer**, expandera **Azure** > **Apptjänst**.

Expandera hello **myResourceGroup** resursgrupp, som du skapade när du först skapade hello Azure webbapp.

Högerklicka på ditt Azure webbapp och markera **visa strömning loggfiler**.

![Strömning logg](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

hello loggar nu strömmas till hello **utdata** fönster. 

![Loggen strömning i utdatafönstret](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Men ser du inte något av spårning hälsningsmeddelande ännu. Som eftersom när du väljer **visa strömning loggfiler**, Azure-webbapp anger hello Spårningsnivån för`Error`, som endast loggar felhändelser (med hello `Trace.TraceError()` metod).

### <a name="change-trace-levels"></a>Ändra spårningsnivåer

toochange hello trace nivåer toooutput andra trace meddelanden, gå tillbaka för**Server Explorer**.

Högerklicka på ditt Azure webbapp igen och välj **inställningar**.

I hello **programloggning (filsystem)** listrutan **utförlig**. Klicka på **Spara**.

![Ändra trace nivå tooVerbose](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Du kan experimentera med olika trace nivåer toosee vilka typer av meddelanden visas för varje nivå. Till exempel hello **Information** nivån innehåller alla loggar som skapats av `Trace.TraceInformation()`, `Trace.TraceWarning()`, och `Trace.TraceError()`, men inte loggar som skapats av `Trace.WriteLine()`.
>
>

Försök att klicka på runt hello att göra-App i Azure i webbläsaren. spåra hälsningsmeddelande strömmas nu toohello **utdata** fönstret i Visual Studio.

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Stoppa strömning logg

toostop hello log-streaming-tjänsten klickar du på hello **stoppa övervakningen av** knapp i hello **utdata** fönster.

![Stoppa strömning logg](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Hantera Azure-webbapp

Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade. 



Hello vänstra menyn klickar du på **Apptjänst**, klicka på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Du har landat i ditt webbprogram sidan. 

Som standard visar hello portal hello **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna. 

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en SQL-databas i Azure
> * Ansluta en ASP.NET-app tooSQL databas
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen loggas från Azure tooyour terminal
> * Hantera hello appen i hello Azure-portalen

I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn toohello webbprogram.

> [!div class="nextstepaction"]
> [Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md)
