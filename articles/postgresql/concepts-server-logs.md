---
title: "Loggas i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Skapar frågan och felloggar i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 696af85cd5609171a719a7e77efbfcdeba0aaaaa
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Loggas i Azure-databas för PostgreSQL 
Azure-databas för PostgreSQL genererar fråge- och loggar. Åtkomst till transaktionsloggar stöds dock inte. Frågan och felloggar kan användas för att identifiera, felsöka och reparera konfigurationsfel och något sämre prestanda. Mer information finns i [felrapportering och loggning](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Åtkomst till serverloggar
Du kan visa och hämta med Azure-portalen, Azure PostgreSQL-server-felloggarna [Azure CLI](howto-configure-server-logs-using-cli.md), och Azure REST API: er.

## <a name="log-retention"></a>Kvarhållning av logg
Du kan ställa in kvarhållningsperioden för loggar systemet med hjälp av den **loggen\_kvarhållning\_period** parameter som är associerade med servern. Enheten för den här parametern är dagar. Standardvärdet är 3 dagar. Det maximala värdet är 7 dagar. Servern måste ha tillräckligt med allokerat lagringsutrymme för att innehålla kvarhållna loggfilerna.
Loggfilerna rotera var en timme eller 100 MB storlek, beroende på vilket som inträffar först.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurera loggning för Azure PostgreSQL-server
Du kan aktivera loggning av frågor och felloggning för servern. Felloggarna kan innehålla information om automatisk vakuum, anslutning och kontrollpunkter.

Du kan aktivera loggning av frågor för PostgreSQL DB-instans genom att ange två serverparametrar: `log\_statement` och `log\_min\_duration\_statement`.

Den **loggen\_instruktionen** parameter styr vilka SQL-instruktioner som loggas. Vi rekommenderar den här parametern ***alla*** logga alla instruktioner; standardvärdet är ingen.

Den **loggen\_min\_varaktighet\_instruktionen** parameteruppsättningar gränsen i millisekunder för en instruktion som ska loggas. Alla SQL-instruktioner som körs längre än parameterinställningen för loggas. Den här parametern är inaktiverad och inställd på minus 1 (-1) som standard. Aktivera den här parametern kan vara användbart i spårar optimerat frågor i dina program.

Den **loggen\_min\_meddelanden** kan du styra vilka nivåer-meddelandet skrivs i serverloggen. Standardvärdet är en varning. 

Mer information om dessa inställningar finns [felrapportering och loggning](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentation. Särskilt konfigurerar Azure-databas för PostgreSQL serverparametrar, finns i [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Nästa steg
- Du hittar loggar med kommandoradsgränssnittet i Azure CLI i [konfigurera och access server-loggar med hjälp av Azure CLI](howto-configure-server-logs-using-cli.md).
- Mer information om serverparametrar finns [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md).
