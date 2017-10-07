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
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a><span data-ttu-id="c46f6-103">Skapa en ASP.NET-app i Azure med SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c46f6-103">Build an ASP.NET app in Azure with SQL Database</span></span>

<span data-ttu-id="c46f6-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="c46f6-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="c46f6-105">Den här kursen visar hur toodeploy en datadrivna ASP.NET webbapp i Azure och koppla den för[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c46f6-105">This tutorial shows you how toodeploy a data-driven ASP.NET web app in Azure and connect it too[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span></span> <span data-ttu-id="c46f6-106">När du är klar har du en ASP.NET-app som körs i [Azure App Service](../app-service/app-service-value-prop-what-is.md) och anslutna tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-106">When you're finished, you have a ASP.NET app running in [Azure App Service](../app-service/app-service-value-prop-what-is.md) and connected tooSQL Database.</span></span>

![Publicerade ASP.NET-program i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="c46f6-108">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="c46f6-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c46f6-109">Skapa en SQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="c46f6-109">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="c46f6-110">Ansluta en ASP.NET-app tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="c46f6-110">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="c46f6-111">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="c46f6-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="c46f6-112">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="c46f6-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="c46f6-113">Dataströmmen loggas från Azure tooyour terminal</span><span class="sxs-lookup"><span data-stu-id="c46f6-113">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="c46f6-114">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c46f6-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c46f6-115">Krav</span><span class="sxs-lookup"><span data-stu-id="c46f6-115">Prerequisites</span></span>

<span data-ttu-id="c46f6-116">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="c46f6-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="c46f6-117">Installera [Visual Studio 2017](https://www.visualstudio.com/downloads/) med hello följande arbetsbelastningar:</span><span class="sxs-lookup"><span data-stu-id="c46f6-117">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
  - <span data-ttu-id="c46f6-118">**ASP.NET och webbutveckling**</span><span class="sxs-lookup"><span data-stu-id="c46f6-118">**ASP.NET and web development**</span></span>
  - <span data-ttu-id="c46f6-119">**Azure Development**</span><span class="sxs-lookup"><span data-stu-id="c46f6-119">**Azure development**</span></span>

  ![ASP.NET och webbutveckling och Azure Development (under webb och moln)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="c46f6-121">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="c46f6-121">Download hello sample</span></span>

<span data-ttu-id="c46f6-122">[Hämta hello exempelprojektet](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="c46f6-122">[Download hello sample project](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span></span>

<span data-ttu-id="c46f6-123">Extrahera (packa) hello *dotnet-sqldb-kursen-master.zip* fil.</span><span class="sxs-lookup"><span data-stu-id="c46f6-123">Extract (unzip) hello  *dotnet-sqldb-tutorial-master.zip* file.</span></span>

<span data-ttu-id="c46f6-124">hello exempelprojektet innehåller en grundläggande [ASP.NET MVC](https://www.asp.net/mvc) CRUD (skapa-Läs-Uppdatera-ta bort) app med hjälp av [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="c46f6-124">hello sample project contains a basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (create-read-update-delete) app using [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="run-hello-app"></a><span data-ttu-id="c46f6-125">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="c46f6-125">Run hello app</span></span>

<span data-ttu-id="c46f6-126">Öppna hello *dotnet-sqldb-kursen-master/DotNetAppSqlDb.sln* filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c46f6-126">Open hello *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* file in Visual Studio.</span></span> 

<span data-ttu-id="c46f6-127">Typen `Ctrl+F5` toorun hello app utan felsökning.</span><span class="sxs-lookup"><span data-stu-id="c46f6-127">Type `Ctrl+F5` toorun hello app without debugging.</span></span> <span data-ttu-id="c46f6-128">hello appen visas i din standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="c46f6-128">hello app is displayed in your default browser.</span></span> <span data-ttu-id="c46f6-129">Välj hello **Skapa nytt** länka och skapa ett par *uppgiften* objekt.</span><span class="sxs-lookup"><span data-stu-id="c46f6-129">Select hello **Create New** link and create a couple *to-do* items.</span></span> 

![Dialogrutan Nytt ASP.NET-projekt](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

<span data-ttu-id="c46f6-131">Testa hello **redigera**, **information**, och **ta bort** länkar.</span><span class="sxs-lookup"><span data-stu-id="c46f6-131">Test hello **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="c46f6-132">hello appen använder en databas kontexten tooconnect med hello-databas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-132">hello app uses a database context tooconnect with hello database.</span></span> <span data-ttu-id="c46f6-133">I det här exemplet hello databaskontexten använder en anslutningssträng som heter `MyDbConnection`.</span><span class="sxs-lookup"><span data-stu-id="c46f6-133">In this sample, hello database context uses a connection string named `MyDbConnection`.</span></span> <span data-ttu-id="c46f6-134">hello anslutningssträngen har angetts i hello *Web.config* fil- och refereras till i hello *Models/MyDatabaseContext.cs* fil.</span><span class="sxs-lookup"><span data-stu-id="c46f6-134">hello connection string is set in hello *Web.config* file and referenced in hello *Models/MyDatabaseContext.cs* file.</span></span> <span data-ttu-id="c46f6-135">hello namn för anslutningssträngen används senare i hello självstudiekursen tooconnect hello Azure web app tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c46f6-135">hello connection string name is used later in hello tutorial tooconnect hello Azure web app tooan Azure SQL Database.</span></span> 

## <a name="publish-tooazure-with-sql-database"></a><span data-ttu-id="c46f6-136">Publicera tooAzure med SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c46f6-136">Publish tooAzure with SQL Database</span></span>

<span data-ttu-id="c46f6-137">I hello **Solution Explorer**, högerklicka på din **DotNetAppSqlDb** projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-137">In hello **Solution Explorer**, right-click your **DotNetAppSqlDb** project and select **Publish**.</span></span>

![Publicera från Solution Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

<span data-ttu-id="c46f6-139">Se till att **Microsoft Azure App Service** är markerat och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-139">Make sure that **Microsoft Azure App Service** is selected and click **Publish**.</span></span>

![Publicera från projektöversiktssidan](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

<span data-ttu-id="c46f6-141">Publicering öppnar hello **skapa App Service** dialogrutan, vilket hjälper dig att skapa alla hello Azure-resurser du behöver toorun ASP.NET-webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="c46f6-141">Publishing opens hello **Create App Service** dialog, which helps you create all hello Azure resources you need toorun your ASP.NET web app in Azure.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="c46f6-142">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="c46f6-142">Sign in tooAzure</span></span>

<span data-ttu-id="c46f6-143">I hello **skapa Apptjänst** dialogrutan klickar du på **Lägg till ett konto**, och logga sedan in tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c46f6-143">In hello **Create App Service** dialog, click **Add an account**, and then sign in tooyour Azure subscription.</span></span> <span data-ttu-id="c46f6-144">Om du redan är inloggad på ett Microsoft-konto kontrollerar du att kontot tillhör din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c46f6-144">If you're already signed into a Microsoft account, make sure that account holds your Azure subscription.</span></span> <span data-ttu-id="c46f6-145">Om hello inloggade Microsoft-konto inte har din Azure-prenumeration, klicka på den tooadd hello rätt konto.</span><span class="sxs-lookup"><span data-stu-id="c46f6-145">If hello signed-in Microsoft account doesn't have your Azure subscription, click it tooadd hello correct account.</span></span>
   
![Logga in tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

<span data-ttu-id="c46f6-147">När du är inloggad är du redo toocreate alla hello resurser som du behöver för din Azure-webbapp i den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c46f6-147">Once signed in, you're ready toocreate all hello resources you need for your Azure web app in this dialog.</span></span>

### <a name="configure-hello-web-app-name"></a><span data-ttu-id="c46f6-148">Konfigurera hello webbprogrammets namn</span><span class="sxs-lookup"><span data-stu-id="c46f6-148">Configure hello web app name</span></span>

<span data-ttu-id="c46f6-149">Du kan behålla hello genereras webbprogrammets namn eller ändra det tooanother unikt namn (giltiga tecken är `a-z`, `0-9`, och `-`).</span><span class="sxs-lookup"><span data-stu-id="c46f6-149">You can keep hello generated web app name, or change it tooanother unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="c46f6-150">hello webbprogrammets namn används som en del av hello standard-URL för din app (`<app_name>.azurewebsites.net`, där `<app_name>` är din webbprogrammets namn).</span><span class="sxs-lookup"><span data-stu-id="c46f6-150">hello web app name is used as part of hello default URL for your app (`<app_name>.azurewebsites.net`, where `<app_name>` is your web app name).</span></span> <span data-ttu-id="c46f6-151">hello webbprogrammets namn måste toobe unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="c46f6-151">hello web app name needs toobe unique across all apps in Azure.</span></span> 

![Skapa app service dialog](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a><span data-ttu-id="c46f6-153">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c46f6-153">Create a resource group</span></span>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="c46f6-154">Nästa för**resursgruppen**, klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-154">Next too**Resource Group**, click **New**.</span></span>

![Nästa tooResource gruppen, klicka på nytt.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

<span data-ttu-id="c46f6-156">Namnet hello resursgruppen **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-156">Name hello resource group **myResourceGroup**.</span></span>

> [!NOTE]
> <span data-ttu-id="c46f6-157">Klicka inte på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-157">Do not click **Create**.</span></span> <span data-ttu-id="c46f6-158">Du måste först tooset upp en SQL-databas i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="c46f6-158">You first need tooset up a SQL Database in a later step.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="c46f6-159">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="c46f6-159">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="c46f6-160">Nästa för**Apptjänstplan**, klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-160">Next too**App Service Plan**, click **New**.</span></span> 

<span data-ttu-id="c46f6-161">I hello **konfigurera Apptjänstplan** dialogrutan Konfigurera hello nya App Service-plan med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="c46f6-161">In hello **Configure App Service Plan** dialog, configure hello new App Service plan with hello following settings:</span></span>

![Skapa apptjänstplan](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| <span data-ttu-id="c46f6-163">Inställning</span><span class="sxs-lookup"><span data-stu-id="c46f6-163">Setting</span></span>  | <span data-ttu-id="c46f6-164">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c46f6-164">Suggested value</span></span> | <span data-ttu-id="c46f6-165">Mer information</span><span class="sxs-lookup"><span data-stu-id="c46f6-165">For more information</span></span> |
| ----------------- | ------------ | ----|
|<span data-ttu-id="c46f6-166">**App Service-Plan**</span><span class="sxs-lookup"><span data-stu-id="c46f6-166">**App Service Plan**</span></span>| <span data-ttu-id="c46f6-167">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c46f6-167">myAppServicePlan</span></span> | [<span data-ttu-id="c46f6-168">App Service-planer</span><span class="sxs-lookup"><span data-stu-id="c46f6-168">App Service plans</span></span>](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|<span data-ttu-id="c46f6-169">**Plats**</span><span class="sxs-lookup"><span data-stu-id="c46f6-169">**Location**</span></span>| <span data-ttu-id="c46f6-170">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="c46f6-170">West Europe</span></span> | [<span data-ttu-id="c46f6-171">Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="c46f6-171">Azure regions</span></span>](https://azure.microsoft.com/regions/) |
|<span data-ttu-id="c46f6-172">**Storlek**</span><span class="sxs-lookup"><span data-stu-id="c46f6-172">**Size**</span></span>| <span data-ttu-id="c46f6-173">Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="c46f6-173">Free</span></span> | [<span data-ttu-id="c46f6-174">Prisnivåer</span><span class="sxs-lookup"><span data-stu-id="c46f6-174">Pricing tiers</span></span>](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a><span data-ttu-id="c46f6-175">Skapa en SQL Server-instans</span><span class="sxs-lookup"><span data-stu-id="c46f6-175">Create a SQL Server instance</span></span>

<span data-ttu-id="c46f6-176">Innan du skapar en databas måste en [logisk Azure SQL Database-server](../sql-database/sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="c46f6-176">Before creating a database, you need an [Azure SQL Database logical server](../sql-database/sql-database-features.md).</span></span> <span data-ttu-id="c46f6-177">En logisk server innehåller en uppsättning databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="c46f6-177">A logical server contains a group of databases managed as a group.</span></span>

<span data-ttu-id="c46f6-178">Välj **utforska ytterligare Azure-tjänster**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-178">Select **Explore additional Azure services**.</span></span>

![Ange webbappnamn](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

<span data-ttu-id="c46f6-180">I hello **Services** klickar du på hello  **+**  ikonen nästa för**SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-180">In hello **Services** tab, click hello **+** icon next too**SQL Database**.</span></span> 

![I fliken hello tjänster klickar du på hello + ikonen nästa tooSQL databas.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

<span data-ttu-id="c46f6-182">I hello **Konfigurera SQL-databas** dialogrutan klickar du på **ny** nästa för**SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-182">In hello **Configure SQL Database** dialog, click **New** next too**SQL Server**.</span></span> 

<span data-ttu-id="c46f6-183">Ett unikt servernamn genereras.</span><span class="sxs-lookup"><span data-stu-id="c46f6-183">A unique server name is generated.</span></span> <span data-ttu-id="c46f6-184">Det här namnet används som en del av hello standard-URL för din logiska server `<server_name>.database.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="c46f6-184">This name is used as part of hello default URL for your logical server, `<server_name>.database.windows.net`.</span></span> <span data-ttu-id="c46f6-185">Det måste vara unikt över alla logiska serverinstanser i Azure.</span><span class="sxs-lookup"><span data-stu-id="c46f6-185">It must be unique across all logical server instances in Azure.</span></span> <span data-ttu-id="c46f6-186">Du kan ändra hello servernamn, men behålla hello genereras värdet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-186">You can change hello server name, but for this tutorial, keep hello generated value.</span></span>

<span data-ttu-id="c46f6-187">Lägga till en administratörsanvändarnamn och lösenord och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-187">Add an administrator username and password, and then select **OK**.</span></span> <span data-ttu-id="c46f6-188">Kraven på lösenordskomplexitet, se [lösenordsprincip](/sql/relational-databases/security/password-policy).</span><span class="sxs-lookup"><span data-stu-id="c46f6-188">For password complexity requirements, see [Password Policy](/sql/relational-databases/security/password-policy).</span></span>

<span data-ttu-id="c46f6-189">Kom ihåg detta användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c46f6-189">Remember this username and password.</span></span> <span data-ttu-id="c46f6-190">Du behöver toomanage hello logisk server-instansen senare.</span><span class="sxs-lookup"><span data-stu-id="c46f6-190">You need them toomanage hello logical server instance later.</span></span>

![Skapa SQL Server-instans](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a><span data-ttu-id="c46f6-192">Skapa en SQL Database</span><span class="sxs-lookup"><span data-stu-id="c46f6-192">Create a SQL Database</span></span>

<span data-ttu-id="c46f6-193">I hello **Konfigurera SQL-databas** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="c46f6-193">In hello **Configure SQL Database** dialog:</span></span> 

* <span data-ttu-id="c46f6-194">Behåll hello standardgenererade **databasnamnet**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-194">Keep hello default generated **Database Name**.</span></span>
* <span data-ttu-id="c46f6-195">I **namn för anslutningssträngen**, typen *MyDbConnection*.</span><span class="sxs-lookup"><span data-stu-id="c46f6-195">In **Connection String Name**, type *MyDbConnection*.</span></span> <span data-ttu-id="c46f6-196">Det här namnet måste matcha hello anslutningssträngen som refereras i *Models/MyDatabaseContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="c46f6-196">This name must match hello connection string that is referenced in *Models/MyDatabaseContext.cs*.</span></span>
* <span data-ttu-id="c46f6-197">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-197">Select **OK**.</span></span>

![Konfigurera SQL-databas](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

<span data-ttu-id="c46f6-199">Hej **skapa App Service** visar hello resurser du har skapat.</span><span class="sxs-lookup"><span data-stu-id="c46f6-199">hello **Create App Service** dialog shows hello resources you've created.</span></span> <span data-ttu-id="c46f6-200">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-200">Click **Create**.</span></span> 

![hello-resurser som du har skapat](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

<span data-ttu-id="c46f6-202">När hello guiden är klar att skapa hello Azure-resurser, publicerar tooAzure din ASP.NET-app.</span><span class="sxs-lookup"><span data-stu-id="c46f6-202">Once hello wizard finishes creating hello Azure resources, it  publishes your ASP.NET app tooAzure.</span></span> <span data-ttu-id="c46f6-203">Standardwebbläsaren startas med hello URL toohello distribuerade appen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-203">Your default browser is launched with hello URL toohello deployed app.</span></span> 

<span data-ttu-id="c46f6-204">Lägg till några arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c46f6-204">Add a few to-do items.</span></span>

![Publicerade ASP.NET-program i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="c46f6-206">Grattis!</span><span class="sxs-lookup"><span data-stu-id="c46f6-206">Congratulations!</span></span> <span data-ttu-id="c46f6-207">ASP.NET-programmet datadrivna körs live i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c46f6-207">Your data-driven ASP.NET application is running live in Azure App Service.</span></span>

## <a name="access-hello-sql-database-locally"></a><span data-ttu-id="c46f6-208">Komma åt lokalt hello SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c46f6-208">Access hello SQL Database locally</span></span>

<span data-ttu-id="c46f6-209">Visual Studio kan du hantera din nya SQL-databas enkelt i hello och utforska **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-209">Visual Studio lets you explore and manage your new SQL Database easily in hello **SQL Server Object Explorer**.</span></span>

### <a name="create-a-database-connection"></a><span data-ttu-id="c46f6-210">Skapa en databasanslutning</span><span class="sxs-lookup"><span data-stu-id="c46f6-210">Create a database connection</span></span>

<span data-ttu-id="c46f6-211">Från hello **visa** väljer du **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-211">From hello **View** menu, select **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="c46f6-212">Hello överst i **SQL Server Object Explorer**, klicka på hello **Lägg till SQL Server** knappen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-212">At hello top of **SQL Server Object Explorer**, click hello **Add SQL Server** button.</span></span>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="c46f6-213">Konfigurera hello databasanslutning</span><span class="sxs-lookup"><span data-stu-id="c46f6-213">Configure hello database connection</span></span>

<span data-ttu-id="c46f6-214">I hello **Anslut** dialogrutan Expandera hello **Azure** nod.</span><span class="sxs-lookup"><span data-stu-id="c46f6-214">In hello **Connect** dialog, expand hello **Azure** node.</span></span> <span data-ttu-id="c46f6-215">Alla SQL-databas instanser i Azure anges här.</span><span class="sxs-lookup"><span data-stu-id="c46f6-215">All your SQL Database instances in Azure are listed here.</span></span>

<span data-ttu-id="c46f6-216">Välj hello `DotNetAppSqlDb` SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-216">Select hello `DotNetAppSqlDb` SQL Database.</span></span> <span data-ttu-id="c46f6-217">hello-anslutning som du skapade tidigare fylls automatiskt längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c46f6-217">hello connection you created earlier is automatically filled at hello bottom.</span></span>

<span data-ttu-id="c46f6-218">Ange hello databasadministratörens lösenord du skapade tidigare och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-218">Type hello database administrator password you created earlier and click **Connect**.</span></span>

![Konfigurera databasanslutningen från Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a><span data-ttu-id="c46f6-220">Tillåta klientanslutning från datorn</span><span class="sxs-lookup"><span data-stu-id="c46f6-220">Allow client connection from your computer</span></span>

<span data-ttu-id="c46f6-221">Hej **skapa en ny brandväggsregel** dialogrutan öppnas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-221">hello **Create a new firewall rule** dialog is opened.</span></span> <span data-ttu-id="c46f6-222">Som standard kan bara anslutningar från Azure-tjänster, till exempel Azure-webbapp i SQL Database-instans.</span><span class="sxs-lookup"><span data-stu-id="c46f6-222">By default, your SQL Database instance only allows connections from Azure services, such as your Azure web app.</span></span> <span data-ttu-id="c46f6-223">tooconnect tooyour databasen, skapa en brandväggsregel i hello SQL Database-instans.</span><span class="sxs-lookup"><span data-stu-id="c46f6-223">tooconnect tooyour database, create a firewall rule in hello SQL Database instance.</span></span> <span data-ttu-id="c46f6-224">hello brandväggsregel tillåter hello offentliga IP-adressen för den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c46f6-224">hello firewall rule allows hello public IP address of your local computer.</span></span>

<span data-ttu-id="c46f6-225">hello dialogrutan är redan fylld med datorns offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-225">hello dialog is already filled with your computer's public IP address.</span></span>

<span data-ttu-id="c46f6-226">Se till att **lägga till klientens IP-Adressen** är markerad och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-226">Make sure that **Add my client IP** is selected and click **OK**.</span></span> 

![Ställa in brandväggen för SQL Database-instans](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

<span data-ttu-id="c46f6-228">När Visual Studio har skapat hello brandväggsinställningen för din instans av SQL-databas, din anslutning visas i **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-228">Once Visual Studio finishes creating hello firewall setting for your SQL Database instance, your connection shows up in **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="c46f6-229">Här kan du utföra hello vanligaste databasåtgärder, till exempel kör frågor, skapa vyer och lagrade procedurer och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="c46f6-229">Here, you can perform hello most common database operations, such as run queries, create views and stored procedures, and more.</span></span> 

<span data-ttu-id="c46f6-230">Högerklicka på hello `Todoes` tabell och välj **visa Data**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-230">Right-click on hello `Todoes` table and select **View Data**.</span></span> 

![Utforska SQL Database-objekt](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a><span data-ttu-id="c46f6-232">Uppdatera app med Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="c46f6-232">Update app with Code First Migrations</span></span>

<span data-ttu-id="c46f6-233">Du kan använda hello välbekanta verktyg i Visual Studio tooupdate din databas och webb-app i Azure.</span><span class="sxs-lookup"><span data-stu-id="c46f6-233">You can use hello familiar tools in Visual Studio tooupdate your database and web app in Azure.</span></span> <span data-ttu-id="c46f6-234">I det här steget använder Code First Migrations i Entity Framework toomake ett ändra tooyour databasschema och publicerar den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c46f6-234">In this step, you use Code First Migrations in Entity Framework toomake a change tooyour database schema and publish it tooAzure.</span></span>

<span data-ttu-id="c46f6-235">Läs mer om hur du använder Entity Framework Code First Migrations [komma igång med Entity Framework 6 Code First med MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="c46f6-235">For more information about using Entity Framework Code First Migrations, see [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="update-your-data-model"></a><span data-ttu-id="c46f6-236">Uppdatera datamodellen</span><span class="sxs-lookup"><span data-stu-id="c46f6-236">Update your data model</span></span>

<span data-ttu-id="c46f6-237">Öppna _Models\Todo.cs_ i hello redigerare.</span><span class="sxs-lookup"><span data-stu-id="c46f6-237">Open _Models\Todo.cs_ in hello code editor.</span></span> <span data-ttu-id="c46f6-238">Lägg till följande egenskapen toohello hello `ToDo` klass:</span><span class="sxs-lookup"><span data-stu-id="c46f6-238">Add hello following property toohello `ToDo` class:</span></span>

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a><span data-ttu-id="c46f6-239">Kör Code First Migrations lokalt</span><span class="sxs-lookup"><span data-stu-id="c46f6-239">Run Code First Migrations locally</span></span>

<span data-ttu-id="c46f6-240">Kör några kommandon toomake uppdateringar tooyour lokal databas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-240">Run a few commands toomake updates tooyour local database.</span></span> 

<span data-ttu-id="c46f6-241">Från hello **verktyg** -menyn klickar du på **NuGet Package Manager** > **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-241">From hello **Tools** menu, click **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="c46f6-242">Aktivera Code First Migrations i hello fönstret Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="c46f6-242">In hello Package Manager Console window, enable Code First Migrations:</span></span>

```PowerShell
Enable-Migrations
```

<span data-ttu-id="c46f6-243">Lägg till en migrering:</span><span class="sxs-lookup"><span data-stu-id="c46f6-243">Add a migration:</span></span>

```PowerShell
Add-Migration AddProperty
```

<span data-ttu-id="c46f6-244">Uppdatera hello lokal databas:</span><span class="sxs-lookup"><span data-stu-id="c46f6-244">Update hello local database:</span></span>

```PowerShell
Update-Database
```

<span data-ttu-id="c46f6-245">Typen `Ctrl+F5` toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="c46f6-245">Type `Ctrl+F5` toorun hello app.</span></span> <span data-ttu-id="c46f6-246">Testa hello redigera detaljer, och skapa länkar.</span><span class="sxs-lookup"><span data-stu-id="c46f6-246">Test hello edit, details, and create links.</span></span>

<span data-ttu-id="c46f6-247">Om programmet hello läser in utan fel, har Code First Migrations slutförts.</span><span class="sxs-lookup"><span data-stu-id="c46f6-247">If hello application loads without errors, then Code First Migrations has succeeded.</span></span> <span data-ttu-id="c46f6-248">Dock hello sidan fortfarande ser ut samma eftersom applogiken inte är ännu använder den här nya egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-248">However, your page still looks hello same because your application logic is not using this new property yet.</span></span> 

### <a name="use-hello-new-property"></a><span data-ttu-id="c46f6-249">Använd hello ny egenskap</span><span class="sxs-lookup"><span data-stu-id="c46f6-249">Use hello new property</span></span>

<span data-ttu-id="c46f6-250">Göra några ändringar i din kod toouse hello `Done` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-250">Make some changes in your code toouse hello `Done` property.</span></span> <span data-ttu-id="c46f6-251">För enkelhetens i den här självstudiekursen ska endast toochange hello `Index` och `Create` toosee hello egenskapen-vyer i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c46f6-251">For simplicity in this tutorial, you're only going toochange hello `Index` and `Create` views toosee hello property in action.</span></span>

<span data-ttu-id="c46f6-252">Öppna _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="c46f6-252">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="c46f6-253">Hitta hello `Create()` metod och Lägg till `Done` toohello lista över egenskaper i hello `Bind` attribut.</span><span class="sxs-lookup"><span data-stu-id="c46f6-253">Find hello `Create()` method and add `Done` toohello list of properties in hello `Bind` attribute.</span></span> <span data-ttu-id="c46f6-254">När du är klar din `Create()` Metodsignaturen ser ut som följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="c46f6-254">When you're done, your `Create()` method signature looks like hello following code:</span></span>

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

<span data-ttu-id="c46f6-255">Öppna _Views\Todos\Create.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="c46f6-255">Open _Views\Todos\Create.cshtml_.</span></span>

<span data-ttu-id="c46f6-256">I hello Razor kod, bör du se en `<div class="form-group">` element som använder `model.Description`, och sedan en annan `<div class="form-group">` element som använder `model.CreatedDate`.</span><span class="sxs-lookup"><span data-stu-id="c46f6-256">In hello Razor code, you should see a `<div class="form-group">` element that uses `model.Description`, and then another `<div class="form-group">` element that uses `model.CreatedDate`.</span></span> <span data-ttu-id="c46f6-257">Direkt efter dessa två element, lägga till en annan `<div class="form-group">` element som använder `model.Done`:</span><span class="sxs-lookup"><span data-stu-id="c46f6-257">Immediately following these two elements, add another `<div class="form-group">` element that uses `model.Done`:</span></span>

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

<span data-ttu-id="c46f6-258">Öppna _Views\Todos\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="c46f6-258">Open _Views\Todos\Index.cshtml_.</span></span>

<span data-ttu-id="c46f6-259">Sök efter hello tom `<th></th>` element.</span><span class="sxs-lookup"><span data-stu-id="c46f6-259">Search for hello empty `<th></th>` element.</span></span> <span data-ttu-id="c46f6-260">Lägg till hello följande Razor kod ovanför det här elementet:</span><span class="sxs-lookup"><span data-stu-id="c46f6-260">Just above this element, add hello following Razor code:</span></span>

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

<span data-ttu-id="c46f6-261">Hitta hello `<td>` element som innehåller hello `Html.ActionLink()` hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="c46f6-261">Find hello `<td>` element that contains hello `Html.ActionLink()` helper methods.</span></span> <span data-ttu-id="c46f6-262">Lägg till hello följande Razor kod ovanför det här elementet:</span><span class="sxs-lookup"><span data-stu-id="c46f6-262">Just above this element, add hello following Razor code:</span></span>

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

<span data-ttu-id="c46f6-263">Det är allt du behöver toosee hello ändringar i hello `Index` och `Create` vyer.</span><span class="sxs-lookup"><span data-stu-id="c46f6-263">That's all you need toosee hello changes in hello `Index` and `Create` views.</span></span> 

<span data-ttu-id="c46f6-264">Typen `Ctrl+F5` toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="c46f6-264">Type `Ctrl+F5` toorun hello app.</span></span>

<span data-ttu-id="c46f6-265">Du kan nu lägga till en att göra-objekt och kontrollera **klar**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-265">You can now add a to-do item and check **Done**.</span></span> <span data-ttu-id="c46f6-266">Sedan ska den visas i din startsida som en slutförd artikel.</span><span class="sxs-lookup"><span data-stu-id="c46f6-266">Then it should show up in your homepage as a completed item.</span></span> <span data-ttu-id="c46f6-267">Kom ihåg att hello `Edit` vyn inte visar hello `Done` fältet, eftersom du inte ändra hello `Edit` vyn.</span><span class="sxs-lookup"><span data-stu-id="c46f6-267">Remember that hello `Edit` view doesn't show hello `Done` field, because you didn't change hello `Edit` view.</span></span>

### <a name="enable-code-first-migrations-in-azure"></a><span data-ttu-id="c46f6-268">Aktivera Code First Migrations i Azure</span><span class="sxs-lookup"><span data-stu-id="c46f6-268">Enable Code First Migrations in Azure</span></span>

<span data-ttu-id="c46f6-269">Nu när koden ändra fungerar, inklusive Databasmigrering publicera den tooyour Azure webbapp och uppdatera din SQL-databas med Code First Migrations för.</span><span class="sxs-lookup"><span data-stu-id="c46f6-269">Now that your code change works, including database migration, you publish it tooyour Azure web app and update your SQL Database with Code First Migrations too.</span></span>

<span data-ttu-id="c46f6-270">Precis innan, högerklicka på projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-270">Just like before, right-click your project and select **Publish**.</span></span>

<span data-ttu-id="c46f6-271">Klicka på **inställningar** tooopen hello Publiceringsguiden.</span><span class="sxs-lookup"><span data-stu-id="c46f6-271">Click **Settings** tooopen hello publish wizard.</span></span>

![Öppna Publiceringsinställningar](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

<span data-ttu-id="c46f6-273">Klicka på hello guiden **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-273">In hello wizard, click **Next**.</span></span>

<span data-ttu-id="c46f6-274">Kontrollera att den hello-anslutningssträngen för SQL-databasen fylls i **MyDatabaseContext (MyDbConnection)**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-274">Make sure that hello connection string for your SQL Database is populated in **MyDatabaseContext (MyDbConnection)**.</span></span> <span data-ttu-id="c46f6-275">Du kan behöva tooselect hello **myToDoAppDb** databasen hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="c46f6-275">You may need tooselect hello **myToDoAppDb** database from hello dropdown.</span></span> 

<span data-ttu-id="c46f6-276">Välj **köra Code First Migrations (körs vid programstart)**, klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-276">Select **Execute Code First Migrations (runs on application start)**, then click **Save**.</span></span>

![Aktivera Code First Migrations i Azure webbapp](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a><span data-ttu-id="c46f6-278">Publicera ändringarna</span><span class="sxs-lookup"><span data-stu-id="c46f6-278">Publish your changes</span></span>

<span data-ttu-id="c46f6-279">Nu när du har aktiverat Code First Migrations i ditt Azure webbapp publicera kodändringarna.</span><span class="sxs-lookup"><span data-stu-id="c46f6-279">Now that you enabled Code First Migrations in your Azure web app, publish your code changes.</span></span>

<span data-ttu-id="c46f6-280">I hello publicera sida klickar du på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-280">In hello publish page, click **Publish**.</span></span>

<span data-ttu-id="c46f6-281">Försök att lägga till arbetsuppgifter igen och välj **klar**, och de visas i din startsida som en slutförd artikel.</span><span class="sxs-lookup"><span data-stu-id="c46f6-281">Try adding to-do items again and select **Done**, and they should show up in your homepage as a completed item.</span></span>

![Azure-webbapp efter koden första migrering](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

<span data-ttu-id="c46f6-283">Alla befintliga arbetsuppgifter fortfarande visas.</span><span class="sxs-lookup"><span data-stu-id="c46f6-283">All your existing to-do items are still displayed.</span></span> <span data-ttu-id="c46f6-284">Befintliga data i SQL-databasen är inte förlorade när du publicerar ASP.NET-programmet.</span><span class="sxs-lookup"><span data-stu-id="c46f6-284">When you republish your ASP.NET application, existing data in your SQL Database is not lost.</span></span> <span data-ttu-id="c46f6-285">Dessutom Code First Migrations bara ändras hello dataschemat och lämnar företaget dina befintliga data.</span><span class="sxs-lookup"><span data-stu-id="c46f6-285">Also, Code First Migrations only changes hello data schema and leaves your existing data intact.</span></span>


## <a name="stream-application-logs"></a><span data-ttu-id="c46f6-286">Dataströmmen programloggar</span><span class="sxs-lookup"><span data-stu-id="c46f6-286">Stream application logs</span></span>

<span data-ttu-id="c46f6-287">Du kan strömma spårning av meddelanden direkt från din Azure web app tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="c46f6-287">You can stream tracing messages directly from your Azure web app tooVisual Studio.</span></span>

<span data-ttu-id="c46f6-288">Öppna _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="c46f6-288">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="c46f6-289">Varje åtgärd som börjar med en `Trace.WriteLine()` metod.</span><span class="sxs-lookup"><span data-stu-id="c46f6-289">Each action starts with a `Trace.WriteLine()` method.</span></span> <span data-ttu-id="c46f6-290">Den här koden läggs tooshow du hur tooadd trace meddelanden tooyour Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="c46f6-290">This code is added tooshow you how tooadd trace messages tooyour Azure web app.</span></span>

### <a name="open-server-explorer"></a><span data-ttu-id="c46f6-291">Öppna Server Explorer</span><span class="sxs-lookup"><span data-stu-id="c46f6-291">Open Server Explorer</span></span>

<span data-ttu-id="c46f6-292">Från hello **visa** väljer du **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-292">From hello **View** menu, select **Server Explorer**.</span></span> <span data-ttu-id="c46f6-293">Du kan konfigurera loggning för din Azure-webbapp i **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-293">You can configure logging for your Azure web app in **Server Explorer**.</span></span> 

### <a name="enable-log-streaming"></a><span data-ttu-id="c46f6-294">Strömning logg</span><span class="sxs-lookup"><span data-stu-id="c46f6-294">Enable log streaming</span></span>

<span data-ttu-id="c46f6-295">I **Server Explorer**, expandera **Azure** > **Apptjänst**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-295">In **Server Explorer**, expand **Azure** > **App Service**.</span></span>

<span data-ttu-id="c46f6-296">Expandera hello **myResourceGroup** resursgrupp, som du skapade när du först skapade hello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="c46f6-296">Expand hello **myResourceGroup** resource group, you created when you first created hello Azure web app.</span></span>

<span data-ttu-id="c46f6-297">Högerklicka på ditt Azure webbapp och markera **visa strömning loggfiler**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-297">Right-click your Azure web app and select **View Streaming Logs**.</span></span>

![Strömning logg](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

<span data-ttu-id="c46f6-299">hello loggar nu strömmas till hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="c46f6-299">hello logs are now streamed into hello **Output** window.</span></span> 

![Loggen strömning i utdatafönstret](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

<span data-ttu-id="c46f6-301">Men ser du inte något av spårning hälsningsmeddelande ännu.</span><span class="sxs-lookup"><span data-stu-id="c46f6-301">However, you don't see any of hello trace messages yet.</span></span> <span data-ttu-id="c46f6-302">Som eftersom när du väljer **visa strömning loggfiler**, Azure-webbapp anger hello Spårningsnivån för`Error`, som endast loggar felhändelser (med hello `Trace.TraceError()` metod).</span><span class="sxs-lookup"><span data-stu-id="c46f6-302">That's because when you first select **View Streaming Logs**, your Azure web app sets hello trace level too`Error`, which only logs error events (with hello `Trace.TraceError()` method).</span></span>

### <a name="change-trace-levels"></a><span data-ttu-id="c46f6-303">Ändra spårningsnivåer</span><span class="sxs-lookup"><span data-stu-id="c46f6-303">Change trace levels</span></span>

<span data-ttu-id="c46f6-304">toochange hello trace nivåer toooutput andra trace meddelanden, gå tillbaka för**Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-304">toochange hello trace levels toooutput other trace messages, go back too**Server Explorer**.</span></span>

<span data-ttu-id="c46f6-305">Högerklicka på ditt Azure webbapp igen och välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-305">Right-click your Azure web app again and select **Settings**.</span></span>

<span data-ttu-id="c46f6-306">I hello **programloggning (filsystem)** listrutan **utförlig**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-306">In hello **Application Logging (File System)** dropdown, select **Verbose**.</span></span> <span data-ttu-id="c46f6-307">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c46f6-307">Click **Save**.</span></span>

![Ändra trace nivå tooVerbose](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> <span data-ttu-id="c46f6-309">Du kan experimentera med olika trace nivåer toosee vilka typer av meddelanden visas för varje nivå.</span><span class="sxs-lookup"><span data-stu-id="c46f6-309">You can experiment with different trace levels toosee what types of messages are displayed for each level.</span></span> <span data-ttu-id="c46f6-310">Till exempel hello **Information** nivån innehåller alla loggar som skapats av `Trace.TraceInformation()`, `Trace.TraceWarning()`, och `Trace.TraceError()`, men inte loggar som skapats av `Trace.WriteLine()`.</span><span class="sxs-lookup"><span data-stu-id="c46f6-310">For example, hello **Information** level includes all logs created by `Trace.TraceInformation()`, `Trace.TraceWarning()`, and `Trace.TraceError()`, but not logs created by `Trace.WriteLine()`.</span></span>
>
>

<span data-ttu-id="c46f6-311">Försök att klicka på runt hello att göra-App i Azure i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c46f6-311">In your browser, try clicking around hello to-do list application in Azure.</span></span> <span data-ttu-id="c46f6-312">spåra hälsningsmeddelande strömmas nu toohello **utdata** fönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c46f6-312">hello trace messages are now streamed toohello **Output** window in Visual Studio.</span></span>

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a><span data-ttu-id="c46f6-313">Stoppa strömning logg</span><span class="sxs-lookup"><span data-stu-id="c46f6-313">Stop log streaming</span></span>

<span data-ttu-id="c46f6-314">toostop hello log-streaming-tjänsten klickar du på hello **stoppa övervakningen av** knapp i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="c46f6-314">toostop hello log-streaming service, click hello **Stop monitoring** button in hello **Output** window.</span></span>

![Stoppa strömning logg](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="c46f6-316">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="c46f6-316">Manage your Azure web app</span></span>

<span data-ttu-id="c46f6-317">Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c46f6-317">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span> 



<span data-ttu-id="c46f6-318">Hello vänstra menyn klickar du på **Apptjänst**, klicka på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="c46f6-318">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

<span data-ttu-id="c46f6-320">Du har landat i ditt webbprogram sidan.</span><span class="sxs-lookup"><span data-stu-id="c46f6-320">You have landed in your web app's page.</span></span> 

<span data-ttu-id="c46f6-321">Som standard visar hello portal hello **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="c46f6-321">By default, hello portal shows hello **Overview** page.</span></span> <span data-ttu-id="c46f6-322">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="c46f6-322">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="c46f6-323">Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="c46f6-323">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="c46f6-324">hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="c46f6-324">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span> 

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="c46f6-326">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c46f6-326">Next steps</span></span>

<span data-ttu-id="c46f6-327">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="c46f6-327">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c46f6-328">Skapa en SQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="c46f6-328">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="c46f6-329">Ansluta en ASP.NET-app tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="c46f6-329">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="c46f6-330">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="c46f6-330">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="c46f6-331">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="c46f6-331">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="c46f6-332">Dataströmmen loggas från Azure tooyour terminal</span><span class="sxs-lookup"><span data-stu-id="c46f6-332">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="c46f6-333">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c46f6-333">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="c46f6-334">I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c46f6-334">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c46f6-335">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="c46f6-335">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
