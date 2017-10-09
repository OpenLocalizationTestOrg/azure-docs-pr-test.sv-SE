---
title: "aaaGet igång med"
description: "Power BI Embedded använder SDK tooadd interaktiva Power BI-rapporter i business intelligence-appar"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/02/2017
ms.author: asaxton
ms.openlocfilehash: 1fef9dd8e0f734b748b930d3f85ad4b517d9661e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-power-bi-embedded-sample"></a>Komma igång med Power BI Embedded exemplet

Med **Microsoft Power BI Embedded**, kan du integrera Power BI-rapporter direkt i ditt webb- eller mobila program. I den här artikeln lär toohello **Power BI Embedded** get igång exempel.

Innan vi gå längre någon vill du förmodligen toosave hello följande resurser. De hjälper dig när du integrerar för Power BI-rapporter i hello sample-appen och egna appar.

* [Exempelwebbapp för arbetsytan](http://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI Embedded API-referens](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* [Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (tillgänglig via NuGet)
* [JavaScript-rapporten bäddas in exempel](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> Innan du kan konfigurera och kör hello Power BI Embedded Kom igång exempel måste toocreate minst en **Arbetsytesamling** i din Azure-prenumeration. toolearn hur toocreate en **Arbetsytesamling** i hello Azure Portal finns [komma igång med Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="configure-hello-sample-app"></a>Konfigurera hello sample-appen

Låt oss gå igenom hur du konfigurerar din Visual Studio development miljö tooaccess hello komponenter nödvändiga toorun hello sample-appen.

1. Hämta och packa upp hello [Power BI Embedded - integrera en rapport i en webbapp](http://go.microsoft.com/fwlink/?LinkId=761493) på GitHub.
2. Öppna **PowerBI embedded.sln** i Visual Studio. Du kan behöva tooexecute hello **uppdateringspaketet** i hello NuGET Package Manager-konsolen i ordning tooupdate hello paket som används i den här lösningen.
3. Skapa hello lösning.
4. Kör hello **ProvisionSample** konsolapp. I hello för konsolen exempelapp etablera en arbetsyta och importerar en PBIX-fil.
5. tooprovision en ny **arbetsytan**, Välj alternativ 1, **samling management**, och välj sedan alternativet 6, **etablera en ny arbetsyta**
6. tooimport en ny **rapporten**, Välj alternativ 2, **rapportera management**, och välj sedan alternativ 3, **importera PBIX Desktop-fil i en arbetsyta**.

7. Ange din **Arbetsytesamling** namn, och **åtkomstnyckeln**. Du kan hämta dessa i hello **Azure Portal**. Mer om hur toolearn tooget din **åtkomstnyckeln**, finns [visa åtkomstnycklar för Power BI API](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) i komma igång med Microsoft Power BI Embedded.

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. Kopiera och spara hello nyskapad **arbetsyte-ID** toouse senare i den här artikeln. Efter hello **arbetsyte-ID** är skapat kan du hittar den hello **Azure Portal**.

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. tooimport en PBIX-filen till din **arbetsytan**, Välj alternativet **6. Importera PBIX Desktop-fil till en befintlig arbetsyta**. Om du inte har en PBIX-fil praktiska, kan du hämta hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).
10. Om du uppmanas ange ett eget namn för din **Dataset**.

Du bör se ett svar som:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> Om PBIX-fil innehåller alla anslutningar direkt fråga, kör du alternativet 7 tooupdate hello-anslutningssträngar.

Nu har du en PBIX för Power BI-rapport som importeras till din **arbetsytan**. Nu ska vi titta på hur toorun hello **Power BI Embedded** Kom igång exempelwebbapp.

## <a name="run-hello-sample-web-app"></a>Kör hello exempelwebbapp
hello web app exempel är ett exempelprogram som återger rapporter som importeras till din **arbetsytan**. Här är hur tooconfigure hello web app exempel.

1. I hello **PowerBI-inbäddade** Visual Studio-lösning höger Klicka på hello **EmbedSample** webbprogram och välj **Ställ in som Startprojekt**.
2. I **web.config**, i hello **EmbedSample** webbprogrammet, redigera hello **appSettings**: **AccessKey**,  **WorkspaceCollection** namn, och **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Kör hello **EmbedSample** webbprogram.

När du kör hello **EmbedSample** webbprogrammet, hello vänstra navigeringsfönstret ska innehålla en **rapporter** menyn. Expandera tooview hello rapport som du har importerat **rapporter**, och klicka på en rapport. Om du har importerat hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello exempelwebbapp skulle se ut så här:

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

När du klickar på en rapport hello **EmbedSample** webbprogram bör se ut ungefär detta:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a>Utforska hello exempelkod

Hej **Microsoft Power BI Embedded** exempel är en exempelwebbapp som visar hur toointegrate **Power BI** rapporter i din app. Den använder en Model-View-Controller (MVC) utforma mönster toodemonstrate bästa praxis. Det här avsnittet beskrivs delar av hello exempelkod som du kan utforska inom hello **PowerBI-inbäddade** web app lösning. hello Model-View-Controller (MVC) mönster avgränsar hello modellering av hello domän, hello presentation och hello åtgärder baserat på användarindata i tre separata klasser: modell, visa och kontroll. toolearn mer om MVC, se [Lär dig mer om ASP.NET](http://www.asp.net/mvc).

Hej **Microsoft Power BI Embedded** exempelkod avgränsas på följande sätt. Varje avsnitt innehåller hello filnamnet i hello PowerBI embedded.sln lösning så att du lätt kan hitta hello koden i hello exemplet.

> [!NOTE]
> Det här avsnittet är en sammanfattning av hello exempelkod som visar hur hello koden har skrivits. tooview hello komplett exempel, Läs in hello PowerBI embedded.sln lösningen i Visual Studio.

### <a name="model"></a>Modellen

hello exemplet har en **ReportsViewModel** och **ReportViewModel**.

**ReportsViewModel.cs**: representerar Power BI-rapporter.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: representerar en Power BI-rapport.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a>Anslutningssträng

hello anslutningssträngen måste ha hello följande format:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Med hjälp av vanliga attribut för servern och databasen misslyckas. Till exempel: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Visa

Hej **visa** hanterar hello visningen av Power BI **rapporter** och en Power BI **rapporten**.

**Reports.cshtml**: iterera över **Model.Reports** toocreate en **ActionLink**. Hej **ActionLink** består på följande sätt:

| En del | Beskrivning |
| --- | --- |
| Rubrik |Namnet på hello rapporten. |
| QueryString |En länk toohello rapport-ID. |

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Ange hello **Model.AccessToken**, och hello Lambda-uttrycket för **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a>Domänkontrollant

**DashboardController.cs**: skapar ett PowerBIClient skicka en **apptoken**. En JSON-Webbtoken (JWT) genereras från hello **signeringsnyckeln** tooget hello **autentiseringsuppgifter**. Hej **autentiseringsuppgifter** är används toocreate en instans av **PowerBIClient**. När du har en instans av **PowerBIClient**, kan du anropa GetReports() och GetReportsAsync().

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Uppgiften<ActionResult> rapporten (sträng reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### <a name="integrate-a-report-into-your-app"></a>Integrera en rapport i din app

När du har en **rapporten**, du använder en **IFrame** tooembed hello Power BI **rapporten**. Här är ett kodstycke från powerbi.js i hello **Microsoft Power BI Embedded** exempel.

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a>Filtrera rapporter som är inbäddad i ditt program

Du kan filtrera en inbäddad rapport med en URL-syntax. toodo kan du lägga till en **$filter** frågesträngparametern med en **eq** operatorn tooyour iFrame-src-url med hello filter har angetts. Här är hello filter frågesyntaxen:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> {tableName/fieldName} får inte innehålla blanksteg eller specialtecken. hello {fieldValue} accepterar ett enskilt kategoriskt värde.  

## <a name="see-also"></a>Se även

[Vanliga scenarier för Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Skapa en ny rapport från en datauppsättning](power-bi-embedded-create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)
