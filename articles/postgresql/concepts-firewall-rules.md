---
title: "Azure-databas för PostgreSQL serverbrandväggsreglerna | Microsoft Docs"
description: "Beskriver brandväggsregler för din Azure-databas för PostgreSQL-servern."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1d46a4434c483c3612a9a7b4cdef718d6dc3e765
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Azure-databas för PostgreSQL serverbrandväggsreglerna
Brandväggar förhindrar alla åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet. hello brandväggen beviljar åtkomst toohello server baserat på hello kommer IP-adressen för varje begäran.
tooconfigure brandväggen, skapa brandväggsregler som anger godkända IP-adressintervall. Du kan skapa brandväggsregler på servernivå hello.

**Regler i brandväggen:** reglerna aktivera klienter tooaccess Azure hela databasen för PostgreSQL-Server, det vill säga hello alla hello databaser inom samma logiska server. Brandväggsregler på servernivå kan konfigureras med hjälp av hello Azure-portalen eller med Azure CLI-kommandona. toocreate servernivå brandväggsregler, måste du vara hello prenumeration ägare eller deltagare prenumeration.

## <a name="firewall-overview"></a>Översikt över brandväggar
Alla databas åtkomst tooyour Azure-databas för PostgreSQL-servern har blockerats av hello-brandväggen som standard. toobegin med servern från en annan dator, behöver du toospecify en eller fler servernivå regler tooenable åtkomst tooyour brandväggsserver. Använd hello brandväggen regler toospecify vilka IP-adressintervall från hello Internet tooallow. Åtkomst toohello Azure webbplats själva påverkas inte av hello brandväggsregler.
Anslutningsförsök från hello Internet och Azure måste först passera genom brandväggen hello innan de kan komma åt PostgreSQL-databasen som visas i följande diagram hello:

![Exempel flödet av hur hello brandvägg fungerar](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a>Ansluta från hello Internet
Brandväggsregler på servernivå tillämpa tooall databaser på hello Azure-databas för PostgreSQL-servern. Om hello IP-adressen för hello-begäran är inom ett hello-intervall som anges i hello servernivå brandväggsregler, beviljas hello-anslutning.
Om hello IP-adressen för begäran om hello inte ligger inom hello-intervall som anges i någon av hello databasnivå eller servernivå brandväggsregler, hello anslutningsbegäran misslyckas.
Om ditt program ansluter med JDBC-drivrutinen för PostgreSQL, kan du stöta felet försöker tooconnect när hello brandväggen blockerar hello-anslutning.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: allvarligt: inga pg\_hba.conf post för värd ”123.45.67.890”, ”adminuser” databas ”postgresql”, SSL

## <a name="programmatically-managing-firewall-rules"></a>Hantera brandväggsregler via programmering
Dessutom hanterade toohello Azure-portalen brandväggen reglerna kan vara genom programmering med Azure CLI.
Se även [skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-hello-database-firewall"></a>Felsöka hello database-brandvägg
Överväg följande punkter när åtkomst toohello Microsoft Azure-databas för PostgreSQL Server-tjänsten inte fungerar som förväntat hello:

* **Lista över tillåtna ändringar toohello inte har börjat gälla ännu:** kan det vara så mycket som en fem-minuters fördröjning för ändras toohello Azure-databas för PostgreSQL Server brandväggen configuration tootake effekt.

* **hello inloggningen har inte behörighet eller ett felaktigt lösenord användes:** om en inloggning inte har behörighet på hello Azure-databas för PostgreSQL-server eller hello-lösenord som används är felaktig, hello anslutning toohello Azure-databas för PostgreSQL-server är nekad. Skapa en brandväggsinställningen endast ger klienterna en möjlighet tooattempt ansluter tooyour server; varje klient måste ange autentiseringsuppgifter för hello säkerhet som behövs.

Till exempel kan använder en JDBC-klient, hello följande fel visas.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: allvarligt: autentisering misslyckades för användaren ”användarnamn”

* **Dynamisk IP-adress:** om du har en Internetanslutning med dynamisk IP-adressering och du har problem med att få hello-brandväggen kan du försöka något av följande lösningar hello:

* Be din Internet Service Provider (ISP) för hello IP-adressintervall som tilldelats tooyour klientdatorer att åtkomst hello Azure-databas för PostgreSQL-Server och lägger sedan till hello IP-adressintervall som en brandväggsregel.

* Hämta statiska IP-adresser i stället för dina klientdatorer och lägger sedan till hello IP-adresser som brandväggsregler.

## <a name="next-steps"></a>Nästa steg
Artiklar om hur du skapar brandväggsregler på servernivå och databasnivå finns:
* [Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen](howto-manage-firewall-using-portal.md)
* [Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI](howto-manage-firewall-using-cli.md)