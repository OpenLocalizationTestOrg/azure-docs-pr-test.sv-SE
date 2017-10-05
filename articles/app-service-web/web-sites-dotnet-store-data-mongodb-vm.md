---
title: "Skapa en webbapp i Azure som ansluter till MongoDB som körs på en virtuell dator"
description: "En självstudiekurs som lär du dig hur du använder Git för att distribuera en ASP.NET-app till Azure App Service som är anslutna till MongoDB på en virtuell dator i Azure."
tags: azure-portal
services: app-service\web, virtual-machines
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: adf7a472-ae00-45a8-aec4-06247e21318b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: cephalin
ms.openlocfilehash: a3f289ed9c764d0859573de4f834e042d0f103c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a>Skapa en webbapp i Azure som ansluter till MongoDB som körs på en virtuell dator
Du kan distribuera ett ASP.NET-program till Azure App Service Web Apps med Git. I den här självstudiekursen skapar du en enkel frontend ASP.NET MVC uppgiften lista över program som ansluter till en MongoDB-databas som körs på en virtuell dator i Azure.  [MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas. När du kör och testa ASP.NET-programmet på utvecklingsdatorn kan du ladda upp programmet till App Service Web Apps med Git.

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="background-knowledge"></a>Bakgrund knowledge
Följande är användbart för den här kursen krävs dock inte:

* C# drivrutiner för MongoDB. Mer information om hur du utvecklar program C# mot MongoDB finns i MongoDB [CSharp språk Center][MongoC#LangCenter]. 
* ASP .NET web application framework. Du kan lära dig om den på den [ASP.net-webbplats][ASP.NET].
* ASP .NET MVC web application framework. Du kan lära dig om den på den [webbplats för ASP.NET MVC][MVCWebSite].
* Azure. Du kan börja läsa [Azure][WindowsAzure].

## <a name="prerequisites"></a>Krav
* [Visual Studio Express 2013 för webben] [ VSEWeb] eller [Visual Studio 2013][VSUlt]
* [Azure SDK för .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* En aktiv Microsoft Azure-prenumeration

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Skapa en virtuell dator och installera MongoDB
Den här kursen förutsätter att du har skapat en virtuell dator i Azure. När du har skapat den virtuella datorn måste du installera MongoDB på den virtuella datorn:

* Om du vill skapa en Windows-dator och installera MongoDB, se [installera MongoDB på en virtuell dator som kör Windows Server i Azure][InstallMongoOnWindowsVM].

När du har skapat den virtuella datorn i Azure och installerat MongoDB, bör du komma ihåg DNS-namnet på den virtuella datorn (”testlinuxvm.cloudapp.net”, till exempel) och den externa porten för MongoDB som du angav i slutpunkten.  Du behöver den här informationen senare under kursen.

<a id="createapp"></a>

## <a name="create-the-application"></a>Skapa programmet
I det här avsnittet ska du skapa ett ASP.NET-program som kallas ”uppgiftslistan” med hjälp av Visual Studio och utföra en inledande distribution till Azure App Service Web Apps. Du kan köra programmet lokalt, men den ansluta till den virtuella datorn i Azure och använda MongoDB-instansen som du skapade finns.

1. I Visual Studio klickar du på **nytt projekt**.
   
    ![Starta sidan nytt projekt][StartPageNewProject]
2. I den **nytt projekt** i den vänstra rutan, Välj fönstret **Visual C#**, och välj sedan **Web**. I den mellersta rutan väljer **ASP.NET-webbprogram**. Namnge ditt projekt ”MyTaskListApp” längst ned, och klicka sedan på **OK**.
   
    ![Dialogrutan Nytt projekt][NewProjectMyTaskListApp]
3. I den **nytt ASP.NET-projekt** dialogrutan **MVC**, och klicka sedan på **OK**.
   
    ![Välj MVC-mallen][VS2013SelectMVCTemplate]
4. Om du inte redan loggat in Microsoft Azure, uppmanas du att logga in. Följ anvisningarna för att logga in på Azure.
5. När du har loggat in kan börja du konfigurera din App Service webbapp. Ange den **Web App name**, **programtjänstplanen**, **resursgruppen**, och **Region**, klicka på **skapa**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. När projektet har skapats är klar, vänta tills webbappen skapas i Azure App Service som anges i den **aktivitet för Azure App Service** fönster. Klicka på **publicera MyTaskListApp till det här webbprogrammet nu**.
7. Klicka på **Publicera**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    När standard ASP.NET-programmet har publicerats till Azure App Service Web Apps kommer att startas i webbläsaren.

## <a name="install-the-mongodb-c-driver"></a>Installera MongoDB C#-drivrutin
MongoDB har stöd för klientsidan för C#-program via en drivrutin som du behöver installera på utvecklingsdatorn lokala. C#-drivrutinen är tillgängliga via NuGet.

Installera MongoDB C#-drivrutinen:

1. I **Solution Explorer**, högerklicka på den **MyTaskListApp** projektet och välj **hantera NuGetPackages**.
   
    ![Hantera NuGet-paket][VS2013ManageNuGetPackages]
2. I den **hantera NuGet-paket** i den vänstra rutan, klicka på **Online**. I den **Sök Online** till höger, Skriv ”mongodb.driver”.  Klicka på **installera** att installera drivrutinen.
   
    ![Sök efter MongoDB C# drivrutin][SearchforMongoDBCSharpDriver]
3. Klicka på **jag accepterar** att acceptera 10gen, Inc. licensvillkoren.
4. Klicka på **Stäng** när drivrutinen har installerats.
    ![MongoDB C# drivrutin installerad][MongoDBCsharpDriverInstalled]

MongoDB C#-drivrutinen har installerats.  Referenser till den **MongoDB.Bson**, **MongoDB.Driver**, och **MongoDB.Driver.Core** bibliotek har lagts till i projektet.

![MongoDB C# drivrutinen referenser][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Lägga till en modell
I **Solution Explorer**, högerklicka på den *modeller* mapp och **Lägg till** en ny **klassen** och ger den namnet *TaskModel.cs*.  I *TaskModel.cs*, Ersätt den befintliga koden med följande kod:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MongoDB.Bson.Serialization.Attributes;
    using MongoDB.Bson.Serialization.IdGenerators;
    using MongoDB.Bson;

    namespace MyTaskListApp.Models
    {
        public class MyTask
        {
            [BsonId(IdGenerator = typeof(CombGuidGenerator))]
            public Guid Id { get; set; }

            [BsonElement("Name")]
            public string Name { get; set; }

            [BsonElement("Category")]
            public string Category { get; set; }

            [BsonElement("Date")]
            public DateTime Date { get; set; }

            [BsonElement("CreatedDate")]
            public DateTime CreatedDate { get; set; }

        }
    }

## <a name="add-the-data-access-layer"></a>Lägg till data access-lager
I **Solution Explorer**, högerklicka på den *MyTaskListApp* projekt och **Lägg till** en **ny mapp** med namnet *DAL*.  Högerklicka på den *DAL* mapp och **Lägg till** en ny **klassen**. Namn på filen för klassen *Dal.cs*.  I *Dal.cs*, Ersätt den befintliga koden med följande kod:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;


    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            private MongoServer mongoServer = null;
            private bool disposed = false;

            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";

            // Default constructor.        
            public Dal()
            {
            }

            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }

            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }

            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }

            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }

            # region IDisposable

            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }

            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        if (mongoServer != null)
                        {
                            this.mongoServer.Disconnect();
                        }
                    }
                }

                this.disposed = true;
            }

            # endregion
        }
    }

