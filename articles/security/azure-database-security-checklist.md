---
title: "Checklista för aaaAzure-säkerhet | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning checklista för Azure database-säkerhet."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: 5e3a69591df3c8508a8707a2d47068342863ce93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-checklist"></a>Checklista för säkerhet i Azure-databas

toohelp förbättra säkerheten, Azure-databasen innehåller ett antal inbyggda säkerhetsåtgärder att du kan använda toolimit och åtkomstkontroll.

Exempel på dessa är:

-   En brandvägg som gör att du toocreate [regler i brandväggen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) begränsa anslutning med IP-adress
-   Servernivå brandväggen tillgänglig från hello Azure-portalen
-   Databasnivå brandväggsregler tillgänglig från SSMS
-   Skyddad anslutning tooyour databasen med hjälp av säkra anslutningssträngar
-   Använd åtkomsthantering
-   Datakryptering
-   SQL Database auditing
-   Hotidentifiering för SQL-databas

## <a name="introduction"></a>Introduktion
Molntjänster kräver nya säkerhet paradigm som är obekanta toomany programanvändare, databasadministratörer och programmerare. Vissa organisationer kan därför tveka tooimplement en molninfrastruktur för datahantering på grund av tooperceived säkerhetsrisker. Mycket av det här problem kan vara undvikas genom en bättre förståelse för hello säkerhetsfunktioner som är inbyggda i Microsoft Azure och Microsoft Azure SQL Database.

## <a name="checklist"></a>Checklista
Vi rekommenderar att du läser hello [säkerhetsmetoder för Azure-databas](https://docs.microsoft.com/en-us/azure/security/azure-database-security-best-practices) artikel tidigare tooreviewing checklistan. Du kommer att kunna tooget hello mesta av den här checklistan när du förstår hello bästa praxis. Du kan sedan använda den här checklistan toomake till att du har adresserat hello viktiga problem i Azure database-säkerhet.


|Checklista för kategori| Beskrivning|
| ------------ | -------- |
|**Skydda Data**||
| <br> Kryptering i rörelse/överföring| <ul><li>[Transport Layer Security](https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol), för kryptering av data när data flyttas toohello nätverk.</li><li>Databasen kräver säker kommunikation från klienter baserat på hello [TDS (Tabular Data Stream)](https://msdn.microsoft.com/en-in/library/dd357628.aspx) protocol över TLS (Transport Layer Security).</li></ul> |
|<br>Vilande kryptering| <ul><li>[Transparent datakryptering](http://go.microsoft.com/fwlink/?LinkId=526242)när inaktiva data lagras fysiskt i någon digital form.</li></ul>|
|**Kontrollera åtkomst**||  
|<br> Åtkomst till databasen | <ul><li>[Autentisering](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) (Azure Active Directory-autentisering) AD för autentisering använder identiteter som hanteras av Azure Active Directory.</li><li>[Auktorisering](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) bevilja användare hello lägsta behörighet.</li></ul> |
|<br>Programåtkomst| <ul><li>[Rad level Security](https://msdn.microsoft.com/library/dn765131) (med hjälp av säkerhetsprinciper, på hello samtidigt begränsa radnivå åtkomst baserat på användarens identitet, roll eller körningen kontexten).</li><li>[Dynamisk Datamaskering](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) (med hjälp av behörighet & principen begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare)</li></ul>|
|**Proaktiv övervakning**||  
| <br>Spåra & identifiering| <ul><li>[Granskning](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing) spårar databashändelser och skriver dem tooan granskningsloggen / aktivitet log din [Azure Storage-konto](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account).</li><li>Spåra Azure Database hälsotillstånd med [aktivitetsloggar för Azure-Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Hotidentifiering](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection) identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen. </li></ul> |
|<br>Azure Security Center| <ul><li>[Data övervakning](https://docs.microsoft.com/en-us/azure/security-center/security-center-enable-auditing-on-sql-databases) använda Azure Security Center som en centraliserad säkerhet övervakningslösning för SQL och andra Azure-tjänster.</li></ul>|     

## <a name="conclusion"></a>Slutsats
Azure-databas är en robust databasplattform, med en fullständig uppsättning funktioner som uppfyller kraven för många organisationens och regelmässig efterlevnad. Du kan enkelt skydda data genom att kontrollera hello fysisk åtkomst tooyour data och använda en mängd olika alternativ för datasäkerhet på hello fil-, kolumn- eller radnivå med Transparent datakryptering, cellnivå kryptering eller säkerhet på radnivå. Alltid gör krypterat också att åtgärder mot krypterade data, förenkla hello processen för programuppdateringar. I sin tur tooauditing åtkomstloggar för SQL Database-aktiviteten finns hello information du behöver, så att du tooknow hur och när data används.

## <a name="next-steps"></a>Nästa steg
Du kan förbättra hello skyddet av databasen mot obehöriga användare eller obehörig åtkomst med bara några få enkla steg. I den här kursen lär du dig:

- Ställ in [regler i brandväggen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) för servern och eller-databasen.
- Skydda dina data med [kryptering](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/sql-server-encryption).
- Aktivera [SQL Database auditing](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).

