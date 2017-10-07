---
title: "aaaMove data från en lokal SQL Server-tooSQL Azure med Azure Data Factory | Microsoft Docs"
description: "Ställ in en ADM-pipeline som composes två data migreringsaktiviteter som tillsammans flyttar data dagligen mellan databaser på lokalt och i hello molnet."
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
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a><span data-ttu-id="b8227-103">Flytta data från en lokal SQL server tooSQL Azure med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b8227-103">Move data from an on-premises SQL server tooSQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="b8227-104">Det här avsnittet visar hur toomove data från en lokal SQL Server-databas tooa SQL Azure Database via Azure Blob Storage med hjälp av hello Azure Data Factory (ADM).</span><span class="sxs-lookup"><span data-stu-id="b8227-104">This topic shows how toomove data from an on-premises SQL Server Database tooa SQL Azure Database via Azure Blob Storage using hello Azure Data Factory (ADF).</span></span>

<span data-ttu-id="b8227-105">En tabell som sammanfattar olika alternativ för att flytta data tooan Azure SQL Database finns [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b8227-105">For a table that summarizes various options for moving data tooan Azure SQL Database, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="b8227-106"><a name="intro"></a>Introduktion: Vad är ADF och när ska den vara används toomigrate data?</span><span class="sxs-lookup"><span data-stu-id="b8227-106"><a name="intro"></a>Introduction: What is ADF and when should it be used toomigrate data?</span></span>
<span data-ttu-id="b8227-107">Azure Data Factory är en helt hanterad molnbaserade integration datatjänst som samordnar och automatiserar hello flytt och transformering av data.</span><span class="sxs-lookup"><span data-stu-id="b8227-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="b8227-108">hello viktiga begrepp i hello ADF modellen är pipeline.</span><span class="sxs-lookup"><span data-stu-id="b8227-108">hello key concept in hello ADF model is pipeline.</span></span> <span data-ttu-id="b8227-109">En pipeline är en logisk gruppering av aktiviteter, som definierar hello åtgärder tooperform på hello data som finns i DataSet.</span><span class="sxs-lookup"><span data-stu-id="b8227-109">A pipeline is a logical grouping of Activities, each of which defines hello actions tooperform on hello data contained in Datasets.</span></span> <span data-ttu-id="b8227-110">Länkade tjänster är används toodefine hello information som behövs för Data Factory tooconnect toohello dataresurser.</span><span class="sxs-lookup"><span data-stu-id="b8227-110">Linked services are used toodefine hello information needed for Data Factory tooconnect toohello data resources.</span></span>

<span data-ttu-id="b8227-111">Med ADF, kan befintliga databearbetning tjänster sammanställas till pipeline-data som är hög tillgänglighet och hanterad i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="b8227-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in hello cloud.</span></span> <span data-ttu-id="b8227-112">Pipelines dessa data kan vara schemalagda tooingest, förbereda, transformera, analysera och publicera data och ADF hanterar och samordnar hello komplexa data och beroenden för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="b8227-112">These data pipelines can be scheduled tooingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates hello complex data and processing dependencies.</span></span> <span data-ttu-id="b8227-113">Lösningar kan hello snabbt inbyggd och distribuerade i molnet, ansluta ett växande antal lokalt och moln-datakällor.</span><span class="sxs-lookup"><span data-stu-id="b8227-113">Solutions can be quickly built and deployed in hello cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="b8227-114">Överväg att använda ADF:</span><span class="sxs-lookup"><span data-stu-id="b8227-114">Consider using ADF:</span></span>

* <span data-ttu-id="b8227-115">När data måste toobe migreras kontinuerligt i ett hybridscenario som har åtkomst till både lokalt och molnresurser</span><span class="sxs-lookup"><span data-stu-id="b8227-115">when data needs toobe continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="b8227-116">När hello data överförda eller toobe måste ändras eller har affärslogik till tooit när migreras.</span><span class="sxs-lookup"><span data-stu-id="b8227-116">when hello data is transacted or needs toobe modified or have business logic added tooit when being migrated.</span></span>

<span data-ttu-id="b8227-117">ADF tillåter hello schemaläggning och övervakning av jobb med hjälp av enkla JSON-skript som hanterar hello flödet av data regelbundet.</span><span class="sxs-lookup"><span data-stu-id="b8227-117">ADF allows for hello scheduling and monitoring of jobs using simple JSON scripts that manage hello movement of data on a periodic basis.</span></span> <span data-ttu-id="b8227-118">ADF har även andra funktioner som stöd för komplex.</span><span class="sxs-lookup"><span data-stu-id="b8227-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="b8227-119">Mer information om ADF finns hello dokumentationen på [Azure Data Factory (ADM)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="b8227-119">For more information on ADF, see hello documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="b8227-120"><a name="scenario"></a>hello Scenario</span><span class="sxs-lookup"><span data-stu-id="b8227-120"><a name="scenario"></a>hello Scenario</span></span>
<span data-ttu-id="b8227-121">Vi har skapat en ADM-pipeline som composes två aktiviteter för migrering av data.</span><span class="sxs-lookup"><span data-stu-id="b8227-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="b8227-122">Tillsammans flytta data dagligen mellan en lokal SQL-databas och en Azure SQL Database i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="b8227-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in hello cloud.</span></span> <span data-ttu-id="b8227-123">hello två aktiviteter är:</span><span class="sxs-lookup"><span data-stu-id="b8227-123">hello two activities are:</span></span>

* <span data-ttu-id="b8227-124">Kopiera data från en lokal SQL Server-databasen tooan Azure Blob Storage-konto</span><span class="sxs-lookup"><span data-stu-id="b8227-124">copy data from an on-premises SQL Server database tooan Azure Blob Storage account</span></span>
* <span data-ttu-id="b8227-125">Kopiera data från hello Azure Blob Storage-konto tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8227-125">copy data from hello Azure Blob Storage account tooan Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="b8227-126">Hej steg som visas här har anpassats från hello mer detaljerad genomgång som tillhandahålls av hello ADF team: [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) refererar till toohello relevanta avsnitt i avsnittet anges vid behov.</span><span class="sxs-lookup"><span data-stu-id="b8227-126">hello steps shown here have been adapted from hello more detailed tutorial provided by hello ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References toohello relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="b8227-127"><a name="prereqs"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="b8227-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="b8227-128">Den här kursen förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="b8227-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="b8227-129">En **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="b8227-129">An **Azure subscription**.</span></span> <span data-ttu-id="b8227-130">Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8227-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="b8227-131">En **Azure storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="b8227-131">An **Azure storage account**.</span></span> <span data-ttu-id="b8227-132">Du kan använda ett Azure storage-konto för att lagra hello data i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b8227-132">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="b8227-133">Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel.</span><span class="sxs-lookup"><span data-stu-id="b8227-133">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="b8227-134">När du har skapat hello storage-konto behöver du tooobtain hello konto nyckel som används för tooaccess hello lagring.</span><span class="sxs-lookup"><span data-stu-id="b8227-134">After you have created hello storage account, you need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="b8227-135">Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="b8227-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="b8227-136">Åtkomst tooan **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="b8227-136">Access tooan **Azure SQL Database**.</span></span> <span data-ttu-id="b8227-137">Om du måste konfigurera en Azure SQL Database hello tpoic [komma igång med Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) innehåller information om hur tooprovision en ny instans av en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8227-137">If you must set up an Azure SQL Database, hello tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how tooprovision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="b8227-138">Installerat och konfigurerat **Azure PowerShell** lokalt.</span><span class="sxs-lookup"><span data-stu-id="b8227-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="b8227-139">Instruktioner finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b8227-139">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="b8227-140">Den här proceduren använder hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b8227-140">This procedure uses hello [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="b8227-141"><a name="upload-data"></a>Överför hello data tooyour lokala SQL Server</span><span class="sxs-lookup"><span data-stu-id="b8227-141"><a name="upload-data"></a> Upload hello data tooyour on-premises SQL Server</span></span>
<span data-ttu-id="b8227-142">Vi använder hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello migreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="b8227-142">We use hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello migration process.</span></span> <span data-ttu-id="b8227-143">hello NYC Taxi dataset är tillgänglig, enligt beskrivningen i det inlägget på Azure-blobblagring [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="b8227-143">hello NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="b8227-144">hello har två filer, hello trip_data.csv filen som innehåller information om resa och hello trip_far.csv filen som innehåller information om hello avgiften betalat för varje resa.</span><span class="sxs-lookup"><span data-stu-id="b8227-144">hello data has two files, hello trip_data.csv file, which contains trip details, and hello  trip_far.csv file, which contains details of hello fare paid for each trip.</span></span> <span data-ttu-id="b8227-145">Ett exempel och en beskrivning av dessa filer finns i [NYC Taxi resor Dataset beskrivning](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="b8227-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="b8227-146">Du kan anpassa hello förfarandet här tooa uppsättning dina egna data eller hello gör enligt med hjälp av hello NYC Taxi dataset.</span><span class="sxs-lookup"><span data-stu-id="b8227-146">You can either adapt hello procedure provided here tooa set of your own data or follow hello steps as described by using hello NYC Taxi dataset.</span></span> <span data-ttu-id="b8227-147">tooupload hello NYC Taxi dataset i dina lokala SQL Server-databas, följ hello proceduren som beskrivs i [Bulk importera Data till SQL Server-databas](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="b8227-147">tooupload hello NYC Taxi dataset into your on-premises SQL Server database, follow hello procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="b8227-148">Dessa instruktioner är för en SQL Server på en virtuell dator i Azure, men hello proceduren för att ladda upp toohello på lokal SQL Server är hello samma.</span><span class="sxs-lookup"><span data-stu-id="b8227-148">These instructions are for a SQL Server on an Azure Virtual Machine, but hello procedure for uploading toohello on-premises SQL Server is hello same.</span></span>

## <span data-ttu-id="b8227-149"><a name="create-adf"></a>Skapa ett Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b8227-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="b8227-150">Hej instruktioner för att skapa en ny Azure Data Factory och en resursgrupp i hello [Azure-portalen](https://portal.azure.com/) tillhandahålls [skapa ett Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="b8227-150">hello instructions for creating a new Azure Data Factory and a resource group in hello [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="b8227-151">Namnet hello ny ADF instans *adfdsp* och namnet hello resursgrupp skapade *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="b8227-151">Name hello new ADF instance *adfdsp* and name hello resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-hello-data-management-gateway"></a><span data-ttu-id="b8227-152">Installera och konfigurera in hello Data Management Gateway</span><span class="sxs-lookup"><span data-stu-id="b8227-152">Install and configure up hello Data Management Gateway</span></span>
<span data-ttu-id="b8227-153">tooenable din pipelines i ett Azure data factory toowork med en lokal SQL Server behöver du tooadd den som en länkad tjänst toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="b8227-153">tooenable your pipelines in an Azure data factory toowork with an on-premises SQL Server, you need tooadd it as a Linked Service toohello data factory.</span></span> <span data-ttu-id="b8227-154">toocreate en länkad tjänst för en lokal SQL Server måste du:</span><span class="sxs-lookup"><span data-stu-id="b8227-154">toocreate a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="b8227-155">Hämta och installera Microsoft Data Management Gateway till hello lokala dator.</span><span class="sxs-lookup"><span data-stu-id="b8227-155">download and install Microsoft Data Management Gateway onto hello on-premises computer.</span></span>
* <span data-ttu-id="b8227-156">Konfigurera hello länkad tjänst för hello lokala datakälla toouse hello gateway.</span><span class="sxs-lookup"><span data-stu-id="b8227-156">configure hello linked service for hello on-premises data source toouse hello gateway.</span></span>

<span data-ttu-id="b8227-157">hello Data Management Gateway Serialiserar och deserializes hello källa och mottagare data på hello dator där det finns.</span><span class="sxs-lookup"><span data-stu-id="b8227-157">hello Data Management Gateway serializes and deserializes hello source and sink data on hello computer where it is hosted.</span></span>

<span data-ttu-id="b8227-158">Ställa in instruktioner och information om Data Management Gateway finns [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="b8227-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="b8227-159"><a name="adflinkedservices"></a>Skapa länkade tjänster tooconnect toohello dataresurser</span><span class="sxs-lookup"><span data-stu-id="b8227-159"><a name="adflinkedservices"></a>Create linked services tooconnect toohello data resources</span></span>
<span data-ttu-id="b8227-160">En länkad tjänst definierar hello information som behövs för Azure Data Factory tooconnect tooa Dataresurs.</span><span class="sxs-lookup"><span data-stu-id="b8227-160">A linked service defines hello information needed for Azure Data Factory tooconnect tooa data resource.</span></span> <span data-ttu-id="b8227-161">hello stegvisa anvisningar för att skapa länkade tjänster finns i [Skapa länkade tjänster](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="b8227-161">hello step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="b8227-162">Vi har tre resurser i det här scenariot som krävs för länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="b8227-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="b8227-163">Länkad tjänst för lokala SQL Server</span><span class="sxs-lookup"><span data-stu-id="b8227-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="b8227-164">Länkad tjänst för Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b8227-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="b8227-165">Länkad tjänst för Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="b8227-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="b8227-166"><a name="adf-linked-service-onprem-sql"></a>Länkad tjänst för lokala SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="b8227-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="b8227-167">toocreate hello länkad tjänst för hello lokala SQL Server:</span><span class="sxs-lookup"><span data-stu-id="b8227-167">toocreate hello linked service for hello on-premises SQL Server:</span></span>

* <span data-ttu-id="b8227-168">Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b8227-168">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="b8227-169">Välj **SQL** och ange hello *användarnamn* och *lösenord* autentiseringsuppgifter för hello lokala SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8227-169">select **SQL** and enter hello *username* and *password* credentials for hello on-premises SQL Server.</span></span> <span data-ttu-id="b8227-170">Du behöver tooenter hello servername som en **instansnamn för fullständigt kvalificerade servername omvänt snedstreck (ServerNamn\InstansNamn)**.</span><span class="sxs-lookup"><span data-stu-id="b8227-170">You need tooenter hello servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="b8227-171">Namnet hello länkade tjänsten *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="b8227-171">Name hello linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="b8227-172"><a name="adf-linked-service-blob-store"></a>Länkad tjänst för Blob</span><span class="sxs-lookup"><span data-stu-id="b8227-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="b8227-173">toocreate hello länkad tjänst för hello Azure Blob Storage-konto:</span><span class="sxs-lookup"><span data-stu-id="b8227-173">toocreate hello linked service for hello Azure Blob Storage account:</span></span>

* <span data-ttu-id="b8227-174">Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b8227-174">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="b8227-175">Välj **Azure Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="b8227-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="b8227-176">Ange hello Azure Blob Storage-konto nyckel och behållare.</span><span class="sxs-lookup"><span data-stu-id="b8227-176">enter hello Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="b8227-177">Namnet hello länkade tjänsten *adfds*.</span><span class="sxs-lookup"><span data-stu-id="b8227-177">Name hello Linked Service *adfds*.</span></span>

### <span data-ttu-id="b8227-178"><a name="adf-linked-service-azure-sql"></a>Länkad tjänst för Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="b8227-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="b8227-179">toocreate hello länkad tjänst för hello Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="b8227-179">toocreate hello linked service for hello Azure SQL Database:</span></span>

* <span data-ttu-id="b8227-180">Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b8227-180">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="b8227-181">Välj **Azure SQL** och ange hello *användarnamn* och *lösenord* autentiseringsuppgifter för hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8227-181">select **Azure SQL** and enter hello *username* and *password* credentials for hello Azure SQL Database.</span></span> <span data-ttu-id="b8227-182">Hej *användarnamn* måste anges som  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="b8227-182">hello *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="b8227-183"><a name="adf-tables"></a>Definiera och skapa tabeller toospecify hur tooaccess hello datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="b8227-183"><a name="adf-tables"></a>Define and create tables toospecify how tooaccess hello datasets</span></span>
<span data-ttu-id="b8227-184">Skapa tabeller som anger hello struktur, plats och tillgängligheten för hello datauppsättningar med hello följa skriptbaserade procedurer.</span><span class="sxs-lookup"><span data-stu-id="b8227-184">Create tables that specify hello structure, location, and availability of hello datasets with hello following script-based procedures.</span></span> <span data-ttu-id="b8227-185">JSON-filer finns används toodefine hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="b8227-185">JSON files are used toodefine hello tables.</span></span> <span data-ttu-id="b8227-186">Mer information om hello strukturen för de här filerna finns [datauppsättningar](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="b8227-186">For more information on hello structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b8227-187">Du bör köra hello `Add-AzureAccount` cmdlet innan du kör hello [ny AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet tooconfirm som hello höger Azure-prenumeration har valts för hello Kommandokörning.</span><span class="sxs-lookup"><span data-stu-id="b8227-187">You should execute hello `Add-AzureAccount` cmdlet before executing hello [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet tooconfirm that hello right Azure subscription is selected for hello command execution.</span></span> <span data-ttu-id="b8227-188">Dokumentation för denna cmdlet finns [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="b8227-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="b8227-189">hello JSON-baserade definitioner i hello tabeller Använd hello följande namn:</span><span class="sxs-lookup"><span data-stu-id="b8227-189">hello JSON-based definitions in hello tables use hello following names:</span></span>

* <span data-ttu-id="b8227-190">Hej **tabellnamn** i hello lokal SQLServer är *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="b8227-190">hello **table name** in hello on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="b8227-191">Hej **behållarnamn** i hello Azure Blob Storage-kontot är *containername*</span><span class="sxs-lookup"><span data-stu-id="b8227-191">hello **container name** in hello Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="b8227-192">Tre tabelldefinitionerna krävs för den här ADF-pipelinen:</span><span class="sxs-lookup"><span data-stu-id="b8227-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="b8227-193">Lokal SQL-tabell</span><span class="sxs-lookup"><span data-stu-id="b8227-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="b8227-194">BLOB-tabell</span><span class="sxs-lookup"><span data-stu-id="b8227-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="b8227-195">SQL Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="b8227-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="b8227-196">De här procedurerna Använd Azure PowerShell toodefine och skapa hello ADF aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b8227-196">These procedures use Azure PowerShell toodefine and create hello ADF activities.</span></span> <span data-ttu-id="b8227-197">Men dessa uppgifter kan också utföras med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b8227-197">But these tasks can also be accomplished using hello Azure portal.</span></span> <span data-ttu-id="b8227-198">Mer information finns i [skapa datauppsättningar](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="b8227-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="b8227-199"><a name="adf-table-onprem-sql"></a>Lokal SQL-tabell</span><span class="sxs-lookup"><span data-stu-id="b8227-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="b8227-200">hello tabelldefinitionen för hello lokala SQL Server har angetts i hello följande JSON-fil:</span><span class="sxs-lookup"><span data-stu-id="b8227-200">hello table definition for hello on-premises SQL Server is specified in hello following JSON file:</span></span>

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

<span data-ttu-id="b8227-201">hello kolumnnamn ingick inte.</span><span class="sxs-lookup"><span data-stu-id="b8227-201">hello column names were not included here.</span></span> <span data-ttu-id="b8227-202">Du kan välja på hello kolumnnamn genom att inkludera dem här underordnad (information finns hello [ADF dokumentationen](../data-factory/data-factory-data-movement-activities.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b8227-202">You can sub-select on hello column names by including them here (for details check hello [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="b8227-203">Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *onpremtabledef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="b8227-203">Copy hello JSON definition of hello table into a file called *onpremtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="b8227-204">Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b8227-204">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="b8227-205"><a name="adf-table-blob-store"></a>BLOB-tabell</span><span class="sxs-lookup"><span data-stu-id="b8227-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="b8227-206">Definitionen för hello tabellen för hello utdata blob befinner sig i hello följande (det här mappar hello inhämtas data från lokala tooAzure blob):</span><span class="sxs-lookup"><span data-stu-id="b8227-206">Definition for hello table for hello output blob location is in hello following (this maps hello ingested data from on-premises tooAzure blob):</span></span>

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

<span data-ttu-id="b8227-207">Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *bloboutputtabledef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="b8227-207">Copy hello JSON definition of hello table into a file called *bloboutputtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="b8227-208">Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b8227-208">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="b8227-209"><a name="adf-table-azure-sq"></a>SQL Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="b8227-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="b8227-210">Definitionen för hello tabellen för hello SQL Azure utdata har hello följande (det här schemat mappar hello data från blob hello):</span><span class="sxs-lookup"><span data-stu-id="b8227-210">Definition for hello table for hello SQL Azure output is in hello following (this schema maps hello data coming from hello blob):</span></span>

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

<span data-ttu-id="b8227-211">Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *AzureSqlTable.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="b8227-211">Copy hello JSON definition of hello table into a file called *AzureSqlTable.json* file and save it tooa known location (here assumed toobe *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="b8227-212">Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b8227-212">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="b8227-213"><a name="adf-pipeline"></a>Definiera och skapa hello pipeline</span><span class="sxs-lookup"><span data-stu-id="b8227-213"><a name="adf-pipeline"></a>Define and create hello pipeline</span></span>
<span data-ttu-id="b8227-214">Ange hello aktiviteter som tillhör toohello pipeline och skapa hello pipeline med hello följa skriptbaserade procedurer.</span><span class="sxs-lookup"><span data-stu-id="b8227-214">Specify hello activities that belong toohello pipeline and create hello pipeline with hello following script-based procedures.</span></span> <span data-ttu-id="b8227-215">En JSON-fil är används toodefine hello pipeline egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b8227-215">A JSON file is used toodefine hello pipeline properties.</span></span>

* <span data-ttu-id="b8227-216">hello skript förutsätter att hello **pipeline namnet** är *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="b8227-216">hello script assumes that hello **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="b8227-217">Observera också att vi anger hello periodicitet hello pipeline toobe på daglig basis och används hello standard körningstiden för hello jobb (12: 00 UTC).</span><span class="sxs-lookup"><span data-stu-id="b8227-217">Also note that we set hello periodicity of hello pipeline toobe executed on daily basis and use hello default execution time for hello job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="b8227-218">hello följande procedurer Använd Azure PowerShell toodefine och skapa hello ADF pipeline.</span><span class="sxs-lookup"><span data-stu-id="b8227-218">hello following procedures use Azure PowerShell toodefine and create hello ADF pipeline.</span></span> <span data-ttu-id="b8227-219">Men den här uppgiften kan också utföras med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b8227-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="b8227-220">Mer information finns i [skapa pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="b8227-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="b8227-221">Med hjälp av hello definitioner som tidigare hello pipeline definition för hello ADF anges enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b8227-221">Using hello table definitions provided previously, hello pipeline definition for hello ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
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
                        "description": "Push data tooSql Azure",        
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

<span data-ttu-id="b8227-222">Kopiera den här JSON-definitionen för hello pipeline till en fil med namnet *pipelinedef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="b8227-222">Copy this JSON definition of hello pipeline into a file called *pipelinedef.json* file and save it tooa known location (here assumed toobe *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="b8227-223">Skapa hello pipeline i ADF med hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b8227-223">Create hello pipeline in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="b8227-224">Bekräfta att du kan se hello-pipeline på hello ADF i hello Azure klassiska Portal visas som följande (när du klickar på hello diagram)</span><span class="sxs-lookup"><span data-stu-id="b8227-224">Confirm that you can see hello pipeline on hello ADF in hello Azure Classic Portal show up as following (when you click hello diagram)</span></span>

![ADF pipeline](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="b8227-226"><a name="adf-pipeline-start"></a>Starta hello Pipeline</span><span class="sxs-lookup"><span data-stu-id="b8227-226"><a name="adf-pipeline-start"></a>Start hello Pipeline</span></span>
<span data-ttu-id="b8227-227">Nu kan köra hello pipeline med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b8227-227">hello pipeline can now be run using hello following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="b8227-228">Hej *startdate* och *enddate* parametervärden måste ersättas med hello verkliga datum som ska hello pipeline toorun toobe.</span><span class="sxs-lookup"><span data-stu-id="b8227-228">hello *startdate* and *enddate* parameter values need toobe replaced with hello actual dates between which you want hello pipeline toorun.</span></span>

<span data-ttu-id="b8227-229">När hello pipeline utförs, bör du kunna toosee hello data visas i hello-behållaren som valts för hello blob, en fil per dag.</span><span class="sxs-lookup"><span data-stu-id="b8227-229">Once hello pipeline executes, you should be able toosee hello data show up in hello container selected for hello blob, one file per day.</span></span>

<span data-ttu-id="b8227-230">Observera att vi inte utnyttjas hello funktionalitet som tillhandahålls av ADF toopipe data inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="b8227-230">Note that we have not leveraged hello functionality provided by ADF toopipe data incrementally.</span></span> <span data-ttu-id="b8227-231">Mer information om hur toodo detta och andra funktioner som tillhandahålls av ADF, se hello [ADF dokumentationen](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="b8227-231">For more information on how toodo this and other capabilities provided by ADF, see hello [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
