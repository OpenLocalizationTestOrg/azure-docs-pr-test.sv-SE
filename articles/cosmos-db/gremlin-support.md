---
title: aaaAzure Cosmos DB Gremlin support | Microsoft Docs
description: "Lär dig mer om hello Gremlin språk från Apache TinkerPop som funktioner och steg och är tillgängliga i Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 6016ccba-0fb9-4218-892e-8f32a1bcc590
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/10/2017
ms.author: denlee
ms.openlocfilehash: f8c2ce50c6946e971f56fe1f3838b0899cb2ad8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Stöd för Azure Cosmos DB Gremlin diagram
Har stöd för Azure Cosmos-DB [Apache Tinkerpop](http://tinkerpop.apache.org) kurva traversal språk [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), vilket är en Graph API för att skapa diagram entiteter och utför åtgärder i diagrammet frågan. Du kan använda hello Gremlin språk toocreate diagram entiteter (formhörnen och kanter), ändra egenskaper i dessa enheter, utföra frågor och traversals och ta bort enheter. 

Azure Cosmos-DB ger funktioner för enterprise-redo toograph databaser. Detta inkluderar global distributionsplatsen, oberoende skalning av lagring och dataflöde, förutsägbar en siffra millisekunders latens, automatisk indexering och 99,99% SLA: er. Eftersom Azure Cosmos DB stöder TinkerPop/Gremlin kan migrera du enkelt program som skrivits med en annan graph-databas utan att behöva toomake kodändringar. Dessutom tack vare stöd för Gremlin Azure Cosmos DB smidigt kan integreras med TinkerPop-aktiverade analytics ramverk som [Apache Spark GraphX](http://spark.apache.org/graphx/). 

Vi ger en snabb genomgång av Gremlin i den här artikeln och räkna upp hello Gremlin funktioner och åtgärder som stöds i hello förhandsgranskning av Graph API-stöd.

## <a name="gremlin-by-example"></a>Gremlin efter exempel
Vi använder en exempel diagram toounderstand hur frågor kan uttryckas i Gremlin. hello visar följande bild ett affärsprogram som hanterar data om användare, enheter i hello form av ett diagram.  

![Visar personer, enheter och intressen exempeldatabasen](./media/gremlin-support/sample-graph.png) 

Det här diagrammet har hello följande vertex typer (kallas ”etikett” i Gremlin):

- Personer: hello diagrammet har tre personer Robin, Thomas och Ben
- Intressen: deras intressen i det här exemplet hello spel Football
- : Hello enheter som används
- Operativsystem: hello-operativsystem som hello enheter körs på

Vi representerar hello relationer mellan dessa enheter via hello följande edge typer/etiketter:

- Vet: till exempel ”Thomas vet Robin”
- Berörda: toorepresent hello intressen hello personer i vår diagram, till exempel ”Ben är intresserad av Football”
- RunsOS: Bärbara datorer kör hello Windows-Operativsystemet
- Användning: toorepresent vilken enhet som en person använder. Robin använder exempelvis en Motorola telefon med serienummer 77

Kör vi några åtgärder mot det här diagrammet med hello [Gremlin konsolen](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console). Du kan också utföra dessa åtgärder med hjälp av Gremlin drivrutiner i hello platform önskat (Java, Node.js, Python eller .NET).  Innan vi tittar på det här stöds i Azure Cosmos DB ska vi titta på några exempel tooget bekant med hello syntax.

Först ska vi titta på CRUD. hello infogar följande Gremlin instruktion hello ”Thomas” hörn i hello diagram:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Därefter infogar hello följande Gremlin instruktionen en ”vet” kant mellan Thomas och Robin.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

hello returnerar följande fråga hello ”person” formhörnen i fallande ordning med deras förnamn:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Där diagram skiner finns när du behöver som tooanswer frågor ”vilka operativsystem vänners Thomas använder”?. Du kan köra den här enkla Gremlin traversal tooget informationen från hello diagram:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Nu ska vi titta på Azure Cosmos DB tillhandahåller för Gremlin utvecklare.

## <a name="gremlin-features"></a>Gremlin funktioner
TinkerPop är en standard som omfattar en mängd olika tekniker för diagrammet. Det måste därför standard terminologi toodescribe vilka funktioner som tillhandahålls av en provider i diagrammet. Azure Cosmos-DB tillhandahåller en beständig, hög samtidighet, skrivbara graph-databas som kan vara partitionerad över flera servrar eller kluster. 

hello visar följande tabell hello TinkerPop funktioner som implementeras av Azure Cosmos DB: 

| Kategori | Azure DB Cosmos-implementering |  Anteckningar | 
| --- | --- | --- |
| Diagram-funktioner | Ger beständighet och ConcurrentAccess i förhandsgranskningen. Utformad toosupport transaktioner | Datorn metoder kan implementeras via hello Spark koppling. |
| Variabeln funktioner | Stöder boolesk, heltal, Byte, Double, Float, Integer, Long, sträng | Har stöd för primitiva typer, är kompatibel med komplexa typer via datamodellen |
| Vertex funktioner | Stöder RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty  | Har stöd för att skapa, ändra och ta bort formhörnen |
| Vertex egenskapen funktioner | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Har stöd för att skapa, ändra och ta bort vertex egenskaper |
| Edge-funktioner | AddEges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Har stöd för att skapa, ändra och ta bort kanter |
| Edge-egenskapen funktioner | Egenskaper för BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Har stöd för att skapa, ändra och ta bort gräns egenskaper |

## <a name="gremlin-wire-format-graphson"></a>Gremlin kabelformat: GraphSON

Använder Azure Cosmos-DB hello [GraphSON format](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) när du returnerar resultat från Gremlin åtgärder. GraphSON är hello Gremlin standardformat för att representera formhörnen och kanter (egenskaper enkla och flera värden) med JSON. 

Hello följande utdrag visar exempelvis en GraphSON representation av en brytpunkt *returnerade toohello klienten* från Azure Cosmos-databasen. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

hello-egenskaper som används av GraphSON för formhörnen är hello följande:

| Egenskap | Beskrivning |
| --- | --- |
| id | hello-ID för hello hörn. Måste vara unika (i kombination med hello värdet av _partition om tillämpligt) |
| Etikett | hello etikett för hello hörn. Detta är valfria och används toodescribe hello entitetstypen. |
| typ | Använda toodistinguish formhörnen från icke-diagram-dokument |
| properties | Egenskapsuppsättning med associerade hello vertex användardefinierade egenskaper. Varje egenskap kan ha flera värden. |
| _partition (konfigureras) | hello partitionsnyckel för hello hörn. Kan vara används tooscale ut diagram toomultiple servrar |
| outE | Innehåller en lista över ut kanter från en nod. Lagra hello angränsande information med vertex möjliggör snabb körning av traversals. Kanter är grupperade baserat på deras etiketter. |

Och hello edge innehåller hello följande information toohelp med navigering tooother delar av hello diagram.

| Egenskap | Beskrivning |
| --- | --- |
| id | hello-ID för hello kant. Måste vara unika (i kombination med hello värdet av _partition om tillämpligt) |
| Etikett | hello etiketten hello kant. Den här egenskapen är valfria och används toodescribe hello relationstyp. |
| inV | Innehåller en lista över på formhörnen för en kant. Lagra hello angränsande information med hello edge möjliggör snabb körning av traversals. Formhörnen är grupperade baserat på deras etiketter. |
| properties | Egenskapsuppsättning för användardefinierade egenskaper som är associerade med hello kant. Varje egenskap kan ha flera värden. |

Varje egenskap kan lagra flera värden i en matris. 

| Egenskap | Beskrivning |
| --- | --- |
| värde | hello-värdet för hello-egenskapen

## <a name="gremlin-partitioning"></a>Gremlin partitionering

I Azure Cosmos DB diagram lagras i behållare som kan skalas oberoende vad gäller lagring och genomströmning (uttryckt i normaliserade begäranden per sekund). Varje behållare måste definiera en valfri men rekommenderad partition nyckelegenskapen som anger en logisk partition gräns för relaterade data. Varje vertex/kanten måste ha en `id` egenskap som är unik för entiteter i partitionen nyckelvärdet. hello information beskrivs i [partitionering i Azure Cosmos DB](partition-data.md).

Gremlin åtgärderna fungerar sömlöst över diagramdata som sträcker sig över flera partitioner i Azure Cosmos-databasen. Det är dock rekommenderas toochoose en partitionsnyckel för ditt diagram som ofta används som filter i frågor har flera separata värden och liknande frekvensen för åtkomst till dessa värden. 

## <a name="gremlin-steps"></a>Gremlin steg
Nu ska vi titta på hello Gremlin steg som stöds av Azure Cosmos DB. En fullständig referens om Gremlin finns [TinkerPop referens](http://tinkerpop.apache.org/docs/current/reference).

| Steg | Beskrivning | TinkerPop 3.2-dokumentation | Anteckningar |
| --- | --- | --- | --- |
| `addE` | Lägger till en kant mellan två formhörnen | [addE steg](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) | |
| `addV` | Lägger till ett hörn toohello diagram | [addV steg](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) | |
| `and` | Ensurest att alla hello traversals returnera ett värde | [och steg](http://tinkerpop.apache.org/docs/current/reference/#and-step) | |
| `as` | Ett steg modulator tooassign en variabel toohello utdata från ett steg | [steg](http://tinkerpop.apache.org/docs/current/reference/#as-step) | |
| `by` | Ett steg modulator som används med `group` och`order` | [för steg](http://tinkerpop.apache.org/docs/current/reference/#by-step) | |
| `coalesce` | Returnerar hello första traversal som returnerar ett resultat | [sammanslagning av steg](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) | |
| `constant` | Returnerar ett konstant värde. Används med`coalesce`| [konstant steg](http://tinkerpop.apache.org/docs/current/reference/#constant-step) | |
| `count` | Returnerar antalet hello från hello traversal | [antal steg](http://tinkerpop.apache.org/docs/current/reference/#count-step) | |
| `dedup` | Returnerar hello värden med borttagna hello dubbletter | [dedupliceringen steg](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) | |
| `drop` | Således hello värden (vertex/kant) | [ta bort steg](http://tinkerpop.apache.org/docs/current/reference/#drop-step) | |
| `fold` | Fungerar som en barriären som beräknar hello aggregering av resultat| [vikning steg](http://tinkerpop.apache.org/docs/current/reference/#fold-step) | |
| `group` | Grupper hello värden baserat på hello etiketter som angetts| [grupp steg](http://tinkerpop.apache.org/docs/current/reference/#group-step) | |
| `has` | Använda toofilter egenskaper, formhörnen och kanter. Stöder `hasLabel`, `hasId`, `hasNot`, och `has` varianter. | [har steg](http://tinkerpop.apache.org/docs/current/reference/#has-step) | |
| `inject` | Mata in värden i en dataström| [mata in steg](http://tinkerpop.apache.org/docs/current/reference/#inject-step) | |
| `is` | Används tooperform ett filter som använder ett booleskt uttryck | [är steg](http://tinkerpop.apache.org/docs/current/reference/#is-step) | |
| `limit` | Använda toolimit antal objekt i hello traversal| [gränsen steg](http://tinkerpop.apache.org/docs/current/reference/#limit-step) | |
| `local` | Lokal packar ett avsnitt i en underfråga traversal, liknande tooa | [lokala steg](http://tinkerpop.apache.org/docs/current/reference/#local-step) | |
| `not` | Används tooproduce hello negationer av ett filter | [inte steg](http://tinkerpop.apache.org/docs/current/reference/#not-step) | |
| `optional` | Returnerar hello resultatet av hello angetts traversal om den ger ett annat resultat returneras hello anropa element | [valfritt steg](http://tinkerpop.apache.org/docs/current/reference/#optional-step) | |
| `or` | Garanterar att minst en av hello traversals returnerar ett värde | [eller -steg](http://tinkerpop.apache.org/docs/current/reference/#or-step) | |
| `order` | Returnerar resultat i hello angetts sorteringsordning | [ordning steg](http://tinkerpop.apache.org/docs/current/reference/#order-step) | |
| `path` | Returnerar hello fullständig sökväg till hello traversal | [sökvägen steg](http://tinkerpop.apache.org/docs/current/reference/#path-step) | |
| `project` | Projekt hello egenskaper som en karta | [projektet steg](http://tinkerpop.apache.org/docs/current/reference/#project-step) | |
| `properties` | Returnerar hello egenskaper för hello angett etiketter | [Egenskaper för-steg](http://tinkerpop.apache.org/docs/current/reference/#properties-step) | |
| `range` | Filter toohello angetts värdeintervallet| [intervallet steg](http://tinkerpop.apache.org/docs/current/reference/#range-step) | |
| `repeat` | Upprepas hello steg för hello angivet antal gånger. Används för upprepning | [Upprepa steg](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) | |
| `sample` | Används toosample resultat från hello traversal | [exempel steg](http://tinkerpop.apache.org/docs/current/reference/#sample-step) | |
| `select` | Används tooproject resultat från hello traversal |  [Välj steg](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Används för icke-blockerande mängder från hello traversal | [lagra steg](http://tinkerpop.apache.org/docs/current/reference/#store-step) | |
| `tree` | Sammanställd sökvägar från en nod i ett träd | [trädet steg](http://tinkerpop.apache.org/docs/current/reference/#tree-step) | |
| `unfold` | Unroll en iterator som ett steg| [vika steg](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) | |
| `union` | Sammanfoga resultat från flera traversals| [Union steg](http://tinkerpop.apache.org/docs/current/reference/#union-step) | |
| `V` | Innehåller hello nödvändiga steg för att traversals mellan hörn och kanter `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV`, och `otherV` för | [Vertex steg](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) | |
| `where` | Använda toofilter resultat från hello traversal. Stöder `eq`, `neq`, `lt`, `lte`, `gt`, `gte`, och `between` operatörer  | [steg där](http://tinkerpop.apache.org/docs/current/reference/#where-step) | |

Azure Cosmos-write-optimerad databasmotor har stöd för automatisk indexering av alla egenskaper i formhörnen och kanter som standard. Därför frågor med filter kan variera frågor, sortering eller mängder om en egenskap bearbetas från hello index och hanteras effektivt. Mer information om hur indexering fungerar i Azure Cosmos-databasen finns på vår [schema-oberoende indexering](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Nästa steg
* Komma igång med att skapa ett program i diagrammet [med hjälp av vår SDK](create-graph-dotnet.md) 
* Lär dig mer om [Azure Cosmos DB graph-support](graph-introduction.md)
