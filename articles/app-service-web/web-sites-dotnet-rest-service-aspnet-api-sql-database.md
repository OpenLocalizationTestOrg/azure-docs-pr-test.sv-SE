---
title: aaaCreate en REST-API i Azure med ASP.NET och SQL-databas | Microsoft Docs
description: "En självstudiekurs som lär du dig hur toodeploy en app som använder hello ASP.NET Web API tooan Azure-webbapp med hjälp av Visual Studio."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: f4916fc0-ea08-41f7-846b-73e41bc88149
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: riande
ms.openlocfilehash: 1ef45dd1582bfda367e53c39f863164422ad678b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a><span data-ttu-id="16af2-103">Skapa ett REST-tjänst som använder ASP.NET Web API och SQL-databas i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="16af2-103">Create a REST service using ASP.NET Web API and SQL Database in Azure App Service</span></span>
<span data-ttu-id="16af2-104">Den här kursen visar hur toodeploy ett ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) med hjälp av guiden för hello Publicera webbplats i Visual Studio 2013 eller Visual Studio 2013 Community Edition.</span><span class="sxs-lookup"><span data-stu-id="16af2-104">This tutorial shows how toodeploy an ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by using hello Publish Web wizard in Visual Studio 2013 or Visual Studio 2013 Community Edition.</span></span> 

<span data-ttu-id="16af2-105">Du kan öppna ett Azure-konto gratis och om du inte redan har Visual Studio 2013, hello SDK installerar Visual Studio 2013 automatiskt för Web Express.</span><span class="sxs-lookup"><span data-stu-id="16af2-105">You can open an Azure account for free, and if you don't already have Visual Studio 2013, hello SDK automatically installs Visual Studio 2013 for Web Express.</span></span> <span data-ttu-id="16af2-106">Så kan du börja utveckla för Azure och helt kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="16af2-106">So you can start developing for Azure entirely for free.</span></span>

<span data-ttu-id="16af2-107">Den här kursen förutsätter att du har några tidigare erfarenheter av att använda Azure.</span><span class="sxs-lookup"><span data-stu-id="16af2-107">This tutorial assumes that you have no prior experience using Azure.</span></span> <span data-ttu-id="16af2-108">På den här kursen har du en enkel webbapp upp och körs i hello moln.</span><span class="sxs-lookup"><span data-stu-id="16af2-108">On completing this tutorial, you'll have a simple web app up and running in hello cloud.</span></span>

<span data-ttu-id="16af2-109">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="16af2-109">You'll learn:</span></span>

* <span data-ttu-id="16af2-110">Hur tooenable datorn för Azure-utveckling genom att installera hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="16af2-110">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="16af2-111">Hur toocreate en Visual Studio ASP.NET MVC 5 projektet och publicera den tooan Azure-app.</span><span class="sxs-lookup"><span data-stu-id="16af2-111">How toocreate a Visual Studio ASP.NET MVC 5 project and publish it tooan Azure app.</span></span>
* <span data-ttu-id="16af2-112">Hur toouse hello ASP.NET Web API tooenable Restful-API-anrop.</span><span class="sxs-lookup"><span data-stu-id="16af2-112">How toouse hello ASP.NET Web API tooenable Restful API calls.</span></span>
* <span data-ttu-id="16af2-113">Hur toouse en SQL-databas toostore data i Azure.</span><span class="sxs-lookup"><span data-stu-id="16af2-113">How toouse a SQL database toostore data in Azure.</span></span>
* <span data-ttu-id="16af2-114">Hur uppdaterar toopublish programmet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="16af2-114">How toopublish application updates tooAzure.</span></span>

<span data-ttu-id="16af2-115">Du skapar ett enkelt kontaktlista webbprogram som bygger på ASP.NET MVC 5 och använder hello ADO.NET Entity Framework för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="16af2-115">You'll build a simple contact list web application that is built on ASP.NET MVC 5 and uses hello ADO.NET Entity Framework for database access.</span></span> <span data-ttu-id="16af2-116">Följande bild visar hello hello slutförts program:</span><span class="sxs-lookup"><span data-stu-id="16af2-116">hello following illustration shows hello completed application:</span></span>

