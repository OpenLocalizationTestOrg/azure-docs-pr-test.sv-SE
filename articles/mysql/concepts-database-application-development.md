---
title: "Databasen application utvecklingsöversikt för Azure-databas för MySQL | Microsoft Docs"
description: "Introducerar designöverväganden som utvecklare bör följa när du skriver programkod för att ansluta till Azure-databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Översikt över utveckling program för Azure-databas för MySQL 
Den här artikeln beskrivs överväganden vid utformning av som utvecklare bör följa när du skriver programkod för att ansluta till Azure-databas för MySQL 

> [!TIP]
> En självstudiekurs visar hur du skapar du en server, skapa en serverbaserad brandvägg, Visa serveregenskaper, skapa databas, Anslut och fråga med arbetsstationen och mysql.exe finns [utforma din första Azure MySQL-databas](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Språk och plattform
Det finns kodexempel för olika programmeringsspråk och plattformar. Du hittar länkar till kodexempel på: [anslutning bibliotek som används för att ansluta till Azure-databas för MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Verktyg
Azure MySQL-databas som använder MySQL community version, kompatibel med MySQL vanliga hanteringsverktyg, till exempel arbetsstationen eller MySQL verktyg, till exempel mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), med mera. Du kan också använda Azure-portalen, Azure CLI och REST API: er för att interagera med databastjänsten.

## <a name="resource-limitations"></a>Resursbegränsningar
Azure MySQL-databas hanterar resurser som är tillgängliga för en server med två olika metoder: 
- Resurser-styrning 
- Tillämpning av gränser.

## <a name="security"></a>Säkerhet
Azure MySQL-databas innehåller resurser för att begränsa åtkomst, skyddar data, konfigurera användare och roll och övervakning av aktiviteter i en MySQL-databas.

## <a name="authentication"></a>Autentisering
Azure MySQL-databas har stöd för server-autentisering av användare och inloggningar.

## <a name="resiliency"></a>Återhämtning
När ett tillfälligt fel uppstår vid anslutning till MySQL-databas ska koden gör anropet. Vi rekommenderar försök logik Använd tillbaka av logik, så att den inte överväldigande SQL-databas med flera klienter som du försöker samtidigt.

- Kodexempel: kodexempel som visar försök logik, se exempel för språket i ditt val på: [anslutning bibliotek som används för att ansluta till Azure-databas för MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Hantera anslutningar
Databasanslutningar är en begränsad resurs, så vi rekommenderar sensible användning av anslutningar vid åtkomst till din MySQL-databas för att få bättre prestanda.
- Åtkomst till databasen med hjälp av anslutningspooler eller beständiga anslutningar.
- Åtkomst till databasen med hjälp av anslutning kort livslängd. 
- Använda logik i ditt program vid anslutningen, för att fånga fel på grund av samtidiga anslutningar har nått det högsta tillåtna. Ange en kort fördröjning i logik för omprövning och sedan vänta tills en slumpmässig tid innan ytterligare anslutningsförsök.