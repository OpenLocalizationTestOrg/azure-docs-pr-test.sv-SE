---
title: "Använder PostgreSQL-tillägg i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver möjligheten att utöka funktionerna i databasen med tillägg i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: 755d1cf1a921f6be8f28a4a8ae515db08d904fcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a><span data-ttu-id="99684-103">PostgreSQL-tillägg i Azure PostgreSQL-databas</span><span class="sxs-lookup"><span data-stu-id="99684-103">PostgreSQL Extensions in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="99684-104">PostgreSQL ger dig möjlighet att utöka funktionerna i databasen med hjälp av tillägg.</span><span class="sxs-lookup"><span data-stu-id="99684-104">PostgreSQL provides the ability to extend the functionality of your database using extensions.</span></span> <span data-ttu-id="99684-105">Tilläggen tillåter att flera relaterade SQL-objekt som buntas ihop i ett paket och som kan läsas in eller tas bort från databasen med ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="99684-105">Extensions allow for multiple related SQL objects to be bundled together in a single package and can be loaded or removed from your database with a single command.</span></span> <span data-ttu-id="99684-106">Tillägg som är inlästa i databasen fungerar precis som funktioner som ingår i.</span><span class="sxs-lookup"><span data-stu-id="99684-106">Extensions once loaded into the database can function just like features that are built in.</span></span> <span data-ttu-id="99684-107">Mer information om PostgreSQL-tillägg finns [paketering relaterade objekt i ett tillägg](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span><span class="sxs-lookup"><span data-stu-id="99684-107">For more information on PostgreSQL extensions, see [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span></span>

## <a name="how-to-use-postgresql-extensions"></a><span data-ttu-id="99684-108">Hur du använder PostgreSQL-tillägg?</span><span class="sxs-lookup"><span data-stu-id="99684-108">How to use PostgreSQL extensions?</span></span>
<span data-ttu-id="99684-109">PostgreSQL tillägg måste vara installerat för din databas innan du kan använda dem.</span><span class="sxs-lookup"><span data-stu-id="99684-109">PostgreSQL extensions need to be installed for your database before you can use them.</span></span> <span data-ttu-id="99684-110">Om du vill installera ett visst tillägg, körs det [skapa tillägg](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) från psql verktyg för att läsa in paketerade objekt i databasen.</span><span class="sxs-lookup"><span data-stu-id="99684-110">To install a particular extension, run the [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) command from psql tool to load the packaged objects into your database.</span></span>

<span data-ttu-id="99684-111">Azure-databas för PostgreSQL stöder en delmängd av nyckeln tillägg som anges här.</span><span class="sxs-lookup"><span data-stu-id="99684-111">Azure Database for PostgreSQL supports a subset of key extensions as listed here.</span></span> <span data-ttu-id="99684-112">Utöver de som visas, stöds inte andra tillägg.</span><span class="sxs-lookup"><span data-stu-id="99684-112">Beyond the ones listed, other extensions are not supported.</span></span> <span data-ttu-id="99684-113">Du kan skapa dina egna tillägg med Azure-databas för PostgreSQL-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="99684-113">You cannot create your own extension with Azure Database for PostgreSQL service.</span></span>

## <a name="extensions-supported-by-azure-database-for-postgresql"></a><span data-ttu-id="99684-114">Tillägg som stöds av Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="99684-114">Extensions supported by Azure Database for PostgreSQL</span></span>
<span data-ttu-id="99684-115">I tabellerna nedan listas de standard PostgreSQL-tillägg som för närvarande stöds av Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="99684-115">The following tables list the standard PostgreSQL extensions that are currently supported by Azure Database for PostgreSQL.</span></span> <span data-ttu-id="99684-116">Du kan också hämta den här informationen genom att fråga pg\_tillgängliga\_tillägg.</span><span class="sxs-lookup"><span data-stu-id="99684-116">You can also obtain this information by querying pg\_available\_extensions.</span></span> 

