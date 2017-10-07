---
title: "aaaAzure SQL-databas benchmark-översikt"
description: "Det här avsnittet beskriver hello Azure SQL Database Benchmark används vid mätning hello prestanda i Azure SQL Database."
services: sql-database
documentationcenter: na
author: jan-eng
manager: jhubbard
editor: monicar
ms.assetid: e26f8a66-2c12-49d7-8297-45b4d48a5c01
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/21/2016
ms.author: janeng
ms.openlocfilehash: 1024e9ada511935f911cb1345b4dc5508997c702
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL Database benchmark-översikt
## <a name="overview"></a>Översikt
Microsoft Azure SQL Database erbjuder tre [tjänstnivåer](sql-database-service-tiers.md) med flera prestandanivåer. Varje prestandanivå ger en ökande uppsättning resurser eller ”power” utformats toodeliver allt högre genomströmning.

Det är viktigt toobe kan tooquantify hur hello ökande kraften i varje prestandanivå översätter till ökad databasens prestanda. Microsoft har utvecklat toodo hello Azure SQL Database Benchmark (ASDB). hello benchmark utför en blandning av grundläggande åtgärder som finns i alla OLTP-arbetsbelastningar. Vi mäter hello genomströmning uppnås för databaser som körs i varje prestandanivå.

hello resurser och kraften i varje servicenivå för tjänstnivå och prestandanivå uttrycks i [Database Transaction Units (Dtu)](sql-database-what-is-a-dtu.md). Dtu: er att toodescribe hello relativa kapacitet för prestandanivå baserat på ett blandat mått av CPU, minne, och läser och skriver priser som erbjuds av varje prestandanivå. Dubblerade hello DTU klassificering av en databas är lika toodoubling hello databasen power. hello benchmark gör att vi tooassess hello effekt på databasens prestanda för hello öka power som erbjuds av varje prestandanivå genom faktiska databasåtgärder vid skalning databasens storlek, antal användare och priser för transaktionen i proportion toohello resurserna toohello databas.

Genom att ange hello genomströmning av grundläggande hello tjänstnivå som använder transaktioner per timme hello Standard-tjänstnivå med hjälp av transaktioner per minut och hello premiumnivån använder transaktioner per sekund, gör det enklare tooquickly relaterar hello potentialen för varje nivå toohello tjänstekraven för ett program.

## <a name="correlating-benchmark-results-tooreal-world-database-performance"></a>Korrelerar benchmark resultat tooreal world databasprestanda
Det är viktigt toounderstand som ASDB som alla prestandamått är representativa och som endast. Hej transaktion priser uppnås med hello benchmark programmet inte kommer att hello samma som de som kan uppnås med andra program. hello benchmark omfattar en mängd olika transaktion typer körs mot ett schema som innehåller en uppsättning tabeller och -datatyper. Medan hello benchmark övningarna hello grundläggande åtgärder som är vanliga tooall OLTP-arbetsbelastningar, representerar inte någon specifik klass av databas eller ett program. hello målet med hello benchmark är tooprovide en rimlig guiden toohello relativa prestandan för en databas som kan förväntas vid skalning uppåt eller nedåt mellan prestandanivåer. I själva verket databaser av olika storlekar och komplexitet, får olika kombinationer av arbetsbelastningar och kommer att svara på olika sätt. Till exempel ett i/o-intensiva program kan träffa IO tröskelvärden snabbare eller ett processorintensiva program kan träffa CPU gränser snabbare. Det finns ingen garanti för att en viss databas skalar på samma sätt som hello benchmark stigande ladda hello.

hello prestandamått och dess metoder beskrivs i detalj nedan.

## <a name="benchmark-summary"></a>Benchmark-översikt
ASDB mäter hello prestanda för en blandning av grundläggande databasåtgärder som förekommer oftast i online transaktionsbearbetning (OLTP) arbetsbelastningar. Även om hello benchmark är utformat med cloud computing-lösningar i åtanke, har hello databasschemat, ifyllning av data och transaktioner utformad toobe brett representerar hello grundläggande element som används mest i OLTP-arbetsbelastningar.

