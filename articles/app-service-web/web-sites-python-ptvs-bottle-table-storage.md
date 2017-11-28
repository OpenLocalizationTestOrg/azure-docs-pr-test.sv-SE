---
title: "aaaBottle och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate ett Bottle program som lagrar data i Azure Table Storage och distribuera hello web app tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: f075124b-db79-4e51-b394-09187dd6c634
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 25b9eb002b8748483d5b9458b7b5860a958b4bb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="e6c28-103">Bottle och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6c28-103">Bottle and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="e6c28-104">I den här kursen använder vi [Python Tools för Visual Studio] toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS.</span><span class="sxs-lookup"><span data-stu-id="e6c28-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="e6c28-105">Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span><span class="sxs-lookup"><span data-stu-id="e6c28-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span></span>

<span data-ttu-id="e6c28-106">Hej webbapp definierar en abstraktion för dess databas, så du kan enkelt växla mellan olika typer av databaser (i minnet, Azure Table Storage MongoDB).</span><span class="sxs-lookup"><span data-stu-id="e6c28-106">hello polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="e6c28-107">Vi lära dig hur toocreate ett Azure Storage-konto, hur tooconfigure hello web app toouse Azure Table Storage och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="e6c28-107">We'll learn how toocreate an Azure Storage account, how tooconfigure hello web app toouse Azure Table Storage, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="e6c28-108">Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med MongoDB, Azure Table Storage, MySQL och SQL Database-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e6c28-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="e6c28-109">Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="e6c28-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6c28-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e6c28-110">Prerequisites</span></span>
* <span data-ttu-id="e6c28-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="e6c28-111">Visual Studio 2015</span></span>
* <span data-ttu-id="e6c28-112">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="e6c28-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="e6c28-113">[Python Tools 2.2 för Visual Studio Samples VSI]</span><span class="sxs-lookup"><span data-stu-id="e6c28-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="e6c28-114">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="e6c28-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="e6c28-115">[Python 2.7 32-bitars] eller [Python 3.4 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="e6c28-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="e6c28-116">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="e6c28-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e6c28-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="e6c28-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="e6c28-118">Skapa hello projekt</span><span class="sxs-lookup"><span data-stu-id="e6c28-118">Create hello Project</span></span>
<span data-ttu-id="e6c28-119">I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="e6c28-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="e6c28-120">Vi skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="e6c28-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="e6c28-121">Sedan kör vi hello programmet lokalt med hjälp av hello standarddatabas i minnet.</span><span class="sxs-lookup"><span data-stu-id="e6c28-121">Then we'll run hello application locally using hello default in-memory repository.</span></span>

1. <span data-ttu-id="e6c28-122">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="e6c28-123">Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-123">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="e6c28-124">Välj **avsökningar Bottle webbprojekt** och klicka på OK toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="e6c28-124">Select **Polls Bottle Web Project** and click OK toocreate hello project.</span></span>
   
     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. <span data-ttu-id="e6c28-126">Du kommer att tillfrågas tooinstall externa paket.</span><span class="sxs-lookup"><span data-stu-id="e6c28-126">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="e6c28-127">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="e6c28-127">Select **Install into a virtual environment**.</span></span>
   
     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. <span data-ttu-id="e6c28-129">Välj **Python 2.7** eller **Python 3.4** som hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="e6c28-129">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="e6c28-131">Bekräfta att hello programmet fungerar genom att trycka på `F5`.</span><span class="sxs-lookup"><span data-stu-id="e6c28-131">Confirm that hello application works by pressing `F5`.</span></span> <span data-ttu-id="e6c28-132">Som standard använder programmet hello en InMemory-lagringsplats som inte kräver någon konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e6c28-132">By default, hello application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="e6c28-133">Alla data går förlorade när hello webbservern har stoppats.</span><span class="sxs-lookup"><span data-stu-id="e6c28-133">All data is lost when hello web server is stopped.</span></span>
