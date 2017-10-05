---
title: "Hur du datakällor för Data-profil"
description: "Artikel markering av hur du lägger till profiler för tabell - och på kolumnnivå data när du registrerar datakällor i Azure Data Catalog och hur du använder data profiler för att förstå datakällor."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 8f4174f0ed74706b8275c8b1f0a62753f2834fa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="0651a-103">Datakällor för dataprofil</span><span class="sxs-lookup"><span data-stu-id="0651a-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="0651a-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="0651a-104">Introduction</span></span>
<span data-ttu-id="0651a-105">**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="0651a-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="0651a-106">Med andra ord **Azure Data Catalog** handlar om hjälper människor identifiera, förstå och använda datakällor och hjälper organisationer att få ut mer av sina befintliga data.</span><span class="sxs-lookup"><span data-stu-id="0651a-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data.</span></span> <span data-ttu-id="0651a-107">När en datakälla har registrerats med **Azure Data Catalog**, dess metadata kopieras och indexeras av tjänsten, men artikeln inte det avslutas.</span><span class="sxs-lookup"><span data-stu-id="0651a-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span>

<span data-ttu-id="0651a-108">Den **Data profilering** funktion i **Azure Data Catalog** undersöker data från datakällor som stöds i katalogen och samlar in information om dessa data och statistik.</span><span class="sxs-lookup"><span data-stu-id="0651a-108">The **Data Profiling** feature of **Azure Data Catalog** examines the data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="0651a-109">Det är enkelt att inkludera en profil för dina datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="0651a-109">It's easy to include a profile of your data assets.</span></span> <span data-ttu-id="0651a-110">När du registrerar en datatillgång, välja **inkludera Data profil** i registreringsverktyget för datakällor.</span><span class="sxs-lookup"><span data-stu-id="0651a-110">When you register a data asset, choose **Include Data Profile** in the data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="0651a-111">Vad är profilering av Data</span><span class="sxs-lookup"><span data-stu-id="0651a-111">What is Data Profiling</span></span>
<span data-ttu-id="0651a-112">Data profilering undersöker data i datakällan som registreras och samlar in information om dessa data och statistik.</span><span class="sxs-lookup"><span data-stu-id="0651a-112">Data profiling examines the data in the data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="0651a-113">Datakällsidentifiering, kan statistiken hjälpa dig att avgöra lämpligheten av informationen som ska lösa sina affärsproblem.</span><span class="sxs-lookup"><span data-stu-id="0651a-113">During data source discovery, these statistics can help you determine the suitability of the data to solve their business problem.</span></span>

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

<span data-ttu-id="0651a-114">Följande datakällor stöder profilering av data:</span><span class="sxs-lookup"><span data-stu-id="0651a-114">The following data sources support data profiling:</span></span>

* <span data-ttu-id="0651a-115">SQL Server (inklusive Azure SQL DB och Azure SQL Data Warehouse) tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="0651a-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="0651a-116">Oracle-tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="0651a-116">Oracle tables and views</span></span>
* <span data-ttu-id="0651a-117">Teradata-tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="0651a-117">Teradata tables and views</span></span>
* <span data-ttu-id="0651a-118">Hive-tabeller</span><span class="sxs-lookup"><span data-stu-id="0651a-118">Hive tables</span></span>