## <a name="schema"></a>Schemat
hello-schemat är utformad toohave tillräckligt med olika och komplexiteten toosupport ett brett spektrum av åtgärder. hello benchmark körs mot en databas som består av sex tabeller. hello tabeller delas in i tre kategorier: fast storlek, skala och växer. Det finns två tabeller i fast storlek. tre skalning tabeller. och en växande tabell. Fast storlek tabeller har ett konstant antal rader. Skalning tabeller har en kardinalitet som är proportionell toodatabase prestanda, men inte ändra under hello prestandamått. hello växande tabell storlek som en skalning tabell på första last, men sedan hello kardinalitet ändringar i hello loppet av körs hello prestandamått som rader infogas och tas bort.

hello schemat innehåller en blandning av datatyper, inklusive heltal, numeriska tecken och datum/tid. hello-schemat innehåller primära och sekundära nycklarna, men inte alla sekundärnycklar - finns det ingen referensintegritetsbegränsningar mellan tabeller.

Ett program för generering av data genererar hello data för inledande hello-databasen. Heltal och numeriska data som genereras med olika strategier. I vissa fall kan distribueras värden slumpmässigt över ett intervall. I annat fall är en uppsättning värden slumpmässigt den omkastade tooensure att en specifik distributionsplats bibehålls. Textfält genereras från en sorterad lista över ord tooproduce realistiska söker data.

hello-databasen är storlek baserat på ”skalfaktor”. hello skalfaktor (förkortas SA) anger hello kardinalitet för hello skalning och växande tabeller. Som beskrivs nedan i hello avsnittet användare och Pacing, hello databasens storlek, antal användare och högsta prestanda alla skala i andel tooeach andra.

## <a name="transactions"></a>Transaktioner
hello arbetsbelastningen består av nio transaktionstyper enligt hello tabellen nedan. Varje transaktion är utformad toohighlight en viss uppsättning egenskaper i hello database engine och system maskinvara, hello med hög kontrast från andra transaktioner. Den här metoden gör det enklare tooassess hello effekten av olika komponenter toooverall prestanda. Hello transaktion ”Läs tunga” producerar till exempel ett stort antal läsåtgärder från disken.

| Transaktionstyp | Beskrivning |
| --- | --- |
| Läs Lite |VÄLJ; i minnet; skrivskyddad |
| Läs medel |VÄLJ; mestadels i minnet; skrivskyddad |
| Läs aktiverat |VÄLJ; oftast inte i minnet; skrivskyddad |
| Uppdatera Lite |UPPDATERA; i minnet; skrivskyddad |
| Uppdatera aktiverat |UPPDATERA; oftast inte i minnet; skrivskyddad |
| Infoga Lite |INFOGA. i minnet; skrivskyddad |
| Infoga aktiverat |INFOGA. oftast inte i minnet; skrivskyddad |
| Ta bort |TA BORT. blandning av i minnet och inte i minnet; skrivskyddad |
| CPU-aktiverat |VÄLJ; i minnet; relativt hög CPU-belastning; skrivskyddad |

## <a name="workload-mix"></a>Blandning av arbetsbelastning
Transaktioner väljs slumpmässigt från en viktad distributionsplats med följande övergripande blandning hello. hello har övergripande blandning ett förhållande för läsning och skrivning på cirka 2:1.

| Transaktionstyp | % av blandning |
| --- | --- |
| Läs Lite |35 |
| Läs medel |20 |
| Läs aktiverat |5 |
| Uppdatera Lite |20 |
| Uppdatera aktiverat |3 |
| Infoga Lite |3 |
| Infoga aktiverat |2 |
| Ta bort |2 |
| CPU-aktiverat |10 |

