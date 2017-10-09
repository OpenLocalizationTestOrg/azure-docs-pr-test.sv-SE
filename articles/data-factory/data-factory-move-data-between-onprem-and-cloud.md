---
title: aaaMove - data i Data Management Gateway | Microsoft Docs
description: "Ställ in en data gateway toomove data mellan lokala och hello molnet. Använd Data Management Gateway i Azure Data Factory toomove dina data."
keywords: datagatewayen dataintegrering, flytta data, gateway-autentiseringsuppgifter
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a><span data-ttu-id="fb53c-105">Flytta data mellan lokala källor och hello moln med Data Management Gateway</span><span class="sxs-lookup"><span data-stu-id="fb53c-105">Move data between on-premises sources and hello cloud with Data Management Gateway</span></span>
<span data-ttu-id="fb53c-106">Den här artikeln innehåller en översikt över dataintegration mellan lokala datalager och datalager i molnet med hjälp av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="fb53c-106">This article provides an overview of data integration between on-premises data stores and cloud data stores using Data Factory.</span></span> <span data-ttu-id="fb53c-107">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln och andra data factory grundläggande begrepp artiklar: [datauppsättningar](data-factory-create-datasets.md) och [pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="fb53c-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article and other data factory core concepts articles: [datasets](data-factory-create-datasets.md) and [pipelines](data-factory-create-pipelines.md).</span></span>

## <a name="data-management-gateway"></a><span data-ttu-id="fb53c-108">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="fb53c-108">Data Management Gateway</span></span>
<span data-ttu-id="fb53c-109">Data Management Gateway måste du installera på din lokala dator tooenable flytta data till och från ett lokalt datalager.</span><span class="sxs-lookup"><span data-stu-id="fb53c-109">You must install Data Management Gateway on your on-premises machine tooenable moving data to/from an on-premises data store.</span></span> <span data-ttu-id="fb53c-110">hello gateway kan installeras på samma dator som hello datalager eller på en annan dator så länge hello gateway kan ansluta toohello datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="fb53c-110">hello gateway can be installed on hello same machine as hello data store or on a different machine as long as hello gateway can connect toohello data store.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb53c-111">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> 

<span data-ttu-id="fb53c-112">hello följande genomgången visar hur toocreate en datafabrik med en rörledning som flyttar data från en lokal **SQL Server** tooan Azure blob storage-databas.</span><span class="sxs-lookup"><span data-stu-id="fb53c-112">hello following walkthrough shows you how toocreate a data factory with a pipeline that moves data from an on-premises **SQL Server** database tooan Azure blob storage.</span></span> <span data-ttu-id="fb53c-113">Som en del av hello genomgången kan du installera och konfigurera hello Data Management Gateway på din dator.</span><span class="sxs-lookup"><span data-stu-id="fb53c-113">As part of hello walkthrough, you install and configure hello Data Management Gateway on your machine.</span></span>

## <a name="walkthrough-copy-on-premises-data-toocloud"></a><span data-ttu-id="fb53c-114">Genomgång: kopiera lokala data toocloud</span><span class="sxs-lookup"><span data-stu-id="fb53c-114">Walkthrough: copy on-premises data toocloud</span></span>
<span data-ttu-id="fb53c-115">I den här genomgången du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb53c-115">In this walkthrough you do hello following steps:</span></span> 

1. <span data-ttu-id="fb53c-116">Skapa en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="fb53c-116">Create a data factory.</span></span>
2. <span data-ttu-id="fb53c-117">Skapa en data management gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-117">Create a data management gateway.</span></span> 
3. <span data-ttu-id="fb53c-118">Skapa länkade tjänster för källa och mottagare datalager.</span><span class="sxs-lookup"><span data-stu-id="fb53c-118">Create linked services for source and sink data stores.</span></span>
4. <span data-ttu-id="fb53c-119">Skapa datauppsättningar toorepresent indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="fb53c-119">Create datasets toorepresent input and output data.</span></span>
5. <span data-ttu-id="fb53c-120">Skapa en pipeline med kopiera aktiviteten toomove hello data.</span><span class="sxs-lookup"><span data-stu-id="fb53c-120">Create a pipeline with a copy activity toomove hello data.</span></span>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="fb53c-121">Förutsättningar för självstudiekursen hello</span><span class="sxs-lookup"><span data-stu-id="fb53c-121">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="fb53c-122">Innan du börjar den här genomgången måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="fb53c-122">Before you begin this walkthrough, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="fb53c-123">**Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-123">**Azure subscription**.</span></span>  <span data-ttu-id="fb53c-124">Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="fb53c-124">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fb53c-125">Se hello [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="fb53c-125">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="fb53c-126">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-126">**Azure Storage Account**.</span></span> <span data-ttu-id="fb53c-127">Du använder hello blob storage som en **mål-sink** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-127">You use hello blob storage as a **destination/sink** data store in this tutorial.</span></span> <span data-ttu-id="fb53c-128">Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.</span><span class="sxs-lookup"><span data-stu-id="fb53c-128">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="fb53c-129">**SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-129">**SQL Server**.</span></span> <span data-ttu-id="fb53c-130">Du använder en lokal SQL Server-databas som **källdata** i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="fb53c-130">You use an on-premises SQL Server database as a **source** data store in this tutorial.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="fb53c-131">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="fb53c-131">Create data factory</span></span>
<span data-ttu-id="fb53c-132">I det här steget kan du använda hello Azure portal toocreate en Azure Data Factory-instans med namnet **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-132">In this step, you use hello Azure portal toocreate an Azure Data Factory instance named **ADFTutorialOnPremDF**.</span></span>

1. <span data-ttu-id="fb53c-133">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fb53c-133">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fb53c-134">Klicka på **+ ny**, klickar du på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-134">Click **+ NEW**, click **Intelligence + analytics**, and click **Data Factory**.</span></span>

   ![Nytt->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. <span data-ttu-id="fb53c-136">I hello **nya datafabriken** anger **ADFTutorialOnPremDF** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-136">In hello **New data factory** page, enter **ADFTutorialOnPremDF** for hello Name.</span></span>

    ![Lägg till tooStartboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > <span data-ttu-id="fb53c-138">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="fb53c-138">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="fb53c-139">Om felmeddelandet hello: **datafabriksnamnet ”ADFTutorialOnPremDF” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel yournameADFTutorialOnPremDF) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-139">If you receive hello error: **Data factory name “ADFTutorialOnPremDF” is not available**, change hello name of hello data factory (for example, yournameADFTutorialOnPremDF) and try creating again.</span></span> <span data-ttu-id="fb53c-140">Använd det här namnet i stället för ADFTutorialOnPremDF när du utför stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-140">Use this name in place of ADFTutorialOnPremDF while performing remaining steps in this tutorial.</span></span>
   >
   > <span data-ttu-id="fb53c-141">hello namn i hello data factory får registreras som en **DNS** namn i hello framtiden och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="fb53c-141">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="fb53c-142">Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="fb53c-142">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="fb53c-143">Välj befintlig **resursgrupp** eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fb53c-143">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="fb53c-144">Hello självstudiekurs skapar du en resursgrupp med namnet: **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-144">For hello tutorial, create a resource group named: **ADFTutorialResourceGroup**.</span></span>
6. <span data-ttu-id="fb53c-145">Klicka på **skapa** på hello **nya data factory** sidan.</span><span class="sxs-lookup"><span data-stu-id="fb53c-145">Click **Create** on hello **New data factory** page.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="fb53c-146">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="fb53c-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="fb53c-147">När du har skapats visas hello **Data Factory** sidan som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="fb53c-147">After creation is complete, you see hello **Data Factory** page as shown in hello following image:</span></span>

   ![Data Factory-startsida](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a><span data-ttu-id="fb53c-149">Skapa en gateway</span><span class="sxs-lookup"><span data-stu-id="fb53c-149">Create gateway</span></span>
1. <span data-ttu-id="fb53c-150">I hello **Datafabriken** klickar du på **författare och distribuera** panelen toolaunch hello **Editor** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="fb53c-150">In hello **Data Factory** page, click **Author and deploy** tile toolaunch hello **Editor** for hello data factory.</span></span>

    ![Ikonen Författare och distribution](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. <span data-ttu-id="fb53c-152">Hello Data Factory-redigeraren, klicka på **... Flera** hello verktygsfält och klicka sedan på **nya datagateway**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-152">In hello Data Factory Editor, click **... More** on hello toolbar and then click **New data gateway**.</span></span> <span data-ttu-id="fb53c-153">Du kan också högerklicka på **Datagatewayar** i hello trädvyn och klicka på **nya datagateway**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-153">Alternatively, you can right-click **Data Gateways** in hello tree view, and click **New data gateway**.</span></span>

   ![Nya datagateway i verktygsfältet](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. <span data-ttu-id="fb53c-155">I hello **skapa** anger **adftutorialgateway** för hello **namn**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-155">In hello **Create** page, enter **adftutorialgateway** for hello **name**, and click **OK**.</span></span>     

    ![Skapa sida för Gateway](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > <span data-ttu-id="fb53c-157">I den här genomgången skapa hello logiska gateway med endast en nod (lokala Windows-dator).</span><span class="sxs-lookup"><span data-stu-id="fb53c-157">In this walkthrough, you create hello logical gateway with only one node (on-premises Windows machine).</span></span> <span data-ttu-id="fb53c-158">Du kan skala ut en data management gateway genom att associera flera lokala datorer med hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-158">You can scale out a data management gateway by associating multiple on-premises machines with hello gateway.</span></span> <span data-ttu-id="fb53c-159">Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod.</span><span class="sxs-lookup"><span data-stu-id="fb53c-159">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="fb53c-160">Den här funktionen finns också en logisk gateway med en enda nod.</span><span class="sxs-lookup"><span data-stu-id="fb53c-160">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="fb53c-161">Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="fb53c-161">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>  
4. <span data-ttu-id="fb53c-162">I hello **konfigurera** klickar du på **installera direkt på den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-162">In hello **Configure** page, click **Install directly on this computer**.</span></span> <span data-ttu-id="fb53c-163">Den här åtgärden hämtar hello installationspaket för hello gateway, installerar, konfigurerar och registrerar hello gateway på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="fb53c-163">This action downloads hello installation package for hello gateway, installs, configures, and registers hello gateway on hello computer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="fb53c-164">Använd Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="fb53c-164">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>
   >
   > <span data-ttu-id="fb53c-165">Om du använder Chrome, gå toohello [Chrome web store](https://chrome.google.com/webstore/), söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.</span><span class="sxs-lookup"><span data-stu-id="fb53c-165">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
   >
   > <span data-ttu-id="fb53c-166">Hello samma för Firefox (installera tillägget).</span><span class="sxs-lookup"><span data-stu-id="fb53c-166">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="fb53c-167">Klicka på **öppna menyn** hello i verktygsfältet (**tre horisontella raderna** hello längst upp till höger), klicka på **tillägg**, söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.</span><span class="sxs-lookup"><span data-stu-id="fb53c-167">Click **Open Menu** button on hello toolbar (**three horizontal lines** in hello top-right corner), click **Add-ons**, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>    
   >
   >

    ![Gateway - konfigurera](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    <span data-ttu-id="fb53c-169">Det här sättet är hello enklaste sättet (enkelklickning) toodownload, installera, konfigurera och registrera hello gateway i ett enda steg.</span><span class="sxs-lookup"><span data-stu-id="fb53c-169">This way is hello easiest way (one-click) toodownload, install, configure, and register hello gateway in one single step.</span></span> <span data-ttu-id="fb53c-170">Du kan se hello **Microsoft Data Management Gateway Configuration Manager** programmet är installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-170">You can see hello **Microsoft Data Management Gateway Configuration Manager** application is installed on your computer.</span></span> <span data-ttu-id="fb53c-171">Du kan också hitta hello körbara **ConfigManager.exe** i hello mapp: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-171">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span></span>

    <span data-ttu-id="fb53c-172">Du kan också hämta och installera gateway manuellt med hjälp av hello länkar i den här sidan och registrera den med hjälp av hello nyckeln visas i hello **ny nyckel** textruta.</span><span class="sxs-lookup"><span data-stu-id="fb53c-172">You can also download and install gateway manually by using hello links in this page and register it using hello key shown in hello **NEW KEY** text box.</span></span>

    <span data-ttu-id="fb53c-173">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel för alla hello information om hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-173">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello gateway.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fb53c-174">Du måste vara administratör på hello lokal dator tooinstall och konfigurera hello Data Management Gateway har.</span><span class="sxs-lookup"><span data-stu-id="fb53c-174">You must be an administrator on hello local computer tooinstall and configure hello Data Management Gateway successfully.</span></span> <span data-ttu-id="fb53c-175">Du kan lägga till fler användare toohello **Data Management Gateway-användare** lokala Windows-gruppen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-175">You can add additional users toohello **Data Management Gateway Users** local Windows group.</span></span> <span data-ttu-id="fb53c-176">hello medlemmar i gruppen kan använda hello Data Management Gateway Configuration Manager-verktyget tooconfigure hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-176">hello members of this group can use hello Data Management Gateway Configuration Manager tool tooconfigure hello gateway.</span></span>
   >
   >
5. <span data-ttu-id="fb53c-177">Vänta några minuter eller vänta tills du ser hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="fb53c-177">Wait for a couple of minutes or wait until you see hello following notification message:</span></span>

    ![Gatewayinstallationen lyckades](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. <span data-ttu-id="fb53c-179">Starta **Data Management Gateway Configuration Manager** på datorn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-179">Launch **Data Management Gateway Configuration Manager** application on your computer.</span></span> <span data-ttu-id="fb53c-180">I hello **Sök** fönster, Skriv **Data Management Gateway** tooaccess det här verktyget.</span><span class="sxs-lookup"><span data-stu-id="fb53c-180">In hello **Search** window, type **Data Management Gateway** tooaccess this utility.</span></span> <span data-ttu-id="fb53c-181">Du kan också hitta hello körbara **ConfigManager.exe** i hello mapp: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span><span class="sxs-lookup"><span data-stu-id="fb53c-181">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span></span>

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. <span data-ttu-id="fb53c-183">Bekräfta att du ser `adftutorialgateway is connected toohello cloud service` meddelande.</span><span class="sxs-lookup"><span data-stu-id="fb53c-183">Confirm that you see `adftutorialgateway is connected toohello cloud service` message.</span></span> <span data-ttu-id="fb53c-184">Hej statusfältet hello nedre visar **anslutna toohello Molntjänsten** tillsammans med en **grön bock**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-184">hello status bar hello bottom displays **Connected toohello cloud service** along with a **green check mark**.</span></span>

    <span data-ttu-id="fb53c-185">På hello **Start** och du kan också göra hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="fb53c-185">On hello **Home** tab, you can also do hello following operations:</span></span>

   * <span data-ttu-id="fb53c-186">**Registrera** en gateway med en nyckel från hello Azure-portalen med hjälp av hello Register-knappen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-186">**Register** a gateway with a key from hello Azure portal by using hello Register button.</span></span>
   * <span data-ttu-id="fb53c-187">**Stoppa** hello Data Management Gateway-värdtjänsten körs på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-187">**Stop** hello Data Management Gateway Host Service running on your gateway machine.</span></span>
   * <span data-ttu-id="fb53c-188">**Schemalägga uppdateringar** toobe som installerats på en specifik tidpunkt hello.</span><span class="sxs-lookup"><span data-stu-id="fb53c-188">**Schedule updates** toobe installed at a specific time of hello day.</span></span>
   * <span data-ttu-id="fb53c-189">Visa när hello gateway var **senast uppdaterad**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-189">View when hello gateway was **last updated**.</span></span>
   * <span data-ttu-id="fb53c-190">Ange tiden då en uppdatering toohello gateway kan installeras.</span><span class="sxs-lookup"><span data-stu-id="fb53c-190">Specify time at which an update toohello gateway can be installed.</span></span>
8. <span data-ttu-id="fb53c-191">Växla toohello **inställningar** fliken hello certifikatet som anges i hello **certifikat** avsnittet är används tooencrypt/dekryptera autentiseringsuppgifterna för hello lokalt datalager som du anger på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-191">Switch toohello **Settings** tab. hello certificate specified in hello **Certificate** section is used tooencrypt/decrypt credentials for hello on-premises data store that you specify on hello portal.</span></span> <span data-ttu-id="fb53c-192">(valfritt) Klicka på **ändra** toouse ditt eget certifikat i stället.</span><span class="sxs-lookup"><span data-stu-id="fb53c-192">(optional) Click **Change** toouse your own certificate instead.</span></span> <span data-ttu-id="fb53c-193">Som standard använder hello gateway hello-certifikat som genereras automatiskt av hello Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fb53c-193">By default, hello gateway uses hello certificate that is auto-generated by hello Data Factory service.</span></span>

    ![Gateway-konfiguration för certifikat](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    <span data-ttu-id="fb53c-195">Du kan också göra hello följande åtgärder på hello **inställningar** fliken:</span><span class="sxs-lookup"><span data-stu-id="fb53c-195">You can also do hello following actions on hello **Settings** tab:</span></span>

   * <span data-ttu-id="fb53c-196">Visa eller exportera hello certifikatet som används av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-196">View or export hello certificate being used by hello gateway.</span></span>
   * <span data-ttu-id="fb53c-197">Ändra hello HTTPS-slutpunkt som används av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-197">Change hello HTTPS endpoint used by hello gateway.</span></span>    
   * <span data-ttu-id="fb53c-198">Ange en HTTP-proxy toobe som används av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-198">Set an HTTP proxy toobe used by hello gateway.</span></span>     
9. <span data-ttu-id="fb53c-199">(valfritt) Växla toohello **diagnostik** kontrollerar hello **aktivera utförlig loggning** om du vill tooenable utförlig loggning som du kan använda tootroubleshoot eventuella problem med hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-199">(optional) Switch toohello **Diagnostics** tab, check hello **Enable verbose logging** option if you want tooenable verbose logging that you can use tootroubleshoot any issues with hello gateway.</span></span> <span data-ttu-id="fb53c-200">hello loggningsinformation finns i **Loggboken** under **program- och tjänstloggar** -> **Data Management Gateway** nod.</span><span class="sxs-lookup"><span data-stu-id="fb53c-200">hello logging information can be found in **Event Viewer** under **Applications and Services Logs** -> **Data Management Gateway** node.</span></span>

    ![Fliken diagnostik](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    <span data-ttu-id="fb53c-202">Du kan också utföra följande åtgärder i hello hello **diagnostik** fliken:</span><span class="sxs-lookup"><span data-stu-id="fb53c-202">You can also perform hello following actions in hello **Diagnostics** tab:</span></span>

   * <span data-ttu-id="fb53c-203">Använd **Testanslutningen** avsnittet tooan lokal datakälla med hjälp av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-203">Use **Test Connection** section tooan on-premises data source using hello gateway.</span></span>
   * <span data-ttu-id="fb53c-204">Klicka på **visa loggfiler** toosee hello Data Management Gateway logga in ett fönster i Loggboken.</span><span class="sxs-lookup"><span data-stu-id="fb53c-204">Click **View Logs** toosee hello Data Management Gateway log in an Event Viewer window.</span></span>
   * <span data-ttu-id="fb53c-205">Klicka på **skicka loggar** tooupload en zip-fil med loggar av senaste sju dagarna tooMicrosoft toofacilitate felsökning av ditt problem.</span><span class="sxs-lookup"><span data-stu-id="fb53c-205">Click **Send Logs** tooupload a zip file with logs of last seven days tooMicrosoft toofacilitate troubleshooting of your issues.</span></span>
10. <span data-ttu-id="fb53c-206">På hello **diagnostik** på fliken hello **Testanslutningen** väljer **SqlServer** för hello hello datatyp lagra, ange hello namnet på hello databasserver, namnet på hello databas, ange autentiseringstypen, ange användarnamn och lösenord och klicka på **Test** tootest om hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="fb53c-206">On hello **Diagnostics** tab, in hello **Test Connection** section, select **SqlServer** for hello type of hello data store, enter hello name of hello database server, name of hello database, specify authentication type, enter user name, and password, and click **Test** tootest whether hello gateway can connect toohello database.</span></span>
11. <span data-ttu-id="fb53c-207">Växeln toohello webbläsare och i hello **Azure-portalen**, klickar du på **OK** på hello **konfigurera** sidan och klicka sedan på hello **nya datagateway** sidan.</span><span class="sxs-lookup"><span data-stu-id="fb53c-207">Switch toohello web browser, and in hello **Azure portal**, click **OK** on hello **Configure** page and then on hello **New data gateway** page.</span></span>
12. <span data-ttu-id="fb53c-208">Du bör se **adftutorialgateway** under **Datagatewayar** i hello trädvyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="fb53c-208">You should see **adftutorialgateway** under **Data Gateways** in hello tree view on hello left.</span></span>  <span data-ttu-id="fb53c-209">Om du klickar på den, bör du se hello associerade JSON.</span><span class="sxs-lookup"><span data-stu-id="fb53c-209">If you click it, you should see hello associated JSON.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="fb53c-210">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="fb53c-210">Create linked services</span></span>
<span data-ttu-id="fb53c-211">I det här steget skapar du två länkade tjänster: **AzureStorageLinkedService** och **SqlServerLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-211">In this step, you create two linked services: **AzureStorageLinkedService** and **SqlServerLinkedService**.</span></span> <span data-ttu-id="fb53c-212">Hej **SqlServerLinkedService** länkar en lokal SQL Server-databas och hello **AzureStorageLinkedService** länkade tjänsten länkar ett Azure blob store toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="fb53c-212">hello **SqlServerLinkedService** links an on-premises SQL Server database and hello **AzureStorageLinkedService** linked service links an Azure blob store toohello data factory.</span></span> <span data-ttu-id="fb53c-213">Du kan skapa en pipeline för senare i den här genomgången som kopierar data från hello lokala SQL Server-databasen toohello Azure blobstore.</span><span class="sxs-lookup"><span data-stu-id="fb53c-213">You create a pipeline later in this walkthrough that copies data from hello on-premises SQL Server database toohello Azure blob store.</span></span>

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a><span data-ttu-id="fb53c-214">Lägga till en länkad tjänst tooan lokala SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="fb53c-214">Add a linked service tooan on-premises SQL Server database</span></span>
1. <span data-ttu-id="fb53c-215">I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** på hello verktygsfältet och välj **SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-215">In hello **Data Factory Editor**, click **New data store** on hello toolbar and select **SQL Server**.</span></span>

   ![Ny SQL Server länkad tjänst](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. <span data-ttu-id="fb53c-217">I hello **JSON-redigerare** på rätt hello, hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb53c-217">In hello **JSON editor** on hello right, do hello following steps:</span></span>

   1. <span data-ttu-id="fb53c-218">För hello **gatewayName**, ange **adftutorialgateway**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-218">For hello **gatewayName**, specify **adftutorialgateway**.</span></span>    
   2. <span data-ttu-id="fb53c-219">I hello **connectionString**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb53c-219">In hello **connectionString**, do hello following steps:</span></span>    

      1. <span data-ttu-id="fb53c-220">För **servername**, anger hello namnet på hello-server som är värd för hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="fb53c-220">For **servername**, enter hello name of hello server that hosts hello SQL Server database.</span></span>
      2. <span data-ttu-id="fb53c-221">För **databasename**, ange hello namnet på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-221">For **databasename**, enter hello name of hello database.</span></span>
      3. <span data-ttu-id="fb53c-222">Klicka på **kryptera** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="fb53c-222">Click **Encrypt** button on hello toolbar.</span></span> <span data-ttu-id="fb53c-223">Du ser hello Autentiseringshanteraren program.</span><span class="sxs-lookup"><span data-stu-id="fb53c-223">You see hello Credentials Manager application.</span></span>

         ![Autentiseringsuppgifter Manager-program](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. <span data-ttu-id="fb53c-225">I hello **ställa in autentiseringsuppgifter** dialogrutan Ange autentiseringstypen, användarnamn och lösenord och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-225">In hello **Setting Credentials** dialog box, specify authentication type, user name, and password, and click **OK**.</span></span> <span data-ttu-id="fb53c-226">Om hello anslutningen är klar, hello krypterade autentiseringsuppgifter lagras i hello JSON och hello dialogrutan stängs.</span><span class="sxs-lookup"><span data-stu-id="fb53c-226">If hello connection is successful, hello encrypted credentials are stored in hello JSON and hello dialog box closes.</span></span>
      5. <span data-ttu-id="fb53c-227">Stäng hello tom webbläsarflik som startas hello dialogruta om den inte är stängd automatiskt och få tillbaka toohello flik med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-227">Close hello empty browser tab that launched hello dialog box if it is not automatically closed and get back toohello tab with hello Azure portal.</span></span>

         <span data-ttu-id="fb53c-228">Hello gateway-datorn är autentiseringsuppgifterna **krypterade** genom att använda ett certifikat som hello Data Factory tjänsten äger.</span><span class="sxs-lookup"><span data-stu-id="fb53c-228">On hello gateway machine, these credentials are **encrypted** by using a certificate that hello Data Factory service owns.</span></span> <span data-ttu-id="fb53c-229">Om du vill ha toouse hello certifikat som är associerad med hello Data Management Gateway i stället Se [ange autentiseringsuppgifterna på ett säkert sätt](#set-credentials-and-security).</span><span class="sxs-lookup"><span data-stu-id="fb53c-229">If you want toouse hello certificate that is associated with hello Data Management Gateway instead, see [Set credentials securely](#set-credentials-and-security).</span></span>    
   3. <span data-ttu-id="fb53c-230">Klicka på **distribuera** på hello i kommandofältet toodeploy hello tjänsten SQL Server som är länkad.</span><span class="sxs-lookup"><span data-stu-id="fb53c-230">Click **Deploy** on hello command bar toodeploy hello SQL Server linked service.</span></span> <span data-ttu-id="fb53c-231">Du bör se hello länkad tjänst i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-231">You should see hello linked service in hello tree view.</span></span>

      ![SQL Server som är länkad tjänst i hello trädvy](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="fb53c-233">Lägg till en länkad tjänst för ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="fb53c-233">Add a linked service for an Azure storage account</span></span>
1. <span data-ttu-id="fb53c-234">I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** hello kommandofältet och klicka på **Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-234">In hello **Data Factory Editor**, click **New data store** on hello command bar and click **Azure storage**.</span></span>
2. <span data-ttu-id="fb53c-235">Anger hello namnet på ditt Azure storage-konto för hello **kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-235">Enter hello name of your Azure storage account for hello **Account name**.</span></span>
3. <span data-ttu-id="fb53c-236">Ange hello nyckel för Azure storage-konto för hello **kontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-236">Enter hello key for your Azure storage account for hello **Account key**.</span></span>
4. <span data-ttu-id="fb53c-237">Klicka på **distribuera** toodeploy hello **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-237">Click **Deploy** toodeploy hello **AzureStorageLinkedService**.</span></span>

## <a name="create-datasets"></a><span data-ttu-id="fb53c-238">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="fb53c-238">Create datasets</span></span>
<span data-ttu-id="fb53c-239">I det här steget kan du skapa indata och utdata datauppsättningar som representerar inkommande och utgående data för hello kopieringen (lokal SQL Server-databas = > Azure blob storage).</span><span class="sxs-lookup"><span data-stu-id="fb53c-239">In this step, you create input and output datasets that represent input and output data for hello copy operation (On-premises SQL Server database => Azure blob storage).</span></span> <span data-ttu-id="fb53c-240">Innan du skapar datauppsättningar hello följande (detaljerade anvisningar följer hello lista):</span><span class="sxs-lookup"><span data-stu-id="fb53c-240">Before creating datasets, do hello following steps (detailed steps follows hello list):</span></span>

* <span data-ttu-id="fb53c-241">Skapa en tabell med namnet **tomma** i hello SQL Server-databas som du lagt till som en länkad tjänst toohello data factory och infoga några exempel poster i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-241">Create a table named **emp** in hello SQL Server Database you added as a linked service toohello data factory and insert a couple of sample entries into hello table.</span></span>
* <span data-ttu-id="fb53c-242">Skapa en blobbbehållare med namnet **adftutorial** i hello Azure blob storage-konto som du lagt till som en länkad tjänst toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="fb53c-242">Create a blob container named **adftutorial** in hello Azure blob storage account you added as a linked service toohello data factory.</span></span>

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a><span data-ttu-id="fb53c-243">Förbered på lokal SQL Server hello genomgång</span><span class="sxs-lookup"><span data-stu-id="fb53c-243">Prepare On-premises SQL Server for hello tutorial</span></span>
1. <span data-ttu-id="fb53c-244">Länkade tjänsten i hello-databasen som du angav för hello lokala SQL Server (**SqlServerLinkedService**), Använd följande SQL-skript toocreate hello hello **tomma** tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-244">In hello database you specified for hello on-premises SQL Server linked service (**SqlServerLinkedService**), use hello following SQL script toocreate hello **emp** table in hello database.</span></span>

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. <span data-ttu-id="fb53c-245">Infoga vissa exempel i hello tabell:</span><span class="sxs-lookup"><span data-stu-id="fb53c-245">Insert some sample into hello table:</span></span>

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a><span data-ttu-id="fb53c-246">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="fb53c-246">Create input dataset</span></span>

1. <span data-ttu-id="fb53c-247">I hello **Data Factory-redigeraren**, klickar du på **... Flera**, klickar du på **ny datamängd** på hello kommandofältet och klicka på **SQL Server-tabellen**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-247">In hello **Data Factory Editor**, click **... More**, click **New dataset** on hello command bar, and click **SQL Server table**.</span></span>
2. <span data-ttu-id="fb53c-248">Ersätt hello JSON i hello högra med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="fb53c-248">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   <span data-ttu-id="fb53c-249">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="fb53c-249">Note hello following points:</span></span>

   * <span data-ttu-id="fb53c-250">**typen** har angetts för**SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-250">**type** is set too**SqlServerTable**.</span></span>
   * <span data-ttu-id="fb53c-251">**tableName** har angetts för**tomma**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-251">**tableName** is set too**emp**.</span></span>
   * <span data-ttu-id="fb53c-252">**linkedServiceName** har angetts för**SqlServerLinkedService** (du har skapat den här länkade tjänsten tidigare i den här genomgången.).</span><span class="sxs-lookup"><span data-stu-id="fb53c-252">**linkedServiceName** is set too**SqlServerLinkedService** (you had created this linked service earlier in this walkthrough.).</span></span>
   * <span data-ttu-id="fb53c-253">För en inkommande dataset som genereras av en annan pipeline i Azure Data Factory, måste du ange **externa** för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-253">For an input dataset that is not generated by another pipeline in Azure Data Factory, you must set **external** too**true**.</span></span> <span data-ttu-id="fb53c-254">Den anger hello indata är producerade externa toohello Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fb53c-254">It denotes hello input data is produced external toohello Azure Data Factory service.</span></span> <span data-ttu-id="fb53c-255">Du kan också ange policyer externa data med hjälp av hello **externalData** element i hello **princip** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb53c-255">You can optionally specify any external data policies using hello **externalData** element in hello **Policy** section.</span></span>    

   <span data-ttu-id="fb53c-256">Se [flytta data till/från SQL Server](data-factory-sqlserver-connector.md) för ytterligare information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="fb53c-256">See [Move data to/from SQL Server](data-factory-sqlserver-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="fb53c-257">Klicka på **distribuera** på hello i kommandofältet toodeploy hello dataset.</span><span class="sxs-lookup"><span data-stu-id="fb53c-257">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="fb53c-258">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="fb53c-258">Create output dataset</span></span>

1. <span data-ttu-id="fb53c-259">I hello **Data Factory-redigeraren**, klickar du på **ny datamängd** på hello kommandofältet och klicka på **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-259">In hello **Data Factory Editor**, click **New dataset** on hello command bar, and click **Azure Blob storage**.</span></span>
2. <span data-ttu-id="fb53c-260">Ersätt hello JSON i hello högra med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="fb53c-260">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   <span data-ttu-id="fb53c-261">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="fb53c-261">Note hello following points:</span></span>

   * <span data-ttu-id="fb53c-262">**typen** har angetts för**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-262">**type** is set too**AzureBlob**.</span></span>
   * <span data-ttu-id="fb53c-263">**linkedServiceName** har angetts för**AzureStorageLinkedService** (du har skapat den här länkade tjänsten i steg 2).</span><span class="sxs-lookup"><span data-stu-id="fb53c-263">**linkedServiceName** is set too**AzureStorageLinkedService** (you had created this linked service in Step 2).</span></span>
   * <span data-ttu-id="fb53c-264">**folderPath** har angetts för**adftutorial/outfromonpremdf** där outfromonpremdf är hello mapp i hello adftutorial behållaren.</span><span class="sxs-lookup"><span data-stu-id="fb53c-264">**folderPath** is set too**adftutorial/outfromonpremdf** where outfromonpremdf is hello folder in hello adftutorial container.</span></span> <span data-ttu-id="fb53c-265">Skapa hello **adftutorial** behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="fb53c-265">Create hello **adftutorial** container if it does not already exist.</span></span>
   * <span data-ttu-id="fb53c-266">Hej **tillgänglighet** har angetts för**varje timme** (**frekvens** ställa in också**timme** och **intervall** ställa in också **1**).</span><span class="sxs-lookup"><span data-stu-id="fb53c-266">hello **availability** is set too**hourly** (**frequency** set too**hour** and **interval** set too**1**).</span></span>  <span data-ttu-id="fb53c-267">hello Data Factory-tjänsten som genererar en utdata datasektorn varje timme i hello **tomma** tabellen i hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="fb53c-267">hello Data Factory service generates an output data slice every hour in hello **emp** table in hello Azure SQL Database.</span></span>

   <span data-ttu-id="fb53c-268">Om du inte anger en **fileName** för en **utdatatabell**, hello genererade filer i hello **folderPath** namnges i hello följande format: Data.<Guid>. txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span><span class="sxs-lookup"><span data-stu-id="fb53c-268">If you do not specify a **fileName** for an **output table**, hello generated files in hello **folderPath** are named in hello following format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span></span>

   <span data-ttu-id="fb53c-269">tooset **folderPath** och **fileName** dynamiskt utifrån hello **SliceStart** tid, använda hello partitionedBy egenskap.</span><span class="sxs-lookup"><span data-stu-id="fb53c-269">tooset **folderPath** and **fileName** dynamically based on hello **SliceStart** time, use hello partitionedBy property.</span></span> <span data-ttu-id="fb53c-270">I följande exempel hello, folderPath använder år, månad och dag från hello SliceStart (starttiden för hello sektorn bearbetas) och fileName använder timme från hello SliceStart.</span><span class="sxs-lookup"><span data-stu-id="fb53c-270">In hello following example, folderPath uses Year, Month, and Day from hello SliceStart (start time of hello slice being processed) and fileName uses Hour from hello SliceStart.</span></span> <span data-ttu-id="fb53c-271">Om exempelvis ett segment skapas för 2014-10-20T08:00:00, hello mappnamn toowikidatagateway/wikisampledataout/2014/10/20 och hello filnamn har angetts too08.csv.</span><span class="sxs-lookup"><span data-stu-id="fb53c-271">For example, if a slice is being produced for 2014-10-20T08:00:00, hello folderName is set toowikidatagateway/wikisampledataout/2014/10/20 and hello fileName is set too08.csv.</span></span>

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    <span data-ttu-id="fb53c-272">Se [flytta data till/från Azure Blob Storage](data-factory-azure-blob-connector.md) mer information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="fb53c-272">See [Move data to/from Azure Blob Storage](data-factory-azure-blob-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="fb53c-273">Klicka på **distribuera** på hello i kommandofältet toodeploy hello dataset.</span><span class="sxs-lookup"><span data-stu-id="fb53c-273">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span> <span data-ttu-id="fb53c-274">Bekräfta att du ser både hello datauppsättningar i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="fb53c-274">Confirm that you see both hello datasets in hello tree view.</span></span>  

## <a name="create-pipeline"></a><span data-ttu-id="fb53c-275">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="fb53c-275">Create pipeline</span></span>
<span data-ttu-id="fb53c-276">I det här steget skapar du en **pipeline** med en **Kopieringsaktiviteten** som använder **EmpOnPremSQLTable** som indata och **OutputBlobTable** som utdata.</span><span class="sxs-lookup"><span data-stu-id="fb53c-276">In this step, you create a **pipeline** with one **Copy Activity** that uses **EmpOnPremSQLTable** as input and **OutputBlobTable** as output.</span></span>

1. <span data-ttu-id="fb53c-277">I Data Factory-redigeraren, klicka på **... More (Mer)** och sedan på **Ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-277">In Data Factory Editor, click **... More**, and click **New pipeline**.</span></span>
2. <span data-ttu-id="fb53c-278">Ersätt hello JSON i hello högra med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="fb53c-278">Replace hello JSON in hello right pane with hello following text:</span></span>    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > <span data-ttu-id="fb53c-279">Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag.</span><span class="sxs-lookup"><span data-stu-id="fb53c-279">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span>
   >
   >

   <span data-ttu-id="fb53c-280">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="fb53c-280">Note hello following points:</span></span>

   * <span data-ttu-id="fb53c-281">Under hello aktiviteter är bara aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-281">In hello activities section, there is only activity whose **type** is set too**Copy**.</span></span>
   * <span data-ttu-id="fb53c-282">**Indata** för hello aktiviteten är inställd för**EmpOnPremSQLTable** och **utdata** för hello aktiviteten är inställd för**OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-282">**Input** for hello activity is set too**EmpOnPremSQLTable** and **output** for hello activity is set too**OutputBlobTable**.</span></span>
   * <span data-ttu-id="fb53c-283">I hello **typeProperties** avsnittet **SqlSource** har angetts som hello **typ av datakälla** och ** BlobSink ** har angetts som hello **sink typen**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-283">In hello **typeProperties** section, **SqlSource** is specified as hello **source type** and **BlobSink **is specified as hello **sink type**.</span></span>
   * <span data-ttu-id="fb53c-284">SQL-frågan `select * from emp` har angetts för hello **sqlReaderQuery** -egenskapen för **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-284">SQL query `select * from emp` is specified for hello **sqlReaderQuery** property of **SqlSource**.</span></span>

   <span data-ttu-id="fb53c-285">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="fb53c-285">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="fb53c-286">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="fb53c-286">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="fb53c-287">Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-287">hello **end** time is optional, but we use it in this tutorial.</span></span>

   <span data-ttu-id="fb53c-288">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="fb53c-288">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="fb53c-289">toorun hello pipeline på obestämd tid, ange **9999-9-9** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-289">toorun hello pipeline indefinitely, specify **9/9/9999** as hello value for hello **end** property.</span></span>

   <span data-ttu-id="fb53c-290">Du definierar hello varaktighet i vilka hello datasektorer bearbetas baserat på hello **tillgänglighet** egenskaper som definierats för varje Azure Data Factory-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="fb53c-290">You are defining hello time duration in which hello data slices are processed based on hello **Availability** properties that were defined for each Azure Data Factory dataset.</span></span>

   <span data-ttu-id="fb53c-291">I exemplet hello finns 24 datasektorer som varje datasektorn produceras per timma.</span><span class="sxs-lookup"><span data-stu-id="fb53c-291">In hello example, there are 24 data slices as each data slice is produced hourly.</span></span>        
3. <span data-ttu-id="fb53c-292">Klicka på **distribuera** på hello i kommandofältet toodeploy hello datauppsättningen (tabellen är en rektangulär dataset).</span><span class="sxs-lookup"><span data-stu-id="fb53c-292">Click **Deploy** on hello command bar toodeploy hello dataset (table is a rectangular dataset).</span></span> <span data-ttu-id="fb53c-293">Bekräfta att hello pipeline visas i hello trädvyn under **Pipelines** nod.</span><span class="sxs-lookup"><span data-stu-id="fb53c-293">Confirm that hello pipeline shows up in hello tree view under **Pipelines** node.</span></span>  
4. <span data-ttu-id="fb53c-294">Klicka på **X** två gånger tooclose hello sidan tooget tillbaka toohello **Datafabriken** för hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-294">Now, click **X** twice tooclose hello page tooget back toohello **Data Factory** page for hello **ADFTutorialOnPremDF**.</span></span>

<span data-ttu-id="fb53c-295">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="fb53c-295">**Congratulations!**</span></span> <span data-ttu-id="fb53c-296">Du har skapat ett Azure data factory, länkade tjänster, datauppsättningar, och en pipeline och schemalagda hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="fb53c-296">You have successfully created an Azure data factory, linked services, datasets, and a pipeline and scheduled hello pipeline.</span></span>

#### <a name="view-hello-data-factory-in-a-diagram-view"></a><span data-ttu-id="fb53c-297">Visa hello data factory i en diagramvy</span><span class="sxs-lookup"><span data-stu-id="fb53c-297">View hello data factory in a Diagram View</span></span>
1. <span data-ttu-id="fb53c-298">I hello **Azure-portalen**, klickar du på **Diagram** panelen på hello startsidan för hello **ADFTutorialOnPremDF** datafabriken.</span><span class="sxs-lookup"><span data-stu-id="fb53c-298">In hello **Azure portal**, click **Diagram** tile on hello home page for hello **ADFTutorialOnPremDF** data factory.</span></span> <span data-ttu-id="fb53c-299">:</span><span class="sxs-lookup"><span data-stu-id="fb53c-299">:</span></span>

    ![Diagram-länk](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. <span data-ttu-id="fb53c-301">Du bör se hello diagram liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="fb53c-301">You should see hello diagram similar toohello following image:</span></span>

    ![Diagramvy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    <span data-ttu-id="fb53c-303">Du kan zooma in, Zooma in, Zooma too100%, Zooma toofit, automatiskt position pipelines och datauppsättningar och visa härkomst information (markerar överordnade och underordnade objekt av valda objekt).</span><span class="sxs-lookup"><span data-stu-id="fb53c-303">You can zoom in, zoom out, zoom too100%, zoom toofit, automatically position pipelines and datasets, and show lineage information (highlights upstream and downstream items of selected items).</span></span>  <span data-ttu-id="fb53c-304">Du kan dubbelklicka på ett objekt (i/o-datauppsättningen eller pipeline) toosee egenskaper för den.</span><span class="sxs-lookup"><span data-stu-id="fb53c-304">You can double-click an object (input/output dataset or pipeline) toosee properties for it.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="fb53c-305">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="fb53c-305">Monitor pipeline</span></span>
<span data-ttu-id="fb53c-306">I det här steget använder du hello Azure portal toomonitor vad som händer i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="fb53c-306">In this step, you use hello Azure portal toomonitor what’s going on in an Azure data factory.</span></span> <span data-ttu-id="fb53c-307">Du kan också använda PowerShell-cmdlets toomonitor datamängd och pipelinor.</span><span class="sxs-lookup"><span data-stu-id="fb53c-307">You can also use PowerShell cmdlets toomonitor datasets and pipelines.</span></span> <span data-ttu-id="fb53c-308">Mer information om övervakning finns [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="fb53c-308">For details about monitoring, see [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md).</span></span>

1. <span data-ttu-id="fb53c-309">Dubbelklicka i hello diagram **EmpOnPremSQLTable**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-309">In hello diagram, double-click **EmpOnPremSQLTable**.</span></span>  

    ![EmpOnPremSQLTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. <span data-ttu-id="fb53c-311">Observera att alla hello datasektorer in finns i **klar** tillstånd eftersom hello pipeline varaktighet (tid tooend starttid) hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="fb53c-311">Notice that all hello data slices up are in **Ready** state because hello pipeline duration (start time tooend time) is in hello past.</span></span> <span data-ttu-id="fb53c-312">Det är också eftersom du har infogat hello data i hello SQL Server-databas och det är där alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="fb53c-312">It is also because you have inserted hello data in hello SQL Server database and it is there all hello time.</span></span> <span data-ttu-id="fb53c-313">Bekräfta att ingen segment visas i hello **problemet segment** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fb53c-313">Confirm that no slices show up in hello **Problem slices** section at hello bottom.</span></span> <span data-ttu-id="fb53c-314">tooview alla hello segment, klicka på **finns mer** längst hello hello lista över segment.</span><span class="sxs-lookup"><span data-stu-id="fb53c-314">tooview all hello slices, click **See More** at hello bottom of hello list of slices.</span></span>
3. <span data-ttu-id="fb53c-315">Nu i hello **datauppsättningar** klickar du på **OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-315">Now, In hello **Datasets** page, click **OutputBlobTable**.</span></span>

    ![OputputBlobTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. <span data-ttu-id="fb53c-317">Klicka på någon datasektorn hello listan och du bör se hello **Datasektorn** sidan.</span><span class="sxs-lookup"><span data-stu-id="fb53c-317">Click any data slice from hello list and you should see hello **Data Slice** page.</span></span> <span data-ttu-id="fb53c-318">Du kan se aktiviteten körs för hello segment.</span><span class="sxs-lookup"><span data-stu-id="fb53c-318">You see activity runs for hello slice.</span></span> <span data-ttu-id="fb53c-319">Endast en aktivitet körs vanligtvis visas.</span><span class="sxs-lookup"><span data-stu-id="fb53c-319">You see only one activity run usually.</span></span>  

    ![Data sektorn bladet](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    <span data-ttu-id="fb53c-321">Om hello segment inte hello **klar** tillstånd, som du kan se hello överordnade sektorer som inte är redo och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista.</span><span class="sxs-lookup"><span data-stu-id="fb53c-321">If hello slice is not in hello **Ready** state, you can see hello upstream slices that are not Ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span>
5. <span data-ttu-id="fb53c-322">Klicka på hello **aktiviteten kör** hello listan vid hello nedre toosee **aktivitet köras information**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-322">Click hello **activity run** from hello list at hello bottom toosee **activity run details**.</span></span>

   ![Aktiviteten kör informationssidan](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   <span data-ttu-id="fb53c-324">Visas information som dataflöde, varaktighet och hello gateway används tootransfer hello data.</span><span class="sxs-lookup"><span data-stu-id="fb53c-324">You would see information such as throughput, duration, and hello gateway used tootransfer hello data.</span></span>
6. <span data-ttu-id="fb53c-325">Klicka på **X** tooclose alla hello sidor förrän du</span><span class="sxs-lookup"><span data-stu-id="fb53c-325">Click **X** tooclose all hello pages until you</span></span>
7. <span data-ttu-id="fb53c-326">Hämta toohello startsidan för hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="fb53c-326">get back toohello home page for hello **ADFTutorialOnPremDF**.</span></span>
8. <span data-ttu-id="fb53c-327">(valfritt) Klicka på **Pipelines**, klickar du på **ADFTutorialOnPremDF**, och detaljvisning indatatabeller (**förbrukad**) eller utdata datauppsättningar (**framställda**).</span><span class="sxs-lookup"><span data-stu-id="fb53c-327">(optional) Click **Pipelines**, click **ADFTutorialOnPremDF**, and drill through input tables (**Consumed**) or output datasets (**Produced**).</span></span>
9. <span data-ttu-id="fb53c-328">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) tooverify att en blob/fil skapas för varje timme.</span><span class="sxs-lookup"><span data-stu-id="fb53c-328">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) tooverify that a blob/file is created for each hour.</span></span>

   ![Azure Lagringsutforskaren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a><span data-ttu-id="fb53c-330">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb53c-330">Next steps</span></span>
* <span data-ttu-id="fb53c-331">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel för alla hello information om hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="fb53c-331">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello Data Management Gateway.</span></span>
* <span data-ttu-id="fb53c-332">Se [kopiera data från Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn om hur toouse Kopieringsaktiviteten toomove data från en källdata lagrar tooa sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="fb53c-332">See [Copy data from Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn about how toouse Copy Activity toomove data from a source data store tooa sink data store.</span></span>
