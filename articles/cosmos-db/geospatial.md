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
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Arbeta med geospatial och GeoJSON platsdata i Azure Cosmos DB
Den här artikeln är en introduktion toohello geospatiala funktioner i [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). När du har läst detta, kommer du att kunna tooanswer hello följande frågor:

* Hur lagrar spatial data i Azure Cosmos DB?
* Hur kan jag fråga geospatiala data i Azure Cosmos-databasen i SQL- och LINQ?
* Hur jag för att aktivera eller inaktivera spatial indexering i Azure Cosmos DB?

Den här artikeln visar hur toowork med spatialdata med hello DocumentDB-API. Finns [GitHub projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) för kodexempel.

## <a name="introduction-toospatial-data"></a>Introduktion toospatial data
Spatialdata beskriver hello position och form av objekt i utrymmet. Dessa motsvarar tooobjects på jorden hello, d.v.s. geospatiala data i de flesta program. Spatialdata kan vara används toorepresent hello platsen för en person, ett intressant plats eller hello gränsen för en stad eller ett lake. Vanliga användningsområden innebär ofta närhet frågor för t.ex. ”hitta alla kaféer nära mitt aktuella plats”. 

### <a name="geojson"></a>GeoJSON
Azure Cosmos-DB har stöd för indexering och frågar geospatiala punkt data som representeras med hello [GeoJSON-specifikationen](https://tools.ietf.org/html/rfc7946). GeoJSON-datastrukturer är alltid giltig JSON-objekt så att de kan lagras och efterfrågas med hjälp av Azure Cosmos DB utan särskilda verktyg eller bibliotek. hello Azure Cosmos DB SDK: er ger hjälpprogramklasser och metoder som gör det enkelt toowork med spatialdata. 

### <a name="points-linestrings-and-polygons"></a>Punkter, LineStrings och polygoner
En **punkt** anger en enda plats i utrymme. I geospatiala data representerar en hello exakta platsen, som kan vara en gatuadress i en affären, en kiosk, en bil eller en stad.  En punkt representeras i GeoJSON (och Azure Cosmos DB) med hjälp av dess koordinat par eller longitud och latitud. Här är ett exempel JSON för en punkt.

**Punkter i Azure Cosmos DB**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> Hej GeoJSON-specifikationen anger longitud första och latitud andra. Precis som i andra mappning program longitud och latitud är vinklar och representeras i grader. Longitudvärden mäts från hello nollmeridianen och är mellan-180 och 180.0 grader och latitud värden mäts från hello ekvatorn och mellan-90.0 och 90.0 grader. 
> 
> Azure Cosmos-DB tolkar koordinater som representeras per hello WGS 84 referenssystem. Mer information om koordinaten referenssystem finns nedan.
> 
> 

Detta kan vara inbäddat i ett Azure DB som Cosmos-dokument som visas i det här exemplet på en profil som innehåller platsdata:

**Använd profil med platsen lagras i Azure Cosmos DB**

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

Dessutom stöder toopoints GeoJSON också LineStrings och polygoner. **LineStrings** representerar en mängd två eller flera punkter i utrymmet och hello linjesegment som ansluter dem. LineStrings finns vanliga toorepresent landsvägar eller floder i geospatiala data. En **Polygon** är en gräns för anslutna pekar som utgör en stängd LineString. Polygoner är vanliga toorepresent fysiska formationer som sjöar eller politiska jurisdiktioner som orter och tillstånd. Här är ett exempel på en Polygon i Azure Cosmos-databasen. 

**Polygoner i GeoJSON**

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
> Hej GeoJSON specifikationen kräver för giltig polygoner hello senaste koordinaten par angivna att hello samma som hello först toocreate en sluten form.
> 
> Punkter i en Polygon måste anges i följd medurs ordning. En Polygon som angetts i medurs ordning representerar hello inversen av hello region i den.
> 
> 

I tillägg tooPoint LineString och Polygon, GeoJSON även anger hur hello representation toogroup flera geospatiala platser och hur tooassociate godtycklig egenskaper med geolokalisering som en **funktionen**. Eftersom de här objekten är giltig JSON, kan de alla lagras och behandlas i Azure Cosmos-databasen. Men Azure Cosmos DB endast stöd för automatisk indexering av punkter.

### <a name="coordinate-reference-systems"></a>Samordna referenssystem
Eftersom hello form hello jorden oregelbundna representeras koordinater geospatiala data i många koordinaten referenssystem (CR), var och en med sin egen ramar på referens- och måttenheter. Hej ”National rutnät av Storbritannien” är exempelvis referenssystem är mycket noggrann för hello Storbritannien men inte utanför den. 

hello populäraste CR används idag är hello World geodetiskt System [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). Använd WGS 84 GPS-enheter och tjänster för många mappning inklusive Google kartor och Bing Maps API: er. Azure Cosmos-DB har stöd för indexering och frågar geospatiala data med hjälp av hello WGS 84 CR. 

## <a name="creating-documents-with-spatial-data"></a>Skapa dokument med spatialdata
När du skapar dokument som innehåller GeoJSON värden indexeras automatiskt med ett spatial index i enlighet toohello indexprincip hello samling. Om du arbetar med en Azure Cosmos DB SDK i ett dynamiskt skrivna språk som Python eller Node.js, måste du skapa giltiga GeoJSON.

**Skapa ett dokument med geospatiala data i Node.js**

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

Om du arbetar med hello DocumentDB APIs, kan du använda hello `Point` och `Polygon` klasser i hello `Microsoft.Azure.Documents.Spatial` namnområde tooembed platsinformation i programobjekt. De här klasserna underlätta hello serialisering och deserialisering av spatialdata i GeoJSON.

**Skapa ett dokument med geospatiala data i .NET**

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

Om du inte har hello latitud och longitud information, men har hello fysiska adresser eller platsnamnet som stad eller ett land, kan du söka efter hello faktiska koordinaterna med hjälp av en tjänst för geokodning som Bing Maps REST-tjänster. Mer information om Bing Maps geokodning [här](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Fråga spatialtyper
Nu när vi har valt en titt på hur tooinsert geospatiala data ska vi ta en titt på hur tooquery dessa data med Azure Cosmos-databasen med SQL och LINQ.

### <a name="spatial-sql-built-in-functions"></a>Spatial inbyggda SQL-funktioner
Azure Cosmos-DB stöder hello följande öppna geospatiala Consortium (OGC) inbyggda funktioner för geospatial frågor. Mer information om hello fullständig uppsättning inbyggda funktioner i hello SQL-språket finns för[frågan Azure Cosmos DB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Användning</strong></td>
  <td><strong>Beskrivning</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (spatial_expr, spatial_expr)</td>
  <td>Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.</td>
</tr>
<tr>
  <td>ST_WITHIN (spatial_expr, spatial_expr)</td>
  <td>Returnerar ett booleskt uttryck som anger om hello första GeoJSON objekt (Point, LineString eller Polygon) är i hello andra GeoJSON objekt (Point, LineString eller Polygon).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Returnerar ett booleskt uttryck som anger om intersect hello två angivna GeoJSON-objekt (Point, LineString eller Polygon).</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.</td>
</tr>
</table>

Spatial funktioner kan inte använda tooperform närhet frågor mot spatialdata. Här är till exempel en fråga som returnerar alla familj dokument att 30 km av hello angivna platsen använder hello ST_DISTANCE inbyggd funktion. 

**Fråga**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultat**

    [{
      "id": "WakefieldFamily"
    }]

Om du inkluderar spatial indexering i indexprincip, sedan hanteras ”avståndet frågor” effektivt via hello index. Mer information om spatial indexering finns hello nedan. Om du inte har en spatial index för hello sökvägar som anges, kan du fortfarande utföra spatial frågor genom att ange `x-ms-documentdb-query-enable-scan` huvudet i begäran med hello värde anges för ”true”. I .NET, detta kan göras genom att skicka hello valfria **FeedOptions** argumentet tooqueries med [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ange tootrue. 

ST_WITHIN kan bara använda toocheck om en punkten befinner sig inom en Polygon. Polygoner är ofta använda toorepresent gränser som postnummer, tillstånd gränser eller fysiska konstruktioner. Igen om du inkluderar spatial indexering i indexprincip, sedan ”i” frågor hanteras effektivt via hello index. 

Polygon argument i ST_WITHIN kan innehålla endast en enkel signal, d.v.s. hello polygoner får inte innehålla tomrum i dem. 

**Fråga**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Resultat**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Liknande toohow Felmatchade typer fungerar i Azure Cosmos DB-fråga om hello plats värde i antingen argumentet är skadat eller ogiltigt sedan det utvärderas för**Odefinierad** och hello utvärderas dokumentet toobe hoppas över från hello resultatet av frågan. Om frågan returnerar inga resultat som kör ST_ISVALIDDETAILED toodebug varför hello spatail typ är ogiltig.     
> 
> 

Azure Cosmos-DB stöder också utföra omvända frågor, dvs. Du kan indexera polygoner eller rader i Azure Cosmos DB och fråga efter hello-områden som innehåller en angiven punkt. Det här mönstret är vanligt i logistik tooidentify t.ex. när en lastbil anländer till eller lämnar avsedda området. 

**Fråga**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Resultat**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID och ST_ISVALIDDETAILED kan vara används toocheck om en Rumsobjektet är giltig. Följande fråga hello kontrollerar exempelvis hello giltigheten för en plats med en out-of latitud intervall (-132.8). ST_ISVALID returnerar bara ett booleskt värde och ST_ISVALIDDETAILED returnerar hello booleska och en sträng som innehåller hello orsaken till den betraktas som ogiltig.

** Fråga **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Resultat**

    [{
      "$1": false
    }]

Dessa funktioner kan också vara används toovalidate polygoner. Till exempel använda här vi ST_ISVALIDDETAILED toovalidate en Polygon som inte är stängd. 

**Fråga**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Resultat**

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a>LINQ-fråga i hello .NET SDK
hello .NET DocumentDB SDK också providers stub-metoder `Distance()` och `Within()` för användning i LINQ-uttryck. Hej DocumentDB LINQ-providern översätter dessa anrop toohello motsvarande SQL inbyggda funktionen metodanrop (ST_DISTANCE och ST_WITHIN respektive). 

Här följer ett exempel på en LINQ-fråga som hittar alla dokument i hello Azure Cosmos DB samling vars ”plats”-värde inom en radie på 30km av hello anges peka med hjälp av LINQ.

**LINQ-fråga för avstånd**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Här är på samma sätt kan en fråga för att söka efter alla hello dokument vars ”plats” ligger inom hello angetts rutan/Polygon. 

**LINQ-fråga efter inom**

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


Nu när vi har valt en titt på hur tooquery dokument med hjälp av LINQ och SQL, ta en titt på hur tooconfigure Azure Cosmos DB för spatial indexering.

## <a name="indexing"></a>Indexering
Vi enligt hello [Schema oberoende indexering med Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papper vi utformat Azure Cosmos DB database engine toobe verkligen schema oberoende och ge förstklassigt stöd för JSON. hello skrivåtgärder optimerade databasmotorn av Azure Cosmos DB förstår internt spatialdata (pekar polygoner och rader) representeras i hello GeoJSON standard.

I kort sagt kan hello geometri planerat från geodetisk koordinater till ett 2D-plan och progressivt indelat i celler med hjälp av en **quadtree**. Dessa celler är mappade too1D baserat på hello plats hello cell i en **Hilbert utrymme fylla kurvan**, som bevarar ort punkter. Dessutom när lokaliseringsuppgifter indexeras går via en process som kallas **tessellation**, d.v.s. alla hello celler som korsar en plats identifieras och lagras som nycklar i hello Azure Cosmos DB index. Frågan samtidigt argument som punkter och polygoner är också fasetterade tooextract hello relevanta cell ID-intervall och sedan används tooretrieve data från hello index.

Om du anger en indexprincip som innehåller rumsindexet för / * (alla sökvägar) och sedan alla punkter hittades i hello samling indexeras för effektiv spatial frågor (ST_WITHIN och ST_DISTANCE). Rumsindex inte har Precisionvärdet och alltid använda ett standardvärde för precision.

> [!NOTE]
> Azure Cosmos-DB har stöd för automatisk indexering av punkter och polygoner LineStrings
> 
> 

hello följande JSON-fragment visas en indexprincip med spatial indexering aktiverat, index d.v.s. helst GeoJSON hittades inom dokument för spatial frågor. Om du ändrar hello indexering genom att använda hello Azure-portalen kan ange du hello följande JSON för indexering princip tooenable spatial indexering på din samling.

**Samlingen indexering princip-JSON med Spatial aktiverad för punkter och polygoner**

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

Här är ett kodfragment i .NET som visar hur toocreate en samling med spatial indexering aktiverat för alla sökvägar som innehåller punkter. 

**Skapa en samling med spatial indexering**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Här är hur du ändrar en befintlig samling tootake utnyttja spatial indexering alla frågor som lagras i dokument.

**Ändra en befintlig samling med spatial indexering**

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
> Om hello plats GeoJSON värde inom hello dokumentet är skadat eller ogiltigt, kommer sedan den inte hämta indexeras för spatial frågor. Du kan verifiera plats värden med hjälp av ST_ISVALID och ST_ISVALIDDETAILED.
> 
> Om din samling definition innehåller en partitionsnyckel, rapporteras indexering omvandling förloppet inte. 
> 
> 

## <a name="next-steps"></a>Nästa steg
Nu när du har fått kännedom om hur tooget startas med geospatial stöd i Azure Cosmos-databasen, kan du:

* Börja koda med hello [geospatiala .NET kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* Hämta händerna med geospatial fråga på hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)
* Lär dig mer om [Azure Cosmos DB-fråga](documentdb-sql-query.md)
* Lär dig mer om [Azure Cosmos DB indexering principer](indexing-policies.md)

