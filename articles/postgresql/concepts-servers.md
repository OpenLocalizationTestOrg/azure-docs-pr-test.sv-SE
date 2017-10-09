---
title: "aaaServer begrepp i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för PostgreSQL-servrar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 9cc7816992f2ddedd76fdf906075a723b97720a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-servers"></a>Azure-databas för PostgreSQL-servrar
Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för PostgreSQL-servrar.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Vad är en Azure-databas för PostgreSQL-servern?
En Azure-databas för PostgreSQL-server är en central administrativ plats för flera databaser. Det är hello samma PostgreSQL server konstruktion som du kanske känner till lokala hälsningsmeddelande. Mer specifikt hello PostgreSQL service hanteras, ger garanti vad gäller prestanda, visar åtkomst och funktioner på servernivå.

En Azure-databas för PostgreSQL-server:

- Skapas inom en Azure-prenumeration.
- Är hello överordnade resurs för databaser.
- Ger ett namnområde för databaser.
- Är en behållare med starka livstid semantik - ta bort en server och tar bort hello finns databaser.
- Collocates resurser i en region.
- Ger en Anslutningens slutpunkt för server och databas åtkomst (. postgresql.database.azure.com).
- Hello scope för av hanteringsprinciper som gäller tooits databaser: inloggning, brandvägg, användare, roller, konfigurationer och så vidare.
- Är tillgänglig i flera versioner. Mer information finns i [stöds PostgreSQL databasversioner](concepts-supported-versions.md).
- Kan utökas av användare. Mer information finns i [PostgreSQL tillägg](concepts-extensions.md).

Du kan skapa en eller flera databaser i en Azure-databas för PostgreSQL-servern. Du kan välja toocreate en enskild databas per server tooutilize alla hello resurser eller skapa flera databaser tooshare hello resurser. hello priser är strukturerade per server, baserat på hello konfiguration av prisnivån compute enheter, lagringsutrymme (GB). Mer information finns i [prisnivåer](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-postgresql-server"></a>Hur jag för att ansluta och autentisera tooan Azure-databas för PostgreSQL-servern?
hello att följande element säkerställa säker åtkomst tooyour databas.

|||
| :-- | :-- |
| **Autentisering och auktorisering** | Azure-databas för PostgreSQL server stöder interna PostgreSQL-autentisering. Du kan ansluta och autentisera tooserver med hello server admin-inloggning. |
| **Protokoll** | hello tjänsten stöder en message-baserat protokoll som används av PostgreSQL. |
| **TCP/IP** | hello-protokollet stöds via TCP/IP och över sockets för Unix-domän. |
| **Brandvägg** | toohelp skydda dina data, en brandväggsregel förhindrar alla åtkomst tooyour database-server eller tooits databaser, förrän du anger vilka datorer som har behörighet. Se [Azure-databas för PostgreSQL serverbrandväggsreglerna](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Hur hanterar ett server?
Du kan hantera Azure-databas för PostgreSQL-servrar med hjälp av hello Azure-portalen eller hello [Azure CLI](/cli/azure/postgres).

## <a name="next-steps"></a>Nästa steg
- En översikt över hello-tjänsten finns [Azure Database PostgreSQL-översikt](overview.md)
- Information om specifik resurs kvoter och begränsningar baserat på din **tjänstnivån**, se [tjänstnivåer](concepts-service-tiers.md)
- Mer information om anslutande toohello tjänsten finns [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md).
