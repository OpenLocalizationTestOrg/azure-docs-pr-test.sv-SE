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
# <a name="get-started-with-power-bi-embedded-sample"></a><span data-ttu-id="fda36-103">Komma igång med Power BI Embedded exemplet</span><span class="sxs-lookup"><span data-stu-id="fda36-103">Get started with Power BI Embedded sample</span></span>

<span data-ttu-id="fda36-104">Med **Microsoft Power BI Embedded**, kan du integrera Power BI-rapporter direkt i ditt webb- eller mobila program.</span><span class="sxs-lookup"><span data-stu-id="fda36-104">With **Microsoft Power BI Embedded**, you can integrate Power BI reports right into your web or mobile applications.</span></span> <span data-ttu-id="fda36-105">I den här artikeln lär toohello **Power BI Embedded** get igång exempel.</span><span class="sxs-lookup"><span data-stu-id="fda36-105">In this article, we'll introduce you toohello **Power BI Embedded** get started sample.</span></span>

<span data-ttu-id="fda36-106">Innan vi gå längre någon vill du förmodligen toosave hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="fda36-106">Before we go any further, you'll probably want toosave hello following resources.</span></span> <span data-ttu-id="fda36-107">De hjälper dig när du integrerar för Power BI-rapporter i hello sample-appen och egna appar.</span><span class="sxs-lookup"><span data-stu-id="fda36-107">They'll help you when integrating Power BI reports into hello sample app and your own apps too.</span></span>

