---
title: "Läs in data till Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "Den här kursen läser in data i Azure SQL Data Warehouse med hjälp av Azure Data Factory och använder en SQL Server-databas som datakälla."
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
ms.openlocfilehash: 12a35213e07ff16bdc1c27be106792bcc032ac80
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="99d2e-103">Läs in data till SQL Data Warehouse med Data Factory</span><span class="sxs-lookup"><span data-stu-id="99d2e-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="99d2e-104">Du kan använda Azure Data Factory för att läsa in data till Azure SQL Data Warehouse från någon av de [stöds källa datalager](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="99d2e-104">You can use Azure Data Factory to load data into Azure SQL Data Warehouse from any of the [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="99d2e-105">T.ex, du kan läsa in data från en Azure SQL-databas eller en Oracle-databas till en SQL data warehouse med hjälp av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="99d2e-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="99d2e-106">Kursen i den här artikeln visar hur du läser in data från en lokal SQL Server-databas till en SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="99d2e-106">Tutorial in this article shows you how to load data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="99d2e-107">**Tid uppskattning**: den här självstudiekursen tar cirka 10 – 15 minuter för att slutföra när alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="99d2e-107">**Time estimate**: This tutorial takes about 10-15 minutes to complete once the prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99d2e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="99d2e-108">Prerequisites</span></span>

- <span data-ttu-id="99d2e-109">Du behöver en **SQL Server-databas** med tabeller som innehåller data som ska kopieras över till SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="99d2e-109">You need a **SQL Server database** with tables that contain the data to be copied over to the SQL data warehouse.</span></span>  

