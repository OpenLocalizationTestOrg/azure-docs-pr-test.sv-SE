---
title: aaaHow toocreate en Webbapp med Redis-Cache | Microsoft Docs
description: "Lär dig hur toocreate ett webbprogram med Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 05/09/2017
ms.author: sdanie
ms.openlocfilehash: d3e6df97b06fdf9032570dc360944be4bd7715de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-web-app-with-redis-cache"></a><span data-ttu-id="e8e49-103">Hur toocreate ett webbprogram med Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-103">How toocreate a Web App with Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8e49-104">.NET</span><span class="sxs-lookup"><span data-stu-id="e8e49-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="e8e49-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e8e49-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="e8e49-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="e8e49-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="e8e49-107">Java</span><span class="sxs-lookup"><span data-stu-id="e8e49-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="e8e49-108">Python</span><span class="sxs-lookup"><span data-stu-id="e8e49-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="e8e49-109">Den här kursen visar hur toocreate och distribuera ett ASP.NET web application tooa webbapp i Azure App Service med Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e8e49-109">This tutorial shows how toocreate and deploy an ASP.NET web application tooa web app in Azure App Service using Visual Studio 2017.</span></span> <span data-ttu-id="e8e49-110">hello exempelprogrammet visar en lista över team statistik från en databas och visar olika sätt toouse Azure Redis-Cache toostore och hämta data från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-110">hello sample application displays a list of team statistics from a database and shows different ways toouse Azure Redis Cache toostore and retrieve data from hello cache.</span></span> <span data-ttu-id="e8e49-111">När du slutför hello kursen har du en webbapp som körs som läser och skriver tooa databasen optimerats med Azure Redis-Cache och värdbaserad i Azure.</span><span class="sxs-lookup"><span data-stu-id="e8e49-111">When you complete hello tutorial you'll have a running web app that reads and writes tooa database, optimized with Azure Redis Cache, and hosted in Azure.</span></span>

<span data-ttu-id="e8e49-112">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="e8e49-112">You'll learn:</span></span>

* <span data-ttu-id="e8e49-113">Hur toocreate en ASP.NET MVC 5 webbprogrammet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8e49-113">How toocreate an ASP.NET MVC 5 web application in Visual Studio.</span></span>
* <span data-ttu-id="e8e49-114">Hur tooaccess data från en databas med hjälp av Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8e49-114">How tooaccess data from a database using Entity Framework.</span></span>
* <span data-ttu-id="e8e49-115">Hur tooimprove datagenomströmning och minska databasbelastningen genom att lagra och hämta data med hjälp av Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-115">How tooimprove data throughput and reduce database load by storing and retrieving data using Azure Redis Cache.</span></span>
* <span data-ttu-id="e8e49-116">Hur toouse en Redis sorterade set tooretrieve hello övre 5 team.</span><span class="sxs-lookup"><span data-stu-id="e8e49-116">How toouse a Redis sorted set tooretrieve hello top 5 teams.</span></span>
* <span data-ttu-id="e8e49-117">Hur tooprovision hello Azure-resurser för hello program med hjälp av en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="e8e49-117">How tooprovision hello Azure resources for hello application using a Resource Manager template.</span></span>
* <span data-ttu-id="e8e49-118">Hur toopublish hello programmet tooAzure med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8e49-118">How toopublish hello application tooAzure using Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8e49-119">Krav</span><span class="sxs-lookup"><span data-stu-id="e8e49-119">Prerequisites</span></span>
<span data-ttu-id="e8e49-120">Du måste ha hello följande förutsättningar toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-120">toocomplete hello tutorial, you must have hello following prerequisites.</span></span>

