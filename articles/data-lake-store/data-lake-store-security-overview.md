---
title: "Översikt över säkerheten i Data Lake Store | Microsoft Docs"
description: "Förstå hur Azure Data Lake Store är en säkrare stordataarkiv"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 52e176711f512e8a3788309a58011c8484821a1e
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/29/2017
---
# <a name="security-in-azure-data-lake-store"></a>Säkerhet i Azure Data Lake Store
Många företag att utnyttja analyser av stordata för affärsinsikter hjälper dem att fatta smarta beslut. En organisation kan ha en komplex och reglerade miljö med ett ökande antal olika användare. Det är viktigt för företaget att se till att affärskritiska data lagras säkrare, med rätt nivå av åtkomst till enskilda användare. Azure Data Lake Store är utformat för att uppfylla dessa säkerhetskrav. I den här artikeln lär dig mer om säkerhetsfunktioner för Data Lake Store, inklusive:

* Autentisering
* Auktorisering
* Isolering av nätverk
* Dataskydd
* Granskning

## <a name="authentication-and-identity-management"></a>Hantering av autentisering och identitet
Autentisering är den process som användarens identitet har verifierats när användaren interagerar med Data Lake Store eller någon tjänst som ansluter till Data Lake Store. För Identitetshantering och autentisering Data Lake Store använder [Azure Active Directory](../active-directory/active-directory-whatis.md), en omfattande identitets- och molnet hanteringslösning som förenklar hanteringen av användare och grupper.

Varje Azure-prenumeration kan associeras med en instans av Azure Active Directory. Endast användare och tjänsteidentiteter som definieras i din Azure Active Directory-tjänst som har åtkomst till ditt Data Lake Store-konto med hjälp av Azure portal, kommandoradsverktyg, eller via klientprogram skapar organisationen med hjälp av Azure Data Lake Store SDK. Viktiga fördelar med att använda Azure Active Directory som en centraliserad åtkomstkontroll är:

* Förenklad livscykel Identitetshantering. Identiteten för en användare eller en tjänst (en huvudnamn tjänstidentitet) kan skapas snabbt och snabbt återkallas genom att ta bort eller inaktiverar kontot i katalogen.
* Multifaktorautentisering. [Multifaktorautentisering](../multi-factor-authentication/multi-factor-authentication.md) ger ett ytterligare lager av säkerhet för användarinloggningar och transaktioner.
* Autentisering från en klient via en öppen standard protokoll, till exempel OAuth eller OpenID.
* Federation med enterprise katalogtjänster och identitet molntjänstleverantörer.

## <a name="authorization-and-access-control"></a>Auktorisering och åtkomstkontroll
När en användare autentiseras av Azure Active Directory så att användaren kan komma åt Azure Data Lake Store, auktorisering kontroller behörighet att komma åt Data Lake Store. Data Lake Store avgränsar auktorisering för konto-relaterade och data som är relaterade aktiviteter på följande sätt:

* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) (RBAC) tillhandahålls av Azure för kontohantering
* POSIX ACL för åtkomst till data i arkivet

### <a name="rbac-for-account-management"></a>RBAC för kontohantering
Fyra grundläggande roller har definierats för Data Lake Store som standard. Rollerna tillåter olika åtgärder i ett Data Lake Store-konto via Azure-portalen, PowerShell-cmdletar och REST API: er. Rollerna ägare och deltagare kan utföra en mängd funktioner för administration på kontot. Du kan tilldela rollen Läsare för användare som bara kan interagerar med data.

