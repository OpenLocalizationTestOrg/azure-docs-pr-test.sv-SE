---
title: aaaDatabase security - Azure Cosmos DB | Microsoft Docs
description: "Lär dig hur Azure Cosmos DB ger skydd och data databassäkerhet för dina data."
keywords: "nosql-databas säkerhet, informationssäkerhet, datasäkerhet, databaskryptering, databasskyddet och IPSec-principer säkerhet testning"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: mimig
ms.openlocfilehash: 85ffa62f611bfad00bf3fc5dbe536f91f97f1113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-security"></a>Azure DB Cosmos database-säkerhet

Den här artikeln beskrivs databasen Metodtips om säkerhet och viktiga funktioner som erbjuds av Azure Cosmos DB toohelp du förebygga, upptäcka och åtgärda toodatabase överträdelser.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Vad är nytt i Azure Cosmos DB säkerhet?

Kryptering i vila är nu tillgänglig för dokument och säkerhetskopior som lagras i Azure Cosmos-DB i alla Azure-regioner. Kryptering i vila används automatiskt för nya och befintliga kunder i dessa regioner. Det finns inget behov av tooconfigure någonting; och du får hello samma bra svarstid, dataflöde, tillgänglighet och funktioner som tidigare med hello fördelen att känna till dina data är säkra med kryptering i vila.

## <a name="how-do-i-secure-my-database"></a>Hur skyddar databasen? 

