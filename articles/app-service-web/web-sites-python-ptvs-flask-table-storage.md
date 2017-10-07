---
title: "aaaFlask och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Flask-webbapp som lagrar data i Azure Table Storage och distribuera den tooAzure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: d8e70a29-aca1-4010-95f5-cfe769e3be06
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1a09d4cc78078a00492ba4fe7e2075df96fb0380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="9668d-103">Flask och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9668d-103">Flask and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="9668d-104">I den här kursen använder vi [Python Tools för Visual Studio] toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS.</span><span class="sxs-lookup"><span data-stu-id="9668d-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="9668d-105">Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span><span class="sxs-lookup"><span data-stu-id="9668d-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span></span>

<span data-ttu-id="9668d-106">Hej webbapp definierar en abstraktion för dess databas, så du kan enkelt växla mellan olika typer av databaser (i minnet, Azure Table Storage MongoDB).</span><span class="sxs-lookup"><span data-stu-id="9668d-106">hello polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="9668d-107">Vi lära dig hur toocreate ett Azure Storage-konto, hur tooconfigure hello web app toouse Azure Table Storage och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="9668d-107">We'll learn how toocreate an Azure Storage account, how tooconfigure hello web app toouse Azure Table Storage, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="9668d-108">Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med MongoDB, Azure Table Storage, MySQL och SQL Database-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9668d-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="9668d-109">Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="9668d-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9668d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9668d-110">Prerequisites</span></span>
* <span data-ttu-id="9668d-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="9668d-111">Visual Studio 2015</span></span>
* <span data-ttu-id="9668d-112">[Python Tools 2.2 för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="9668d-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="9668d-113">[Python Tools 2.2 för Visual Studio Samples VSI]</span><span class="sxs-lookup"><span data-stu-id="9668d-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="9668d-114">[Azure SDK Tools för VS 2015]</span><span class="sxs-lookup"><span data-stu-id="9668d-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="9668d-115">[Python 2.7 32-bitars] eller [Python 3.4 32-bitars]</span><span class="sxs-lookup"><span data-stu-id="9668d-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="9668d-116">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="9668d-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="9668d-117">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="9668d-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="9668d-118">Skapa hello projekt</span><span class="sxs-lookup"><span data-stu-id="9668d-118">Create hello Project</span></span>
<span data-ttu-id="9668d-119">I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall.</span><span class="sxs-lookup"><span data-stu-id="9668d-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="9668d-120">Vi skapa en virtuell miljö och installera nödvändiga paket.</span><span class="sxs-lookup"><span data-stu-id="9668d-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="9668d-121">Sedan kör vi hello programmet lokalt med hjälp av hello standarddatabas i minnet.</span><span class="sxs-lookup"><span data-stu-id="9668d-121">Then we'll run hello application locally using hello default in-memory repository.</span></span>

1. <span data-ttu-id="9668d-122">I Visual Studio väljer du **Arkiv**, **Nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="9668d-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="9668d-123">Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**.</span><span class="sxs-lookup"><span data-stu-id="9668d-123">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="9668d-124">Välj **avsökningar Flask-webbprojekt** och klicka på OK toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="9668d-124">Select **Polls Flask Web Project** and click OK toocreate hello project.</span></span>
   
     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. <span data-ttu-id="9668d-126">Du kommer att tillfrågas tooinstall externa paket.</span><span class="sxs-lookup"><span data-stu-id="9668d-126">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="9668d-127">Välj **Install into a virtual environment** (Installera i en virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="9668d-127">Select **Install into a virtual environment**.</span></span>
   
     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. <span data-ttu-id="9668d-129">Välj **Python 2.7** eller **Python 3.4** som hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="9668d-129">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="9668d-131">Bekräfta att hello programmet fungerar genom att trycka på `F5`.</span><span class="sxs-lookup"><span data-stu-id="9668d-131">Confirm that hello application works by pressing `F5`.</span></span> <span data-ttu-id="9668d-132">Som standard använder programmet hello en InMemory-lagringsplats som inte kräver någon konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9668d-132">By default, hello application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="9668d-133">Alla data går förlorade när hello webbservern har stoppats.</span><span class="sxs-lookup"><span data-stu-id="9668d-133">All data is lost when hello web server is stopped.</span></span>
6. <span data-ttu-id="9668d-134">Klicka på **skapa Exempelomröstningar**, klicka på en omröstning och rösta.</span><span class="sxs-lookup"><span data-stu-id="9668d-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9668d-136">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="9668d-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="9668d-137">toouse lagringsåtgärder du behöver ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9668d-137">toouse storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="9668d-138">Du kan skapa ett lagringskonto genom att följa dessa steg.</span><span class="sxs-lookup"><span data-stu-id="9668d-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="9668d-139">Logga in på hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9668d-139">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9668d-140">Klicka på hello **ny** ikonen hello längst upp till vänster i hello Portal och klicka sedan på **Data + lagring** > **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="9668d-140">Click hello **New** icon on hello top left of hello Portal, then click **Data + Storage** > **Storage Account**.</span></span> <span data-ttu-id="9668d-141">Klicka på **skapa**, och ge hello storage-konto ett unikt namn och skapa en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md) för den.</span><span class="sxs-lookup"><span data-stu-id="9668d-141">Click on **Create**, then give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Snabbregistrering](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="9668d-143">När hello storage-konto har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello lagringskontots blad är öppet tooshow det hör toohello ny resurs gruppen du Skapa.</span><span class="sxs-lookup"><span data-stu-id="9668d-143">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="9668d-144">Klicka på hello **åtkomstnycklar** ingår i hello lagringskontots bladet.</span><span class="sxs-lookup"><span data-stu-id="9668d-144">Click hello **Access Keys** part in hello storage account's blade.</span></span> <span data-ttu-id="9668d-145">Anteckna hello kontonamnet och hello key1.</span><span class="sxs-lookup"><span data-stu-id="9668d-145">Take note of hello account name and hello key1.</span></span>
   
      ![Nycklar](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="9668d-147">Vi behöver denna information tooconfigure ditt projekt i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9668d-147">We will need this information tooconfigure your project in hello next section.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="9668d-148">Konfigurera hello projekt</span><span class="sxs-lookup"><span data-stu-id="9668d-148">Configure hello Project</span></span>
<span data-ttu-id="9668d-149">I det här avsnittet konfigurerar vi våra program toouse hello lagringskonto vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="9668d-149">In this section, we'll configure our application toouse hello storage account we just created.</span></span> <span data-ttu-id="9668d-150">Vi se hur tooobtain anslutningsinställningar från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9668d-150">We'll see how tooobtain connection settings from hello Azure Portal.</span></span> <span data-ttu-id="9668d-151">Sedan ska vi kör hello program lokalt.</span><span class="sxs-lookup"><span data-stu-id="9668d-151">Then we'll run hello application locally.</span></span>

1. <span data-ttu-id="9668d-152">Högerklicka på projektnoden i Solution Explorer i Visual Studio och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="9668d-152">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="9668d-153">Klicka på hello **felsöka** fliken.</span><span class="sxs-lookup"><span data-stu-id="9668d-153">Click on hello **Debug** tab.</span></span>
   
     ![Inställningar för felsökning av projektet](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="9668d-155">Ange hello värden för miljövariabler som krävs av programmet hello i **kommandot Debug Server**, **miljö**.</span><span class="sxs-lookup"><span data-stu-id="9668d-155">Set hello values of environment variables required by hello application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="9668d-156">Detta anger hello miljövariabler när du **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="9668d-156">This will set hello environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="9668d-157">Om du vill ange hello variabler toobe när du **Start utan Debugging**, ange hello samma värden under **kommandot Kör servern** samt.</span><span class="sxs-lookup"><span data-stu-id="9668d-157">If you want hello variables toobe set when you **Start Without Debugging**, set hello same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="9668d-158">Du kan också definiera miljövariablerna med hjälp av hello på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="9668d-158">Alternatively, you can define environment variables using hello Windows Control Panel.</span></span> <span data-ttu-id="9668d-159">Detta är ett bättre alternativ om du vill lagra autentiseringsuppgifter i källkoden tooavoid / project filen.</span><span class="sxs-lookup"><span data-stu-id="9668d-159">This is a better option if you want tooavoid storing credentials in source code / project file.</span></span> <span data-ttu-id="9668d-160">Observera att du måste toorestart Visual Studio hello nya miljö värden toobe tillgängliga toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="9668d-160">Note that you will need toorestart Visual Studio for hello new environment values toobe available toohello application.</span></span>
3. <span data-ttu-id="9668d-161">hello-kod som implementerar hello Azure Table Storage databasen är i **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="9668d-161">hello code that implements hello Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="9668d-162">Se hello [dokumentationen] för mer information om hur toouse Tabelltjänsten från Python.</span><span class="sxs-lookup"><span data-stu-id="9668d-162">See hello [documentation] for more information on how toouse Table Service from Python.</span></span>
4. <span data-ttu-id="9668d-163">Kör programmet hello med `F5`.</span><span class="sxs-lookup"><span data-stu-id="9668d-163">Run hello application with `F5`.</span></span> <span data-ttu-id="9668d-164">Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="9668d-164">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9668d-165">hello virtuell miljö för Python 2.7 kan orsaka ett undantag break i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9668d-165">hello Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="9668d-166">Tryck på `F5` toocontinue inläsning hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="9668d-166">Press `F5` toocontinue loading hello web project.</span></span>
   > 
   > 
5. <span data-ttu-id="9668d-167">Bläddra toohello **om** sidan tooverify som hello program använder hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="9668d-167">Browse toohello **About** page tooverify that hello application is using hello **Azure Table Storage** repository.</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a><span data-ttu-id="9668d-169">Utforska hello Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="9668d-169">Explore hello Azure Table Storage</span></span>
<span data-ttu-id="9668d-170">Det är enkelt tooview och redigera storage-tabeller med Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9668d-170">It's easy tooview and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="9668d-171">I det här avsnittet använder vi Server Explorer tooview hello innehållet i hello avsökningar programmet tabeller.</span><span class="sxs-lookup"><span data-stu-id="9668d-171">In this section we'll use Server Explorer tooview hello contents of hello polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="9668d-172">Detta kräver Microsoft Azure-verktyg toobe installerat, som är tillgängliga som en del av hello [Azure SDK för .NET].</span><span class="sxs-lookup"><span data-stu-id="9668d-172">This requires Microsoft Azure Tools toobe installed, which are available as part of hello [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="9668d-173">Öppna **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="9668d-173">Open **Cloud Explorer**.</span></span> <span data-ttu-id="9668d-174">Expandera **Lagringskonton**, ditt lagringskonto, sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="9668d-174">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="9668d-176">Dubbelklicka på hello **polls** eller **val** tabell tooview hello innehållet i hello tabell i ett dokumentfönster, samt lägga till eller ta bort/redigera entiteter.</span><span class="sxs-lookup"><span data-stu-id="9668d-176">Double-click on hello **polls** or **choices** table tooview hello contents of hello table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tabell frågeresultat](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="9668d-178">Publicera hello web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="9668d-178">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="9668d-179">hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="9668d-179">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="9668d-180">I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.</span><span class="sxs-lookup"><span data-stu-id="9668d-180">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="9668d-182">Klicka på **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="9668d-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="9668d-183">Klicka på **ny** toocreate ett nytt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9668d-183">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="9668d-184">Fyll i följande fält hello och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9668d-184">Fill in hello following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="9668d-185">**Webbappens namn**</span><span class="sxs-lookup"><span data-stu-id="9668d-185">**Web App name**</span></span>
   * <span data-ttu-id="9668d-186">**App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="9668d-186">**App Service plan**</span></span>
   * <span data-ttu-id="9668d-187">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="9668d-187">**Resource group**</span></span>
   * <span data-ttu-id="9668d-188">**Region**</span><span class="sxs-lookup"><span data-stu-id="9668d-188">**Region**</span></span>
   * <span data-ttu-id="9668d-189">Lämna **databasserver** ställa in också**ingen databas**</span><span class="sxs-lookup"><span data-stu-id="9668d-189">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="9668d-190">Godkänn alla standardvärden och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="9668d-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="9668d-191">Webbläsaren öppnas automatiskt toohello publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9668d-191">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="9668d-192">Om du bläddrar toohello om sidan ser du att den använder hello **i minnet** databasen, inte hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="9668d-192">If you browse toohello about page, you'll see that it uses hello **In-Memory** repository, not hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="9668d-193">Det beror på att hello miljövariablerna inte anges på instansen för hello Web Apps i Azure App Service, så att den använder hello standardvärden som anges i **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="9668d-193">That's because hello environment variables are not set on hello Web Apps instance in Azure App Service, so it uses hello default values specified in **settings.py**.</span></span>

## <a name="configure-hello-web-apps-instance"></a><span data-ttu-id="9668d-194">Konfigurera hello Web Apps instans</span><span class="sxs-lookup"><span data-stu-id="9668d-194">Configure hello Web Apps instance</span></span>
<span data-ttu-id="9668d-195">I det här avsnittet konfigurerar vi miljövariabler för hello Web Apps-instansen.</span><span class="sxs-lookup"><span data-stu-id="9668d-195">In this section, we'll configure environment variables for hello Web Apps instance.</span></span>

1. <span data-ttu-id="9668d-196">I [Azure Portal](https://portal.azure.com), öppna hello webbapps blad genom att klicka på **Bläddra** > **Apptjänster** > din webbprogrammets namn.</span><span class="sxs-lookup"><span data-stu-id="9668d-196">In [Azure Portal](https://portal.azure.com), open hello web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="9668d-197">I din webbapps blad klickar du på **alla inställningar**, klicka på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="9668d-197">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="9668d-198">Bläddra nedåt toohello **appinställningar** och ange hello värden för **databasen\_namn**, **lagring\_namn** och  **LAGRING\_NYCKELN** enligt beskrivningen i hello **konfigurera hello projektet** ovan.</span><span class="sxs-lookup"><span data-stu-id="9668d-198">Scroll down toohello **App settings** section and set hello values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in hello **Configure hello project** section above.</span></span>
   
     ![App-inställningar](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="9668d-200">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9668d-200">Click on **Save**.</span></span> <span data-ttu-id="9668d-201">När du har fått hello meddelanden att hello ändringarna har tillämpats, klickar du på **Bläddra** från hello Web app huvudblad.</span><span class="sxs-lookup"><span data-stu-id="9668d-201">After you've received hello notifications that hello changes were applied, click on **Browse** from hello Web app main blade.</span></span>
5. <span data-ttu-id="9668d-202">Du bör se hello web app fungerar som förväntat, med hjälp av hello **Azure Table Storage** databasen.</span><span class="sxs-lookup"><span data-stu-id="9668d-202">You should see hello web app working as expected, using hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="9668d-203">Grattis!</span><span class="sxs-lookup"><span data-stu-id="9668d-203">Congratulations!</span></span>
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="9668d-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9668d-205">Next steps</span></span>
<span data-ttu-id="9668d-206">Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Flask och Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="9668d-206">Follow these links toolearn more about Python Tools for Visual Studio, Flask and Azure Table Storage.</span></span>

* <span data-ttu-id="9668d-207">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="9668d-207">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="9668d-208">[Webbprojekt]</span><span class="sxs-lookup"><span data-stu-id="9668d-208">[Web Projects]</span></span>
  * <span data-ttu-id="9668d-209">[Molntjänstprojekt]</span><span class="sxs-lookup"><span data-stu-id="9668d-209">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="9668d-210">[Fjärrfelsökning i Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="9668d-210">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="9668d-211">[Flask-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="9668d-211">[Flask Documentation]</span></span>
* <span data-ttu-id="9668d-212">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="9668d-212">[Azure Storage]</span></span>
* <span data-ttu-id="9668d-213">[Azure SDK för Python]</span><span class="sxs-lookup"><span data-stu-id="9668d-213">[Azure SDK for Python]</span></span>
* <span data-ttu-id="9668d-214">[Hur tooUse hello Table Storage-tjänsten från Python]</span><span class="sxs-lookup"><span data-stu-id="9668d-214">[How tooUse hello Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="9668d-215">Nyheter</span><span class="sxs-lookup"><span data-stu-id="9668d-215">What's changed</span></span>
* <span data-ttu-id="9668d-216">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="9668d-216">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentationen]:../cosmos-db/table-storage-how-to-use-python.md
[Hur tooUse hello Table Storage-tjänsten från Python]:../cosmos-db/table-storage-how-to-use-python.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK för .NET]: http://azure.microsoft.com/downloads/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSI]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?linkid=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Flask-dokumentation]: http://flask.pocoo.org/
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK för Python]: https://github.com/Azure/azure-sdk-for-python
