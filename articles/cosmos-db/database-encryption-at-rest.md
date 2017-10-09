---
title: 'Databaskryptering vilande: Azure Cosmos DB | Microsoft Docs'
description: "Lär dig hur Azure Cosmos DB tillhandahåller standard kryptering av alla data."
services: cosmos-db
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: d52d55fe45fdd18394166ec7ccd6e46e8f8f434b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Azure DB Cosmos databaskryptering i vila

Kryptering i vila är en fras som ofta refererar toohello kryptering av data på nonvolatile lagringsenheter, t.ex. solid-state-hårddiskar (SSD) och hårddiskenheter (HDD). Cosmos DB lagrar primära databaserna på SSD-enheter. Dess media bilagor och säkerhetskopior lagras i Azure Blob storage som vanligtvis har säkerhetskopierats av hårddiskar. Med hello versionen av kryptering i vila för Cosmos DB krypteras alla databaser, media bilagor och säkerhetskopieringar. Dina data är krypterad under överföringen (via hello nätverk) och i vila (nonvolatile lagring), vilket ger dig kryptering från slutpunkt till slutpunkt.

Eftersom en PaaS-tjänst, Cosmos DB är mycket enkelt toouse. Eftersom alla data som lagras i Cosmos-databasen är krypterat i vila och under transport, har du inte tootake någon åtgärd. Ett annat sätt tooput detta är att kryptering i vila är ”på” som standard. Det finns inga kontroller tooturn den eller inaktivera. Vi använder den här funktionen medan vi fortsätta toomeet vår [tillgänglighet och prestanda SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implement-encryption-at-rest"></a>Implementera kryptering i vila

Kryptering i vila implementeras med hjälp av ett antal säkerhetstekniker, inklusive system för säker lagring av nycklar, krypterade nätverk och kryptografiska API: er. Datorer som kan dekryptera och bearbeta data har toocommunicate med system som hanterar nycklar. hello diagram visar hur lagring av krypterade data och hello hantering av nycklar är åtskilda. 

![Design diagram](./media/database-encryption-at-rest/design-diagram.png)

hello grundläggande flödet av en användarbegäran är följande:
- hello användarkonto databasen görs klar och lagringsnycklar hämtas via en begäran toohello Management Tjänstresursprovider.
- En användare skapar en anslutning tooCosmos DB via HTTPS/secure transport. (hello SDK abstrakt hello information.)
- hello användaren skickar en JSON-dokument toobe lagras över hello tidigare skapade säker anslutning.
- hello JSON-dokument indexeras om hello användaren har inaktiverat indexering.
- Både hello JSON-dokumentet och indexinformationen skrivs toosecure lagring.
- Med jämna mellanrum data läses från hello säker lagring och säkerhetskopierade toohello Azure krypterade Blob Store.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>F: hur mycket kostar Azure Storage om Storage Service-kryptering aktiveras?
S: finns utan extra kostnad.

### <a name="q-who-manages-hello-encryption-keys"></a>F: som hanterar hello krypteringsnycklar?
S: hello nycklar som hanteras av Microsoft.

### <a name="q-how-often-are-encryption-keys-rotated"></a>F: hur ofta roteras krypteringsnycklar?
S: Microsoft har interna riktlinjer för kryptering viktiga rotation, vilket följer Cosmos DB. specifika riktlinjer för hello publiceras inte. Microsoft publicera hello [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), som ses som en del av interna riktlinjer och har användbara Metodtips för utvecklare.

### <a name="q-can-i-use-my-own-encryption-keys"></a>F: kan jag använda min egen krypteringsnycklar?
A: vi arbetat hårda tookeep hello service enkelt toouse cosmos DB är en PaaS-tjänst. Vi har lagt märke till den här frågan är ofta tillfrågas som en proxy-fråga för att uppfylla efterföljandekrav som PCI-DSS. Skapa den här funktionen arbetat, vi med efterlevnad granskare tooensure att kunder som använder Cosmos DB uppfyller deras behov utan hello måste toomanage nycklar sig själva.
Därför kan ger vi för närvarande inte användare hello alternativet tooburden sig själva med nyckelhantering.

### <a name="q-what-regions-have-encryption-turned-on"></a>F: vad regioner har kryptering aktiverat?
S: alla Azure Cosmos DB regioner har kryptering aktiverat för alla användardata.

### <a name="q-does-encryption-affect-hello-performance-latency-and-throughput-slas"></a>F: kryptering påverkar hello prestanda svarstid och genomströmning SLA: er?
S: finns ingen inverkan ändringar toohello prestanda eller serviceavtal nu att kryptering i vila är aktiverad för alla nya och befintliga konton. Du kan läsa mer på hello [SLA för Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) sidan toosee hello senaste garantier.

### <a name="q-does-hello-local-emulator-support-encryption-at-rest"></a>F: hello lokala emulator stöder kryptering i vila?
S: hello-emulatorn är ett fristående verktyg för utveckling och testning och använda inte hello Nyckelhanteringstjänster som hello hanterade Cosmos-DB-tjänsten används. Vår rekommendation är tooenable BitLocker på enheter där du lagrar känslig emulatorn testdata. Hej [emulator stöder ändring hello data standardkatalogen](local-emulator.md) samt med en känd plats.

## <a name="next-steps"></a>Nästa steg

En översikt över Cosmos DB säkerhets- och hello senaste förbättringarna finns [Azure Cosmos DB databassäkerhet](database-security.md).
Mer information om Microsoft-certifikat finns hello [Azure Säkerhetscenter](https://azure.microsoft.com/en-us/support/trust-center/).
