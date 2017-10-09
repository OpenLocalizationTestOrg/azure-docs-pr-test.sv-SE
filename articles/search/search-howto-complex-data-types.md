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
# <a name="how-toomodel-complex-data-types-in-azure-search"></a>Hur toomodel komplexa datatyper i Azure Search
Externa datauppsättningar används toopopulate Azure Search index omfattar ibland hierarkisk eller kapslade underordnade strukturer som inte nedbrytning av snyggt i en tabellvy raduppsättning. Exempel på sådana strukturer kan innehålla flera platser och telefonnummer för en kund, flera färger och storlekar för en enskild SKU flera författare av en enda bok och så vidare. Modeling villkor du kan se dessa strukturer som anges tooas *komplexa datatyper*, *sammansatta datatyper*, *sammansatta datatyper*, eller *sammanställd datatyper*, tooname ett fåtal.

Komplexa datatyper stöds inte internt i Azure Search, men en beprövad lösning innehåller två steg för att förenkla hello struktur och sedan använda en **samling** datatypen tooreconstitute hello inre struktur. Följande hello-tekniken som beskrivs i den här artikeln låter hello innehåll toobe eftersökt fasetterad, filtreras och sorteras.

## <a name="example-of-a-complex-data-structure"></a>Exempel på en komplex struktur
Hello data i fråga finns vanligen som en uppsättning JSON eller XML-dokument eller objekt i en NoSQL-arkivet, t.ex Azure Cosmos DB. Hello challenge beror strukturellt, från att ha flera underordnade objekt som behöver toobe genomsöks och filtreras.  Vidta följande JSON-dokument som innehåller en uppsättning kontakter som exempel hello som en startpunkt för att illustrera hello lösning:

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

Medan hello fält med namnet ”id” 'name' och 'företag' kan enkelt mappas 1 som fält i ett Azure Search-index, hello 'platser' fältet innehåller en matris med-platser med både en uppsättning plats-ID: N, samt beskrivningar av platsen. Med hänsyn till att Azure Search inte har en datatyp som stöder detta, måste ett annat sätt toomodel detta i Azure Search. 

> [!NOTE]
> Den här tekniken beskrivs också av Kirk Evans i ett blogginlägg [indexering DocumentDB med Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), vilket visar att en teknik som kallas ”förenkling hello data”, där du skulle ha ett fält med namnet `locationsID` och `locationsDescription` som är både [samlingar](https://msdn.microsoft.com/library/azure/dn798938.aspx) (eller en matris med strängar).   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a>Del 1: Förenkla hello matris till enskilda fält
toocreate Azure Search index som hanterar den här datauppsättningen skapa enskilda fält för hello kapslade underordnade: `locationsID` och `locationsDescription` med datatypen [samlingar](https://msdn.microsoft.com/library/azure/dn798938.aspx) (eller en matris med strängar). I de här fälten skulle du indexera '1' och '2' hello-värden i hello `locationsID` för John Smith och hello värden '3' och '4' till hello `locationsID` för Jen Campbell.  

Dina data i Azure Search skulle se ut så här: 

![exempeldata, 2 rader](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a>Del 2: Lägga till en samling fält i hello indexdefinitionen
I hello indexeringsschema Se hello fältdefinitioner liknande toothis exempel.

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

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a>Validera Sök beteenden och eventuellt utökar hello index
Under förutsättning att du skapade hello index och läsa in hello, kan du nu testa hello lösning tooverify Sök köra frågor mot hello dataset. Varje **samling** fältet måste innehålla **sökbara**, **filtrera** och **facetable**. Du ska kunna toorun frågor som:

* Hitta alla personer som arbetar på hello ”Adventureworks huvudkontoret”.
* Hämta ett antal hello antalet personer som arbetar i ett hem Office.  
* Hello personer som arbetar på en Home Office, visar du vilket kontor som de arbetar tillsammans med antalet hello personer på varje plats.  

När den här tekniken infaller isär är när du behöver toodo en sökning som kombinerar både hello plats-id samt hello Platsbeskrivning. Exempel:

* Hitta alla personer som de har ett hemkontor och har en plats-ID på 4.  

Om du kommer ihåg hello ursprungliga innehållet såg ut så här:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Nu när vi har avgränsat hello data i separata fält, vi har dock inget sätt att veta om hello hemkontor för Jen Campbell gäller för`locationsID 3` eller `locationsID 4`.  

Det här fallet toohandle definiera ett annat fält i hello index som kombinerar alla hello data till en enda samling.  I vårt exempel kommer vi kallar det här fältet `locationsCombined` och vi kommer att separeras hello innehåll med en `||` men du kan välja alla avgränsare som du anser skulle vara en unik uppsättning tecken för ditt innehåll. Exempel: 

![exempeldata, 2 rader med avgränsare](./media/search-howto-complex-data-types/sample-data-2.png)

Med den här `locationsCombined` fältet vi kan nu hantera ännu fler frågor som:

* Visa antalet personer som arbetar på en 'hemkontor' med plats-Id för '4'.  
* Sök efter personer som arbetar på en Home Office med plats-Id '4'. 

## <a name="limitations"></a>Begränsningar
Den här metoden är användbar för ett antal scenarier, men den kan inte användas i samtliga fall.  Exempel:

1. Om du inte har en statisk uppsättning fält i en komplex datatyp och det fanns inga sätt toomap typer alla hello möjliga tooa fält. 
2. Uppdatering av hello kapslade objekt kräver vissa extra arbete toodetermine exakt vad som måste uppdateras i hello Azure Search index toobe

## <a name="sample-code"></a>Exempelkod
Du kan se ett exempel på inställningarna i tooindex komplexa JSON-data i Azure Search och genomföra ett antal frågor via den här datauppsättningen på den här [GitHub-repo-](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Nästa steg
[Rösta på inbyggt stöd för komplexa datatyper](https://feedback.azure.com/forums/263029-azure-search) på hello Azure Search UserVoice sidan och ange några ytterligare indata som du vill att oss tooconsider om funktionen implementering. Du kan också nå ut toome direkt på Twitter på @liamca.

