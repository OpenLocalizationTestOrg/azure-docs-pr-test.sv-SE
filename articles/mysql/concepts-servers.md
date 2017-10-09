---
title: "aaaServer begrepp i Azure-databas för MySQL | Microsoft Docs"
description: "Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 4d994cbdbde93ac5af3f4a2a7375b5851bebb1cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Server begrepp i Azure för MySQL-databas
Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Vad är en Azure-databas för MySQL-servern?

En Azure-databas för MySQL-server är en central administrativ plats för flera databaser. Det är hello samma MySQL server konstruktion som du kanske känner till lokala hälsningsmeddelande. Mer specifikt hello Azure-databas för MySQL-tjänst som hanteras, ger garanti vad gäller prestanda, visar åtkomst och funktioner på servernivå.

En Azure-databas för MySQL-server:

- Skapas inom en Azure-prenumeration.
- Är hello överordnade resurs för databaser.
- Ger ett namnområde för databaser.
- Är en behållare med starka livstid semantik - ta bort en server och tar bort hello finns databaser.
- Collocates resurser i en region.
- Ger en Anslutningens slutpunkt för server och åtkomst till databasen.
- Hello scope för av hanteringsprinciper som gäller tooits databaser: inloggning, brandvägg, användare, roller, konfigurationer och så vidare.
- Är tillgänglig i flera versioner. Mer information finns i [stöd för Azure-databas för MySQL-databasversioner](./concepts-supported-versions.md).

Du kan skapa en eller flera databaser på en Azure Database för MySQL-server. Du kan välja toocreate en enskild databas per server tooutilize alla hello resurser eller skapa flera databaser tooshare hello resurser. hello priser är strukturerade per server, baserat på hello konfiguration av prisnivån compute enheter, lagringsutrymme (GB). Mer information finns i [prisnivåer](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-mysql-server"></a>Hur jag för att ansluta och autentisera tooan Azure-databas för MySQL-servern?

hello att följande element säkerställa säker åtkomst tooyour databas.

|||
| :-- | :-- |
| **Autentisering och auktorisering** | Azure-databas för MySQL-server stöder interna MySQL-autentisering. Du kan ansluta och autentisera tooserver med hello server admin-inloggning. |
| **Protokoll** | hello tjänsten stöder en message-baserat protokoll som används av MySQL. |
| **TCP/IP** | hello-protokollet stöds via TCP/IP och över sockets för Unix-domän. |
| **Brandvägg** | toohelp skydda dina data, en brandväggsregel förhindrar alla åtkomst tooyour database-server eller tooits databaser, förrän du anger vilka datorer som har behörighet. Se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md). |
| **SSL** | hello-tjänsten har stöd för att framtvinga SSL-anslutningar mellan dina program och databasservern.  Se [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Hur hanterar ett server?
Du kan hantera Azure-databas för MySQL-servrar med hjälp av hello Azure-portalen eller hello Azure CLI.

## <a name="next-steps"></a>Nästa steg
- En översikt över hello-tjänsten finns [Azure Database MySQL-översikt](./overview.md)
- Information om specifik resurs kvoter och begränsningar baserat på din **tjänstnivån**, se [tjänstnivåer](./concepts-service-tiers.md)
- Information om anslutande toohello tjänsten finns [anslutningsbibliotek för Azure-databas för MySQL](./concepts-connection-libraries.md).
