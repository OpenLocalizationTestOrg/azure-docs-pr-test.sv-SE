---
title: aaaWorking med geospatiala data i Azure Cosmos DB | Microsoft Docs
description: "Förstå hur toocreate, index- och fråga spatial objekt med Azure Cosmos DB och hello DocumentDB-API."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 82ce2898-a9f9-4acf-af4d-8ca4ba9c7b8f
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/22/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1e40b78cb4595631d845d46c21d07a30c8b972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a><span data-ttu-id="53035-103">Arbeta med geospatial och GeoJSON platsdata i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="53035-103">Working with geospatial and GeoJSON location data in Azure Cosmos DB</span></span>
<span data-ttu-id="53035-104">Den här artikeln är en introduktion toohello geospatiala funktioner i [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="53035-104">This article is an introduction toohello geospatial functionality in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="53035-105">När du har läst detta, kommer du att kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="53035-105">After reading this, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="53035-106">Hur lagrar spatial data i Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="53035-106">How do I store spatial data in Azure Cosmos DB?</span></span>
* <span data-ttu-id="53035-107">Hur kan jag fråga geospatiala data i Azure Cosmos-databasen i SQL- och LINQ?</span><span class="sxs-lookup"><span data-stu-id="53035-107">How can I query geospatial data in Azure Cosmos DB in SQL and LINQ?</span></span>
* <span data-ttu-id="53035-108">Hur jag för att aktivera eller inaktivera spatial indexering i Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="53035-108">How do I enable or disable spatial indexing in Azure Cosmos DB?</span></span>

<span data-ttu-id="53035-109">Den här artikeln visar hur toowork med spatialdata med hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="53035-109">This article shows how toowork with spatial data with hello DocumentDB API.</span></span> <span data-ttu-id="53035-110">Finns [GitHub projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) för kodexempel.</span><span class="sxs-lookup"><span data-stu-id="53035-110">Please see this [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) for code samples.</span></span>

## <a name="introduction-toospatial-data"></a><span data-ttu-id="53035-111">Introduktion toospatial data</span><span class="sxs-lookup"><span data-stu-id="53035-111">Introduction toospatial data</span></span>
<span data-ttu-id="53035-112">Spatialdata beskriver hello position och form av objekt i utrymmet.</span><span class="sxs-lookup"><span data-stu-id="53035-112">Spatial data describes hello position and shape of objects in space.</span></span> <span data-ttu-id="53035-113">Dessa motsvarar tooobjects på jorden hello, d.v.s. geospatiala data i de flesta program.</span><span class="sxs-lookup"><span data-stu-id="53035-113">In most applications, these correspond tooobjects on hello earth, i.e. geospatial data.</span></span> <span data-ttu-id="53035-114">Spatialdata kan vara används toorepresent hello platsen för en person, ett intressant plats eller hello gränsen för en stad eller ett lake.</span><span class="sxs-lookup"><span data-stu-id="53035-114">Spatial data can be used toorepresent hello location of a person, a place of interest, or hello boundary of a city, or a lake.</span></span> <span data-ttu-id="53035-115">Vanliga användningsområden innebär ofta närhet frågor för t.ex. ”hitta alla kaféer nära mitt aktuella plats”.</span><span class="sxs-lookup"><span data-stu-id="53035-115">Common use cases often involve proximity queries, for e.g., "find all coffee shops near my current location".</span></span> 

### <a name="geojson"></a><span data-ttu-id="53035-116">GeoJSON</span><span class="sxs-lookup"><span data-stu-id="53035-116">GeoJSON</span></span>
<span data-ttu-id="53035-117">Azure Cosmos-DB har stöd för indexering och frågar geospatiala punkt data som representeras med hello [GeoJSON-specifikationen](https://tools.ietf.org/html/rfc7946).</span><span class="sxs-lookup"><span data-stu-id="53035-117">Azure Cosmos DB supports indexing and querying of geospatial point data that's represented using hello [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span></span> <span data-ttu-id="53035-118">GeoJSON-datastrukturer är alltid giltig JSON-objekt så att de kan lagras och efterfrågas med hjälp av Azure Cosmos DB utan särskilda verktyg eller bibliotek.</span><span class="sxs-lookup"><span data-stu-id="53035-118">GeoJSON data structures are always valid JSON objects, so they can be stored and queried using Azure Cosmos DB without any specialized tools or libraries.</span></span> <span data-ttu-id="53035-119">hello Azure Cosmos DB SDK: er ger hjälpprogramklasser och metoder som gör det enkelt toowork med spatialdata.</span><span class="sxs-lookup"><span data-stu-id="53035-119">hello Azure Cosmos DB SDKs provide helper classes and methods that make it easy toowork with spatial data.</span></span> 

### <a name="points-linestrings-and-polygons"></a><span data-ttu-id="53035-120">Punkter, LineStrings och polygoner</span><span class="sxs-lookup"><span data-stu-id="53035-120">Points, LineStrings and Polygons</span></span>
<span data-ttu-id="53035-121">En **punkt** anger en enda plats i utrymme.</span><span class="sxs-lookup"><span data-stu-id="53035-121">A **Point** denotes a single position in space.</span></span> <span data-ttu-id="53035-122">I geospatiala data representerar en hello exakta platsen, som kan vara en gatuadress i en affären, en kiosk, en bil eller en stad.</span><span class="sxs-lookup"><span data-stu-id="53035-122">In geospatial data, a Point represents hello exact location, which could be a street address of a grocery store, a kiosk, an automobile or a city.</span></span>  <span data-ttu-id="53035-123">En punkt representeras i GeoJSON (och Azure Cosmos DB) med hjälp av dess koordinat par eller longitud och latitud.</span><span class="sxs-lookup"><span data-stu-id="53035-123">A point is represented in GeoJSON (and Azure Cosmos DB) using its coordinate pair or longitude and latitude.</span></span> <span data-ttu-id="53035-124">Här är ett exempel JSON för en punkt.</span><span class="sxs-lookup"><span data-stu-id="53035-124">Here's an example JSON for a point.</span></span>

<span data-ttu-id="53035-125">**Punkter i Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="53035-125">**Points in Azure Cosmos DB**</span></span>

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> <span data-ttu-id="53035-126">Hej GeoJSON-specifikationen anger longitud första och latitud andra.</span><span class="sxs-lookup"><span data-stu-id="53035-126">hello GeoJSON specification specifies longitude first and latitude second.</span></span> <span data-ttu-id="53035-127">Precis som i andra mappning program longitud och latitud är vinklar och representeras i grader.</span><span class="sxs-lookup"><span data-stu-id="53035-127">Like in other mapping applications, longitude and latitude are angles and represented in terms of degrees.</span></span> <span data-ttu-id="53035-128">Longitudvärden mäts från hello nollmeridianen och är mellan-180 och 180.0 grader och latitud värden mäts från hello ekvatorn och mellan-90.0 och 90.0 grader.</span><span class="sxs-lookup"><span data-stu-id="53035-128">Longitude values are measured from hello Prime Meridian and are between -180 and 180.0 degrees, and latitude values are measured from hello equator and are between -90.0 and 90.0 degrees.</span></span> 
> 
> <span data-ttu-id="53035-129">Azure Cosmos-DB tolkar koordinater som representeras per hello WGS 84 referenssystem.</span><span class="sxs-lookup"><span data-stu-id="53035-129">Azure Cosmos DB interprets coordinates as represented per hello WGS-84 reference system.</span></span> <span data-ttu-id="53035-130">Mer information om koordinaten referenssystem finns nedan.</span><span class="sxs-lookup"><span data-stu-id="53035-130">Please see below for more details about coordinate reference systems.</span></span>
> 
> 

<span data-ttu-id="53035-131">Detta kan vara inbäddat i ett Azure DB som Cosmos-dokument som visas i det här exemplet på en profil som innehåller platsdata:</span><span class="sxs-lookup"><span data-stu-id="53035-131">This can be embedded in an Azure Cosmos DB document as shown in this example of a user profile containing location data:</span></span>

<span data-ttu-id="53035-132">**Använd profil med platsen lagras i Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="53035-132">**Use Profile with Location stored in Azure Cosmos DB**</span></span>

```json
{
    "id":"documentdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

<span data-ttu-id="53035-133">Dessutom stöder toopoints GeoJSON också LineStrings och polygoner.</span><span class="sxs-lookup"><span data-stu-id="53035-133">In addition toopoints, GeoJSON also supports LineStrings and Polygons.</span></span> <span data-ttu-id="53035-134">**LineStrings** representerar en mängd två eller flera punkter i utrymmet och hello linjesegment som ansluter dem.</span><span class="sxs-lookup"><span data-stu-id="53035-134">**LineStrings** represent a series of two or more points in space and hello line segments that connect them.</span></span> <span data-ttu-id="53035-135">LineStrings finns vanliga toorepresent landsvägar eller floder i geospatiala data.</span><span class="sxs-lookup"><span data-stu-id="53035-135">In geospatial data, LineStrings are commonly used toorepresent highways or rivers.</span></span> <span data-ttu-id="53035-136">En **Polygon** är en gräns för anslutna pekar som utgör en stängd LineString.</span><span class="sxs-lookup"><span data-stu-id="53035-136">A **Polygon** is a boundary of connected points that forms a closed LineString.</span></span> <span data-ttu-id="53035-137">Polygoner är vanliga toorepresent fysiska formationer som sjöar eller politiska jurisdiktioner som orter och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="53035-137">Polygons are commonly used toorepresent natural formations like lakes or political jurisdictions like cities and states.</span></span> <span data-ttu-id="53035-138">Här är ett exempel på en Polygon i Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="53035-138">Here's an example of a Polygon in Azure Cosmos DB.</span></span> 

<span data-ttu-id="53035-139">**Polygoner i GeoJSON**</span><span class="sxs-lookup"><span data-stu-id="53035-139">**Polygons in GeoJSON**</span></span>

```json
{
    "type":"Polygon",
    "coordinates":[
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ]
}
```

> [!NOTE]
> <span data-ttu-id="53035-140">Hej GeoJSON specifikationen kräver för giltig polygoner hello senaste koordinaten par angivna att hello samma som hello först toocreate en sluten form.</span><span class="sxs-lookup"><span data-stu-id="53035-140">hello GeoJSON specification requires that for valid Polygons, hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span>
> 
> <span data-ttu-id="53035-141">Punkter i en Polygon måste anges i följd medurs ordning.</span><span class="sxs-lookup"><span data-stu-id="53035-141">Points within a Polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="53035-142">En Polygon som angetts i medurs ordning representerar hello inversen av hello region i den.</span><span class="sxs-lookup"><span data-stu-id="53035-142">A Polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>
> 
> 

<span data-ttu-id="53035-143">I tillägg tooPoint LineString och Polygon, GeoJSON även anger hur hello representation toogroup flera geospatiala platser och hur tooassociate godtycklig egenskaper med geolokalisering som en **funktionen**.</span><span class="sxs-lookup"><span data-stu-id="53035-143">In addition tooPoint, LineString and Polygon, GeoJSON also specifies hello representation for how toogroup multiple geospatial locations, as well as how tooassociate arbitrary properties with geolocation as a **Feature**.</span></span> <span data-ttu-id="53035-144">Eftersom de här objekten är giltig JSON, kan de alla lagras och behandlas i Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="53035-144">Since these objects are valid JSON, they can all be stored and processed in Azure Cosmos DB.</span></span> <span data-ttu-id="53035-145">Men Azure Cosmos DB endast stöd för automatisk indexering av punkter.</span><span class="sxs-lookup"><span data-stu-id="53035-145">However Azure Cosmos DB only supports automatic indexing of points.</span></span>

### <a name="coordinate-reference-systems"></a><span data-ttu-id="53035-146">Samordna referenssystem</span><span class="sxs-lookup"><span data-stu-id="53035-146">Coordinate reference systems</span></span>
<span data-ttu-id="53035-147">Eftersom hello form hello jorden oregelbundna representeras koordinater geospatiala data i många koordinaten referenssystem (CR), var och en med sin egen ramar på referens- och måttenheter.</span><span class="sxs-lookup"><span data-stu-id="53035-147">Since hello shape of hello earth is irregular, coordinates of geospatial data is represented in many coordinate reference systems (CRS), each with their own frames of reference and units of measurement.</span></span> <span data-ttu-id="53035-148">Hej ”National rutnät av Storbritannien” är exempelvis referenssystem är mycket noggrann för hello Storbritannien men inte utanför den.</span><span class="sxs-lookup"><span data-stu-id="53035-148">For example, hello "National Grid of Britain" is a reference system is very accurate for hello United Kingdom, but not outside it.</span></span> 

<span data-ttu-id="53035-149">hello populäraste CR används idag är hello World geodetiskt System [WGS 84](http://earth-info.nga.mil/GandG/wgs84/).</span><span class="sxs-lookup"><span data-stu-id="53035-149">hello most popular CRS in use today is hello World Geodetic System  [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span></span> <span data-ttu-id="53035-150">Använd WGS 84 GPS-enheter och tjänster för många mappning inklusive Google kartor och Bing Maps API: er.</span><span class="sxs-lookup"><span data-stu-id="53035-150">GPS devices, and many mapping services including Google Maps and Bing Maps APIs use WGS-84.</span></span> <span data-ttu-id="53035-151">Azure Cosmos-DB har stöd för indexering och frågar geospatiala data med hjälp av hello WGS 84 CR.</span><span class="sxs-lookup"><span data-stu-id="53035-151">Azure Cosmos DB supports indexing and querying of geospatial data using hello WGS-84 CRS only.</span></span> 

## <a name="creating-documents-with-spatial-data"></a><span data-ttu-id="53035-152">Skapa dokument med spatialdata</span><span class="sxs-lookup"><span data-stu-id="53035-152">Creating documents with spatial data</span></span>
<span data-ttu-id="53035-153">När du skapar dokument som innehåller GeoJSON värden indexeras automatiskt med ett spatial index i enlighet toohello indexprincip hello samling.</span><span class="sxs-lookup"><span data-stu-id="53035-153">When you create documents that contain GeoJSON values, they are automatically indexed with a spatial index in accordance toohello indexing policy of hello collection.</span></span> <span data-ttu-id="53035-154">Om du arbetar med en Azure Cosmos DB SDK i ett dynamiskt skrivna språk som Python eller Node.js, måste du skapa giltiga GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="53035-154">If you're working with an Azure Cosmos DB SDK in a dynamically typed language like Python or Node.js, you must create valid GeoJSON.</span></span>

<span data-ttu-id="53035-155">**Skapa ett dokument med geospatiala data i Node.js**</span><span class="sxs-lookup"><span data-stu-id="53035-155">**Create Document with Geospatial data in Node.js**</span></span>

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within hello callback
});
```

<span data-ttu-id="53035-156">Om du arbetar med hello DocumentDB APIs, kan du använda hello `Point` och `Polygon` klasser i hello `Microsoft.Azure.Documents.Spatial` namnområde tooembed platsinformation i programobjekt.</span><span class="sxs-lookup"><span data-stu-id="53035-156">If you're working with hello DocumentDB APIs, you can use hello `Point` and `Polygon` classes within hello `Microsoft.Azure.Documents.Spatial` namespace tooembed location information within your application objects.</span></span> <span data-ttu-id="53035-157">De här klasserna underlätta hello serialisering och deserialisering av spatialdata i GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="53035-157">These classes help simplify hello serialization and deserialization of spatial data into GeoJSON.</span></span>

<span data-ttu-id="53035-158">**Skapa ett dokument med geospatiala data i .NET**</span><span class="sxs-lookup"><span data-stu-id="53035-158">**Create Document with Geospatial data in .NET**</span></span>

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "documentdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

<span data-ttu-id="53035-159">Om du inte har hello latitud och longitud information, men har hello fysiska adresser eller platsnamnet som stad eller ett land, kan du söka efter hello faktiska koordinaterna med hjälp av en tjänst för geokodning som Bing Maps REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="53035-159">If you don't have hello latitude and longitude information, but have hello physical addresses or location name like city or country, you can look up hello actual coordinates by using a geocoding service like Bing Maps REST Services.</span></span> <span data-ttu-id="53035-160">Mer information om Bing Maps geokodning [här](https://msdn.microsoft.com/library/ff701713.aspx).</span><span class="sxs-lookup"><span data-stu-id="53035-160">Learn more about Bing Maps geocoding [here](https://msdn.microsoft.com/library/ff701713.aspx).</span></span>

## <a name="querying-spatial-types"></a><span data-ttu-id="53035-161">Fråga spatialtyper</span><span class="sxs-lookup"><span data-stu-id="53035-161">Querying spatial types</span></span>
<span data-ttu-id="53035-162">Nu när vi har valt en titt på hur tooinsert geospatiala data ska vi ta en titt på hur tooquery dessa data med Azure Cosmos-databasen med SQL och LINQ.</span><span class="sxs-lookup"><span data-stu-id="53035-162">Now that we've taken a look at how tooinsert geospatial data, let's take a look at how tooquery this data using Azure Cosmos DB using SQL and LINQ.</span></span>

### <a name="spatial-sql-built-in-functions"></a><span data-ttu-id="53035-163">Spatial inbyggda SQL-funktioner</span><span class="sxs-lookup"><span data-stu-id="53035-163">Spatial SQL built-in functions</span></span>
<span data-ttu-id="53035-164">Azure Cosmos-DB stöder hello följande öppna geospatiala Consortium (OGC) inbyggda funktioner för geospatial frågor.</span><span class="sxs-lookup"><span data-stu-id="53035-164">Azure Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> <span data-ttu-id="53035-165">Mer information om hello fullständig uppsättning inbyggda funktioner i hello SQL-språket finns för[frågan Azure Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="53035-165">For more details on hello complete set of built-in functions in hello SQL language, please refer too[Query Azure Cosmos DB](documentdb-sql-query.md).</span></span>

<table>
<tr>
  <td><span data-ttu-id="53035-166"><strong>Användning</strong></span><span class="sxs-lookup"><span data-stu-id="53035-166"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="53035-167"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="53035-167"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="53035-168">ST_DISTANCE (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="53035-168">ST_DISTANCE (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="53035-169">Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.</span><span class="sxs-lookup"><span data-stu-id="53035-169">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="53035-170">ST_WITHIN (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="53035-170">ST_WITHIN (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="53035-171">Returnerar ett booleskt uttryck som anger om hello första GeoJSON objekt (Point, LineString eller Polygon) är i hello andra GeoJSON objekt (Point, LineString eller Polygon).</span><span class="sxs-lookup"><span data-stu-id="53035-171">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="53035-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="53035-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="53035-173">Returnerar ett booleskt uttryck som anger om intersect hello två angivna GeoJSON-objekt (Point, LineString eller Polygon).</span><span class="sxs-lookup"><span data-stu-id="53035-173">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="53035-174">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="53035-174">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="53035-175">Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.</span><span class="sxs-lookup"><span data-stu-id="53035-175">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="53035-176">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="53035-176">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="53035-177">Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="53035-177">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="53035-178">Spatial funktioner kan inte använda tooperform närhet frågor mot spatialdata.</span><span class="sxs-lookup"><span data-stu-id="53035-178">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="53035-179">Här är till exempel en fråga som returnerar alla familj dokument att 30 km av hello angivna platsen använder hello ST_DISTANCE inbyggd funktion.</span><span class="sxs-lookup"><span data-stu-id="53035-179">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="53035-180">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="53035-180">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="53035-181">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="53035-181">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="53035-182">Om du inkluderar spatial indexering i indexprincip, sedan hanteras ”avståndet frågor” effektivt via hello index.</span><span class="sxs-lookup"><span data-stu-id="53035-182">If you include spatial indexing in your indexing policy, then "distance queries" will be served efficiently through hello index.</span></span> <span data-ttu-id="53035-183">Mer information om spatial indexering finns hello nedan.</span><span class="sxs-lookup"><span data-stu-id="53035-183">For more details on spatial indexing, please see hello section below.</span></span> <span data-ttu-id="53035-184">Om du inte har en spatial index för hello sökvägar som anges, kan du fortfarande utföra spatial frågor genom att ange `x-ms-documentdb-query-enable-scan` huvudet i begäran med hello värde anges för ”true”.</span><span class="sxs-lookup"><span data-stu-id="53035-184">If you don't have a spatial index for hello specified paths, you can still perform spatial queries by specifying `x-ms-documentdb-query-enable-scan` request header with hello value set too"true".</span></span> <span data-ttu-id="53035-185">I .NET, detta kan göras genom att skicka hello valfria **FeedOptions** argumentet tooqueries med [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ange tootrue.</span><span class="sxs-lookup"><span data-stu-id="53035-185">In .NET, this can be done by passing hello optional **FeedOptions** argument tooqueries with [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) set tootrue.</span></span> 

<span data-ttu-id="53035-186">ST_WITHIN kan bara använda toocheck om en punkten befinner sig inom en Polygon.</span><span class="sxs-lookup"><span data-stu-id="53035-186">ST_WITHIN can be used toocheck if a point lies within a Polygon.</span></span> <span data-ttu-id="53035-187">Polygoner är ofta använda toorepresent gränser som postnummer, tillstånd gränser eller fysiska konstruktioner.</span><span class="sxs-lookup"><span data-stu-id="53035-187">Commonly Polygons are used toorepresent boundaries like zip codes, state boundaries, or natural formations.</span></span> <span data-ttu-id="53035-188">Igen om du inkluderar spatial indexering i indexprincip, sedan ”i” frågor hanteras effektivt via hello index.</span><span class="sxs-lookup"><span data-stu-id="53035-188">Again if you include spatial indexing in your indexing policy, then "within" queries will be served efficiently through hello index.</span></span> 

<span data-ttu-id="53035-189">Polygon argument i ST_WITHIN kan innehålla endast en enkel signal, d.v.s. hello polygoner får inte innehålla tomrum i dem.</span><span class="sxs-lookup"><span data-stu-id="53035-189">Polygon arguments in ST_WITHIN can contain only a single ring, i.e. hello Polygons must not contain holes in them.</span></span> 

<span data-ttu-id="53035-190">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="53035-190">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

<span data-ttu-id="53035-191">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="53035-191">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> <span data-ttu-id="53035-192">Liknande toohow Felmatchade typer fungerar i Azure Cosmos DB-fråga om hello plats värde i antingen argumentet är skadat eller ogiltigt sedan det utvärderas för**Odefinierad** och hello utvärderas dokumentet toobe hoppas över från hello resultatet av frågan.</span><span class="sxs-lookup"><span data-stu-id="53035-192">Similar toohow mismatched types works in Azure Cosmos DB query, if hello location value specified in either argument is malformed or invalid, then it will evaluate too**undefined** and hello evaluated document toobe skipped from hello query results.</span></span> <span data-ttu-id="53035-193">Om frågan returnerar inga resultat som kör ST_ISVALIDDETAILED toodebug varför hello spatail typ är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="53035-193">If your query returns no results, run ST_ISVALIDDETAILED toodebug why hello spatail type is invalid.</span></span>     
> 
> 

<span data-ttu-id="53035-194">Azure Cosmos-DB stöder också utföra omvända frågor, dvs. Du kan indexera polygoner eller rader i Azure Cosmos DB och fråga efter hello-områden som innehåller en angiven punkt.</span><span class="sxs-lookup"><span data-stu-id="53035-194">Azure Cosmos DB also supports performing inverse queries, i.e. you can index Polygons or lines in Azure Cosmos DB, then query for hello areas that contain a specified point.</span></span> <span data-ttu-id="53035-195">Det här mönstret är vanligt i logistik tooidentify t.ex. när en lastbil anländer till eller lämnar avsedda området.</span><span class="sxs-lookup"><span data-stu-id="53035-195">This pattern is commonly used in logistics tooidentify e.g. when a truck enters or leaves a designated area.</span></span> 

<span data-ttu-id="53035-196">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="53035-196">**Query**</span></span>

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


<span data-ttu-id="53035-197">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="53035-197">**Results**</span></span>

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

<span data-ttu-id="53035-198">ST_ISVALID och ST_ISVALIDDETAILED kan vara används toocheck om en Rumsobjektet är giltig.</span><span class="sxs-lookup"><span data-stu-id="53035-198">ST_ISVALID and ST_ISVALIDDETAILED can be used toocheck if a spatial object is valid.</span></span> <span data-ttu-id="53035-199">Följande fråga hello kontrollerar exempelvis hello giltigheten för en plats med en out-of latitud intervall (-132.8).</span><span class="sxs-lookup"><span data-stu-id="53035-199">For example, hello following query checks hello validity of a point with an out of range latitude value (-132.8).</span></span> <span data-ttu-id="53035-200">ST_ISVALID returnerar bara ett booleskt värde och ST_ISVALIDDETAILED returnerar hello booleska och en sträng som innehåller hello orsaken till den betraktas som ogiltig.</span><span class="sxs-lookup"><span data-stu-id="53035-200">ST_ISVALID returns just a Boolean value, and ST_ISVALIDDETAILED returns hello Boolean and a string containing hello reason why it is considered invalid.</span></span>

<span data-ttu-id="53035-201">** Fråga **</span><span class="sxs-lookup"><span data-stu-id="53035-201">** Query **</span></span>

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

<span data-ttu-id="53035-202">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="53035-202">**Results**</span></span>

    [{
      "$1": false
    }]

<span data-ttu-id="53035-203">Dessa funktioner kan också vara används toovalidate polygoner.</span><span class="sxs-lookup"><span data-stu-id="53035-203">These functions can also be used toovalidate Polygons.</span></span> <span data-ttu-id="53035-204">Till exempel använda här vi ST_ISVALIDDETAILED toovalidate en Polygon som inte är stängd.</span><span class="sxs-lookup"><span data-stu-id="53035-204">For example, here we use ST_ISVALIDDETAILED toovalidate a Polygon that is not closed.</span></span> 

<span data-ttu-id="53035-205">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="53035-205">**Query**</span></span>

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

<span data-ttu-id="53035-206">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="53035-206">**Results**</span></span>

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a><span data-ttu-id="53035-207">LINQ-fråga i hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="53035-207">LINQ Querying in hello .NET SDK</span></span>
<span data-ttu-id="53035-208">hello .NET DocumentDB SDK också providers stub-metoder `Distance()` och `Within()` för användning i LINQ-uttryck.</span><span class="sxs-lookup"><span data-stu-id="53035-208">hello DocumentDB .NET SDK also providers stub methods `Distance()` and `Within()` for use within LINQ expressions.</span></span> <span data-ttu-id="53035-209">Hej DocumentDB LINQ-providern översätter dessa anrop toohello motsvarande SQL inbyggda funktionen metodanrop (ST_DISTANCE och ST_WITHIN respektive).</span><span class="sxs-lookup"><span data-stu-id="53035-209">hello DocumentDB LINQ provider translates these method calls toohello equivalent SQL built-in function calls (ST_DISTANCE and ST_WITHIN respectively).</span></span> 

<span data-ttu-id="53035-210">Här följer ett exempel på en LINQ-fråga som hittar alla dokument i hello Azure Cosmos DB samling vars ”plats”-värde inom en radie på 30km av hello anges peka med hjälp av LINQ.</span><span class="sxs-lookup"><span data-stu-id="53035-210">Here's an example of a LINQ query that finds all documents in hello Azure Cosmos DB collection whose "location" value is within a radius of 30km of hello specified point using LINQ.</span></span>

<span data-ttu-id="53035-211">**LINQ-fråga för avstånd**</span><span class="sxs-lookup"><span data-stu-id="53035-211">**LINQ query for Distance**</span></span>

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

<span data-ttu-id="53035-212">Här är på samma sätt kan en fråga för att söka efter alla hello dokument vars ”plats” ligger inom hello angetts rutan/Polygon.</span><span class="sxs-lookup"><span data-stu-id="53035-212">Similarly, here's a query for finding all hello documents whose "location" is within hello specified box/Polygon.</span></span> 

<span data-ttu-id="53035-213">**LINQ-fråga efter inom**</span><span class="sxs-lookup"><span data-stu-id="53035-213">**LINQ query for Within**</span></span>

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


<span data-ttu-id="53035-214">Nu när vi har valt en titt på hur tooquery dokument med hjälp av LINQ och SQL, ta en titt på hur tooconfigure Azure Cosmos DB för spatial indexering.</span><span class="sxs-lookup"><span data-stu-id="53035-214">Now that we've taken a look at how tooquery documents using LINQ and SQL, let's take a look at how tooconfigure Azure Cosmos DB for spatial indexing.</span></span>

## <a name="indexing"></a><span data-ttu-id="53035-215">Indexering</span><span class="sxs-lookup"><span data-stu-id="53035-215">Indexing</span></span>
<span data-ttu-id="53035-216">Vi enligt hello [Schema oberoende indexering med Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papper vi utformat Azure Cosmos DB database engine toobe verkligen schema oberoende och ge förstklassigt stöd för JSON.</span><span class="sxs-lookup"><span data-stu-id="53035-216">As we described in hello [Schema Agnostic Indexing with Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paper, we designed Azure Cosmos DB’s database engine toobe truly schema agnostic and provide first class support for JSON.</span></span> <span data-ttu-id="53035-217">hello skrivåtgärder optimerade databasmotorn av Azure Cosmos DB förstår internt spatialdata (pekar polygoner och rader) representeras i hello GeoJSON standard.</span><span class="sxs-lookup"><span data-stu-id="53035-217">hello write optimized database engine of Azure Cosmos DB natively understands spatial data (points, Polygons and lines) represented in hello GeoJSON standard.</span></span>

<span data-ttu-id="53035-218">I kort sagt kan hello geometri planerat från geodetisk koordinater till ett 2D-plan och progressivt indelat i celler med hjälp av en **quadtree**.</span><span class="sxs-lookup"><span data-stu-id="53035-218">In a nutshell, hello geometry is projected from geodetic coordinates onto a 2D plane then divided progressively into cells using a **quadtree**.</span></span> <span data-ttu-id="53035-219">Dessa celler är mappade too1D baserat på hello plats hello cell i en **Hilbert utrymme fylla kurvan**, som bevarar ort punkter.</span><span class="sxs-lookup"><span data-stu-id="53035-219">These cells are mapped too1D based on hello location of hello cell within a **Hilbert space filling curve**, which preserves locality of points.</span></span> <span data-ttu-id="53035-220">Dessutom när lokaliseringsuppgifter indexeras går via en process som kallas **tessellation**, d.v.s. alla hello celler som korsar en plats identifieras och lagras som nycklar i hello Azure Cosmos DB index.</span><span class="sxs-lookup"><span data-stu-id="53035-220">Additionally when location data is indexed, it goes through a process known as **tessellation**, i.e. all hello cells that intersect a location are identified and stored as keys in hello Azure Cosmos DB index.</span></span> <span data-ttu-id="53035-221">Frågan samtidigt argument som punkter och polygoner är också fasetterade tooextract hello relevanta cell ID-intervall och sedan används tooretrieve data från hello index.</span><span class="sxs-lookup"><span data-stu-id="53035-221">At query time, arguments like points and Polygons are also tessellated tooextract hello relevant cell ID ranges, then used tooretrieve data from hello index.</span></span>

<span data-ttu-id="53035-222">Om du anger en indexprincip som innehåller rumsindexet för / * (alla sökvägar) och sedan alla punkter hittades i hello samling indexeras för effektiv spatial frågor (ST_WITHIN och ST_DISTANCE).</span><span class="sxs-lookup"><span data-stu-id="53035-222">If you specify an indexing policy that includes spatial index for /* (all paths), then all points found within hello collection are indexed for efficient spatial queries (ST_WITHIN and ST_DISTANCE).</span></span> <span data-ttu-id="53035-223">Rumsindex inte har Precisionvärdet och alltid använda ett standardvärde för precision.</span><span class="sxs-lookup"><span data-stu-id="53035-223">Spatial indexes do not have a precision value, and always use a default precision value.</span></span>

> [!NOTE]
> <span data-ttu-id="53035-224">Azure Cosmos-DB har stöd för automatisk indexering av punkter och polygoner LineStrings</span><span class="sxs-lookup"><span data-stu-id="53035-224">Azure Cosmos DB supports automatic indexing of Points, Polygons, and LineStrings</span></span>
> 
> 

<span data-ttu-id="53035-225">hello följande JSON-fragment visas en indexprincip med spatial indexering aktiverat, index d.v.s. helst GeoJSON hittades inom dokument för spatial frågor.</span><span class="sxs-lookup"><span data-stu-id="53035-225">hello following JSON snippet shows an indexing policy with spatial indexing enabled, i.e. index any GeoJSON point found within documents for spatial querying.</span></span> <span data-ttu-id="53035-226">Om du ändrar hello indexering genom att använda hello Azure-portalen kan ange du hello följande JSON för indexering princip tooenable spatial indexering på din samling.</span><span class="sxs-lookup"><span data-stu-id="53035-226">If you are modifying hello indexing policy using hello Azure Portal, you can specify hello following JSON for indexing policy tooenable spatial indexing on your collection.</span></span>

<span data-ttu-id="53035-227">**Samlingen indexering princip-JSON med Spatial aktiverad för punkter och polygoner**</span><span class="sxs-lookup"><span data-stu-id="53035-227">**Collection Indexing Policy JSON with Spatial enabled for points and Polygons**</span></span>

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

<span data-ttu-id="53035-228">Här är ett kodfragment i .NET som visar hur toocreate en samling med spatial indexering aktiverat för alla sökvägar som innehåller punkter.</span><span class="sxs-lookup"><span data-stu-id="53035-228">Here's a code snippet in .NET that shows how toocreate a collection with spatial indexing turned on for all paths containing points.</span></span> 

<span data-ttu-id="53035-229">**Skapa en samling med spatial indexering**</span><span class="sxs-lookup"><span data-stu-id="53035-229">**Create a collection with spatial indexing**</span></span>

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

<span data-ttu-id="53035-230">Här är hur du ändrar en befintlig samling tootake utnyttja spatial indexering alla frågor som lagras i dokument.</span><span class="sxs-lookup"><span data-stu-id="53035-230">And here's how you can modify an existing collection tootake advantage of spatial indexing over any points that are stored within documents.</span></span>

<span data-ttu-id="53035-231">**Ändra en befintlig samling med spatial indexering**</span><span class="sxs-lookup"><span data-stu-id="53035-231">**Modify an existing collection with spatial indexing**</span></span>

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing toocomplete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> <span data-ttu-id="53035-232">Om hello plats GeoJSON värde inom hello dokumentet är skadat eller ogiltigt, kommer sedan den inte hämta indexeras för spatial frågor.</span><span class="sxs-lookup"><span data-stu-id="53035-232">If hello location GeoJSON value within hello document is malformed or invalid, then it will not get indexed for spatial querying.</span></span> <span data-ttu-id="53035-233">Du kan verifiera plats värden med hjälp av ST_ISVALID och ST_ISVALIDDETAILED.</span><span class="sxs-lookup"><span data-stu-id="53035-233">You can validate location values using ST_ISVALID and ST_ISVALIDDETAILED.</span></span>
> 
> <span data-ttu-id="53035-234">Om din samling definition innehåller en partitionsnyckel, rapporteras indexering omvandling förloppet inte.</span><span class="sxs-lookup"><span data-stu-id="53035-234">If your collection definition includes a partition key, indexing transformation progress is not reported.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="53035-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53035-235">Next steps</span></span>
<span data-ttu-id="53035-236">Nu när du har fått kännedom om hur tooget startas med geospatial stöd i Azure Cosmos-databasen, kan du:</span><span class="sxs-lookup"><span data-stu-id="53035-236">Now that you've learnt about how tooget started with geospatial support in Azure Cosmos DB, you can:</span></span>

* <span data-ttu-id="53035-237">Börja koda med hello [geospatiala .NET kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="53035-237">Start coding with hello [Geospatial .NET code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span></span>
* <span data-ttu-id="53035-238">Hämta händerna med geospatial fråga på hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span><span class="sxs-lookup"><span data-stu-id="53035-238">Get hands on with geospatial querying at hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span></span>
* <span data-ttu-id="53035-239">Lär dig mer om [Azure Cosmos DB-fråga](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="53035-239">Learn more about [Azure Cosmos DB Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="53035-240">Lär dig mer om [Azure Cosmos DB indexering principer](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="53035-240">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>

