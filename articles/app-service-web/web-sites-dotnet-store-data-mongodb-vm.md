---
title: "aaaCreate en webbapp i Azure som ansluter tooMongoDB som körs på en virtuell dator"
description: "En självstudiekurs som lär du dig hur toouse Git toodeploy tooAzure en ASP.NET-app App Service är anslutna tooMongoDB på en virtuell dator i Azure."
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
ms.openlocfilehash: 1f5f42c28c3c294d92c9ebf1499374931d47c010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a>Skapa en webbapp i Azure som ansluter tooMongoDB som körs på en virtuell dator
Du kan distribuera ett ASP.NET-program tooAzure App Service Web Apps med Git. I den här självstudiekursen skapar du en enkel frontend ASP.NET MVC uppgiften lista över program som ansluter tooa MongoDB-databas som körs på en virtuell dator i Azure.  [MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas. När du kör och testa hello ASP.NET-program på utvecklingsdatorn kan du kommer att överföra hello programmet tooApp Service Web Apps med Git.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="background-knowledge"></a>Bakgrund knowledge
Hello följande är användbart för den här kursen krävs dock inte:

* hello C#-drivrutin för MongoDB. Mer information om hur du utvecklar program C# mot MongoDB finns hello MongoDB [CSharp språk Center][MongoC#LangCenter]. 
* hello ASP .NET web application framework. Du kan lära dig om den på hello [ASP.net-webbplats][ASP.NET].
* hello ASP .NET MVC web application framework. Du kan lära dig om den på hello [webbplats för ASP.NET MVC][MVCWebSite].
* Azure. Du kan börja läsa [Azure][WindowsAzure].

## <a name="prerequisites"></a>Krav
* [Visual Studio Express 2013 för webben] [ VSEWeb] eller [Visual Studio 2013][VSUlt]
* [Azure SDK för .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* En aktiv Microsoft Azure-prenumeration

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Skapa en virtuell dator och installera MongoDB
Den här kursen förutsätter att du har skapat en virtuell dator i Azure. När du skapar hello virtuell dator måste tooinstall MongoDB på hello virtuell dator:

* toocreate Windows-dator och installera MongoDB, se [installera MongoDB på en virtuell dator som kör Windows Server i Azure][InstallMongoOnWindowsVM].

När du har skapat hello virtuell dator i Azure och installerat MongoDB, vara säker på att tooremember hello DNS-namn för hello virtuella datorn (”testlinuxvm.cloudapp.net”, till exempel) och hello externa porten för MongoDB som du angav i hello slutpunkt.  Du behöver den här informationen senare i självstudiekursen hello.

<a id="createapp"></a>

## <a name="create-hello-application"></a>Skapa hello program
I det här avsnittet ska du skapa ett ASP.NET-program som kallas ”uppgiftslistan” med hjälp av Visual Studio och utföra en inledande distribution tooAzure App Service Web Apps. Du kommer att köra hello programmet lokalt, men den ansluta tooyour virtuell dator på Azure och använda hello MongoDB-instansen som du skapade finns.

1. I Visual Studio klickar du på **nytt projekt**.
   
    ![Starta sidan nytt projekt][StartPageNewProject]
2. I hello **nytt projekt** i hello till vänster och välj fönstret **Visual C#**, och välj sedan **Web**. I mittfönstret hello väljer **ASP.NET-webbprogram**. Namnge projektet ”MyTaskListApp” hello längst ned i och klicka sedan på **OK**.
   
    ![Dialogrutan Nytt projekt][NewProjectMyTaskListApp]
3. I hello **nytt ASP.NET-projekt** dialogrutan **MVC**, och klicka sedan på **OK**.
   
    ![Välj MVC-mallen][VS2013SelectMVCTemplate]
4. Om du inte redan inloggad på Microsoft Azure, kommer du att ange toosign i. Följ hello prompter toosign till Azure.
5. När du har loggat in kan börja du konfigurera din App Service webbapp. Ange hello **Web App name**, **programtjänstplanen**, **resursgruppen**, och **Region**, klicka på **skapa**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. När hello projektet har skapats, vänta på att skapas i Azure App Service som anges i hello hello web app toobe **aktivitet för Azure App Service** fönster. Klicka på **publicera MyTaskListApp toothis Web App nu**.
7. Klicka på **Publicera**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    När ASP.NET-standardprogrammet publicerade tooAzure App Service Web Apps kan startas i hello webbläsare.

## <a name="install-hello-mongodb-c-driver"></a>Installera hello MongoDB C#-drivrutinen
MongoDB har stöd för klientsidan för C#-program via en drivrutin som du behöver tooinstall på utvecklingsdatorn lokala. hello C# drivrutinen är tillgängliga via NuGet.

tooinstall hello MongoDB C#-drivrutinen:

1. I **Solution Explorer**, högerklicka på hello **MyTaskListApp** projektet och välj **hantera NuGetPackages**.
   
    ![Hantera NuGet-paket][VS2013ManageNuGetPackages]
2. I hello **hantera NuGet-paket** hello vänster, klicka på **Online**. I hello **Sök Online** på hello höger, Skriv ”mongodb.driver”.  Klicka på **installera** tooinstall hello-drivrutinen.
   
    ![Sök efter MongoDB C# drivrutin][SearchforMongoDBCSharpDriver]
3. Klicka på **jag accepterar** tooaccept hello 10gen, Inc. licensvillkor.
4. Klicka på **Stäng** när hello-drivrutinen har installerats.
    ![MongoDB C# drivrutin installerad][MongoDBCsharpDriverInstalled]

Hej MongoDB C#-drivrutinen har installerats.  Referenser toohello **MongoDB.Bson**, **MongoDB.Driver**, och **MongoDB.Driver.Core** toohello projekt har lagts till bibliotek.

![MongoDB C# drivrutinen referenser][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Lägga till en modell
I **Solution Explorer**, högerklicka på hello *modeller* mapp och **Lägg till** en ny **klassen** och ger den namnet *TaskModel.cs* .  I *TaskModel.cs*, Ersätt hello befintlig kod med hello följande kod:

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

## <a name="add-hello-data-access-layer"></a>Lägg till hello dataåtkomstnivå
I **Solution Explorer**, högerklicka på hello *MyTaskListApp* projekt och **Lägg till** en **ny mapp** med namnet *DAL*.  Högerklicka på hello *DAL* mapp och **Lägg till** en ny **klassen**. Namnet hello klassen filen *Dal.cs*.  I *Dal.cs*, Ersätt hello befintlig kod med hello följande kod:

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

            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  hello database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";

            // Default constructor.        
            public Dal()
            {
            }

            // Gets all Task items from hello MongoDB server.        
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

            // Creates a Task and inserts it into hello collection in MongoDB.
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
Öppna hello *Controllers\HomeController.cs* filen i **Solution Explorer** och Ersätt hello befintlig kod med hello följande:

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

## <a name="set-up-hello-styles"></a>Ställ in hello-format
toochange hello rubrik överst hello på hello-sidan, öppna hello *Views\Shared\\_Layout.cshtml* filen i **Solution Explorer** och Ersätt ”namn” i hello navigeringsfält för sidhuvud med ”Mina aktivitet Visa en lista med program ”så att den ser ut så här:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

Öppna hello i ordning tooset hello uppgiftslista meny, *\Views\Home\Index.cshtml* filen och ersätter hello befintlig kod med hello följande kod:

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


tooadd hello möjlighet toocreate en ny uppgift, högerklicka på hello *Views\Home\\*  mapp och **Lägg till** en **visa**.  Namnge hello visa *skapa*. Ersätt hello kod med hello följande:

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

## <a name="set-hello-mongodb-connection-string"></a>Ange anslutningssträngen för hello MongoDB
I **Solution Explorer**öppnar hello *DAL/Dal.cs* fil. Hitta hello följande kodrad:

    private string connectionString = "mongodb://<vm-dns-name>";

Ersätt `<vm-dns-name>` med hello DNS-namnet på hello virtuell dator som kör MongoDB som du skapade i hello [skapa en virtuell dator och installera MongoDB] [ Create a virtual machine and install MongoDB] steg i den här kursen.  toofind hello DNS-namnet på den virtuella datorn, gå toohello Azure Portal, Välj **virtuella datorer**, och hitta **DNS-namnet**.

Om hello hello virtuella datorns DNS-namn är ”testlinuxvm.cloudapp.net” och MongoDB lyssnar på standardporten för hello 27017 hello raden för anslutningssträngen i koden kommer att se ut:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Om hello virtuell datorslutpunkt anger en annan extern port för MongoDB, kan du ange hello port i hello anslutningssträng:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

Mer information om MongoDB-anslutningssträngar finns [anslutningar][MongoConnectionStrings].

## <a name="test-hello-local-deployment"></a>Testa hello lokala distributionen
toorun programmet på utvecklingsdatorn, Välj **Start Debugging** från hello **felsöka** -menyn eller träffar **F5**. IIS Express startar och en webbläsare öppnas och startar hello programmets startsida.  Du kan lägga till en ny uppgift läggs toohello MongoDB-databas som körs på den virtuella datorn i Azure.

![Min lista-programmet för aktivitet][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a>Publicera tooAzure App Service Web Apps
I det här avsnittet kommer du publicera dina ändringar tooAzure App Service Web Apps.

1. I Solution Explorer högerklickar du på **MyTaskListApp** igen och klicka på **publicera**.
2. Klicka på **Publicera**.
   
    Du bör nu se ditt webbprogram som körs i Azure App Service och åt hello MongoDB-databas i Azure Virtual Machines.

## <a name="summary"></a>Sammanfattning
Du har nu har distribuerat din ASP.NET-program tooAzure App Service Web Apps. tooview hello-webbprogram:

1. Logga in på hello Azure-portalen.
2. Klicka på **Webbappar**. 
3. Välj ditt webbprogram i hello **Web Apps** lista.

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
[Create and run hello My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy hello ASP.NET application toohello web site using Git]: #deployapp