![RBAC-roller](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-roller")

Observera att även om rollerna har tilldelats för hantering av vissa roller påverkar åtkomst till data. Du måste använda ACL: er för att styra åtkomsten till åtgärder som en användare kan utföra i filsystemet. I följande tabell visas en sammanfattning av rights management och data till åtkomstbehörigheter för standardrollerna.

| Roller | Rights Management | Åtkomstbehörighet för data | Förklaring |
| --- | --- | --- | --- |
| Ingen roll |Ingen |Lyder ACL |Användaren kan inte använda Azure-portalen eller Azure PowerShell-cmdlets för att bläddra i Data Lake Store. Användaren kan använda kommandoradsverktyget verktyg. |
| Ägare |Alla |Alla |Rollen som ägare är en superanvändare. Den här rollen kan hantera allt och har fullständig åtkomst till data. |
| Läsare |Skrivskyddad |Lyder ACL |Rollen Läsare kan visa allt om kontohantering, t.ex vilka användare som är tilldelad till vilken roll. Rollen Läsare göra inte några ändringar. |
| Deltagare |Alla utom Lägg till och ta bort roller |Lyder ACL |Deltagarrollen kan hantera vissa aspekter av ett konto, till exempel distributioner och skapa och hantera aviseringar. Deltagarrollen kan inte lägga till eller ta bort roller. |
| Administratör för användaråtkomst |Lägga till och ta bort roller |Lyder ACL |Rollen Administratör för användaråtkomst kan hantera användarnas åtkomst till konton. |

Instruktioner finns i [tilldela användare eller säkerhetsgrupper datasjölagerkonton](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Med ACL: er för åtgärder på filsystem
Data Lake Store är ett hierarkiskt filsystem som Hadoop Distributed File System (HDFS) och stöder [POSIX ACL: er](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Den styr läsa (r), skriva (b) och köra (behörigheter till resurser för rollen som ägare, för gruppen ägare och för andra användare och grupper x). I Data Lake Store Public Preview (den aktuella versionen), kan ACL:er aktiveras i rotmappen, undermappar och enskilda filer. Mer information om hur åtkomstkontrollposter fungerar i kontexten för Data Lake Store finns [Åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).

Vi rekommenderar att du definierar ACL: er för flera användare med hjälp av [säkerhetsgrupper](../active-directory/active-directory-groups-create-azure-portal.md). Lägga till användare i en säkerhetsgrupp och tilldela sedan ACL: er för en fil eller mapp till säkerhetsgruppen. Detta är användbart när du vill ange anpassade åtkomst eftersom du är begränsad till att lägga till upp till nio poster för anpassade åtkomst. Mer information om hur du skyddar data som lagras i Data Lake Store med hjälp av Azure Active Directory-säkerhetsgrupper bättre finns [tilldela användare eller säkerhetsgrupp som ACL: er till Azure Data Lake Store-filsystem](data-lake-store-secure-data.md#filepermissions).

![Visa en lista över standard och anpassad åtkomst](./media/data-lake-store-security-overview/adl.acl.2.png "listan standard och anpassad åtkomst")

## <a name="network-isolation"></a>Isolering av nätverk
Använd Data Lake Store för att styra åtkomsten till ditt datalager på nätverksnivån. Du kan fastställa brandväggar och definiera ett intervall med IP-adresser för dina betrodda klienter. Klienter som har en IP-adress inom det angivna intervallet kan ansluta till Data Lake Store med ett IP-adressintervall.

![Brandväggsinställningar och IP-åtkomst till](./media/data-lake-store-security-overview/firewall-ip-access.png "inställningar och IP-adress för brandvägg")

## <a name="data-protection"></a>Dataskydd
Azure Data Lake Store skyddar dina data under hela sin livslängd. För data under överföring använder Data Lake Store branschstandard Transport Layer Security (TLS) protokollet för att skydda data över nätverket.

![Kryptering för Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "kryptering i Data Lake Store")

Data Lake Store innehåller också kryptering för data som lagras i kontot. Du kan välja att ha krypterade data eller välja ingen kryptering. Om du väljer i för kryptering av är data som lagras i Data Lake Store krypterad innan lagrar på beständiga medium. I sådana fall Data Lake Store automatiskt krypterar data innan beständighet och dekrypterar data innan hämtning, så att den är helt transparent för klienten att komma åt data. Det finns inga kodändring krävs på klientsidan för att kryptera/dekryptera data.

Data Lake Store ger två lägen för hantering, av nycklar för att hantera master krypteringsnycklarna (MEKs) som krävs för att dekryptera data som lagras i Data Lake Store. Du kan antingen låta Data Lake Store hantera MEKs för dig, eller välja att behålla ägarskap för MEKs med Azure Key Vault-konto. Du kan ange läget för nyckelhantering medan när du skapar ett Data Lake Store-konto. Mer information om hur du skapar en krypteringsrelaterad konfiguration finns i [Kom igång med Azure Data Lake Store med hjälp av Azure Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Granskning och diagnostiska loggar
Du kan använda granskning eller av diagnostiska loggar, beroende på om du söker efter loggar för datorhanteringsrelaterade aktiviteter eller data som är relaterade aktiviteter.

* Management-relaterade aktiviteter använder Azure Resource Manager API: er och är anslutna i Azure-portalen via granskningsloggar.
* Data som är relaterade aktiviteter använder WebHDFS REST API: er och är anslutna i Azure-portalen via diagnostikloggar.

### <a name="auditing-logs"></a>Granskningsloggar
För att följa föreskrifter kan kan en organisation kräva tillräcklig granskningshistorik om behöver gräva till specifika incidenter. Data Lake Store har inbyggd övervakning och granskning och loggar alla aktiviteter för hantering av kontot.

Visa och välja de kolumner som du vill logga för granskningshistorik för kontot management. Du kan också exportera granskningsloggar till Azure Storage.

![Granskningsloggar](./media/data-lake-store-security-overview/audit-logs.png "Granskningsloggar")

### <a name="diagnostic-logs"></a>Diagnostikloggar
Du kan ange dataåtkomst granskningshistorik i Azure-portalen (i diagnostikinställningar) och skapa ett Azure Blob storage-konto som var loggfilerna lagras.

![Diagnostikloggar](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostikloggar")

När du har konfigurerat diagnostikinställningar du kan visa loggarna på den **diagnostikloggar** fliken.

Mer information om hur du arbetar med diagnostikloggar med Azure Data Lake Store finns [åtkomst till diagnostikloggarna för Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Sammanfattning
Enterprise-kunder kräver en analytics molnet dataplattform som är säkert och enkelt att använda. Azure Data Lake Store är utformat för att dessa krav via Identitetshantering och autentisering via Azure Active Directory-integrering, ACL-baserad auktorisering, isolering av nätverk, datakryptering vid överföring och i vila (i framtiden kommer)-adressen och granskning.

Om du vill se nya funktioner i Data Lake Store Skicka oss din feedback den [Data Lake Store UserVoice forum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Kom igång med Data Lake Store](data-lake-store-get-started-portal.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

