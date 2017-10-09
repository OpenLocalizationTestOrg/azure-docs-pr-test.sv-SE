---
title: "utvecklingsöversikt för aaaDatabase program för Azure-databas för MySQL | Microsoft Docs"
description: "Introducerar designöverväganden som utvecklare bör följa när du skriver programmet kod tooconnect tooAzure databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Översikt över utveckling program för Azure-databas för MySQL 
Den här artikeln beskrivs överväganden vid utformning av som utvecklare bör följa när du skriver programmet kod tooconnect tooAzure databas för MySQL 

> [!TIP]
> En självstudiekurs visar du hur toocreate en server, skapa en serverbaserad brandvägg, Visa serveregenskaper, skapa databas, Anslut och fråga med arbetsstationen och mysql.exe finns [utforma din första Azure MySQL-databas](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Språk och plattform
Det finns kodexempel för olika programmeringsspråk och plattformar. Du hittar länkar toohello kodexempel på: [anslutning bibliotek används tooconnect tooAzure databas för MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Verktyg
Azure-databas för MySQL använder hello MySQL community-versionen, kompatibel med MySQL vanliga hanteringsverktyg, till exempel arbetsstationen eller MySQL verktyg, till exempel mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), med mera. Du kan också använda hello Azure-portalen, Azure CLI och REST API: er toointeract med hello database-tjänsten.

## <a name="resource-limitations"></a>Resursbegränsningar
Azure MySQL-databas hanterar hello resurser tillgängliga tooa server på två olika sätt: 
- Resurser-styrning 
- Tillämpning av gränser.

## <a name="security"></a>Säkerhet
Azure MySQL-databas innehåller resurser för att begränsa åtkomst, skyddar data, konfigurera användare och roll och övervakning av aktiviteter i en MySQL-databas.

## <a name="authentication"></a>Autentisering
Azure MySQL-databas har stöd för server-autentisering av användare och inloggningar.

## <a name="resiliency"></a>Återhämtning
När ett tillfälligt fel uppstår vid anslutning tooMySQL databasen bör koden försöka hello-anrop. Vi rekommenderar hello försök logik användning av en logik, så att den inte överväldigande hello SQL-databas med flera klienter som du försöker samtidigt.

- Kodexempel: kodexempel som visar försök logik, se exempel för hello språk på: [anslutning bibliotek används tooconnect tooAzure databas för MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Hantera anslutningar
Databasanslutningar är en begränsad resurs, så vi rekommenderar sensible användning av anslutningar vid åtkomst till din MySQL-databas tooachieve bättre prestanda.
- Access hello-databas med hjälp av anslutningspooler eller beständiga anslutningar.
- Access hello-databas med hjälp av anslutning kort livslängd. 
- Använd logik i programmet hello gång hello anslutningsförsök, toocatch fel på grund av tooconcurrent anslutningar har uppnåtts hello tillåtna. Försök logik i hello, ange en kort fördröjning och sedan vänta tills en slumpmässig tid innan hello ytterligare anslutningsförsök.
