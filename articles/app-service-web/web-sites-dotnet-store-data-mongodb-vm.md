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
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a><span data-ttu-id="d68fe-103">Skapa en webbapp i Azure som ansluter till MongoDB som körs på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d68fe-103">Create a web app in Azure that connects to MongoDB running on a virtual machine</span></span>
<span data-ttu-id="d68fe-104">Du kan distribuera ett ASP.NET-program till Azure App Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="d68fe-104">Using Git, you can deploy an ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="d68fe-105">I den här självstudiekursen skapar du en enkel frontend ASP.NET MVC uppgiften lista över program som ansluter till en MongoDB-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d68fe-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects to a MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="d68fe-106">[MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d68fe-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="d68fe-107">När du kör och testa ASP.NET-programmet på utvecklingsdatorn kan du ladda upp programmet till App Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="d68fe-107">After running and testing the ASP.NET application on your development computer, you will upload the application to App Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="d68fe-108">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="d68fe-108">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d68fe-109">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d68fe-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="d68fe-110">Bakgrund knowledge</span><span class="sxs-lookup"><span data-stu-id="d68fe-110">Background knowledge</span></span>
<span data-ttu-id="d68fe-111">Följande är användbart för den här kursen krävs dock inte:</span><span class="sxs-lookup"><span data-stu-id="d68fe-111">Knowledge of the following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="d68fe-112">C# drivrutiner för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d68fe-112">The C# driver for MongoDB.</span></span> <span data-ttu-id="d68fe-113">Mer information om hur du utvecklar program C# mot MongoDB finns i MongoDB [CSharp språk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="d68fe-113">For more information on developing C# applications against MongoDB, see the MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="d68fe-114">ASP .NET web application framework.</span><span class="sxs-lookup"><span data-stu-id="d68fe-114">The ASP .NET web application framework.</span></span> <span data-ttu-id="d68fe-115">Du kan lära dig om den på den [ASP.net-webbplats][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="d68fe-115">You can learn all about it at the [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="d68fe-116">ASP .NET MVC web application framework.</span><span class="sxs-lookup"><span data-stu-id="d68fe-116">The ASP .NET MVC web application framework.</span></span> <span data-ttu-id="d68fe-117">Du kan lära dig om den på den [webbplats för ASP.NET MVC][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="d68fe-117">You can learn all about it at the [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="d68fe-118">Azure.</span><span class="sxs-lookup"><span data-stu-id="d68fe-118">Azure.</span></span> <span data-ttu-id="d68fe-119">Du kan börja läsa [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="d68fe-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d68fe-120">Krav</span><span class="sxs-lookup"><span data-stu-id="d68fe-120">Prerequisites</span></span>
* <span data-ttu-id="d68fe-121">[Visual Studio Express 2013 för webben] [ VSEWeb] eller [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="d68fe-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="d68fe-122">Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="d68fe-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="d68fe-123">En aktiv Microsoft Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d68fe-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="d68fe-124">Skapa en virtuell dator och installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="d68fe-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="d68fe-125">Den här kursen förutsätter att du har skapat en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d68fe-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="d68fe-126">När du har skapat den virtuella datorn måste du installera MongoDB på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="d68fe-126">After creating the virtual machine you need to install MongoDB on the virtual machine:</span></span>

* <span data-ttu-id="d68fe-127">Om du vill skapa en Windows-dator och installera MongoDB, se [installera MongoDB på en virtuell dator som kör Windows Server i Azure][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="d68fe-127">To create a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="d68fe-128">När du har skapat den virtuella datorn i Azure och installerat MongoDB, bör du komma ihåg DNS-namnet på den virtuella datorn (”testlinuxvm.cloudapp.net”, till exempel) och den externa porten för MongoDB som du angav i slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d68fe-128">After you have created the virtual machine in Azure and installed MongoDB, be sure to remember the DNS name of the virtual machine ("testlinuxvm.cloudapp.net", for example) and the external port for MongoDB that you specified in the endpoint.</span></span>  <span data-ttu-id="d68fe-129">Du behöver den här informationen senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="d68fe-129">You will need this information later in the tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-the-application"></a><span data-ttu-id="d68fe-130">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="d68fe-130">Create the application</span></span>
<span data-ttu-id="d68fe-131">I det här avsnittet ska du skapa ett ASP.NET-program som kallas ”uppgiftslistan” med hjälp av Visual Studio och utföra en inledande distribution till Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d68fe-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment to Azure App Service Web Apps.</span></span> <span data-ttu-id="d68fe-132">Du kan köra programmet lokalt, men den ansluta till den virtuella datorn i Azure och använda MongoDB-instansen som du skapade finns.</span><span class="sxs-lookup"><span data-stu-id="d68fe-132">You will run the application locally, but it will connect to your virtual machine on Azure and use the MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="d68fe-133">I Visual Studio klickar du på **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Starta sidan nytt projekt][StartPageNewProject]
2. <span data-ttu-id="d68fe-135">I den **nytt projekt** i den vänstra rutan, Välj fönstret **Visual C#**, och välj sedan **Web**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-135">In the **New Project** window, in the left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="d68fe-136">I den mellersta rutan väljer **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-136">In the middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="d68fe-137">Namnge ditt projekt ”MyTaskListApp” längst ned, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-137">At the bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Dialogrutan Nytt projekt][NewProjectMyTaskListApp]
3. <span data-ttu-id="d68fe-139">I den **nytt ASP.NET-projekt** dialogrutan **MVC**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-139">In the **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![Välj MVC-mallen][VS2013SelectMVCTemplate]
4. <span data-ttu-id="d68fe-141">Om du inte redan loggat in Microsoft Azure, uppmanas du att logga in.</span><span class="sxs-lookup"><span data-stu-id="d68fe-141">If you aren't already signed into Microsoft Azure, you will be prompted to sign in.</span></span> <span data-ttu-id="d68fe-142">Följ anvisningarna för att logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="d68fe-142">Follow the prompts to sign into Azure.</span></span>
5. <span data-ttu-id="d68fe-143">När du har loggat in kan börja du konfigurera din App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="d68fe-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="d68fe-144">Ange den **Web App name**, **programtjänstplanen**, **resursgruppen**, och **Region**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-144">Specify the **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="d68fe-145">När projektet har skapats är klar, vänta tills webbappen skapas i Azure App Service som anges i den **aktivitet för Azure App Service** fönster.</span><span class="sxs-lookup"><span data-stu-id="d68fe-145">After the project creation completes, wait for the web app to be created in Azure App Service as indicated in the **Azure App Service Activity** window.</span></span> <span data-ttu-id="d68fe-146">Klicka på **publicera MyTaskListApp till det här webbprogrammet nu**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-146">Then, click **Publish MyTaskListApp to this Web App now**.</span></span>
7. <span data-ttu-id="d68fe-147">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="d68fe-148">När standard ASP.NET-programmet har publicerats till Azure App Service Web Apps kommer att startas i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d68fe-148">Once your default ASP.NET application is published to Azure App Service Web Apps, it will be launched in the browser.</span></span>

## <a name="install-the-mongodb-c-driver"></a><span data-ttu-id="d68fe-149">Installera MongoDB C#-drivrutin</span><span class="sxs-lookup"><span data-stu-id="d68fe-149">Install the MongoDB C# driver</span></span>
<span data-ttu-id="d68fe-150">MongoDB har stöd för klientsidan för C#-program via en drivrutin som du behöver installera på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="d68fe-150">MongoDB offers client-side support for C# applications through a driver, which you need to install on your local development computer.</span></span> <span data-ttu-id="d68fe-151">C#-drivrutinen är tillgängliga via NuGet.</span><span class="sxs-lookup"><span data-stu-id="d68fe-151">The C# driver is available through NuGet.</span></span>

<span data-ttu-id="d68fe-152">Installera MongoDB C#-drivrutinen:</span><span class="sxs-lookup"><span data-stu-id="d68fe-152">To install the MongoDB C# driver:</span></span>

1. <span data-ttu-id="d68fe-153">I **Solution Explorer**, högerklicka på den **MyTaskListApp** projektet och välj **hantera NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-153">In **Solution Explorer**, right-click the **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![Hantera NuGet-paket][VS2013ManageNuGetPackages]
2. <span data-ttu-id="d68fe-155">I den **hantera NuGet-paket** i den vänstra rutan, klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-155">In the **Manage NuGet Packages** window, in the left pane, click **Online**.</span></span> <span data-ttu-id="d68fe-156">I den **Sök Online** till höger, Skriv ”mongodb.driver”.</span><span class="sxs-lookup"><span data-stu-id="d68fe-156">In the **Search Online** box on the right, type "mongodb.driver".</span></span>  <span data-ttu-id="d68fe-157">Klicka på **installera** att installera drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="d68fe-157">Click **Install** to install the driver.</span></span>
   
    ![Sök efter MongoDB C# drivrutin][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="d68fe-159">Klicka på **jag accepterar** att acceptera 10gen, Inc. licensvillkoren.</span><span class="sxs-lookup"><span data-stu-id="d68fe-159">Click **I Accept** to accept the 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="d68fe-160">Klicka på **Stäng** när drivrutinen har installerats.</span><span class="sxs-lookup"><span data-stu-id="d68fe-160">Click **Close** after the driver has installed.</span></span>
    <span data-ttu-id="d68fe-161">![MongoDB C# drivrutin installerad][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="d68fe-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="d68fe-162">MongoDB C#-drivrutinen har installerats.</span><span class="sxs-lookup"><span data-stu-id="d68fe-162">The MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="d68fe-163">Referenser till den **MongoDB.Bson**, **MongoDB.Driver**, och **MongoDB.Driver.Core** bibliotek har lagts till i projektet.</span><span class="sxs-lookup"><span data-stu-id="d68fe-163">References to the **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added to the project.</span></span>

![MongoDB C# drivrutinen referenser][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="d68fe-165">Lägga till en modell</span><span class="sxs-lookup"><span data-stu-id="d68fe-165">Add a model</span></span>
<span data-ttu-id="d68fe-166">I **Solution Explorer**, högerklicka på den *modeller* mapp och **Lägg till** en ny **klassen** och ger den namnet *TaskModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="d68fe-166">In **Solution Explorer**, right-click the *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="d68fe-167">I *TaskModel.cs*, Ersätt den befintliga koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d68fe-167">In *TaskModel.cs*, replace the existing code with the following code:</span></span>

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

## <a name="add-the-data-access-layer"></a><span data-ttu-id="d68fe-168">Lägg till data access-lager</span><span class="sxs-lookup"><span data-stu-id="d68fe-168">Add the data access layer</span></span>
<span data-ttu-id="d68fe-169">I **Solution Explorer**, högerklicka på den *MyTaskListApp* projekt och **Lägg till** en **ny mapp** med namnet *DAL*.</span><span class="sxs-lookup"><span data-stu-id="d68fe-169">In **Solution Explorer**, right-click the *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="d68fe-170">Högerklicka på den *DAL* mapp och **Lägg till** en ny **klassen**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-170">Right-click the *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="d68fe-171">Namn på filen för klassen *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="d68fe-171">Name the class file *Dal.cs*.</span></span>  <span data-ttu-id="d68fe-172">I *Dal.cs*, Ersätt den befintliga koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d68fe-172">In *Dal.cs*, replace the existing code with the following code:</span></span>

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

## <a name="add-a-controller"></a><span data-ttu-id="d68fe-173">Lägga till en styrenhet</span><span class="sxs-lookup"><span data-stu-id="d68fe-173">Add a controller</span></span>
<span data-ttu-id="d68fe-174">Öppna den *Controllers\HomeController.cs* filen i **Solution Explorer** och Ersätt den befintliga koden med följande:</span><span class="sxs-lookup"><span data-stu-id="d68fe-174">Open the *Controllers\HomeController.cs* file in **Solution Explorer** and replace the existing code with the following:</span></span>

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

## <a name="set-up-the-styles"></a><span data-ttu-id="d68fe-175">Ställ in formatmallar</span><span class="sxs-lookup"><span data-stu-id="d68fe-175">Set up the styles</span></span>
<span data-ttu-id="d68fe-176">Om du vill ändra rubriken överst på sidan, öppna den *Views\Shared\\_Layout.cshtml* filen i **Solution Explorer** och Ersätt ”namn” i rubriken navigeringsfält för med ”uppgiftslistan Programmet ”så att den ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d68fe-176">To change the title at the top of the page, open the *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in the navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="d68fe-177">För att ställa in uppgiftslista-menyn, öppna den *\Views\Home\Index.cshtml* filen och ersätter den befintliga koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="d68fe-177">In order to set up the Task List menu, open the *\Views\Home\Index.cshtml* file and replace the existing code with the following code:</span></span>

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


<span data-ttu-id="d68fe-178">Om du vill lägga till möjligheten att skapa en ny uppgift, högerklicka på den *Views\Home\\*  mapp och **Lägg till** en **visa**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-178">To add the ability to create a new task, right-click the *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="d68fe-179">Namnge vyn *skapa*.</span><span class="sxs-lookup"><span data-stu-id="d68fe-179">Name the view *Create*.</span></span> <span data-ttu-id="d68fe-180">Ersätt Koden med följande:</span><span class="sxs-lookup"><span data-stu-id="d68fe-180">Replace the code with the following:</span></span>

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

<span data-ttu-id="d68fe-181">**Solution Explorer** bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="d68fe-181">**Solution Explorer** should look like this:</span></span>

![Solution Explorer][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a><span data-ttu-id="d68fe-183">Ange anslutningssträngen för MongoDB</span><span class="sxs-lookup"><span data-stu-id="d68fe-183">Set the MongoDB connection string</span></span>
<span data-ttu-id="d68fe-184">I **Solution Explorer**öppnar den *DAL/Dal.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="d68fe-184">In **Solution Explorer**, open the *DAL/Dal.cs* file.</span></span> <span data-ttu-id="d68fe-185">Sök efter följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="d68fe-185">Find the following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="d68fe-186">Ersätt `<vm-dns-name>` med DNS-namnet på den virtuella datorn körs MongoDB som du skapade i den [skapa en virtuell dator och installera MongoDB] [ Create a virtual machine and install MongoDB] steg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d68fe-186">Replace `<vm-dns-name>` with the DNS name of the virtual machine running MongoDB you created in the [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="d68fe-187">DNS-namnet på den virtuella datorn finns i Azure Portal, Välj **virtuella datorer**, och hitta **DNS-namnet**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-187">To find the DNS name of your virtual machine, go to the Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="d68fe-188">Om DNS-namnet på den virtuella datorn är ”testlinuxvm.cloudapp.net” och MongoDB lyssnar på standardporten 27017 raden för anslutningssträngen i koden kommer att se ut:</span><span class="sxs-lookup"><span data-stu-id="d68fe-188">If the DNS name of the virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on the default port 27017, the connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="d68fe-189">Om den virtuella datorslutpunkten anger en annan extern port för MongoDB, kan du ange porten i anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="d68fe-189">If the virtual machine endpoint specifies a different external port for MongoDB, you can specifiy the port in the connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="d68fe-190">Mer information om MongoDB-anslutningssträngar finns [anslutningar][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="d68fe-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-the-local-deployment"></a><span data-ttu-id="d68fe-191">Testa lokala distributionen</span><span class="sxs-lookup"><span data-stu-id="d68fe-191">Test the local deployment</span></span>
<span data-ttu-id="d68fe-192">Om du vill köra programmet på utvecklingsdatorn **Start Debugging** från den **felsöka** -menyn eller träffar **F5**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-192">To run your application on your development computer, select **Start Debugging** from the **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="d68fe-193">IIS Express startar och en webbläsare öppnas och startar programmets startsida.</span><span class="sxs-lookup"><span data-stu-id="d68fe-193">IIS Express starts and a browser opens and launches the application's home page.</span></span>  <span data-ttu-id="d68fe-194">Du kan lägga till en ny uppgift som ska läggas till MongoDB-databas som körs på den virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="d68fe-194">You can add a new task, which will be added to the MongoDB database running on your virtual machine in Azure.</span></span>

![Min lista-programmet för aktivitet][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a><span data-ttu-id="d68fe-196">Publicera till Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="d68fe-196">Publish to Azure App Service Web Apps</span></span>
<span data-ttu-id="d68fe-197">I det här avsnittet kommer du publicerar ändringarna till Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d68fe-197">In this section you will publish your changes to Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="d68fe-198">I Solution Explorer högerklickar du på **MyTaskListApp** igen och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="d68fe-199">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="d68fe-200">Du bör nu se ditt webbprogram som körs i Azure App Service och åtkomst till MongoDB-databas i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="d68fe-200">You should now see your web app running in Azure App Service and accessing the MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="d68fe-201">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d68fe-201">Summary</span></span>
<span data-ttu-id="d68fe-202">Du har nu distribuerats ASP.NET-programmet till Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d68fe-202">You have now successfully deployed your ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="d68fe-203">Visa webbprogrammet:</span><span class="sxs-lookup"><span data-stu-id="d68fe-203">To view the web app:</span></span>

1. <span data-ttu-id="d68fe-204">Logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d68fe-204">Log into the Azure Portal.</span></span>
2. <span data-ttu-id="d68fe-205">Klicka på **Webbappar**.</span><span class="sxs-lookup"><span data-stu-id="d68fe-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="d68fe-206">Välj ditt webbprogram i den **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="d68fe-206">Select your web app in the **Web Apps** list.</span></span>

<span data-ttu-id="d68fe-207">Läs mer om hur du utvecklar program C# mot MongoDB [CSharp språk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="d68fe-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

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
