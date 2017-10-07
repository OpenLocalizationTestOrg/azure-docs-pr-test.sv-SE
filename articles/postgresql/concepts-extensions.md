---
title: "aaaUsing PostgreSQL tillägg i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver hello möjlighet tooextend hello funktioner i databasen med tillägg i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: af2462d7a923b934bc0329153be7079ba86e8856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-tillägg i Azure PostgreSQL-databas
PostgreSQL innehåller hello möjlighet tooextend hello funktioner för din databas med hjälp av tillägg. Tillägg kan för flera relaterade SQL-objekt-toobe buntas ihop i ett paket och läsas in eller tas bort från databasen med ett enda kommando. Tillägg som är inlästa i hello-databasen fungerar precis som funktioner som ingår i. Mer information om PostgreSQL-tillägg finns [paketering relaterade objekt i ett tillägg](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-toouse-postgresql-extensions"></a>Hur toouse PostgreSQL tillägg?
PostgreSQL tillägg måste toobe installerad för din databas innan du kan använda dem. tooinstall ett visst tillägg, körs det [skapa tillägg](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) kommando från psql verktyget tooload hello paketerade objekt i databasen.

Azure-databas för PostgreSQL stöder en delmängd av nyckeln tillägg som anges här. Andra tillägg stöds inte för utöver hello som visas. Du kan skapa dina egna tillägg med Azure-databas för PostgreSQL-tjänsten.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Tillägg som stöds av Azure-databas för PostgreSQL
hello följande tabeller listan hello PostgreSQL standardtillägg som för närvarande stöds av Azure-databas för PostgreSQL. Du kan också hämta den här informationen genom att fråga pg\_tillgängliga\_tillägg. 

### <a name="data-types-extensions"></a>Datatyperna tillägg

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Ger en strängtyp skiftlägeskänsliga tecken |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Tillhandahåller datatyp för att lagra uppsättningar med nyckel/värde-par |

### <a name="functions-extensions"></a>Funktioner tillägg

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Innehåller flera funktioner toodetermine likheter och avståndet mellan strängar. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Ger funktioner och operatorer för att ändra null utan matriser med heltal. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Innehåller kryptografiska funktioner |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Hanterar partitionerade tabeller genom tid eller ID |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Tillhandahåller funktioner och operatorer för att fastställa hello likartade alfanumeriskt baserat på matchning av trigram |
| [UUID-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generera universellt unika identifierare (UUID: er) |

### <a name="full-text-search-extensions"></a>Tillägg för fulltext-sökning

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | En text search ordlista som tar bort accent (Diakritisk tecken) från lexemes. |

### <a name="index-types-extensions"></a>Index typer tillägg

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [BTree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Innehåller exempel GIN operatorn klasser som implementerar B-trädet som beteendet för vissa datatyper. |
| [BTree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Innehåller GiST index operatorn klasser som implementerar B-trädet. |

### <a name="language-extensions"></a>-Tillägg

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL inläsningsbar procedurmässig språk |

### <a name="miscellaneous-extensions"></a>Diverse tillägg

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Ger möjlighet att undersöka vad som händer i hello delade buffertminne i realtid. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Erbjuder ett sätt tooload relationsdata i hello buffertminne. |
| [PG\_stat\_instruktioner](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Ger möjlighet att spåra körningen statistik för alla SQL-instruktioner som körs av en server. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Extern data wrapper används tooaccess data som lagras i externa PostgreSQL-servrar |

### <a name="postgis"></a>PostGIS

> [!div class="mx-tableFixed"]
| **Tillägg** | **Beskrivning** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologi, postgis\_tiger\_geocoder, postgis\_sfcgal | Spatial- och geografiska objekt för PostgreSQL. |
| adress\_standardizer, adress\_standardizer\_data\_oss | Använda tooparse en adress i innehåll. Använda toosupport geokodning adress normalisering steg. |
| [pgrouting](http://pgrouting.org/) | Utökar hello PostGIS / PostgreSQL geospatiala databas tooprovide geospatiala routningsfunktionen. |

## <a name="next-steps"></a>Nästa steg
Ett tillägg som du vill att toouse ser? Meddela oss. Rösta på befintliga begäranden eller skapa nya feedback och dina önskemål i vår [kunden Feedbackforum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
