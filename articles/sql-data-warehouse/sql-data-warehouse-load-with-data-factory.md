---
title: "aaaLoad data till Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "Den här kursen läser in data i Azure SQL Data Warehouse med hjälp av Azure Data Factory och använder en SQL Server-databas som hello datakälla."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="688cc-103">Läs in data till SQL Data Warehouse med Data Factory</span><span class="sxs-lookup"><span data-stu-id="688cc-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="688cc-104">Du kan använda Azure Data Factory tooload data till Azure SQL Data Warehouse från någon av hello [stöds källa datalager](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="688cc-104">You can use Azure Data Factory tooload data into Azure SQL Data Warehouse from any of hello [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="688cc-105">T.ex, du kan läsa in data från en Azure SQL-databas eller en Oracle-databas till en SQL data warehouse med hjälp av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="688cc-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="688cc-106">Kursen i den här artikeln visar hur tooload data från en lokal SQL Server-databas till en SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="688cc-106">Tutorial in this article shows you how tooload data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="688cc-107">**Tid uppskattning**: den här kursen tar 10 – 15 minuter toocomplete när hello krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="688cc-107">**Time estimate**: This tutorial takes about 10-15 minutes toocomplete once hello prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="688cc-108">Krav</span><span class="sxs-lookup"><span data-stu-id="688cc-108">Prerequisites</span></span>

- <span data-ttu-id="688cc-109">Du behöver en **SQL Server-databas** med tabeller som innehåller hello data kopieras toobe via toohello SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="688cc-109">You need a **SQL Server database** with tables that contain hello data toobe copied over toohello SQL data warehouse.</span></span>  

- <span data-ttu-id="688cc-110">Du behöver ett online **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="688cc-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="688cc-111">Om du inte redan har ett data warehouse lär du dig hur för[skapar ett Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="688cc-111">If you do not already have a data warehouse, learn how too[Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="688cc-112">Du behöver ett **Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="688cc-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="688cc-113">Om du inte redan har ett lagringskonto, lär du dig hur för[skapa ett lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="688cc-113">If you do not already have a storage account, learn how too[Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="688cc-114">Leta upp hello storage-konto för bästa prestanda och hello datalagret i hello samma Azure-region.</span><span class="sxs-lookup"><span data-stu-id="688cc-114">For best performance, locate hello storage account and hello data warehouse in hello same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="688cc-115">Konfigurera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="688cc-115">Configure a data factory</span></span>
1. <span data-ttu-id="688cc-116">Logga in toohello [Azure-portalen][].</span><span class="sxs-lookup"><span data-stu-id="688cc-116">Log in toohello [Azure portal][].</span></span>
2. <span data-ttu-id="688cc-117">Leta upp ditt data warehouse och på tooopen den.</span><span class="sxs-lookup"><span data-stu-id="688cc-117">Locate your data warehouse and click tooopen it.</span></span>
3. <span data-ttu-id="688cc-118">I hello huvudblad, klickar du på **Läs in Data** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="688cc-118">In hello main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Starta guiden Läs in Data](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="688cc-120">Om du inte har en datafabrik i din Azure-prenumeration visas en **nya Data Factory** dialogrutan i en separat flik i webbläsaren hello.</span><span class="sxs-lookup"><span data-stu-id="688cc-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of hello browser.</span></span> <span data-ttu-id="688cc-121">Fyll i hello information som efterfrågas och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-121">Fill in hello requested information, and click **Create**.</span></span> <span data-ttu-id="688cc-122">När du har skapat hello data factory hello **nya Data Factory** dialogrutan stängs och du ser hello **Välj Data Factory** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="688cc-122">After hello data factory is created, hello **New Data Factory** dialog box closes, and you see hello **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="688cc-123">Om du har en eller flera datafabriker redan i hello Azure-prenumeration kan du se hello **Välj Data Factory** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="688cc-123">If you have one or more data factories already in hello Azure subscription, you see hello **Select Data Factory** dialog box.</span></span> <span data-ttu-id="688cc-124">I den här dialogrutan kan du antingen välja en befintlig datafabrik eller klicka på **skapa nya data factory** toocreate en ny.</span><span class="sxs-lookup"><span data-stu-id="688cc-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** toocreate a new one.</span></span>

    ![Konfigurera Data Factory](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="688cc-126">I hello **Välj Data Factory** dialogrutan, hello **läsa in data** alternativet är markerat som standard.</span><span class="sxs-lookup"><span data-stu-id="688cc-126">In hello **Select Data Factory** dialog box, hello **Load data** option is selected by default.</span></span> <span data-ttu-id="688cc-127">Klicka på **nästa** toostart att skapa en aktivitet för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="688cc-127">Click **Next** toostart creating a data loading task.</span></span>

## <a name="configure-hello-data-factory-properties"></a><span data-ttu-id="688cc-128">Konfigurera egenskaper för hello data factory</span><span class="sxs-lookup"><span data-stu-id="688cc-128">Configure hello data factory properties</span></span>
<span data-ttu-id="688cc-129">Nu när du har skapat en datafabrik är hello nästa steg tooconfigure hello datainläsning schema.</span><span class="sxs-lookup"><span data-stu-id="688cc-129">Now that you have created a data factory, hello next step is tooconfigure hello data loading schedule.</span></span>

1. <span data-ttu-id="688cc-130">För **aktivitet**, ange **DWLoadData fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="688cc-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="688cc-131">Använd hello standard **kör nu en gång** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-131">Use hello default **Run once now** option, click **Next**.</span></span>

    ![Konfigurera belastningen schema](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a><span data-ttu-id="688cc-133">Konfigurera datalager för hello källa och gateway</span><span class="sxs-lookup"><span data-stu-id="688cc-133">Configure hello source data store and gateway</span></span>
<span data-ttu-id="688cc-134">Nu kan du se Data Factory om hello lokala SQL Server-databas som du vill tooload data.</span><span class="sxs-lookup"><span data-stu-id="688cc-134">Now you tell Data Factory about hello on-premises SQL Server database from which you want tooload data.</span></span>

1. <span data-ttu-id="688cc-135">Välj **SQL Server** från hello stöds källdata lagra katalog och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-135">Choose **SQL Server** from hello supported source data store catalog, and click **Next**.</span></span>

    ![Välj källa för SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="688cc-137">En **ange hello lokala SQL Server-databas** visas.</span><span class="sxs-lookup"><span data-stu-id="688cc-137">A **Specify hello on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="688cc-138">Hej först **anslutningsnamn** är fältet fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="688cc-138">hello first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="688cc-139">hello andra fältet Begär hello namnet på hello **Gateway**.</span><span class="sxs-lookup"><span data-stu-id="688cc-139">hello second field asks for hello name of hello **Gateway**.</span></span> <span data-ttu-id="688cc-140">Om du använder en befintlig datafabrik som redan har en gateway kan du återanvända hello gateway genom att välja den hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="688cc-140">If you are using an existing data factory that already has a gateway, you can reuse hello gateway by selecting it from hello drop-down list.</span></span> <span data-ttu-id="688cc-141">Klicka på hello **skapa Gateway** länka toocreate en Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="688cc-141">Click hello **Create Gateway** link toocreate a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="688cc-142">Om hello källdata lagrar lokalt eller i en virtuell Azure IaaS-dator, en Data Management Gateway måste anges.</span><span class="sxs-lookup"><span data-stu-id="688cc-142">If hello source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="688cc-143">En gateway har en 1-1-relation med en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="688cc-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="688cc-144">Det går inte att använda från en annan data factory, men den kan användas av flera uppgifter med i hello för datainläsning samma data factory.</span><span class="sxs-lookup"><span data-stu-id="688cc-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in hello same data factory.</span></span> <span data-ttu-id="688cc-145">En gateway kan vara används tooconnect toomultiple datalager när du kör uppgifter för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="688cc-145">A gateway can be used tooconnect toomultiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="688cc-146">Detaljerad information om hello gateway finns [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="688cc-146">For detailed information about hello gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="688cc-147">En **skapa Gateway** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="688cc-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="688cc-148">Namn, ange **GatewayForDWLoading**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="688cc-149">En **konfigurera Gateway** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="688cc-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="688cc-150">Klicka på **starta Expressinstallationen på den här datorn** tooautomatically hämta, installera och registrera Data Management Gateway på den aktuella datorn.</span><span class="sxs-lookup"><span data-stu-id="688cc-150">Click **Launch express setup on this computer** tooautomatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="688cc-151">hello förloppet visas i ett popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="688cc-151">hello progress is shown in a pop-up window.</span></span> <span data-ttu-id="688cc-152">Om hello datorn inte kan ansluta toohello datalagret, kan du manuellt [ladda ned och installera hello gateway](https://www.microsoft.com/download/details.aspx?id=39717) lagrar på en dator som kan ansluta toohello data och sedan använder hello viktiga tooregister.</span><span class="sxs-lookup"><span data-stu-id="688cc-152">If hello machine cannot connect toohello data store, you can manually [download and install hello gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect toohello data store, and then use hello key tooregister.</span></span>
    > [!NOTE]
    > <span data-ttu-id="688cc-153">hello Expressinstallationen fungerar internt med Microsoft Edge och Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="688cc-153">hello express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="688cc-154">Om du använder Google Chrome, måste du först installera hello ClickOnce-tillägget från Chrome web store.</span><span class="sxs-lookup"><span data-stu-id="688cc-154">If you are using Google Chrome, first install hello ClickOnce extension from Chrome web store.</span></span>

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="688cc-156">Vänta tills hello gateway installationsprogrammet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="688cc-156">Wait for hello gateway setup toocomplete.</span></span> <span data-ttu-id="688cc-157">När hello gatewayen har registrerats och är online, hello popup-fönstret stängs och hello ny gateway visas i fältet för hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="688cc-157">Once hello gateway is successfully registered and is online, hello pop-up window closes and hello new gateway appears in hello gateway field.</span></span> <span data-ttu-id="688cc-158">Sedan fyller du i hello rest obligatoriska fält enligt följande och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-158">Then fill in hello rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="688cc-159">**Servernamnet**: namnet på hello lokala SQL Server.</span><span class="sxs-lookup"><span data-stu-id="688cc-159">**Server name**: Name of hello on-premises SQL Server.</span></span>
    - <span data-ttu-id="688cc-160">**Databasnamnet**: SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="688cc-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="688cc-161">**Autentiseringsuppgifter kryptering**: Använd hello standard ”av webbläsaren”.</span><span class="sxs-lookup"><span data-stu-id="688cc-161">**Credential encryption**: Use hello default "By web browser".</span></span>
    - <span data-ttu-id="688cc-162">**Autentiseringstypen**: Välj hello typ av autentisering som du använder.</span><span class="sxs-lookup"><span data-stu-id="688cc-162">**Authentication type**: Choose hello type of authentication you are using.</span></span>
    - <span data-ttu-id="688cc-163">**Användarnamnet** och **lösenord**: Ange hello användarnamn och lösenord för en användare som har behörigheten toocopy hello data.</span><span class="sxs-lookup"><span data-stu-id="688cc-163">**User name** and **password**: Enter hello user name and password for a user who has permission toocopy hello data.</span></span>

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="688cc-165">hello nästa steg är toochoose hello tabeller från vilka toocopy hello data.</span><span class="sxs-lookup"><span data-stu-id="688cc-165">hello next step is toochoose hello tables from which toocopy hello data.</span></span> <span data-ttu-id="688cc-166">Du kan filtrera hello tabeller med hjälp av nyckelord.</span><span class="sxs-lookup"><span data-stu-id="688cc-166">You can filter hello tables by using keywords.</span></span> <span data-ttu-id="688cc-167">Och du kan förhandsgranska hello data och tabellen schemat i hello nedre panelen.</span><span class="sxs-lookup"><span data-stu-id="688cc-167">And you can preview hello data and table schema in hello bottom panel.</span></span> <span data-ttu-id="688cc-168">När du slutför ditt val klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-168">After you finish your selection, click **Next**.</span></span>

    ![Välj tabeller](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a><span data-ttu-id="688cc-170">Konfigurera hello mål, ditt SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="688cc-170">Configure hello destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="688cc-171">Nu kan du se Data Factory om hello målinformation.</span><span class="sxs-lookup"><span data-stu-id="688cc-171">Now you tell Data Factory about hello destination information.</span></span>

1. <span data-ttu-id="688cc-172">SQL Data Warehouse anslutningsinformationen fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="688cc-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="688cc-173">Ange hello lösenord för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="688cc-173">Enter hello password for hello user name.</span></span> <span data-ttu-id="688cc-174">och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-174">and click **Next**.</span></span>

    ![Konfigurera mål](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="688cc-176">Mappningen för en intelligent tabellen visas som mappar toodestination källtabellerna baserat på namn.</span><span class="sxs-lookup"><span data-stu-id="688cc-176">An intelligent table mapping appears that maps source toodestination tables based on table names.</span></span> <span data-ttu-id="688cc-177">Om hello tabellen inte finns i hello målet, som standard ADF skapar en med hello samma namn (Detta gäller tooSQL Server eller Azure SQL Database som källa).</span><span class="sxs-lookup"><span data-stu-id="688cc-177">If hello table does not exist in hello destination, by default ADF will create one with hello same name (this applies tooSQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="688cc-178">Du kan också välja toomap tooan befintlig tabell.</span><span class="sxs-lookup"><span data-stu-id="688cc-178">You can also choose toomap tooan existing table.</span></span> <span data-ttu-id="688cc-179">Granska och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-179">Review and click **Next**.</span></span>

    ![Mappa tabeller](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="688cc-181">Granska hello schemamappning och leta efter fel eller varningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="688cc-181">Review hello schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="688cc-182">Intelligent mappningen baserat på kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="688cc-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="688cc-183">Om det finns en typkonvertering för data som inte stöds mellan hello käll- och kolumnen, visas ett felmeddelande om du tillsammans med motsvarande hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="688cc-183">If there is an unsupported data type conversion between hello source and destination column, you see an error message alongside hello corresponding table.</span></span> <span data-ttu-id="688cc-184">Om du väljer toolet Data Factory automatiskt skapa hello tabeller, rätt datatypkonvertering kan inträffa om det behövs toofix hello inkompatibilitet mellan käll- och Arkiv.</span><span class="sxs-lookup"><span data-stu-id="688cc-184">If you choose toolet Data Factory auto create hello tables, proper data type conversion may happen if needed toofix hello incompatibility between source and destination stores.</span></span>

    ![Mappa schema](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="688cc-186">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-186">Click **Next**.</span></span>

## <a name="configure-hello-performance-settings"></a><span data-ttu-id="688cc-187">Konfigurera hello prestandainställningar</span><span class="sxs-lookup"><span data-stu-id="688cc-187">Configure hello performance settings</span></span>
<span data-ttu-id="688cc-188">Hello prestandakonfigurationer kan du konfigurera ett Azure storage-konto som används för mellanlagring hello data innan den läses in i SQL Data Warehouse performantly med [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="688cc-188">In hello Performance configurations, you configure an Azure storage account used for staging hello data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="688cc-189">Därefter kopiera hello rensas hello mellanliggande data i lagring automatiskt.</span><span class="sxs-lookup"><span data-stu-id="688cc-189">After hello copy is done, hello interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="688cc-190">Välj ett befintligt Azure Storage-konto och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="688cc-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Konfigurera fristående blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a><span data-ttu-id="688cc-192">Granska sammanfattningsinformation och distribuera hello pipeline</span><span class="sxs-lookup"><span data-stu-id="688cc-192">Review summary information and deploy hello pipeline</span></span>

<span data-ttu-id="688cc-193">Granska hello konfigurationen och klicka på **Slutför** knappen toodeploy hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="688cc-193">Review hello configuration and click **Finish** button toodeploy hello pipeline.</span></span>

![Distribuera data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="688cc-195">Övervaka förloppet för datainläsning</span><span class="sxs-lookup"><span data-stu-id="688cc-195">Monitor data loading progress</span></span>

<span data-ttu-id="688cc-196">Du kan se hello förlopp och resultat i hello **distribution** sidan.</span><span class="sxs-lookup"><span data-stu-id="688cc-196">You can see hello deployment progress and results in hello **Deployment** page.</span></span>

1. <span data-ttu-id="688cc-197">När hello distributionen är klar klickar du på hello länken **Klicka här toomonitor kopiera pipeline** toomonitor datainläsning pågår.</span><span class="sxs-lookup"><span data-stu-id="688cc-197">Once hello deployment is done, click hello link that says **Click here toomonitor copy pipeline** toomonitor data loading progress.</span></span>

    ![Övervaka pipeline](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="688cc-199">hello nyskapad **DWLoadData fromSQLServer** pipeline för inläsning av data är automatisk från hello vänstra **Resursläsaren**.</span><span class="sxs-lookup"><span data-stu-id="688cc-199">hello newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from hello left-hand **Resource Explorer**.</span></span>

    ![Visa pipeline](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="688cc-201">Klicka till hello pipeline hello mitten panelen toosee hello detaljerad status för varje tabell som mappar tooan aktivitet.</span><span class="sxs-lookup"><span data-stu-id="688cc-201">Click into hello pipeline in hello middle panel toosee hello detailed status for each table that maps tooan Activity.</span></span>

    ![Visa tabellen aktivitet](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="688cc-203">Ytterligare Klicka i en aktivitet och du ser hello datainläsning information i hello högra panelen, inklusive datastorleken, rader, genomflöde och så vidare.</span><span class="sxs-lookup"><span data-stu-id="688cc-203">Further click into an activity and you see hello data loading details in hello right panel including data size, rows, throughput, etc.</span></span>

    ![Visa tabellen Aktivitetsinformation](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="688cc-205">toolaunch denna övervakning visa senare, gå tooyour SQL Data Warehouse, klicka på **Läs in Data > Azure Data Factory**, Välj din factory och välj **övervaka befintliga läser in aktiviteter**.</span><span class="sxs-lookup"><span data-stu-id="688cc-205">toolaunch this monitoring view later, go tooyour SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="688cc-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="688cc-206">Next steps</span></span>

<span data-ttu-id="688cc-207">toomigrate din databas tooSQL datalagret, se [migreringsöversikt](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="688cc-207">toomigrate your database tooSQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="688cc-208">toolearn mer om Azure Data Factory och dess funktioner för flytt av data, se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="688cc-208">toolearn more about Azure Data Factory and its data movement capabilities, see hello following articles:</span></span>

- [<span data-ttu-id="688cc-209">Introduktion tooAzure Data Factory</span><span class="sxs-lookup"><span data-stu-id="688cc-209">Introduction tooAzure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="688cc-210">Flytta data med hjälp av Kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="688cc-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="688cc-211">Flytta data tooand från Azure SQL Data Warehouse med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="688cc-211">Move data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="688cc-212">tooexplore dina data i SQL Data Warehouse finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="688cc-212">tooexplore your data in SQL Data Warehouse, see hello following articles:</span></span>

- [<span data-ttu-id="688cc-213">Anslut tooSQL Data Warehouse med Visual Studio och SSDT</span><span class="sxs-lookup"><span data-stu-id="688cc-213">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="688cc-214">[Visuella data med Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="688cc-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[Azure-portalen]: https://portal.azure.com
