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
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a><span data-ttu-id="17c68-103">PostgreSQL-tillägg i Azure PostgreSQL-databas</span><span class="sxs-lookup"><span data-stu-id="17c68-103">PostgreSQL Extensions in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="17c68-104">PostgreSQL innehåller hello möjlighet tooextend hello funktioner för din databas med hjälp av tillägg.</span><span class="sxs-lookup"><span data-stu-id="17c68-104">PostgreSQL provides hello ability tooextend hello functionality of your database using extensions.</span></span> <span data-ttu-id="17c68-105">Tillägg kan för flera relaterade SQL-objekt-toobe buntas ihop i ett paket och läsas in eller tas bort från databasen med ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="17c68-105">Extensions allow for multiple related SQL objects toobe bundled together in a single package and can be loaded or removed from your database with a single command.</span></span> <span data-ttu-id="17c68-106">Tillägg som är inlästa i hello-databasen fungerar precis som funktioner som ingår i.</span><span class="sxs-lookup"><span data-stu-id="17c68-106">Extensions once loaded into hello database can function just like features that are built in.</span></span> <span data-ttu-id="17c68-107">Mer information om PostgreSQL-tillägg finns [paketering relaterade objekt i ett tillägg](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span><span class="sxs-lookup"><span data-stu-id="17c68-107">For more information on PostgreSQL extensions, see [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span></span>

## <a name="how-toouse-postgresql-extensions"></a><span data-ttu-id="17c68-108">Hur toouse PostgreSQL tillägg?</span><span class="sxs-lookup"><span data-stu-id="17c68-108">How toouse PostgreSQL extensions?</span></span>
<span data-ttu-id="17c68-109">PostgreSQL tillägg måste toobe installerad för din databas innan du kan använda dem.</span><span class="sxs-lookup"><span data-stu-id="17c68-109">PostgreSQL extensions need toobe installed for your database before you can use them.</span></span> <span data-ttu-id="17c68-110">tooinstall ett visst tillägg, körs det [skapa tillägg](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) kommando från psql verktyget tooload hello paketerade objekt i databasen.</span><span class="sxs-lookup"><span data-stu-id="17c68-110">tooinstall a particular extension, run the [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) command from psql tool tooload hello packaged objects into your database.</span></span>

<span data-ttu-id="17c68-111">Azure-databas för PostgreSQL stöder en delmängd av nyckeln tillägg som anges här.</span><span class="sxs-lookup"><span data-stu-id="17c68-111">Azure Database for PostgreSQL supports a subset of key extensions as listed here.</span></span> <span data-ttu-id="17c68-112">Andra tillägg stöds inte för utöver hello som visas.</span><span class="sxs-lookup"><span data-stu-id="17c68-112">Beyond hello ones listed, other extensions are not supported.</span></span> <span data-ttu-id="17c68-113">Du kan skapa dina egna tillägg med Azure-databas för PostgreSQL-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="17c68-113">You cannot create your own extension with Azure Database for PostgreSQL service.</span></span>

## <a name="extensions-supported-by-azure-database-for-postgresql"></a><span data-ttu-id="17c68-114">Tillägg som stöds av Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="17c68-114">Extensions supported by Azure Database for PostgreSQL</span></span>
<span data-ttu-id="17c68-115">hello följande tabeller listan hello PostgreSQL standardtillägg som för närvarande stöds av Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="17c68-115">hello following tables list hello standard PostgreSQL extensions that are currently supported by Azure Database for PostgreSQL.</span></span> <span data-ttu-id="17c68-116">Du kan också hämta den här informationen genom att fråga pg\_tillgängliga\_tillägg.</span><span class="sxs-lookup"><span data-stu-id="17c68-116">You can also obtain this information by querying pg\_available\_extensions.</span></span> 

### <a name="data-types-extensions"></a><span data-ttu-id="17c68-117">Datatyperna tillägg</span><span class="sxs-lookup"><span data-stu-id="17c68-117">Data types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-118">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-118">**Extension**</span></span> | <span data-ttu-id="17c68-119">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-119">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-120">citext</span><span class="sxs-lookup"><span data-stu-id="17c68-120">citext</span></span>](https://www.postgresql.org/docs/9.6/static/citext.html) | <span data-ttu-id="17c68-121">Ger en strängtyp skiftlägeskänsliga tecken</span><span class="sxs-lookup"><span data-stu-id="17c68-121">Provides a case-insensitive character string type</span></span> |
| [<span data-ttu-id="17c68-122">hstore</span><span class="sxs-lookup"><span data-stu-id="17c68-122">hstore</span></span>](https://www.postgresql.org/docs/9.6/static/hstore.html) | <span data-ttu-id="17c68-123">Tillhandahåller datatyp för att lagra uppsättningar med nyckel/värde-par</span><span class="sxs-lookup"><span data-stu-id="17c68-123">Provides data type for storing sets of key/value pairs</span></span> |

### <a name="functions-extensions"></a><span data-ttu-id="17c68-124">Funktioner tillägg</span><span class="sxs-lookup"><span data-stu-id="17c68-124">Functions extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-125">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-125">**Extension**</span></span> | <span data-ttu-id="17c68-126">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-126">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-127">fuzzystrmatch</span><span class="sxs-lookup"><span data-stu-id="17c68-127">fuzzystrmatch</span></span>](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | <span data-ttu-id="17c68-128">Innehåller flera funktioner toodetermine likheter och avståndet mellan strängar.</span><span class="sxs-lookup"><span data-stu-id="17c68-128">Provides several functions toodetermine similarities and distance between strings.</span></span> |
| [<span data-ttu-id="17c68-129">intarray</span><span class="sxs-lookup"><span data-stu-id="17c68-129">intarray</span></span>](https://www.postgresql.org/docs/9.6/static/intarray.html) | <span data-ttu-id="17c68-130">Ger funktioner och operatorer för att ändra null utan matriser med heltal.</span><span class="sxs-lookup"><span data-stu-id="17c68-130">Provides functions and operators for manipulating null-free arrays of integers.</span></span> |
| [<span data-ttu-id="17c68-131">pgcrypto</span><span class="sxs-lookup"><span data-stu-id="17c68-131">pgcrypto</span></span>](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | <span data-ttu-id="17c68-132">Innehåller kryptografiska funktioner</span><span class="sxs-lookup"><span data-stu-id="17c68-132">Provides cryptographic functions</span></span> |
| [<span data-ttu-id="17c68-133">PG\_partman</span><span class="sxs-lookup"><span data-stu-id="17c68-133">pg\_partman</span></span>](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | <span data-ttu-id="17c68-134">Hanterar partitionerade tabeller genom tid eller ID</span><span class="sxs-lookup"><span data-stu-id="17c68-134">Manages partitioned tables by time or ID</span></span> |
| [<span data-ttu-id="17c68-135">PG\_trgm</span><span class="sxs-lookup"><span data-stu-id="17c68-135">pg\_trgm</span></span>](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | <span data-ttu-id="17c68-136">Tillhandahåller funktioner och operatorer för att fastställa hello likartade alfanumeriskt baserat på matchning av trigram</span><span class="sxs-lookup"><span data-stu-id="17c68-136">Provides functions and operators for determining hello similarity of alphanumeric text based on trigram matching</span></span> |
| [<span data-ttu-id="17c68-137">UUID-ossp</span><span class="sxs-lookup"><span data-stu-id="17c68-137">uuid-ossp</span></span>](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | <span data-ttu-id="17c68-138">Generera universellt unika identifierare (UUID: er)</span><span class="sxs-lookup"><span data-stu-id="17c68-138">Generate universally unique identifiers (UUIDs)</span></span> |

### <a name="full-text-search-extensions"></a><span data-ttu-id="17c68-139">Tillägg för fulltext-sökning</span><span class="sxs-lookup"><span data-stu-id="17c68-139">Full-text Search extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-140">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-140">**Extension**</span></span> | <span data-ttu-id="17c68-141">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-141">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-142">unaccent</span><span class="sxs-lookup"><span data-stu-id="17c68-142">unaccent</span></span>](https://www.postgresql.org/docs/9.6/static/unaccent.html) | <span data-ttu-id="17c68-143">En text search ordlista som tar bort accent (Diakritisk tecken) från lexemes.</span><span class="sxs-lookup"><span data-stu-id="17c68-143">A text search dictionary that removes accents (diacritic signs) from lexemes.</span></span> |

### <a name="index-types-extensions"></a><span data-ttu-id="17c68-144">Index typer tillägg</span><span class="sxs-lookup"><span data-stu-id="17c68-144">Index Types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-145">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-145">**Extension**</span></span> | <span data-ttu-id="17c68-146">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-146">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-147">BTree\_gin</span><span class="sxs-lookup"><span data-stu-id="17c68-147">btree\_gin</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | <span data-ttu-id="17c68-148">Innehåller exempel GIN operatorn klasser som implementerar B-trädet som beteendet för vissa datatyper.</span><span class="sxs-lookup"><span data-stu-id="17c68-148">Provides sample GIN operator classes that implement B-tree like behavior for certain data types.</span></span> |
| [<span data-ttu-id="17c68-149">BTree\_gist</span><span class="sxs-lookup"><span data-stu-id="17c68-149">btree\_gist</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | <span data-ttu-id="17c68-150">Innehåller GiST index operatorn klasser som implementerar B-trädet.</span><span class="sxs-lookup"><span data-stu-id="17c68-150">Provides GiST index operator classes that implement B-tree.</span></span> |

### <a name="language-extensions"></a><span data-ttu-id="17c68-151">-Tillägg</span><span class="sxs-lookup"><span data-stu-id="17c68-151">Language extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-152">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-152">**Extension**</span></span> | <span data-ttu-id="17c68-153">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-153">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-154">plpgsql</span><span class="sxs-lookup"><span data-stu-id="17c68-154">plpgsql</span></span>](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | <span data-ttu-id="17c68-155">PL/pgSQL inläsningsbar procedurmässig språk</span><span class="sxs-lookup"><span data-stu-id="17c68-155">PL/pgSQL loadable procedural language</span></span> |

### <a name="miscellaneous-extensions"></a><span data-ttu-id="17c68-156">Diverse tillägg</span><span class="sxs-lookup"><span data-stu-id="17c68-156">Miscellaneous extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-157">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-157">**Extension**</span></span> | <span data-ttu-id="17c68-158">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-158">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="17c68-159">PG\_buffercache</span><span class="sxs-lookup"><span data-stu-id="17c68-159">pg\_buffercache</span></span>](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | <span data-ttu-id="17c68-160">Ger möjlighet att undersöka vad som händer i hello delade buffertminne i realtid.</span><span class="sxs-lookup"><span data-stu-id="17c68-160">Provides a means for examining what's happening in hello shared buffer cache in real time.</span></span> |
| [<span data-ttu-id="17c68-161">PG\_prewarm</span><span class="sxs-lookup"><span data-stu-id="17c68-161">pg\_prewarm</span></span>](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | <span data-ttu-id="17c68-162">Erbjuder ett sätt tooload relationsdata i hello buffertminne.</span><span class="sxs-lookup"><span data-stu-id="17c68-162">Provides a way tooload relation data into hello buffer cache.</span></span> |
| [<span data-ttu-id="17c68-163">PG\_stat\_instruktioner</span><span class="sxs-lookup"><span data-stu-id="17c68-163">pg\_stat\_statements</span></span>](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | <span data-ttu-id="17c68-164">Ger möjlighet att spåra körningen statistik för alla SQL-instruktioner som körs av en server.</span><span class="sxs-lookup"><span data-stu-id="17c68-164">Provides a means for tracking execution statistics of all SQL statements executed by a server.</span></span> |
| [<span data-ttu-id="17c68-165">postgres\_fdw</span><span class="sxs-lookup"><span data-stu-id="17c68-165">postgres\_fdw</span></span>](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | <span data-ttu-id="17c68-166">Extern data wrapper används tooaccess data som lagras i externa PostgreSQL-servrar</span><span class="sxs-lookup"><span data-stu-id="17c68-166">Foreign-data wrapper used tooaccess data stored in external PostgreSQL servers</span></span> |

### <a name="postgis"></a><span data-ttu-id="17c68-167">PostGIS</span><span class="sxs-lookup"><span data-stu-id="17c68-167">PostGIS</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="17c68-168">**Tillägg**</span><span class="sxs-lookup"><span data-stu-id="17c68-168">**Extension**</span></span> | <span data-ttu-id="17c68-169">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="17c68-169">**Description**</span></span> |
|---|---|
| <span data-ttu-id="17c68-170">[PostGIS](http://www.postgis.net/), postgis\_topologi, postgis\_tiger\_geocoder, postgis\_sfcgal</span><span class="sxs-lookup"><span data-stu-id="17c68-170">[PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal</span></span> | <span data-ttu-id="17c68-171">Spatial- och geografiska objekt för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="17c68-171">Spatial and geographic objects for PostgreSQL.</span></span> |
| <span data-ttu-id="17c68-172">adress\_standardizer, adress\_standardizer\_data\_oss</span><span class="sxs-lookup"><span data-stu-id="17c68-172">address\_standardizer, address\_standardizer\_data\_us</span></span> | <span data-ttu-id="17c68-173">Använda tooparse en adress i innehåll.</span><span class="sxs-lookup"><span data-stu-id="17c68-173">Used tooparse an address into constituent elements.</span></span> <span data-ttu-id="17c68-174">Använda toosupport geokodning adress normalisering steg.</span><span class="sxs-lookup"><span data-stu-id="17c68-174">Used toosupport geocoding address normalization step.</span></span> |
| [<span data-ttu-id="17c68-175">pgrouting</span><span class="sxs-lookup"><span data-stu-id="17c68-175">pgrouting</span></span>](http://pgrouting.org/) | <span data-ttu-id="17c68-176">Utökar hello PostGIS / PostgreSQL geospatiala databas tooprovide geospatiala routningsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="17c68-176">Extends hello PostGIS / PostgreSQL geospatial database tooprovide geospatial routing functionality.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="17c68-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17c68-177">Next steps</span></span>
<span data-ttu-id="17c68-178">Ett tillägg som du vill att toouse ser?</span><span class="sxs-lookup"><span data-stu-id="17c68-178">Don't see an extension you'd like toouse?</span></span> <span data-ttu-id="17c68-179">Meddela oss.</span><span class="sxs-lookup"><span data-stu-id="17c68-179">Please let us know.</span></span> <span data-ttu-id="17c68-180">Rösta på befintliga begäranden eller skapa nya feedback och dina önskemål i vår [kunden Feedbackforum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span><span class="sxs-lookup"><span data-stu-id="17c68-180">Vote for existing requests or create new feedback and wishes in our [Customer feedback forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span></span>