## <a name="add-a-controller"></a>Lägga till en styrenhet
Öppna den *Controllers\HomeController.cs* filen i **Solution Explorer** och Ersätt den befintliga koden med följande:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using MyTaskListApp.Models;
    using System.Configuration;

    namespace MyTaskListApp.Controllers
    {
        public class HomeController : Controller, IDisposable
        {
            private Dal dal = new Dal();
            private bool disposed = false;
            //
            // GET: /MyTask/

            public ActionResult Index()
            {
                return View(dal.GetAllTasks());
            }

            //
            // GET: /MyTask/Create

            public ActionResult Create()
            {
                return View();
            }

            //
            // POST: /MyTask/Create

            [HttpPost]
            public ActionResult Create(MyTask task)
            {
                try
                {
                    dal.CreateTask(task);
                    return RedirectToAction("Index");
                }
                catch
                {
                    return View();
                }
            }

            public ActionResult About()
            {
                return View();
            }

            # region IDisposable

            new protected void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }

            new protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        this.dal.Dispose();
                    }
                }

                this.disposed = true;
            }

            # endregion

        }
    }

## <a name="set-up-the-styles"></a>Ställ in formatmallar
Om du vill ändra rubriken överst på sidan, öppna den *Views\Shared\\_Layout.cshtml* filen i **Solution Explorer** och Ersätt ”namn” i rubriken navigeringsfält för med ”uppgiftslistan Programmet ”så att den ser ut så här:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

För att ställa in uppgiftslista-menyn, öppna den *\Views\Home\Index.cshtml* filen och ersätter den befintliga koden med följande kod:

    @model IEnumerable<MyTaskListApp.Models.MyTask>

    @{
        ViewBag.Title = "My Task List";
    }

    <h2>My Task List</h2>

    <table border="1">
        <tr>
            <th>Task</th>
            <th>Category</th>
            <th>Date</th>

        </tr>

    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Category)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Date)
            </td>

        </tr>
    }

    </table>
    <div>  @Html.Partial("Create", new MyTaskListApp.Models.MyTask())</div>


