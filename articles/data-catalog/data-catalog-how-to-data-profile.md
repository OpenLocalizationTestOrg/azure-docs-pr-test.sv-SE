---
title: "aaaHow tooData profil datakällor"
description: "Hur-tooarticle syntaxmarkering hur tooinclude tabell - och på kolumnnivå data profiler när du registrerar datakällor i Azure Data Catalog och hur data toouse profiler toounderstand datakällor."
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
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="030df-103">Datakällor för dataprofil</span><span class="sxs-lookup"><span data-stu-id="030df-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="030df-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="030df-104">Introduction</span></span>
<span data-ttu-id="030df-105">**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="030df-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="030df-106">Med andra ord **Azure Data Catalog** är allt om hjälpa personer identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga data.</span><span class="sxs-lookup"><span data-stu-id="030df-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data.</span></span> <span data-ttu-id="030df-107">När en datakälla har registrerats med **Azure Data Catalog**, dess metadata kopieras och indexeras av hello service, men hello artikeln inte det avslutas.</span><span class="sxs-lookup"><span data-stu-id="030df-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span>

<span data-ttu-id="030df-108">Hej **Data profilering** funktion i **Azure Data Catalog** undersöker hello data från datakällor som stöds i katalogen och samlar in information om dessa data och statistik.</span><span class="sxs-lookup"><span data-stu-id="030df-108">hello **Data Profiling** feature of **Azure Data Catalog** examines hello data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="030df-109">Det är enkelt tooinclude en profil för dina datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="030df-109">It's easy tooinclude a profile of your data assets.</span></span> <span data-ttu-id="030df-110">När du registrerar en datatillgång, välja **inkludera Data profil** i hello registreringsverktyget för datakällor.</span><span class="sxs-lookup"><span data-stu-id="030df-110">When you register a data asset, choose **Include Data Profile** in hello data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="030df-111">Vad är profilering av Data</span><span class="sxs-lookup"><span data-stu-id="030df-111">What is Data Profiling</span></span>
<span data-ttu-id="030df-112">Data profilering undersöker hello data i hello datakällan som registreras och samlar in information om dessa data och statistik.</span><span class="sxs-lookup"><span data-stu-id="030df-112">Data profiling examines hello data in hello data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="030df-113">Datakällsidentifiering, kan statistiken hjälpa dig att avgöra hello lämplighet hello data toosolve sina affärsproblem.</span><span class="sxs-lookup"><span data-stu-id="030df-113">During data source discovery, these statistics can help you determine hello suitability of hello data toosolve their business problem.</span></span>

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

<span data-ttu-id="030df-114">hello stöder följande datakällor profilering av data:</span><span class="sxs-lookup"><span data-stu-id="030df-114">hello following data sources support data profiling:</span></span>

* <span data-ttu-id="030df-115">SQL Server (inklusive Azure SQL DB och Azure SQL Data Warehouse) tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="030df-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="030df-116">Oracle-tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="030df-116">Oracle tables and views</span></span>
* <span data-ttu-id="030df-117">Teradata-tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="030df-117">Teradata tables and views</span></span>
* <span data-ttu-id="030df-118">Hive-tabeller</span><span class="sxs-lookup"><span data-stu-id="030df-118">Hive tables</span></span>

