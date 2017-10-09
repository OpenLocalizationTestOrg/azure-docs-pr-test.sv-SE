---
title: aaaHow toomodel komplexa datatyper i Azure Search | Microsoft Docs
description: "Kapslade eller hierarkisk datastrukturer kan utformas i ett Azure Search-index med Flat raduppsättning och samlingar datatyp."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: b330c5b322f4f33123a454be11733b977684b9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomodel-complex-data-types-in-azure-search"></a><span data-ttu-id="0107c-103">Hur toomodel komplexa datatyper i Azure Search</span><span class="sxs-lookup"><span data-stu-id="0107c-103">How toomodel complex data types in Azure Search</span></span>
<span data-ttu-id="0107c-104">Externa datauppsättningar används toopopulate Azure Search index omfattar ibland hierarkisk eller kapslade underordnade strukturer som inte nedbrytning av snyggt i en tabellvy raduppsättning.</span><span class="sxs-lookup"><span data-stu-id="0107c-104">External datasets used toopopulate an Azure Search index sometimes include hierarchical or nested substructures that do not break down neatly into a tabular rowset.</span></span> <span data-ttu-id="0107c-105">Exempel på sådana strukturer kan innehålla flera platser och telefonnummer för en kund, flera färger och storlekar för en enskild SKU flera författare av en enda bok och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0107c-105">Examples of such structures might include multiple locations and phone numbers for a single customer, multiple colors and sizes for a single SKU, multiple authors of a single book, and so on.</span></span> <span data-ttu-id="0107c-106">Modeling villkor du kan se dessa strukturer som anges tooas *komplexa datatyper*, *sammansatta datatyper*, *sammansatta datatyper*, eller *sammanställd datatyper*, tooname ett fåtal.</span><span class="sxs-lookup"><span data-stu-id="0107c-106">In modeling terms, you might see these structures referred tooas *complex data types*, *compound data types*, *composite data types*, or *aggregate data types*, tooname a few.</span></span>

<span data-ttu-id="0107c-107">Komplexa datatyper stöds inte internt i Azure Search, men en beprövad lösning innehåller två steg för att förenkla hello struktur och sedan använda en **samling** datatypen tooreconstitute hello inre struktur.</span><span class="sxs-lookup"><span data-stu-id="0107c-107">Complex data types are not natively supported in Azure Search, but a proven workaround includes a two-step process of flattening hello structure and then using a **Collection** data type tooreconstitute hello interior structure.</span></span> <span data-ttu-id="0107c-108">Följande hello-tekniken som beskrivs i den här artikeln låter hello innehåll toobe eftersökt fasetterad, filtreras och sorteras.</span><span class="sxs-lookup"><span data-stu-id="0107c-108">Following hello technique described in this article allows hello content toobe searched, faceted, filtered, and sorted.</span></span>

## <a name="example-of-a-complex-data-structure"></a><span data-ttu-id="0107c-109">Exempel på en komplex struktur</span><span class="sxs-lookup"><span data-stu-id="0107c-109">Example of a complex data structure</span></span>
<span data-ttu-id="0107c-110">Hello data i fråga finns vanligen som en uppsättning JSON eller XML-dokument eller objekt i en NoSQL-arkivet, t.ex Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0107c-110">Typically, hello data in question resides as a set of JSON or XML documents, or as items in a NoSQL store such as Azure Cosmos DB.</span></span> <span data-ttu-id="0107c-111">Hello challenge beror strukturellt, från att ha flera underordnade objekt som behöver toobe genomsöks och filtreras.</span><span class="sxs-lookup"><span data-stu-id="0107c-111">Structurally, hello challenge stems from having multiple child items that need toobe searched and filtered.</span></span>  <span data-ttu-id="0107c-112">Vidta följande JSON-dokument som innehåller en uppsättning kontakter som exempel hello som en startpunkt för att illustrera hello lösning:</span><span class="sxs-lookup"><span data-stu-id="0107c-112">As a starting point for illustrating hello workaround, take hello following JSON document that lists a set of contacts as an example:</span></span>

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

<span data-ttu-id="0107c-113">Medan hello fält med namnet ”id” 'name' och 'företag' kan enkelt mappas 1 som fält i ett Azure Search-index, hello 'platser' fältet innehåller en matris med-platser med både en uppsättning plats-ID: N, samt beskrivningar av platsen.</span><span class="sxs-lookup"><span data-stu-id="0107c-113">While hello fields named ‘id’, ‘name’ and ‘company’ can easily be mapped one-to-one as fields within an Azure Search index, hello ‘locations’ field contains an array of locations, having both a set of location IDs as well as location descriptions.</span></span> <span data-ttu-id="0107c-114">Med hänsyn till att Azure Search inte har en datatyp som stöder detta, måste ett annat sätt toomodel detta i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="0107c-114">Given that Azure Search does not have a data type that supports this, we need a different way toomodel this in Azure Search.</span></span> 