## <a name="users-and-pacing"></a>Användare och hastighetsstyrningen
hello benchmark arbetsbelastning drivs från ett verktyg som skickar transaktioner över en uppsättning anslutningar toosimulate hello beteendet för ett antal samtidiga användare. Även om alla hello anslutningar och transaktioner är datorn som genererats för enkelhetens skull refererar vi toothese anslutningar som ”användare”. Även om varje användare fungerar oberoende av andra användare, alla användare utför hello samma cykel steg som visas nedan:

1. Upprätta en databasanslutning.
2. Upprepa tills signalerat tooexit:
   * Välj en transaktion slumpmässigt (från en viktad distributionsplats).
   * Utför hello markerad transaktion och mått hello svarstid.
   * Vänta tills en pacing fördröjning.
3. Stäng hello databasanslutning.
4. Avsluta.

Hej hastighetsstyrningen fördröjningen (i steg 2c) väljs slumpmässigt, men med en distributionsplats som har 1.0 sekund i genomsnitt. Därmed kan varje användare i genomsnitt generera maximalt en transaktion per sekund.

## <a name="scaling-rules"></a>Skalning regler
hello antalet användare som bestäms av hello databasens storlek (i skalfaktor enheter). Det finns en användare för varje fem skalfaktor enheter. På grund av hello hastighetsstyrningen fördröjning, kan en användare skapa maximalt en transaktion per sekund i genomsnitt.

Till exempel en-skalfaktor 500 (SA = 500) databasen har 100 användare och kan uppnå högst 100 Transaktionsprogram. toodrive en högre hastighet Transaktionsprogram kräver fler användare och en större databas.

hello tabellen nedan visar hello antalet användare som faktiskt råkat ut för varje servicenivå för tjänstnivå och prestandanivå.

| Tjänstnivån (prestandanivå) | Användare | Databasstorlek |
| --- | --- | --- |
| Basic |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1-GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6/P3) |800 |114 GB |

## <a name="measurement-duration"></a>Mätning varaktighet
En giltig benchmark-körning kräver minst en timme stabilitet mätning varaktighet.

## <a name="metrics"></a>Mått
hello nyckelvärden i hello benchmark är genomflöde och svarstid.

* Dataflödet är hello grundläggande prestandamått i hello prestandamått. Genomströmning rapporteras i transaktioner per i-tid, inventering av alla transaktionstyper av.
* Svarstiden är ett mått på prestanda, förutsägbarhet. hello svar tidsbegränsning varierar beroende på typ av tjänster med högre klasser av tjänsten som har en strängare svar tiden som krävs, enligt nedan.

| Tjänsteklass | Genomströmning mått | Tiden som krävs för svaret |
| --- | --- | --- |
| Premium |Transaktioner per sekund |95: e percentilen 0,5 sekunder |
| Standard |Transaktioner per minut |90: e percentilen 1.0 sekunder |
| Basic |Transaktioner per timme |80: e percentilen 2.0 sekunder |

## <a name="conclusion"></a>Slutsats
hello Azure SQL Database Benchmark mäter hello relativa prestandan för Azure SQL-databas som körs över hello intervallet för tillgängliga servicenivåer och prestandanivåer. hello benchmark utför en blandning av grundläggande databasåtgärder som förekommer oftast i online transaktionsbearbetning (OLTP) arbetsbelastningar. Genom att mäta faktiska prestanda ger hello benchmark ett mer beskrivande utvärdering av hello effekt på genomflödet i ändra hello prestandanivå än vad som är möjligt genom att bara ange hello-resurser som tillhandahålls av varje nivå, till exempel CPU-hastighet, minne och IOPS . I framtida hello, kommer fortsätta tooevolve hello benchmark toobroaden sitt omfång och expandera hello informationen.

## <a name="resources"></a>Resurser
[Introduktion tooSQL databas](sql-database-technical-overview.md)

[Servicenivåer och prestandanivåer](sql-database-service-tiers.md)

[Prestandaråd för enskilda databaser](sql-database-performance-guidance.md)