<span data-ttu-id="0651a-119">Inklusive profiler data när du registrerar datatillgångar hjälper användarna att besvara frågor om datakällor, inklusive:</span><span class="sxs-lookup"><span data-stu-id="0651a-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="0651a-120">Kan den användas för att lösa problemet mitt företag?</span><span class="sxs-lookup"><span data-stu-id="0651a-120">Can it be used to solve my business problem?</span></span>
* <span data-ttu-id="0651a-121">Data följer särskilda standarder eller mönster?</span><span class="sxs-lookup"><span data-stu-id="0651a-121">Does the data conform to particular standards or patterns?</span></span>
* <span data-ttu-id="0651a-122">Vilka är några av avvikelser av datakällan?</span><span class="sxs-lookup"><span data-stu-id="0651a-122">What are some of the anomalies of the data source?</span></span>
* <span data-ttu-id="0651a-123">Vad är möjliga utmaningarna med att integrera informationen i mitt program?</span><span class="sxs-lookup"><span data-stu-id="0651a-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="0651a-124">Du kan också lägga till dokumentationen för en tillgång för att beskriva hur data kan integreras i ett program.</span><span class="sxs-lookup"><span data-stu-id="0651a-124">You can also add documentation to an asset to describe how data could be integrated into an application.</span></span> <span data-ttu-id="0651a-125">Se [så här dokumenterar du datakällor](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="0651a-125">See [How to document data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="0651a-126">Hur du lägger till en profil för data när du registrerar en datakälla</span><span class="sxs-lookup"><span data-stu-id="0651a-126">How to include a data profile when registering a data source</span></span>
<span data-ttu-id="0651a-127">Det är enkelt att inkludera en profil av datakällan.</span><span class="sxs-lookup"><span data-stu-id="0651a-127">It's easy to include a profile of your data source.</span></span> <span data-ttu-id="0651a-128">När du registrerar en datakälla i den **objekt som ska registreras** panelen registreringsverktyget för datakällor väljer **inkludera Data profil**.</span><span class="sxs-lookup"><span data-stu-id="0651a-128">When you register a data source, in the **Objects to be registered** panel of the data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="0651a-129">Mer information om hur du registrerar datakällor finns [att registrera datakällor](data-catalog-how-to-register.md) och [Kom igång med Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0651a-129">To learn more about how to register data sources, see [How to register data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="0651a-130">Filtrering på datatillgångar som innehåller data profiler</span><span class="sxs-lookup"><span data-stu-id="0651a-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="0651a-131">För att identifiera datatillgångar som innehåller en data-profil, kan du inkludera `has:tableDataProfiles` eller `has:columnsDataProfiles` som en av dina sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="0651a-131">To discover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="0651a-132">Att välja **inkludera Data profil** i datakällan registreringsverktyget innehåller både tabell och på kolumnnivå profilinformation.</span><span class="sxs-lookup"><span data-stu-id="0651a-132">Selecting **Include Data Profile** in the data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="0651a-133">Data Catalog-API: et kan dock datatillgångar registreras med bara en uppsättning profilinformation som ingår.</span><span class="sxs-lookup"><span data-stu-id="0651a-133">However, the Data Catalog API allows data assets to be registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="0651a-134">Visa information om profilen</span><span class="sxs-lookup"><span data-stu-id="0651a-134">Viewing data profile information</span></span>
<span data-ttu-id="0651a-135">Du kan visa information om profilen när du har hittat en lämplig datakälla med en profil.</span><span class="sxs-lookup"><span data-stu-id="0651a-135">Once you find a suitable data source with a profile, you can view the data profile details.</span></span> <span data-ttu-id="0651a-136">Om du vill visa data profil, markera en datatillgång och välj **Data profil** i fönstret som Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="0651a-136">To view the data profile, select a data asset and choose **Data Profile** in the Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="0651a-137">En data-profil i **Azure Data Catalog** visar tabellen och kolumnen profil information, bland annat:</span><span class="sxs-lookup"><span data-stu-id="0651a-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="0651a-138">Objektet data profil</span><span class="sxs-lookup"><span data-stu-id="0651a-138">Object data profile</span></span>
* <span data-ttu-id="0651a-139">Antal rader</span><span class="sxs-lookup"><span data-stu-id="0651a-139">Number of rows</span></span>
* <span data-ttu-id="0651a-140">Tabellens storlek</span><span class="sxs-lookup"><span data-stu-id="0651a-140">Table size</span></span>
* <span data-ttu-id="0651a-141">Då objektet senast uppdaterades</span><span class="sxs-lookup"><span data-stu-id="0651a-141">When the object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="0651a-142">Kolumnen data profil</span><span class="sxs-lookup"><span data-stu-id="0651a-142">Column data profile</span></span>
* <span data-ttu-id="0651a-143">Kolumnens datatyp</span><span class="sxs-lookup"><span data-stu-id="0651a-143">Column data type</span></span>
* <span data-ttu-id="0651a-144">Antalet distinkta värden</span><span class="sxs-lookup"><span data-stu-id="0651a-144">Number of distinct values</span></span>
* <span data-ttu-id="0651a-145">Antal rader med NULL-värden</span><span class="sxs-lookup"><span data-stu-id="0651a-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="0651a-146">Lägsta, högsta, medelvärde och standardavvikelsen för kolumnvärden</span><span class="sxs-lookup"><span data-stu-id="0651a-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="0651a-147">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0651a-147">Summary</span></span>
<span data-ttu-id="0651a-148">Data profilering ger statistik och information om registrerade datatillgångar för att bestämma lämpligheten av informationen som ska lösa affärsproblem.</span><span class="sxs-lookup"><span data-stu-id="0651a-148">Data profiling provides statistics and information about registered data assets to help you determine the suitability of the data to solve business problems.</span></span> <span data-ttu-id="0651a-149">Tillsammans med kommentera och dokumentera datakällor, kan data-profiler ge användare en bättre förståelse av dina data.</span><span class="sxs-lookup"><span data-stu-id="0651a-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="0651a-150">Se även</span><span class="sxs-lookup"><span data-stu-id="0651a-150">See Also</span></span>
* [<span data-ttu-id="0651a-151">Så här registrerar du datakällor</span><span class="sxs-lookup"><span data-stu-id="0651a-151">How to register data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="0651a-152">Kom igång med Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0651a-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
