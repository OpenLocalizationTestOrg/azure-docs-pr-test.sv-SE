---
title: "Så här dokumenterar du datakällor | Microsoft Docs"
description: "Artikel syntaxmarkering så här dokumenterar du datatillgångar i Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: ffe951f60afb57524568fe1ed3b3668d0088e124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="6a051-103">Dokumentera datakällor</span><span class="sxs-lookup"><span data-stu-id="6a051-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="6a051-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6a051-104">Introduction</span></span>
<span data-ttu-id="6a051-105">**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="6a051-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="6a051-106">Med andra ord **Azure Data Catalog** handlar om hjälper människor identifiera, *förstå*, och använda datakällor och hjälper organisationer att få ut mer av sina befintliga data.</span><span class="sxs-lookup"><span data-stu-id="6a051-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations to get more value from their existing data.</span></span>

<span data-ttu-id="6a051-107">När en datakälla har registrerats med **Azure Data Catalog**, dess metadata kopieras och indexeras av tjänsten, men artikeln inte det avslutas.</span><span class="sxs-lookup"><span data-stu-id="6a051-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span> <span data-ttu-id="6a051-108">**Azure Data Catalog** dessutom tillåter användarna att ange sina egna komplett dokumentation som kan beskriver användnings- och vanliga scenarier för datakällan.</span><span class="sxs-lookup"><span data-stu-id="6a051-108">**Azure Data Catalog** also allows users to provide their own complete documentation that can describe the usage and common scenarios for the data source.</span></span>

<span data-ttu-id="6a051-109">I [så här kommenterar du datakällor](data-catalog-how-to-annotate.md), du lära dig att experter som vet datakällan kan kommentera det med taggar och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="6a051-109">In [How to annotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know the data source can annotate it with tags and a description.</span></span> <span data-ttu-id="6a051-110">Den **Azure Data Catalog** portal innehåller en textredigerare så att användare kan fullständigt dokumentera datatillgångar och behållare.</span><span class="sxs-lookup"><span data-stu-id="6a051-110">The **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="6a051-111">Redigeraren innehåller styckeformatering, till exempel rubriker, textformatering, punktlista, numrerade listor och tabeller.</span><span class="sxs-lookup"><span data-stu-id="6a051-111">The editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="6a051-112">Taggar och beskrivningar är perfekt för enkla anteckningar.</span><span class="sxs-lookup"><span data-stu-id="6a051-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="6a051-113">För att bättre förstå användningen av en datakälla och affärsscenarier för en datakälla för datakonsumenterna kan dock en expert tillhandahålla fullständig, detaljerad dokumentation.</span><span class="sxs-lookup"><span data-stu-id="6a051-113">However, to help data consumers better understand the use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="6a051-114">Det är enkelt att dokumentera en datakälla.</span><span class="sxs-lookup"><span data-stu-id="6a051-114">It's easy to document a data source.</span></span> <span data-ttu-id="6a051-115">Välj en datatillgång eller behållaren och välj **dokumentationen**.</span><span class="sxs-lookup"><span data-stu-id="6a051-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="6a051-116">Dokumentera datatillgångar</span><span class="sxs-lookup"><span data-stu-id="6a051-116">Documenting data assets</span></span>
<span data-ttu-id="6a051-117">Fördelen med **Azure Data Catalog** dokumentationen kan du använda Data Catalog som en innehållsdatabas för att skapa en fullständig berättelsen för dina datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="6a051-117">The benefit of **Azure Data Catalog** documentation allows you to use your Data Catalog as a content repository to create a complete narrative of your data assets.</span></span> <span data-ttu-id="6a051-118">Du kan utforska detaljerad innehåll som beskriver behållare och tabeller.</span><span class="sxs-lookup"><span data-stu-id="6a051-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="6a051-119">Om du redan har innehåll i en annan innehållsdatabasen, till exempel SharePoint eller en filresurs, kan du lägga till tillgången dokumentationen länkar till den här befintligt innehåll.</span><span class="sxs-lookup"><span data-stu-id="6a051-119">If you already have content in another content repository, such as SharePoint or a file share, you can add to the asset documentation links to reference this existing content.</span></span> <span data-ttu-id="6a051-120">Den här funktionen gör det lättare att hitta din befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="6a051-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="6a051-121">Dokumentationen ingår inte i sökindex.</span><span class="sxs-lookup"><span data-stu-id="6a051-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="6a051-122">Nivån av dokumentationen kan variera mellan som beskriver egenskaper och värdet för en tillgång behållare för data att en detaljerad beskrivning av tabellens schema i en behållare.</span><span class="sxs-lookup"><span data-stu-id="6a051-122">The level of documentation can range from describing the characteristics and value of a data asset container to a detailed description of table schema within a container.</span></span> <span data-ttu-id="6a051-123">Nivån av dokumentationen bör styrs av dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="6a051-123">The level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="6a051-124">Men i allmänhet följer här några- och nackdelar med dokumentera datatillgångar:</span><span class="sxs-lookup"><span data-stu-id="6a051-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="6a051-125">Dokumentera bara en behållare: allt innehåll på en plats, men saknar nödvändiga information för användare att fatta ett välgrundat beslut.</span><span class="sxs-lookup"><span data-stu-id="6a051-125">Document just a container: All the content is in one place, but might lack necessary details for users to make an informed decision.</span></span>
* <span data-ttu-id="6a051-126">Dokumentera bara tabellerna: innehåll som är specifik för objektet, men användarna har flera platser för dokument.</span><span class="sxs-lookup"><span data-stu-id="6a051-126">Document just the tables: Content is specific to that object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="6a051-127">Dokumentera behållare och tabeller: mest omfattande metod, men kan leda till flera underhåll av dokumenten.</span><span class="sxs-lookup"><span data-stu-id="6a051-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of the documents.</span></span>

## <a name="summary"></a><span data-ttu-id="6a051-128">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6a051-128">Summary</span></span>
<span data-ttu-id="6a051-129">Dokumentera datakällor med **Azure Data Catalog** kan skapa en berättelsen om dina datatillgångar i så mycket information som du vill.</span><span class="sxs-lookup"><span data-stu-id="6a051-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="6a051-130">Du kan länka till innehåll som lagras i en befintlig innehåll databas som sammanför din befintliga dokumenten och datatillgångar med hjälp av länkar.</span><span class="sxs-lookup"><span data-stu-id="6a051-130">By using links, you can link to content stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="6a051-131">När användarna identifiera lämpliga datatillgångar, har de en fullständig uppsättning dokumentation.</span><span class="sxs-lookup"><span data-stu-id="6a051-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