6. <span data-ttu-id="e6c28-134">Klicka på **skapa Exempelomröstningar**, klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="e6c28-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="e6c28-136">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="e6c28-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="e6c28-137">toouse lagringsåtgärder du behöver ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e6c28-137">toouse storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="e6c28-138">Du kan skapa ett lagringskonto genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="e6c28-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="e6c28-139">Logga in på hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e6c28-139">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e6c28-140">Klicka på hello **ny** ikonen hello längst upp till vänster i hello Portal och klicka sedan på **Data + lagring** > **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-140">Click hello **New** icon on hello top left of hello Portal, then click **Data + Storage** > **Storage Account**.</span></span>  <span data-ttu-id="e6c28-141">Klicka på hello **skapa** knappen ge hello storage-konto ett unikt namn och sedan skapa en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md) för den.</span><span class="sxs-lookup"><span data-stu-id="e6c28-141">Click hello **Create** button, then give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Snabbregistrering](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="e6c28-143">När hello storage-konto har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello lagringskontots blad är öppet tooshow det hör toohello ny resurs gruppen du Skapa.</span><span class="sxs-lookup"><span data-stu-id="e6c28-143">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="e6c28-144">Klicka på hello **åtkomstnycklar** ingår i hello lagringskontots bladet.</span><span class="sxs-lookup"><span data-stu-id="e6c28-144">Click hello **Access keys** part in hello storage account's blade.</span></span> <span data-ttu-id="e6c28-145">Anteckna hello kontonamnet och key1.</span><span class="sxs-lookup"><span data-stu-id="e6c28-145">Take note of hello account name and key1.</span></span>
   
      ![Nycklar](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="e6c28-147">Vi behöver denna information tooconfigure ditt projekt i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6c28-147">We will need this information tooconfigure your project in hello next section.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="e6c28-148">Konfigurera hello projekt</span><span class="sxs-lookup"><span data-stu-id="e6c28-148">Configure hello Project</span></span>
<span data-ttu-id="e6c28-149">I det här avsnittet konfigurerar vi våra program toouse hello lagringskonto vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="e6c28-149">In this section, we'll configure our application toouse hello storage account we just created.</span></span> <span data-ttu-id="e6c28-150">Sedan ska vi kör hello program lokalt.</span><span class="sxs-lookup"><span data-stu-id="e6c28-150">Then we'll run hello application locally.</span></span>

1. <span data-ttu-id="e6c28-151">Högerklicka på projektnoden i Solution Explorer i Visual Studio och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-151">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="e6c28-152">Klicka på hello **felsöka** fliken.</span><span class="sxs-lookup"><span data-stu-id="e6c28-152">Click on hello **Debug** tab.</span></span>
   
     ![Inställningar för felsökning av projektet](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="e6c28-154">Ange hello värden för miljövariabler som krävs av programmet hello i **kommandot Debug Server**, **miljö**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-154">Set hello values of environment variables required by hello application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="e6c28-155">Detta anger hello miljövariabler när du **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-155">This will set hello environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="e6c28-156">Om du vill ange hello variabler toobe när du **Start utan Debugging**, ange hello samma värden under **kommandot Kör servern** samt.</span><span class="sxs-lookup"><span data-stu-id="e6c28-156">If you want hello variables toobe set when you **Start Without Debugging**, set hello same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="e6c28-157">Du kan också definiera miljövariablerna med hjälp av hello på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-157">Alternatively, you can define environment variables using hello Windows Control Panel.</span></span> <span data-ttu-id="e6c28-158">Detta är ett bättre alternativ om du vill lagra autentiseringsuppgifter i källkoden tooavoid / project filen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-158">This is a better option if you want tooavoid storing credentials in source code / project file.</span></span> <span data-ttu-id="e6c28-159">Observera att du måste toorestart Visual Studio hello nya miljö värden toobe tillgängliga toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="e6c28-159">Note that you will need toorestart Visual Studio for hello new environment values toobe available toohello application.</span></span>
3. <span data-ttu-id="e6c28-160">hello-kod som implementerar hello Azure Table Storage databasen är i **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-160">hello code that implements hello Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="e6c28-161">Se hello [dokumentationen] för mer information om hur toouse Tabelltjänsten från Python.</span><span class="sxs-lookup"><span data-stu-id="e6c28-161">See hello [documentation] for more information on how toouse Table Service from Python.</span></span>
4. <span data-ttu-id="e6c28-162">Kör programmet hello med `F5`.</span><span class="sxs-lookup"><span data-stu-id="e6c28-162">Run hello application with `F5`.</span></span> <span data-ttu-id="e6c28-163">Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="e6c28-163">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e6c28-164">hello virtuell miljö för Python 2.7 kan orsaka ett undantag break i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6c28-164">hello Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="e6c28-165">Tryck på `F5` toocontinue inläsning hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="e6c28-165">Press `F5` toocontinue loading hello web project.</span></span>
   > 
   > 
5. <span data-ttu-id="e6c28-166">Bläddra toohello **om** sidan tooverify som hello program använder hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-166">Browse toohello **About** page tooverify that hello application is using hello **Azure Table Storage** repository.</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a><span data-ttu-id="e6c28-168">Utforska hello Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="e6c28-168">Explore hello Azure Table Storage</span></span>
<span data-ttu-id="e6c28-169">Det är enkelt tooview och redigera storage-tabeller med Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6c28-169">It's easy tooview and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="e6c28-170">I det här avsnittet använder vi Server Explorer tooview hello innehållet i hello avsökningar programmet tabeller.</span><span class="sxs-lookup"><span data-stu-id="e6c28-170">In this section we'll use Server Explorer tooview hello contents of hello polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="e6c28-171">Detta kräver Microsoft Azure-verktyg toobe installerat, som är tillgängliga som en del av hello [Azure SDK för .NET].</span><span class="sxs-lookup"><span data-stu-id="e6c28-171">This requires Microsoft Azure Tools toobe installed, which are available as part of hello [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="e6c28-172">Öppna **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-172">Open **Cloud Explorer**.</span></span> <span data-ttu-id="e6c28-173">Expandera **Lagringskonton**, ditt lagringskonto, sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-173">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="e6c28-175">Dubbelklicka på hello **polls** eller **val** tabell tooview hello innehållet i hello tabell i ett dokumentfönster, samt lägga till eller ta bort/redigera entiteter.</span><span class="sxs-lookup"><span data-stu-id="e6c28-175">Double-click on hello **polls** or **choices** table tooview hello contents of hello table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tabell frågeresultat](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="e6c28-177">Publicera hello web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="e6c28-177">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="e6c28-178">hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e6c28-178">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="e6c28-179">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-179">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="e6c28-181">Klicka på **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-181">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="e6c28-182">Klicka på **ny** toocreate ett nytt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e6c28-182">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="e6c28-183">Fyll i följande fält hello och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-183">Fill in hello following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="e6c28-184">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="e6c28-184">**Web App name**</span></span>
   * <span data-ttu-id="e6c28-185">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="e6c28-185">**App Service plan**</span></span>
   * <span data-ttu-id="e6c28-186">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="e6c28-186">**Resource group**</span></span>
   * <span data-ttu-id="e6c28-187">**Region**</span><span class="sxs-lookup"><span data-stu-id="e6c28-187">**Region**</span></span>
   * <span data-ttu-id="e6c28-188">Lämna **databasserver** ställa in också**ingen databas**</span><span class="sxs-lookup"><span data-stu-id="e6c28-188">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="e6c28-189">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-189">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="e6c28-190">Webbläsaren öppnas automatiskt toohello publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e6c28-190">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="e6c28-191">Om du bläddrar toohello om sidan ser du att den använder hello **i minnet** databasen, inte hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-191">If you browse toohello about page, you'll see that it uses hello **In-Memory** repository, not hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="e6c28-192">Det beror på att hello miljövariablerna inte anges på instansen för hello Web Apps i Azure App Service, så att den använder hello standardvärden som anges i **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-192">That's because hello environment variables are not set on hello Web Apps instance in Azure App Service, so it uses hello default values specified in **settings.py**.</span></span>

## <a name="configure-hello-web-apps-instance"></a><span data-ttu-id="e6c28-193">Konfigurera hello Web Apps instans</span><span class="sxs-lookup"><span data-stu-id="e6c28-193">Configure hello Web Apps instance</span></span>
<span data-ttu-id="e6c28-194">I det här avsnittet konfigurerar vi miljövariabler för hello Web Apps-instansen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-194">In this section, we'll configure environment variables for hello Web Apps instance.</span></span>

1. <span data-ttu-id="e6c28-195">I [Azure Portal], öppna hello webbapps blad genom att klicka på **Bläddra** > **Apptjänster** > din webbprogrammets namn.</span><span class="sxs-lookup"><span data-stu-id="e6c28-195">In [Azure Portal], open hello web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="e6c28-196">I din webbapps blad klickar du på **alla inställningar**, klicka på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-196">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="e6c28-197">Bläddra nedåt toohello **appinställningar** och ange hello värden för **databasen\_namn**, **lagring\_namn** och  **LAGRING\_NYCKELN** enligt beskrivningen i hello **konfigurera hello projektet** ovan.</span><span class="sxs-lookup"><span data-stu-id="e6c28-197">Scroll down toohello **App settings** section and set hello values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in hello **Configure hello project** section above.</span></span>
   
     ![App-inställningar](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="e6c28-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e6c28-199">Click on **Save**.</span></span> <span data-ttu-id="e6c28-200">När du har fått hello meddelanden att hello ändringarna har tillämpats, klickar du på **Bläddra** från hello Web app huvudblad.</span><span class="sxs-lookup"><span data-stu-id="e6c28-200">After you've received hello notifications that hello changes were applied, click on **Browse** from hello Web app main blade.</span></span>
5. <span data-ttu-id="e6c28-201">Du bör se hello web app fungerar som förväntat, med hjälp av hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="e6c28-201">You should see hello web app working as expected, using hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="e6c28-202">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e6c28-202">Congratulations!</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="e6c28-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6c28-204">Next steps</span></span>
<span data-ttu-id="e6c28-205">Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Bottle och Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="e6c28-205">Follow these links toolearn more about Python Tools for Visual Studio, Bottle and Azure Table Storage.</span></span>

* <span data-ttu-id="e6c28-206">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="e6c28-206">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="e6c28-207">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="e6c28-207">[Web Projects]</span></span>
  * <span data-ttu-id="e6c28-208">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="e6c28-208">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="e6c28-209">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="e6c28-209">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="e6c28-210">[Bottle dokumentation]</span><span class="sxs-lookup"><span data-stu-id="e6c28-210">[Bottle Documentation]</span></span>
* <span data-ttu-id="e6c28-211">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="e6c28-211">[Azure Storage]</span></span>
* <span data-ttu-id="e6c28-212">[Azure SDK för Python]</span><span class="sxs-lookup"><span data-stu-id="e6c28-212">[Azure SDK for Python]</span></span>
* <span data-ttu-id="e6c28-213">[Hur tooUse hello Table Storage-tjänsten från Python]</span><span class="sxs-lookup"><span data-stu-id="e6c28-213">[How tooUse hello Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="e6c28-214">Nyheter</span><span class="sxs-lookup"><span data-stu-id="e6c28-214">What's changed</span></span>
* <span data-ttu-id="e6c28-215">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="e6c28-215">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentationen]:../cosmos-db/table-storage-how-to-use-python.md
[Hur tooUse hello Table Storage-tjänsten från Python]:../cosmos-db/table-storage-how-to-use-python.md


<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK för .NET]: http://azure.microsoft.com/downloads/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=624025
[Python Tools 2.2 för Visual Studio Samples VSI]: http://go.microsoft.com/fwlink/?LinkId=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Bottle dokumentation]: http://bottlepy.org/docs/dev/index.html
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK för Python]: https://github.com/Azure/azure-sdk-for-python
