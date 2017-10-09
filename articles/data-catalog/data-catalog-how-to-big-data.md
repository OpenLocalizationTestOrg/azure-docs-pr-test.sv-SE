---
title: "aaaHow toowork med 'stordata' datakällor | Microsoft Docs"
description: "Hur tooarticle färgmarkera mönster för att använda Azure Data Catalog med 'stordata' datakällor, inklusive Azure Blob Storage, Azure Data Lake och Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a><span data-ttu-id="d6e7d-103">Hur toowork med stordata datakällor i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="d6e7d-103">How toowork with big data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="d6e7d-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d6e7d-104">Introduction</span></span>
<span data-ttu-id="d6e7d-105">**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="d6e7d-106">Allt handlar om hjälper människor identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga datakällor, inklusive stordata.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-106">It is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data sources, including big data.</span></span>

<span data-ttu-id="d6e7d-107">**Azure Data Catalog** stöder hello registrering av Azure-bloggen Storage-blobbar och samt Hadoop HDFS filer och kataloger.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-107">**Azure Data Catalog** supports hello registration of Azure Blog Storage blobs and directories as well as Hadoop HDFS files and directories.</span></span> <span data-ttu-id="d6e7d-108">hello halvstrukturerade arten av de här datakällorna ger stor flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-108">hello semi-structured nature of these data sources provides great flexibility.</span></span> <span data-ttu-id="d6e7d-109">Dock tooget hello största möjliga värde från att registrera dem med **Azure Data Catalog**, användare måste ta hänsyn till hur hello datakällor är ordnade.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-109">However, tooget hello most value from registering them with **Azure Data Catalog**, users must consider how hello data sources are organized.</span></span>

## <a name="directories-as-logical-data-sets"></a><span data-ttu-id="d6e7d-110">Kataloger som logiska datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="d6e7d-110">Directories as logical data sets</span></span>
<span data-ttu-id="d6e7d-111">Vanliga mönstret för att ordna stora datakällor är tootreat kataloger som logiska datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-111">A common pattern for organizing big data sources is tootreat directories as logical data sets.</span></span> <span data-ttu-id="d6e7d-112">Översta kataloger är används toodefine en datamängd, medan undermappar definiera partitioner och hello-filer som de innehåller datalagring hello sig själv.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-112">Top-level directories are used toodefine a data set, while subfolders define partitions, and hello files they contain store hello data itself.</span></span>

<span data-ttu-id="d6e7d-113">Ett exempel på det här mönstret kan vara:</span><span class="sxs-lookup"><span data-stu-id="d6e7d-113">An example of this pattern might be:</span></span>

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

<span data-ttu-id="d6e7d-114">I det här exemplet representerar vehicle_maintenance_events och location_tracking_events logiska datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-114">In this example, vehicle_maintenance_events and location_tracking_events represent logical data sets.</span></span> <span data-ttu-id="d6e7d-115">Var och en av mapparna innehåller filer som är ordnad efter år och månad i undermappar.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-115">Each of these folders contains data files that are organized by year and month into subfolders.</span></span> <span data-ttu-id="d6e7d-116">Var och en av dessa mappar kan innehålla hundratals eller tusentals filer.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-116">Each of these folders could potentially contain hundreds or thousands of files.</span></span>

<span data-ttu-id="d6e7d-117">I det här mönstret registrering av enskilda filer med **Azure Data Catalog** förmodligen inte vara meningsfullt.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-117">In this pattern, registering individual files with **Azure Data Catalog** probably does not make sense.</span></span> <span data-ttu-id="d6e7d-118">I stället registrera hello kataloger som representerar hello datauppsättningar som vara meningsfullt toohello användare arbetar med hello data.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-118">Instead, register hello directories that represent hello data sets that be meaningful toohello users working with hello data.</span></span>

