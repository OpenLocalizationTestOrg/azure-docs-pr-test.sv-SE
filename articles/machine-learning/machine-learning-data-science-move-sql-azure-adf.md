---
title: "Flytta data från en lokal SQL Server till SQL Azure med Azure Data Factory | Microsoft Docs"
description: "Ställ in en ADM-pipeline som composes två data migreringsaktiviteter som tillsammans flyttar data dagligen mellan databaser på lokalt och i molnet."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 39fe26d3388be8b558f05063a8965889c013a41e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a><span data-ttu-id="f1a6d-103">Flytta data från en lokal SQLServer till SQL Azure med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f1a6d-103">Move data from an on-premises SQL server to SQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="f1a6d-104">Det här avsnittet visar hur du flyttar data från en lokal SQL Server-databas till en SQL Azure Database via Azure Blob Storage med hjälp av Azure Data Factory (ADM).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-104">This topic shows how to move data from an on-premises SQL Server Database to a SQL Azure Database via Azure Blob Storage using the Azure Data Factory (ADF).</span></span>

<span data-ttu-id="f1a6d-105">En tabell som sammanfattar olika alternativ för att flytta data till en Azure SQL Database finns [flytta data till en Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-105">For a table that summarizes various options for moving data to an Azure SQL Database, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="f1a6d-106"><a name="intro"></a>Introduktion: Vad är ADF och när den används för att migrera data?</span><span class="sxs-lookup"><span data-stu-id="f1a6d-106"><a name="intro"></a>Introduction: What is ADF and when should it be used to migrate data?</span></span>
<span data-ttu-id="f1a6d-107">Azure Data Factory är en helt hanterad molnbaserade integration datatjänst som samordnar och automatiserar flytt och transformering av data.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="f1a6d-108">Viktiga begrepp i ADF-modellen är pipeline.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-108">The key concept in the ADF model is pipeline.</span></span> <span data-ttu-id="f1a6d-109">En pipeline är en logisk gruppering av aktiviteter, som definierar åtgärderna som ska utföras på de data som finns i DataSet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-109">A pipeline is a logical grouping of Activities, each of which defines the actions to perform on the data contained in Datasets.</span></span> <span data-ttu-id="f1a6d-110">Länkade tjänster används för att definiera den information som behövs för Data Factory för att ansluta till dataresurser.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-110">Linked services are used to define the information needed for Data Factory to connect to the data resources.</span></span>

<span data-ttu-id="f1a6d-111">Med ADF, kan befintliga databearbetning tjänster sammanställas till pipeline-data som är hög tillgänglighet och hanterad i molnet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in the cloud.</span></span> <span data-ttu-id="f1a6d-112">Dessa data pipelines kan schemaläggas för att mata in, förbereda, transformera, analysera och publicera data och ADF hanterar och samordnar komplexa data och beroenden för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-112">These data pipelines can be scheduled to ingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates the complex data and processing dependencies.</span></span> <span data-ttu-id="f1a6d-113">Lösningar kan snabbt inbyggda och distribueras i molnet, ansluta ett växande antal lokalt och molntjänster datakällor.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-113">Solutions can be quickly built and deployed in the cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="f1a6d-114">Överväg att använda ADF:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-114">Consider using ADF:</span></span>

* <span data-ttu-id="f1a6d-115">När data ska migreras kontinuerligt i ett hybridscenario som har åtkomst till både lokalt och molnresurser</span><span class="sxs-lookup"><span data-stu-id="f1a6d-115">when data needs to be continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="f1a6d-116">När data är överförd eller måste ändras eller har affärslogik som lagts till när migreras.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-116">when the data is transacted or needs to be modified or have business logic added to it when being migrated.</span></span>

<span data-ttu-id="f1a6d-117">ADF möjliggör schemaläggningen och övervakning av jobb med hjälp av enkla JSON-skript som hanterar flödet av data regelbundet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-117">ADF allows for the scheduling and monitoring of jobs using simple JSON scripts that manage the movement of data on a periodic basis.</span></span> <span data-ttu-id="f1a6d-118">ADF har även andra funktioner som stöd för komplex.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="f1a6d-119">Mer information om ADF finns i dokumentationen på [Azure Data Factory (ADM)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-119">For more information on ADF, see the documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="f1a6d-120"><a name="scenario"></a>Scenariot</span><span class="sxs-lookup"><span data-stu-id="f1a6d-120"><a name="scenario"></a>The Scenario</span></span>
<span data-ttu-id="f1a6d-121">Vi har skapat en ADM-pipeline som composes två aktiviteter för migrering av data.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="f1a6d-122">Tillsammans flytta data dagligen mellan en lokal SQL-databas och en Azure SQL Database i molnet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in the cloud.</span></span> <span data-ttu-id="f1a6d-123">Det finns två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-123">The two activities are:</span></span>

* <span data-ttu-id="f1a6d-124">Kopiera data från en lokal SQL Server-databas till ett Azure Blob Storage-konto</span><span class="sxs-lookup"><span data-stu-id="f1a6d-124">copy data from an on-premises SQL Server database to an Azure Blob Storage account</span></span>
* <span data-ttu-id="f1a6d-125">Kopiera data från Azure Blob Storage-konto till en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-125">copy data from the Azure Blob Storage account to an Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="f1a6d-126">Steg som visas här har anpassats från mer detaljerad genomgång som tillhandahålls av ADF-teamet: [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) referenser till de relevanta avsnitten i avsnittet Ange när det är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-126">The steps shown here have been adapted from the more detailed tutorial provided by the ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References to the relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="f1a6d-127"><a name="prereqs"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="f1a6d-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="f1a6d-128">Den här kursen förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="f1a6d-129">En **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-129">An **Azure subscription**.</span></span> <span data-ttu-id="f1a6d-130">Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f1a6d-131">En **Azure storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-131">An **Azure storage account**.</span></span> <span data-ttu-id="f1a6d-132">Du kan använda ett Azure storage-konto för att lagra data i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-132">You use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="f1a6d-133">Om du inte har ett Azure storage-konto finns i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-133">If you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="f1a6d-134">När du har skapat lagringskontot som du behöver hämta nyckeln konto används för åtkomst till lagringen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-134">After you have created the storage account, you need to obtain the account key used to access the storage.</span></span> <span data-ttu-id="f1a6d-135">Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="f1a6d-136">Åtkomst till en **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-136">Access to an **Azure SQL Database**.</span></span> <span data-ttu-id="f1a6d-137">Om du måste skapa en Azure SQL Database, tpoic [komma igång med Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) innehåller information om hur du etablerar en ny instans av en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-137">If you must set up an Azure SQL Database, the tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how to provision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="f1a6d-138">Installerat och konfigurerat **Azure PowerShell** lokalt.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="f1a6d-139">Instruktioner finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-139">For instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="f1a6d-140">Den här proceduren använder den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-140">This procedure uses the [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="f1a6d-141"><a name="upload-data"></a>Ladda upp data till din lokala SQL Server</span><span class="sxs-lookup"><span data-stu-id="f1a6d-141"><a name="upload-data"></a> Upload the data to your on-premises SQL Server</span></span>
<span data-ttu-id="f1a6d-142">Vi använder den [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) att demonstrera migreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-142">We use the [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) to demonstrate the migration process.</span></span> <span data-ttu-id="f1a6d-143">NYC Taxi dataset är tillgängligt, enligt beskrivningen i det inlägget på Azure-blobblagring [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-143">The NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="f1a6d-144">Data har två filer, trip_data.csv-fil som innehåller information om kommunikation, och filen trip_far.csv, som innehåller information om avgiften betalat för varje resa.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-144">The data has two files, the trip_data.csv file, which contains trip details, and the  trip_far.csv file, which contains details of the fare paid for each trip.</span></span> <span data-ttu-id="f1a6d-145">Ett exempel och en beskrivning av dessa filer finns i [NYC Taxi resor Dataset beskrivning](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="f1a6d-146">Du kan anpassa det förfarande som anges här till en uppsättning med dina egna data eller Följ stegen som beskrivs med NYC Taxi dataset.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-146">You can either adapt the procedure provided here to a set of your own data or follow the steps as described by using the NYC Taxi dataset.</span></span> <span data-ttu-id="f1a6d-147">Överför NYC Taxi dataset till din lokala SQL Server-databas genom att följa proceduren som beskrivs i [Bulk importera Data till SQL Server-databas](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-147">To upload the NYC Taxi dataset into your on-premises SQL Server database, follow the procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="f1a6d-148">Dessa instruktioner är för en SQL Server på en virtuell dator i Azure, men proceduren för att ladda upp till den lokala SQL Server är samma.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-148">These instructions are for a SQL Server on an Azure Virtual Machine, but the procedure for uploading to the on-premises SQL Server is the same.</span></span>

## <span data-ttu-id="f1a6d-149"><a name="create-adf"></a>Skapa ett Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f1a6d-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="f1a6d-150">Instruktioner för att skapa en ny Azure Data Factory och en resursgrupp i den [Azure-portalen](https://portal.azure.com/) tillhandahålls [skapa ett Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-150">The instructions for creating a new Azure Data Factory and a resource group in the [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="f1a6d-151">Namnge den nya instansen ADF *adfdsp* och kalla resursgruppen skapade *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-151">Name the new ADF instance *adfdsp* and name the resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-the-data-management-gateway"></a><span data-ttu-id="f1a6d-152">Installera och konfigurera upp Data Management Gateway</span><span class="sxs-lookup"><span data-stu-id="f1a6d-152">Install and configure up the Data Management Gateway</span></span>
<span data-ttu-id="f1a6d-153">Om du vill aktivera din pipelines i ett Azure data factory för att arbeta med en lokal SQL Server som du behöver lägga till den som en länkad tjänst datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-153">To enable your pipelines in an Azure data factory to work with an on-premises SQL Server, you need to add it as a Linked Service to the data factory.</span></span> <span data-ttu-id="f1a6d-154">Om du vill skapa en länkad tjänst för en lokal SQL Server, måste du:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-154">To create a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="f1a6d-155">Hämta och installera Microsoft Data Management Gateway till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-155">download and install Microsoft Data Management Gateway onto the on-premises computer.</span></span>
* <span data-ttu-id="f1a6d-156">Konfigurera den länkade tjänsten för lokala datakällan som ska använda gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-156">configure the linked service for the on-premises data source to use the gateway.</span></span>

<span data-ttu-id="f1a6d-157">Data Management Gateway Serialiserar och deserializes källa och mottagare data på den dator där den finns.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-157">The Data Management Gateway serializes and deserializes the source and sink data on the computer where it is hosted.</span></span>

<span data-ttu-id="f1a6d-158">Ställa in instruktioner och information om Data Management Gateway finns [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="f1a6d-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="f1a6d-159"><a name="adflinkedservices"></a>Skapa länkade tjänster att ansluta till dataresurser</span><span class="sxs-lookup"><span data-stu-id="f1a6d-159"><a name="adflinkedservices"></a>Create linked services to connect to the data resources</span></span>
<span data-ttu-id="f1a6d-160">En länkad tjänst definierar den information som behövs för Azure Data Factory för att ansluta till en Dataresurs.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-160">A linked service defines the information needed for Azure Data Factory to connect to a data resource.</span></span> <span data-ttu-id="f1a6d-161">Stegvisa anvisningar för att skapa länkade tjänster finns i [Skapa länkade tjänster](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-161">The step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="f1a6d-162">Vi har tre resurser i det här scenariot som krävs för länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="f1a6d-163">Länkad tjänst för lokala SQL Server</span><span class="sxs-lookup"><span data-stu-id="f1a6d-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="f1a6d-164">Länkad tjänst för Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="f1a6d-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="f1a6d-165">Länkad tjänst för Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="f1a6d-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="f1a6d-166"><a name="adf-linked-service-onprem-sql"></a>Länkad tjänst för lokala SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="f1a6d-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="f1a6d-167">För att skapa den länkade tjänsten för lokala SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-167">To create the linked service for the on-premises SQL Server:</span></span>

* <span data-ttu-id="f1a6d-168">Klicka på den **datalagret** i ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-168">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="f1a6d-169">Välj **SQL** och ange den *användarnamn* och *lösenord* autentiseringsuppgifter för lokal SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-169">select **SQL** and enter the *username* and *password* credentials for the on-premises SQL Server.</span></span> <span data-ttu-id="f1a6d-170">Du måste ange servernamn som en **instansnamn för fullständigt kvalificerade servername omvänt snedstreck (ServerNamn\InstansNamn)**.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-170">You need to enter the servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="f1a6d-171">Namnge den länkade tjänsten *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-171">Name the linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="f1a6d-172"><a name="adf-linked-service-blob-store"></a>Länkad tjänst för Blob</span><span class="sxs-lookup"><span data-stu-id="f1a6d-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="f1a6d-173">För att skapa den länkade tjänsten för Azure Blob Storage-konto:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-173">To create the linked service for the Azure Blob Storage account:</span></span>

* <span data-ttu-id="f1a6d-174">Klicka på den **datalagret** i ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-174">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="f1a6d-175">Välj **Azure Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="f1a6d-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="f1a6d-176">Ange Azure Blob Storage-nyckel och en behållare kontonamnet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-176">enter the Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="f1a6d-177">Namnge den länkade tjänsten *adfds*.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-177">Name the Linked Service *adfds*.</span></span>

### <span data-ttu-id="f1a6d-178"><a name="adf-linked-service-azure-sql"></a>Länkad tjänst för Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="f1a6d-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="f1a6d-179">För att skapa den länkade tjänsten för Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-179">To create the linked service for the Azure SQL Database:</span></span>

* <span data-ttu-id="f1a6d-180">Klicka på den **datalagret** i ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-180">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="f1a6d-181">Välj **Azure SQL** och ange den *användarnamn* och *lösenord* autentiseringsuppgifter för Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-181">select **Azure SQL** and enter the *username* and *password* credentials for the Azure SQL Database.</span></span> <span data-ttu-id="f1a6d-182">Den *användarnamn* måste anges som  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="f1a6d-182">The *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="f1a6d-183"><a name="adf-tables"></a>Definiera och skapa tabeller för att ange hur du kommer åt datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="f1a6d-183"><a name="adf-tables"></a>Define and create tables to specify how to access the datasets</span></span>
<span data-ttu-id="f1a6d-184">Skapa tabeller som anger strukturen, plats och tillgängligheten för datauppsättningar med följande skript-baserad.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-184">Create tables that specify the structure, location, and availability of the datasets with the following script-based procedures.</span></span> <span data-ttu-id="f1a6d-185">JSON-filer används för att definiera tabellerna.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-185">JSON files are used to define the tables.</span></span> <span data-ttu-id="f1a6d-186">Mer information om strukturen för de här filerna finns [datauppsättningar](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-186">For more information on the structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f1a6d-187">Du bör köra de `Add-AzureAccount` cmdlet innan du kör den [ny AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet för att bekräfta att rätt Azure-prenumeration har valts för Kommandokörningen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-187">You should execute the `Add-AzureAccount` cmdlet before executing the [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet to confirm that the right Azure subscription is selected for the command execution.</span></span> <span data-ttu-id="f1a6d-188">Dokumentation för denna cmdlet finns [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="f1a6d-189">JSON-baserade definitionerna i tabellerna använda följande namn:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-189">The JSON-based definitions in the tables use the following names:</span></span>

* <span data-ttu-id="f1a6d-190">den **tabellnamn** i den lokala SQL server är *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="f1a6d-190">the **table name** in the on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="f1a6d-191">den **behållarnamn** i Azure Blob Storage-kontot är *containername*</span><span class="sxs-lookup"><span data-stu-id="f1a6d-191">the **container name** in the Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="f1a6d-192">Tre tabelldefinitionerna krävs för den här ADF-pipelinen:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="f1a6d-193">Lokal SQL-tabell</span><span class="sxs-lookup"><span data-stu-id="f1a6d-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="f1a6d-194">BLOB-tabell</span><span class="sxs-lookup"><span data-stu-id="f1a6d-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="f1a6d-195">SQL Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="f1a6d-196">De här procedurerna använda Azure PowerShell för att definiera och skapa ADF-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-196">These procedures use Azure PowerShell to define and create the ADF activities.</span></span> <span data-ttu-id="f1a6d-197">Men dessa uppgifter kan också utföras med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-197">But these tasks can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="f1a6d-198">Mer information finns i [skapa datauppsättningar](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="f1a6d-199"><a name="adf-table-onprem-sql"></a>Lokal SQL-tabell</span><span class="sxs-lookup"><span data-stu-id="f1a6d-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="f1a6d-200">Tabelldefinitionen för lokala SQL Server har angetts för följande JSON-fil:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-200">The table definition for the on-premises SQL Server is specified in the following JSON file:</span></span>

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

<span data-ttu-id="f1a6d-201">Kolumnnamnen ingick inte.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-201">The column names were not included here.</span></span> <span data-ttu-id="f1a6d-202">Du kan välja på kolumnnamnen underordnad genom att inkludera dem här (information den [ADF dokumentationen](../data-factory/data-factory-data-movement-activities.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-202">You can sub-select on the column names by including them here (for details check the [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="f1a6d-203">Kopiera JSON-definitionen av tabellen i en fil kallad *onpremtabledef.json* filen och spara den på en känd plats (här antas vara *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-203">Copy the JSON definition of the table into a file called *onpremtabledef.json* file and save it to a known location (here assumed to be *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="f1a6d-204">Skapa tabellen i ADF med följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-204">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="f1a6d-205"><a name="adf-table-blob-store"></a>BLOB-tabell</span><span class="sxs-lookup"><span data-stu-id="f1a6d-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="f1a6d-206">Definitionen för tabellen för platsen blob finns i följande (det här mappar infogade data från lokalt till Azure blob):</span><span class="sxs-lookup"><span data-stu-id="f1a6d-206">Definition for the table for the output blob location is in the following (this maps the ingested data from on-premises to Azure blob):</span></span>

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

<span data-ttu-id="f1a6d-207">Kopiera JSON-definitionen av tabellen i en fil kallad *bloboutputtabledef.json* filen och spara den på en känd plats (här antas vara *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-207">Copy the JSON definition of the table into a file called *bloboutputtabledef.json* file and save it to a known location (here assumed to be *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="f1a6d-208">Skapa tabellen i ADF med följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-208">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="f1a6d-209"><a name="adf-table-azure-sq"></a>SQL Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="f1a6d-210">Definitionen för tabellen för SQL Azure utdata finns i följande (det här schemat mappar data från blob):</span><span class="sxs-lookup"><span data-stu-id="f1a6d-210">Definition for the table for the SQL Azure output is in the following (this schema maps the data coming from the blob):</span></span>

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

<span data-ttu-id="f1a6d-211">Kopiera JSON-definitionen av tabellen i en fil kallad *AzureSqlTable.json* filen och spara den på en känd plats (här antas vara *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-211">Copy the JSON definition of the table into a file called *AzureSqlTable.json* file and save it to a known location (here assumed to be *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="f1a6d-212">Skapa tabellen i ADF med följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-212">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="f1a6d-213"><a name="adf-pipeline"></a>Definiera och skapa pipelinen</span><span class="sxs-lookup"><span data-stu-id="f1a6d-213"><a name="adf-pipeline"></a>Define and create the pipeline</span></span>
<span data-ttu-id="f1a6d-214">Ange de aktiviteter som tillhör pipelinen och skapa pipeline med följande skript-baserad.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-214">Specify the activities that belong to the pipeline and create the pipeline with the following script-based procedures.</span></span> <span data-ttu-id="f1a6d-215">En JSON-fil används för att definiera egenskaperna pipeline.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-215">A JSON file is used to define the pipeline properties.</span></span>

* <span data-ttu-id="f1a6d-216">Skriptet förutsätter att den **pipeline namnet** är *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-216">The script assumes that the **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="f1a6d-217">Observera också att vi anger periodiciteten för pipeline köras dag och använder standard körningstid för jobb (12: 00 UTC).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-217">Also note that we set the periodicity of the pipeline to be executed on daily basis and use the default execution time for the job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="f1a6d-218">Följande procedurer använda Azure PowerShell för att definiera och skapa ADF-pipeline.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-218">The following procedures use Azure PowerShell to define and create the ADF pipeline.</span></span> <span data-ttu-id="f1a6d-219">Men den här uppgiften kan också utföras med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="f1a6d-220">Mer information finns i [skapa pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="f1a6d-221">Med tabelldefinitionerna tidigare anges pipeline-definitionen för ADF enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-221">Using the table definitions provided previously, the pipeline definition for the ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

<span data-ttu-id="f1a6d-222">Kopiera den här JSON-definitionen i pipeline till en fil kallad *pipelinedef.json* filen och spara den på en känd plats (här antas vara *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-222">Copy this JSON definition of the pipeline into a file called *pipelinedef.json* file and save it to a known location (here assumed to be *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="f1a6d-223">Skapa pipelinen i ADF med följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-223">Create the pipeline in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="f1a6d-224">Bekräfta att du kan se pipelinen på ADF i den klassiska Azure-portalen visas som följande (när du klickar på diagrammet)</span><span class="sxs-lookup"><span data-stu-id="f1a6d-224">Confirm that you can see the pipeline on the ADF in the Azure Classic Portal show up as following (when you click the diagram)</span></span>

![ADF pipeline](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="f1a6d-226"><a name="adf-pipeline-start"></a>Starta Pipeline</span><span class="sxs-lookup"><span data-stu-id="f1a6d-226"><a name="adf-pipeline-start"></a>Start the Pipeline</span></span>
<span data-ttu-id="f1a6d-227">Nu kan köra pipelinen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f1a6d-227">The pipeline can now be run using the following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="f1a6d-228">Den *startdate* och *enddate* parametervärden måste ersättas med verkliga datum som ska köra pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-228">The *startdate* and *enddate* parameter values need to be replaced with the actual dates between which you want the pipeline to run.</span></span>

<span data-ttu-id="f1a6d-229">När pipelinen körs, bör du kunna se data som visas i behållaren som valts för blob, en fil per dag.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-229">Once the pipeline executes, you should be able to see the data show up in the container selected for the blob, one file per day.</span></span>

<span data-ttu-id="f1a6d-230">Observera att vi inte utnyttjas funktionerna i ADF till pipe data inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="f1a6d-230">Note that we have not leveraged the functionality provided by ADF to pipe data incrementally.</span></span> <span data-ttu-id="f1a6d-231">Mer information om hur du gör detta och andra funktioner som tillhandahålls av ADF finns i [ADF dokumentationen](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="f1a6d-231">For more information on how to do this and other capabilities provided by ADF, see the [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