* [<span data-ttu-id="e8e49-121">Azure-konto</span><span class="sxs-lookup"><span data-stu-id="e8e49-121">Azure account</span></span>](#azure-account)
* [<span data-ttu-id="e8e49-122">Visual Studio 2017 med hello Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="e8e49-122">Visual Studio 2017 with hello Azure SDK for .NET</span></span>](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a><span data-ttu-id="e8e49-123">Azure-konto</span><span class="sxs-lookup"><span data-stu-id="e8e49-123">Azure account</span></span>
<span data-ttu-id="e8e49-124">Du behöver en Azure-konto toocomplete hello vägledning.</span><span class="sxs-lookup"><span data-stu-id="e8e49-124">You need an Azure account toocomplete hello tutorial.</span></span> <span data-ttu-id="e8e49-125">Du kan:</span><span class="sxs-lookup"><span data-stu-id="e8e49-125">You can:</span></span>

* <span data-ttu-id="e8e49-126">[Öppna ett Azure-konto kostnadsfritt](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="e8e49-126">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="e8e49-127">Du får då krediter som kan vara används tootry ut betald Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e8e49-127">You get credits that can be used tootry out paid Azure services.</span></span> <span data-ttu-id="e8e49-128">Även efter hello krediten är slut, kan du hålla hello-konto och använda gratis Azure-tjänster och funktioner.</span><span class="sxs-lookup"><span data-stu-id="e8e49-128">Even after hello credits are used up, you can keep hello account and use free Azure services and features.</span></span>
* <span data-ttu-id="e8e49-129">[Aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="e8e49-129">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="e8e49-130">Din MSDN-prenumeration ger dig krediter varje månad som kan användas för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="e8e49-130">Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a><span data-ttu-id="e8e49-131">Visual Studio 2017 med hello Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="e8e49-131">Visual Studio 2017 with hello Azure SDK for .NET</span></span>
<span data-ttu-id="e8e49-132">hello kursen gäller för Visual Studio 2017 med hello [Azure SDK för .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span><span class="sxs-lookup"><span data-stu-id="e8e49-132">hello tutorial is written for Visual Studio 2017 with hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span></span> <span data-ttu-id="e8e49-133">hello Azure SDK 2.9.5 ingår i installationsprogrammet för hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8e49-133">hello Azure SDK 2.9.5 is included with hello Visual Studio installer.</span></span>

<span data-ttu-id="e8e49-134">Om du har Visual Studio 2015, du kan följa hello självstudier med hello [Azure SDK för .NET](../dotnet-sdk.md) 2.8.2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e8e49-134">If you have Visual Studio 2015, you can follow hello tutorial with hello [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 or later.</span></span> <span data-ttu-id="e8e49-135">[Hämta hello senaste Azure SDK för Visual Studio 2015 här](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="e8e49-135">[Download hello latest Azure SDK for Visual Studio 2015 here](http://go.microsoft.com/fwlink/?linkid=518003).</span></span> <span data-ttu-id="e8e49-136">Visual Studio installeras automatiskt med hello SDK om du inte redan har det.</span><span class="sxs-lookup"><span data-stu-id="e8e49-136">Visual Studio is automatically installed with hello SDK if you don't already have it.</span></span> <span data-ttu-id="e8e49-137">Vissa av skärmarna kan skilja sig från hello illustrationer visas i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-137">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

<span data-ttu-id="e8e49-138">Om du har Visual Studio 2013, kan du [download hello senaste Azure SDK för Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span><span class="sxs-lookup"><span data-stu-id="e8e49-138">If you have Visual Studio 2013, you can [download hello latest Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span></span> <span data-ttu-id="e8e49-139">Vissa av skärmarna kan skilja sig från hello illustrationer visas i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-139">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

## <a name="create-hello-visual-studio-project"></a><span data-ttu-id="e8e49-140">Skapa hello Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e8e49-140">Create hello Visual Studio project</span></span>
1. <span data-ttu-id="e8e49-141">Öppna Visual Studio och klicka på **Arkiv**, **Nytt**, **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-141">Open Visual Studio and click **File**, **New**, **Project**.</span></span>
2. <span data-ttu-id="e8e49-142">Expandera hello **Visual C#** nod i hello **mallar** väljer **moln**, och klicka på **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-142">Expand hello **Visual C#** node in hello **Templates** list, select **Cloud**, and click **ASP.NET Web Application**.</span></span> <span data-ttu-id="e8e49-143">Kontrollera att **.NET Framework 4.5.2** eller senare har valts.</span><span class="sxs-lookup"><span data-stu-id="e8e49-143">Ensure that **.NET Framework 4.5.2** or higher is selected.</span></span>  <span data-ttu-id="e8e49-144">Typen **ContosoTeamStats** till hello **namn** textrutan och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-144">Type **ContosoTeamStats** into hello **Name** textbox and click **OK**.</span></span>
   
    ![Skapa projekt][cache-create-project]
3. <span data-ttu-id="e8e49-146">Välj **MVC** som hello projekttypen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-146">Select **MVC** as hello project type.</span></span> 

    <span data-ttu-id="e8e49-147">Se till att **ingen autentisering** har angetts för hello **autentisering** inställningar.</span><span class="sxs-lookup"><span data-stu-id="e8e49-147">Ensure that **No Authentication** is specified for hello **Authentication** settings.</span></span> <span data-ttu-id="e8e49-148">Hello standard kan endast ange toosomething annan beroende på din version av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8e49-148">Depending on your version of Visual Studio, hello default may be set toosomething else.</span></span> <span data-ttu-id="e8e49-149">toochange, klickar du på **ändra autentisering** och välj **ingen autentisering**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-149">toochange it, click **Change Authentication** and select **No Authentication**.</span></span>

    <span data-ttu-id="e8e49-150">Om du följer tillsammans med Visual Studio 2015, avmarkera hello **värd i molnet hello** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e8e49-150">If you are following along with Visual Studio 2015, clear hello **Host in hello cloud** checkbox.</span></span> <span data-ttu-id="e8e49-151">Du kommer [etablera hello Azure-resurser](#provision-the-azure-resources) och [publicera hello programmet tooAzure](#publish-the-application-to-azure) i efterföljande steg i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-151">You'll [provision hello Azure resources](#provision-the-azure-resources) and [publish hello application tooAzure](#publish-the-application-to-azure) in subsequent steps in hello tutorial.</span></span> <span data-ttu-id="e8e49-152">Ett exempel på etablering av en Apptjänst-webbapp från Visual Studio genom att låta **värd i molnet hello** markerad finns [Kom igång med Webbappar i Azure App Service med hjälp av ASP.NET och Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e8e49-152">For an example of provisioning an App Service web app from Visual Studio by leaving **Host in hello cloud** checked, see [Get started with Web Apps in Azure App Service, using ASP.NET and Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
    ![Välj projektmall][cache-select-template]
4. <span data-ttu-id="e8e49-154">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-154">Click **OK** toocreate hello project.</span></span>

## <a name="create-hello-aspnet-mvc-application"></a><span data-ttu-id="e8e49-155">Skapa hello ASP.NET MVC-program</span><span class="sxs-lookup"><span data-stu-id="e8e49-155">Create hello ASP.NET MVC application</span></span>
<span data-ttu-id="e8e49-156">I det här avsnittet av kursen hello skapar du grundläggande hello-program som läser och visar team statistik från en-databas.</span><span class="sxs-lookup"><span data-stu-id="e8e49-156">In this section of hello tutorial, you'll create hello basic application that reads and displays team statistics from a database.</span></span>

* [<span data-ttu-id="e8e49-157">Lägg till hello Entity Framework NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="e8e49-157">Add hello Entity Framework NuGet package</span></span>](#add-the-entity-framework-nuget-package)
* [<span data-ttu-id="e8e49-158">Lägga till hello modell</span><span class="sxs-lookup"><span data-stu-id="e8e49-158">Add hello model</span></span>](#add-the-model)
* [<span data-ttu-id="e8e49-159">Lägga till hello-styrenhet</span><span class="sxs-lookup"><span data-stu-id="e8e49-159">Add hello controller</span></span>](#add-the-controller)
* [<span data-ttu-id="e8e49-160">Konfigurera hello vyer</span><span class="sxs-lookup"><span data-stu-id="e8e49-160">Configure hello views</span></span>](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a><span data-ttu-id="e8e49-161">Lägg till hello Entity Framework NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="e8e49-161">Add hello Entity Framework NuGet package</span></span>

1. <span data-ttu-id="e8e49-162">Klicka på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="e8e49-162">Click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="e8e49-163">Kör hello följande kommando från hello **Pakethanterarkonsolen** fönster.</span><span class="sxs-lookup"><span data-stu-id="e8e49-163">Run hello following command from hello **Package Manager Console** window.</span></span>
    
    ```
    Install-Package EntityFramework
    ```

<span data-ttu-id="e8e49-164">Mer information om det här paketet finns hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet-sidan.</span><span class="sxs-lookup"><span data-stu-id="e8e49-164">For more information about this package, see hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet page.</span></span>

### <a name="add-hello-model"></a><span data-ttu-id="e8e49-165">Lägga till hello modell</span><span class="sxs-lookup"><span data-stu-id="e8e49-165">Add hello model</span></span>
1. <span data-ttu-id="e8e49-166">Högerklicka på **Modeller** i **Solution Explorer** och välj **Lägg till**, **Klass**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-166">Right-click **Models** in **Solution Explorer**, and choose **Add**, **Class**.</span></span> 
   
    ![Lägg till modell][cache-model-add-class]
2. <span data-ttu-id="e8e49-168">Ange `Team` för hello klassnamn och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-168">Enter `Team` for hello class name and click **Add**.</span></span>
   
    ![Lägg till modellklass][cache-model-add-class-dialog]
3. <span data-ttu-id="e8e49-170">Ersätt hello `using` instruktioner överst hello i hello `Team.cs` fil med hello följande `using` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="e8e49-170">Replace hello `using` statements at hello top of hello `Team.cs` file with hello following `using` statements.</span></span>

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. <span data-ttu-id="e8e49-171">Ersätt hello definitionen av hello `Team` klassen med följande kodavsnitt som innehåller en uppdaterad hello `Team` klassen definition samt vissa andra hjälpprogramklasser för Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8e49-171">Replace hello definition of hello `Team` class with hello following code snippet that contains an updated `Team` class definition as well as some other Entity Framework helper classes.</span></span> <span data-ttu-id="e8e49-172">Mer information om hello kod första metoden tooEntity Framework som används i den här kursen finns [kod första tooa ny databas](https://msdn.microsoft.com/data/jj193542).</span><span class="sxs-lookup"><span data-stu-id="e8e49-172">For more information on hello code first approach tooEntity Framework that's used in this tutorial, see [Code first tooa new database](https://msdn.microsoft.com/data/jj193542).</span></span>

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. <span data-ttu-id="e8e49-173">I **Solution Explorer**, dubbelklicka på **web.config** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="e8e49-173">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="e8e49-175">Lägg till följande hello `connectionStrings` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-175">Add hello following `connectionStrings` section.</span></span> <span data-ttu-id="e8e49-176">hello namnet på hello anslutningssträng måste matcha hello namnet på hello Entity Framework databasen kontexten klassen som är `TeamContext`.</span><span class="sxs-lookup"><span data-stu-id="e8e49-176">hello name of hello connection string must match hello name of hello Entity Framework database context class which is `TeamContext`.</span></span>

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="e8e49-177">Du kan lägga till nya hello `connectionStrings` avsnittet så att den följer `configSections`som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-177">You can add hello new `connectionStrings` section so that it follows `configSections`, as shown in hello following example.</span></span>

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

    > [!NOTE]
    > <span data-ttu-id="e8e49-178">Anslutningssträngen kan vara olika beroende på hello version av Visual Studio och SQL Server Express edition används toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-178">Your connection string may be different depending on hello version of Visual Studio and SQL Server Express edition used toocomplete hello tutorial.</span></span> <span data-ttu-id="e8e49-179">hello web.config mallen ska vara konfigurerade toomatch installationen och kan innehålla `Data Source` poster som `(LocalDB)\v11.0` (från SQL Server Express 2012) eller `Data Source=(LocalDB)\MSSQLLocalDB` (från SQL Server Express 2014 och nyare).</span><span class="sxs-lookup"><span data-stu-id="e8e49-179">hello web.config template should be configured toomatch your installation, and may contain `Data Source` entries like `(LocalDB)\v11.0` (from SQL Server Express 2012) or `Data Source=(LocalDB)\MSSQLLocalDB` (from SQL Server Express 2014 and newer).</span></span> <span data-ttu-id="e8e49-180">Mer information om anslutningssträngar och SQL Express-versioner finns i [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span><span class="sxs-lookup"><span data-stu-id="e8e49-180">For more information about connection strings and SQL Express versions, see [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span></span>

### <a name="add-hello-controller"></a><span data-ttu-id="e8e49-181">Lägga till hello-styrenhet</span><span class="sxs-lookup"><span data-stu-id="e8e49-181">Add hello controller</span></span>
1. <span data-ttu-id="e8e49-182">Tryck på **F6** toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-182">Press **F6** toobuild hello project.</span></span> 
2. <span data-ttu-id="e8e49-183">I **Solution Explorer**, högerklicka på hello **domänkontrollanter** mapp och välj **Lägg till**, **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-183">In **Solution Explorer**, right-click hello **Controllers** folder and choose **Add**, **Controller**.</span></span>
   
    ![Lägga till en kontrollant][cache-add-controller]
3. <span data-ttu-id="e8e49-185">Välj **MVC 5-kontrollant med vyer, med hjälp av Entity Framework** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-185">Choose **MVC 5 Controller with views, using Entity Framework**, and click **Add**.</span></span> <span data-ttu-id="e8e49-186">Om du får ett felmeddelande när du klickar på **Lägg till**, se till att du har skapat hello projekt först.</span><span class="sxs-lookup"><span data-stu-id="e8e49-186">If you get an error after clicking **Add**, ensure that you have built hello project first.</span></span>
   
    ![Lägga till en kontrollantklass][cache-add-controller-class]
4. <span data-ttu-id="e8e49-188">Välj **Team (ContosoTeamStats.Models)** från hello **Modellklass** listrutan.</span><span class="sxs-lookup"><span data-stu-id="e8e49-188">Select **Team (ContosoTeamStats.Models)** from hello **Model class** drop-down list.</span></span> <span data-ttu-id="e8e49-189">Välj **TeamContext (ContosoTeamStats.Models)** från hello **datakontextklass** listrutan.</span><span class="sxs-lookup"><span data-stu-id="e8e49-189">Select **TeamContext (ContosoTeamStats.Models)** from hello **Data context class** drop-down list.</span></span> <span data-ttu-id="e8e49-190">Typen `TeamsController` i hello **domänkontrollant** textrutan (om det inte fylls i automatiskt).</span><span class="sxs-lookup"><span data-stu-id="e8e49-190">Type `TeamsController` in hello **Controller** name textbox (if it is not automatically populated).</span></span> <span data-ttu-id="e8e49-191">Klicka på **Lägg till** toocreate hello controller-klassen och Lägg till hello standardvyer.</span><span class="sxs-lookup"><span data-stu-id="e8e49-191">Click **Add** toocreate hello controller class and add hello default views.</span></span>
   
    ![Konfigurera kontrollanten][cache-configure-controller]
5. <span data-ttu-id="e8e49-193">I **Solution Explorer**, expandera **Global.asax** och dubbelklicka på **Global.asax.cs** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="e8e49-193">In **Solution Explorer**, expand **Global.asax** and double-click **Global.asax.cs** tooopen it.</span></span>
   
    ![Global.asax.cs][cache-global-asax]
6. <span data-ttu-id="e8e49-195">Lägg till följande två hello `using` instruktioner överst hello i hello-filen under hello andra `using` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="e8e49-195">Add hello following two `using` statements at hello top of hello file under hello other `using` statements.</span></span>

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. <span data-ttu-id="e8e49-196">Lägg till följande kodrad i slutet av hello av hello hello `Application_Start` metod.</span><span class="sxs-lookup"><span data-stu-id="e8e49-196">Add hello following line of code at hello end of hello `Application_Start` method.</span></span>

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. <span data-ttu-id="e8e49-197">I **Solution Explorer** expanderar du `App_Start` och dubbelklickar på `RouteConfig.cs`.</span><span class="sxs-lookup"><span data-stu-id="e8e49-197">In **Solution Explorer**, expand `App_Start` and double-click `RouteConfig.cs`.</span></span>
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. <span data-ttu-id="e8e49-199">Ersätt `controller = "Home"` i följande kod i hello hello `RegisterRoutes` metod med `controller = "Teams"` som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-199">Replace `controller = "Home"` in hello following code in hello `RegisterRoutes` method with `controller = "Teams"` as shown in hello following example.</span></span>

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a><span data-ttu-id="e8e49-200">Konfigurera hello vyer</span><span class="sxs-lookup"><span data-stu-id="e8e49-200">Configure hello views</span></span>
1. <span data-ttu-id="e8e49-201">I **Solution Explorer**, expandera hello **vyer** mapp och sedan hello **delade** mappen och dubbelklickar på **_Layout.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-201">In **Solution Explorer**, expand hello **Views** folder and then hello **Shared** folder, and double-click **_Layout.cshtml**.</span></span> 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. <span data-ttu-id="e8e49-203">Ändra hello innehåll hello `title` element och Ersätt `My ASP.NET Application` med `Contoso Team Stats` som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-203">Change hello contents of hello `title` element and replace `My ASP.NET Application` with `Contoso Team Stats` as shown in hello following example.</span></span>

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. <span data-ttu-id="e8e49-204">I hello `body` och uppdatera hello först `Html.ActionLink` -instruktionen och Ersätt `Application name` med `Contoso Team Stats` och Ersätt `Home` med `Teams`.</span><span class="sxs-lookup"><span data-stu-id="e8e49-204">In hello `body` section, update hello first `Html.ActionLink` statement and replace `Application name` with `Contoso Team Stats` and replace `Home` with `Teams`.</span></span>
   
   * <span data-ttu-id="e8e49-205">Innan: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="e8e49-205">Before: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
   * <span data-ttu-id="e8e49-206">Efter: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="e8e49-206">After: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
     
     ![Kodändringar][cache-layout-cshtml-code]
2. <span data-ttu-id="e8e49-208">Tryck på **Ctrl + F5** toobuild och kör hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-208">Press **Ctrl+F5** toobuild and run hello application.</span></span> <span data-ttu-id="e8e49-209">Den här versionen av programmet hello läser hello resultat direkt från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-209">This version of hello application reads hello results directly from hello database.</span></span> <span data-ttu-id="e8e49-210">Obs hello **Skapa nytt**, **redigera**, **information**, och **ta bort** åtgärder som automatiskt har lagts till toohello program av hello **MVC 5 styrenhet med vyer, med hjälp av Entity Framework** kodskelett.</span><span class="sxs-lookup"><span data-stu-id="e8e49-210">Note hello **Create New**, **Edit**, **Details**, and **Delete** actions that were automatically added toohello application by hello **MVC 5 Controller with views, using Entity Framework** scaffold.</span></span> <span data-ttu-id="e8e49-211">I nästa avsnitt om hello hello kursen ska du lägga till Redis-Cache toooptimize hello data komma åt och erbjuder ytterligare funktioner toohello program.</span><span class="sxs-lookup"><span data-stu-id="e8e49-211">In hello next section of hello tutorial you'll add Redis Cache toooptimize hello data access and provide additional features toohello application.</span></span>

![Startprogram][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a><span data-ttu-id="e8e49-213">Konfigurera hello programmet toouse Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-213">Configure hello application toouse Redis Cache</span></span>
<span data-ttu-id="e8e49-214">I det här avsnittet av kursen hello du konfigurera hello exempel programmet toostore och hämta Contoso teamet statistik från Azure Redis-Cache-instans med hjälp av hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cacheklienten.</span><span class="sxs-lookup"><span data-stu-id="e8e49-214">In this section of hello tutorial, you'll configure hello sample application toostore and retrieve Contoso team statistics from an Azure Redis Cache instance by using hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cache client.</span></span>

* [<span data-ttu-id="e8e49-215">Konfigurera hello programmet toouse StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="e8e49-215">Configure hello application toouse StackExchange.Redis</span></span>](#configure-the-application-to-use-stackexchangeredis)
* [<span data-ttu-id="e8e49-216">Uppdatera hello TeamsController klassen tooreturn resultat från hello cache eller hello-databas</span><span class="sxs-lookup"><span data-stu-id="e8e49-216">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [<span data-ttu-id="e8e49-217">Uppdatera hello skapa, redigera och ta bort metoder toowork med hello-cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-217">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [<span data-ttu-id="e8e49-218">Uppdatera hello team Index visa toowork med hello-cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-218">Update hello Teams Index view toowork with hello cache</span></span>](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a><span data-ttu-id="e8e49-219">Konfigurera hello programmet toouse StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="e8e49-219">Configure hello application toouse StackExchange.Redis</span></span>
1. <span data-ttu-id="e8e49-220">tooconfigure ett klientprogram i Visual Studio med hello StackExchange.Redis NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="e8e49-220">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="e8e49-221">Kör hello följande kommando från hello `Package Manager Console` fönster.</span><span class="sxs-lookup"><span data-stu-id="e8e49-221">Run hello following command from hello `Package Manager Console` window.</span></span>
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    <span data-ttu-id="e8e49-222">Hej NuGet paket hämtar och lägger till hello krävs sammansättningsreferenser för din klient programmet tooaccess Azure Redis-Cache med hello StackExchange.Redis cache-klienten.</span><span class="sxs-lookup"><span data-stu-id="e8e49-222">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span> <span data-ttu-id="e8e49-223">Om du föredrar toouse ett starkt krypterat namn version av hello `StackExchange.Redis` klientbiblioteket, installera hello `StackExchange.Redis.StrongName` paketet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-223">If you prefer toouse a strong-named version of hello `StackExchange.Redis` client library, install hello `StackExchange.Redis.StrongName` package.</span></span>
3. <span data-ttu-id="e8e49-224">I **Solution Explorer**, expandera hello **domänkontrollanter** mappen och dubbelklickar på **TeamsController.cs** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="e8e49-224">In **Solution Explorer**, expand hello **Controllers** folder and double-click **TeamsController.cs** tooopen it.</span></span>
   
    ![Teamkontrollant][cache-teamscontroller]
4. <span data-ttu-id="e8e49-226">Lägg till följande två hello `using` instruktioner för**TeamsController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-226">Add hello following two `using` statements too**TeamsController.cs**.</span></span>

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. <span data-ttu-id="e8e49-227">Lägg till följande två egenskaper toohello hello `TeamsController` klass.</span><span class="sxs-lookup"><span data-stu-id="e8e49-227">Add hello following two properties toohello `TeamsController` class.</span></span>

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. <span data-ttu-id="e8e49-228">Skapa en fil på datorn med namnet `WebAppPlusCacheAppSecrets.config` och placera den på en plats som inte kontrolleras in med hello källkoden för exempelprogrammet, bör du bestämma toocheck den i någon plats.</span><span class="sxs-lookup"><span data-stu-id="e8e49-228">Create a file on your computer named `WebAppPlusCacheAppSecrets.config` and place it in a location that won't be checked in with hello source code of your sample application, should you decide toocheck it in somewhere.</span></span> <span data-ttu-id="e8e49-229">I det här exemplet hello `AppSettingsSecrets.config` filen finns `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="e8e49-229">In this example hello `AppSettingsSecrets.config` file is located at `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span></span>
   
    <span data-ttu-id="e8e49-230">Redigera hello `WebAppPlusCacheAppSecrets.config` och Lägg till hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-230">Edit hello `WebAppPlusCacheAppSecrets.config` file and add hello following contents.</span></span> <span data-ttu-id="e8e49-231">Om du kör programmet hello lokalt är den här informationen används tooconnect tooyour Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="e8e49-231">If you run hello application locally this information is used tooconnect tooyour Azure Redis Cache instance.</span></span> <span data-ttu-id="e8e49-232">Senare i självstudiekursen hello du etablera en instans av Azure Redis-Cache och uppdatera hello cache namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="e8e49-232">Later in hello tutorial you'll provision an Azure Redis Cache instance and update hello cache name and password.</span></span> <span data-ttu-id="e8e49-233">Om du inte planerar toorun hello exempelprogrammet lokalt kan du hoppa över hello skapandet av den här filen och hello efterföljande steg som refererar till hello filen eftersom när du distribuerar tooAzure hello program hämtar hello cache anslutningsinformation från hello app inställningen för hello Web App och inte från den här filen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-233">If you don't plan toorun hello sample application locally you can skip hello creation of this file and hello subsequent steps that reference hello file, because when you deploy tooAzure hello application retrieves hello cache connection information from hello app setting for hello Web App and not from this file.</span></span> <span data-ttu-id="e8e49-234">Eftersom hello `WebAppPlusCacheAppSecrets.config` distribueras inte tooAzure med ditt program behöver du inte den om du ska toorun hello programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-234">Since hello `WebAppPlusCacheAppSecrets.config` is not deployed tooAzure with your application, you don't need it unless you are going toorun hello application locally.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="e8e49-235">I **Solution Explorer**, dubbelklicka på **web.config** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="e8e49-235">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="e8e49-237">Lägg till följande hello `file` attributet toohello `appSettings` element.</span><span class="sxs-lookup"><span data-stu-id="e8e49-237">Add hello following `file` attribute toohello `appSettings` element.</span></span> <span data-ttu-id="e8e49-238">Om du använde ett annat namn eller en plats i stället dessa värden för hello som visas i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="e8e49-238">If you used a different file name or location, substitute those values for hello ones shown in hello example.</span></span>
   
   * <span data-ttu-id="e8e49-239">Innan: `<appSettings>`</span><span class="sxs-lookup"><span data-stu-id="e8e49-239">Before: `<appSettings>`</span></span>
   * <span data-ttu-id="e8e49-240">Efter: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span><span class="sxs-lookup"><span data-stu-id="e8e49-240">After: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span></span>
     
   <span data-ttu-id="e8e49-241">hello ASP.NET-körningsmiljön sammanfogar hello innehållet i hello externa filen med hello markeringar i hello `<appSettings>` element.</span><span class="sxs-lookup"><span data-stu-id="e8e49-241">hello ASP.NET runtime merges hello contents of hello external file with hello markup in hello `<appSettings>` element.</span></span> <span data-ttu-id="e8e49-242">hello runtime ignorerar hello Filattributet om hello angivna filen inte kan hittas.</span><span class="sxs-lookup"><span data-stu-id="e8e49-242">hello runtime ignores hello file attribute if hello specified file cannot be found.</span></span> <span data-ttu-id="e8e49-243">Din hemliga (hello anslutning sträng tooyour cache) ingår inte som en del av hello källkoden för hello program.</span><span class="sxs-lookup"><span data-stu-id="e8e49-243">Your secrets (hello connection string tooyour cache) are not included as part of hello source code for hello application.</span></span> <span data-ttu-id="e8e49-244">När du distribuerar din web app tooAzure hello `WebAppPlusCacheAppSecrests.config` filen inte distribueras (som du vill).</span><span class="sxs-lookup"><span data-stu-id="e8e49-244">When you deploy your web app tooAzure, hello `WebAppPlusCacheAppSecrests.config` file won't be deployed (that's what you want).</span></span> <span data-ttu-id="e8e49-245">Det finns flera sätt toospecify dessa hemligheter för i Azure och i den här självstudiekursen de konfigureras automatiskt för dig när du [etablera hello Azure-resurser](#provision-the-azure-resources) i ett efterföljande steg.</span><span class="sxs-lookup"><span data-stu-id="e8e49-245">There are several ways toospecify these secrets in Azure, and in this tutorial they are configured automatically for you when you [provision hello Azure resources](#provision-the-azure-resources) in a subsequent tutorial step.</span></span> <span data-ttu-id="e8e49-246">Mer information om hur du arbetar med hemligheter i Azure finns [bästa praxis för att distribuera lösenord och andra känsliga data tooASP.NET och Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span><span class="sxs-lookup"><span data-stu-id="e8e49-246">For more information about working with secrets in Azure, see [Best practices for deploying passwords and other sensitive data tooASP.NET and Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span></span>

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a><span data-ttu-id="e8e49-247">Uppdatera hello TeamsController klassen tooreturn resultat från hello cache eller hello-databas</span><span class="sxs-lookup"><span data-stu-id="e8e49-247">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>
<span data-ttu-id="e8e49-248">I det här exemplet kan team statistik hämtas från databasen hello eller hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-248">In this sample, team statistics can be retrieved from hello database or from hello cache.</span></span> <span data-ttu-id="e8e49-249">Team statistik lagras i cacheminnet för hello som en serialiserad `List<Team>`, och även som en sorterad uppsättning med Redis-datatyper.</span><span class="sxs-lookup"><span data-stu-id="e8e49-249">Team statistics are stored in hello cache as a serialized `List<Team>`, and also as a sorted set using Redis data types.</span></span> <span data-ttu-id="e8e49-250">När du hämtar objekt från en sorterad uppsättning hämtar du vissa, alla eller frågar efter vissa objekt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-250">When retrieving items from a sorted set, you can retrieve some, all, or query for certain items.</span></span> <span data-ttu-id="e8e49-251">I det här exemplet ska du fråga hello sorteras set för hello övre 5 team rangordnas av antal wins.</span><span class="sxs-lookup"><span data-stu-id="e8e49-251">In this sample you'll query hello sorted set for hello top 5 teams ranked by number of wins.</span></span>

> [!NOTE]
> <span data-ttu-id="e8e49-252">Det är inte obligatoriska toostore hello team statistik i olika format i hello-cachen i ordning toouse Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-252">It is not required toostore hello team statistics in multiple formats in hello cache in order toouse Azure Redis Cache.</span></span> <span data-ttu-id="e8e49-253">Den här kursen använder flera format toodemonstrate vissa hello olika sätt och olika datatyper du kan använda toocache data.</span><span class="sxs-lookup"><span data-stu-id="e8e49-253">This tutorial uses multiple formats toodemonstrate some of hello different ways and different data types you can use toocache data.</span></span>
> 
> 

1. <span data-ttu-id="e8e49-254">Lägg till följande hello `using` instruktioner toohello `TeamsController.cs` filen överst hello med hello andra `using` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="e8e49-254">Add hello following `using` statements toohello `TeamsController.cs` file at hello top with hello other `using` statements.</span></span>

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. <span data-ttu-id="e8e49-255">Ersätta hello aktuella `public ActionResult Index()` metodimplementering med hello efter implementering.</span><span class="sxs-lookup"><span data-stu-id="e8e49-255">Replace hello current `public ActionResult Index()` method implementation with hello following implementation.</span></span>

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear hello results from hello cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild hello database with sample data.
                RebuildDB();
                break;
        }

        // Measure hello time it takes tooretrieve hello results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve hello top 5 teams from hello sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from hello cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from hello database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add hello elapsed time of hello operation toohello ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. <span data-ttu-id="e8e49-256">Lägg till följande tre metoder toohello hello `TeamsController` klassen tooimplement hello `playGames`, `clearCache`, och `rebuildDB` åtgärdstyper från hello växla instruktionen som har lagts till i hello föregående kodfragment.</span><span class="sxs-lookup"><span data-stu-id="e8e49-256">Add hello following three methods toohello `TeamsController` class tooimplement hello `playGames`, `clearCache`, and `rebuildDB` action types from hello switch statement added in hello previous code snippet.</span></span>
   
    <span data-ttu-id="e8e49-257">Hej `PlayGames` metoden programuppdateringarna hello team statistik genom att simulera en säsongen spel, sparar hello resultat toohello databasen och raderar hello nu inaktuella data från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-257">hello `PlayGames` method updates hello team statistics by simulating a season of games, saves hello results toohello database, and clears hello now outdated data from hello cache.</span></span>

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    <span data-ttu-id="e8e49-258">Hej `RebuildDB` metoden initierar hello databasen med hello standarduppsättning team genererar statistik för dem och raderar hello nu inaktuella data från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-258">hello `RebuildDB` method reinitializes hello database with hello default set of teams, generates statistics for them, and clears hello now outdated data from hello cache.</span></span>

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize hello database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    <span data-ttu-id="e8e49-259">Hej `ClearCachedTeams` metod tar du bort cachelagrade team statistik från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-259">hello `ClearCachedTeams` method removes any cached team statistics from hello cache.</span></span>

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. <span data-ttu-id="e8e49-260">Lägg till följande fyra metoderna toohello hello `TeamsController` klassen tooimplement hello hämtas hello team statistik från hello cache och hello databasen på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-260">Add hello following four methods toohello `TeamsController` class tooimplement hello various ways of retrieving hello team statistics from hello cache and hello database.</span></span> <span data-ttu-id="e8e49-261">Var och en av dessa metoder returnerar en `List<Team>` som visas av hello vyn.</span><span class="sxs-lookup"><span data-stu-id="e8e49-261">Each of these methods returns a `List<Team>` which is then displayed by hello view.</span></span>
   
    <span data-ttu-id="e8e49-262">Hej `GetFromDB` metoden läser hello team statistik från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-262">hello `GetFromDB` method reads hello team statistics from hello database.</span></span>
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    <span data-ttu-id="e8e49-263">Hej `GetFromList` metod läser hello team statistik från cachen som en serialiserad `List<Team>`.</span><span class="sxs-lookup"><span data-stu-id="e8e49-263">hello `GetFromList` method reads hello team statistics from cache as a serialized `List<Team>`.</span></span> <span data-ttu-id="e8e49-264">Om det finns en cache-miss, läsa från hello databasen hello team statistik och lagras sedan i hello cache för nästa gång.</span><span class="sxs-lookup"><span data-stu-id="e8e49-264">If there is a cache miss, hello team statistics are read from hello database and then stored in hello cache for next time.</span></span> <span data-ttu-id="e8e49-265">I det här exemplet använder vi JSON.NET serialisering tooserialize hello .NET objekt tooand från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-265">In this sample we're using JSON.NET serialization tooserialize hello .NET objects tooand from hello cache.</span></span> <span data-ttu-id="e8e49-266">Mer information finns i [hur toowork med .NET objekt i Azure Redis-Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span><span class="sxs-lookup"><span data-stu-id="e8e49-266">For more information, see [How toowork with .NET objects in Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span></span>

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    <span data-ttu-id="e8e49-267">Hej `GetFromSortedSet` metoden läser hello team statistik från en cachelagrad sorterade.</span><span class="sxs-lookup"><span data-stu-id="e8e49-267">hello `GetFromSortedSet` method reads hello team statistics from a cached sorted set.</span></span> <span data-ttu-id="e8e49-268">Om det finns en cache-miss, läsa från hello databasen och hello cachelagras som en sorterad uppsättning hello team statistik.</span><span class="sxs-lookup"><span data-stu-id="e8e49-268">If there is a cache miss, hello team statistics are read from hello database and stored in hello cache as a sorted set.</span></span>

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding toosorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    <span data-ttu-id="e8e49-269">Hej `GetFromSortedSetTop5` metoden läser hello övre 5 team från hello cachelagras sorteras uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-269">hello `GetFromSortedSetTop5` method reads hello top 5 teams from hello cached sorted set.</span></span> <span data-ttu-id="e8e49-270">Startar det genom att kontrollerar hello cache hello befintliga hello `teamsSortedSet` nyckel.</span><span class="sxs-lookup"><span data-stu-id="e8e49-270">It starts by checking hello cache for hello existence of hello `teamsSortedSet` key.</span></span> <span data-ttu-id="e8e49-271">Om den här nyckeln inte finns hello `GetFromSortedSet` metoden anropas tooread hello team statistik och lagrar dem i hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-271">If this key is not present, hello `GetFromSortedSet` method is called tooread hello team statistics and store them in hello cache.</span></span> <span data-ttu-id="e8e49-272">Därefter efterfrågas hello cachelagrade sorterade set hello övre 5 team som returneras.</span><span class="sxs-lookup"><span data-stu-id="e8e49-272">Next, hello cached sorted set is queried for hello top 5 teams which are returned.</span></span>

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load hello entire sorted set into hello cache.
            GetFromSortedSet();

            // Retrieve hello top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get hello top 5 teams from hello sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a><span data-ttu-id="e8e49-273">Uppdatera hello skapa, redigera och ta bort metoder toowork med hello-cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-273">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>
<span data-ttu-id="e8e49-274">hello scaffold-teknik för kod som har skapats som en del av det här exemplet innehåller metoder tooadd, redigera och ta bort team.</span><span class="sxs-lookup"><span data-stu-id="e8e49-274">hello scaffolding code that was generated as part of this sample includes methods tooadd, edit, and delete teams.</span></span> <span data-ttu-id="e8e49-275">Varje gång som ett team läggs till, redigera eller ta bort blir hello data i hello cache inaktuella.</span><span class="sxs-lookup"><span data-stu-id="e8e49-275">Anytime a team is added, edited, or removed, hello data in hello cache becomes outdated.</span></span> <span data-ttu-id="e8e49-276">I det här avsnittet ska du ändra cachelagras dessa tre metoder tooclear hello team så att hello cachen inte synkroniserad med hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-276">In this section you'll modify these three methods tooclear hello cached teams so that hello cache won't be out of sync with hello database.</span></span>

1. <span data-ttu-id="e8e49-277">Bläddra toohello `Create(Team team)` metod i hello `TeamsController` klass.</span><span class="sxs-lookup"><span data-stu-id="e8e49-277">Browse toohello `Create(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="e8e49-278">Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-278">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Create
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. <span data-ttu-id="e8e49-279">Bläddra toohello `Edit(Team team)` metod i hello `TeamsController` klass.</span><span class="sxs-lookup"><span data-stu-id="e8e49-279">Browse toohello `Edit(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="e8e49-280">Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-280">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Edit/5
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. <span data-ttu-id="e8e49-281">Bläddra toohello `DeleteConfirmed(int id)` metod i hello `TeamsController` klass.</span><span class="sxs-lookup"><span data-stu-id="e8e49-281">Browse toohello `DeleteConfirmed(int id)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="e8e49-282">Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-282">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, hello cache is out of date.
        // Clear hello cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a><span data-ttu-id="e8e49-283">Uppdatera hello team Index visa toowork med hello-cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-283">Update hello Teams Index view toowork with hello cache</span></span>
1. <span data-ttu-id="e8e49-284">I **Solution Explorer**, expandera hello **vyer** mapp och sedan hello **team** mappen och dubbelklickar på **Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-284">In **Solution Explorer**, expand hello **Views** folder, then hello **Teams** folder, and double-click **Index.cshtml**.</span></span>
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. <span data-ttu-id="e8e49-286">Leta efter hello följande punkt elementet hello övre delen av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-286">Near hello top of hello file, look for hello following paragraph element.</span></span>
   
    ![Åtgärdstabell][cache-teams-index-table]
   
    <span data-ttu-id="e8e49-288">Detta är hello länken toocreate ett nytt team.</span><span class="sxs-lookup"><span data-stu-id="e8e49-288">This is hello link toocreate a new team.</span></span> <span data-ttu-id="e8e49-289">Ersätt hello punkt element med hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-289">Replace hello paragraph element with hello following table.</span></span> <span data-ttu-id="e8e49-290">Den här tabellen har åtgärdslänkar för att skapa ett nytt team spela upp en ny säsongen för spel, hello cachen rensas, hämtar hello team från hello-cache i olika format, hämta hello team från hello databas och återskapa hello databas med ny exempeldata.</span><span class="sxs-lookup"><span data-stu-id="e8e49-290">This table has action links for creating a new team, playing a new season of games, clearing hello cache, retrieving hello teams from hello cache in several formats, retrieving hello teams from hello database, and rebuilding hello database with fresh sample data.</span></span>

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. <span data-ttu-id="e8e49-291">Rulla toohello längst ned på hello **Index.cshtml** och Lägg till följande hello `tr` elementet så att den är hello sista raden i hello senast tabell i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-291">Scroll toohello bottom of hello **Index.cshtml** file and add hello following `tr` element so that it is hello last row in hello last table in hello file.</span></span>
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    <span data-ttu-id="e8e49-292">Den här raden visar hello värdet för `ViewBag.Msg` som innehåller en statusrapport om hello den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e8e49-292">This row displays hello value of `ViewBag.Msg` which contains a status report about hello current operation.</span></span> <span data-ttu-id="e8e49-293">Hej `ViewBag.Msg` anges när du klickar på någon av hello åtgärdslänkar hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e8e49-293">hello `ViewBag.Msg` is set when you click any of hello action links from hello previous step.</span></span>   
   
    ![Statusmeddelande][cache-status-message]
2. <span data-ttu-id="e8e49-295">Tryck på **F6** toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-295">Press **F6** toobuild hello project.</span></span>

## <a name="provision-hello-azure-resources"></a><span data-ttu-id="e8e49-296">Etablera hello Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="e8e49-296">Provision hello Azure resources</span></span>
<span data-ttu-id="e8e49-297">toohost ditt program i Azure måste du först etablera hello Azure-tjänster som krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="e8e49-297">toohost your application in Azure, you must first provision hello Azure services that your application requires.</span></span> <span data-ttu-id="e8e49-298">hello exempelprogrammet i den här kursen använder hello följande Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e8e49-298">hello sample application in this tutorial uses hello following Azure services.</span></span>

* <span data-ttu-id="e8e49-299">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-299">Azure Redis Cache</span></span>
* <span data-ttu-id="e8e49-300">App Service-webbapp</span><span class="sxs-lookup"><span data-stu-id="e8e49-300">App Service Web App</span></span>
* <span data-ttu-id="e8e49-301">SQL Database</span><span class="sxs-lookup"><span data-stu-id="e8e49-301">SQL Database</span></span>

<span data-ttu-id="e8e49-302">toodeploy dessa tjänster tooa ny eller befintlig resursgrupp för ditt val klickar du på följande hello **distribuera tooAzure** knappen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-302">toodeploy these services tooa new or existing resource group of your choice, click hello following **Deploy tooAzure** button.</span></span>

<span data-ttu-id="e8e49-303">[! [Distribuera tooAzure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e8e49-303">[![Deploy tooAzure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span></span>

<span data-ttu-id="e8e49-304">Detta **distribuera tooAzure** knappen använder hello [skapa en Webbapp plus Redis-Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) mallen tooprovision dessa tjänster och ange hello anslutningssträngen för hello SQL-databasen och hello inställningen för hello anslutningssträngen för Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-304">This **Deploy tooAzure** button uses hello [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) template tooprovision these services and set hello connection string for hello SQL Database and hello application setting for hello Azure Redis Cache connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="e8e49-305">Om du inte har något Azure-konto kan du [skapa ett kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e8e49-305">If you don't have an Azure account, you can [create a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.</span></span>
> 
> 

<span data-ttu-id="e8e49-306">Klicka på hello **distribuera tooAzure** tar dig toohello Azure-portalen och initierar hello skapar hello resurser som beskrivs av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-306">Clicking hello **Deploy tooAzure** button takes you toohello Azure portal and initiates hello process of creating hello resources described by hello template.</span></span>

![Distribuera tooAzure][cache-deploy-to-azure-step-1]

1. <span data-ttu-id="e8e49-308">I hello **grunderna** avsnittet Välj hello Azure-prenumeration toouse och välj en befintlig resursgrupp eller skapa ett nytt och ange hello resursgruppens plats.</span><span class="sxs-lookup"><span data-stu-id="e8e49-308">In hello **Basics** section, select hello Azure subscription toouse, and select an existing resource group or create a new one, and specify hello resource group location.</span></span>
2. <span data-ttu-id="e8e49-309">I hello **inställningar** , anger ett **administratörsinloggning** (Använd inte **admin**), **inloggningen administratörslösenord**, och  **Databasnamn**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-309">In hello **Settings** section, specify an **Administrator Login** (don't use **admin**), **Administrator Login Password**, and **Database Name**.</span></span> <span data-ttu-id="e8e49-310">hello har andra parametrar konfigurerats för en kostnadsfri Apptjänst värd planerings- och lägre kostnad alternativ för hello SQL Database och Azure Redis-Cache som inte levereras med en kostnadsfri nivå.</span><span class="sxs-lookup"><span data-stu-id="e8e49-310">hello other parameters are configured for a free App Service hosting plan, and lower-cost options for hello SQL Database and Azure Redis Cache, which don't come with a free tier.</span></span>

    ![Distribuera tooAzure][cache-deploy-to-azure-step-2]

3. <span data-ttu-id="e8e49-312">Rulla toohello slutet av hello-sidan, skrivskyddade hello villkor när du har konfigurerat hello önskade inställningar och kontrollera hello **acceptera toohello villkoren ovan** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e8e49-312">After configuring hello desired settings, scroll toohello end of hello page, read hello terms and conditions, and check hello **I agree toohello terms and conditions stated above** checkbox.</span></span>
4. <span data-ttu-id="e8e49-313">toobegin etablering hello resurser, klickar du på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-313">toobegin provisioning hello resources, click **Purchase**.</span></span>

<span data-ttu-id="e8e49-314">tooview hello förloppet för din distribution, klicka på meddelandeikonen hello och på **distributionen har startat**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-314">tooview hello progress of your deployment, click hello notification icon and click **Deployment started**.</span></span>

![Distributionen har påbörjats][cache-deployment-started]

<span data-ttu-id="e8e49-316">Du kan visa hello statusen för distributionen på hello **Microsoft.Template** bladet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-316">You can view hello status of your deployment on hello **Microsoft.Template** blade.</span></span>

![Distribuera tooAzure][cache-deploy-to-azure-step-3]

<span data-ttu-id="e8e49-318">När etableringen är klar, kan du publicera dina program tooAzure från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8e49-318">When provisioning is complete, you can publish your application tooAzure from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="e8e49-319">Eventuella fel som kan uppstå under etableringsprocessen hello visas på hello **Microsoft.Template** bladet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-319">Any errors that may occur during hello provisioning process are displayed on hello **Microsoft.Template** blade.</span></span> <span data-ttu-id="e8e49-320">Vanliga fel är för många SQL-servrar eller för många värdplaner för den kostnadsfria apptjänsten per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e8e49-320">Common errors are too many SQL Servers or too many Free App Service hosting plans per subscription.</span></span> <span data-ttu-id="e8e49-321">Lös eventuella fel och starta om hello processen genom att klicka på **omdistribuera** på hello **Microsoft.Template** bladet eller hello **distribuera tooAzure** knappen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-321">Resolve any errors and restart hello process by clicking **Redeploy** on hello **Microsoft.Template** blade or hello **Deploy tooAzure** button in this tutorial.</span></span>
> 
> 

## <a name="publish-hello-application-tooazure"></a><span data-ttu-id="e8e49-322">Publicera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="e8e49-322">Publish hello application tooAzure</span></span>
<span data-ttu-id="e8e49-323">I det här steget i självstudiekursen hello du publicera hello programmet tooAzure och kör det i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-323">In this step of hello tutorial, you'll publish hello application tooAzure and run it in hello cloud.</span></span>

1. <span data-ttu-id="e8e49-324">Högerklicka på hello **ContosoTeamStats** projektet i Visual Studio och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-324">Right-click hello **ContosoTeamStats** project in Visual Studio and choose **Publish**.</span></span>
   
    ![Publicera][cache-publish-app]
2. <span data-ttu-id="e8e49-326">Klicka på **Microsoft Azure App Service**, välj **Välj befintlig** och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-326">Click **Microsoft Azure App Service**, choose **Select Existing**, and click **Publish**.</span></span>
   
    ![Publicera][cache-publish-to-app-service]
3. <span data-ttu-id="e8e49-328">Välj hello-prenumeration som används när du skapar hello Azure-resurser, expandera hello resursgruppen som innehåller hello resurser och välj hello önskad Web App.</span><span class="sxs-lookup"><span data-stu-id="e8e49-328">Select hello subscription used when creating hello Azure resources, expand hello resource group containing hello resources, and select hello desired Web App.</span></span> <span data-ttu-id="e8e49-329">Om du använde hello **distribuera tooAzure** knappen Web App-namnet börjar med **webbplats** följt av vissa ytterligare tecken.</span><span class="sxs-lookup"><span data-stu-id="e8e49-329">If you used hello **Deploy tooAzure** button your Web App name starts with **webSite** followed by some additional characters.</span></span>
   
    ![Välj webbapp][cache-select-web-app]
4. <span data-ttu-id="e8e49-331">Klicka på **OK** toobegin hello publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-331">Click **OK** toobegin hello publishing process.</span></span> <span data-ttu-id="e8e49-332">Efter en liten stund hello publiceringsprocessen har slutförts och en webbläsare startas med hello kör exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-332">After a few moments hello publishing process completes and a browser is launched with hello running sample application.</span></span> <span data-ttu-id="e8e49-333">Om du får ett DNS-fel vid validering eller publicera och hello etableringsprocessen för hello Azure-resurser för programmet hello har precis slutfört, vänta en stund och försök igen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-333">If you get a DNS error when validating or publishing, and hello provisioning process for hello Azure resources for hello application has just recently completed, wait a moment and try again.</span></span>
   
    ![Cachen har lagts till][cache-added-to-application]

<span data-ttu-id="e8e49-335">hello beskrivs följande tabell varje åtgärdslänken från hello exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="e8e49-335">hello following table describes each action link from hello sample application.</span></span>

| <span data-ttu-id="e8e49-336">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="e8e49-336">Action</span></span> | <span data-ttu-id="e8e49-337">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e8e49-337">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e8e49-338">Skapa ny</span><span class="sxs-lookup"><span data-stu-id="e8e49-338">Create New</span></span> |<span data-ttu-id="e8e49-339">Skapa ett nytt team.</span><span class="sxs-lookup"><span data-stu-id="e8e49-339">Create a new Team.</span></span> |
| <span data-ttu-id="e8e49-340">Spela säsong</span><span class="sxs-lookup"><span data-stu-id="e8e49-340">Play Season</span></span> |<span data-ttu-id="e8e49-341">Spela upp ett säsongen för spel, uppdatering hello team statistik och avmarkera något inaktuell team data från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-341">Play a season of games, update hello team stats, and clear any outdated team data from hello cache.</span></span> |
| <span data-ttu-id="e8e49-342">Rensa cacheminne</span><span class="sxs-lookup"><span data-stu-id="e8e49-342">Clear Cache</span></span> |<span data-ttu-id="e8e49-343">Rensa hello team statistik från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-343">Clear hello team stats from hello cache.</span></span> |
| <span data-ttu-id="e8e49-344">Lista från cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-344">List from Cache</span></span> |<span data-ttu-id="e8e49-345">Hämta hello team statistik från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e8e49-345">Retrieve hello team stats from hello cache.</span></span> <span data-ttu-id="e8e49-346">Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache för nästa gång.</span><span class="sxs-lookup"><span data-stu-id="e8e49-346">If there is a cache miss, load hello stats from hello database and save toohello cache for next time.</span></span> |
| <span data-ttu-id="e8e49-347">Sorterad uppsättning från cachen</span><span class="sxs-lookup"><span data-stu-id="e8e49-347">Sorted Set from Cache</span></span> |<span data-ttu-id="e8e49-348">Hämta hello team statistik från hello cache med hjälp av en sortering.</span><span class="sxs-lookup"><span data-stu-id="e8e49-348">Retrieve hello team stats from hello cache using a sorted set.</span></span> <span data-ttu-id="e8e49-349">Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache med hjälp av en sortering.</span><span class="sxs-lookup"><span data-stu-id="e8e49-349">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="e8e49-350">Översta 5 teamen från cachen</span><span class="sxs-lookup"><span data-stu-id="e8e49-350">Top 5 Teams from Cache</span></span> |<span data-ttu-id="e8e49-351">Hämta hello övre 5 team från hello cache med hjälp av en sortering.</span><span class="sxs-lookup"><span data-stu-id="e8e49-351">Retrieve hello top 5 teams from hello cache using a sorted set.</span></span> <span data-ttu-id="e8e49-352">Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache med hjälp av en sortering.</span><span class="sxs-lookup"><span data-stu-id="e8e49-352">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="e8e49-353">Läsa in från databas</span><span class="sxs-lookup"><span data-stu-id="e8e49-353">Load from DB</span></span> |<span data-ttu-id="e8e49-354">Hämta hello team statistik från hello-databas.</span><span class="sxs-lookup"><span data-stu-id="e8e49-354">Retrieve hello team stats from hello database.</span></span> |
| <span data-ttu-id="e8e49-355">Återskapa databas</span><span class="sxs-lookup"><span data-stu-id="e8e49-355">Rebuild DB</span></span> |<span data-ttu-id="e8e49-356">Återskapa hello databasen och öppna den igen med exempeldata i teamet.</span><span class="sxs-lookup"><span data-stu-id="e8e49-356">Rebuild hello database and reload it with sample team data.</span></span> |
| <span data-ttu-id="e8e49-357">Redigera/Information/Ta bort</span><span class="sxs-lookup"><span data-stu-id="e8e49-357">Edit / Details / Delete</span></span> |<span data-ttu-id="e8e49-358">Redigera ett team, visa information om ett team, ta bort ett team.</span><span class="sxs-lookup"><span data-stu-id="e8e49-358">Edit a team, view details for a team, delete a team.</span></span> |

<span data-ttu-id="e8e49-359">Klicka på några av hello åtgärder och experimentera med att hämta hello data från olika källor för hello.</span><span class="sxs-lookup"><span data-stu-id="e8e49-359">Click some of hello actions and experiment with retrieving hello data from hello different sources.</span></span> <span data-ttu-id="e8e49-360">Inte hello skillnader i hello tid det tar toocomplete hello hello data hämtades från hello databasen och hello cache på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-360">Not hello differences in hello time it takes toocomplete hello various ways of retrieving hello data from hello database and hello cache.</span></span>

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a><span data-ttu-id="e8e49-361">Ta bort hello resurser när du är klar med hello program</span><span class="sxs-lookup"><span data-stu-id="e8e49-361">Delete hello resources when you are finished with hello application</span></span>
<span data-ttu-id="e8e49-362">När du är klar med hello självstudiekursen exempelprogram hello Azure kan ta bort resurser som används i ordning tooconserve kostnad och resurser.</span><span class="sxs-lookup"><span data-stu-id="e8e49-362">When you are finished with hello sample tutorial application, you can delete hello Azure resources used in order tooconserve cost and resources.</span></span> <span data-ttu-id="e8e49-363">Om du använder hello **distribuera tooAzure** knapp i hello [etablera hello Azure-resurser](#provision-the-azure-resources) avsnittet och alla dina resurser finns i hello samma resursgrupp, du kan ta bort dem tillsammans i en åtgärden genom att ta bort hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-363">If you use hello **Deploy tooAzure** button in hello [Provision hello Azure resources](#provision-the-azure-resources) section and all of your resources are contained in hello same resource group, you can delete them together in one operation by deleting hello resource group.</span></span>

1. <span data-ttu-id="e8e49-364">Logga in toohello [Azure-portalen](https://portal.azure.com) och på **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-364">Sign in toohello [Azure portal](https://portal.azure.com) and click **Resource groups**.</span></span>
2. <span data-ttu-id="e8e49-365">Ange hello namnet på resursgruppen i hello **Filtrera objekt...**  textruta.</span><span class="sxs-lookup"><span data-stu-id="e8e49-365">Type hello name of your resource group into hello **Filter items...** textbox.</span></span>
3. <span data-ttu-id="e8e49-366">Klicka på **...**  toohello höger i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-366">Click **...** toohello right of your resource group.</span></span>
4. <span data-ttu-id="e8e49-367">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-367">Click **Delete**.</span></span>
   
    ![Ta bort][cache-delete-resource-group]
5. <span data-ttu-id="e8e49-369">Hello-typnamn för din resursgrupp och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e8e49-369">Type hello name of your resource group and click **Delete**.</span></span>
   
    ![Bekräfta borttagning][cache-delete-confirm]

<span data-ttu-id="e8e49-371">Efter några ögonblick hello resurs tas gruppen och alla dess befintliga resurser bort.</span><span class="sxs-lookup"><span data-stu-id="e8e49-371">After a few moments hello resource group and all of its contained resources are deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e8e49-372">Observera att om du tar bort en resursgrupp är irreversibel och att hello resursgruppen och alla hello resurser i den tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="e8e49-372">Note that deleting a resource group is irreversible and that hello resource group and all hello resources in it are permanently deleted.</span></span> <span data-ttu-id="e8e49-373">Kontrollera att du inte oavsiktligt bort hello fel resursgrupp eller resurser.</span><span class="sxs-lookup"><span data-stu-id="e8e49-373">Make sure that you do not accidentally delete hello wrong resource group or resources.</span></span> <span data-ttu-id="e8e49-374">Om du har skapat hello resurser som värd för det här exemplet i en befintlig resursgrupp kan du ta bort varje resurs separat från deras respektive blad.</span><span class="sxs-lookup"><span data-stu-id="e8e49-374">If you created hello resources for hosting this sample inside an existing resource group, you can delete each resource individually from their respective blades.</span></span>
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a><span data-ttu-id="e8e49-375">Kör exempelprogrammet hello på din lokala dator</span><span class="sxs-lookup"><span data-stu-id="e8e49-375">Run hello sample application on your local machine</span></span>
<span data-ttu-id="e8e49-376">toorun hello program lokalt på datorn, du behöver ett Azure Redis-Cache-instansen i vilka toocache dina data.</span><span class="sxs-lookup"><span data-stu-id="e8e49-376">toorun hello application locally on your machine, you need an Azure Redis Cache instance in which toocache your data.</span></span> 

* <span data-ttu-id="e8e49-377">Om du har publicerat program-tooAzure enligt beskrivningen i föregående avsnitt i hello kan använda du hello Azure Redis-Cache-instans som har etablerats under steget.</span><span class="sxs-lookup"><span data-stu-id="e8e49-377">If you have published your application tooAzure as described in hello previous section, you can use hello Azure Redis Cache instance that was provisioned during that step.</span></span>
* <span data-ttu-id="e8e49-378">Om du har en annan befintlig Azure Redis-Cache-instans kan använda du den toorun det här exemplet lokalt.</span><span class="sxs-lookup"><span data-stu-id="e8e49-378">If you have another existing Azure Redis Cache instance, you can use that toorun this sample locally.</span></span>
* <span data-ttu-id="e8e49-379">Om du behöver toocreate en Azure Redis-Cache-instans, kan du följa hello stegen i [skapa ett cacheminne](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="e8e49-379">If you need toocreate an Azure Redis Cache instance, you can follow hello steps in [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

<span data-ttu-id="e8e49-380">När du har valt eller skapat hello cache toouse, bläddra toohello cache i hello Azure-portalen och hämta hello [värdnamn](cache-configure.md#properties) och [åtkomstnycklar](cache-configure.md#access-keys) för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="e8e49-380">Once you have selected or created hello cache toouse, browse toohello cache in hello Azure portal and retrieve hello [host name](cache-configure.md#properties) and [access keys](cache-configure.md#access-keys) for your cache.</span></span> <span data-ttu-id="e8e49-381">Instruktioner finns i [Konfigurera Redis-cacheinställningarna](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="e8e49-381">For instructions, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

1. <span data-ttu-id="e8e49-382">Öppna hello `WebAppPlusCacheAppSecrets.config` -fil som du skapade under hello [konfigurera hello programmet toouse Redis-Cache](#configure-the-application-to-use-redis-cache) steg i den här kursen använder hello redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="e8e49-382">Open hello `WebAppPlusCacheAppSecrets.config` file that you created during hello [Configure hello application toouse Redis Cache](#configure-the-application-to-use-redis-cache) step of this tutorial using hello editor of your choice.</span></span>
2. <span data-ttu-id="e8e49-383">Redigera hello `value` attributet och Ersätt `MyCache.redis.cache.windows.net` med hello [värdnamn](cache-configure.md#properties) för ditt cacheminne, och ange antingen hello [primära och sekundära nycklarna](cache-configure.md#access-keys) för ditt cacheminne som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="e8e49-383">Edit hello `value` attribute and replace `MyCache.redis.cache.windows.net` with hello [host name](cache-configure.md#properties) of your cache, and specify either hello [primary or secondary key](cache-configure.md#access-keys) of your cache as hello password.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="e8e49-384">Tryck på **Ctrl + F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e8e49-384">Press **Ctrl+F5** toorun hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="e8e49-385">Observera att eftersom hello-programmet, inklusive hello databas körs lokalt och hello Redis-Cache finns i Azure, hello cache kan visas toounder-utföra hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-385">Note that because hello application, including hello database, is running locally and hello Redis Cache is hosted in Azure, hello cache may appear toounder-perform hello database.</span></span> <span data-ttu-id="e8e49-386">För bästa prestanda hello klientprogrammet och Azure Redis-Cache-instansen måste vara i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="e8e49-386">For best performance, hello client application and Azure Redis Cache instance should be in hello same location.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e8e49-387">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8e49-387">Next steps</span></span>
* <span data-ttu-id="e8e49-388">Lär dig mer om [komma igång med ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) på hello [ASP.NET](http://asp.net/) plats.</span><span class="sxs-lookup"><span data-stu-id="e8e49-388">Learn more about [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) on hello [ASP.NET](http://asp.net/) site.</span></span>
* <span data-ttu-id="e8e49-389">Fler exempel för att skapa en ASP.NET-Webbapp i App Service finns [skapa och distribuera en ASP.NET-webbapp i Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) från hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Anslut [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="e8e49-389">For more examples of creating an ASP.NET Web App in App Service, see [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span>
  * <span data-ttu-id="e8e49-390">Läs mer Snabbstart från hello HealthClinic.biz demo [Azure Developer Tools Snabbstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="e8e49-390">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>
* <span data-ttu-id="e8e49-391">Mer information om hello [kod första tooa ny databas](https://msdn.microsoft.com/data/jj193542) hanterar tooEntity Framework som används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-391">Learn more about hello [Code first tooa new database](https://msdn.microsoft.com/data/jj193542) approach tooEntity Framework that's used in this tutorial.</span></span>
* <span data-ttu-id="e8e49-392">Läs mer om [webbappar i Azure App Service](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8e49-392">Learn more about [web apps in Azure App Service](../app-service-web/app-service-web-overview.md).</span></span>
* <span data-ttu-id="e8e49-393">Lär dig hur för[övervakaren](cache-how-to-monitor.md) ditt cacheminne i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-393">Learn how too[monitor](cache-how-to-monitor.md) your cache in hello Azure portal.</span></span>
* <span data-ttu-id="e8e49-394">Utforska Azure Redis Caches premiumfunktioner</span><span class="sxs-lookup"><span data-stu-id="e8e49-394">Explore Azure Redis Cache premium features</span></span>
  
  * [<span data-ttu-id="e8e49-395">Hur tooconfigure persistence för Premium Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-395">How tooconfigure persistence for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-persistence.md)
  * [<span data-ttu-id="e8e49-396">Hur tooconfigure klustring för Premium Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-396">How tooconfigure clustering for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-clustering.md)
  * [<span data-ttu-id="e8e49-397">Hur tooconfigure virtuella nätverket har stöd för Premium Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="e8e49-397">How tooconfigure Virtual Network support for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-vnet.md)
  * <span data-ttu-id="e8e49-398">Se hello [Azure Redis-Cache vanliga frågor och svar](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) för mer information om storlek, genomflöde och bandbredd med premium cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="e8e49-398">See hello [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) for more details about size, throughput, and bandwidth with premium caches.</span></span>

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