- <span data-ttu-id="99d2e-110">Du behöver ett online **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="99d2e-111">Om du inte redan har ett data warehouse, lär du dig hur du [skapar ett Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="99d2e-111">If you do not already have a data warehouse, learn how to [Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="99d2e-112">Du behöver ett **Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="99d2e-113">Om du inte redan har ett lagringskonto, lär du dig hur du [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="99d2e-113">If you do not already have a storage account, learn how to [Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="99d2e-114">Leta upp storage-konto och datalagret i samma Azure-region för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="99d2e-114">For best performance, locate the storage account and the data warehouse in the same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="99d2e-115">Konfigurera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="99d2e-115">Configure a data factory</span></span>
1. <span data-ttu-id="99d2e-116">Logga in på [Azure-portalen][].</span><span class="sxs-lookup"><span data-stu-id="99d2e-116">Log in to the [Azure portal][].</span></span>
2. <span data-ttu-id="99d2e-117">Leta upp ditt data warehouse och klicka på för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="99d2e-117">Locate your data warehouse and click to open it.</span></span>
3. <span data-ttu-id="99d2e-118">I bladet huvudsakliga klickar du på **Läs in Data** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-118">In the main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Starta guiden Läs in Data](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="99d2e-120">Om du inte har en datafabrik i din Azure-prenumeration visas en **nya Data Factory** dialogrutan i en separat flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="99d2e-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of the browser.</span></span> <span data-ttu-id="99d2e-121">Fyll i den begärda informationen och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-121">Fill in the requested information, and click **Create**.</span></span> <span data-ttu-id="99d2e-122">När datafabriken har skapats kan den **nya Data Factory** dialogrutan stängs och du ser den **Välj Data Factory** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99d2e-122">After the data factory is created, the **New Data Factory** dialog box closes, and you see the **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="99d2e-123">Om du har en eller flera datafabriker redan i Azure-prenumeration kan du se den **Välj Data Factory** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99d2e-123">If you have one or more data factories already in the Azure subscription, you see the **Select Data Factory** dialog box.</span></span> <span data-ttu-id="99d2e-124">I den här dialogrutan kan du antingen välja en befintlig datafabrik eller klicka på **skapa nya data factory** att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="99d2e-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** to create a new one.</span></span>

    ![Konfigurera Data Factory](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="99d2e-126">I den **Välj Data Factory** dialogrutan den **läsa in data** alternativet är markerat som standard.</span><span class="sxs-lookup"><span data-stu-id="99d2e-126">In the **Select Data Factory** dialog box, the **Load data** option is selected by default.</span></span> <span data-ttu-id="99d2e-127">Klicka på **nästa** börja skapa en aktivitet för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="99d2e-127">Click **Next** to start creating a data loading task.</span></span>

## <a name="configure-the-data-factory-properties"></a><span data-ttu-id="99d2e-128">Konfigurera egenskaper för data factory</span><span class="sxs-lookup"><span data-stu-id="99d2e-128">Configure the data factory properties</span></span>
<span data-ttu-id="99d2e-129">Nu när du har skapat en datafabrik, är nästa steg att konfigurera schemat för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="99d2e-129">Now that you have created a data factory, the next step is to configure the data loading schedule.</span></span>

1. <span data-ttu-id="99d2e-130">För **aktivitet**, ange **DWLoadData fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="99d2e-131">Använd standard **kör nu en gång** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-131">Use the default **Run once now** option, click **Next**.</span></span>

    ![Konfigurera belastningen schema](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-source-data-store-and-gateway"></a><span data-ttu-id="99d2e-133">Konfigurera datalager för källa och gateway</span><span class="sxs-lookup"><span data-stu-id="99d2e-133">Configure the source data store and gateway</span></span>
<span data-ttu-id="99d2e-134">Nu kan du se Data Factory om lokala SQL Server-databasen som du vill läsa in data.</span><span class="sxs-lookup"><span data-stu-id="99d2e-134">Now you tell Data Factory about the on-premises SQL Server database from which you want to load data.</span></span>

1. <span data-ttu-id="99d2e-135">Välj **SQL Server** stöds källdata lagra katalog och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-135">Choose **SQL Server** from the supported source data store catalog, and click **Next**.</span></span>

    ![Välj källa för SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="99d2e-137">En **ange SQL Server-databas lokalt** visas.</span><span class="sxs-lookup"><span data-stu-id="99d2e-137">A **Specify the on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="99d2e-138">Först **anslutningsnamn** är fältet fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="99d2e-138">The first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="99d2e-139">Det andra fältet uppmanas du att ange namnet på den **Gateway**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-139">The second field asks for the name of the **Gateway**.</span></span> <span data-ttu-id="99d2e-140">Om du använder en befintlig datafabrik som redan har en gateway kan du återanvända gatewayen genom att välja den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="99d2e-140">If you are using an existing data factory that already has a gateway, you can reuse the gateway by selecting it from the drop-down list.</span></span> <span data-ttu-id="99d2e-141">Klicka på den **skapa Gateway** länken för att skapa en Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="99d2e-141">Click the **Create Gateway** link to create a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="99d2e-142">Om käll-datalagret är lokalt eller i en virtuell Azure IaaS-dator, en Data Management Gateway måste anges.</span><span class="sxs-lookup"><span data-stu-id="99d2e-142">If the source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="99d2e-143">En gateway har en 1-1-relation med en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="99d2e-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="99d2e-144">Det går inte att använda från en annan data factory, men den kan användas av flera uppgifter med i samma data factory för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="99d2e-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in the same data factory.</span></span> <span data-ttu-id="99d2e-145">En gateway kan användas för att ansluta till flera datalager om uppgifter för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="99d2e-145">A gateway can be used to connect to multiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="99d2e-146">Detaljerad information om gateway finns [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="99d2e-146">For detailed information about the gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="99d2e-147">En **skapa Gateway** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="99d2e-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="99d2e-148">Namn, ange **GatewayForDWLoading**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="99d2e-149">En **konfigurera Gateway** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="99d2e-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="99d2e-150">Klicka på **starta Expressinstallationen på den här datorn** för att automatiskt hämta, installera och registrera Data Management Gateway på den aktuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99d2e-150">Click **Launch express setup on this computer** to automatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="99d2e-151">Förloppet visas i ett popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="99d2e-151">The progress is shown in a pop-up window.</span></span> <span data-ttu-id="99d2e-152">Om datorn inte kan ansluta till datalagret kan du manuellt [ladda ned och installera gatewayen](https://www.microsoft.com/download/details.aspx?id=39717) lagra på en dator som kan ansluta till data och använder sedan för att registrera.</span><span class="sxs-lookup"><span data-stu-id="99d2e-152">If the machine cannot connect to the data store, you can manually [download and install the gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect to the data store, and then use the key to register.</span></span>
    > [!NOTE]
    > <span data-ttu-id="99d2e-153">Expressinstallation fungerar internt med Microsoft Edge och Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="99d2e-153">The express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="99d2e-154">Om du använder Google Chrome, måste du först installera tillägget ClickOnce från Chrome web store.</span><span class="sxs-lookup"><span data-stu-id="99d2e-154">If you are using Google Chrome, first install the ClickOnce extension from Chrome web store.</span></span>

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="99d2e-156">Vänta tills gateway-installationen ska kunna slutföras.</span><span class="sxs-lookup"><span data-stu-id="99d2e-156">Wait for the gateway setup to complete.</span></span> <span data-ttu-id="99d2e-157">När gatewayen har registrerats och är online, popup-fönstret stängs och den nya gatewayen visas i fältet för gateway.</span><span class="sxs-lookup"><span data-stu-id="99d2e-157">Once the gateway is successfully registered and is online, the pop-up window closes and the new gateway appears in the gateway field.</span></span> <span data-ttu-id="99d2e-158">Sedan fylla i resten obligatoriska fält enligt följande och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-158">Then fill in the rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="99d2e-159">**Servernamnet**: namnet på den lokala SQL Server.</span><span class="sxs-lookup"><span data-stu-id="99d2e-159">**Server name**: Name of the on-premises SQL Server.</span></span>
    - <span data-ttu-id="99d2e-160">**Databasnamnet**: SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="99d2e-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="99d2e-161">**Autentiseringsuppgifter kryptering**: använder standard ”av webbläsaren”.</span><span class="sxs-lookup"><span data-stu-id="99d2e-161">**Credential encryption**: Use the default "By web browser".</span></span>
    - <span data-ttu-id="99d2e-162">**Autentiseringstypen**: Välj typ av autentisering som du använder.</span><span class="sxs-lookup"><span data-stu-id="99d2e-162">**Authentication type**: Choose the type of authentication you are using.</span></span>
    - <span data-ttu-id="99d2e-163">**Användarnamnet** och **lösenord**: Ange användarnamn och lösenord för en användare som har behörighet att kopiera data.</span><span class="sxs-lookup"><span data-stu-id="99d2e-163">**User name** and **password**: Enter the user name and password for a user who has permission to copy the data.</span></span>

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="99d2e-165">Nästa steg är att välja tabeller som du vill kopiera data från.</span><span class="sxs-lookup"><span data-stu-id="99d2e-165">The next step is to choose the tables from which to copy the data.</span></span> <span data-ttu-id="99d2e-166">Du kan filtrera tabeller med hjälp av nyckelord.</span><span class="sxs-lookup"><span data-stu-id="99d2e-166">You can filter the tables by using keywords.</span></span> <span data-ttu-id="99d2e-167">Och du kan förhandsgranska data och tabellen schema i den nedre panelen.</span><span class="sxs-lookup"><span data-stu-id="99d2e-167">And you can preview the data and table schema in the bottom panel.</span></span> <span data-ttu-id="99d2e-168">När du slutför ditt val klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-168">After you finish your selection, click **Next**.</span></span>

    ![Välj tabeller](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a><span data-ttu-id="99d2e-170">Konfigurera mål, ditt SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="99d2e-170">Configure the destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="99d2e-171">Nu kan du se Data Factory om informationen som mål.</span><span class="sxs-lookup"><span data-stu-id="99d2e-171">Now you tell Data Factory about the destination information.</span></span>

1. <span data-ttu-id="99d2e-172">SQL Data Warehouse anslutningsinformationen fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="99d2e-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="99d2e-173">Ange lösenordet för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="99d2e-173">Enter the password for the user name.</span></span> <span data-ttu-id="99d2e-174">och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-174">and click **Next**.</span></span>

    ![Konfigurera mål](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="99d2e-176">Mappningen för en intelligent tabellen visas maps källa måltabell baserat på namn.</span><span class="sxs-lookup"><span data-stu-id="99d2e-176">An intelligent table mapping appears that maps source to destination tables based on table names.</span></span> <span data-ttu-id="99d2e-177">Om tabellen inte finns i målet, som standard skapar ADF en med samma namn (Detta gäller för SQL Server eller Azure SQL Database som källa).</span><span class="sxs-lookup"><span data-stu-id="99d2e-177">If the table does not exist in the destination, by default ADF will create one with the same name (this applies to SQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="99d2e-178">Du kan också välja att mappa till en befintlig tabell.</span><span class="sxs-lookup"><span data-stu-id="99d2e-178">You can also choose to map to an existing table.</span></span> <span data-ttu-id="99d2e-179">Granska och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-179">Review and click **Next**.</span></span>

    ![Mappa tabeller](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="99d2e-181">Granska mappningen schemat och leta efter fel eller varningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="99d2e-181">Review the schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="99d2e-182">Intelligent mappningen baserat på kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="99d2e-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="99d2e-183">Om det finns en typkonvertering för data som inte stöds mellan käll- och kolumnen, visas ett felmeddelande om du tillsammans med motsvarande register.</span><span class="sxs-lookup"><span data-stu-id="99d2e-183">If there is an unsupported data type conversion between the source and destination column, you see an error message alongside the corresponding table.</span></span> <span data-ttu-id="99d2e-184">Om du väljer att låta Data Factory automatiskt skapa tabellerna kan rätt datatypkonvertering inträffa om du behöver åtgärda inkompatibilitet mellan käll- och lagrar.</span><span class="sxs-lookup"><span data-stu-id="99d2e-184">If you choose to let Data Factory auto create the tables, proper data type conversion may happen if needed to fix the incompatibility between source and destination stores.</span></span>

    ![Mappa schema](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="99d2e-186">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-186">Click **Next**.</span></span>

## <a name="configure-the-performance-settings"></a><span data-ttu-id="99d2e-187">Konfigurera prestandainställningar för</span><span class="sxs-lookup"><span data-stu-id="99d2e-187">Configure the performance settings</span></span>
<span data-ttu-id="99d2e-188">Prestanda-konfigurationer som du konfigurerar ett Azure storage-konto som används för mellanlagring av data innan den läses in i SQL Data Warehouse performantly med [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="99d2e-188">In the Performance configurations, you configure an Azure storage account used for staging the data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="99d2e-189">När kopieringen är klar rensas mellanliggande data i lagring automatiskt.</span><span class="sxs-lookup"><span data-stu-id="99d2e-189">After the copy is done, the interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="99d2e-190">Välj ett befintligt Azure Storage-konto och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Konfigurera fristående blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a><span data-ttu-id="99d2e-192">Granska sammanfattningsinformation och distribuera pipelinen</span><span class="sxs-lookup"><span data-stu-id="99d2e-192">Review summary information and deploy the pipeline</span></span>

<span data-ttu-id="99d2e-193">Granska konfigurationen och klickar på **Slutför** för att distribuera pipelinen.</span><span class="sxs-lookup"><span data-stu-id="99d2e-193">Review the configuration and click **Finish** button to deploy the pipeline.</span></span>

![Distribuera data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="99d2e-195">Övervaka förloppet för datainläsning</span><span class="sxs-lookup"><span data-stu-id="99d2e-195">Monitor data loading progress</span></span>

<span data-ttu-id="99d2e-196">Du kan se förlopp och resultat i den **distribution** sidan.</span><span class="sxs-lookup"><span data-stu-id="99d2e-196">You can see the deployment progress and results in the **Deployment** page.</span></span>

1. <span data-ttu-id="99d2e-197">När distributionen är klar klickar du på länken **Klicka här för att övervaka kopiera pipeline** att övervaka förloppet för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="99d2e-197">Once the deployment is done, click the link that says **Click here to monitor copy pipeline** to monitor data loading progress.</span></span>

    ![Övervaka pipeline](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="99d2e-199">Den nyligen skapade **DWLoadData fromSQLServer** pipeline för inläsning av data är automatisk för från den vänstra **Resursläsaren**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-199">The newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from the left-hand **Resource Explorer**.</span></span>

    ![Visa pipeline](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="99d2e-201">Klicka i pipeline i den mittersta panelen om du vill se detaljerad status för varje tabell som mappar till en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="99d2e-201">Click into the pipeline in the middle panel to see the detailed status for each table that maps to an Activity.</span></span>

    ![Visa tabellen aktivitet](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="99d2e-203">Ytterligare Klicka i en aktivitet och du ser de data som lästes in informationen i den högra panelen inklusive datastorleken, rader, genomflöde och så vidare.</span><span class="sxs-lookup"><span data-stu-id="99d2e-203">Further click into an activity and you see the data loading details in the right panel including data size, rows, throughput, etc.</span></span>

    ![Visa tabellen Aktivitetsinformation](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="99d2e-205">Om du vill starta den här övervakningsvyn senare, gå till ditt SQL Data Warehouse, klickar du på **Läs in Data > Azure Data Factory**, Välj din factory och välj **övervaka befintliga läser in aktiviteter**.</span><span class="sxs-lookup"><span data-stu-id="99d2e-205">To launch this monitoring view later, go to your SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99d2e-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99d2e-206">Next steps</span></span>

<span data-ttu-id="99d2e-207">Om du vill migrera din databas till SQL Data Warehouse, se [migreringsöversikt](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="99d2e-207">To migrate your database to SQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="99d2e-208">Om du vill veta mer om Azure Data Factory och dess funktioner för flytt av data, finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="99d2e-208">To learn more about Azure Data Factory and its data movement capabilities, see the following articles:</span></span>

- [<span data-ttu-id="99d2e-209">Introduktion till Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="99d2e-209">Introduction to Azure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="99d2e-210">Flytta data med hjälp av Kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="99d2e-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="99d2e-211">Flytta data till och från Azure SQL Data Warehouse med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="99d2e-211">Move data to and from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="99d2e-212">Om du vill utforska dina data i SQL Data Warehouse, finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="99d2e-212">To explore your data in SQL Data Warehouse, see the following articles:</span></span>

- [<span data-ttu-id="99d2e-213">Ansluta till SQL Data Warehouse med Visual Studio och SSDT</span><span class="sxs-lookup"><span data-stu-id="99d2e-213">Connect to SQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="99d2e-214">[Visuella data med Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="99d2e-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[Azure-portalen]: https://portal.azure.com
