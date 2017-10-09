---
title: 'Introduktion tooAzure Cosmos DB Graph API: er | Microsoft Docs'
description: "Lär dig hur du kan använda Azure Cosmos DB toostore, fråga och bläddra massiv diagram med låg latens med frågespråk för hello hello Gremlin diagram av Apache TinkerPop."
services: cosmos-db
author: dennyglee
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/21/2017
ms.author: denlee
ms.openlocfilehash: a4e79a4aa27976966baf0554928026177991ff69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-graph-api"></a>Introduktion tooAzure Cosmos DB: Graph API

[Azure Cosmos-DB](introduction.md) är hello globalt distribuerad databas som flera modellen tjänst från Microsoft för verksamhetskritiska program. Azure Cosmos-DB tillhandahåller [nyckelfärdig global distributionsplatsen](distribute-data-globally.md), [elastisk skalbarhet av dataflöden och lagringsutrymmen](partition-data.md) över hela världen, en siffra millisekunders latens vid hello 99th percentilen, [fem väldefinierade konsekvensnivåer](consistency-levels.md), och garanteras hög tillgänglighet, som alla backas upp av [branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos-DB [indexerar automatiskt data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) utan att du toodeal med hantering av schemat och index. Det stöder flera modeller och dokument, nyckelvärde graf och kolumndatamodeller.

![Gremlin, diagram och Azure Cosmos DB](./media/graph-introduction/graph-gremlin.png)

hello Azure Cosmos DB Graph API innehåller:

- Diagrammet modellering
- Traversal API: er
- Nyckelfärdig global distributionsplatsen
- Elastisk skalning av lagring och dataflöde med mindre än 10 ms läsa svarstiderna och mindre än 15 ms på hello 99th percentil
- Automatisk indexering med snabbmeddelanden frågan tillgänglighet
- Justerbara konsekvensnivåer
- Omfattande SLA inklusive 99,99% tillgänglighet

tooquery Azure Cosmos DB, kan du använda hello [Apache TinkerPop](http://tinkerpop.apache.org) kurva traversal språk [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), eller andra TinkerPop-kompatibel graph-system som [Apache Spark GraphX](spark-connector-graph.md).

Den här artikeln innehåller en översikt över hello Azure Cosmos DB Graph API och förklarar hur du kan använda den toostore massiv diagram med miljarder hörn och kanter. Du kan fråga hello diagram med millisekunds fördröjning och enkelt utvecklas hello diagrammet struktur och schema.

## <a name="graph-database"></a>Diagram-databas
Data är som det visas i hello verkligheten naturligt kopplade. Traditionella modellering fokuserar på enheter. För många program finns också en behovet toomodel eller toomodel både enheter och relationer naturligt.

En [diagram](http://mathworld.wolfram.com/Graph.html) är en struktur som består av [formhörnen](http://mathworld.wolfram.com/GraphVertex.html) och [kanter](http://mathworld.wolfram.com/GraphEdge.html). Både formhörnen och kanter kan ha ett godtyckligt antal egenskaper. Formhörnen betecknar diskreta objekt som en person, en plats eller en händelse. Kanter anger relationer mellan formhörnen. Till exempel kan en person känna av en annan person, vara inblandad i en händelse och nyligen använts på en plats. Egenskaper för snabba och information om hello formhörnen och kanter. Exempel egenskaper innehåller en nod som har ett namn, ålder och kant som har en tidsstämpel och/eller en vikt. Mer formellt den här modellen kallas en [egenskapen diagram](http://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos-DB stöder hello egenskapen diagrammet modell.

Till exempel exempel hello följande diagram visar relationerna mellan personer, mobila enheter, intressen och operativsystem.

![Visar personer, enheter och intressen exempeldatabasen](./media/graph-introduction/sample-graph.png)

Diagram är användbara toounderstand en mängd olika datauppsättningar i vetenskap, teknik och företag. Diagrammet databaser kan du modellera och lagra diagram naturligt och effektivt, vilket gör dem användbart i många fall. Diagrammet databaser är vanligtvis NoSQL-databaser, eftersom dessa används ofta också fall måste schemaflexibilitet och snabb iteration.

Diagram erbjuder en bok och kraftfull datamodeller tekniken. Men detta ensamt är inte en tillräcklig orsak toouse en graph-databas. För många användningsområden och mönster som omfattar diagram traversals överträffar diagram traditionella SQL och NoSQL-databaser genom att i storlek. Denna skillnad i prestanda är ytterligare förökas när det rör sig över mer än en relation som friend i en vän.

Du kan kombinera hello snabb traversals diagram databaser har med diagrammet algoritmer som djup första sökning, breda första Sök och Dijkstras algoritmen, toosolve problem i olika domäner som sociala nätverk, innehållshantering, geospatiala, och rekommendationer.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Planeten skala diagram med Azure Cosmos DB
Azure Cosmos-DB är en helt hanterad graph-databas som erbjuder global distributionsplatsen, elastisk skalbarhet av lagring och dataflöde, automatisk indexering och fråga, justerbara konsekvensnivåer och stöd för hello TinkerPop standard.  

![Azure DB Cosmos-diagram-arkitektur](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos-DB erbjuder följande hello differentierade funktioner jämfört med tooother diagram databaser i hello marknaden:

* Elastiska och skalbara dataflöden och lagring

 Diagram i verkligheten hello måste tooscale utöver hello kapacitet för en enskild server. Med Azure Cosmos DB kan du skala ditt diagram sömlöst över flera servrar. Du kan även skala hello genomflödet i diagrammet oberoende baserat på ditt åtkomstmönster. Azure Cosmos-DB stöder graph-databaser som kan skalas toovirtually obegränsade lagringsstorlekar och dataflöde.

* Flera regioner replikering

 Azure Cosmos-DB replikerar transparent ditt diagram tooall dataområden som du har kopplat till ditt konto. Replikering kan du toodevelop program som kräver global åtkomst toodata. Det finns kompromisser i hello områden av konsekvens, tillgänglighet och prestanda och motsvarande garantier. Azure Cosmos-DB tillhandahåller transparent regional växling vid fel med flera API: er. Du kan skala dataflöden och lagringsutrymmen Elastiskt över hello jordglob.

* Snabb frågor och traversals med välbekanta Gremlin syntax

 Lagra heterogena hörn och kanter och fråga dokumenten via en bekant Gremlin syntax. Azure Cosmos-DB använder en samtidig frigöra lås, loggstrukturerad indexering teknik tooautomatically index allt innehåll. Den här funktionen möjliggör omfattande förfrågningar i realtid och traversals utan hello måste toospecify schematips, sekundärindex eller vyer. Läs mer i [frågan diagram med hjälp av Gremlin](gremlin-support.md).

* Helt förvaltad

 Azure Cosmos-DB eliminerar hello måste toomanage databasen och datorresurser. Som en helt hanterad Microsoft Azure-tjänst har inte behöver toomanage virtuella datorer, distribuera och konfigurera programvara, hantera skalning eller hantera komplexa datanivå uppgraderingar. Varje diagram säkerhetskopieras och skyddas mot regionala fel automatiskt. Du kan enkelt lägga till ett Azure DB som Cosmos-konto och etablera kapacitet när du behöver den så att du kan fokusera på ditt program i stället för att använda och hantera din databas.

* Automatisk indexering

 Standard Azure Cosmos DB automatiskt indexerar alla hello egenskaper i noder och kanter i hello diagram och inte förväntar sig eller kräver något schema eller att sekundärindex.

* Kompatibilitet med Apache TinkerPop

 Azure Cosmos-DB internt stöder hello öppen källkod Apache TinkerPop standard och kan integreras med andra diagram TinkerPop-aktiverade system. Du kan därför enkelt migrera från en annan graph-databas, exempel Titan eller Neo4j, eller använda Azure Cosmos DB med diagrammet analytics ramverk som [Apache Spark GraphX](spark-connector-graph.md).

* Justerbara konsekvensnivåer

 Välj fem väldefinierade konsekvenskontroll nivåer tooachieve optimal kompromiss mellan konsekvens och prestanda. Azure Cosmos DB erbjuder fem olika konsekvensnivåer för frågor och läsåtgärder: stark, bunden utgång, session, enhetligt prefix och slutlig. Dessa detaljerade, väldefinierade konsekvensnivåerna kan toomake ljud kompromisserna konsekvens, tillgänglighet och svarstid. Läs mer i [med konsekvenskontroll nivåer toomaximize tillgänglighet och prestanda i DocumentDB](consistency-levels.md).

Azure Cosmos-DB kan också använda flera modeller som dokument och diagram, inom hello samma behållare-databaser. Du kan använda en dokumentet samling toostore diagramdata bredvid dokument. Du kan använda båda SQL-frågor via JSON och Gremlin frågor tooquery hello samma data som ett diagram.

## <a name="getting-started"></a>Komma igång
Du kan använda hello Azure-kommandoradsgränssnittet (CLI), Azure Powershell eller hello Azure-portalen med stöd för graph API toocreate Azure Cosmos DB konton. När du har skapat konton hello Azure-portalen innehåller en tjänstslutpunkt som `https://<youraccount>.graphs.azure.com`, som ger en WebSocket-klientdel för Gremlin. Du kan konfigurera din TinkerPop-kompatibel-verktyg som hello [Gremin konsolen](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), tooconnect toothis slutpunkt och bygga program i Java, Node.js och eventuella Gremlin klientdrivrutinen.

hello följande tabell visar populära Gremlin drivrutiner som du kan använda mot Azure Cosmos DB:

| Ladda ned | Dokumentation |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin JavaScript på Github](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin konsolen](https://tinkerpop.apache.org/downloads.html) |[TinkerPop dokument](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos-DB tillhandahåller också ett .NET-bibliotek som har Gremlin tilläggsmetoder ovanpå hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) via NuGet. Det här biblioteket innehåller en Gremlin ”pågående”-server som du kan använda tooconnect direkt tooDocumentDB datapartitioner.

| Ladda ned | Dokumentation |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Med hjälp av hello [Azure Cosmos DB emulatorn](local-emulator.md), du kan använda hello Graph API toodevelop och testa lokalt utan att skapa en Azure-prenumeration eller kostnader. När du är nöjd med hur programmet fungerar i hello emulatorn kan växla du toousing ett Azure DB som Cosmos-konto i hello molnet.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Scenarier för diagram stöd för Azure Cosmos DB
Här följer några scenarier där diagrammet stöd för Azure Cosmos DB kan användas:

* Sociala nätverk

 Genom att kombinera data om dina kunder och deras interaktioner med andra personer kan du utveckla anpassade upplevelser, förutsäga kunden beteende eller ansluter personer med andra med liknande intressen. Azure Cosmos-DB kan använda toomanage sociala nätverk och spåra kundinställningar och data.

* Rekommendation motorer

 Det här scenariot används ofta i hello detaljhandel. Du kan skapa anpassade rekommendationer genom att kombinera information om produkter, användare och användarinteraktioner som köp, bläddra eller klassificeringen av ett objekt. Hej låg latens, elastisk skalbarhet och inbyggda diagram stöd för Azure Cosmos DB är idealisk för modellering dessa interaktioner.

* Geospatial

 Många program i telekommunikation logistik och planera din resa måste toofind en plats av intresse inom ett område eller hitta hello kortaste/optimala väg mellan två platser. Azure Cosmos-DB är en fysisk anpassning för dessa problem.

* Internet of Things

 Med hello nätverks- och anslutningar mellan IoT-enheter som modellerats som ett diagram du skapa en bättre förståelse för hello tillståndet för dina enheter och resurser och lära dig hur ändringar i en del av hello nätverket kan potentiellt påverkar en annan del.

## <a name="next-steps"></a>Nästa steg
toolearn mer om graph-stöd i Azure Cosmos DB finns:

* Kom igång med hello [Azure Cosmos DB diagrammet kursen](create-graph-dotnet.md).
* Mer information om hur för[fråga diagram i Azure Cosmos-databasen med Gremlin](gremlin-support.md).
