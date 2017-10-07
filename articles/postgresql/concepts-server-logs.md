---
title: "aaaServer loggar i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Skapar frågan och felloggar i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 22575f3882ce67fe640952f0a8dbfd8dcb4b16fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Loggas i Azure-databas för PostgreSQL 
Azure-databas för PostgreSQL genererar fråge- och loggar. Tootransaction åtkomstloggar stöds dock inte. Dessa loggar kan använda tooidentify, felsöka och reparera konfigurationsfel och bästa möjliga prestanda. Mer information finns i [felrapportering och loggning](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Åtkomst till server-loggar
Du kan visa och hämta med hello Azure-portalen, Azure PostgreSQL-server-felloggarna [Azure CLI](howto-configure-server-logs-using-cli.md) och Azure REST API: er.

## <a name="log-retention"></a>Kvarhållning av logg
Du kan ange hello kvarhållningsperiod för systemloggarna med den **loggen\_kvarhållning\_period** parameter som är associerade med servern. hello enheten för den här parametern är dagar (måste tooconfirm). hello standardvärdet är tre dagar. hello maxvärdet är 7 dagar. Observera att servern måste ha tillräckligt med allokerat lagringsutrymme toocontain hello behålls loggfiler.
hello loggfiler roteras varje timme eller 100MB storlek, beroende på vilket som inträffar först.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurera loggning för Azure PostgreSQL-server
Du kan aktivera loggning av frågor och felloggning för servern. Felloggarna kan innehålla automatisk vakuum, information om anslutningen och kontrollpunkter.

Du kan aktivera loggning av frågor för PostgreSQL DB-instans genom att ange två serverparametrar: logga\_-instruktionen och logga\_min\_varaktighet\_instruktionen.

Hej **loggen\_instruktionen** parameter styr vilka SQL-instruktioner som loggas. Vi rekommenderar den här parametern för***alla*** toolog alla instruktioner; hello standardvärdet ingen är (måste tooconfirm).

Hej **loggen\_min\_varaktighet\_instruktionen** parameteruppsättningar hello gränsen i millisekunder för en instruktion toobe loggas. Alla SQL-instruktioner som körs längre än hello parameterinställningen loggas. Den här parametern är inaktiverat och ange toominus 1 (-1) som standard (måste tooconfirm). Aktivera den här parametern kan vara användbart i spårar optimerat frågor i dina program.

Hej **loggen\_min\_meddelanden** kan du toocontrol vilka meddelandenivåer skrivs toohello serverloggen. hello standard är en varning. (måste tooconfirm)

Mer information om dessa inställningar finns [felrapportering och loggning](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentation. Särskilt konfigurerar Azure-databas för PostgreSQL serverparametrar, finns i [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md).

## <a name="next-steps"></a>Nästa steg
- tooaccess loggar med Azure CLI kommandoradsgränssnittet finns [konfigurera och access server-loggar med hjälp av Azure CLI](howto-configure-server-logs-using-cli.md)
- Mer information om serverparametrar finns [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md).