Om du vill lägga till möjligheten att skapa en ny uppgift, högerklicka på den *Views\Home\\*  mapp och **Lägg till** en **visa**.  Namnge vyn *skapa*. Ersätt Koden med följande:

    @model MyTaskListApp.Models.MyTask

    <script src="@Url.Content("~/Scripts/jquery-1.10.2.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.unobtrusive.min.js")" type="text/javascript"></script>

    @using (Html.BeginForm("Create", "Home")) {
        @Html.ValidationSummary(true)
        <fieldset>
            <legend>New Task</legend>

            <div class="editor-label">
                @Html.LabelFor(model => model.Name)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Name)
                @Html.ValidationMessageFor(model => model.Name)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Category)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Category)
                @Html.ValidationMessageFor(model => model.Category)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Date)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Date)
                @Html.ValidationMessageFor(model => model.Date)
            </div>

            <p>
                <input type="submit" value="Create" />
            </p>
        </fieldset>
    }

**Solution Explorer** bör se ut så här:

![Solution Explorer][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a>Ange anslutningssträngen för MongoDB
I **Solution Explorer**öppnar den *DAL/Dal.cs* fil. Sök efter följande kodrad:

    private string connectionString = "mongodb://<vm-dns-name>";

Ersätt `<vm-dns-name>` med DNS-namnet på den virtuella datorn körs MongoDB som du skapade i den [skapa en virtuell dator och installera MongoDB] [ Create a virtual machine and install MongoDB] steg i den här kursen.  DNS-namnet på den virtuella datorn finns i Azure Portal, Välj **virtuella datorer**, och hitta **DNS-namnet**.

Om DNS-namnet på den virtuella datorn är ”testlinuxvm.cloudapp.net” och MongoDB lyssnar på standardporten 27017 raden för anslutningssträngen i koden kommer att se ut:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Om den virtuella datorslutpunkten anger en annan extern port för MongoDB, kan du ange porten i anslutningssträngen:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

Mer information om MongoDB-anslutningssträngar finns [anslutningar][MongoConnectionStrings].

## <a name="test-the-local-deployment"></a>Testa lokala distributionen
Om du vill köra programmet på utvecklingsdatorn **Start Debugging** från den **felsöka** -menyn eller träffar **F5**. IIS Express startar och en webbläsare öppnas och startar programmets startsida.  Du kan lägga till en ny uppgift som ska läggas till MongoDB-databas som körs på den virtuella datorn i Azure.

![Min lista-programmet för aktivitet][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a>Publicera till Azure App Service Web Apps
I det här avsnittet kommer du publicerar ändringarna till Azure App Service Web Apps.

1. I Solution Explorer högerklickar du på **MyTaskListApp** igen och klicka på **publicera**.
2. Klicka på **Publicera**.
   
    Du bör nu se ditt webbprogram som körs i Azure App Service och åtkomst till MongoDB-databas i Azure Virtual Machines.

## <a name="summary"></a>Sammanfattning
Du har nu distribuerats ASP.NET-programmet till Azure App Service Web Apps. Visa webbprogrammet:

1. Logga in på Azure-portalen.
2. Klicka på **Webbappar**. 
3. Välj ditt webbprogram i den **Web Apps** lista.

Läs mer om hur du utvecklar program C# mot MongoDB [CSharp språk Center][MongoC#LangCenter]. 

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- HYPERLINKS -->

[AzurePortal]: http://manage.windowsazure.com
[WindowsAzure]: http://www.windowsazure.com
[MongoC#LangCenter]: http://docs.mongodb.org/ecosystem/drivers/csharp/
[MVCWebSite]: http://www.asp.net/mvc
[ASP.NET]: http://www.asp.net/
[MongoConnectionStrings]: http://www.mongodb.org/display/DOCS/Connections
[MongoDB]: http://www.mongodb.org
[InstallMongoOnWindowsVM]:../virtual-machines/windows/classic/install-mongodb.md
[VSEWeb]: http://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express
[VSUlt]: http://www.microsoft.com/visualstudio/eng/2013-downloads

<!-- IMAGES -->


[StartPageNewProject]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProject.png
[NewProjectMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProjectMyTaskListApp.png
[VS2013SelectMVCTemplate]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013SelectMVCTemplate.png
[VS2013DefaultMVCApplication]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013DefaultMVCApplication.png
[VS2013ManageNuGetPackages]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013ManageNuGetPackages.png
[SearchforMongoDBCSharpDriver]: ./media/web-sites-dotnet-store-data-mongodb-vm/SearchforMongoDBCSharpDriver.png
[MongoDBCsharpDriverInstalled]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCsharpDriverInstalled.png
[MongoDBCSharpDriverReferences]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCSharpDriverReferences.png
[SolutionExplorerMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/SolutionExplorerMyTaskListApp.png
[TaskListAppBlank]: ./media/web-sites-dotnet-store-data-mongodb-vm/TaskListAppBlank.png
[WAWSCreateWebSite]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSCreateWebSite.png
[WAWSDashboardMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSDashboardMyTaskListApp.png
[Image9]: ./media/web-sites-dotnet-store-data-mongodb-vm/RepoReady.png
[Image10]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitInstructions.png
[Image11]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitDeploymentComplete.png

<!-- TOC BOOKMARKS -->
[Create a virtual machine and install MongoDB]: #virtualmachine
[Create and run the My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy the ASP.NET application to the web site using Git]: #deployapp
