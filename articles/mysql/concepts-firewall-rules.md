---
title: "aaaAzure databas för MySQL serverbrandväggsreglerna | Microsoft Docs"
description: "Beskriver brandväggsregler för din Azure-databas för MySQL-servern."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1f85310385da947b6c492aa6aa21c1b885c2a97d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Azure-databas för MySQL serverbrandväggsreglerna
Brandväggar förhindrar alla åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet. hello brandväggen beviljar åtkomst toohello server baserat på hello kommer IP-adressen för varje begäran.

tooconfigure brandväggen, skapa brandväggsregler som anger godkända IP-adressintervall. Du kan skapa brandväggsregler på servernivå hello.

**Regler i brandväggen:** reglerna aktivera klienter tooaccess Azure hela databasen för MySQL-server, det vill säga hello alla hello databaser inom samma logiska server. Brandväggsregler på servernivå kan konfigureras med hjälp av hello Azure-portalen eller med Azure CLI-kommandona. toocreate servernivå brandväggsregler, måste du vara hello prenumeration ägare eller deltagare prenumeration.

## <a name="firewall-overview"></a>Översikt över brandväggar
Alla databas åtkomst tooyour Azure-databas för MySQL-servern har blockerats av hello-brandväggen som standard. toobegin med servern från en annan dator, behöver du toospecify en eller fler servernivå regler tooenable åtkomst tooyour brandväggsserver. Använd hello brandväggen regler toospecify vilka IP-adressintervall från hello Internet tooallow. Åtkomst toohello Azure webbplats själva påverkas inte av hello brandväggsregler.

Anslutningsförsök från hello Internet och Azure måste först passera genom brandväggen hello innan de kan komma åt Azure-databasen för MySQL-databas som visas i följande diagram hello:

![Exempel flödet av hur hello brandvägg fungerar](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a>Ansluta från hello Internet
Brandväggsregler på servernivå tillämpa tooall databaser på hello Azure-databas för MySQL-servern.

Om hello IP-adressen för hello-begäran är inom ett hello-intervall som anges i hello servernivå brandväggsregler, beviljas hello-anslutning.

Om hello IP-adressen för begäran om hello inte ligger inom hello-intervall som anges i någon av hello databasnivå eller servernivå brandväggsregler, hello anslutningsbegäran misslyckas.

## <a name="programmatically-managing-firewall-rules"></a>Hantera brandväggsregler via programmering
Dessutom hanterade toohello Azure-portalen brandväggen reglerna kan vara genom programmering med Azure CLI. Se även [skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-hello-database-firewall"></a>Felsöka hello database-brandvägg
Överväg följande punkter när åtkomst toohello Microsoft Azure-databas för MySQL server-tjänsten inte fungerar som förväntat hello:

* **Lista över tillåtna ändringar toohello inte har börjat gälla ännu:** kan det vara så mycket som en fem-minuters fördröjning för ändras toohello Azure-databas för MySQL Server brandväggen configuration tootake effekt.

* **hello inloggningen har inte behörighet eller ett felaktigt lösenord användes:** om en inloggning inte har behörighet på hello Azure-databas för MySQL-server eller hello-lösenord som används är felaktig, hello anslutning toohello Azure-databas för MySQL server nekas. Skapa en brandväggsinställningen endast ger klienterna en möjlighet tooattempt ansluter tooyour server; varje klient måste ange autentiseringsuppgifter för hello säkerhet som behövs.

* **Dynamisk IP-adress:** om du har en Internetanslutning med dynamisk IP-adressering och du har problem med att få hello-brandväggen kan du försöka något av följande lösningar hello:

* Be din Internet Service Provider (ISP) för hello IP-adressintervall som tilldelats tooyour klientdatorer att åtkomst hello Azure-databas för MySQL-servern och lägger sedan till hello IP-adressintervall som en brandväggsregel.

* Hämta statiska IP-adresser i stället för dina klientdatorer och lägger sedan till hello IP-adresser som brandväggsregler.

## <a name="next-steps"></a>Nästa steg

[Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen](./howto-manage-firewall-using-portal.md)
[skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)