## <a name="reference-data-files"></a><span data-ttu-id="d6e7d-119">Referens-datafiler</span><span class="sxs-lookup"><span data-stu-id="d6e7d-119">Reference data files</span></span>
<span data-ttu-id="d6e7d-120">En kompletterande mönstret är toostore referensdatamängder som enskilda filer.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-120">A complementary pattern is toostore reference data sets as individual files.</span></span> <span data-ttu-id="d6e7d-121">Dessa datauppsättningar kan betraktas som ”liten” hello-sida av stordata och är ofta liknande toodimensions i en modell med analysdata.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-121">These data sets may be thought of as hello 'small' side of big data, and are often similar toodimensions in an analytical data model.</span></span> <span data-ttu-id="d6e7d-122">Referens-datafiler innehåller poster som används tooprovide kontext för hello huvuddelen av hello datafiler lagrade på andra ställen i hello stordataarkiv.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-122">Reference data files contain records that are used tooprovide context for hello bulk of hello data files stored elsewhere in hello big data store.</span></span>

<span data-ttu-id="d6e7d-123">Ett exempel på det här mönstret kan vara:</span><span class="sxs-lookup"><span data-stu-id="d6e7d-123">An example of this pattern might be:</span></span>

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

<span data-ttu-id="d6e7d-124">När en analytiker eller data forskare fungerar med hello uppgifterna i hello större katalogstrukturer hello data i dessa filer kan det vara används tooprovide mer detaljerad information för enheter som kallas tooonly efter namn eller ID i hello större data Ange.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-124">When an analyst or data scientist is working with hello data contained in hello larger directory structures, hello data in these reference files can be used tooprovide more detailed information for entities that are referred tooonly by name or ID in hello larger data set.</span></span>

<span data-ttu-id="d6e7d-125">I det här mönstret är det klokt tooregister hello referens för enskilda filer med **Azure Data Catalog**.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-125">In this pattern, it makes sense tooregister hello individual reference data files with **Azure Data Catalog**.</span></span> <span data-ttu-id="d6e7d-126">Representerar en datauppsättning för varje fil och var och en kan kommenterats och identifierade individuellt.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-126">Each file represents a data set, and each one can be annotated and discovered individually.</span></span>

## <a name="alternate-patterns"></a><span data-ttu-id="d6e7d-127">Alternativa mönster</span><span class="sxs-lookup"><span data-stu-id="d6e7d-127">Alternate patterns</span></span>
<span data-ttu-id="d6e7d-128">hello mönster som beskrivs i föregående avsnitt hello finns två möjliga sätt en stordataarkiv kan ordnas, men varje implementering är olika.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-128">hello patterns described in hello preceding section are just two possible ways a big data store may be organized, but each implementation is different.</span></span> <span data-ttu-id="d6e7d-129">Oavsett hur dina datakällor är strukturerade, när du registrerar ett stort datakällor med **Azure Data Catalog**, fokusera på registrera hello filer och kataloger som representerar hello datauppsättningar av värdet tooothers inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-129">Regardless of how your data sources are structured, when registering big data sources with **Azure Data Catalog**, focus on registering hello files and directories that represent hello data sets that are of value tooothers within your organization.</span></span> <span data-ttu-id="d6e7d-130">Registrerar alla filer och kataloger kan göra hello-katalogen, vilket gör det svårare för användare toofind de behöver.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-130">Registering all files and directories can clutter hello catalog, making it harder for users toofind what they need.</span></span>

## <a name="summary"></a><span data-ttu-id="d6e7d-131">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d6e7d-131">Summary</span></span>
<span data-ttu-id="d6e7d-132">Registrera datakällor med **Azure Data Catalog** gör dem lättare toodiscover och förstå.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-132">Registering data sources with **Azure Data Catalog** makes them easier toodiscover and understand.</span></span> <span data-ttu-id="d6e7d-133">Genom att registrera och kommentera hello stordata filer och kataloger som representerar logiska datauppsättningar, kan du hjälpa användarna att hitta och använda hello stort datakällor de behöver.</span><span class="sxs-lookup"><span data-stu-id="d6e7d-133">By registering and annotating hello big data files and directories that represent logical data sets, you can help users find and use hello big data sources they need.</span></span>
