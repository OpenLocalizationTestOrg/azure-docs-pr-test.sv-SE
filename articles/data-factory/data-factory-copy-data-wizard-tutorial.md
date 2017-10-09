---
title: "Självstudie: Skapa en pipeline med Copy Wizard | Microsoft Docs"
description: "I kursen får skapa du ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten med hello guiden Kopiera stöds av Data Factory"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a><span data-ttu-id="d69db-103">Självstudie: Skapa en pipeline med en kopieringsaktivitet med hjälp av Guiden Data Factory-kopia</span><span class="sxs-lookup"><span data-stu-id="d69db-103">Tutorial: Create a pipeline with Copy Activity using Data Factory Copy Wizard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d69db-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="d69db-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="d69db-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="d69db-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="d69db-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d69db-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="d69db-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d69db-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="d69db-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d69db-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="d69db-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="d69db-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="d69db-110">REST API</span><span class="sxs-lookup"><span data-stu-id="d69db-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="d69db-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="d69db-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="d69db-112">De här självstudierna visar hur toouse hello **guiden Kopiera** toocopy data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d69db-112">This tutorial shows you how toouse hello **Copy Wizard** toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

<span data-ttu-id="d69db-113">hello Azure Data Factory **guiden Kopiera** kan du tooquickly skapar en pipeline för data som kopierar data från ett datalager för stöds källa data store tooa stöds mål.</span><span class="sxs-lookup"><span data-stu-id="d69db-113">hello Azure Data Factory **Copy Wizard** allows you tooquickly create a data pipeline that copies data from a supported source data store tooa supported destination data store.</span></span> <span data-ttu-id="d69db-114">Därför rekommenderar vi att du använder guiden hello som ett första steg toocreate en exempel-pipeline för ditt scenario för flytt av data.</span><span class="sxs-lookup"><span data-stu-id="d69db-114">Therefore, we recommend that you use hello wizard as a first step toocreate a sample pipeline for your data movement scenario.</span></span> <span data-ttu-id="d69db-115">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="d69db-115">For a list of data stores supported as sources and as destinations, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>  

