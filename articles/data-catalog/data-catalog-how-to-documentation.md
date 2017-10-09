---
title: "datakällor för aaaHow toodocument | Microsoft Docs"
description: "Hur tooarticle syntaxmarkering hur toodocument datatillgångar i Azure Data Catalog."
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
ms.openlocfilehash: 4e46838d91ab4d0c0bc569ac526a0c729134bb10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="7acc9-103">Dokumentera datakällor</span><span class="sxs-lookup"><span data-stu-id="7acc9-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="7acc9-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="7acc9-104">Introduction</span></span>
<span data-ttu-id="7acc9-105">**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="7acc9-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="7acc9-106">Med andra ord **Azure Data Catalog** handlar om hjälper människor identifiera, *förstå*, och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga data.</span><span class="sxs-lookup"><span data-stu-id="7acc9-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations tooget more value from their existing data.</span></span>

<span data-ttu-id="7acc9-107">När en datakälla har registrerats med **Azure Data Catalog**, dess metadata kopieras och indexeras av hello service, men hello artikeln inte det avslutas.</span><span class="sxs-lookup"><span data-stu-id="7acc9-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span> <span data-ttu-id="7acc9-108">**Azure Data Catalog** kan användare tooprovide sina egna komplett dokumentation som kan beskriver hello användnings- och vanliga scenarier för hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="7acc9-108">**Azure Data Catalog** also allows users tooprovide their own complete documentation that can describe hello usage and common scenarios for hello data source.</span></span>

<span data-ttu-id="7acc9-109">I [hur tooannotate datakällor](data-catalog-how-to-annotate.md), du lära dig att kan kommentera av experter som vet hello-datakälla med taggar och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="7acc9-109">In [How tooannotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know hello data source can annotate it with tags and a description.</span></span> <span data-ttu-id="7acc9-110">Hej **Azure Data Catalog** portal innehåller en textredigerare så att användare kan fullständigt dokumentera datatillgångar och behållare.</span><span class="sxs-lookup"><span data-stu-id="7acc9-110">hello **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="7acc9-111">hello-redigeraren innehåller styckeformatering, till exempel rubriker, formatering, punktlista, numrerade listor och tabeller.</span><span class="sxs-lookup"><span data-stu-id="7acc9-111">hello editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="7acc9-112">Taggar och beskrivningar är perfekt för enkla anteckningar.</span><span class="sxs-lookup"><span data-stu-id="7acc9-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="7acc9-113">Dock toohelp datakonsumenterna bättre förstå hello användning av en datakälla och affärsscenarier för en datakälla för en expert kan ge fullständig och detaljerad dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7acc9-113">However, toohelp data consumers better understand hello use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="7acc9-114">Det är enkelt toodocument en datakälla.</span><span class="sxs-lookup"><span data-stu-id="7acc9-114">It's easy toodocument a data source.</span></span> <span data-ttu-id="7acc9-115">Välj en datatillgång eller behållaren och välj **dokumentationen**.</span><span class="sxs-lookup"><span data-stu-id="7acc9-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="7acc9-116">Dokumentera datatillgångar</span><span class="sxs-lookup"><span data-stu-id="7acc9-116">Documenting data assets</span></span>
<span data-ttu-id="7acc9-117">Hej fördelen **Azure Data Catalog** dokumentationen kan du toouse dina Data Catalog som en innehållsdatabas toocreate en fullständig berättelsen för dina datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="7acc9-117">hello benefit of **Azure Data Catalog** documentation allows you toouse your Data Catalog as a content repository toocreate a complete narrative of your data assets.</span></span> <span data-ttu-id="7acc9-118">Du kan utforska detaljerad innehåll som beskriver behållare och tabeller.</span><span class="sxs-lookup"><span data-stu-id="7acc9-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="7acc9-119">Om du redan har innehåll i en annan innehållsdatabasen, till exempel SharePoint eller en filresurs, du kan lägga till toohello tillgången dokumentationen länkar tooreference befintliga innehållet.</span><span class="sxs-lookup"><span data-stu-id="7acc9-119">If you already have content in another content repository, such as SharePoint or a file share, you can add toohello asset documentation links tooreference this existing content.</span></span> <span data-ttu-id="7acc9-120">Den här funktionen gör det lättare att hitta din befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="7acc9-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="7acc9-121">Dokumentationen ingår inte i sökindex.</span><span class="sxs-lookup"><span data-stu-id="7acc9-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="7acc9-122">hello nivå av dokumentationen kan variera mellan som beskriver hello egenskaper och värde för en tillgång behållaren tooa detaljerad beskrivning av tabellens schema i en behållare.</span><span class="sxs-lookup"><span data-stu-id="7acc9-122">hello level of documentation can range from describing hello characteristics and value of a data asset container tooa detailed description of table schema within a container.</span></span> <span data-ttu-id="7acc9-123">hello andelen dokumentationen bör styrs av dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="7acc9-123">hello level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="7acc9-124">Men i allmänhet följer här några- och nackdelar med dokumentera datatillgångar:</span><span class="sxs-lookup"><span data-stu-id="7acc9-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="7acc9-125">Dokumentera bara en behållare: alla hello innehåll finns i en enda plats, men kanske saknas nödvändiga uppgifter för användare toomake ett välgrundat beslut.</span><span class="sxs-lookup"><span data-stu-id="7acc9-125">Document just a container: All hello content is in one place, but might lack necessary details for users toomake an informed decision.</span></span>
* <span data-ttu-id="7acc9-126">Dokumentera bara hello tabeller: innehåll är särskilda toothat objekt, men användarna har flera platser för dokument.</span><span class="sxs-lookup"><span data-stu-id="7acc9-126">Document just hello tables: Content is specific toothat object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="7acc9-127">Dokumentera behållare och tabeller: mest omfattande metod, men kan leda till flera underhåll av hello dokument.</span><span class="sxs-lookup"><span data-stu-id="7acc9-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of hello documents.</span></span>

## <a name="summary"></a><span data-ttu-id="7acc9-128">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7acc9-128">Summary</span></span>
<span data-ttu-id="7acc9-129">Dokumentera datakällor med **Azure Data Catalog** kan skapa en berättelsen om dina datatillgångar i så mycket information som du vill.</span><span class="sxs-lookup"><span data-stu-id="7acc9-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="7acc9-130">Du kan länka toocontent lagras i en befintlig innehåll databas som sammanför din befintliga dokumenten och datatillgångar med hjälp av länkar.</span><span class="sxs-lookup"><span data-stu-id="7acc9-130">By using links, you can link toocontent stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="7acc9-131">När användarna identifiera lämpliga datatillgångar, har de en fullständig uppsättning dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7acc9-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
