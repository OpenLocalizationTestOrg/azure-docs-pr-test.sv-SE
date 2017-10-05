---
title: Skapa ett REST-API i Azure med ASP.NET och SQL DB | Microsoft Docs
description: "En självstudiekurs som lär du dig hur du distribuerar en app som använder ASP.NET Web API till en Azure-webbapp med hjälp av Visual Studio."
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
ms.openlocfilehash: 64c18f2cfabbb7af6ffd89b4c2a9095fca1cf799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a><span data-ttu-id="71656-103">Skapa ett REST-tjänst som använder ASP.NET Web API och SQL-databas i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="71656-103">Create a REST service using ASP.NET Web API and SQL Database in Azure App Service</span></span>
<span data-ttu-id="71656-104">Den här kursen visar hur du distribuerar ett ASP.NET-webbprogram till en [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) med hjälp av guiden Publicera webbplats i Visual Studio 2013 eller Visual Studio 2013 Community Edition.</span><span class="sxs-lookup"><span data-stu-id="71656-104">This tutorial shows how to deploy an ASP.NET web app to an [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by using the Publish Web wizard in Visual Studio 2013 or Visual Studio 2013 Community Edition.</span></span> 

<span data-ttu-id="71656-105">Du kan öppna ett Azure-konto gratis och om du inte redan har Visual Studio 2013, SDK installerar Visual Studio 2013 automatiskt för Web Express.</span><span class="sxs-lookup"><span data-stu-id="71656-105">You can open an Azure account for free, and if you don't already have Visual Studio 2013, the SDK automatically installs Visual Studio 2013 for Web Express.</span></span> <span data-ttu-id="71656-106">Så kan du börja utveckla för Azure och helt kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="71656-106">So you can start developing for Azure entirely for free.</span></span>

<span data-ttu-id="71656-107">Den här kursen förutsätter att du har några tidigare erfarenheter av att använda Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-107">This tutorial assumes that you have no prior experience using Azure.</span></span> <span data-ttu-id="71656-108">På den här kursen har du en enkel webbapp upp och körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="71656-108">On completing this tutorial, you'll have a simple web app up and running in the cloud.</span></span>

<span data-ttu-id="71656-109">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="71656-109">You'll learn:</span></span>

* <span data-ttu-id="71656-110">Hur du aktiverar datorn för Azure-utveckling genom att installera Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="71656-110">How to enable your machine for Azure development by installing the Azure SDK.</span></span>
* <span data-ttu-id="71656-111">Hur du skapar ett projekt i Visual Studio ASP.NET MVC 5 och publicera det till en Azure-app.</span><span class="sxs-lookup"><span data-stu-id="71656-111">How to create a Visual Studio ASP.NET MVC 5 project and publish it to an Azure app.</span></span>
* <span data-ttu-id="71656-112">Hur du använder ASP.NET Web API för att aktivera Restful-API-anrop.</span><span class="sxs-lookup"><span data-stu-id="71656-112">How to use the ASP.NET Web API to enable Restful API calls.</span></span>
* <span data-ttu-id="71656-113">Hur du använder en SQL-databas för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-113">How to use a SQL database to store data in Azure.</span></span>
* <span data-ttu-id="71656-114">Hur du publicerar program uppdateras till Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-114">How to publish application updates to Azure.</span></span>

<span data-ttu-id="71656-115">Du måste skapa ett enkelt kontaktlista webbprogram som bygger på ASP.NET MVC 5 och använder ADO.NET Entity Framework för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-115">You'll build a simple contact list web application that is built on ASP.NET MVC 5 and uses the ADO.NET Entity Framework for database access.</span></span> <span data-ttu-id="71656-116">Följande bild visar det färdiga programmet:</span><span class="sxs-lookup"><span data-stu-id="71656-116">The following illustration shows the completed application:</span></span>

![Skärmbild av webbplats][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-the-project"></a><span data-ttu-id="71656-118">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="71656-118">Create the project</span></span>
1. <span data-ttu-id="71656-119">Starta Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="71656-119">Start Visual Studio 2013.</span></span>
2. <span data-ttu-id="71656-120">Från den **filen** Klicka **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="71656-120">From the **File** menu click **New Project**.</span></span>
3. <span data-ttu-id="71656-121">I den **nytt projekt** dialogrutan Expandera **Visual C#** och välj **Web** och välj sedan **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="71656-121">In the **New Project** dialog box, expand **Visual C#** and select **Web**  and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="71656-122">Ge programmet namnet **ContactManager** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="71656-122">Name the application **ContactManager** and click **OK**.</span></span>
   
    ![Dialogrutan Nytt projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. <span data-ttu-id="71656-124">I den **nytt ASP.NET-projekt** dialogrutan markerar du den **MVC** mall, kontrollera **Web API** och klicka sedan på **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="71656-124">In the **New ASP.NET Project** dialog box, select the **MVC** template, check **Web API** and then click **Change Authentication**.</span></span>
5. <span data-ttu-id="71656-125">I dialogrutan **Ändra autentisering** klickar du på **Ingen autentisering** och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="71656-125">In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span>
   
    ![Ingen autentisering](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    <span data-ttu-id="71656-127">Det exempelprogram som du skapar inte funktioner som kräver att användare kan logga in.</span><span class="sxs-lookup"><span data-stu-id="71656-127">The sample application you're creating won't have features that require users to log in.</span></span> <span data-ttu-id="71656-128">Information om hur du implementerar autentisering och auktorisering funktioner finns i [nästa steg](#nextsteps) avsnittet i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="71656-128">For information about how to implement authentication and authorization features, see the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span> 
6. <span data-ttu-id="71656-129">I den **nytt ASP.NET-projekt** dialogrutan Kontrollera den **värd i molnet** är markerad och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="71656-129">In the **New ASP.NET Project** dialog box, make sure the **Host in the Cloud** is checked and click **OK**.</span></span>

<span data-ttu-id="71656-130">Om du redan inte har registrerat till Azure, uppmanas du att logga in.</span><span class="sxs-lookup"><span data-stu-id="71656-130">If you have not previously signed in to Azure, you will be prompted to sign in.</span></span>

1. <span data-ttu-id="71656-131">Guiden för konfiguration av föreslås ett unikt namn baserat på *ContactManager* (se bilden nedan).</span><span class="sxs-lookup"><span data-stu-id="71656-131">The configuration wizard will suggest a unique name based on *ContactManager* (see the image below).</span></span> <span data-ttu-id="71656-132">Välj en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="71656-132">Select a region near you.</span></span> <span data-ttu-id="71656-133">Du kan använda [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") att hitta den lägsta svarstid datacentralen.</span><span class="sxs-lookup"><span data-stu-id="71656-133">You can use [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") to find the lowest latency data center.</span></span> 
2. <span data-ttu-id="71656-134">Om du inte har skapat en databasserver innan du väljer **Skapa ny server**, ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="71656-134">If you haven't created a database server before, select **Create new server**, enter a database user name and password.</span></span>
   
    ![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

<span data-ttu-id="71656-136">Om du har en databasserver, använder du den och skapa en ny databas.</span><span class="sxs-lookup"><span data-stu-id="71656-136">If you have a database server, use that to create a new database.</span></span> <span data-ttu-id="71656-137">Databasservrar är en värdefull resurs och du normalt vill skapa flera databaser på samma server för testning och utveckling istället för att skapa en databasserver per databas.</span><span class="sxs-lookup"><span data-stu-id="71656-137">Database servers are a precious resource, and you generally want to create multiple databases on the same server for testing and development rather than creating a database server per database.</span></span> <span data-ttu-id="71656-138">Kontrollera att webbplatsen och databasen är i samma region.</span><span class="sxs-lookup"><span data-stu-id="71656-138">Make sure your web site and database are in the same region.</span></span>

![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-the-page-header-and-footer"></a><span data-ttu-id="71656-140">Ange sidhuvud och sidfot</span><span class="sxs-lookup"><span data-stu-id="71656-140">Set the page header and footer</span></span>
1. <span data-ttu-id="71656-141">I **Solution Explorer**, expandera den *Views\Shared* mapp och öppna den *_Layout.cshtml* fil.</span><span class="sxs-lookup"><span data-stu-id="71656-141">In **Solution Explorer**, expand the *Views\Shared* folder and open the *_Layout.cshtml* file.</span></span>
   
    ![_Layout.cshtml i Solution Explorer][newapp004]
2. <span data-ttu-id="71656-143">Ersätt innehållet i den *Views\Shared_Layout.cshtml* med följande kod:</span><span class="sxs-lookup"><span data-stu-id="71656-143">Replace the contents of the *Views\Shared_Layout.cshtml* file with the following code:</span></span>

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

<span data-ttu-id="71656-144">Koden ovan ändrar namnet på appen från ”mitt ASP.NET-program” till ”Kontakta Manager” och det tar bort länkar till **Start**, **om** och **Kontakta**.</span><span class="sxs-lookup"><span data-stu-id="71656-144">The markup above changes the app name from "My ASP.NET App" to "Contact Manager", and it removes the links to **Home**, **About** and **Contact**.</span></span>

### <a name="run-the-application-locally"></a><span data-ttu-id="71656-145">Kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="71656-145">Run the application locally</span></span>
1. <span data-ttu-id="71656-146">Tryck på CTRL+F5 för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="71656-146">Press CTRL+F5 to run the application.</span></span>
   <span data-ttu-id="71656-147">Med programmets startsida visas i standardwebbläsaren.</span><span class="sxs-lookup"><span data-stu-id="71656-147">The application home page appears in the default browser.</span></span>
    <span data-ttu-id="71656-148">![Att göra-lista startsidan](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span><span class="sxs-lookup"><span data-stu-id="71656-148">![To Do List home page](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span></span>

<span data-ttu-id="71656-149">Det här är allt du behöver göra nu att skapa program som du ska distribuera till Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-149">This is all you need to do for now to create the application that you'll deploy to Azure.</span></span> <span data-ttu-id="71656-150">Senare ska du lägga till databasfunktioner.</span><span class="sxs-lookup"><span data-stu-id="71656-150">Later you'll add database functionality.</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="71656-151">Distribuera programmet till Azure</span><span class="sxs-lookup"><span data-stu-id="71656-151">Deploy the application to Azure</span></span>
1. <span data-ttu-id="71656-152">I Visual Studio högerklickar du på projektet i **Solution Explorer** och välj **publicera** på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="71656-152">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
   
    ![Publicera i snabbmenyn för projektet][PublishVSSolution]
   
    <span data-ttu-id="71656-154">Den **Publicera webbplats** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="71656-154">The **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="71656-155">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="71656-155">Click **Publish**.</span></span>

![Fliken Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

<span data-ttu-id="71656-157">Visual Studio börjar filerna kopieras till Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="71656-157">Visual Studio begins the process of copying the files to the Azure server.</span></span> <span data-ttu-id="71656-158">Den **utdata** fönster visar vilka distributionsåtgärder som utförts och rapporterar slutförande av distributionen.</span><span class="sxs-lookup"><span data-stu-id="71656-158">The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.</span></span>

1. <span data-ttu-id="71656-159">Standardwebbläsare öppnas automatiskt URL för den distribuerade webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="71656-159">The default browser automatically opens to the URL of the deployed site.</span></span>
   
   <span data-ttu-id="71656-160">Programmet som du skapade körs nu i molnet.</span><span class="sxs-lookup"><span data-stu-id="71656-160">The application you created is now running in the cloud.</span></span>
   
   ![Att göra-lista startsidan körs i Azure][rxz2]

## <a name="add-a-database-to-the-application"></a><span data-ttu-id="71656-162">Lägga till en databas för programmet</span><span class="sxs-lookup"><span data-stu-id="71656-162">Add a database to the application</span></span>
<span data-ttu-id="71656-163">Därefter ska du uppdatera MVC-program för att lägga till möjligheten att visa och uppdatera kontakter och lagra data i en databas.</span><span class="sxs-lookup"><span data-stu-id="71656-163">Next, you'll update the MVC application to add the ability to display and update contacts and store the data in a database.</span></span> <span data-ttu-id="71656-164">Programmet använder Entity Framework för att skapa databasen och för att läsa och uppdatera data i databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-164">The application will use the Entity Framework to create the database and to read and update data in the database.</span></span>

### <a name="add-data-model-classes-for-the-contacts"></a><span data-ttu-id="71656-165">Lägg till modellen dataklasser för kontakter</span><span class="sxs-lookup"><span data-stu-id="71656-165">Add data model classes for the contacts</span></span>
<span data-ttu-id="71656-166">Börjar du med att skapa en enkel datamodell i kod.</span><span class="sxs-lookup"><span data-stu-id="71656-166">You begin by creating a simple data model in code.</span></span>

1. <span data-ttu-id="71656-167">I **Solution Explorer**, högerklicka på mappen modeller, klicka på **Lägg till**, och sedan **klassen**.</span><span class="sxs-lookup"><span data-stu-id="71656-167">In **Solution Explorer**, right-click the Models folder, click **Add**, and then **Class**.</span></span>
   
    ![Lägg till klass i modeller mappen snabbmenyn][adddb001]
2. <span data-ttu-id="71656-169">I den **Lägg till nytt objekt** dialogrutan namn på den nya klassfilen *Contact.cs*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-169">In the **Add New Item** dialog box, name the new class file *Contact.cs*, and then click **Add**.</span></span>
   
    ![Lägg till nytt objekt-dialogruta][adddb002]
3. <span data-ttu-id="71656-171">Ersätt innehållet i filen Contacts.cs med följande kod.</span><span class="sxs-lookup"><span data-stu-id="71656-171">Replace the contents of the Contacts.cs file with the following code.</span></span>
   
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

<span data-ttu-id="71656-172">Den **Kontakta** klassen definierar de data som ska lagras för varje kontakt, plus en primär nyckel, kontakt ID som krävs av databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-172">The **Contact** class defines the data that you will store for each contact, plus a primary key, ContactID, that is needed by the database.</span></span> <span data-ttu-id="71656-173">Du kan få mer information om datamodeller i den [nästa steg](#nextsteps) avsnittet i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="71656-173">You can get more information about data models in the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span>

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a><span data-ttu-id="71656-174">Skapa webbsidor som gör appanvändare kan arbeta med kontakter</span><span class="sxs-lookup"><span data-stu-id="71656-174">Create web pages that enable app users to work with the contacts</span></span>
<span data-ttu-id="71656-175">ASP.NET MVC funktionen scaffold-teknik kan automatiskt generera kod som utför skapa, läsa, uppdatera och ta bort åtgärder (CRUD).</span><span class="sxs-lookup"><span data-stu-id="71656-175">The ASP.NET MVC the scaffolding feature can automatically generate code that performs create, read, update, and delete (CRUD) actions.</span></span>

## <a name="add-a-controller-and-a-view-for-the-data"></a><span data-ttu-id="71656-176">Lägg till en domänkontrollant och en vy för data</span><span class="sxs-lookup"><span data-stu-id="71656-176">Add a Controller and a view for the data</span></span>
1. <span data-ttu-id="71656-177">I **Solution Explorer**, expandera mappen domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="71656-177">In **Solution Explorer**, expand the Controllers folder.</span></span>
2. <span data-ttu-id="71656-178">Skapa projektet **(Ctrl + Skift + B)**.</span><span class="sxs-lookup"><span data-stu-id="71656-178">Build the project **(Ctrl+Shift+B)**.</span></span> <span data-ttu-id="71656-179">(Du måste skapa projektet innan du använder scaffold-teknik mekanism.)</span><span class="sxs-lookup"><span data-stu-id="71656-179">(You must build the project before using scaffolding mechanism.)</span></span> 
3. <span data-ttu-id="71656-180">Högerklicka på mappen domänkontrollanter och klicka på **Lägg till**, och klicka sedan på **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="71656-180">Right-click the Controllers folder and click **Add**, and then click **Controller**.</span></span>
   
    ![Lägga till domänkontrollanter i snabbmenyn för domänkontrollanter mapp][addcode001]
4. <span data-ttu-id="71656-182">I den **Lägg till Kodskelett** dialogrutan **MVC-enhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-182">In the **Add Scaffold** dialog box, select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
   
   ![Lägga till en kontrollant](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. <span data-ttu-id="71656-184">Ange namnet på domänkontrollanten **HomeController**.</span><span class="sxs-lookup"><span data-stu-id="71656-184">Set the controller name to **HomeController**.</span></span> <span data-ttu-id="71656-185">Välj **Kontakta** som modellklass.</span><span class="sxs-lookup"><span data-stu-id="71656-185">Select **Contact** as your model class.</span></span> <span data-ttu-id="71656-186">Klicka på den **nya datakontexten** knappen och acceptera standardvärdet ”ContactManager.Models.ContactManagerContext” för den **nya datakontexttyp**.</span><span class="sxs-lookup"><span data-stu-id="71656-186">Click the **New data context** button and accept the default "ContactManager.Models.ContactManagerContext" for the **New data context type**.</span></span> <span data-ttu-id="71656-187">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-187">Click **Add**.</span></span>

    <span data-ttu-id="71656-188">En dialogruta där du uppmanas: ”en fil med namnet HomeController redan.</span><span class="sxs-lookup"><span data-stu-id="71656-188">A dialog box will prompt you: "A file with the name HomeController already exits.</span></span> <span data-ttu-id="71656-189">Vill du ersätta den ”?.</span><span class="sxs-lookup"><span data-stu-id="71656-189">Do you want to replace it?".</span></span> <span data-ttu-id="71656-190">Klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="71656-190">Click **Yes**.</span></span> <span data-ttu-id="71656-191">Vi skriver över Start styrenheten som skapades med det nya projektet.</span><span class="sxs-lookup"><span data-stu-id="71656-191">We are overwriting the Home Controller that was created with the new project.</span></span> <span data-ttu-id="71656-192">Vi använder den nya Start-styrenheten för våra kontaktlista.</span><span class="sxs-lookup"><span data-stu-id="71656-192">We will use the new Home Controller for our contact list.</span></span>

    <span data-ttu-id="71656-193">Visual Studio skapar kontrollantmetoder och vyer för CRUD-åtgärder för databasen för **Kontakta** objekt.</span><span class="sxs-lookup"><span data-stu-id="71656-193">Visual Studio creates controller methods and views for CRUD database operations for **Contact** objects.</span></span>

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a><span data-ttu-id="71656-194">Aktivera migrering, skapa databasen, lägga till exempeldata och en data-initieraren</span><span class="sxs-lookup"><span data-stu-id="71656-194">Enable Migrations, create the database, add sample data and a data initializer</span></span>
<span data-ttu-id="71656-195">Nästa uppgift är att aktivera den [Code First Migrations](http://curah.microsoft.com/55220) funktion för att kunna skapa databasen baserat på den datamodell som du skapade.</span><span class="sxs-lookup"><span data-stu-id="71656-195">The next task is to enable the [Code First Migrations](http://curah.microsoft.com/55220) feature in order to create the database based on the data model you created.</span></span>

1. <span data-ttu-id="71656-196">I den **verktyg** väljer du **Library Package Manager** och sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="71656-196">In the **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>
   
    ![Package Manager-konsolen i Verktyg-menyn][addcode008]
2. <span data-ttu-id="71656-198">I den **Pakethanterarkonsolen** fönstret, anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="71656-198">In the **Package Manager Console** window, enter the following command:</span></span>
   
        enable-migrations 
   
    <span data-ttu-id="71656-199">Den **aktivera migreringar** kommando skapar en *migreringar* mapp och dess placeras i den mappen en *Configuration.cs* fil som du kan redigera för att konfigurera migreringar.</span><span class="sxs-lookup"><span data-stu-id="71656-199">The **enable-migrations** command creates a *Migrations* folder and it puts in that folder a *Configuration.cs* file that you can edit to configure Migrations.</span></span> 
3. <span data-ttu-id="71656-200">I den **Pakethanterarkonsolen** fönstret, anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="71656-200">In the **Package Manager Console** window, enter the following command:</span></span>
   
        add-migration Initial
   
    <span data-ttu-id="71656-201">Den **lägga till migrering inledande** kommandot genererar en klass som heter  **&lt;date_stamp&gt;inledande** som skapar databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-201">The **add-migration Initial** command generates a class named **&lt;date_stamp&gt;Initial** that creates the database.</span></span> <span data-ttu-id="71656-202">Den första parametern ( *inledande* ) är valfri och används för att skapa namn på filen.</span><span class="sxs-lookup"><span data-stu-id="71656-202">The first parameter ( *Initial* ) is arbitrary and used to create the name of the file.</span></span> <span data-ttu-id="71656-203">Du kan se de nya klassen filerna i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="71656-203">You can see the new class files in **Solution Explorer**.</span></span>
   
    <span data-ttu-id="71656-204">I den **inledande** klassen, de **in** metoden skapar tabellen kontakter och **ned** metod (används när du vill gå tillbaka till föregående tillstånd) släpper den.</span><span class="sxs-lookup"><span data-stu-id="71656-204">In the **Initial** class, the **Up** method creates the Contacts table, and the **Down** method (used when you want to return to the previous state) drops it.</span></span>
4. <span data-ttu-id="71656-205">Öppna den *Migrations\Configuration.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="71656-205">Open the *Migrations\Configuration.cs* file.</span></span> 
5. <span data-ttu-id="71656-206">Lägg till följande namnområden.</span><span class="sxs-lookup"><span data-stu-id="71656-206">Add the following namespaces.</span></span> 
   
         using ContactManager.Models;
6. <span data-ttu-id="71656-207">Ersätt den *Seed* metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="71656-207">Replace the *Seed* method with the following code:</span></span>
   
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
   
    <span data-ttu-id="71656-208">Den här koden ovan ska initiera databasen med kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="71656-208">This code above will initialize the database with the contact information.</span></span> <span data-ttu-id="71656-209">Mer information om seeding databasen finns [felsökning (EF Entity Framework) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span><span class="sxs-lookup"><span data-stu-id="71656-209">For more information on seeding the database, see [Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span></span>
7. <span data-ttu-id="71656-210">I den **Pakethanterarkonsolen** anger du kommandot:</span><span class="sxs-lookup"><span data-stu-id="71656-210">In the **Package Manager Console** enter the command:</span></span>
   
        update-database
   
    ![Package Manager-konsolen kommandon][addcode009]
   
    <span data-ttu-id="71656-212">Den **uppdatering databasen** körs den första migrering som skapar databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-212">The **update-database** runs the first migration which creates the database.</span></span> <span data-ttu-id="71656-213">Som standard skapas databasen som en SQL Server Express LocalDB-databas.</span><span class="sxs-lookup"><span data-stu-id="71656-213">By default, the database is created as a SQL Server Express LocalDB database.</span></span>
8. <span data-ttu-id="71656-214">Tryck på CTRL+F5 för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="71656-214">Press CTRL+F5 to run the application.</span></span> 

<span data-ttu-id="71656-215">Programmet visar seed-data och redigera, information och ta bort länkar.</span><span class="sxs-lookup"><span data-stu-id="71656-215">The application shows the seed data and provides edit, details and delete links.</span></span>

![MVC-vy av data][rxz3]

## <a name="edit-the-view"></a><span data-ttu-id="71656-217">Redigera vy</span><span class="sxs-lookup"><span data-stu-id="71656-217">Edit the View</span></span>
1. <span data-ttu-id="71656-218">Öppna den *Views\Home\Index.cshtml* fil.</span><span class="sxs-lookup"><span data-stu-id="71656-218">Open the *Views\Home\Index.cshtml* file.</span></span> <span data-ttu-id="71656-219">I nästa steg, kommer vi att ersätta den genererade koden med kod som använder [jQuery](http://jquery.com/) och [Knockout.js](http://knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="71656-219">In the next step, we will replace the generated markup with code that uses [jQuery](http://jquery.com/) and [Knockout.js](http://knockoutjs.com/).</span></span> <span data-ttu-id="71656-220">Den här nya koden hämtar listan över kontakter från att använda webb-API och JSON och kopplar sedan kontaktdata till Användargränssnittet med knockout.js.</span><span class="sxs-lookup"><span data-stu-id="71656-220">This new code retrieves the list of contacts from using web API and JSON and then binds the contact data to the UI using knockout.js.</span></span> <span data-ttu-id="71656-221">Mer information finns i [nästa steg](#nextsteps) avsnittet i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="71656-221">For more information, see the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span> 
2. <span data-ttu-id="71656-222">Ersätt innehållet i filen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="71656-222">Replace the contents of the file with the following code.</span></span>
   
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
3. <span data-ttu-id="71656-223">Högerklicka på mappen innehåll och klicka på **Lägg till**, och klicka sedan på **nytt objekt...** .</span><span class="sxs-lookup"><span data-stu-id="71656-223">Right-click the Content folder and click **Add**, and then click **New Item...**.</span></span>
   
    ![Lägg till formatmall innehållsmappen snabbmenyn][addcode005]
4. <span data-ttu-id="71656-225">I den **Lägg till nytt objekt** dialogrutan Ange **Style** i övre högra sökrutan och väljer sedan **formatmall**.</span><span class="sxs-lookup"><span data-stu-id="71656-225">In the **Add New Item** dialog box, enter **Style** in the upper right search box and then select **Style Sheet**.</span></span>
    <span data-ttu-id="71656-226">![Lägg till nytt objekt-dialogruta][rxStyle]</span><span class="sxs-lookup"><span data-stu-id="71656-226">![Add New Item dialog box][rxStyle]</span></span>
5. <span data-ttu-id="71656-227">Namn på filen *Contacts.css* och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-227">Name the file *Contacts.css* and click **Add**.</span></span> <span data-ttu-id="71656-228">Ersätt innehållet i filen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="71656-228">Replace the contents of the file with the following code.</span></span>
   
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
   
    <span data-ttu-id="71656-229">Vi använder den här formatmallen för layout, färger och format som används i appen kontakta manager.</span><span class="sxs-lookup"><span data-stu-id="71656-229">We will use this style sheet for the layout, colors and styles used in the contact manager app.</span></span>
6. <span data-ttu-id="71656-230">Öppna den *App_Start\BundleConfig.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="71656-230">Open the *App_Start\BundleConfig.cs* file.</span></span>
7. <span data-ttu-id="71656-231">Lägg till följande kod för att registrera den [blockerade](http://knockoutjs.com/index.html "KO") plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="71656-231">Add the following code to register the [Knockout](http://knockoutjs.com/index.html "KO") plugin.</span></span>
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    <span data-ttu-id="71656-232">Det här exemplet använder blockering för att förenkla dynamiska JavaScript-kod som hanterar skärmen mallar.</span><span class="sxs-lookup"><span data-stu-id="71656-232">This sample using knockout to simplify dynamic JavaScript code that handles the screen templates.</span></span>
8. <span data-ttu-id="71656-233">Ändra innehållet/css-post för att registrera den *contacts.css* formatmall.</span><span class="sxs-lookup"><span data-stu-id="71656-233">Modify the contents/css entry to register the *contacts.css* style sheet.</span></span> <span data-ttu-id="71656-234">Ändra följande rad:</span><span class="sxs-lookup"><span data-stu-id="71656-234">Change the following line:</span></span>
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   <span data-ttu-id="71656-235">Så här:</span><span class="sxs-lookup"><span data-stu-id="71656-235">To:</span></span>
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. <span data-ttu-id="71656-236">Kör följande kommando för att installera blockering i Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="71656-236">In the Package Manager Console, run the following command to install Knockout.</span></span>
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-the-web-api-restful-interface"></a><span data-ttu-id="71656-237">Lägga till en styrenhet för Web API Restful-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="71656-237">Add a controller for the Web API Restful interface</span></span>
1. <span data-ttu-id="71656-238">I **Solution Explorer**, högerklicka på domänkontrollanter och klickar på **Lägg till** och sedan **domänkontrollant...**</span><span class="sxs-lookup"><span data-stu-id="71656-238">In **Solution Explorer**, right-click Controllers and click **Add** and then **Controller....**</span></span> 
2. <span data-ttu-id="71656-239">I den **Lägg till Kodskelett** dialogrutan Ange **Web API 2-styrenhet med åtgärder, med hjälp av Entity Framework** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-239">In the **Add Scaffold** dialog box, enter **Web API 2 Controller with actions, using Entity Framework** and then click **Add**.</span></span>
   
    ![Lägga till API-styrenhet](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. <span data-ttu-id="71656-241">I den **Lägg till styrenhet** dialogrutan Ange ”ContactsController” som domänkontrollantens namn.</span><span class="sxs-lookup"><span data-stu-id="71656-241">In the **Add Controller** dialog box, enter "ContactsController" as your controller name.</span></span> <span data-ttu-id="71656-242">Välj ”kontakta (ContactManager.Models)” för den **Modellklass**.</span><span class="sxs-lookup"><span data-stu-id="71656-242">Select "Contact (ContactManager.Models)" for the **Model class**.</span></span>  <span data-ttu-id="71656-243">Behåll standardvärdet för den **datakontextklass**.</span><span class="sxs-lookup"><span data-stu-id="71656-243">Keep the default value for the **Data context class**.</span></span> 
4. <span data-ttu-id="71656-244">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-244">Click **Add**.</span></span>

### <a name="run-the-application-locally"></a><span data-ttu-id="71656-245">Kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="71656-245">Run the application locally</span></span>
1. <span data-ttu-id="71656-246">Tryck på CTRL+F5 för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="71656-246">Press CTRL+F5 to run the application.</span></span>
   
    ![Sidan Index][intro001]
2. <span data-ttu-id="71656-248">Ange en kontakt och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71656-248">Enter a contact and click **Add**.</span></span> <span data-ttu-id="71656-249">Appen skickar tillbaka till startsidan och visar den kontakt som du har angett.</span><span class="sxs-lookup"><span data-stu-id="71656-249">The app returns to the home page and displays the contact you entered.</span></span>
   
    ![Sidan index med uppgiften listobjekt][addwebapi004]
3. <span data-ttu-id="71656-251">Lägg till i webbläsaren, **/api/contacts** i URL: en.</span><span class="sxs-lookup"><span data-stu-id="71656-251">In the browser, append **/api/contacts** to the URL.</span></span>
   
    <span data-ttu-id="71656-252">Den resulterande URL: en ska likna http://localhost:1234-api-kontakter.</span><span class="sxs-lookup"><span data-stu-id="71656-252">The resulting URL will resemble http://localhost:1234/api/contacts.</span></span> <span data-ttu-id="71656-253">RESTful-webb-API som du har lagt till returnerar lagrade kontakter.</span><span class="sxs-lookup"><span data-stu-id="71656-253">The RESTful web API you added returns the stored contacts.</span></span> <span data-ttu-id="71656-254">Firefox och Chrome visas data i XML-format.</span><span class="sxs-lookup"><span data-stu-id="71656-254">Firefox and Chrome will display the data in XML format.</span></span>
   
    ![Sidan index med uppgiften listobjekt][rxFFchrome]

    <span data-ttu-id="71656-256">IE uppmanas du att öppna eller spara kontakter.</span><span class="sxs-lookup"><span data-stu-id="71656-256">IE will prompt you to open or save the contacts.</span></span>

    ![Webb-API spara dialogrutan][addwebapi006]


    <span data-ttu-id="71656-258">Du kan öppna returnerade kontakter i anteckningar eller en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="71656-258">You can open the returned contacts in notepad or a browser.</span></span>

    <span data-ttu-id="71656-259">Den här utdatan kan användas av ett annat program, till exempel mobila webbsida eller ett program.</span><span class="sxs-lookup"><span data-stu-id="71656-259">This output can be consumed by another application such as mobile web page or application.</span></span>

    ![Webb-API spara dialogrutan][addwebapi007]

    <span data-ttu-id="71656-261">**Säkerhetsvarning**: nu är ditt program är osäker och sårbar för angrepp CSRF.</span><span class="sxs-lookup"><span data-stu-id="71656-261">**Security Warning**: At this point, your application is insecure and vulnerable to CSRF attack.</span></span> <span data-ttu-id="71656-262">Vi tar bort den här säkerhetsrisken senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="71656-262">Later in the tutorial we will remove this vulnerability.</span></span> <span data-ttu-id="71656-263">Mer information finns i [hindrar webbplatser begära förfalskning (CSRF) attacker][prevent-csrf-attacks].</span><span class="sxs-lookup"><span data-stu-id="71656-263">For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].</span></span>
## <a name="add-xsrf-protection"></a><span data-ttu-id="71656-264">Lägga till XSRF skydd</span><span class="sxs-lookup"><span data-stu-id="71656-264">Add XSRF Protection</span></span>
<span data-ttu-id="71656-265">Begäran förfalskning (även kallat XSRF eller CSRF) är en attack mot webbaserat program där en skadlig webbplats kan påverka interaktionen mellan en klientens webbläsare och en webbplats som är betrodda av den webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="71656-265">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted applications whereby a malicious website can influence the interaction between a client browser and a website trusted by that browser.</span></span> <span data-ttu-id="71656-266">Dessa attacker är möjligt eftersom webbläsare skickar autentiseringstoken automatiskt med varje begäran till en webbplats.</span><span class="sxs-lookup"><span data-stu-id="71656-266">These attacks are made possible because web browsers will send authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="71656-267">Kanoniskt exempel är en autentiseringscookie, till exempel ASP. NETS biljetten för formulär för autentisering.</span><span class="sxs-lookup"><span data-stu-id="71656-267">The canonical example is an authentication cookie, such as ASP.NET's Forms Authentication ticket.</span></span> <span data-ttu-id="71656-268">Webbplatser som använder alla beständiga autentiseringsmekanism (till exempel Windows-autentisering, grundläggande och så vidare) kan vara mål av angrepp.</span><span class="sxs-lookup"><span data-stu-id="71656-268">However, websites which use any persistent authentication mechanism (such as Windows Authentication, Basic, and so forth) can be targeted by these attacks.</span></span>

<span data-ttu-id="71656-269">En attack XSRF skiljer sig från en phishing-attacker.</span><span class="sxs-lookup"><span data-stu-id="71656-269">An XSRF attack is distinct from a phishing attack.</span></span> <span data-ttu-id="71656-270">Nätfiskeattacker kräver interaktion från drabbade.</span><span class="sxs-lookup"><span data-stu-id="71656-270">Phishing attacks require interaction from the victim.</span></span> <span data-ttu-id="71656-271">En skadlig webbplats efterliknar målwebbplatsen i en phishing-attacker och offer lurad till att angriparen känslig information.</span><span class="sxs-lookup"><span data-stu-id="71656-271">In a phishing attack, a malicious website will mimic the target website, and the victim is fooled into providing sensitive information to the attacker.</span></span> <span data-ttu-id="71656-272">I en XSRF-attack krävs det ofta utan interaktion från drabbade.</span><span class="sxs-lookup"><span data-stu-id="71656-272">In an XSRF attack, there is often no interaction necessary from the victim.</span></span> <span data-ttu-id="71656-273">I stället förlitande angriparen i webbläsare som automatiskt skickar alla relevanta cookies till mål-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="71656-273">Rather, the attacker is relying on the browser automatically sending all relevant cookies to the destination website.</span></span>

<span data-ttu-id="71656-274">Mer information finns i [säkerhet öppna Webbapprojektet](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span><span class="sxs-lookup"><span data-stu-id="71656-274">For more information, see the [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span></span>

1. <span data-ttu-id="71656-275">I **Solution Explorer**, höger **ContactManager** projektet och klicka på **Lägg till** och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="71656-275">In **Solution Explorer**, right **ContactManager** project and click **Add** and then click **Class**.</span></span>
2. <span data-ttu-id="71656-276">Namn på filen *ValidateHttpAntiForgeryTokenAttribute.cs* och Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="71656-276">Name the file *ValidateHttpAntiForgeryTokenAttribute.cs* and add the following code:</span></span>
   
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
3. <span data-ttu-id="71656-277">Lägg till följande *med* instruktionen till kontrakt styrenheten så att du har åtkomst till den **[ValidateHttpAntiForgeryToken]** attribut.</span><span class="sxs-lookup"><span data-stu-id="71656-277">Add the following *using* statement to the contracts controller so you have access to the **[ValidateHttpAntiForgeryToken]** attribute.</span></span>
   
        using ContactManager.Filters;
4. <span data-ttu-id="71656-278">Lägg till den **[ValidateHttpAntiForgeryToken]** attributet Post-metoderna för den **ContactsController** att skydda mot XSRF hot.</span><span class="sxs-lookup"><span data-stu-id="71656-278">Add the **[ValidateHttpAntiForgeryToken]** attribute to the Post methods of the **ContactsController** to protect it from XSRF threats.</span></span> <span data-ttu-id="71656-279">Du kan lägga till den ”PutContact”, ”PostContact” och **DeleteContact** åtgärdsmetoder.</span><span class="sxs-lookup"><span data-stu-id="71656-279">You will add it to the "PutContact",  "PostContact" and **DeleteContact** action methods.</span></span>
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. <span data-ttu-id="71656-280">Uppdatering av *skript* avsnitt i den *Views\Home\Index.cshtml* filen för att inkludera kod för att hämta XSRF-token.</span><span class="sxs-lookup"><span data-stu-id="71656-280">Update the *Scripts* section of the *Views\Home\Index.cshtml* file to include code to get the XSRF tokens.</span></span>
   
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

## <a name="publish-the-application-update-to-azure-and-sql-database"></a><span data-ttu-id="71656-281">Publicera programuppdateringen till Azure och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="71656-281">Publish the application update to Azure and SQL Database</span></span>
<span data-ttu-id="71656-282">Om du vill publicera programmet måste upprepa du proceduren du följt tidigare.</span><span class="sxs-lookup"><span data-stu-id="71656-282">To publish the application, you repeat the procedure you followed earlier.</span></span>

1. <span data-ttu-id="71656-283">I **Solution Explorer**, högerklicka på projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="71656-283">In **Solution Explorer**, right click the project and select **Publish**.</span></span>
   
    ![Publicera][rxP]
2. <span data-ttu-id="71656-285">Klicka på fliken **Settings** (Inställningar).</span><span class="sxs-lookup"><span data-stu-id="71656-285">Click the **Settings** tab.</span></span>
3. <span data-ttu-id="71656-286">Under **ContactsManagerContext(ContactsManagerContext)**, klicka på den **v** ikon för att ändra *Remote anslutningssträngen* i anslutningssträngen för databasen kontakta.</span><span class="sxs-lookup"><span data-stu-id="71656-286">Under **ContactsManagerContext(ContactsManagerContext)**, click the **v** icon to change *Remote connection string* to the connection string for the contact database.</span></span> <span data-ttu-id="71656-287">Klicka på **ContactDB**.</span><span class="sxs-lookup"><span data-stu-id="71656-287">Click **ContactDB**.</span></span>
   
    ![Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. <span data-ttu-id="71656-289">Markera kryssrutan för **köra Code First Migrations (körs vid programstart)**.</span><span class="sxs-lookup"><span data-stu-id="71656-289">Check the box for **Execute Code First Migrations (runs on application start)**.</span></span>
5. <span data-ttu-id="71656-290">Klicka på **nästa** och klicka sedan på **Preview**.</span><span class="sxs-lookup"><span data-stu-id="71656-290">Click **Next** and then click **Preview**.</span></span> <span data-ttu-id="71656-291">Visual Studio visar en lista över de filer som lagts till eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="71656-291">Visual Studio displays a list of the files that will be added or updated.</span></span>
6. <span data-ttu-id="71656-292">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="71656-292">Click **Publish**.</span></span>
   <span data-ttu-id="71656-293">När distributionen är klar öppnas i webbläsaren startsidan för programmet.</span><span class="sxs-lookup"><span data-stu-id="71656-293">After the deployment completes, the browser opens to the home page of the application.</span></span>
   
    ![Sidan index med inga kontakter][intro001]
   
    <span data-ttu-id="71656-295">Visual Studio publiceringsprocess automatiskt konfigurerade anslutningssträngen i den distribuerade *Web.config* fil att peka till SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-295">The Visual Studio publish process automatically configured the connection string in the deployed *Web.config* file to point to the SQL database.</span></span> <span data-ttu-id="71656-296">Den kan också konfigureras Code First Migrations för att automatiskt uppgradera databasen till den senaste versionen första gången programmet ansluter till databasen efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="71656-296">It also configured Code First Migrations to automatically upgrade the database to the latest version the first time the application accesses the database after deployment.</span></span>
   
    <span data-ttu-id="71656-297">På grund av den här konfigurationen Code First skapade databasen genom att köra koden i den **inledande** klass som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="71656-297">As a result of this configuration, Code First created the database by running the code in the **Initial** class that you created earlier.</span></span> <span data-ttu-id="71656-298">Det gjorde detta första gången programmet försökte få åtkomst till databasen efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="71656-298">It did this the first time the application tried to access the database after deployment.</span></span>
7. <span data-ttu-id="71656-299">Ange en kontakt som du gjorde när du körde appen lokalt, kontrollera att databasen distributionen lyckades.</span><span class="sxs-lookup"><span data-stu-id="71656-299">Enter a contact as you did when you ran the app locally, to verify that database deployment succeeded.</span></span>

<span data-ttu-id="71656-300">När du ser att objektet som du anger sparas och visas på sidan Kontakta manager vet du att den har lagrats i databasen.</span><span class="sxs-lookup"><span data-stu-id="71656-300">When you see that the item you enter is saved and appears on the contact manager page, you know that it has been stored in the database.</span></span>

![Sidan index med kontakter][addwebapi004]

<span data-ttu-id="71656-302">Programmet körs nu i molnet, använder SQL-databas för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="71656-302">The application is now running in the cloud, using SQL Database to store its data.</span></span> <span data-ttu-id="71656-303">Ta bort när du är klar med att testa programmet i Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-303">After you finish testing the application in Azure, delete it.</span></span> <span data-ttu-id="71656-304">Programmet är offentlig och har inte en mekanism för att begränsa åtkomst.</span><span class="sxs-lookup"><span data-stu-id="71656-304">The application is public and doesn't have a mechanism to limit access.</span></span>

> [!NOTE]
> <span data-ttu-id="71656-305">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="71656-305">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="71656-306">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="71656-306">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="71656-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71656-307">Next Steps</span></span>
<span data-ttu-id="71656-308">Ett annat sätt att lagra data i ett Azure-program är att använda Azure storage som ger lagring av icke-relationella data i form av blobbar och tabeller.</span><span class="sxs-lookup"><span data-stu-id="71656-308">Another way to store data in an Azure application is to use Azure storage, which provide non-relational data storage in the form of blobs and tables.</span></span> <span data-ttu-id="71656-309">Följande länkar ger mer information om Web API, ASP.NET MVC och Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="71656-309">The following links provide more information on Web API, ASP.NET MVC and Window Azure.</span></span>

* <span data-ttu-id="71656-310">[Komma igång med Entity Framework med MVC][EFCodeFirstMVCTutorial]</span><span class="sxs-lookup"><span data-stu-id="71656-310">[Getting Started with Entity Framework using MVC][EFCodeFirstMVCTutorial]</span></span>
* [<span data-ttu-id="71656-311">Introduktion till ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="71656-311">Intro to ASP.NET MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="71656-312">Din första ASP.NET-webb-API</span><span class="sxs-lookup"><span data-stu-id="71656-312">Your First ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [<span data-ttu-id="71656-313">Felsökning av WAWS</span><span class="sxs-lookup"><span data-stu-id="71656-313">Debugging WAWS</span></span>](web-sites-dotnet-troubleshoot-visual-studio.md)

<span data-ttu-id="71656-314">Den här kursen och exempelprogrammet har skrivits av [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) med hjälp av Tom Dykstra och Barry Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)).</span><span class="sxs-lookup"><span data-stu-id="71656-314">This tutorial and the sample application was written by [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) with assistance from Tom Dykstra and Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)).</span></span> 

<span data-ttu-id="71656-315">Kontrollera bättre lämna feedback om vad du tyckte om, eller vad du skulle vilja se, inte bara om kursen sig själv, men även om de produkter som visar den.</span><span class="sxs-lookup"><span data-stu-id="71656-315">Please leave feedback on what you liked or what you would like to see improved, not only about the tutorial itself but also about the products that it demonstrates.</span></span> <span data-ttu-id="71656-316">Din feedback hjälper oss att prioritera förbättringar.</span><span class="sxs-lookup"><span data-stu-id="71656-316">Your feedback will help us prioritize improvements.</span></span> <span data-ttu-id="71656-317">Vi kan särskilt intresserad av att ta reda på hur mycket intresse finns i flera automation för processen för att konfigurera och distribuera medlemskapsdatabasen.</span><span class="sxs-lookup"><span data-stu-id="71656-317">We are especially interested in finding out how much interest there is in more automation for the process of configuring and deploying the membership database.</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="71656-318">Nyheter</span><span class="sxs-lookup"><span data-stu-id="71656-318">What's changed</span></span>
* <span data-ttu-id="71656-319">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="71656-319">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
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

