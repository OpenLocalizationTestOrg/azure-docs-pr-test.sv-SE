---
title: Flytta data - Data Management Gateway | Microsoft Docs
description: "Konfigurera en datagateway för att flytta data mellan lokalt och i molnet. Flytta dina data med hjälp av Data Management Gateway i Azure Data Factory."
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
ms.openlocfilehash: 565091e24a8c0009793e2e2365fb95013cad5028
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a><span data-ttu-id="f4cf2-105">Flytta data mellan lokala källor och molnet med Data Management Gateway</span><span class="sxs-lookup"><span data-stu-id="f4cf2-105">Move data between on-premises sources and the cloud with Data Management Gateway</span></span>
<span data-ttu-id="f4cf2-106">Den här artikeln innehåller en översikt över dataintegration mellan lokala datalager och datalager i molnet med hjälp av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-106">This article provides an overview of data integration between on-premises data stores and cloud data stores using Data Factory.</span></span> <span data-ttu-id="f4cf2-107">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln och andra data factory grundläggande begrepp artiklar: [datauppsättningar](data-factory-create-datasets.md) och [pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-107">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article and other data factory core concepts articles: [datasets](data-factory-create-datasets.md) and [pipelines](data-factory-create-pipelines.md).</span></span>

## <a name="data-management-gateway"></a><span data-ttu-id="f4cf2-108">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="f4cf2-108">Data Management Gateway</span></span>
<span data-ttu-id="f4cf2-109">Data Management Gateway måste du installera på den lokala datorn att flytta data till och från ett lokalt datalager.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-109">You must install Data Management Gateway on your on-premises machine to enable moving data to/from an on-premises data store.</span></span> <span data-ttu-id="f4cf2-110">Gatewayen kan installeras på samma dator som dataarkiv eller på en annan dator som en gateway kan ansluta till datalagret.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-110">The gateway can be installed on the same machine as the data store or on a different machine as long as the gateway can connect to the data store.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4cf2-111">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> 

<span data-ttu-id="f4cf2-112">Den här genomgången visar hur du skapar en datafabrik med en pipeline som flyttar data från en lokal **SQL Server** databasen till en Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-112">The following walkthrough shows you how to create a data factory with a pipeline that moves data from an on-premises **SQL Server** database to an Azure blob storage.</span></span> <span data-ttu-id="f4cf2-113">Som en del av den här genomgången kommer du att installera och konfigurera Data Management Gateway på din dator.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-113">As part of the walkthrough, you install and configure the Data Management Gateway on your machine.</span></span>

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a><span data-ttu-id="f4cf2-114">Genomgång: kopiera lokala data till molnet</span><span class="sxs-lookup"><span data-stu-id="f4cf2-114">Walkthrough: copy on-premises data to cloud</span></span>
<span data-ttu-id="f4cf2-115">I den här genomgången kan du göra följande:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-115">In this walkthrough you do the following steps:</span></span> 

1. <span data-ttu-id="f4cf2-116">Skapa en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-116">Create a data factory.</span></span>
2. <span data-ttu-id="f4cf2-117">Skapa en data management gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-117">Create a data management gateway.</span></span> 
3. <span data-ttu-id="f4cf2-118">Skapa länkade tjänster för källa och mottagare datalager.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-118">Create linked services for source and sink data stores.</span></span>
4. <span data-ttu-id="f4cf2-119">Skapa datauppsättningar som representerar indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-119">Create datasets to represent input and output data.</span></span>
5. <span data-ttu-id="f4cf2-120">Skapa en pipeline med en kopieringsaktiviteten att flytta data.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-120">Create a pipeline with a copy activity to move the data.</span></span>

## <a name="prerequisites-for-the-tutorial"></a><span data-ttu-id="f4cf2-121">Förutsättningar för självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="f4cf2-121">Prerequisites for the tutorial</span></span>
<span data-ttu-id="f4cf2-122">Innan du börjar den här genomgången måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-122">Before you begin this walkthrough, you must have the following prerequisites:</span></span>