### <a name="data-types-extensions"></a><span data-ttu-id="99684-117">Datatyperna tillägg</span><span class="sxs-lookup"><span data-stu-id="99684-117">Data types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-118">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-118">**Extension**</span></span> | <span data-ttu-id="99684-119">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-119">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-120">citext</span><span class="sxs-lookup"><span data-stu-id="99684-120">citext</span></span>](https://www.postgresql.org/docs/9.6/static/citext.html) | <span data-ttu-id="99684-121">Ger en strängtyp skiftlägeskänsliga tecken</span><span class="sxs-lookup"><span data-stu-id="99684-121">Provides a case-insensitive character string type</span></span> |
| [<span data-ttu-id="99684-122">hstore</span><span class="sxs-lookup"><span data-stu-id="99684-122">hstore</span></span>](https://www.postgresql.org/docs/9.6/static/hstore.html) | <span data-ttu-id="99684-123">Tillhandahåller datatyp för att lagra uppsättningar med nyckel/värde-par</span><span class="sxs-lookup"><span data-stu-id="99684-123">Provides data type for storing sets of key/value pairs</span></span> |

### <a name="functions-extensions"></a><span data-ttu-id="99684-124">Funktioner tillägg</span><span class="sxs-lookup"><span data-stu-id="99684-124">Functions extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-125">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-125">**Extension**</span></span> | <span data-ttu-id="99684-126">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-126">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-127">fuzzystrmatch</span><span class="sxs-lookup"><span data-stu-id="99684-127">fuzzystrmatch</span></span>](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | <span data-ttu-id="99684-128">Innehåller flera funktioner för att fastställa likheter och avståndet mellan strängar.</span><span class="sxs-lookup"><span data-stu-id="99684-128">Provides several functions to determine similarities and distance between strings.</span></span> |
| [<span data-ttu-id="99684-129">intarray</span><span class="sxs-lookup"><span data-stu-id="99684-129">intarray</span></span>](https://www.postgresql.org/docs/9.6/static/intarray.html) | <span data-ttu-id="99684-130">Ger funktioner och operatorer för att ändra null utan matriser med heltal.</span><span class="sxs-lookup"><span data-stu-id="99684-130">Provides functions and operators for manipulating null-free arrays of integers.</span></span> |
| [<span data-ttu-id="99684-131">pgcrypto</span><span class="sxs-lookup"><span data-stu-id="99684-131">pgcrypto</span></span>](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | <span data-ttu-id="99684-132">Innehåller kryptografiska funktioner</span><span class="sxs-lookup"><span data-stu-id="99684-132">Provides cryptographic functions</span></span> |
| [<span data-ttu-id="99684-133">PG\_partman</span><span class="sxs-lookup"><span data-stu-id="99684-133">pg\_partman</span></span>](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | <span data-ttu-id="99684-134">Hanterar partitionerade tabeller genom tid eller ID</span><span class="sxs-lookup"><span data-stu-id="99684-134">Manages partitioned tables by time or ID</span></span> |
| [<span data-ttu-id="99684-135">PG\_trgm</span><span class="sxs-lookup"><span data-stu-id="99684-135">pg\_trgm</span></span>](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | <span data-ttu-id="99684-136">Tillhandahåller funktioner och operatorer för att fastställa liknande alfanumeriskt baserat på matchning av trigram</span><span class="sxs-lookup"><span data-stu-id="99684-136">Provides functions and operators for determining the similarity of alphanumeric text based on trigram matching</span></span> |
| [<span data-ttu-id="99684-137">UUID-ossp</span><span class="sxs-lookup"><span data-stu-id="99684-137">uuid-ossp</span></span>](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | <span data-ttu-id="99684-138">Generera universellt unika identifierare (UUID: er)</span><span class="sxs-lookup"><span data-stu-id="99684-138">Generate universally unique identifiers (UUIDs)</span></span> |

### <a name="full-text-search-extensions"></a><span data-ttu-id="99684-139">Tillägg för fulltext-sökning</span><span class="sxs-lookup"><span data-stu-id="99684-139">Full-text Search extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-140">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-140">**Extension**</span></span> | <span data-ttu-id="99684-141">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-141">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-142">unaccent</span><span class="sxs-lookup"><span data-stu-id="99684-142">unaccent</span></span>](https://www.postgresql.org/docs/9.6/static/unaccent.html) | <span data-ttu-id="99684-143">En text search ordlista som tar bort accent (Diakritisk tecken) från lexemes.</span><span class="sxs-lookup"><span data-stu-id="99684-143">A text search dictionary that removes accents (diacritic signs) from lexemes.</span></span> |

### <a name="index-types-extensions"></a><span data-ttu-id="99684-144">Index typer tillägg</span><span class="sxs-lookup"><span data-stu-id="99684-144">Index Types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-145">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-145">**Extension**</span></span> | <span data-ttu-id="99684-146">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-146">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-147">BTree\_gin</span><span class="sxs-lookup"><span data-stu-id="99684-147">btree\_gin</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | <span data-ttu-id="99684-148">Innehåller exempel GIN operatorn klasser som implementerar B-trädet som beteendet för vissa datatyper.</span><span class="sxs-lookup"><span data-stu-id="99684-148">Provides sample GIN operator classes that implement B-tree like behavior for certain data types.</span></span> |
| [<span data-ttu-id="99684-149">BTree\_gist</span><span class="sxs-lookup"><span data-stu-id="99684-149">btree\_gist</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | <span data-ttu-id="99684-150">Innehåller GiST index operatorn klasser som implementerar B-trädet.</span><span class="sxs-lookup"><span data-stu-id="99684-150">Provides GiST index operator classes that implement B-tree.</span></span> |

### <a name="language-extensions"></a><span data-ttu-id="99684-151">-Tillägg</span><span class="sxs-lookup"><span data-stu-id="99684-151">Language extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-152">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-152">**Extension**</span></span> | <span data-ttu-id="99684-153">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-153">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-154">plpgsql</span><span class="sxs-lookup"><span data-stu-id="99684-154">plpgsql</span></span>](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | <span data-ttu-id="99684-155">PL/pgSQL inläsningsbar procedurmässig språk</span><span class="sxs-lookup"><span data-stu-id="99684-155">PL/pgSQL loadable procedural language</span></span> |

### <a name="miscellaneous-extensions"></a><span data-ttu-id="99684-156">Diverse tillägg</span><span class="sxs-lookup"><span data-stu-id="99684-156">Miscellaneous extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-157">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-157">**Extension**</span></span> | <span data-ttu-id="99684-158">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-158">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="99684-159">PG\_buffercache</span><span class="sxs-lookup"><span data-stu-id="99684-159">pg\_buffercache</span></span>](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | <span data-ttu-id="99684-160">Ger möjlighet att undersöka vad som händer i delade buffert cachen i realtid.</span><span class="sxs-lookup"><span data-stu-id="99684-160">Provides a means for examining what's happening in the shared buffer cache in real time.</span></span> |
| [<span data-ttu-id="99684-161">PG\_prewarm</span><span class="sxs-lookup"><span data-stu-id="99684-161">pg\_prewarm</span></span>](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | <span data-ttu-id="99684-162">Ger ett sätt att läsa in relationsdata i bufferten cachen.</span><span class="sxs-lookup"><span data-stu-id="99684-162">Provides a way to load relation data into the buffer cache.</span></span> |
| [<span data-ttu-id="99684-163">PG\_stat\_instruktioner</span><span class="sxs-lookup"><span data-stu-id="99684-163">pg\_stat\_statements</span></span>](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | <span data-ttu-id="99684-164">Ger möjlighet att spåra körningen statistik för alla SQL-instruktioner som körs av en server.</span><span class="sxs-lookup"><span data-stu-id="99684-164">Provides a means for tracking execution statistics of all SQL statements executed by a server.</span></span> |
| [<span data-ttu-id="99684-165">postgres\_fdw</span><span class="sxs-lookup"><span data-stu-id="99684-165">postgres\_fdw</span></span>](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | <span data-ttu-id="99684-166">Extern data wrapper används för att komma åt data som lagras i externa PostgreSQL-servrar</span><span class="sxs-lookup"><span data-stu-id="99684-166">Foreign-data wrapper used to access data stored in external PostgreSQL servers</span></span> |

### <a name="postgis"></a><span data-ttu-id="99684-167">PostGIS</span><span class="sxs-lookup"><span data-stu-id="99684-167">PostGIS</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="99684-168">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="99684-168">**Extension**</span></span> | <span data-ttu-id="99684-169">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="99684-169">**Description**</span></span> |
|---|---|
| <span data-ttu-id="99684-170">[PostGIS](http://www.postgis.net/), postgis\_topologi, postgis\_tiger\_geocoder, postgis\_sfcgal</span><span class="sxs-lookup"><span data-stu-id="99684-170">[PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal</span></span> | <span data-ttu-id="99684-171">Spatial- och geografiska objekt för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="99684-171">Spatial and geographic objects for PostgreSQL.</span></span> |
| <span data-ttu-id="99684-172">adress\_standardizer, adress\_standardizer\_data\_oss</span><span class="sxs-lookup"><span data-stu-id="99684-172">address\_standardizer, address\_standardizer\_data\_us</span></span> | <span data-ttu-id="99684-173">Används för att parsa en adress till innehåll.</span><span class="sxs-lookup"><span data-stu-id="99684-173">Used to parse an address into constituent elements.</span></span> <span data-ttu-id="99684-174">Används för att stödja geokodning adress normalisering steg.</span><span class="sxs-lookup"><span data-stu-id="99684-174">Used to support geocoding address normalization step.</span></span> |
| [<span data-ttu-id="99684-175">pgrouting</span><span class="sxs-lookup"><span data-stu-id="99684-175">pgrouting</span></span>](http://pgrouting.org/) | <span data-ttu-id="99684-176">Utökar PostGIS / PostgreSQL geospatiala database tillhandahåller geospatiala routning funktioner.</span><span class="sxs-lookup"><span data-stu-id="99684-176">Extends the PostGIS / PostgreSQL geospatial database to provide geospatial routing functionality.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="99684-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99684-177">Next steps</span></span>
<span data-ttu-id="99684-178">Ser ett tillägg som du vill använda?</span><span class="sxs-lookup"><span data-stu-id="99684-178">Don't see an extension you'd like to use?</span></span> <span data-ttu-id="99684-179">Meddela oss.</span><span class="sxs-lookup"><span data-stu-id="99684-179">Please let us know.</span></span> <span data-ttu-id="99684-180">Rösta på befintliga begäranden eller skapa nya feedback och dina önskemål i vår [kunden Feedbackforum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span><span class="sxs-lookup"><span data-stu-id="99684-180">Vote for existing requests or create new feedback and wishes in our [Customer feedback forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span></span>