> [!NOTE]
> <span data-ttu-id="0107c-115">Den här tekniken beskrivs också av Kirk Evans i ett blogginlägg [indexering DocumentDB med Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), vilket visar att en teknik som kallas ”förenkling hello data”, där du skulle ha ett fält med namnet `locationsID` och `locationsDescription` som är både [samlingar](https://msdn.microsoft.com/library/azure/dn798938.aspx) (eller en matris med strängar).</span><span class="sxs-lookup"><span data-stu-id="0107c-115">This technique is also described by Kirk Evans in a blog post [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), which shows a technique called "flattening hello data", whereby you would have a field called `locationsID` and `locationsDescription` that are both [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span>   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a><span data-ttu-id="0107c-116">Del 1: Förenkla hello matris till enskilda fält</span><span class="sxs-lookup"><span data-stu-id="0107c-116">Part 1: Flatten hello array into individual fields</span></span>
<span data-ttu-id="0107c-117">toocreate Azure Search index som hanterar den här datauppsättningen skapa enskilda fält för hello kapslade underordnade: `locationsID` och `locationsDescription` med datatypen [samlingar](https://msdn.microsoft.com/library/azure/dn798938.aspx) (eller en matris med strängar).</span><span class="sxs-lookup"><span data-stu-id="0107c-117">toocreate an Azure Search index that accommodates this dataset, create individual fields for hello nested substructure: `locationsID` and `locationsDescription` with a data type of [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span> <span data-ttu-id="0107c-118">I de här fälten skulle du indexera '1' och '2' hello-värden i hello `locationsID` för John Smith och hello värden '3' och '4' till hello `locationsID` för Jen Campbell.</span><span class="sxs-lookup"><span data-stu-id="0107c-118">In these fields you would index hello values ‘1’ and ‘2’ into hello `locationsID` field for John Smith and hello values ‘3’ & ‘4’ into hello `locationsID` field for Jen Campbell.</span></span>  

<span data-ttu-id="0107c-119">Dina data i Azure Search skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="0107c-119">Your data within Azure Search would look like this:</span></span> 

![exempeldata, 2 rader](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a><span data-ttu-id="0107c-121">Del 2: Lägga till en samling fält i hello indexdefinitionen</span><span class="sxs-lookup"><span data-stu-id="0107c-121">Part 2: Add a collection field in hello index definition</span></span>
<span data-ttu-id="0107c-122">I hello indexeringsschema Se hello fältdefinitioner liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="0107c-122">In hello index schema, hello field definitions might look similar toothis example.</span></span>

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a><span data-ttu-id="0107c-123">Validera Sök beteenden och eventuellt utökar hello index</span><span class="sxs-lookup"><span data-stu-id="0107c-123">Validate search behaviors and optionally extend hello index</span></span>
<span data-ttu-id="0107c-124">Under förutsättning att du skapade hello index och läsa in hello, kan du nu testa hello lösning tooverify Sök köra frågor mot hello dataset.</span><span class="sxs-lookup"><span data-stu-id="0107c-124">Assuming you created hello index and loaded hello data, you can now test hello solution tooverify search query execution against hello dataset.</span></span> <span data-ttu-id="0107c-125">Varje **samling** fältet måste innehålla **sökbara**, **filtrera** och **facetable**.</span><span class="sxs-lookup"><span data-stu-id="0107c-125">Each **collection** field should be **searchable**, **filterable** and **facetable**.</span></span> <span data-ttu-id="0107c-126">Du ska kunna toorun frågor som:</span><span class="sxs-lookup"><span data-stu-id="0107c-126">You should be able toorun queries like:</span></span>

* <span data-ttu-id="0107c-127">Hitta alla personer som arbetar på hello ”Adventureworks huvudkontoret”.</span><span class="sxs-lookup"><span data-stu-id="0107c-127">Find all people who work at hello ‘Adventureworks Headquarters’.</span></span>
* <span data-ttu-id="0107c-128">Hämta ett antal hello antalet personer som arbetar i ett hem Office.</span><span class="sxs-lookup"><span data-stu-id="0107c-128">Get a count of hello number of people who work in a ‘Home Office’.</span></span>  
* <span data-ttu-id="0107c-129">Hello personer som arbetar på en Home Office, visar du vilket kontor som de arbetar tillsammans med antalet hello personer på varje plats.</span><span class="sxs-lookup"><span data-stu-id="0107c-129">Of hello people who work at a ‘Home Office’, show what other offices they work along with a count of hello people in each location.</span></span>  

<span data-ttu-id="0107c-130">När den här tekniken infaller isär är när du behöver toodo en sökning som kombinerar både hello plats-id samt hello Platsbeskrivning.</span><span class="sxs-lookup"><span data-stu-id="0107c-130">Where this technique falls apart is when you need toodo a search that combines both hello location id as well as hello location description.</span></span> <span data-ttu-id="0107c-131">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0107c-131">For example:</span></span>

* <span data-ttu-id="0107c-132">Hitta alla personer som de har ett hemkontor och har en plats-ID på 4.</span><span class="sxs-lookup"><span data-stu-id="0107c-132">Find all people where they have a Home Office AND has a location ID of 4.</span></span>  

<span data-ttu-id="0107c-133">Om du kommer ihåg hello ursprungliga innehållet såg ut så här:</span><span class="sxs-lookup"><span data-stu-id="0107c-133">If you recall hello original content looked like this:</span></span>

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

<span data-ttu-id="0107c-134">Nu när vi har avgränsat hello data i separata fält, vi har dock inget sätt att veta om hello hemkontor för Jen Campbell gäller för`locationsID 3` eller `locationsID 4`.</span><span class="sxs-lookup"><span data-stu-id="0107c-134">However, now that we have separated hello data into separate fields, we have no way of knowing if hello Home Office for Jen Campbell relates too`locationsID 3` or `locationsID 4`.</span></span>  

<span data-ttu-id="0107c-135">Det här fallet toohandle definiera ett annat fält i hello index som kombinerar alla hello data till en enda samling.</span><span class="sxs-lookup"><span data-stu-id="0107c-135">toohandle this case, define another field in hello index that combines all of hello data into a single collection.</span></span>  <span data-ttu-id="0107c-136">I vårt exempel kommer vi kallar det här fältet `locationsCombined` och vi kommer att separeras hello innehåll med en `||` men du kan välja alla avgränsare som du anser skulle vara en unik uppsättning tecken för ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="0107c-136">For our example, we will call this field `locationsCombined` and we will separate hello content with a `||` although you can choose any separator that you think would be a unique set of characters for your content.</span></span> <span data-ttu-id="0107c-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0107c-137">For example:</span></span> 

![exempeldata, 2 rader med avgränsare](./media/search-howto-complex-data-types/sample-data-2.png)

<span data-ttu-id="0107c-139">Med den här `locationsCombined` fältet vi kan nu hantera ännu fler frågor som:</span><span class="sxs-lookup"><span data-stu-id="0107c-139">Using this `locationsCombined` field, we can now accommodate even more queries, such as:</span></span>

* <span data-ttu-id="0107c-140">Visa antalet personer som arbetar på en 'hemkontor' med plats-Id för '4'.</span><span class="sxs-lookup"><span data-stu-id="0107c-140">Show a count of people who work at a ‘Home Office’ with location Id of ‘4’.</span></span>  
* <span data-ttu-id="0107c-141">Sök efter personer som arbetar på en Home Office med plats-Id '4'.</span><span class="sxs-lookup"><span data-stu-id="0107c-141">Search for people who work at a ‘Home Office’ with location Id ‘4’.</span></span> 

## <a name="limitations"></a><span data-ttu-id="0107c-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="0107c-142">Limitations</span></span>
<span data-ttu-id="0107c-143">Den här metoden är användbar för ett antal scenarier, men den kan inte användas i samtliga fall.</span><span class="sxs-lookup"><span data-stu-id="0107c-143">This technique is useful for a number of scenarios, but it is not applicable in every case.</span></span>  <span data-ttu-id="0107c-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0107c-144">For example:</span></span>

1. <span data-ttu-id="0107c-145">Om du inte har en statisk uppsättning fält i en komplex datatyp och det fanns inga sätt toomap typer alla hello möjliga tooa fält.</span><span class="sxs-lookup"><span data-stu-id="0107c-145">If you do not have a static set of fields in your complex data type and there was no way toomap all hello possible types tooa single field.</span></span> 
2. <span data-ttu-id="0107c-146">Uppdatering av hello kapslade objekt kräver vissa extra arbete toodetermine exakt vad som måste uppdateras i hello Azure Search index toobe</span><span class="sxs-lookup"><span data-stu-id="0107c-146">Updating hello nested objects requires some extra work toodetermine exactly what needs toobe updated in hello Azure Search index</span></span>

## <a name="sample-code"></a><span data-ttu-id="0107c-147">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="0107c-147">Sample code</span></span>
<span data-ttu-id="0107c-148">Du kan se ett exempel på inställningarna i tooindex komplexa JSON-data i Azure Search och genomföra ett antal frågor via den här datauppsättningen på den här [GitHub-repo-](https://github.com/liamca/AzureSearchComplexTypes).</span><span class="sxs-lookup"><span data-stu-id="0107c-148">You can see an example on how tooindex a complex JSON data set into Azure Search and perform a number of queries over this dataset at this [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).</span></span>

## <a name="next-step"></a><span data-ttu-id="0107c-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0107c-149">Next step</span></span>
<span data-ttu-id="0107c-150">[Rösta på inbyggt stöd för komplexa datatyper](https://feedback.azure.com/forums/263029-azure-search) på hello Azure Search UserVoice sidan och ange några ytterligare indata som du vill att oss tooconsider om funktionen implementering.</span><span class="sxs-lookup"><span data-stu-id="0107c-150">[Vote for native support for complex data types](https://feedback.azure.com/forums/263029-azure-search) on hello Azure Search UserVoice page and provide any additional input that you’d like us tooconsider regarding feature implementation.</span></span> <span data-ttu-id="0107c-151">Du kan också nå ut toome direkt på Twitter på @liamca.</span><span class="sxs-lookup"><span data-stu-id="0107c-151">You can also reach out toome directly on Twitter at @liamca.</span></span>