<span data-ttu-id="030df-119">Inklusive profiler data när du registrerar datatillgångar hjälper användarna att besvara frågor om datakällor, inklusive:</span><span class="sxs-lookup"><span data-stu-id="030df-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="030df-120">Kan det vara används toosolve mitt företag problem?</span><span class="sxs-lookup"><span data-stu-id="030df-120">Can it be used toosolve my business problem?</span></span>
* <span data-ttu-id="030df-121">Överensstämmer hello data tooparticular standarder eller mönster?</span><span class="sxs-lookup"><span data-stu-id="030df-121">Does hello data conform tooparticular standards or patterns?</span></span>
* <span data-ttu-id="030df-122">Vilka är några av hello avvikelser i hello-datakällan?</span><span class="sxs-lookup"><span data-stu-id="030df-122">What are some of hello anomalies of hello data source?</span></span>
* <span data-ttu-id="030df-123">Vad är möjliga utmaningarna med att integrera informationen i mitt program?</span><span class="sxs-lookup"><span data-stu-id="030df-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="030df-124">Du kan också lägga till dokumentationen tooan tillgången toodescribe hur data kan integreras i ett program.</span><span class="sxs-lookup"><span data-stu-id="030df-124">You can also add documentation tooan asset toodescribe how data could be integrated into an application.</span></span> <span data-ttu-id="030df-125">Se [hur toodocument datakällor](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="030df-125">See [How toodocument data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="030df-126">Hur tooinclude data profilen när du registrerar en datakälla</span><span class="sxs-lookup"><span data-stu-id="030df-126">How tooinclude a data profile when registering a data source</span></span>
<span data-ttu-id="030df-127">Det är enkelt tooinclude en profil av datakällan.</span><span class="sxs-lookup"><span data-stu-id="030df-127">It's easy tooinclude a profile of your data source.</span></span> <span data-ttu-id="030df-128">När du registrerar en datakälla i hello **objekt toobe registrerade** panelen av hello registrering av datakälla verktyget, välja **inkludera Data profil**.</span><span class="sxs-lookup"><span data-stu-id="030df-128">When you register a data source, in hello **Objects toobe registered** panel of hello data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="030df-129">toolearn mer om hur tooregister datakällor, se [hur tooregister datakällor](data-catalog-how-to-register.md) och [Kom igång med Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="030df-129">toolearn more about how tooregister data sources, see [How tooregister data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="030df-130">Filtrering på datatillgångar som innehåller data profiler</span><span class="sxs-lookup"><span data-stu-id="030df-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="030df-131">toodiscover datatillgångar som innehåller en data-profil, som du kan inkludera `has:tableDataProfiles` eller `has:columnsDataProfiles` som en av dina sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="030df-131">toodiscover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="030df-132">Att välja **inkludera Data profil** i hello data registreringsverktyget för datakällor innehåller både tabell och på kolumnnivå profilinformation.</span><span class="sxs-lookup"><span data-stu-id="030df-132">Selecting **Include Data Profile** in hello data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="030df-133">Hello kan Data Catalog-API: et dock data tillgångar toobe registrerats med bara en uppsättning profilinformation som ingår.</span><span class="sxs-lookup"><span data-stu-id="030df-133">However, hello Data Catalog API allows data assets toobe registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="030df-134">Visa information om profilen</span><span class="sxs-lookup"><span data-stu-id="030df-134">Viewing data profile information</span></span>
<span data-ttu-id="030df-135">När du har hittat en lämplig datakälla med en profil kan visa du hello information om data.</span><span class="sxs-lookup"><span data-stu-id="030df-135">Once you find a suitable data source with a profile, you can view hello data profile details.</span></span> <span data-ttu-id="030df-136">tooview hello data profil, Välj en datatillgång och välj **Data profil** i portalen hello Data Catalog-fönstret.</span><span class="sxs-lookup"><span data-stu-id="030df-136">tooview hello data profile, select a data asset and choose **Data Profile** in hello Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="030df-137">En data-profil i **Azure Data Catalog** visar tabellen och kolumnen profil information, bland annat:</span><span class="sxs-lookup"><span data-stu-id="030df-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="030df-138">Objektet data profil</span><span class="sxs-lookup"><span data-stu-id="030df-138">Object data profile</span></span>
* <span data-ttu-id="030df-139">Antal rader</span><span class="sxs-lookup"><span data-stu-id="030df-139">Number of rows</span></span>
* <span data-ttu-id="030df-140">Tabellens storlek</span><span class="sxs-lookup"><span data-stu-id="030df-140">Table size</span></span>
* <span data-ttu-id="030df-141">När hello objekt uppdaterades senast</span><span class="sxs-lookup"><span data-stu-id="030df-141">When hello object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="030df-142">Kolumnen data profil</span><span class="sxs-lookup"><span data-stu-id="030df-142">Column data profile</span></span>
* <span data-ttu-id="030df-143">Kolumnens datatyp</span><span class="sxs-lookup"><span data-stu-id="030df-143">Column data type</span></span>
* <span data-ttu-id="030df-144">Antalet distinkta värden</span><span class="sxs-lookup"><span data-stu-id="030df-144">Number of distinct values</span></span>
* <span data-ttu-id="030df-145">Antal rader med NULL-värden</span><span class="sxs-lookup"><span data-stu-id="030df-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="030df-146">Lägsta, högsta, medelvärde och standardavvikelsen för kolumnvärden</span><span class="sxs-lookup"><span data-stu-id="030df-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="030df-147">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="030df-147">Summary</span></span>
<span data-ttu-id="030df-148">Profilering av data innehåller statistik och information om registrerade data tillgångar toohelp du fastställa hello lämplighet hello data toosolve affärsproblem.</span><span class="sxs-lookup"><span data-stu-id="030df-148">Data profiling provides statistics and information about registered data assets toohelp you determine hello suitability of hello data toosolve business problems.</span></span> <span data-ttu-id="030df-149">Tillsammans med kommentera och dokumentera datakällor, kan data-profiler ge användare en bättre förståelse av dina data.</span><span class="sxs-lookup"><span data-stu-id="030df-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="030df-150">Se även</span><span class="sxs-lookup"><span data-stu-id="030df-150">See Also</span></span>
* [<span data-ttu-id="030df-151">Hur tooregister datakällor</span><span class="sxs-lookup"><span data-stu-id="030df-151">How tooregister data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="030df-152">Kom igång med Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="030df-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
