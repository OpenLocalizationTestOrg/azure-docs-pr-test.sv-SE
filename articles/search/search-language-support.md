---
title: "aaaAzure Sök flera språk | Microsoft Docs"
description: "Azure Search stöder 56 språk utnyttja språkanalys från Lucene och bearbetning av naturligt språk teknik från Microsoft."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: 9a2e567a82ee563521c12ea320f6c484a8e73f04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a><span data-ttu-id="68db2-103">Skapa ett index för dokument på flera språk i Azure Search</span><span class="sxs-lookup"><span data-stu-id="68db2-103">Create an index for documents in multiple languages in Azure Search</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="68db2-104">Portal</span><span class="sxs-lookup"><span data-stu-id="68db2-104">Portal</span></span>](search-language-support.md)
> * [<span data-ttu-id="68db2-105">REST</span><span class="sxs-lookup"><span data-stu-id="68db2-105">REST</span></span>](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [<span data-ttu-id="68db2-106">.NET</span><span class="sxs-lookup"><span data-stu-id="68db2-106">.NET</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

<span data-ttu-id="68db2-107">Unleashing hello kraften hos språkanalys är så enkel som en egenskap på ett sökbara fält i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="68db2-107">Unleashing hello power of language analyzers is as easy as setting one property on a searchable field in hello index definition.</span></span> <span data-ttu-id="68db2-108">Nu kan du göra det här steget i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="68db2-108">Now you can do this step in hello portal.</span></span>

<span data-ttu-id="68db2-109">Nedan visas skärmdumpar av hello Azure Portal, blad för Azure Search som tillåter att användare toodefine en indexeringsschema.</span><span class="sxs-lookup"><span data-stu-id="68db2-109">Below are screenshots of hello Azure Portal blades for Azure Search that allow users toodefine an index schema.</span></span> <span data-ttu-id="68db2-110">Användare kan skapa alla hello fält och ange hello analyzer-egenskapen för dem från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="68db2-110">From this blade, users can create all of hello fields and set hello analyzer property for each of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68db2-111">Du kan bara ange en språk analyzer under fältdefinition, som i när du skapar ett nytt index från hello bakgrund eller när du lägger till ett nytt fält tooan befintliga index.</span><span class="sxs-lookup"><span data-stu-id="68db2-111">You can only set a language analyzer during field definition, as in when creating a new index from hello ground up, or when adding a new field tooan existing index.</span></span> <span data-ttu-id="68db2-112">Kontrollera att du anger fullständigt alla attribut, inklusive hello analyzer när du skapar hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="68db2-112">Make sure you fully specify all attributes, including hello analyzer, while creating hello field.</span></span> <span data-ttu-id="68db2-113">Du kommer inte att kunna tooedit hello attribut eller ändra hello analyzer typ när du sparar ändringarna.</span><span class="sxs-lookup"><span data-stu-id="68db2-113">You won't be able tooedit hello attributes or change hello analyzer type once you save your changes.</span></span>
>
>

## <a name="define-a-new-field-definition"></a><span data-ttu-id="68db2-114">Definiera en ny fältdefinition</span><span class="sxs-lookup"><span data-stu-id="68db2-114">Define a new field definition</span></span>
1. <span data-ttu-id="68db2-115">Logga in toohello [Azure-portalen](https://portal.azure.com) och öppna hello service bladet för din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="68db2-115">Sign in toohello [Azure portal](https://portal.azure.com) and open hello service blade of your search service.</span></span>
2. <span data-ttu-id="68db2-116">Klicka på **Lägg till index** menyraden överst hello i hello service instrumentpanelen toostart ett nytt index eller öppna ett befintligt index tooset en analyzer på nya fält som du vill lägga till i hello kommandot tooan befintligt index.</span><span class="sxs-lookup"><span data-stu-id="68db2-116">Click **Add index** in hello command bar at hello top of hello service dashboard toostart a new index, or open an existing index tooset an analyzer on new fields you're adding tooan existing index.</span></span>
3. <span data-ttu-id="68db2-117">hello fält bladet visas med alternativ för att definiera hello schemat för hello index, inklusive hello Analyzer fliken används för att välja ett språk analyzer.</span><span class="sxs-lookup"><span data-stu-id="68db2-117">hello Fields blade appears, giving you options for defining hello schema of hello index, including hello Analyzer tab used for choosing a language analyzer.</span></span>
4. <span data-ttu-id="68db2-118">Starta en fältdefinition av i fälten genom att ange ett namn, väljer hello datatyp och anger attributen toomark hello fält som fulltext sökbara, strängfält i sökresultat kan användas i aspekten navigeringsstrukturer kan sorteras och så vidare.</span><span class="sxs-lookup"><span data-stu-id="68db2-118">In Fields, start a field definition by providing a name, choosing hello data type, and setting attributes toomark hello field as full text searchable, retrievable in search results, usable in facet navigation structures, sortable, and so forth.</span></span>
5. <span data-ttu-id="68db2-119">Innan du fortsätter toohello nästa fält, öppna hello **Analyzer** fliken.</span><span class="sxs-lookup"><span data-stu-id="68db2-119">Before moving on toohello next field, open hello **Analyzer** tab.</span></span>

<span data-ttu-id="68db2-120">![][1]
*tooselect en analyzer fliken hello Analyzer på bladet för hello-fält*</span><span class="sxs-lookup"><span data-stu-id="68db2-120">![][1]
*tooselect an analyzer, click hello Analyzer tab on hello Fields blade*</span></span>

## <a name="choose-an-analyzer"></a><span data-ttu-id="68db2-121">Välj en analyzer</span><span class="sxs-lookup"><span data-stu-id="68db2-121">Choose an analyzer</span></span>
1. <span data-ttu-id="68db2-122">Rulla toofind hello fält som du definierar.</span><span class="sxs-lookup"><span data-stu-id="68db2-122">Scroll toofind hello field you are defining.</span></span>
2. <span data-ttu-id="68db2-123">Om du inte har markerats hello fältet sökbart, klickar du på hello kryssrutan nu toomark som **sökbara**.</span><span class="sxs-lookup"><span data-stu-id="68db2-123">If you haven't marked hello field as searchable, click hello checkbox now toomark it as **Searchable**.</span></span>
3. <span data-ttu-id="68db2-124">Klicka på hello Analyzer området toodisplay hello lista över tillgängliga analyzers.</span><span class="sxs-lookup"><span data-stu-id="68db2-124">Click hello Analyzer area toodisplay hello list of available analyzers.</span></span>
4. <span data-ttu-id="68db2-125">Välj hello analyzer du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="68db2-125">Choose hello analyzer you want toouse.</span></span>

<span data-ttu-id="68db2-126">![][2]
*Välj något av hello stöds analyzers för varje fält*</span><span class="sxs-lookup"><span data-stu-id="68db2-126">![][2]
*Select one of hello supported analyzers for each field*</span></span>

<span data-ttu-id="68db2-127">Som standard använder alla sökbara fält hello [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) som är oberoende av språk.</span><span class="sxs-lookup"><span data-stu-id="68db2-127">By default, all searchable fields use hello [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) which is language agnostic.</span></span> <span data-ttu-id="68db2-128">tooview hello fullständig lista över stöds analyzers, se [språkstöd i Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span><span class="sxs-lookup"><span data-stu-id="68db2-128">tooview hello full list of supported analyzers, see [Language Support in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span></span>

<span data-ttu-id="68db2-129">När hello språk analyzer har valts för ett fält, ska det användas med varje fulltextindexering och begäran för det här fältet.</span><span class="sxs-lookup"><span data-stu-id="68db2-129">Once hello language analyzer is selected for a field, it will be used with each indexing and search request for that field.</span></span> <span data-ttu-id="68db2-130">När en fråga skickas ut mot flera fält med olika analyzers kommer hello frågan att behandlas oberoende av hello rätt analyzers för varje fält.</span><span class="sxs-lookup"><span data-stu-id="68db2-130">When a query is issued against multiple fields using different analyzers, hello query will be processed independently by hello right analyzers for each field.</span></span>

<span data-ttu-id="68db2-131">Många webb- och mobilprogram kan du hantera användare runt hello världen med olika språk.</span><span class="sxs-lookup"><span data-stu-id="68db2-131">Many web and mobile applications serve users around hello globe using different languages.</span></span> <span data-ttu-id="68db2-132">Det är möjligt toodefine ett index för ett scenario som detta genom att skapa ett fält för varje språk som stöds.</span><span class="sxs-lookup"><span data-stu-id="68db2-132">It’s possible toodefine an index for a scenario like this by creating a field for each language supported.</span></span>

<span data-ttu-id="68db2-133">![][3]
*Indexdefinitionen med en beskrivningsfält för varje språk som stöds*</span><span class="sxs-lookup"><span data-stu-id="68db2-133">![][3]
*Index definition with a description field for each language supported*</span></span>

<span data-ttu-id="68db2-134">Om hello språket för hello agenten skickar en fråga kallas en sökbegäran kan vara begränsade tooa specifika fält med hello **searchFields** Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="68db2-134">If hello language of hello agent issuing a query is known, a search request can be scoped tooa specific field using hello **searchFields** query parameter.</span></span> <span data-ttu-id="68db2-135">hello utfärdas följande fråga endast mot hello beskrivning i polska:</span><span class="sxs-lookup"><span data-stu-id="68db2-135">hello following query will be issued only against hello description in Polish:</span></span>

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

<span data-ttu-id="68db2-136">Du kan fråga ditt index från hello-portalen med hjälp av **Sök explorer** toopaste i en fråga liknande toohello visas ovan.</span><span class="sxs-lookup"><span data-stu-id="68db2-136">You can query your index from hello portal, using **Search explorer** toopaste in a query similar toohello one shown above.</span></span> <span data-ttu-id="68db2-137">Sök explorer är tillgänglig från hello kommandofältet hello service-bladet.</span><span class="sxs-lookup"><span data-stu-id="68db2-137">Search explorer is available from hello command bar in hello service blade.</span></span> <span data-ttu-id="68db2-138">Se [fråga ditt Azure Search-index i hello portal](search-explorer.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="68db2-138">See [Query your Azure Search index in hello portal](search-explorer.md) for details.</span></span>

<span data-ttu-id="68db2-139">Ibland är hello språket för hello agenten skickar en fråga Okänd, i vilket fall hello fråga kan utfärdas mot alla fält samtidigt.</span><span class="sxs-lookup"><span data-stu-id="68db2-139">Sometimes hello language of hello agent issuing a query is not known, in which case hello query can be issued against all fields simultaneously.</span></span> <span data-ttu-id="68db2-140">Om det behövs, inställningar för resulterar i ett visst språk kan definieras med [bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="68db2-140">If needed, preference for results in a certain language can be defined using [scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span> <span data-ttu-id="68db2-141">Matchningar i hello beskrivning på engelska bedömas i hello exemplet nedan högre relativa toomatches på polska och franska:</span><span class="sxs-lookup"><span data-stu-id="68db2-141">In hello example below, matches found in hello description in English will be scored higher relative toomatches in Polish and French:</span></span>

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

<span data-ttu-id="68db2-142">Om du är en .NET-utvecklare, Observera att du kan konfigurera språkanalys med hello [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span><span class="sxs-lookup"><span data-stu-id="68db2-142">If you're a .NET developer, note that you can configure language analyzers using hello [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span></span> <span data-ttu-id="68db2-143">senaste hello-versionen har stöd för hello Microsoft språkanalys samt.</span><span class="sxs-lookup"><span data-stu-id="68db2-143">hello latest release includes support for hello Microsoft language analyzers as well.</span></span>

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