Datasäkerhet är en delad ansvar mellan du hello kund och databas-provider. Hello mängden ansvar du har kan variera beroende på hello databasprovidern du väljer. Om du väljer en lokal lösning måste tooprovide allt från slutpunkt skydd toophysical säkerheten för din maskinvara – finns ingen enkel aktivitet. Om du väljer en PaaS databasen molntjänstleverantör, till exempel Azure Cosmos DB minskar ditt område rör avsevärt. Hej följande bild, lånar från Microsofts [delade ansvarsområden för Cloud Computing](https://aka.ms/sharedresponsibility) faktabladet, visar hur ditt ansvar minskar med en PaaS-provider som Azure Cosmos DB.

![Kund- och providern ansvarsområden](./media/database-security/nosql-database-security-responsibilities.png)

Hej föregående diagram visar utförligt molnet säkerhetskomponenter, men vilka objekt du behöver tooworry om specifikt för din databaslösning? Och hur kan du jämföra lösningar tooeach andra? 

Vi rekommenderar följande checklista för krav på vilka toocompare databassystem hello:

- Nätverkssäkerhet och brandväggsinställningar
- Användarautentisering och detaljerade användarkontroller
- Möjlighet tooreplicate data globalt för regionala fel
- Möjlighet tooperform redundans från en data center tooanother
- Replikering av lokala data inom ett datacenter
- Automatisk säkerhetskopiering
- Återställning av borttagna data från säkerhetskopior
- Skydda och isolera känsliga data
- Övervakning för attacker
- Svara tooattacks
- Möjlighet toogeo avgränsningstecken tooadhere toodata styrning begränsningar
- Fysiska skydd av servrar i skyddade Datacenter

Och även om det kan verka uppenbara, senaste [storskaliga databasen överträdelser](http://thehackernews.com/2017/01/mongodb-database-security.html) Påminn oss av hello enkla men kritiska betydelse hello följande krav:
- Korrigeringsfil servrar som hålls in toodate
- HTTPS som standard/SSL-kryptering
- Administrativa konton med starka lösenord

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Hur skydda databasen i Azure Cosmos DB?

Nu ska vi gå tillbaka till föregående lista hello - hur många av dessa säkerhetskrav Azure Cosmos DB tillhandahåller? Varje enskild komponent.

Nu ska vi prova var och en i detalj.

|Säkerhetskrav|Azure Cosmos DB säkerhet metod|
|---|---|---|
|Nätverkssäkerhet|Med hjälp av en IP-brandvägg är hello första lagret för skydd toosecure din databas. Azure Cosmos-DB stöder drivs IP-baserade åtkomstkontroller för inkommande brandväggsstöd-princip. hello IP-baserade åtkomstkontroller är liknande toohello brandväggsregler som används av traditionella databassystem, men de har expanderats så att Azure DB som Cosmos-databaskonto är endast tillgänglig från godkända uppsättning datorer eller molntjänster. <br><br>Azure Cosmos-DB aktiverar du tooenable en specifik IP-adress (168.61.48.0), ett IP-adressintervall (168.61.48.0/8) och kombinationer av IP-adresser och intervall. <br><br>Alla begäranden från datorer utanför den här listan över tillåtna blockeras av Azure Cosmos DB. Begäranden från godkända datorer och molntjänster måste Slutför hello autentisering processen toobe ges åtkomstkontroll toohello resurser.<br><br>Läs mer i [Azure DB som Cosmos-brandväggsstöd](firewall-support.md).|
|Auktorisering|Azure Cosmos-DB använder hashbaserad meddelandeautentiseringskod (HMAC) för auktorisering. <br><br>Varje begäran hashas med hello hemliga nyckel och hello efterföljande Base64-kodad hash skickas med varje anrop tooAzure Cosmos DB. toovalidate hello begäran hello Azure DB som Cosmos-tjänsten använder hello rätt hemliga nyckel och egenskaper toogenerate ett hash-värde och sedan jämförs hello-värde med en hello i hello begäran. Om två hello-värdena matchar, hello åtgärden har korrekt behörighet och hello begäran behandlas, annars är det ett auktoriseringsfel och hello begäran avvisas.<br><br>Du kan använda antingen en [huvudnyckeln](secure-access-to-data.md#master-keys), eller en [resurs token](secure-access-to-data.md#resource-tokens) så att tillgång till ingående tooa resurs, till exempel ett dokument.<br><br>Läs mer i [tooAzure Cosmos DB resurser för säkerhetsåtkomst](secure-access-to-data.md).|
|Användare och behörigheter|Med hjälp av hello [huvudnyckeln](#master-key) för hello konto, du kan skapa användare och behörigheten resurser per databas. En [resurs token](#resource-token) är associerad med en behörighet i en databas och bestämmer om hello användare har åtkomst (skrivskyddad, skrivskyddad eller ingen åtkomst) tooan programresursen i hello-databasen. Programresurser inkludera samlingar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er. hello resurs token och sedan används vid autentisering tooprovide eller neka åtkomst toohello resurs.<br><br>Läs mer i [tooAzure Cosmos DB resurser för säkerhetsåtkomst](secure-access-to-data.md).|
|Active directory-integrering (RBAC)| Du kan också ge åtkomst toohello konto åtkomstkontroll (IAM) i hello Azure-portalen enligt hello skärmbild som följer den här tabellen. IAM ger rollbaserad åtkomstkontroll och integreras med Active Directory. Du kan använda inbyggda roller eller anpassade roller för enskilda användare och grupper som visas i följande bild hello.|
|Globala replikering|Azure Cosmos-DB erbjuder NYCKELFÄRDIGT distributionslistor, vilket gör att du tooreplicate dina data tooany ett Azures globalt datacenter med hello klickar du på en knapp. Globala replikering kan du skalanpassa globalt och ger åtkomst till låg latens tooyour data runt hello world.<br><br>Globala replikering garanterar hello kontexten för säkerhet, skydd mot regionala fel.<br><br>Läs mer i [distribuera data globalt](distribute-data-globally.md).|
|Regional växling vid fel|Om du har replikerats dina data i flera datacenter, samlar Azure Cosmos DB automatiskt över dina åtgärder ska regionala datacenter i offlineläge. Du kan skapa en prioriterad lista med redundans regioner med hjälp av hello regioner där dina data replikeras. <br><br>Läs mer i [Regional växling vid fel i Azure Cosmos DB](regional-failover.md).|
|Lokal replikering|Även inom ett datacenter, Azure Cosmos DB automatiskt replikerar data för hög tillgänglighet ger du hello valet av [konsekvensnivåer](consistency-levels.md). Detta garanterar en [99,99% drifttid tillgänglighets-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) och levereras med en finansiell säkerhet - något inga andra tjänster i databasen kan ge.|
|Automatisk onlinesäkerhetskopieringar|Azure DB Cosmos-databaser säkerhetskopieras regelbundet och lagras i en georedundant store. <br><br>Läs mer i [automatisk online säkerhetskopiering och återställning med Azure Cosmos DB](online-backup-and-restore.md).|
|Återställa borttagna data|hello automatiserade onlinesäkerhetskopieringar kan vara används toorecover data som du kan ha oavsiktligt borttagna för ~ 30 dagar efter hello händelsen. <br><br>Läs mer i [automatisk online säkerhetskopiering och återställning med Azure Cosmos DB](online-backup-and-restore.md)|
|Skydda och isolera känsliga data|Alla data i hello regioner som anges i [vad är nytt?](#whats-new) nu krypterat i vila.<br><br>Personligt identifierbar information och andra känsliga data kan vara isolerad toospecific samlingar och skrivskyddad eller skrivskyddad åtkomst kan vara begränsad toospecific användare.|
|Övervakare för attacker|Du kan övervaka ditt konto för normalt och onormalt aktiviteten med granskningsloggning och aktivitetsloggar. Du kan visa vilka åtgärder som utfördes på dina resurser, som initierade hello igen när hello åtgärden uppstod hello status på hello igen och mycket mer som visas i hello skärmbilden nedan.|
|Svara tooattacks|När du har kontaktat Azure-supporten tooreport potentiella attacker, inletts en process i steg 5 incidenter. hello målet med hello 5-processen är toorestore normal drift säkerhet och åtgärder så snabbt som möjligt när ett problem upptäcks och en undersökning har startats.<br><br>Läs mer i [Microsoft Azure Security Response i hello molnet](https://aka.ms/securityresponsepaper).|
|Geobegränsning|Azure Cosmos-DB säkerställer datastyrning och kompatibilitet för suveräna regioner (till exempel Tyskland Kina oss Gov).|
|Skyddade lokaler|Data i Azure Cosmos databasen lagras på SSD i Azures skyddade datacenter.<br><br>Läs mer i [globala Microsoft-datacenter](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS-kryptering|Alla klient-till-tjänst Azure Cosmos DB interaktioner är SSL/TLS 1.2 tillämpas. All inom datacentret och mellan datacenter replikering är också SSL/TLS 1.2 tillämpas.|
|Vilande kryptering|Alla data som lagras i Azure Cosmos DB krypterat i vila. Läs mer i [Azure Cosmos DB kryptering i vila](.\database-encryption-at-rest.md)|
|Korrigeringsfil servrar|Azure Cosmos DB eliminerar hello måste toomanage och korrigering servrar, som har utförts för dig, automatiskt som en hanterad databas.|
|Administrativa konton med starka lösenord|Det är svårt toobelieve vi även måste toomention detta krav, men till skillnad från vissa av våra konkurrenter, det är omöjligt toohave ett administratörskonto utan lösenord i Azure Cosmos-databasen.<br><br> Säkerhet via SSL och HMAC hemliga autentisering är inbyggd i som standard.|
|Säkerhet och data protection certifieringar|Azure Cosmos-DB har [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [Europeiska modellen satser (EUMC)](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses), och [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) certifikat. Ytterligare certifieringar pågår.|

hello följande skärmbild visar Active directory-integrering (RBAC) som använder åtkomstkontroll (IAM) i hello Azure-portalen: ![åtkomstkontroll (IAM) i hello Azure portal – visar database-säkerhet](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

hello följande skärmbild visar hur du kan använda granskningsloggning och aktivitet loggar toomonitor ditt konto: ![aktivitet loggar för Azure Cosmos DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Nästa steg

Mer information om huvudnycklar och resursen token finns [att skydda åtkomst tooAzure Cosmos DB data](secure-access-to-data.md).

Mer information om Microsoft-certifikat finns [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/).