<span data-ttu-id="d69db-116">Den här kursen visar hur toocreate ett Azure data factory, starta hello guiden Kopiera, gå igenom ett antal steg tooprovide information om ditt scenario för införandet/flytt av data.</span><span class="sxs-lookup"><span data-stu-id="d69db-116">This tutorial shows you how toocreate an Azure data factory, launch hello Copy Wizard, go through a series of steps tooprovide details about your data ingestion/movement scenario.</span></span> <span data-ttu-id="d69db-117">När du slutför stegen i guiden hello hello guiden skapar automatiskt en pipeline med en Kopieringsaktiviteten toocopy data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d69db-117">When you finish steps in hello wizard, hello wizard automatically creates a pipeline with a Copy Activity toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="d69db-118">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="d69db-118">For more information about Copy Activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d69db-119">Krav</span><span class="sxs-lookup"><span data-stu-id="d69db-119">Prerequisites</span></span>
<span data-ttu-id="d69db-120">Slutföra förutsättningar som anges i hello [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d69db-120">Complete prerequisites listed in hello [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="d69db-121">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="d69db-121">Create data factory</span></span>
<span data-ttu-id="d69db-122">I det här steget kan du använda hello Azure portal toocreate ett Azure data factory med namnet **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="d69db-122">In this step, you use hello Azure portal toocreate an Azure data factory named **ADFTutorialDataFactory**.</span></span>

1. <span data-ttu-id="d69db-123">Logga in för[Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d69db-123">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d69db-124">Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Data + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="d69db-124">Click **+ NEW** from hello top-left corner, click **Data + analytics**, and click **Data Factory**.</span></span> 
   
   ![Nytt->DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. <span data-ttu-id="d69db-126">I hello **nya data factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="d69db-126">In hello **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="d69db-127">Ange **ADFTutorialDataFactory** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="d69db-127">Enter **ADFTutorialDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="d69db-128">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="d69db-128">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="d69db-129">Om felmeddelandet hello: `Data factory name “ADFTutorialDataFactory” is not available`, ändra hello namn i hello data factory (till exempel yournameADFTutorialDataFactoryYYYYMMDD) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="d69db-129">If you receive hello error: `Data factory name “ADFTutorialDataFactory” is not available`, change hello name of hello data factory (for example, yournameADFTutorialDataFactoryYYYYMMDD) and try creating again.</span></span> <span data-ttu-id="d69db-130">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="d69db-130">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
      
       ![Datafabriksnamnet är inte tillgängligt](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. <span data-ttu-id="d69db-132">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d69db-132">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="d69db-133">För resursgruppen, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="d69db-133">For Resource Group, do one of hello following steps:</span></span> 
      
      - <span data-ttu-id="d69db-134">Välj **använda befintliga** tooselect en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d69db-134">Select **Use existing** tooselect an existing resource group.</span></span>
      - <span data-ttu-id="d69db-135">Välj **Skapa nytt** tooenter ett namn för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d69db-135">Select **Create new** tooenter a name for a resource group.</span></span>
          
        <span data-ttu-id="d69db-136">Några av hello stegen i den här självstudiekursen förutsätts att du använder hello namn: **ADFTutorialResourceGroup** för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d69db-136">Some of hello steps in this tutorial assume that you use hello name: **ADFTutorialResourceGroup** for hello resource group.</span></span> <span data-ttu-id="d69db-137">toolearn om resursgrupper finns [resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d69db-137">toolearn about resource groups, see [Using resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>
   4. <span data-ttu-id="d69db-138">Välj en **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="d69db-138">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="d69db-139">Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="d69db-139">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="d69db-140">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-140">Click **Create**.</span></span>
      
       ![Bladet Ny datafabrik](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. <span data-ttu-id="d69db-142">När hello har skapats visas hello **Data Factory** bladet som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="d69db-142">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>
   
   ![Datafabrikens startsida](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a><span data-ttu-id="d69db-144">Använda guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="d69db-144">Launch Copy Wizard</span></span>
1. <span data-ttu-id="d69db-145">På hello Data Factory-bladet, klickar du på **kopiera data [FÖRHANDSGRANSKNING]** toolaunch hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="d69db-145">On hello Data Factory blade, click **Copy data [PREVIEW]** toolaunch hello **Copy Wizard**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="d69db-146">Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställningen i hello webbläsarinställningar (eller) Håll den är aktiverad och skapa ett undantag för  **login.microsoftonline.com** och försök sedan starta hello guiden igen.</span><span class="sxs-lookup"><span data-stu-id="d69db-146">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting in hello browser settings (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="d69db-147">I hello **egenskaper** sidan:</span><span class="sxs-lookup"><span data-stu-id="d69db-147">In hello **Properties** page:</span></span>
   
   1. <span data-ttu-id="d69db-148">Ange **CopyFromBlobToAzureSql** som **aktivitetsnamn**</span><span class="sxs-lookup"><span data-stu-id="d69db-148">Enter **CopyFromBlobToAzureSql** for **Task name**</span></span>
   2. <span data-ttu-id="d69db-149">Ange en **beskrivning** (valfritt).</span><span class="sxs-lookup"><span data-stu-id="d69db-149">Enter **description** (optional).</span></span>
   3. <span data-ttu-id="d69db-150">Ändra hello **startdatum tid** och hello **sluttiden datum** så att hello slutdatum är inställt tootoday starta datum toofive dagar tidigare.</span><span class="sxs-lookup"><span data-stu-id="d69db-150">Change hello **Start date time** and hello **End date time** so that hello end date is set tootoday and start date toofive days earlier.</span></span>  
   4. <span data-ttu-id="d69db-151">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-151">Click **Next**.</span></span>  
      
      ![Verktyget Kopiera – sidan Egenskaper](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. <span data-ttu-id="d69db-153">På hello **källa datalagret** klickar du på **Azure Blob Storage** panelen.</span><span class="sxs-lookup"><span data-stu-id="d69db-153">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="d69db-154">Du kan använda det här datalagret för sidan toospecify hello källa för hello kopiera aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d69db-154">You use this page toospecify hello source data store for hello copy task.</span></span> 
   
    ![Verktyget Kopiera – sidan Källans datalager](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. <span data-ttu-id="d69db-156">På hello **ange hello Azure Blob storage-konto** sidan:</span><span class="sxs-lookup"><span data-stu-id="d69db-156">On hello **Specify hello Azure Blob storage account** page:</span></span>
   
   1. <span data-ttu-id="d69db-157">Ange **AzureStorageLinkedService** som **Namn på länkad tjänst**.</span><span class="sxs-lookup"><span data-stu-id="d69db-157">Enter **AzureStorageLinkedService** for **Linked service name**.</span></span>
   2. <span data-ttu-id="d69db-158">Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.</span><span class="sxs-lookup"><span data-stu-id="d69db-158">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="d69db-159">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d69db-159">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="d69db-160">Välj en **Azure storage-konto** från hello lista över Azure storage-konton tillgängliga i hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d69db-160">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="d69db-161">Du kan också välja tooenter lagringsutrymmet kontoinställningar manuellt genom att välja **ange manuellt** alternativ för hello **konto urvalsmetod**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-161">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**, and then click **Next**.</span></span> 
      
      ![Kopiera verktyget - Ange hello Azure Blob storage-konto](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. <span data-ttu-id="d69db-163">På **Välj hello inkommande fil eller mapp** sidan:</span><span class="sxs-lookup"><span data-stu-id="d69db-163">On **Choose hello input file or folder** page:</span></span>
   
   1. <span data-ttu-id="d69db-164">Dubbelklicka på **adftutorial** (mapp).</span><span class="sxs-lookup"><span data-stu-id="d69db-164">Double-click **adftutorial** (folder).</span></span>
   2. <span data-ttu-id="d69db-165">Välj **emp.txt** och klicka på **Välj**</span><span class="sxs-lookup"><span data-stu-id="d69db-165">Select **emp.txt**, and click **Choose**</span></span>
      
      ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. <span data-ttu-id="d69db-167">På hello **Välj hello inkommande fil eller mapp** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-167">On hello **Choose hello input file or folder** page, click **Next**.</span></span> <span data-ttu-id="d69db-168">Välj inte **Binär kopia**.</span><span class="sxs-lookup"><span data-stu-id="d69db-168">Do not select **Binary copy**.</span></span> 
   
    ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. <span data-ttu-id="d69db-170">På hello **filen formatinställningar** kan du se hello avgränsare och hello schemat som är detekterade hello guiden med hello fil-parsning.</span><span class="sxs-lookup"><span data-stu-id="d69db-170">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> <span data-ttu-id="d69db-171">Du kan även ange hello avgränsare manuellt för hello kopiera guiden toostop auto-identifiering eller toooverride.</span><span class="sxs-lookup"><span data-stu-id="d69db-171">You can also enter hello delimiters manually for hello copy wizard toostop auto-detecting or toooverride.</span></span> <span data-ttu-id="d69db-172">Klicka på **nästa** när du granskar hello avgränsare och förhandsgranska data.</span><span class="sxs-lookup"><span data-stu-id="d69db-172">Click **Next** after you review hello delimiters and preview data.</span></span> 
   
    ![Verktyget Kopiera – Filformatinställningar](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. <span data-ttu-id="d69db-174">Hello mål data lagra sidan, Välj **Azure SQL Database**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-174">On hello Destination data store page, select **Azure SQL Database**, and click **Next**.</span></span>
   
    ![Verktyget kopiera - Välj målarkiv](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. <span data-ttu-id="d69db-176">På **ange hello Azure SQL database** sidan:</span><span class="sxs-lookup"><span data-stu-id="d69db-176">On **Specify hello Azure SQL database** page:</span></span>
   
   1. <span data-ttu-id="d69db-177">Ange **AzureSqlLinkedService** för hello **anslutningsnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="d69db-177">Enter **AzureSqlLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="d69db-178">Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för server/databasval**.</span><span class="sxs-lookup"><span data-stu-id="d69db-178">Confirm that **From Azure subscriptions** option is selected for **Server / database selection method**.</span></span>
   3. <span data-ttu-id="d69db-179">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d69db-179">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="d69db-180">Välj **Servernamn** och **Databas**.</span><span class="sxs-lookup"><span data-stu-id="d69db-180">Select **Server name** and **Database**.</span></span>
   5. <span data-ttu-id="d69db-181">Ange **Användarnamn** och **Lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d69db-181">Enter **User name** and **Password**.</span></span>
   6. <span data-ttu-id="d69db-182">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-182">Click **Next**.</span></span>  
      
      ![Verktyget Kopiera - Ange Azure SQL-databas](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. <span data-ttu-id="d69db-184">På hello **tabell mappning** väljer **tomma** för hello **mål** hello nedrullningsbara listan och klicka **NEDPIL** (valfritt) toosee hello schemat och toopreview hello data.</span><span class="sxs-lookup"><span data-stu-id="d69db-184">On hello **Table mapping** page, select **emp** for hello **Destination** field from hello drop-down list, click **down arrow** (optional) toosee hello schema and toopreview hello data.</span></span>
    
     ![Verktyget Kopiera – Tabellmappning](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. <span data-ttu-id="d69db-186">På hello **schemamappning** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-186">On hello **Schema mapping** page, click **Next**.</span></span>
    
    ![Verktyget Kopiera - schemamappning](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. <span data-ttu-id="d69db-188">På hello **prestandainställningar** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d69db-188">On hello **Performance settings** page, click **Next**.</span></span> 
    
    ![Verktyget Kopiera - prestandainställningar](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. <span data-ttu-id="d69db-190">Granska informationen i hello **sammanfattning** och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d69db-190">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="d69db-191">hello skapas två länkade tjänster, två datamängder (indata och utdata) och en pipeline i hello data factory (från där startas hello guiden Kopiera).</span><span class="sxs-lookup"><span data-stu-id="d69db-191">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span> 
    
    ![Verktyget Kopiera - prestandainställningar](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a><span data-ttu-id="d69db-193">Starta övervakning och hantera program</span><span class="sxs-lookup"><span data-stu-id="d69db-193">Launch Monitor and Manage application</span></span>
1. <span data-ttu-id="d69db-194">På hello **distribution** klickar du på länken hello: `Click here toomonitor copy pipeline`.</span><span class="sxs-lookup"><span data-stu-id="d69db-194">On hello **Deployment** page, click hello link: `Click here toomonitor copy pipeline`.</span></span>
   
   ![Verktyget Kopiera – Distributionen är klar](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. <span data-ttu-id="d69db-196">hello-program startas i en separat flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d69db-196">hello monitoring application is launched in a separate tab in your web browser.</span></span>   
   
   ![Övervakningsapp](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. <span data-ttu-id="d69db-198">toosee hello senaste statusen för varje timme segment klickar du på **uppdatera** knapp i hello **aktivitet WINDOWS** lista längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d69db-198">toosee hello latest status of hourly slices, click **Refresh** button in hello **ACTIVITY WINDOWS** list at hello bottom.</span></span> <span data-ttu-id="d69db-199">Du kan se fem aktivitet windows i fem dagar mellan start- och sluttider för hello pipelinen.</span><span class="sxs-lookup"><span data-stu-id="d69db-199">You see five activity windows for five days between start and end times for hello pipeline.</span></span> <span data-ttu-id="d69db-200">hello listan uppdateras inte automatiskt, så du kanske behöver tooclick uppdatera några gånger innan du ser alla hello aktivitet windows hello klar att skriva.</span><span class="sxs-lookup"><span data-stu-id="d69db-200">hello list is not automatically refreshed, so you may need tooclick Refresh a couple of times before you see all hello activity windows in hello Ready state.</span></span> 
4. <span data-ttu-id="d69db-201">Välj en aktivitetsfönstret hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d69db-201">Select an activity window in hello list.</span></span> <span data-ttu-id="d69db-202">Se hello information om det i hello **aktivitet fönstret Explorer** på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="d69db-202">See hello details about it in hello **Activity Window Explorer** on hello right.</span></span>

    ![Information om aktivitetsfönstret](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    <span data-ttu-id="d69db-204">Lägg märke till att hello datum 11, 12, 13, 14 och 15 i grön färg, vilket innebär att det redan har producerats hello dagliga utdata segment för dessa datum.</span><span class="sxs-lookup"><span data-stu-id="d69db-204">Notice that hello dates 11, 12, 13, 14, and 15 are in green color, which means that hello daily output slices for these dates have already been produced.</span></span> <span data-ttu-id="d69db-205">Du också se färgkodning på hello pipeline och hello utdatamängden i hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="d69db-205">You also see this color coding on hello pipeline and hello output dataset in hello diagram view.</span></span> <span data-ttu-id="d69db-206">I hello föregående steg, se att två segment har redan skapats ett segment håller på att behandlas och hello andra två som väntar på toobe bearbetas (baserat på syntax i hello färger).</span><span class="sxs-lookup"><span data-stu-id="d69db-206">In hello previous step, notice that two slices have already been produced, one slice is currently being processed, and hello other two are waiting toobe processed (based on hello color coding).</span></span> 

    <span data-ttu-id="d69db-207">Mer information om hur du använder det här programmet finns i artikeln [Övervaka och hantera pipeline med övervakningsappen](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="d69db-207">For more information on using this application, see [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d69db-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d69db-208">Next steps</span></span>
<span data-ttu-id="d69db-209">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="d69db-209">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="d69db-210">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="d69db-210">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="d69db-211">Mer information om fält och egenskaper som visas i guiden för hello kopiera för ett datalager klickar du på hello länk för hello-datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="d69db-211">For details about fields/properties that you see in hello copy wizard for a data store, click hello link for hello data store in hello table.</span></span> 