![Skärmbild av webbplats][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a><span data-ttu-id="16af2-118">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="16af2-118">Create hello project</span></span>
1. <span data-ttu-id="16af2-119">Starta Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="16af2-119">Start Visual Studio 2013.</span></span>
2. <span data-ttu-id="16af2-120">Från hello **filen** Klicka **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="16af2-120">From hello **File** menu click **New Project**.</span></span>
3. <span data-ttu-id="16af2-121">I hello **nytt projekt** dialogrutan Expandera **Visual C#** och välj **Web** och välj sedan **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="16af2-121">In hello **New Project** dialog box, expand **Visual C#** and select **Web**  and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="16af2-122">Namnge programmet hello **ContactManager** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="16af2-122">Name hello application **ContactManager** and click **OK**.</span></span>
   
    ![Dialogrutan Nytt projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. <span data-ttu-id="16af2-124">I hello **nytt ASP.NET-projekt** dialogrutan, Välj hello **MVC** mall, kontrollera **Web API** och klicka sedan på **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="16af2-124">In hello **New ASP.NET Project** dialog box, select hello **MVC** template, check **Web API** and then click **Change Authentication**.</span></span>
5. <span data-ttu-id="16af2-125">I hello **ändra autentisering** dialogrutan klickar du på **ingen autentisering**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="16af2-125">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span>
   
    ![Ingen autentisering](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    <span data-ttu-id="16af2-127">hello exempelprogram som du skapar inte funktioner som kräver användare toolog i.</span><span class="sxs-lookup"><span data-stu-id="16af2-127">hello sample application you're creating won't have features that require users toolog in.</span></span> <span data-ttu-id="16af2-128">Information om hur tooimplement autentisering och auktorisering funktioner finns hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="16af2-128">For information about how tooimplement authentication and authorization features, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
6. <span data-ttu-id="16af2-129">I hello **nytt ASP.NET-projekt** dialogrutan, se till att hello **värd i hello molnet** är markerad och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="16af2-129">In hello **New ASP.NET Project** dialog box, make sure hello **Host in hello Cloud** is checked and click **OK**.</span></span>

<span data-ttu-id="16af2-130">Om du redan inte har registrerat i tooAzure, kommer du att ange toosign i.</span><span class="sxs-lookup"><span data-stu-id="16af2-130">If you have not previously signed in tooAzure, you will be prompted toosign in.</span></span>

1. <span data-ttu-id="16af2-131">hello konfigurationsguiden föreslås ett unikt namn baserat på *ContactManager* (se hello bilden nedan).</span><span class="sxs-lookup"><span data-stu-id="16af2-131">hello configuration wizard will suggest a unique name based on *ContactManager* (see hello image below).</span></span> <span data-ttu-id="16af2-132">Välj en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="16af2-132">Select a region near you.</span></span> <span data-ttu-id="16af2-133">Du kan använda [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello lägsta svarstid datacenter.</span><span class="sxs-lookup"><span data-stu-id="16af2-133">You can use [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello lowest latency data center.</span></span> 
2. <span data-ttu-id="16af2-134">Om du inte har skapat en databasserver innan du väljer **Skapa ny server**, ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="16af2-134">If you haven't created a database server before, select **Create new server**, enter a database user name and password.</span></span>
   
    ![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

<span data-ttu-id="16af2-136">Om du har en databasserver, använder du den toocreate en ny databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-136">If you have a database server, use that toocreate a new database.</span></span> <span data-ttu-id="16af2-137">Databasservrar är en värdefull resurs och vanligtvis vill toocreate flera databaser på hello samma server för testning och utveckling istället för att skapa en databasserver per databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-137">Database servers are a precious resource, and you generally want toocreate multiple databases on hello same server for testing and development rather than creating a database server per database.</span></span> <span data-ttu-id="16af2-138">Se till att webbplatsen och databasen tillhör hello samma region.</span><span class="sxs-lookup"><span data-stu-id="16af2-138">Make sure your web site and database are in hello same region.</span></span>

![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a><span data-ttu-id="16af2-140">Ange hello sidhuvud och sidfot</span><span class="sxs-lookup"><span data-stu-id="16af2-140">Set hello page header and footer</span></span>
1. <span data-ttu-id="16af2-141">I **Solution Explorer**, expandera hello *Views\Shared* mapp och öppna hello *_Layout.cshtml* fil.</span><span class="sxs-lookup"><span data-stu-id="16af2-141">In **Solution Explorer**, expand hello *Views\Shared* folder and open hello *_Layout.cshtml* file.</span></span>
   
    ![_Layout.cshtml i Solution Explorer][newapp004]
2. <span data-ttu-id="16af2-143">Ersätt hello innehållet i hello *Views\Shared_Layout.cshtml* fil med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="16af2-143">Replace hello contents of hello *Views\Shared_Layout.cshtml* file with hello following code:</span></span>

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>

<span data-ttu-id="16af2-144">hello markup ovan ändringar hello appens namn från ”Mina ASP.NET-App” för ”Kontakta Manager” och tar bort hello länkar för**Start**, **om** och **Kontakta**.</span><span class="sxs-lookup"><span data-stu-id="16af2-144">hello markup above changes hello app name from "My ASP.NET App" too"Contact Manager", and it removes hello links too**Home**, **About** and **Contact**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="16af2-145">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="16af2-145">Run hello application locally</span></span>
1. <span data-ttu-id="16af2-146">Tryck på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="16af2-146">Press CTRL+F5 toorun hello application.</span></span>
   <span data-ttu-id="16af2-147">startsidan för hello programmet visas i hello standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="16af2-147">hello application home page appears in hello default browser.</span></span>
    <span data-ttu-id="16af2-148">![startsidan för tooDo lista](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span><span class="sxs-lookup"><span data-stu-id="16af2-148">![tooDo List home page](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span></span>

<span data-ttu-id="16af2-149">Det här är allt du behöver toodo för att du ska distribuera tooAzure nu toocreate hello program.</span><span class="sxs-lookup"><span data-stu-id="16af2-149">This is all you need toodo for now toocreate hello application that you'll deploy tooAzure.</span></span> <span data-ttu-id="16af2-150">Senare ska du lägga till databasfunktioner.</span><span class="sxs-lookup"><span data-stu-id="16af2-150">Later you'll add database functionality.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="16af2-151">Distribuera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="16af2-151">Deploy hello application tooAzure</span></span>
1. <span data-ttu-id="16af2-152">I Visual Studio högerklickar du på hello-projekt i **Solution Explorer** och välj **publicera** hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="16af2-152">In Visual Studio, right-click hello project in **Solution Explorer** and select **Publish** from hello context menu.</span></span>
   
    ![Publicera i snabbmenyn för projektet][PublishVSSolution]
   
    <span data-ttu-id="16af2-154">Hej **Publicera webbplats** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="16af2-154">hello **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="16af2-155">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="16af2-155">Click **Publish**.</span></span>

![Fliken Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

<span data-ttu-id="16af2-157">Visual Studio börjar hello processen att kopiera hello filer toohello Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="16af2-157">Visual Studio begins hello process of copying hello files toohello Azure server.</span></span> <span data-ttu-id="16af2-158">Hej **utdata** fönster visar vilka distributionsåtgärder som utförts och rapporterar slutförande av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="16af2-158">hello **Output** window shows what deployment actions were taken and reports successful completion of hello deployment.</span></span>

1. <span data-ttu-id="16af2-159">hello standardwebbläsare öppnas automatiskt toohello URL för hello distribueras webbplats.</span><span class="sxs-lookup"><span data-stu-id="16af2-159">hello default browser automatically opens toohello URL of hello deployed site.</span></span>
   
   <span data-ttu-id="16af2-160">hello-program som du skapade körs nu i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="16af2-160">hello application you created is now running in hello cloud.</span></span>
   
   ![startsidan för tooDo lista körs i Azure][rxz2]

## <a name="add-a-database-toohello-application"></a><span data-ttu-id="16af2-162">Lägg till ett databasprogram toohello</span><span class="sxs-lookup"><span data-stu-id="16af2-162">Add a database toohello application</span></span>
<span data-ttu-id="16af2-163">Därefter måste du uppdatera hello MVC-program tooadd hello möjlighet toodisplay och uppdatera kontakter och lagra hello data i en databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-163">Next, you'll update hello MVC application tooadd hello ability toodisplay and update contacts and store hello data in a database.</span></span> <span data-ttu-id="16af2-164">hello program använder hello Entity Framework toocreate hello databasen och tooread och uppdatera data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-164">hello application will use hello Entity Framework toocreate hello database and tooread and update data in hello database.</span></span>

### <a name="add-data-model-classes-for-hello-contacts"></a><span data-ttu-id="16af2-165">Lägg till modellen dataklasser för hello kontakter</span><span class="sxs-lookup"><span data-stu-id="16af2-165">Add data model classes for hello contacts</span></span>
<span data-ttu-id="16af2-166">Börjar du med att skapa en enkel datamodell i kod.</span><span class="sxs-lookup"><span data-stu-id="16af2-166">You begin by creating a simple data model in code.</span></span>

1. <span data-ttu-id="16af2-167">I **Solution Explorer**, högerklicka på mappen för hello modeller, klicka på **Lägg till**, och sedan **klassen**.</span><span class="sxs-lookup"><span data-stu-id="16af2-167">In **Solution Explorer**, right-click hello Models folder, click **Add**, and then **Class**.</span></span>
   
    ![Lägg till klass i modeller mappen snabbmenyn][adddb001]
2. <span data-ttu-id="16af2-169">I hello **Lägg till nytt objekt** dialogrutan, namnet hello nya klassfilen *Contact.cs*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-169">In hello **Add New Item** dialog box, name hello new class file *Contact.cs*, and then click **Add**.</span></span>
   
    ![Lägg till nytt objekt-dialogruta][adddb002]
3. <span data-ttu-id="16af2-171">Ersätt hello innehållet i hello Contacts.cs filen med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="16af2-171">Replace hello contents of hello Contacts.cs file with hello following code.</span></span>
   
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
               {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

<span data-ttu-id="16af2-172">Hej **Kontakta** definierar klassen hello data som ska lagras för varje kontakt, plus en primär nyckel, kontakt ID som krävs av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="16af2-172">hello **Contact** class defines hello data that you will store for each contact, plus a primary key, ContactID, that is needed by hello database.</span></span> <span data-ttu-id="16af2-173">Du kan hämta mer information om datamodeller i hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="16af2-173">You can get more information about data models in hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span>

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a><span data-ttu-id="16af2-174">Skapa webbsidor som gör app användare toowork med hello kontakter</span><span class="sxs-lookup"><span data-stu-id="16af2-174">Create web pages that enable app users toowork with hello contacts</span></span>
<span data-ttu-id="16af2-175">hello ASP.NET MVC hello scaffold-teknik för funktionen automatiskt generera kod som utför skapa, läsa, uppdatera och ta bort åtgärder (CRUD).</span><span class="sxs-lookup"><span data-stu-id="16af2-175">hello ASP.NET MVC hello scaffolding feature can automatically generate code that performs create, read, update, and delete (CRUD) actions.</span></span>

## <a name="add-a-controller-and-a-view-for-hello-data"></a><span data-ttu-id="16af2-176">Lägg till en domänkontrollant och en vy för hello data</span><span class="sxs-lookup"><span data-stu-id="16af2-176">Add a Controller and a view for hello data</span></span>
1. <span data-ttu-id="16af2-177">I **Solution Explorer**, expandera hello domänkontrollanter mappen.</span><span class="sxs-lookup"><span data-stu-id="16af2-177">In **Solution Explorer**, expand hello Controllers folder.</span></span>
2. <span data-ttu-id="16af2-178">Skapa hello projektet **(Ctrl + Skift + B)**.</span><span class="sxs-lookup"><span data-stu-id="16af2-178">Build hello project **(Ctrl+Shift+B)**.</span></span> <span data-ttu-id="16af2-179">(Du måste skapa hello projektet innan du använder scaffold-teknik mekanism.)</span><span class="sxs-lookup"><span data-stu-id="16af2-179">(You must build hello project before using scaffolding mechanism.)</span></span> 
3. <span data-ttu-id="16af2-180">Högerklicka på mappen för hello-styrenheter och klicka på **Lägg till**, och klicka sedan på **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="16af2-180">Right-click hello Controllers folder and click **Add**, and then click **Controller**.</span></span>
   
    ![Lägga till domänkontrollanter i snabbmenyn för domänkontrollanter mapp][addcode001]
4. <span data-ttu-id="16af2-182">I hello **Lägg till Kodskelett** dialogrutan **MVC-enhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-182">In hello **Add Scaffold** dialog box, select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
   
   ![Lägga till en kontrollant](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. <span data-ttu-id="16af2-184">Ange hello Kontrollnamn för**HomeController**.</span><span class="sxs-lookup"><span data-stu-id="16af2-184">Set hello controller name too**HomeController**.</span></span> <span data-ttu-id="16af2-185">Välj **Kontakta** som modellklass.</span><span class="sxs-lookup"><span data-stu-id="16af2-185">Select **Contact** as your model class.</span></span> <span data-ttu-id="16af2-186">Klicka på hello **nya datakontexten** knappen och acceptera hello standard ”ContactManager.Models.ContactManagerContext” för hello **nya datakontexttyp**.</span><span class="sxs-lookup"><span data-stu-id="16af2-186">Click hello **New data context** button and accept hello default "ContactManager.Models.ContactManagerContext" for hello **New data context type**.</span></span> <span data-ttu-id="16af2-187">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-187">Click **Add**.</span></span>

    <span data-ttu-id="16af2-188">En dialogruta där du uppmanas: ”en fil med namnet hello HomeController redan.</span><span class="sxs-lookup"><span data-stu-id="16af2-188">A dialog box will prompt you: "A file with hello name HomeController already exits.</span></span> <span data-ttu-id="16af2-189">Vill du tooreplace den ”?.</span><span class="sxs-lookup"><span data-stu-id="16af2-189">Do you want tooreplace it?".</span></span> <span data-ttu-id="16af2-190">Klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="16af2-190">Click **Yes**.</span></span> <span data-ttu-id="16af2-191">Vi skriver över hello Start domänkontrollant som har skapats med hello nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="16af2-191">We are overwriting hello Home Controller that was created with hello new project.</span></span> <span data-ttu-id="16af2-192">Vi använder hello nya Start styrenhet för våra kontaktlista.</span><span class="sxs-lookup"><span data-stu-id="16af2-192">We will use hello new Home Controller for our contact list.</span></span>

    <span data-ttu-id="16af2-193">Visual Studio skapar kontrollantmetoder och vyer för CRUD-åtgärder för databasen för **Kontakta** objekt.</span><span class="sxs-lookup"><span data-stu-id="16af2-193">Visual Studio creates controller methods and views for CRUD database operations for **Contact** objects.</span></span>

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a><span data-ttu-id="16af2-194">Aktivera migrering, skapa hello databas, lägga till exempeldata och en data-initieraren</span><span class="sxs-lookup"><span data-stu-id="16af2-194">Enable Migrations, create hello database, add sample data and a data initializer</span></span>
<span data-ttu-id="16af2-195">hello nästa uppgift är tooenable hello [Code First Migrations](http://curah.microsoft.com/55220) funktion i ordning toocreate hello databasen utifrån hello-datamodell som du skapade.</span><span class="sxs-lookup"><span data-stu-id="16af2-195">hello next task is tooenable hello [Code First Migrations](http://curah.microsoft.com/55220) feature in order toocreate hello database based on hello data model you created.</span></span>

1. <span data-ttu-id="16af2-196">I hello **verktyg** väljer du **Library Package Manager** och sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="16af2-196">In hello **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>
   
    ![Package Manager-konsolen i Verktyg-menyn][addcode008]
2. <span data-ttu-id="16af2-198">I hello **Pakethanterarkonsolen** fönstret Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="16af2-198">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        enable-migrations 
   
    <span data-ttu-id="16af2-199">Hej **aktivera migreringar** kommando skapar en *migreringar* mapp och dess placeras i den mappen en *Configuration.cs* fil att du kan redigera tooconfigure migreringar.</span><span class="sxs-lookup"><span data-stu-id="16af2-199">hello **enable-migrations** command creates a *Migrations* folder and it puts in that folder a *Configuration.cs* file that you can edit tooconfigure Migrations.</span></span> 
3. <span data-ttu-id="16af2-200">I hello **Pakethanterarkonsolen** fönstret Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="16af2-200">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        add-migration Initial
   
    <span data-ttu-id="16af2-201">Hej **lägga till migrering inledande** kommandot genererar en klass med namnet  **&lt;date_stamp&gt;inledande** som skapar hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="16af2-201">hello **add-migration Initial** command generates a class named **&lt;date_stamp&gt;Initial** that creates hello database.</span></span> <span data-ttu-id="16af2-202">Hej första parametern ( *inledande* ) är valfri och används toocreate hello namnet på hello-filen.</span><span class="sxs-lookup"><span data-stu-id="16af2-202">hello first parameter ( *Initial* ) is arbitrary and used toocreate hello name of hello file.</span></span> <span data-ttu-id="16af2-203">Du kan se hello nya klassfiler i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="16af2-203">You can see hello new class files in **Solution Explorer**.</span></span>
   
    <span data-ttu-id="16af2-204">I hello **inledande** class, hello **in** metoden skapar hello tabellen för kontakter och hello **ned** metod (används när du vill tooreturn toohello läget) släpper den.</span><span class="sxs-lookup"><span data-stu-id="16af2-204">In hello **Initial** class, hello **Up** method creates hello Contacts table, and hello **Down** method (used when you want tooreturn toohello previous state) drops it.</span></span>
4. <span data-ttu-id="16af2-205">Öppna hello *Migrations\Configuration.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="16af2-205">Open hello *Migrations\Configuration.cs* file.</span></span> 
5. <span data-ttu-id="16af2-206">Lägg till hello följande namnområden.</span><span class="sxs-lookup"><span data-stu-id="16af2-206">Add hello following namespaces.</span></span> 
   
         using ContactManager.Models;
6. <span data-ttu-id="16af2-207">Ersätt hello *Seed* metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="16af2-207">Replace hello *Seed* method with hello following code:</span></span>
   
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }
   
    <span data-ttu-id="16af2-208">Den här koden ovan ska initiera hello databasen med hello kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="16af2-208">This code above will initialize hello database with hello contact information.</span></span> <span data-ttu-id="16af2-209">Mer information om seeding hello databasen finns [felsökning (EF Entity Framework) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span><span class="sxs-lookup"><span data-stu-id="16af2-209">For more information on seeding hello database, see [Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span></span>
7. <span data-ttu-id="16af2-210">I hello **Pakethanterarkonsolen** ange hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="16af2-210">In hello **Package Manager Console** enter hello command:</span></span>
   
        update-database
   
    ![Package Manager-konsolen kommandon][addcode009]
   
    <span data-ttu-id="16af2-212">Hej **uppdatering databasen** körs hello första migrering som skapar hello-databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-212">hello **update-database** runs hello first migration which creates hello database.</span></span> <span data-ttu-id="16af2-213">Som standard skapas hello databasen som en SQL Server Express LocalDB-databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-213">By default, hello database is created as a SQL Server Express LocalDB database.</span></span>
8. <span data-ttu-id="16af2-214">Tryck på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="16af2-214">Press CTRL+F5 toorun hello application.</span></span> 

<span data-ttu-id="16af2-215">hello programmet hello fördefiniera data och ger redigera, information och ta bort länkar.</span><span class="sxs-lookup"><span data-stu-id="16af2-215">hello application shows hello seed data and provides edit, details and delete links.</span></span>

![MVC-vy av data][rxz3]

## <a name="edit-hello-view"></a><span data-ttu-id="16af2-217">Redigera hello vy</span><span class="sxs-lookup"><span data-stu-id="16af2-217">Edit hello View</span></span>
1. <span data-ttu-id="16af2-218">Öppna hello *Views\Home\Index.cshtml* fil.</span><span class="sxs-lookup"><span data-stu-id="16af2-218">Open hello *Views\Home\Index.cshtml* file.</span></span> <span data-ttu-id="16af2-219">I hello nästa steg, kommer vi att ersätta hello genereras markup med kod som använder [jQuery](http://jquery.com/) och [Knockout.js](http://knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="16af2-219">In hello next step, we will replace hello generated markup with code that uses [jQuery](http://jquery.com/) and [Knockout.js](http://knockoutjs.com/).</span></span> <span data-ttu-id="16af2-220">Den här nya koden hämtar hello lista över kontakter från att använda webb-API och JSON och bindningar hello kontakta data toohello användargränssnitt med knockout.js.</span><span class="sxs-lookup"><span data-stu-id="16af2-220">This new code retrieves hello list of contacts from using web API and JSON and then binds hello contact data toohello UI using knockout.js.</span></span> <span data-ttu-id="16af2-221">Mer information finns i hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="16af2-221">For more information, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
2. <span data-ttu-id="16af2-222">Ersätt hello innehållet i hello-fil med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="16af2-222">Replace hello contents of hello file with hello following code.</span></span>
   
        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }
   
                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());    
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>
3. <span data-ttu-id="16af2-223">Högerklicka på hello innehållsmappen och på **Lägg till**, och klicka sedan på **nytt objekt...** .</span><span class="sxs-lookup"><span data-stu-id="16af2-223">Right-click hello Content folder and click **Add**, and then click **New Item...**.</span></span>
   
    ![Lägg till formatmall innehållsmappen snabbmenyn][addcode005]
4. <span data-ttu-id="16af2-225">I hello **Lägg till nytt objekt** dialogrutan Ange **Style** i hello övre högra sökrutan och väljer sedan **formatmall**.</span><span class="sxs-lookup"><span data-stu-id="16af2-225">In hello **Add New Item** dialog box, enter **Style** in hello upper right search box and then select **Style Sheet**.</span></span>
    <span data-ttu-id="16af2-226">![Lägg till nytt objekt-dialogruta][rxStyle]</span><span class="sxs-lookup"><span data-stu-id="16af2-226">![Add New Item dialog box][rxStyle]</span></span>
5. <span data-ttu-id="16af2-227">Namnet hello filen *Contacts.css* och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-227">Name hello file *Contacts.css* and click **Add**.</span></span> <span data-ttu-id="16af2-228">Ersätt hello innehållet i hello-fil med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="16af2-228">Replace hello contents of hello file with hello following code.</span></span>
   
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }
   
    <span data-ttu-id="16af2-229">Vi använder den här formatmallen för hello layout, färger och format som används i hello kontakta manager app.</span><span class="sxs-lookup"><span data-stu-id="16af2-229">We will use this style sheet for hello layout, colors and styles used in hello contact manager app.</span></span>
6. <span data-ttu-id="16af2-230">Öppna hello *App_Start\BundleConfig.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="16af2-230">Open hello *App_Start\BundleConfig.cs* file.</span></span>
7. <span data-ttu-id="16af2-231">Lägg till följande kod tooregister hello hello [blockerade](http://knockoutjs.com/index.html "KO") plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="16af2-231">Add hello following code tooregister hello [Knockout](http://knockoutjs.com/index.html "KO") plugin.</span></span>
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    <span data-ttu-id="16af2-232">Det här exemplet använder blockerade toosimplify dynamiska JavaScript-kod som hanterar mallar för hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="16af2-232">This sample using knockout toosimplify dynamic JavaScript code that handles hello screen templates.</span></span>
8. <span data-ttu-id="16af2-233">Ändra hello innehållet/css post tooregister hello *contacts.css* formatmall.</span><span class="sxs-lookup"><span data-stu-id="16af2-233">Modify hello contents/css entry tooregister hello *contacts.css* style sheet.</span></span> <span data-ttu-id="16af2-234">Ändra hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="16af2-234">Change hello following line:</span></span>
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   <span data-ttu-id="16af2-235">Så här:</span><span class="sxs-lookup"><span data-stu-id="16af2-235">To:</span></span>
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. <span data-ttu-id="16af2-236">Kör hello efter kommandot tooinstall blockering i hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="16af2-236">In hello Package Manager Console, run hello following command tooinstall Knockout.</span></span>
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a><span data-ttu-id="16af2-237">Lägga till en styrenhet för hello Web API Restful-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="16af2-237">Add a controller for hello Web API Restful interface</span></span>
1. <span data-ttu-id="16af2-238">I **Solution Explorer**, högerklicka på domänkontrollanter och klickar på **Lägg till** och sedan **domänkontrollant...**</span><span class="sxs-lookup"><span data-stu-id="16af2-238">In **Solution Explorer**, right-click Controllers and click **Add** and then **Controller....**</span></span> 
2. <span data-ttu-id="16af2-239">I hello **Lägg till Kodskelett** dialogrutan Ange **Web API 2-styrenhet med åtgärder, med hjälp av Entity Framework** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-239">In hello **Add Scaffold** dialog box, enter **Web API 2 Controller with actions, using Entity Framework** and then click **Add**.</span></span>
   
    ![Lägga till API-styrenhet](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. <span data-ttu-id="16af2-241">I hello **Lägg till styrenhet** dialogrutan Ange ”ContactsController” som domänkontrollantens namn.</span><span class="sxs-lookup"><span data-stu-id="16af2-241">In hello **Add Controller** dialog box, enter "ContactsController" as your controller name.</span></span> <span data-ttu-id="16af2-242">Välj ”kontakta (ContactManager.Models)” för hello **Modellklass**.</span><span class="sxs-lookup"><span data-stu-id="16af2-242">Select "Contact (ContactManager.Models)" for hello **Model class**.</span></span>  <span data-ttu-id="16af2-243">Behåll hello standardvärdet för hello **datakontextklass**.</span><span class="sxs-lookup"><span data-stu-id="16af2-243">Keep hello default value for hello **Data context class**.</span></span> 
4. <span data-ttu-id="16af2-244">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-244">Click **Add**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="16af2-245">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="16af2-245">Run hello application locally</span></span>
1. <span data-ttu-id="16af2-246">Tryck på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="16af2-246">Press CTRL+F5 toorun hello application.</span></span>
   
    ![Sidan Index][intro001]
2. <span data-ttu-id="16af2-248">Ange en kontakt och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16af2-248">Enter a contact and click **Add**.</span></span> <span data-ttu-id="16af2-249">hello app returnerar toohello startsidan och visar hello-kontakt som du har angett.</span><span class="sxs-lookup"><span data-stu-id="16af2-249">hello app returns toohello home page and displays hello contact you entered.</span></span>
   
    ![Sidan index med uppgiften listobjekt][addwebapi004]
3. <span data-ttu-id="16af2-251">Lägg till i hello webbläsare, **/api/contacts** toohello URL.</span><span class="sxs-lookup"><span data-stu-id="16af2-251">In hello browser, append **/api/contacts** toohello URL.</span></span>
   
    <span data-ttu-id="16af2-252">hello resulterande URL: en ska likna http://localhost:1234-api-kontakter.</span><span class="sxs-lookup"><span data-stu-id="16af2-252">hello resulting URL will resemble http://localhost:1234/api/contacts.</span></span> <span data-ttu-id="16af2-253">hello RESTful-webb-API som du har lagt till returnerar hello lagras kontakter.</span><span class="sxs-lookup"><span data-stu-id="16af2-253">hello RESTful web API you added returns hello stored contacts.</span></span> <span data-ttu-id="16af2-254">Firefox och Chrome visar hello data i XML-format.</span><span class="sxs-lookup"><span data-stu-id="16af2-254">Firefox and Chrome will display hello data in XML format.</span></span>
   
    ![Sidan index med uppgiften listobjekt][rxFFchrome]

    <span data-ttu-id="16af2-256">Internet Explorer ska fråga tooopen eller spara hello kontakter.</span><span class="sxs-lookup"><span data-stu-id="16af2-256">IE will prompt you tooopen or save hello contacts.</span></span>

    ![Webb-API spara dialogrutan][addwebapi006]


    <span data-ttu-id="16af2-258">Du kan öppna hello returneras kontakter i anteckningar eller en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="16af2-258">You can open hello returned contacts in notepad or a browser.</span></span>

    <span data-ttu-id="16af2-259">Den här utdatan kan användas av ett annat program, till exempel mobila webbsida eller ett program.</span><span class="sxs-lookup"><span data-stu-id="16af2-259">This output can be consumed by another application such as mobile web page or application.</span></span>

    ![Webb-API spara dialogrutan][addwebapi007]

    <span data-ttu-id="16af2-261">**Säkerhetsvarning**: nu är ditt program är osäker och sårbar tooCSRF attack.</span><span class="sxs-lookup"><span data-stu-id="16af2-261">**Security Warning**: At this point, your application is insecure and vulnerable tooCSRF attack.</span></span> <span data-ttu-id="16af2-262">Senare i självstudiekursen hello vi ta bort den här säkerhetsrisken.</span><span class="sxs-lookup"><span data-stu-id="16af2-262">Later in hello tutorial we will remove this vulnerability.</span></span> <span data-ttu-id="16af2-263">Mer information finns i [hindrar webbplatser begära förfalskning (CSRF) attacker][prevent-csrf-attacks].</span><span class="sxs-lookup"><span data-stu-id="16af2-263">For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].</span></span>
## <a name="add-xsrf-protection"></a><span data-ttu-id="16af2-264">Lägga till XSRF skydd</span><span class="sxs-lookup"><span data-stu-id="16af2-264">Add XSRF Protection</span></span>
<span data-ttu-id="16af2-265">Begäran förfalskning (även kallat XSRF eller CSRF) är en attack mot webbaserat program där en skadlig webbplats kan påverka hello interaktionen mellan en klientens webbläsare och en webbplats som är betrodda av den webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="16af2-265">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted applications whereby a malicious website can influence hello interaction between a client browser and a website trusted by that browser.</span></span> <span data-ttu-id="16af2-266">Dessa attacker är möjligt eftersom webbläsare skickar autentiseringstoken automatiskt med varje begäran tooa webbplats.</span><span class="sxs-lookup"><span data-stu-id="16af2-266">These attacks are made possible because web browsers will send authentication tokens automatically with every request tooa website.</span></span> <span data-ttu-id="16af2-267">hello kanoniska exempel är en autentiseringscookie, till exempel ASP. NETS biljetten för formulär för autentisering.</span><span class="sxs-lookup"><span data-stu-id="16af2-267">hello canonical example is an authentication cookie, such as ASP.NET's Forms Authentication ticket.</span></span> <span data-ttu-id="16af2-268">Webbplatser som använder alla beständiga autentiseringsmekanism (till exempel Windows-autentisering, grundläggande och så vidare) kan vara mål av angrepp.</span><span class="sxs-lookup"><span data-stu-id="16af2-268">However, websites which use any persistent authentication mechanism (such as Windows Authentication, Basic, and so forth) can be targeted by these attacks.</span></span>

<span data-ttu-id="16af2-269">En attack XSRF skiljer sig från en phishing-attacker.</span><span class="sxs-lookup"><span data-stu-id="16af2-269">An XSRF attack is distinct from a phishing attack.</span></span> <span data-ttu-id="16af2-270">Nätfiskeattacker kräver interaktion från hello drabbade.</span><span class="sxs-lookup"><span data-stu-id="16af2-270">Phishing attacks require interaction from hello victim.</span></span> <span data-ttu-id="16af2-271">En skadlig webbplats efterliknar hello målwebbplats i en phishing-attacker och hello drabbade lurad till att tillhandahålla känslig information toohello angripare.</span><span class="sxs-lookup"><span data-stu-id="16af2-271">In a phishing attack, a malicious website will mimic hello target website, and hello victim is fooled into providing sensitive information toohello attacker.</span></span> <span data-ttu-id="16af2-272">I en XSRF-attack krävs det ofta utan interaktion från hello drabbade.</span><span class="sxs-lookup"><span data-stu-id="16af2-272">In an XSRF attack, there is often no interaction necessary from hello victim.</span></span> <span data-ttu-id="16af2-273">I stället förlitande hello angripare hello webbläsare som automatiskt skickar alla relevanta cookies toohello mål webbplats.</span><span class="sxs-lookup"><span data-stu-id="16af2-273">Rather, hello attacker is relying on hello browser automatically sending all relevant cookies toohello destination website.</span></span>

<span data-ttu-id="16af2-274">Mer information finns i hello [säkerhet öppna Webbapprojektet](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span><span class="sxs-lookup"><span data-stu-id="16af2-274">For more information, see hello [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span></span>

1. <span data-ttu-id="16af2-275">I **Solution Explorer**, höger **ContactManager** projektet och klicka på **Lägg till** och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="16af2-275">In **Solution Explorer**, right **ContactManager** project and click **Add** and then click **Class**.</span></span>
2. <span data-ttu-id="16af2-276">Namnet hello filen *ValidateHttpAntiForgeryTokenAttribute.cs* och Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="16af2-276">Name hello file *ValidateHttpAntiForgeryTokenAttribute.cs* and add hello following code:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }
3. <span data-ttu-id="16af2-277">Lägg till följande hello *med* instruktionen toohello kontrakt domänkontrollant så att du har åtkomst toohello **[ValidateHttpAntiForgeryToken]** attribut.</span><span class="sxs-lookup"><span data-stu-id="16af2-277">Add hello following *using* statement toohello contracts controller so you have access toohello **[ValidateHttpAntiForgeryToken]** attribute.</span></span>
   
        using ContactManager.Filters;
4. <span data-ttu-id="16af2-278">Lägg till hello **[ValidateHttpAntiForgeryToken]** attributet toohello Post-metoderna för hello **ContactsController** tooprotect från XSRF hot.</span><span class="sxs-lookup"><span data-stu-id="16af2-278">Add hello **[ValidateHttpAntiForgeryToken]** attribute toohello Post methods of hello **ContactsController** tooprotect it from XSRF threats.</span></span> <span data-ttu-id="16af2-279">Du lägger till det toohello ”PutContact”, ”PostContact” och **DeleteContact** åtgärdsmetoder.</span><span class="sxs-lookup"><span data-stu-id="16af2-279">You will add it toohello "PutContact",  "PostContact" and **DeleteContact** action methods.</span></span>
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. <span data-ttu-id="16af2-280">Uppdatera hello *skript* avsnitt i hello *Views\Home\Index.cshtml* filen tooinclude kod tooget hello XSRF token.</span><span class="sxs-lookup"><span data-stu-id="16af2-280">Update hello *Scripts* section of hello *Views\Home\Index.cshtml* file tooinclude code tooget hello XSRF tokens.</span></span>
   
         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }
   
               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {
   
                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });
   
                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
   
                     });
                  }
   
                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a><span data-ttu-id="16af2-281">Publicera hello programmet uppdatering tooAzure och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="16af2-281">Publish hello application update tooAzure and SQL Database</span></span>
<span data-ttu-id="16af2-282">toopublish hello programmet du upprepa hello proceduren du följt tidigare.</span><span class="sxs-lookup"><span data-stu-id="16af2-282">toopublish hello application, you repeat hello procedure you followed earlier.</span></span>

1. <span data-ttu-id="16af2-283">I **Solution Explorer**, högerklicka på hello projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="16af2-283">In **Solution Explorer**, right click hello project and select **Publish**.</span></span>
   
    ![Publicera][rxP]
2. <span data-ttu-id="16af2-285">Klicka på hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="16af2-285">Click hello **Settings** tab.</span></span>
3. <span data-ttu-id="16af2-286">Under **ContactsManagerContext(ContactsManagerContext)**, klicka på hello **v** ikonen toochange *Remote anslutningssträngen* toohello anslutningssträngen för hello kontakt databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-286">Under **ContactsManagerContext(ContactsManagerContext)**, click hello **v** icon toochange *Remote connection string* toohello connection string for hello contact database.</span></span> <span data-ttu-id="16af2-287">Klicka på **ContactDB**.</span><span class="sxs-lookup"><span data-stu-id="16af2-287">Click **ContactDB**.</span></span>
   
    ![Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. <span data-ttu-id="16af2-289">Kryssrutan för hello **köra Code First Migrations (körs vid programstart)**.</span><span class="sxs-lookup"><span data-stu-id="16af2-289">Check hello box for **Execute Code First Migrations (runs on application start)**.</span></span>
5. <span data-ttu-id="16af2-290">Klicka på **nästa** och klicka sedan på **Preview**.</span><span class="sxs-lookup"><span data-stu-id="16af2-290">Click **Next** and then click **Preview**.</span></span> <span data-ttu-id="16af2-291">Visual Studio visar en lista över hello-filer som lagts till eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="16af2-291">Visual Studio displays a list of hello files that will be added or updated.</span></span>
6. <span data-ttu-id="16af2-292">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="16af2-292">Click **Publish**.</span></span>
   <span data-ttu-id="16af2-293">När hello distributionen är klar öppnar toohello startsida hello programmet hello webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="16af2-293">After hello deployment completes, hello browser opens toohello home page of hello application.</span></span>
   
    ![Sidan index med inga kontakter][intro001]
   
    <span data-ttu-id="16af2-295">hello Visual Studio Publicera process som automatiskt konfigurerade hello anslutningssträngen i hello distribueras *Web.config* filen toopoint toohello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="16af2-295">hello Visual Studio publish process automatically configured hello connection string in hello deployed *Web.config* file toopoint toohello SQL database.</span></span> <span data-ttu-id="16af2-296">Det kan också konfigurerad Code First Migrations tooautomatically uppgradera hello toohello senaste databasversionen hello första gången hello program ansluter hello databasen efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="16af2-296">It also configured Code First Migrations tooautomatically upgrade hello database toohello latest version hello first time hello application accesses hello database after deployment.</span></span>
   
    <span data-ttu-id="16af2-297">På grund av den här konfigurationen Code First skapade hello databas genom att köra hello kod i hello **inledande** klass som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="16af2-297">As a result of this configuration, Code First created hello database by running hello code in hello **Initial** class that you created earlier.</span></span> <span data-ttu-id="16af2-298">Det gjorde hello första gången hello programmet försökte tooaccess hello databasen efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="16af2-298">It did this hello first time hello application tried tooaccess hello database after deployment.</span></span>
7. <span data-ttu-id="16af2-299">Ange en kontakt som du gjorde när du körde hello appen lokalt, tooverify databasen distributionen lyckades.</span><span class="sxs-lookup"><span data-stu-id="16af2-299">Enter a contact as you did when you ran hello app locally, tooverify that database deployment succeeded.</span></span>

<span data-ttu-id="16af2-300">När du ser att hello-objektet som du anger sparas och visas på hello kontakta manager-sidan, vet du har lagrats i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="16af2-300">When you see that hello item you enter is saved and appears on hello contact manager page, you know that it has been stored in hello database.</span></span>

![Sidan index med kontakter][addwebapi004]

<span data-ttu-id="16af2-302">hello programmet körs nu i hello molnet med hjälp av SQL-databas toostore sina data.</span><span class="sxs-lookup"><span data-stu-id="16af2-302">hello application is now running in hello cloud, using SQL Database toostore its data.</span></span> <span data-ttu-id="16af2-303">Ta bort när du är klar med att testa hello program i Azure.</span><span class="sxs-lookup"><span data-stu-id="16af2-303">After you finish testing hello application in Azure, delete it.</span></span> <span data-ttu-id="16af2-304">hello programmet är offentlig och har inte en mekanism toolimit åtkomst.</span><span class="sxs-lookup"><span data-stu-id="16af2-304">hello application is public and doesn't have a mechanism toolimit access.</span></span>

> [!NOTE]
> <span data-ttu-id="16af2-305">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="16af2-305">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="16af2-306">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="16af2-306">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="16af2-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16af2-307">Next Steps</span></span>
<span data-ttu-id="16af2-308">Ett annat sätt toostore data i ett Azure-program är toouse Azure-lagring som innehåller icke-relationella datalagring i hello form av blobbar och tabeller.</span><span class="sxs-lookup"><span data-stu-id="16af2-308">Another way toostore data in an Azure application is toouse Azure storage, which provide non-relational data storage in hello form of blobs and tables.</span></span> <span data-ttu-id="16af2-309">hello följande länkar ger mer information om Web API, ASP.NET MVC och Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="16af2-309">hello following links provide more information on Web API, ASP.NET MVC and Window Azure.</span></span>

* <span data-ttu-id="16af2-310">[Komma igång med Entity Framework med MVC][EFCodeFirstMVCTutorial]</span><span class="sxs-lookup"><span data-stu-id="16af2-310">[Getting Started with Entity Framework using MVC][EFCodeFirstMVCTutorial]</span></span>
* [<span data-ttu-id="16af2-311">Introduktion tooASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="16af2-311">Intro tooASP.NET MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="16af2-312">Din första ASP.NET-webb-API</span><span class="sxs-lookup"><span data-stu-id="16af2-312">Your First ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [<span data-ttu-id="16af2-313">Felsökning av WAWS</span><span class="sxs-lookup"><span data-stu-id="16af2-313">Debugging WAWS</span></span>](web-sites-dotnet-troubleshoot-visual-studio.md)

<span data-ttu-id="16af2-314">Den här kursen och hello exempelprogrammet har skrivits av [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) med hjälp av Tom Dykstra och Barry Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)).</span><span class="sxs-lookup"><span data-stu-id="16af2-314">This tutorial and hello sample application was written by [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) with assistance from Tom Dykstra and Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)).</span></span> 

<span data-ttu-id="16af2-315">Lämna feedback på vad du tyckte om, eller vad du vill toosee förbättras inte bara om hello kursen själva, utan även om hello produkter som visar den.</span><span class="sxs-lookup"><span data-stu-id="16af2-315">Please leave feedback on what you liked or what you would like toosee improved, not only about hello tutorial itself but also about hello products that it demonstrates.</span></span> <span data-ttu-id="16af2-316">Din feedback hjälper oss att prioritera förbättringar.</span><span class="sxs-lookup"><span data-stu-id="16af2-316">Your feedback will help us prioritize improvements.</span></span> <span data-ttu-id="16af2-317">Vi kan särskilt intresserad av att ta reda på hur mycket intresse det är i flera automation för hello process för att konfigurera och distribuera hello medlemskap i databasen.</span><span class="sxs-lookup"><span data-stu-id="16af2-317">We are especially interested in finding out how much interest there is in more automation for hello process of configuring and deploying hello membership database.</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="16af2-318">Nyheter</span><span class="sxs-lookup"><span data-stu-id="16af2-318">What's changed</span></span>
* <span data-ttu-id="16af2-319">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="16af2-319">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles toohello Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update hello Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks

