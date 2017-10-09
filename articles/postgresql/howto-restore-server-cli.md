---
title: "Hur tooback upp och återställa en server i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Lär dig hur tooback upp och återställning av en server i Azure-databas för PostgreSQL med hjälp av hello Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a>Hur tooback upp och återställning av en server i Azure-databas för PostgreSQL med hjälp av hello Azure CLI

Använd Azure-databas för PostgreSQL toorestore en server-databasen tooan tidigare datum som sträcker sig från 7 too35 dagar.

## <a name="prerequisites"></a>Krav
toocomplete detta hur tooguide behöver du:
- En [Azure-databas för PostgreSQL-server och databas](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Om du installerar och använder hello Azure CLI lokalt, kräver den här hur tooguide att du använder Azure CLI version 2.0 eller senare. Ange tooconfirm hello version Kommandotolken hello Azure CLI `az --version`. tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Säkerhetskopiera sker automatiskt
När du använder Azure-databas för PostgreSQL har hello database-tjänsten gör en säkerhetskopia av hello service automatiskt var femte minut. 

För grundläggande nivån är hello säkerhetskopior tillgängliga för 7 dagar. För standardnivån är hello säkerhetskopior tillgängliga för 35 dagar. Mer information finns i [Azure-databas för PostgreSQL prisnivåer](concepts-service-tiers.md).

Med funktionen om automatisk säkerhetskopiering kan du återställa hello-servern och dess databaser tooan tidigare datum eller tidpunkt.

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a>Återställa en databas tooa tidigare punkt i tiden med hjälp av hello Azure CLI
Använd Azure-databas för PostgreSQL toorestore hello server tooa tidigare tidpunkt. hello återställa data är kopierade tooa ny server och hello befintlig server är kvar. Om en tabell av misstag utelämnas kl. tolv idag så, kan du återställa toohello tid innan på dagen. Sedan kan du hämta hello saknas tabellen och data från hello återställts kopia av hello-server. 

Använd hello Azure CLI-toorestore hello servern [az postgres server återställning](/cli/azure/postgres/server#restore) kommando.

### <a name="run-hello-restore-command"></a>Kör hello restore-kommandot

toorestore hello server Kommandotolken hello Azure CLI, ange hello följande kommando:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hej `az postgres server restore` kommandot kräver hello följande parametrar:
| Inställning | Föreslaget värde | Beskrivning  |
| --- | --- | --- |
| resursgruppen. |  myResourceGroup |  Resursgruppens namn där det finns hello källservern.  |
| namn | mypgserver återställs | hello namnet på hello nya server som skapas av hello restore-kommandot. |
| Återställ punkt i tiden | 2017-04-13T13:59:00Z | Välj en punkt i tiden toorestore till. Datumet och tiden måste vara inom hello källserverns säkerhetskopiera Bevarandeperiod. Använd hello ISO8601 format för datum och tid. Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`. Du kan också använda hello UTC Zulu format, till exempel `2017-04-13T13:59:00Z`. |
| käll-servern | mypgserver 20170401 | hello namnet eller ID för hello källa server toorestore från. |

När du återställer en server tooan tidigare tidpunkt, skapas en ny server. hello ursprungliga servern och dess databaser från hello har angetts tidpunkten är kopierade toohello ny server.

hello plats och prissättning nivåvärden hello för hello återställa servern vara samma som hello ursprungliga servern. 

Hej `az postgres server restore` kommandot är synkront. När hello server har återställts, kan du använda det igen toorepeat hello process för en annan plats i tid. 

Återställa slutförs efter hello, leta upp hello ny server och kontrollera att hello data återställs som förväntat.

## <a name="next-steps"></a>Nästa steg
[Anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)