* [<span data-ttu-id="fda36-108">Exempelwebbapp för arbetsytan</span><span class="sxs-lookup"><span data-stu-id="fda36-108">Sample workspace web app</span></span>](http://go.microsoft.com/fwlink/?LinkId=761493)
* [<span data-ttu-id="fda36-109">Power BI Embedded API-referens</span><span class="sxs-lookup"><span data-stu-id="fda36-109">Power BI Embedded API reference</span></span>](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* <span data-ttu-id="fda36-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (tillgänglig via NuGet)</span><span class="sxs-lookup"><span data-stu-id="fda36-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (available via NuGet)</span></span>
* [<span data-ttu-id="fda36-111">JavaScript-rapporten bäddas in exempel</span><span class="sxs-lookup"><span data-stu-id="fda36-111">JavaScript Report Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> <span data-ttu-id="fda36-112">Innan du kan konfigurera och kör hello Power BI Embedded Kom igång exempel måste toocreate minst en **Arbetsytesamling** i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fda36-112">Before you can configure and run hello Power BI Embedded get started sample, you need toocreate at least one **Workspace Collection** in your Azure subscription.</span></span> <span data-ttu-id="fda36-113">toolearn hur toocreate en **Arbetsytesamling** i hello Azure Portal finns [komma igång med Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fda36-113">toolearn how toocreate a **Workspace Collection** in hello Azure Portal see [Getting Started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="configure-hello-sample-app"></a><span data-ttu-id="fda36-114">Konfigurera hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="fda36-114">Configure hello sample app</span></span>

<span data-ttu-id="fda36-115">Låt oss gå igenom hur du konfigurerar din Visual Studio development miljö tooaccess hello komponenter nödvändiga toorun hello sample-appen.</span><span class="sxs-lookup"><span data-stu-id="fda36-115">Let's walk through setting up your Visual Studio development environment tooaccess hello  components needed toorun hello sample app.</span></span>

1. <span data-ttu-id="fda36-116">Hämta och packa upp hello [Power BI Embedded - integrera en rapport i en webbapp](http://go.microsoft.com/fwlink/?LinkId=761493) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="fda36-116">Download and unzip hello [Power BI Embedded - Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) sample on GitHub.</span></span>
2. <span data-ttu-id="fda36-117">Öppna **PowerBI embedded.sln** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fda36-117">Open **PowerBI-embedded.sln** in Visual Studio.</span></span> <span data-ttu-id="fda36-118">Du kan behöva tooexecute hello **uppdateringspaketet** i hello NuGET Package Manager-konsolen i ordning tooupdate hello paket som används i den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="fda36-118">You may need tooexecute hello **Update-Package** command in hello NuGET Package Manager Console in order tooupdate hello packages used in this solution.</span></span>
3. <span data-ttu-id="fda36-119">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="fda36-119">Build hello solution.</span></span>
4. <span data-ttu-id="fda36-120">Kör hello **ProvisionSample** konsolapp.</span><span class="sxs-lookup"><span data-stu-id="fda36-120">Run hello **ProvisionSample** console app.</span></span> <span data-ttu-id="fda36-121">I hello för konsolen exempelapp etablera en arbetsyta och importerar en PBIX-fil.</span><span class="sxs-lookup"><span data-stu-id="fda36-121">In hello sample console app, you provision a workspace and import a PBIX file.</span></span>
5. <span data-ttu-id="fda36-122">tooprovision en ny **arbetsytan**, Välj alternativ 1, **samling management**, och välj sedan alternativet 6, **etablera en ny arbetsyta**</span><span class="sxs-lookup"><span data-stu-id="fda36-122">tooprovision a new **Workspace**, select option 1, **Collection management**, and then select option 6, **Provision a new Workspace**</span></span>
6. <span data-ttu-id="fda36-123">tooimport en ny **rapporten**, Välj alternativ 2, **rapportera management**, och välj sedan alternativ 3, **importera PBIX Desktop-fil i en arbetsyta**.</span><span class="sxs-lookup"><span data-stu-id="fda36-123">tooimport a new **Report**, select option 2, **Report management**, and then select option 3, **Import PBIX Desktop file into a workspace**.</span></span>

7. <span data-ttu-id="fda36-124">Ange din **Arbetsytesamling** namn, och **åtkomstnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="fda36-124">Enter your **Workspace Collection** name, and **Access Key**.</span></span> <span data-ttu-id="fda36-125">Du kan hämta dessa i hello **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="fda36-125">You can get these in hello **Azure Portal**.</span></span> <span data-ttu-id="fda36-126">Mer om hur toolearn tooget din **åtkomstnyckeln**, finns [visa åtkomstnycklar för Power BI API](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) i komma igång med Microsoft Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="fda36-126">toolearn more about how tooget your **Access Key**, see [View Power BI API Access Keys](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in Get started with Microsoft Power BI Embedded.</span></span>

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. <span data-ttu-id="fda36-127">Kopiera och spara hello nyskapad **arbetsyte-ID** toouse senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fda36-127">Copy and save hello newly created **Workspace ID** toouse later in this article.</span></span> <span data-ttu-id="fda36-128">Efter hello **arbetsyte-ID** är skapat kan du hittar den hello **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="fda36-128">After hello **Workspace ID** is created, you can find it hello **Azure Portal**.</span></span>

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. <span data-ttu-id="fda36-129">tooimport en PBIX-filen till din **arbetsytan**, Välj alternativet **6. Importera PBIX Desktop-fil till en befintlig arbetsyta**.</span><span class="sxs-lookup"><span data-stu-id="fda36-129">tooimport a PBIX file into your **Workspace**, select option **6. Import PBIX Desktop file into an existing workspace**.</span></span> <span data-ttu-id="fda36-130">Om du inte har en PBIX-fil praktiska, kan du hämta hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="fda36-130">If you don't have a PBIX file handy, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>
10. <span data-ttu-id="fda36-131">Om du uppmanas ange ett eget namn för din **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="fda36-131">If prompted, enter a friendly name for your **Dataset**.</span></span>

<span data-ttu-id="fda36-132">Du bör se ett svar som:</span><span class="sxs-lookup"><span data-stu-id="fda36-132">You should see a response like:</span></span>

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> <span data-ttu-id="fda36-133">Om PBIX-fil innehåller alla anslutningar direkt fråga, kör du alternativet 7 tooupdate hello-anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="fda36-133">If your PBIX file contains any direct query connections, run option 7 tooupdate hello connection strings.</span></span>

<span data-ttu-id="fda36-134">Nu har du en PBIX för Power BI-rapport som importeras till din **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="fda36-134">At this point, you have a Power BI PBIX report imported into your **Workspace**.</span></span> <span data-ttu-id="fda36-135">Nu ska vi titta på hur toorun hello **Power BI Embedded** Kom igång exempelwebbapp.</span><span class="sxs-lookup"><span data-stu-id="fda36-135">Now, let's look at how toorun hello **Power BI Embedded** get started sample web app.</span></span>

## <a name="run-hello-sample-web-app"></a><span data-ttu-id="fda36-136">Kör hello exempelwebbapp</span><span class="sxs-lookup"><span data-stu-id="fda36-136">Run hello sample web app</span></span>
<span data-ttu-id="fda36-137">hello web app exempel är ett exempelprogram som återger rapporter som importeras till din **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="fda36-137">hello web app sample is a sample application that renders reports imported into your **Workspace**.</span></span> <span data-ttu-id="fda36-138">Här är hur tooconfigure hello web app exempel.</span><span class="sxs-lookup"><span data-stu-id="fda36-138">Here's how tooconfigure hello web app sample.</span></span>

1. <span data-ttu-id="fda36-139">I hello **PowerBI-inbäddade** Visual Studio-lösning höger Klicka på hello **EmbedSample** webbprogram och välj **Ställ in som Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="fda36-139">In hello **PowerBI-embedded** Visual Studio solution, right click hello **EmbedSample** web application, and choose **Set as StartUp project**.</span></span>
2. <span data-ttu-id="fda36-140">I **web.config**, i hello **EmbedSample** webbprogrammet, redigera hello **appSettings**: **AccessKey**,  **WorkspaceCollection** namn, och **WorkspaceId**.</span><span class="sxs-lookup"><span data-stu-id="fda36-140">In **web.config**, in hello **EmbedSample** web application, edit hello **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.</span></span>

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. <span data-ttu-id="fda36-141">Kör hello **EmbedSample** webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fda36-141">Run hello **EmbedSample** web application.</span></span>

<span data-ttu-id="fda36-142">När du kör hello **EmbedSample** webbprogrammet, hello vänstra navigeringsfönstret ska innehålla en **rapporter** menyn.</span><span class="sxs-lookup"><span data-stu-id="fda36-142">Once you run hello **EmbedSample** web application, hello left navigation panel should contain a **Reports** menu.</span></span> <span data-ttu-id="fda36-143">Expandera tooview hello rapport som du har importerat **rapporter**, och klicka på en rapport.</span><span class="sxs-lookup"><span data-stu-id="fda36-143">tooview hello report you imported, expand **Reports**, and click a report.</span></span> <span data-ttu-id="fda36-144">Om du har importerat hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello exempelwebbapp skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="fda36-144">If you imported hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

<span data-ttu-id="fda36-145">När du klickar på en rapport hello **EmbedSample** webbprogram bör se ut ungefär detta:</span><span class="sxs-lookup"><span data-stu-id="fda36-145">After you click a report, hello **EmbedSample** web application should look something this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a><span data-ttu-id="fda36-146">Utforska hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="fda36-146">Explore hello sample code</span></span>

<span data-ttu-id="fda36-147">Hej **Microsoft Power BI Embedded** exempel är en exempelwebbapp som visar hur toointegrate **Power BI** rapporter i din app.</span><span class="sxs-lookup"><span data-stu-id="fda36-147">hello **Microsoft Power BI Embedded** sample is an example web app that shows you how toointegrate **Power BI** reports into your app.</span></span> <span data-ttu-id="fda36-148">Den använder en Model-View-Controller (MVC) utforma mönster toodemonstrate bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="fda36-148">It uses a Model-View-Controller (MVC) design pattern toodemonstrate best practices.</span></span> <span data-ttu-id="fda36-149">Det här avsnittet beskrivs delar av hello exempelkod som du kan utforska inom hello **PowerBI-inbäddade** web app lösning.</span><span class="sxs-lookup"><span data-stu-id="fda36-149">This section highlights parts of hello sample code that you can explore within hello **PowerBI-embedded** web app solution.</span></span> <span data-ttu-id="fda36-150">hello Model-View-Controller (MVC) mönster avgränsar hello modellering av hello domän, hello presentation och hello åtgärder baserat på användarindata i tre separata klasser: modell, visa och kontroll.</span><span class="sxs-lookup"><span data-stu-id="fda36-150">hello Model-View-Controller (MVC) pattern separates hello modeling of hello domain, hello presentation, and hello actions based on user input into three separate classes: Model, View, and Control.</span></span> <span data-ttu-id="fda36-151">toolearn mer om MVC, se [Lär dig mer om ASP.NET](http://www.asp.net/mvc).</span><span class="sxs-lookup"><span data-stu-id="fda36-151">toolearn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).</span></span>

<span data-ttu-id="fda36-152">Hej **Microsoft Power BI Embedded** exempelkod avgränsas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="fda36-152">hello **Microsoft Power BI Embedded** sample code is separated as follows.</span></span> <span data-ttu-id="fda36-153">Varje avsnitt innehåller hello filnamnet i hello PowerBI embedded.sln lösning så att du lätt kan hitta hello koden i hello exemplet.</span><span class="sxs-lookup"><span data-stu-id="fda36-153">Each section includes hello file name in hello PowerBI-embedded.sln solution so that you can easily find hello code in hello sample.</span></span>

> [!NOTE]
> <span data-ttu-id="fda36-154">Det här avsnittet är en sammanfattning av hello exempelkod som visar hur hello koden har skrivits.</span><span class="sxs-lookup"><span data-stu-id="fda36-154">This section is a summary of hello sample code that shows how hello code was written.</span></span> <span data-ttu-id="fda36-155">tooview hello komplett exempel, Läs in hello PowerBI embedded.sln lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fda36-155">tooview hello complete sample, please load hello PowerBI-embedded.sln solution in Visual Studio.</span></span>

### <a name="model"></a><span data-ttu-id="fda36-156">Modellen</span><span class="sxs-lookup"><span data-stu-id="fda36-156">Model</span></span>

<span data-ttu-id="fda36-157">hello exemplet har en **ReportsViewModel** och **ReportViewModel**.</span><span class="sxs-lookup"><span data-stu-id="fda36-157">hello sample has a **ReportsViewModel** and **ReportViewModel**.</span></span>

<span data-ttu-id="fda36-158">**ReportsViewModel.cs**: representerar Power BI-rapporter.</span><span class="sxs-lookup"><span data-stu-id="fda36-158">**ReportsViewModel.cs**: Represents Power BI Reports.</span></span>

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

<span data-ttu-id="fda36-159">**ReportViewModel.cs**: representerar en Power BI-rapport.</span><span class="sxs-lookup"><span data-stu-id="fda36-159">**ReportViewModel.cs**: Represents a Power BI Report.</span></span>

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a><span data-ttu-id="fda36-160">Anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fda36-160">Connection string</span></span>

<span data-ttu-id="fda36-161">hello anslutningssträngen måste ha hello följande format:</span><span class="sxs-lookup"><span data-stu-id="fda36-161">hello connection string must be in hello following format:</span></span>

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

<span data-ttu-id="fda36-162">Med hjälp av vanliga attribut för servern och databasen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fda36-162">Using common server and database attributes will fail.</span></span> <span data-ttu-id="fda36-163">Till exempel: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span><span class="sxs-lookup"><span data-stu-id="fda36-163">For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span></span>

### <a name="view"></a><span data-ttu-id="fda36-164">Visa</span><span class="sxs-lookup"><span data-stu-id="fda36-164">View</span></span>

<span data-ttu-id="fda36-165">Hej **visa** hanterar hello visningen av Power BI **rapporter** och en Power BI **rapporten**.</span><span class="sxs-lookup"><span data-stu-id="fda36-165">hello **View** manages hello display of Power BI **Reports** and a Power BI **Report**.</span></span>

<span data-ttu-id="fda36-166">**Reports.cshtml**: iterera över **Model.Reports** toocreate en **ActionLink**.</span><span class="sxs-lookup"><span data-stu-id="fda36-166">**Reports.cshtml**: Iterate over **Model.Reports** toocreate an **ActionLink**.</span></span> <span data-ttu-id="fda36-167">Hej **ActionLink** består på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fda36-167">hello **ActionLink** is composed as follows:</span></span>

| <span data-ttu-id="fda36-168">En del</span><span class="sxs-lookup"><span data-stu-id="fda36-168">Part</span></span> | <span data-ttu-id="fda36-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fda36-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fda36-170">Rubrik</span><span class="sxs-lookup"><span data-stu-id="fda36-170">Title</span></span> |<span data-ttu-id="fda36-171">Namnet på hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="fda36-171">Name of hello Report.</span></span> |
| <span data-ttu-id="fda36-172">QueryString</span><span class="sxs-lookup"><span data-stu-id="fda36-172">QueryString</span></span> |<span data-ttu-id="fda36-173">En länk toohello rapport-ID.</span><span class="sxs-lookup"><span data-stu-id="fda36-173">A link toohello Report ID.</span></span> |

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

<span data-ttu-id="fda36-174">Report.cshtml: Ange hello **Model.AccessToken**, och hello Lambda-uttrycket för **PowerBIReportFor**.</span><span class="sxs-lookup"><span data-stu-id="fda36-174">Report.cshtml: Set hello **Model.AccessToken**, and hello Lambda expression for **PowerBIReportFor**.</span></span>

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a><span data-ttu-id="fda36-175">Domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="fda36-175">Controller</span></span>

<span data-ttu-id="fda36-176">**DashboardController.cs**: skapar ett PowerBIClient skicka en **apptoken**.</span><span class="sxs-lookup"><span data-stu-id="fda36-176">**DashboardController.cs**: Creates a PowerBIClient passing an **app token**.</span></span> <span data-ttu-id="fda36-177">En JSON-Webbtoken (JWT) genereras från hello **signeringsnyckeln** tooget hello **autentiseringsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="fda36-177">A JSON Web Token (JWT) is generated from hello **Signing Key** tooget hello **Credentials**.</span></span> <span data-ttu-id="fda36-178">Hej **autentiseringsuppgifter** är används toocreate en instans av **PowerBIClient**.</span><span class="sxs-lookup"><span data-stu-id="fda36-178">hello **Credentials** are used toocreate an instance of **PowerBIClient**.</span></span> <span data-ttu-id="fda36-179">När du har en instans av **PowerBIClient**, kan du anropa GetReports() och GetReportsAsync().</span><span class="sxs-lookup"><span data-stu-id="fda36-179">Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().</span></span>

<span data-ttu-id="fda36-180">CreatePowerBIClient()</span><span class="sxs-lookup"><span data-stu-id="fda36-180">CreatePowerBIClient()</span></span>

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

<span data-ttu-id="fda36-181">ActionResult Reports()</span><span class="sxs-lookup"><span data-stu-id="fda36-181">ActionResult Reports()</span></span>

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


<span data-ttu-id="fda36-182">Uppgiften<ActionResult> rapporten (sträng reportId)</span><span class="sxs-lookup"><span data-stu-id="fda36-182">Task<ActionResult> Report(string reportId)</span></span>

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

### <a name="integrate-a-report-into-your-app"></a><span data-ttu-id="fda36-183">Integrera en rapport i din app</span><span class="sxs-lookup"><span data-stu-id="fda36-183">Integrate a report into your app</span></span>

<span data-ttu-id="fda36-184">När du har en **rapporten**, du använder en **IFrame** tooembed hello Power BI **rapporten**.</span><span class="sxs-lookup"><span data-stu-id="fda36-184">Once you have a **Report**, you use an **IFrame** tooembed hello Power BI **Report**.</span></span> <span data-ttu-id="fda36-185">Här är ett kodstycke från powerbi.js i hello **Microsoft Power BI Embedded** exempel.</span><span class="sxs-lookup"><span data-stu-id="fda36-185">Here is a code snippet from  powerbi.js in hello **Microsoft Power BI Embedded** sample.</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a><span data-ttu-id="fda36-186">Filtrera rapporter som är inbäddad i ditt program</span><span class="sxs-lookup"><span data-stu-id="fda36-186">Filter reports embedded in your application</span></span>

<span data-ttu-id="fda36-187">Du kan filtrera en inbäddad rapport med en URL-syntax.</span><span class="sxs-lookup"><span data-stu-id="fda36-187">You can filter an embedded report using a URL syntax.</span></span> <span data-ttu-id="fda36-188">toodo kan du lägga till en **$filter** frågesträngparametern med en **eq** operatorn tooyour iFrame-src-url med hello filter har angetts.</span><span class="sxs-lookup"><span data-stu-id="fda36-188">toodo this, you add a **$filter** query string parameter with an **eq** operator tooyour iFrame src url with hello filter specified.</span></span> <span data-ttu-id="fda36-189">Här är hello filter frågesyntaxen:</span><span class="sxs-lookup"><span data-stu-id="fda36-189">Here is hello filter query syntax:</span></span>

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> <span data-ttu-id="fda36-190">{tableName/fieldName} får inte innehålla blanksteg eller specialtecken.</span><span class="sxs-lookup"><span data-stu-id="fda36-190">{tableName/fieldName} cannot include spaces or special characters.</span></span> <span data-ttu-id="fda36-191">hello {fieldValue} accepterar ett enskilt kategoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="fda36-191">hello {fieldValue} accepts a single categorical value.</span></span>  

## <a name="see-also"></a><span data-ttu-id="fda36-192">Se även</span><span class="sxs-lookup"><span data-stu-id="fda36-192">See also</span></span>

[<span data-ttu-id="fda36-193">Vanliga scenarier för Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fda36-193">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="fda36-194">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fda36-194">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="fda36-195">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="fda36-195">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="fda36-196">Skapa en ny rapport från en datauppsättning</span><span class="sxs-lookup"><span data-stu-id="fda36-196">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="fda36-197">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="fda36-197">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="fda36-198">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="fda36-198">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="fda36-199">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="fda36-199">More questions?</span></span> [<span data-ttu-id="fda36-200">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="fda36-200">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
