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
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a><span data-ttu-id="d8af9-103">Skapa en webbapp i Azure som ansluter tooMongoDB som körs på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d8af9-103">Create a web app in Azure that connects tooMongoDB running on a virtual machine</span></span>
<span data-ttu-id="d8af9-104">Du kan distribuera ett ASP.NET-program tooAzure App Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="d8af9-104">Using Git, you can deploy an ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="d8af9-105">I den här självstudiekursen skapar du en enkel frontend ASP.NET MVC uppgiften lista över program som ansluter tooa MongoDB-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af9-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects tooa MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="d8af9-106">[MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d8af9-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="d8af9-107">När du kör och testa hello ASP.NET-program på utvecklingsdatorn kan du kommer att överföra hello programmet tooApp Service Web Apps med Git.</span><span class="sxs-lookup"><span data-stu-id="d8af9-107">After running and testing hello ASP.NET application on your development computer, you will upload hello application tooApp Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="d8af9-108">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d8af9-108">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d8af9-109">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d8af9-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="d8af9-110">Bakgrund knowledge</span><span class="sxs-lookup"><span data-stu-id="d8af9-110">Background knowledge</span></span>
<span data-ttu-id="d8af9-111">Hello följande är användbart för den här kursen krävs dock inte:</span><span class="sxs-lookup"><span data-stu-id="d8af9-111">Knowledge of hello following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="d8af9-112">hello C#-drivrutin för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d8af9-112">hello C# driver for MongoDB.</span></span> <span data-ttu-id="d8af9-113">Mer information om hur du utvecklar program C# mot MongoDB finns hello MongoDB [CSharp språk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="d8af9-113">For more information on developing C# applications against MongoDB, see hello MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="d8af9-114">hello ASP .NET web application framework.</span><span class="sxs-lookup"><span data-stu-id="d8af9-114">hello ASP .NET web application framework.</span></span> <span data-ttu-id="d8af9-115">Du kan lära dig om den på hello [ASP.net-webbplats][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="d8af9-115">You can learn all about it at hello [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="d8af9-116">hello ASP .NET MVC web application framework.</span><span class="sxs-lookup"><span data-stu-id="d8af9-116">hello ASP .NET MVC web application framework.</span></span> <span data-ttu-id="d8af9-117">Du kan lära dig om den på hello [webbplats för ASP.NET MVC][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="d8af9-117">You can learn all about it at hello [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="d8af9-118">Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af9-118">Azure.</span></span> <span data-ttu-id="d8af9-119">Du kan börja läsa [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="d8af9-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8af9-120">Krav</span><span class="sxs-lookup"><span data-stu-id="d8af9-120">Prerequisites</span></span>
* <span data-ttu-id="d8af9-121">[Visual Studio Express 2013 för webben] [ VSEWeb] eller [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="d8af9-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="d8af9-122">Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="d8af9-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="d8af9-123">En aktiv Microsoft Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8af9-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="d8af9-124">Skapa en virtuell dator och installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="d8af9-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="d8af9-125">Den här kursen förutsätter att du har skapat en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af9-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="d8af9-126">När du skapar hello virtuell dator måste tooinstall MongoDB på hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="d8af9-126">After creating hello virtual machine you need tooinstall MongoDB on hello virtual machine:</span></span>

* <span data-ttu-id="d8af9-127">toocreate Windows-dator och installera MongoDB, se [installera MongoDB på en virtuell dator som kör Windows Server i Azure][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="d8af9-127">toocreate a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="d8af9-128">När du har skapat hello virtuell dator i Azure och installerat MongoDB, vara säker på att tooremember hello DNS-namn för hello virtuella datorn (”testlinuxvm.cloudapp.net”, till exempel) och hello externa porten för MongoDB som du angav i hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d8af9-128">After you have created hello virtual machine in Azure and installed MongoDB, be sure tooremember hello DNS name of hello virtual machine ("testlinuxvm.cloudapp.net", for example) and hello external port for MongoDB that you specified in hello endpoint.</span></span>  <span data-ttu-id="d8af9-129">Du behöver den här informationen senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="d8af9-129">You will need this information later in hello tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-hello-application"></a><span data-ttu-id="d8af9-130">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="d8af9-130">Create hello application</span></span>
<span data-ttu-id="d8af9-131">I det här avsnittet ska du skapa ett ASP.NET-program som kallas ”uppgiftslistan” med hjälp av Visual Studio och utföra en inledande distribution tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d8af9-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment tooAzure App Service Web Apps.</span></span> <span data-ttu-id="d8af9-132">Du kommer att köra hello programmet lokalt, men den ansluta tooyour virtuell dator på Azure och använda hello MongoDB-instansen som du skapade finns.</span><span class="sxs-lookup"><span data-stu-id="d8af9-132">You will run hello application locally, but it will connect tooyour virtual machine on Azure and use hello MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="d8af9-133">I Visual Studio klickar du på **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Starta sidan nytt projekt][StartPageNewProject]
2. <span data-ttu-id="d8af9-135">I hello **nytt projekt** i hello till vänster och välj fönstret **Visual C#**, och välj sedan **Web**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-135">In hello **New Project** window, in hello left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="d8af9-136">I mittfönstret hello väljer **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-136">In hello middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="d8af9-137">Namnge projektet ”MyTaskListApp” hello längst ned i och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-137">At hello bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Dialogrutan Nytt projekt][NewProjectMyTaskListApp]
3. <span data-ttu-id="d8af9-139">I hello **nytt ASP.NET-projekt** dialogrutan **MVC**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-139">In hello **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![Välj MVC-mallen][VS2013SelectMVCTemplate]
4. <span data-ttu-id="d8af9-141">Om du inte redan inloggad på Microsoft Azure, kommer du att ange toosign i.</span><span class="sxs-lookup"><span data-stu-id="d8af9-141">If you aren't already signed into Microsoft Azure, you will be prompted toosign in.</span></span> <span data-ttu-id="d8af9-142">Följ hello prompter toosign till Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af9-142">Follow hello prompts toosign into Azure.</span></span>
5. <span data-ttu-id="d8af9-143">När du har loggat in kan börja du konfigurera din App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="d8af9-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="d8af9-144">Ange hello **Web App name**, **programtjänstplanen**, **resursgruppen**, och **Region**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-144">Specify hello **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="d8af9-145">När hello projektet har skapats, vänta på att skapas i Azure App Service som anges i hello hello web app toobe **aktivitet för Azure App Service** fönster.</span><span class="sxs-lookup"><span data-stu-id="d8af9-145">After hello project creation completes, wait for hello web app toobe created in Azure App Service as indicated in hello **Azure App Service Activity** window.</span></span> <span data-ttu-id="d8af9-146">Klicka på **publicera MyTaskListApp toothis Web App nu**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-146">Then, click **Publish MyTaskListApp toothis Web App now**.</span></span>
7. <span data-ttu-id="d8af9-147">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="d8af9-148">När ASP.NET-standardprogrammet publicerade tooAzure App Service Web Apps kan startas i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d8af9-148">Once your default ASP.NET application is published tooAzure App Service Web Apps, it will be launched in hello browser.</span></span>

## <a name="install-hello-mongodb-c-driver"></a><span data-ttu-id="d8af9-149">Installera hello MongoDB C#-drivrutinen</span><span class="sxs-lookup"><span data-stu-id="d8af9-149">Install hello MongoDB C# driver</span></span>
<span data-ttu-id="d8af9-150">MongoDB har stöd för klientsidan för C#-program via en drivrutin som du behöver tooinstall på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="d8af9-150">MongoDB offers client-side support for C# applications through a driver, which you need tooinstall on your local development computer.</span></span> <span data-ttu-id="d8af9-151">hello C# drivrutinen är tillgängliga via NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8af9-151">hello C# driver is available through NuGet.</span></span>

<span data-ttu-id="d8af9-152">tooinstall hello MongoDB C#-drivrutinen:</span><span class="sxs-lookup"><span data-stu-id="d8af9-152">tooinstall hello MongoDB C# driver:</span></span>

1. <span data-ttu-id="d8af9-153">I **Solution Explorer**, högerklicka på hello **MyTaskListApp** projektet och välj **hantera NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-153">In **Solution Explorer**, right-click hello **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![Hantera NuGet-paket][VS2013ManageNuGetPackages]
2. <span data-ttu-id="d8af9-155">I hello **hantera NuGet-paket** hello vänster, klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-155">In hello **Manage NuGet Packages** window, in hello left pane, click **Online**.</span></span> <span data-ttu-id="d8af9-156">I hello **Sök Online** på hello höger, Skriv ”mongodb.driver”.</span><span class="sxs-lookup"><span data-stu-id="d8af9-156">In hello **Search Online** box on hello right, type "mongodb.driver".</span></span>  <span data-ttu-id="d8af9-157">Klicka på **installera** tooinstall hello-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="d8af9-157">Click **Install** tooinstall hello driver.</span></span>
   
    ![Sök efter MongoDB C# drivrutin][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="d8af9-159">Klicka på **jag accepterar** tooaccept hello 10gen, Inc. licensvillkor.</span><span class="sxs-lookup"><span data-stu-id="d8af9-159">Click **I Accept** tooaccept hello 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="d8af9-160">Klicka på **Stäng** när hello-drivrutinen har installerats.</span><span class="sxs-lookup"><span data-stu-id="d8af9-160">Click **Close** after hello driver has installed.</span></span>
    <span data-ttu-id="d8af9-161">![MongoDB C# drivrutin installerad][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="d8af9-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="d8af9-162">Hej MongoDB C#-drivrutinen har installerats.</span><span class="sxs-lookup"><span data-stu-id="d8af9-162">hello MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="d8af9-163">Referenser toohello **MongoDB.Bson**, **MongoDB.Driver**, och **MongoDB.Driver.Core** toohello projekt har lagts till bibliotek.</span><span class="sxs-lookup"><span data-stu-id="d8af9-163">References toohello **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added toohello project.</span></span>

![MongoDB C# drivrutinen referenser][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="d8af9-165">Lägga till en modell</span><span class="sxs-lookup"><span data-stu-id="d8af9-165">Add a model</span></span>
<span data-ttu-id="d8af9-166">I **Solution Explorer**, högerklicka på hello *modeller* mapp och **Lägg till** en ny **klassen** och ger den namnet *TaskModel.cs* .</span><span class="sxs-lookup"><span data-stu-id="d8af9-166">In **Solution Explorer**, right-click hello *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="d8af9-167">I *TaskModel.cs*, Ersätt hello befintlig kod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d8af9-167">In *TaskModel.cs*, replace hello existing code with hello following code:</span></span>

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

## <a name="add-hello-data-access-layer"></a><span data-ttu-id="d8af9-168">Lägg till hello dataåtkomstnivå</span><span class="sxs-lookup"><span data-stu-id="d8af9-168">Add hello data access layer</span></span>
<span data-ttu-id="d8af9-169">I **Solution Explorer**, högerklicka på hello *MyTaskListApp* projekt och **Lägg till** en **ny mapp** med namnet *DAL*.</span><span class="sxs-lookup"><span data-stu-id="d8af9-169">In **Solution Explorer**, right-click hello *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="d8af9-170">Högerklicka på hello *DAL* mapp och **Lägg till** en ny **klassen**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-170">Right-click hello *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="d8af9-171">Namnet hello klassen filen *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8af9-171">Name hello class file *Dal.cs*.</span></span>  <span data-ttu-id="d8af9-172">I *Dal.cs*, Ersätt hello befintlig kod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d8af9-172">In *Dal.cs*, replace hello existing code with hello following code:</span></span>

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

## <a name="add-a-controller"></a><span data-ttu-id="d8af9-173">Lägga till en styrenhet</span><span class="sxs-lookup"><span data-stu-id="d8af9-173">Add a controller</span></span>
<span data-ttu-id="d8af9-174">Öppna hello *Controllers\HomeController.cs* filen i **Solution Explorer** och Ersätt hello befintlig kod med hello följande:</span><span class="sxs-lookup"><span data-stu-id="d8af9-174">Open hello *Controllers\HomeController.cs* file in **Solution Explorer** and replace hello existing code with hello following:</span></span>

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

## <a name="set-up-hello-styles"></a><span data-ttu-id="d8af9-175">Ställ in hello-format</span><span class="sxs-lookup"><span data-stu-id="d8af9-175">Set up hello styles</span></span>
<span data-ttu-id="d8af9-176">toochange hello rubrik överst hello på hello-sidan, öppna hello *Views\Shared\\_Layout.cshtml* filen i **Solution Explorer** och Ersätt ”namn” i hello navigeringsfält för sidhuvud med ”Mina aktivitet Visa en lista med program ”så att den ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d8af9-176">toochange hello title at hello top of hello page, open hello *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in hello navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="d8af9-177">Öppna hello i ordning tooset hello uppgiftslista meny, *\Views\Home\Index.cshtml* filen och ersätter hello befintlig kod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d8af9-177">In order tooset up hello Task List menu, open hello *\Views\Home\Index.cshtml* file and replace hello existing code with hello following code:</span></span>

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


<span data-ttu-id="d8af9-178">tooadd hello möjlighet toocreate en ny uppgift, högerklicka på hello *Views\Home\\*  mapp och **Lägg till** en **visa**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-178">tooadd hello ability toocreate a new task, right-click hello *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="d8af9-179">Namnge hello visa *skapa*.</span><span class="sxs-lookup"><span data-stu-id="d8af9-179">Name hello view *Create*.</span></span> <span data-ttu-id="d8af9-180">Ersätt hello kod med hello följande:</span><span class="sxs-lookup"><span data-stu-id="d8af9-180">Replace hello code with hello following:</span></span>

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

<span data-ttu-id="d8af9-181">**Solution Explorer** bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="d8af9-181">**Solution Explorer** should look like this:</span></span>

![Solution Explorer][SolutionExplorerMyTaskListApp]

## <a name="set-hello-mongodb-connection-string"></a><span data-ttu-id="d8af9-183">Ange anslutningssträngen för hello MongoDB</span><span class="sxs-lookup"><span data-stu-id="d8af9-183">Set hello MongoDB connection string</span></span>
<span data-ttu-id="d8af9-184">I **Solution Explorer**öppnar hello *DAL/Dal.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="d8af9-184">In **Solution Explorer**, open hello *DAL/Dal.cs* file.</span></span> <span data-ttu-id="d8af9-185">Hitta hello följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="d8af9-185">Find hello following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="d8af9-186">Ersätt `<vm-dns-name>` med hello DNS-namnet på hello virtuell dator som kör MongoDB som du skapade i hello [skapa en virtuell dator och installera MongoDB] [ Create a virtual machine and install MongoDB] steg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8af9-186">Replace `<vm-dns-name>` with hello DNS name of hello virtual machine running MongoDB you created in hello [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="d8af9-187">toofind hello DNS-namnet på den virtuella datorn, gå toohello Azure Portal, Välj **virtuella datorer**, och hitta **DNS-namnet**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-187">toofind hello DNS name of your virtual machine, go toohello Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="d8af9-188">Om hello hello virtuella datorns DNS-namn är ”testlinuxvm.cloudapp.net” och MongoDB lyssnar på standardporten för hello 27017 hello raden för anslutningssträngen i koden kommer att se ut:</span><span class="sxs-lookup"><span data-stu-id="d8af9-188">If hello DNS name of hello virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on hello default port 27017, hello connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="d8af9-189">Om hello virtuell datorslutpunkt anger en annan extern port för MongoDB, kan du ange hello port i hello anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="d8af9-189">If hello virtual machine endpoint specifies a different external port for MongoDB, you can specifiy hello port in hello connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="d8af9-190">Mer information om MongoDB-anslutningssträngar finns [anslutningar][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="d8af9-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-hello-local-deployment"></a><span data-ttu-id="d8af9-191">Testa hello lokala distributionen</span><span class="sxs-lookup"><span data-stu-id="d8af9-191">Test hello local deployment</span></span>
<span data-ttu-id="d8af9-192">toorun programmet på utvecklingsdatorn, Välj **Start Debugging** från hello **felsöka** -menyn eller träffar **F5**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-192">toorun your application on your development computer, select **Start Debugging** from hello **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="d8af9-193">IIS Express startar och en webbläsare öppnas och startar hello programmets startsida.</span><span class="sxs-lookup"><span data-stu-id="d8af9-193">IIS Express starts and a browser opens and launches hello application's home page.</span></span>  <span data-ttu-id="d8af9-194">Du kan lägga till en ny uppgift läggs toohello MongoDB-databas som körs på den virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af9-194">You can add a new task, which will be added toohello MongoDB database running on your virtual machine in Azure.</span></span>

![Min lista-programmet för aktivitet][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a><span data-ttu-id="d8af9-196">Publicera tooAzure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="d8af9-196">Publish tooAzure App Service Web Apps</span></span>
<span data-ttu-id="d8af9-197">I det här avsnittet kommer du publicera dina ändringar tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d8af9-197">In this section you will publish your changes tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="d8af9-198">I Solution Explorer högerklickar du på **MyTaskListApp** igen och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="d8af9-199">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="d8af9-200">Du bör nu se ditt webbprogram som körs i Azure App Service och åt hello MongoDB-databas i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="d8af9-200">You should now see your web app running in Azure App Service and accessing hello MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="d8af9-201">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d8af9-201">Summary</span></span>
<span data-ttu-id="d8af9-202">Du har nu har distribuerat din ASP.NET-program tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d8af9-202">You have now successfully deployed your ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="d8af9-203">tooview hello-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="d8af9-203">tooview hello web app:</span></span>

1. <span data-ttu-id="d8af9-204">Logga in på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d8af9-204">Log into hello Azure Portal.</span></span>
2. <span data-ttu-id="d8af9-205">Klicka på **Webbappar**.</span><span class="sxs-lookup"><span data-stu-id="d8af9-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="d8af9-206">Välj ditt webbprogram i hello **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="d8af9-206">Select your web app in hello **Web Apps** list.</span></span>

<span data-ttu-id="d8af9-207">Läs mer om hur du utvecklar program C# mot MongoDB [CSharp språk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="d8af9-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

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