* <span data-ttu-id="f4cf2-123">**Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-123">**Azure subscription**.</span></span>  <span data-ttu-id="f4cf2-124">Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-124">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f4cf2-125">Finns det [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-125">See the [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="f4cf2-126">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-126">**Azure Storage Account**.</span></span> <span data-ttu-id="f4cf2-127">Du använder blobblagring som en **mål-sink** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-127">You use the blob storage as a **destination/sink** data store in this tutorial.</span></span> <span data-ttu-id="f4cf2-128">Om du inte har ett Azure storage-konto finns i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel steg för att skapa en.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-128">if you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps to create one.</span></span>
* <span data-ttu-id="f4cf2-129">**SQLServer**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-129">**SQL Server**.</span></span> <span data-ttu-id="f4cf2-130">Du använder en lokal SQL Server-databas som en **källa** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-130">You use an on-premises SQL Server database as a **source** data store in this tutorial.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="f4cf2-131">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="f4cf2-131">Create data factory</span></span>
<span data-ttu-id="f4cf2-132">I det här steget kan du använda Azure-portalen för att skapa en Azure Data Factory-instans med namnet **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-132">In this step, you use the Azure portal to create an Azure Data Factory instance named **ADFTutorialOnPremDF**.</span></span>

1. <span data-ttu-id="f4cf2-133">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-133">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f4cf2-134">Klicka på **+ ny**, klickar du på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-134">Click **+ NEW**, click **Intelligence + analytics**, and click **Data Factory**.</span></span>

   ![Nytt->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. <span data-ttu-id="f4cf2-136">I den **nya datafabriken** anger **ADFTutorialOnPremDF** för namnet.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-136">In the **New data factory** page, enter **ADFTutorialOnPremDF** for the Name.</span></span>

    ![Lägg till på startsidan](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > <span data-ttu-id="f4cf2-138">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-138">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="f4cf2-139">Om du får felmeddelandet: **datafabriksnamnet ”ADFTutorialOnPremDF” är inte tillgänglig**, ändra namnet på data factory (till exempel yournameADFTutorialOnPremDF) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-139">If you receive the error: **Data factory name “ADFTutorialOnPremDF” is not available**, change the name of the data factory (for example, yournameADFTutorialOnPremDF) and try creating again.</span></span> <span data-ttu-id="f4cf2-140">Använd det här namnet i stället för ADFTutorialOnPremDF när du utför stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-140">Use this name in place of ADFTutorialOnPremDF while performing remaining steps in this tutorial.</span></span>
   >
   > <span data-ttu-id="f4cf2-141">Namnet på datafabriken kan komma att registreras som ett **DNS**-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-141">The name of the data factory may be registered as a **DNS** name in the future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="f4cf2-142">Välj den **Azure-prenumeration** där du vill att datafabriken ska skapas.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-142">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="f4cf2-143">Välj befintlig **resursgrupp** eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-143">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="f4cf2-144">För självstudierna skapar du en resursgrupp med namnet: **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-144">For the tutorial, create a resource group named: **ADFTutorialResourceGroup**.</span></span>
6. <span data-ttu-id="f4cf2-145">Klicka på **skapa** på den **nya data factory** sidan.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-145">Click **Create** on the **New data factory** page.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="f4cf2-146">Om du vill skapa Data Factory-instanser måste du vara medlem i [Data Factory-deltagarrollen](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) på gruppnivå/resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-146">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="f4cf2-147">När du har skapats visas den **Data Factory** sidan som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-147">After creation is complete, you see the **Data Factory** page as shown in the following image:</span></span>

   ![Data Factory-startsida](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a><span data-ttu-id="f4cf2-149">Skapa en gateway</span><span class="sxs-lookup"><span data-stu-id="f4cf2-149">Create gateway</span></span>
1. <span data-ttu-id="f4cf2-150">I den **Datafabriken** klickar du på **författare och distribuera** rutan för att starta den **Editor** för data factory.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-150">In the **Data Factory** page, click **Author and deploy** tile to launch the **Editor** for the data factory.</span></span>

    ![Ikonen Författare och distribution](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. <span data-ttu-id="f4cf2-152">I den Data Factory-redigeraren, klicka på **... Flera** i verktygsfältet och klicka sedan på **nya datagateway**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-152">In the Data Factory Editor, click **... More** on the toolbar and then click **New data gateway**.</span></span> <span data-ttu-id="f4cf2-153">Du kan också högerklicka på **Datagatewayar** i trädvyn och klicka på **nya datagateway**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-153">Alternatively, you can right-click **Data Gateways** in the tree view, and click **New data gateway**.</span></span>

   ![Nya datagateway i verktygsfältet](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. <span data-ttu-id="f4cf2-155">I den **skapa** anger **adftutorialgateway** för den **namn**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-155">In the **Create** page, enter **adftutorialgateway** for the **name**, and click **OK**.</span></span>     

    ![Skapa sida för Gateway](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > <span data-ttu-id="f4cf2-157">I den här genomgången Skapa logiska gateway med endast en nod (lokala Windows-dator).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-157">In this walkthrough, you create the logical gateway with only one node (on-premises Windows machine).</span></span> <span data-ttu-id="f4cf2-158">Du kan skala ut en data management gateway genom att associera flera lokala datorer med gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-158">You can scale out a data management gateway by associating multiple on-premises machines with the gateway.</span></span> <span data-ttu-id="f4cf2-159">Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-159">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="f4cf2-160">Den här funktionen finns också en logisk gateway med en enda nod.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-160">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="f4cf2-161">Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-161">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>  
4. <span data-ttu-id="f4cf2-162">I den **konfigurera** klickar du på **installera direkt på den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-162">In the **Configure** page, click **Install directly on this computer**.</span></span> <span data-ttu-id="f4cf2-163">Den här åtgärden hämtar installationspaketet för gatewayen, installerar, konfigurerar och registrerar gatewayen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-163">This action downloads the installation package for the gateway, installs, configures, and registers the gateway on the computer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="f4cf2-164">Använd Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-164">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>
   >
   > <span data-ttu-id="f4cf2-165">Om du använder Chrome, går du till [Chrome Web Store](https://chrome.google.com/webstore/), söker efter nyckelordet ”ClickOnce”, väljer ett av ClickOnce-tilläggen och installerar det.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-165">If you are using Chrome, go to the [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>
   >
   > <span data-ttu-id="f4cf2-166">Gör likadant för Firefox (installera tillägget).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-166">Do the same for Firefox (install add-in).</span></span> <span data-ttu-id="f4cf2-167">Klicka på **öppna menyn** i verktygsfältet (**tre horisontella raderna** i det övre högra hörnet), klickar du på **tillägg**, söka med nyckelordet ”ClickOnce”, väljer du något av de ClickOnce-tillägg och installera den.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-167">Click **Open Menu** button on the toolbar (**three horizontal lines** in the top-right corner), click **Add-ons**, search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>    
   >
   >

    ![Gateway - konfigurera](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    <span data-ttu-id="f4cf2-169">Det här sättet är det enklaste sättet (enkelklickning) för att hämta, installera, konfigurera och registrera gatewayen i ett enda steg.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-169">This way is the easiest way (one-click) to download, install, configure, and register the gateway in one single step.</span></span> <span data-ttu-id="f4cf2-170">Du kan se den **Microsoft Data Management Gateway Configuration Manager** programmet är installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-170">You can see the **Microsoft Data Management Gateway Configuration Manager** application is installed on your computer.</span></span> <span data-ttu-id="f4cf2-171">Du kan också hitta den körbara filen **ConfigManager.exe** i mappen: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-171">You can also find the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span></span>

    <span data-ttu-id="f4cf2-172">Du kan också hämta och installera gateway manuellt med hjälp av länkarna på den här sidan och registrera den med den nyckel som visas i den **ny nyckel** textruta.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-172">You can also download and install gateway manually by using the links in this page and register it using the key shown in the **NEW KEY** text box.</span></span>

    <span data-ttu-id="f4cf2-173">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel detaljerad information om gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-173">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all the details about the gateway.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f4cf2-174">Du måste vara administratör på den lokala datorn för att installera och konfigurera Data Management Gateway har.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-174">You must be an administrator on the local computer to install and configure the Data Management Gateway successfully.</span></span> <span data-ttu-id="f4cf2-175">Du kan lägga till fler användare att de **Data Management Gateway-användare** lokala Windows-gruppen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-175">You can add additional users to the **Data Management Gateway Users** local Windows group.</span></span> <span data-ttu-id="f4cf2-176">Medlemmar i gruppen kan använda verktyget Data Management Gateway Configuration Manager för att konfigurera gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-176">The members of this group can use the Data Management Gateway Configuration Manager tool to configure the gateway.</span></span>
   >
   >
5. <span data-ttu-id="f4cf2-177">Vänta några minuter eller vänta tills du ser följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-177">Wait for a couple of minutes or wait until you see the following notification message:</span></span>

    ![Gatewayinstallationen lyckades](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. <span data-ttu-id="f4cf2-179">Starta **Data Management Gateway Configuration Manager** på datorn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-179">Launch **Data Management Gateway Configuration Manager** application on your computer.</span></span> <span data-ttu-id="f4cf2-180">I den **Sök** fönster, Skriv **Data Management Gateway** åtkomst till det här verktyget.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-180">In the **Search** window, type **Data Management Gateway** to access this utility.</span></span> <span data-ttu-id="f4cf2-181">Du kan också hitta den körbara filen **ConfigManager.exe** i mappen: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span><span class="sxs-lookup"><span data-stu-id="f4cf2-181">You can also find the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span></span>

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. <span data-ttu-id="f4cf2-183">Bekräfta att du ser `adftutorialgateway is connected to the cloud service` meddelande.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-183">Confirm that you see `adftutorialgateway is connected to the cloud service` message.</span></span> <span data-ttu-id="f4cf2-184">Statusfältet längst ned visar **anslutna till Molntjänsten** tillsammans med en **grön bock**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-184">The status bar the bottom displays **Connected to the cloud service** along with a **green check mark**.</span></span>

    <span data-ttu-id="f4cf2-185">På den **Start** fliken kan du också göra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-185">On the **Home** tab, you can also do the following operations:</span></span>

   * <span data-ttu-id="f4cf2-186">**Registrera** en gateway med en nyckel från Azure-portalen med hjälp av knappen registrera.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-186">**Register** a gateway with a key from the Azure portal by using the Register button.</span></span>
   * <span data-ttu-id="f4cf2-187">**Stoppa** i Data Management Gateway-värdtjänsten körs på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-187">**Stop** the Data Management Gateway Host Service running on your gateway machine.</span></span>
   * <span data-ttu-id="f4cf2-188">**Schemalägga uppdateringar** installeras vid en viss tid på dagen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-188">**Schedule updates** to be installed at a specific time of the day.</span></span>
   * <span data-ttu-id="f4cf2-189">Visa när gatewayen **senast uppdaterad**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-189">View when the gateway was **last updated**.</span></span>
   * <span data-ttu-id="f4cf2-190">Ange tiden då en uppdatering till gateway kan installeras.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-190">Specify time at which an update to the gateway can be installed.</span></span>
8. <span data-ttu-id="f4cf2-191">Växla till den **inställningar** fliken. Certifikatet som anges i den **certifikat** avsnitt används för att kryptera/dekryptera autentiseringsuppgifterna för lokal datalagret som du anger på portalen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-191">Switch to the **Settings** tab. The certificate specified in the **Certificate** section is used to encrypt/decrypt credentials for the on-premises data store that you specify on the portal.</span></span> <span data-ttu-id="f4cf2-192">(valfritt) Klicka på **ändra** använda ditt eget certifikat i stället.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-192">(optional) Click **Change** to use your own certificate instead.</span></span> <span data-ttu-id="f4cf2-193">Som standard använder det certifikat som genereras automatiskt av tjänsten Data Factory i gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-193">By default, the gateway uses the certificate that is auto-generated by the Data Factory service.</span></span>

    ![Gateway-konfiguration för certifikat](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    <span data-ttu-id="f4cf2-195">Du kan också utföra följande åtgärder på den **inställningar** fliken:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-195">You can also do the following actions on the **Settings** tab:</span></span>

   * <span data-ttu-id="f4cf2-196">Visa eller exportera certifikatet som används av gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-196">View or export the certificate being used by the gateway.</span></span>
   * <span data-ttu-id="f4cf2-197">Ändra den HTTPS-slutpunkt som används av gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-197">Change the HTTPS endpoint used by the gateway.</span></span>    
   * <span data-ttu-id="f4cf2-198">Ange en HTTP-proxy som ska användas av gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-198">Set an HTTP proxy to be used by the gateway.</span></span>     
9. <span data-ttu-id="f4cf2-199">(valfritt) Växla till den **diagnostik** markerar den **aktivera utförlig loggning** om du vill aktivera utförlig loggning som du kan använda för att felsöka eventuella problem med gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-199">(optional) Switch to the **Diagnostics** tab, check the **Enable verbose logging** option if you want to enable verbose logging that you can use to troubleshoot any issues with the gateway.</span></span> <span data-ttu-id="f4cf2-200">Loggningsinformationen om finns i **Loggboken** under **program- och tjänstloggar** -> **Data Management Gateway** nod.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-200">The logging information can be found in **Event Viewer** under **Applications and Services Logs** -> **Data Management Gateway** node.</span></span>

    ![Fliken diagnostik](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    <span data-ttu-id="f4cf2-202">Du kan också utföra följande åtgärder i den **diagnostik** fliken:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-202">You can also perform the following actions in the **Diagnostics** tab:</span></span>

   * <span data-ttu-id="f4cf2-203">Använd **Testanslutningen** avsnittet till en lokal datakälla med gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-203">Use **Test Connection** section to an on-premises data source using the gateway.</span></span>
   * <span data-ttu-id="f4cf2-204">Klicka på **visa loggfiler** att se Data Management Gateway-loggen i ett fönster i Loggboken.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-204">Click **View Logs** to see the Data Management Gateway log in an Event Viewer window.</span></span>
   * <span data-ttu-id="f4cf2-205">Klicka på **skicka loggar** att överföra en zip-fil med loggar av senaste sju dagarna till Microsoft för att underlätta felsökning av ditt problem.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-205">Click **Send Logs** to upload a zip file with logs of last seven days to Microsoft to facilitate troubleshooting of your issues.</span></span>
10. <span data-ttu-id="f4cf2-206">På den **diagnostik** fliken den **Testanslutningen** väljer **SqlServer** för typ av datalagret, ange namnet på databasservern, namnet på databasen, Ange autentiseringstypen, ange användarnamn och lösenord och klicka på **testa** att testa om gatewayen kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-206">On the **Diagnostics** tab, in the **Test Connection** section, select **SqlServer** for the type of the data store, enter the name of the database server, name of the database, specify authentication type, enter user name, and password, and click **Test** to test whether the gateway can connect to the database.</span></span>
11. <span data-ttu-id="f4cf2-207">Växla till webbläsaren och i den **Azure-portalen**, klickar du på **OK** på den **konfigurera** sidan och klicka sedan på den **nya datagateway** sidan.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-207">Switch to the web browser, and in the **Azure portal**, click **OK** on the **Configure** page and then on the **New data gateway** page.</span></span>
12. <span data-ttu-id="f4cf2-208">Du bör se **adftutorialgateway** under **Datagatewayar** i trädvyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-208">You should see **adftutorialgateway** under **Data Gateways** in the tree view on the left.</span></span>  <span data-ttu-id="f4cf2-209">Om du klickar på det, bör du se associerade JSON.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-209">If you click it, you should see the associated JSON.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="f4cf2-210">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="f4cf2-210">Create linked services</span></span>
<span data-ttu-id="f4cf2-211">I det här steget skapar du två länkade tjänster: **AzureStorageLinkedService** och **SqlServerLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-211">In this step, you create two linked services: **AzureStorageLinkedService** and **SqlServerLinkedService**.</span></span> <span data-ttu-id="f4cf2-212">Den **SqlServerLinkedService** länkar en lokal SQL Server-databas och **AzureStorageLinkedService** länkade tjänsten länkar ett Azure blobstore till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-212">The **SqlServerLinkedService** links an on-premises SQL Server database and the **AzureStorageLinkedService** linked service links an Azure blob store to the data factory.</span></span> <span data-ttu-id="f4cf2-213">Du kan skapa en pipeline för senare i den här genomgången som kopierar data från den lokala SQL Server-databasen till Azure blobstore.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-213">You create a pipeline later in this walkthrough that copies data from the on-premises SQL Server database to the Azure blob store.</span></span>

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a><span data-ttu-id="f4cf2-214">Lägg till en länkad tjänst till en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="f4cf2-214">Add a linked service to an on-premises SQL Server database</span></span>
1. <span data-ttu-id="f4cf2-215">I den **Data Factory-redigeraren**, klickar du på **Nytt datalager** i verktygsfältet och välj **SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-215">In the **Data Factory Editor**, click **New data store** on the toolbar and select **SQL Server**.</span></span>

   ![Ny SQL Server länkad tjänst](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. <span data-ttu-id="f4cf2-217">I den **JSON-redigerare** till höger, gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-217">In the **JSON editor** on the right, do the following steps:</span></span>

   1. <span data-ttu-id="f4cf2-218">För den **gatewayName**, ange **adftutorialgateway**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-218">For the **gatewayName**, specify **adftutorialgateway**.</span></span>    
   2. <span data-ttu-id="f4cf2-219">I den **connectionString**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-219">In the **connectionString**, do the following steps:</span></span>    

      1. <span data-ttu-id="f4cf2-220">För **servername**, ange namnet på servern där SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-220">For **servername**, enter the name of the server that hosts the SQL Server database.</span></span>
      2. <span data-ttu-id="f4cf2-221">För **databasename**, ange namnet på databasen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-221">For **databasename**, enter the name of the database.</span></span>
      3. <span data-ttu-id="f4cf2-222">Klicka på **kryptera** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-222">Click **Encrypt** button on the toolbar.</span></span> <span data-ttu-id="f4cf2-223">Du kan se programmet Autentiseringshanteraren.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-223">You see the Credentials Manager application.</span></span>

         ![Autentiseringsuppgifter Manager-program](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. <span data-ttu-id="f4cf2-225">I den **ställa in autentiseringsuppgifter** dialogrutan Ange autentiseringstypen, användarnamn och lösenord och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-225">In the **Setting Credentials** dialog box, specify authentication type, user name, and password, and click **OK**.</span></span> <span data-ttu-id="f4cf2-226">Om anslutningen lyckas krypterade autentiseringsuppgifter lagras i JSON och dialogrutan stängs.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-226">If the connection is successful, the encrypted credentials are stored in the JSON and the dialog box closes.</span></span>
      5. <span data-ttu-id="f4cf2-227">Stäng fliken tom webbläsare som startas i dialogrutan om det inte är stängd automatiskt och gå tillbaka till fliken med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-227">Close the empty browser tab that launched the dialog box if it is not automatically closed and get back to the tab with the Azure portal.</span></span>

         <span data-ttu-id="f4cf2-228">Gateway-datorn är autentiseringsuppgifterna **krypterade** genom att använda ett certifikat som äger Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-228">On the gateway machine, these credentials are **encrypted** by using a certificate that the Data Factory service owns.</span></span> <span data-ttu-id="f4cf2-229">Om du vill använda det certifikat som är associerad med Data Management Gateway i stället, se [ange autentiseringsuppgifterna på ett säkert sätt](#set-credentials-and-security).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-229">If you want to use the certificate that is associated with the Data Management Gateway instead, see [Set credentials securely](#set-credentials-and-security).</span></span>    
   3. <span data-ttu-id="f4cf2-230">Klicka på **distribuera** länkade tjänsten i kommandofältet SQL Server ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-230">Click **Deploy** on the command bar to deploy the SQL Server linked service.</span></span> <span data-ttu-id="f4cf2-231">Du bör se den länkade tjänsten i trädvyn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-231">You should see the linked service in the tree view.</span></span>

      ![SQL Server som är länkad tjänst i trädvyn](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="f4cf2-233">Lägg till en länkad tjänst för ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="f4cf2-233">Add a linked service for an Azure storage account</span></span>
1. <span data-ttu-id="f4cf2-234">I den **Data Factory-redigeraren**, klickar du på **Nytt datalager** i kommandofältet och på **Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-234">In the **Data Factory Editor**, click **New data store** on the command bar and click **Azure storage**.</span></span>
2. <span data-ttu-id="f4cf2-235">Ange namnet på ditt Azure storage-konto för den **kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-235">Enter the name of your Azure storage account for the **Account name**.</span></span>
3. <span data-ttu-id="f4cf2-236">Ange nyckel för Azure storage-konto för den **kontonyckel**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-236">Enter the key for your Azure storage account for the **Account key**.</span></span>
4. <span data-ttu-id="f4cf2-237">Klicka på **distribuera** att distribuera den **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-237">Click **Deploy** to deploy the **AzureStorageLinkedService**.</span></span>

## <a name="create-datasets"></a><span data-ttu-id="f4cf2-238">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="f4cf2-238">Create datasets</span></span>
<span data-ttu-id="f4cf2-239">I det här steget kan du skapa indata och utdata datauppsättningar som representerar inkommande och utgående data för kopieringen (lokal SQL Server-databas = > Azure blob storage).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-239">In this step, you create input and output datasets that represent input and output data for the copy operation (On-premises SQL Server database => Azure blob storage).</span></span> <span data-ttu-id="f4cf2-240">Innan du skapar datauppsättningar, gör du följande steg (detaljerade anvisningar följer listan):</span><span class="sxs-lookup"><span data-stu-id="f4cf2-240">Before creating datasets, do the following steps (detailed steps follows the list):</span></span>

* <span data-ttu-id="f4cf2-241">Skapa en tabell med namnet **tomma** i SQL Server-databasen du läggas till som en länkad tjänst till data factory och infogar ett par exempel poster i tabellen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-241">Create a table named **emp** in the SQL Server Database you added as a linked service to the data factory and insert a couple of sample entries into the table.</span></span>
* <span data-ttu-id="f4cf2-242">Skapa en blobbbehållare med namnet **adftutorial** i Azure blob storage-konto som du lagt till som en länkad tjänst till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-242">Create a blob container named **adftutorial** in the Azure blob storage account you added as a linked service to the data factory.</span></span>

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a><span data-ttu-id="f4cf2-243">Förbered på lokal SQL Server under kursen</span><span class="sxs-lookup"><span data-stu-id="f4cf2-243">Prepare On-premises SQL Server for the tutorial</span></span>
1. <span data-ttu-id="f4cf2-244">I databasen som du angav för lokalt SQL Server länkade tjänsten (**SqlServerLinkedService**), Använd följande SQL-skript för att skapa den **tomma** tabell i databasen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-244">In the database you specified for the on-premises SQL Server linked service (**SqlServerLinkedService**), use the following SQL script to create the **emp** table in the database.</span></span>

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
2. <span data-ttu-id="f4cf2-245">Infoga vissa exempel i tabellen:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-245">Insert some sample into the table:</span></span>

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a><span data-ttu-id="f4cf2-246">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="f4cf2-246">Create input dataset</span></span>

1. <span data-ttu-id="f4cf2-247">I **Data Factory-redigeraren**, klicka på **... Flera**, klickar du på **ny datamängd** i kommandofältet och på **SQL Server-tabellen**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-247">In the **Data Factory Editor**, click **... More**, click **New dataset** on the command bar, and click **SQL Server table**.</span></span>
2. <span data-ttu-id="f4cf2-248">Ersätt JSON i den högra rutan med följande text:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-248">Replace the JSON in the right pane with the following text:</span></span>

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
   <span data-ttu-id="f4cf2-249">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-249">Note the following points:</span></span>

   * <span data-ttu-id="f4cf2-250">**typen** är inställd på **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-250">**type** is set to **SqlServerTable**.</span></span>
   * <span data-ttu-id="f4cf2-251">**tableName** är inställd på **tomma**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-251">**tableName** is set to **emp**.</span></span>
   * <span data-ttu-id="f4cf2-252">**linkedServiceName** är inställd på **SqlServerLinkedService** (du har skapat den här länkade tjänsten tidigare i den här genomgången.).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-252">**linkedServiceName** is set to **SqlServerLinkedService** (you had created this linked service earlier in this walkthrough.).</span></span>
   * <span data-ttu-id="f4cf2-253">För en inkommande dataset som genereras av en annan pipeline i Azure Data Factory, måste du ange **externa** till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-253">For an input dataset that is not generated by another pipeline in Azure Data Factory, you must set **external** to **true**.</span></span> <span data-ttu-id="f4cf2-254">Den anger indata skapas utanför Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-254">It denotes the input data is produced external to the Azure Data Factory service.</span></span> <span data-ttu-id="f4cf2-255">Du kan också ange policyer externa data med hjälp av den **externalData** element i den **princip** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-255">You can optionally specify any external data policies using the **externalData** element in the **Policy** section.</span></span>    

   <span data-ttu-id="f4cf2-256">Se [flytta data till/från SQL Server](data-factory-sqlserver-connector.md) för ytterligare information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-256">See [Move data to/from SQL Server](data-factory-sqlserver-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="f4cf2-257">Klicka på **distribuera** i kommandofältet distribuera dataset.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-257">Click **Deploy** on the command bar to deploy the dataset.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="f4cf2-258">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="f4cf2-258">Create output dataset</span></span>

1. <span data-ttu-id="f4cf2-259">I den **Data Factory-redigeraren**, klickar du på **ny datamängd** i kommandofältet och på **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-259">In the **Data Factory Editor**, click **New dataset** on the command bar, and click **Azure Blob storage**.</span></span>
2. <span data-ttu-id="f4cf2-260">Ersätt JSON i den högra rutan med följande text:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-260">Replace the JSON in the right pane with the following text:</span></span>

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
   <span data-ttu-id="f4cf2-261">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-261">Note the following points:</span></span>

   * <span data-ttu-id="f4cf2-262">**typen** är inställd på **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-262">**type** is set to **AzureBlob**.</span></span>
   * <span data-ttu-id="f4cf2-263">**linkedServiceName** är inställd på **AzureStorageLinkedService** (du har skapat den här länkade tjänsten i steg 2).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-263">**linkedServiceName** is set to **AzureStorageLinkedService** (you had created this linked service in Step 2).</span></span>
   * <span data-ttu-id="f4cf2-264">**folderPath** är inställd på **adftutorial/outfromonpremdf** där outfromonpremdf är mappen i behållaren adftutorial.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-264">**folderPath** is set to **adftutorial/outfromonpremdf** where outfromonpremdf is the folder in the adftutorial container.</span></span> <span data-ttu-id="f4cf2-265">Skapa den **adftutorial** behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-265">Create the **adftutorial** container if it does not already exist.</span></span>
   * <span data-ttu-id="f4cf2-266">**Tillgängligheten** anges till **varje timme** (**frekvens** inställd på **timme** och **intervall** inställd på **1**).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-266">The **availability** is set to **hourly** (**frequency** set to **hour** and **interval** set to **1**).</span></span>  <span data-ttu-id="f4cf2-267">Data Factory-tjänsten genererar ett utdata datasektorn varje timme i den **tomma** tabell i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-267">The Data Factory service generates an output data slice every hour in the **emp** table in the Azure SQL Database.</span></span>

   <span data-ttu-id="f4cf2-268">Om du inte anger en **fileName** för en **utdatatabell**, genereras filer i den **folderPath** namnges i följande format: Data.<Guid>. txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-268">If you do not specify a **fileName** for an **output table**, the generated files in the **folderPath** are named in the following format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span></span>

   <span data-ttu-id="f4cf2-269">Ange **folderPath** och **fileName** dynamiskt baserat på de **SliceStart** tid, använder du egenskapen partitionedBy.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-269">To set **folderPath** and **fileName** dynamically based on the **SliceStart** time, use the partitionedBy property.</span></span> <span data-ttu-id="f4cf2-270">I följande exempel använder folderPath Year, Month och Day från SliceStart (starttiden för den sektor som bearbetas) och fileName använder Hour från SliceStart.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-270">In the following example, folderPath uses Year, Month, and Day from the SliceStart (start time of the slice being processed) and fileName uses Hour from the SliceStart.</span></span> <span data-ttu-id="f4cf2-271">Om exempelvis en sektor produceras 2014-10-20T08:00:00, anges folderName till wikidatagateway/wikisampledataout/2014/10/20 och fileName anges till 08.csv.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-271">For example, if a slice is being produced for 2014-10-20T08:00:00, the folderName is set to wikidatagateway/wikisampledataout/2014/10/20 and the fileName is set to 08.csv.</span></span>

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

    <span data-ttu-id="f4cf2-272">Se [flytta data till/från Azure Blob Storage](data-factory-azure-blob-connector.md) mer information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-272">See [Move data to/from Azure Blob Storage](data-factory-azure-blob-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="f4cf2-273">Klicka på **distribuera** i kommandofältet distribuera dataset.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-273">Click **Deploy** on the command bar to deploy the dataset.</span></span> <span data-ttu-id="f4cf2-274">Bekräfta att du ser både datauppsättningar i trädvyn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-274">Confirm that you see both the datasets in the tree view.</span></span>  

## <a name="create-pipeline"></a><span data-ttu-id="f4cf2-275">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="f4cf2-275">Create pipeline</span></span>
<span data-ttu-id="f4cf2-276">I det här steget skapar du en **pipeline** med en **Kopieringsaktiviteten** som använder **EmpOnPremSQLTable** som indata och **OutputBlobTable** som utdata.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-276">In this step, you create a **pipeline** with one **Copy Activity** that uses **EmpOnPremSQLTable** as input and **OutputBlobTable** as output.</span></span>

1. <span data-ttu-id="f4cf2-277">I Data Factory-redigeraren, klicka på **... More (Mer)** och sedan på **Ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-277">In Data Factory Editor, click **... More**, and click **New pipeline**.</span></span>
2. <span data-ttu-id="f4cf2-278">Ersätt JSON i den högra rutan med följande text:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-278">Replace the JSON in the right pane with the following text:</span></span>    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server to blob",
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
   > <span data-ttu-id="f4cf2-279">Ersätt värdet i **start**egenskapen med den aktuella dagen och **slut**värdet med nästa dag.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-279">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span>
   >
   >

   <span data-ttu-id="f4cf2-280">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-280">Note the following points:</span></span>

   * <span data-ttu-id="f4cf2-281">I avsnittet aktiviteter är bara aktivitet vars **typen** är inställd på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-281">In the activities section, there is only activity whose **type** is set to **Copy**.</span></span>
   * <span data-ttu-id="f4cf2-282">**Indata** för aktiviteten är inställd på **EmpOnPremSQLTable** och **utdata** för aktiviteten är inställd på **OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-282">**Input** for the activity is set to **EmpOnPremSQLTable** and **output** for the activity is set to **OutputBlobTable**.</span></span>
   * <span data-ttu-id="f4cf2-283">I den **typeProperties** avsnittet **SqlSource** har angetts som den **typ av datakälla** och ** BlobSink ** har angetts som den **sink typen**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-283">In the **typeProperties** section, **SqlSource** is specified as the **source type** and **BlobSink **is specified as the **sink type**.</span></span>
   * <span data-ttu-id="f4cf2-284">SQL-frågan `select * from emp` har angetts för den **sqlReaderQuery** -egenskapen för **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-284">SQL query `select * from emp` is specified for the **sqlReaderQuery** property of **SqlSource**.</span></span>

   <span data-ttu-id="f4cf2-285">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-285">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="f4cf2-286">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-286">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="f4cf2-287">**Sluttiden** är valfri, men vi använder den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-287">The **end** time is optional, but we use it in this tutorial.</span></span>

   <span data-ttu-id="f4cf2-288">Om du inte anger värdet för **slut**egenskapen, beräknas det som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-288">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="f4cf2-289">Om du vill köra pipelinen på obestämd tid, anger du **9/9/9999** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-289">To run the pipeline indefinitely, specify **9/9/9999** as the value for the **end** property.</span></span>

   <span data-ttu-id="f4cf2-290">Du definierar hur länge som datasektorer bearbetas baserat på de **tillgänglighet** egenskaper som definierats för varje Azure Data Factory-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-290">You are defining the time duration in which the data slices are processed based on the **Availability** properties that were defined for each Azure Data Factory dataset.</span></span>

   <span data-ttu-id="f4cf2-291">I exemplet finns det 24 datasektorer eftersom varje datasektor skapas varje timme.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-291">In the example, there are 24 data slices as each data slice is produced hourly.</span></span>        
3. <span data-ttu-id="f4cf2-292">Klicka på **distribuera** i kommandofältet distribuera datauppsättningen (tabellen är en rektangulär dataset).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-292">Click **Deploy** on the command bar to deploy the dataset (table is a rectangular dataset).</span></span> <span data-ttu-id="f4cf2-293">Bekräfta att pipeline visas i trädvyn under **Pipelines** nod.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-293">Confirm that the pipeline shows up in the tree view under **Pipelines** node.</span></span>  
4. <span data-ttu-id="f4cf2-294">Klicka på **X** två gånger för att stänga sidan om du vill gå tillbaka till den **Datafabriken** för den **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-294">Now, click **X** twice to close the page to get back to the **Data Factory** page for the **ADFTutorialOnPremDF**.</span></span>

<span data-ttu-id="f4cf2-295">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="f4cf2-295">**Congratulations!**</span></span> <span data-ttu-id="f4cf2-296">Du har skapat ett Azure data factory, länkade tjänster, datauppsättningar och en pipeline och schemalagda pipeline.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-296">You have successfully created an Azure data factory, linked services, datasets, and a pipeline and scheduled the pipeline.</span></span>

#### <a name="view-the-data-factory-in-a-diagram-view"></a><span data-ttu-id="f4cf2-297">Visa datafabriken i en diagramvy</span><span class="sxs-lookup"><span data-stu-id="f4cf2-297">View the data factory in a Diagram View</span></span>
1. <span data-ttu-id="f4cf2-298">I den **Azure-portalen**, klickar du på **Diagram** panelen på startsidan för den **ADFTutorialOnPremDF** datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-298">In the **Azure portal**, click **Diagram** tile on the home page for the **ADFTutorialOnPremDF** data factory.</span></span> <span data-ttu-id="f4cf2-299">:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-299">:</span></span>

    ![Diagram-länk](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. <span data-ttu-id="f4cf2-301">Du bör se ett diagram som liknar följande bild:</span><span class="sxs-lookup"><span data-stu-id="f4cf2-301">You should see the diagram similar to the following image:</span></span>

    ![Diagramvy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    <span data-ttu-id="f4cf2-303">Du kan zooma in, Zooma ut, Zooma till 100% zoomning att passa automatiskt placera rörledningar och datauppsättningar och visa härkomst information (markerar överordnade och underordnade objekt av valda objekt).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-303">You can zoom in, zoom out, zoom to 100%, zoom to fit, automatically position pipelines and datasets, and show lineage information (highlights upstream and downstream items of selected items).</span></span>  <span data-ttu-id="f4cf2-304">Du kan dubbelklicka på ett objekt (i/o-datauppsättningen eller pipeline) finns i egenskaperna för den.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-304">You can double-click an object (input/output dataset or pipeline) to see properties for it.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="f4cf2-305">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="f4cf2-305">Monitor pipeline</span></span>
<span data-ttu-id="f4cf2-306">I det här steget ska du använda Azure-portalen för att övervaka vad som händer i en Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-306">In this step, you use the Azure portal to monitor what’s going on in an Azure data factory.</span></span> <span data-ttu-id="f4cf2-307">Du kan också använda PowerShell-cmdletar för att övervaka datauppsättningar och pipelines.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-307">You can also use PowerShell cmdlets to monitor datasets and pipelines.</span></span> <span data-ttu-id="f4cf2-308">Mer information om övervakning finns [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-308">For details about monitoring, see [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md).</span></span>

1. <span data-ttu-id="f4cf2-309">I diagrammet, dubbelklickar du på **EmpOnPremSQLTable**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-309">In the diagram, double-click **EmpOnPremSQLTable**.</span></span>  

    ![EmpOnPremSQLTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. <span data-ttu-id="f4cf2-311">Lägg märke till att alla datasektorer in **klar** tillstånd eftersom pipeline varaktighet (starttiden till sluttiden) har passerats.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-311">Notice that all the data slices up are in **Ready** state because the pipeline duration (start time to end time) is in the past.</span></span> <span data-ttu-id="f4cf2-312">Det är också eftersom du har infogat data i SQL Server-databasen och det är det hela tiden.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-312">It is also because you have inserted the data in the SQL Server database and it is there all the time.</span></span> <span data-ttu-id="f4cf2-313">Bekräfta att ingen segment visas i den **problemet segment** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-313">Confirm that no slices show up in the **Problem slices** section at the bottom.</span></span> <span data-ttu-id="f4cf2-314">Om du vill visa alla segment, klickar du på **finns mer** längst ned i listan över segment.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-314">To view all the slices, click **See More** at the bottom of the list of slices.</span></span>
3. <span data-ttu-id="f4cf2-315">Nu i den **datauppsättningar** klickar du på **OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-315">Now, In the **Datasets** page, click **OutputBlobTable**.</span></span>

    ![OputputBlobTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. <span data-ttu-id="f4cf2-317">Klicka på några data-segment i listan och du bör se den **Datasektorn** sidan.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-317">Click any data slice from the list and you should see the **Data Slice** page.</span></span> <span data-ttu-id="f4cf2-318">Du kan se aktiviteten körs för sektorn.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-318">You see activity runs for the slice.</span></span> <span data-ttu-id="f4cf2-319">Endast en aktivitet körs vanligtvis visas.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-319">You see only one activity run usually.</span></span>  

    ![Data sektorn bladet](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    <span data-ttu-id="f4cf2-321">Om sektorn inte har statusen **Klar** kan du se sektorer uppströms som inte är klara och som blockerar aktuell sektor från att köras i listan **Sektorer uppströms som inte är klara**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-321">If the slice is not in the **Ready** state, you can see the upstream slices that are not Ready and are blocking the current slice from executing in the **Upstream slices that are not ready** list.</span></span>
5. <span data-ttu-id="f4cf2-322">Klicka på den **aktiviteten kör** i listrutan längst ned för att se **aktivitet köras information**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-322">Click the **activity run** from the list at the bottom to see **activity run details**.</span></span>

   ![Aktiviteten kör informationssidan](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   <span data-ttu-id="f4cf2-324">Visas information, till exempel dataflöde, varaktighet och den gateway som används för att överföra data.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-324">You would see information such as throughput, duration, and the gateway used to transfer the data.</span></span>
6. <span data-ttu-id="f4cf2-325">Klicka på **X** att stänga alla sidor förrän du</span><span class="sxs-lookup"><span data-stu-id="f4cf2-325">Click **X** to close all the pages until you</span></span>
7. <span data-ttu-id="f4cf2-326">Gå tillbaka till startsidan för den **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-326">get back to the home page for the **ADFTutorialOnPremDF**.</span></span>
8. <span data-ttu-id="f4cf2-327">(valfritt) Klicka på **Pipelines**, klickar du på **ADFTutorialOnPremDF**, och detaljvisning indatatabeller (**förbrukad**) eller utdata datauppsättningar (**framställda**).</span><span class="sxs-lookup"><span data-stu-id="f4cf2-327">(optional) Click **Pipelines**, click **ADFTutorialOnPremDF**, and drill through input tables (**Consumed**) or output datasets (**Produced**).</span></span>
9. <span data-ttu-id="f4cf2-328">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) att verifiera att en blob/fil skapas för varje timme.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-328">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to verify that a blob/file is created for each hour.</span></span>

   ![Azure Lagringsutforskaren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a><span data-ttu-id="f4cf2-330">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f4cf2-330">Next steps</span></span>
* <span data-ttu-id="f4cf2-331">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel detaljerad information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-331">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all the details about the Data Management Gateway.</span></span>
* <span data-ttu-id="f4cf2-332">Se [kopiera data från Azure Blob till Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) mer information om hur du använder Kopieringsaktiviteten för att flytta data från ett dataarkiv som källa till ett sink-datalager.</span><span class="sxs-lookup"><span data-stu-id="f4cf2-332">See [Copy data from Azure Blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) to learn about how to use Copy Activity to move data from a source data store to a sink data store.</span></span>
