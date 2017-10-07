---
title: "aaaOverview på säkerheten i Data Lake Store | Microsoft Docs"
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
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 69e20bd046a9427202074baf59bff13f5b6a83ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-in-azure-data-lake-store"></a>Säkerhet i Azure Data Lake Store
Många företag att utnyttja analyser av stordata för business insikter toohelp dem göra beslut för smartkort. En organisation kan ha en komplex och reglerade miljö med ett ökande antal olika användare. Det är viktigt för ett företag toomake att affärskritiska data lagras säkrare, med hello rätt nivå av tooindividual användare för åtkomst. Azure Data Lake Store är utformat toohelp uppfylla dessa säkerhetskrav. I den här artikeln lär dig mer om hello säkerhetsfunktionerna i Data Lake Store, inklusive:

* Autentisering
* Auktorisering
* Isolering av nätverk
* Dataskydd
* Granskning

## <a name="authentication-and-identity-management"></a>Hantering av autentisering och identitet
Autentisering är hello process som användarens identitet har verifierats när hello användare interagerar med Data Lake Store eller någon tjänst som ansluter tooData Lake Store. För Identitetshantering och autentisering Data Lake Store använder [Azure Active Directory](../active-directory/active-directory-whatis.md), en omfattande identitets- och molnet hanteringslösning som förenklar hello hantering av användare och grupper.

Varje Azure-prenumeration kan associeras med en instans av Azure Active Directory. Endast användare och tjänsteidentiteter som definieras i Azure Active Directory-tjänsten har åtkomst till ditt Data Lake Store-konto med hjälp av hello Azure portal, kommandoradsverktyg, eller via klientprogram som din organisation skapar med hjälp av hello Azure Data Datasjölager SDK. Viktiga fördelar med att använda Azure Active Directory som en centraliserad åtkomstkontroll är:

* Förenklad livscykel Identitetshantering. hello identiteten för en användare eller en tjänst (en huvudnamn tjänstidentitet) kan skapas snabbt och snabbt återkallas av helt enkelt ta bort eller inaktivera hello konto i hello directory.
* Multifaktorautentisering. [Multifaktorautentisering](../multi-factor-authentication/multi-factor-authentication.md) ger ett ytterligare lager av säkerhet för användarinloggningar och transaktioner.
* Autentisering från en klient via en öppen standard protokoll, till exempel OAuth eller OpenID.
* Federation med enterprise katalogtjänster och identitet molntjänstleverantörer.

## <a name="authorization-and-access-control"></a>Auktorisering och åtkomstkontroll
När en användare autentiseras av Azure Active Directory så att hello användare har åtkomst till Azure Data Lake Store, auktorisering kontroller behörighet att komma åt Data Lake Store. Data Lake Store avgränsar auktorisering för konto-relaterade och data som är relaterade aktiviteter i hello följande sätt:

* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) (RBAC) tillhandahålls av Azure för kontohantering
* POSIX ACL för åtkomst till data i hello store

### <a name="rbac-for-account-management"></a>RBAC för kontohantering
Fyra grundläggande roller har definierats för Data Lake Store som standard. hello roller tillåter olika åtgärder i ett Data Lake Store-konto via hello Azure-portalen, PowerShell-cmdletar och REST API: er. hello ägare och deltagare roller kan utföra en mängd funktioner för administration på hello-konto. Du kan tilldela hello Reader rollen toousers som bara kan interagerar med data.

![RBAC-roller](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-roller")

Observera att även om rollerna har tilldelats för hantering av vissa roller påverkar åtkomst toodata. Du måste toouse ACL: er toocontrol åtkomst toooperations som en användare kan utföra i hello-filsystemet. hello följande tabell visas en sammanfattning av rights management och data till åtkomstbehörigheter för hello standardroller.

| Roller | Rights Management | Åtkomstbehörighet för data | Förklaring |
| --- | --- | --- | --- |
| Ingen roll |Ingen |Lyder ACL |hello-användare kan inte använda hello Azure-portalen eller Azure PowerShell cmdlets toobrowse Data Lake Store. hello-användare kan använda kommandoradsverktyget verktyg. |
| Ägare |Alla |Alla |hello ägarrollen är en superanvändare. Den här rollen kan hantera allt och har fullständig åtkomst toodata. |
| Läsare |Skrivskyddad |Lyder ACL |rollen för hello läsare kan visa allt om kontohantering, till exempel som användaren har tilldelats toowhich roll. rollen för hello läsare göra inte några ändringar. |
| Deltagare |Alla utom Lägg till och ta bort roller |Lyder ACL |hello deltagarrollen kan hantera vissa aspekter av ett konto, till exempel distributioner och skapa och hantera aviseringar. hello deltagarrollen kan inte lägga till eller ta bort roller. |
| Administratör för användaråtkomst |Lägga till och ta bort roller |Lyder ACL |Hej administratör för användaråtkomst rollen kan hantera användare åtkomst tooaccounts. |

Instruktioner finns i [tilldela användarna eller säkerhetsgrupperna tooData Lake Store-konton](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Med ACL: er för åtgärder på filsystem
Data Lake Store är ett hierarkiskt filsystem som Hadoop Distributed File System (HDFS) och stöder [POSIX ACL: er](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Den styr läsa (r), skriva (b) och köra (behörigheter tooresources för hello ägarrollen, hello ägare av Grupprincip och för andra användare och grupper x). I hello Data Lake Store Public Preview (hello aktuella versionen), kan du aktivera ACL: er hello rotmappen för undermappar och på enskilda filer. Mer information om hur åtkomstkontrollposter fungerar i kontexten för Data Lake Store finns [Åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md).

Vi rekommenderar att du definierar ACL: er för flera användare med hjälp av [säkerhetsgrupper](../active-directory/active-directory-accessmanagement-manage-groups.md). Lägg till säkerhetsgruppen tooa för användare och tilldela sedan hello ACL: er för en fil eller mapp toothat säkerhetsgrupp. Detta är användbart när du vill tooprovide anpassade åtkomst, eftersom du är begränsad tooadding högst nio poster för anpassade åtkomst. Mer information om hur toobetter skyddar data som lagras i Data Lake Store med hjälp av Azure Active Directory-säkerhetsgrupper finns [tilldela användare eller säkerhetsgrupp som ACL: er toohello Azure Data Lake Store-filsystem](data-lake-store-secure-data.md#filepermissions).

![Visa en lista över standard och anpassad åtkomst](./media/data-lake-store-security-overview/adl.acl.2.png "listan standard och anpassad åtkomst")

## <a name="network-isolation"></a>Isolering av nätverk
Använd Data Lake Store toohelp kontroll åtkomst tooyour datalager på hello nätverksnivån. Du kan fastställa brandväggar och definiera ett intervall med IP-adresser för dina betrodda klienter. Med ett IP-adressintervall kan endast klienter som har en IP-adress inom intervallet hello definierats ansluta tooData Lake Store.

![Brandväggsinställningar och IP-åtkomst till](./media/data-lake-store-security-overview/firewall-ip-access.png "inställningar och IP-adress för brandvägg")

## <a name="data-protection"></a>Dataskydd
Azure Data Lake Store skyddar dina data under hela sin livslängd. För data under överföring använder Data Lake Store hello branschstandard Transport Layer Security (TLS) protokollet toosecure data över hello nätverk.

![Kryptering för Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "kryptering i Data Lake Store")

Data Lake Store ger också kryptering för data som lagras i hello-konto. Du valde toohave data krypteras eller välja utan kryptering. Om du väljer i för kryptering av är data som lagras i Data Lake Store krypterade tidigare toostoring på beständiga media. I så fall Data Lake Store automatiskt krypterar data tidigare toopersisting och dekrypterar data tidigare tooretrieval, så att den är helt transparent toohello klientens hello dataåtkomst. Det finns inga kodändring krävs på hello klientens sida tooencrypt/dekryptera data.

Data Lake Store ger två lägen för hantering, av nycklar för att hantera master krypteringsnycklarna (MEKs) som krävs för att dekryptera data som lagras i hello Data Lake Store. Du kan antingen låta Data Lake Store hantera hello MEKs för dig, eller välj tooretain ägarskap för hello MEKs med Azure Key Vault-konto. Du kan ange hello läge för nyckelhantering medan när du skapar ett Data Lake Store-konto. Mer information om hur tooprovide krypteringsrelaterade konfiguration, se [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Granskning och diagnostiska loggar
Du kan använda granskning eller av diagnostiska loggar, beroende på om du söker efter loggar för datorhanteringsrelaterade aktiviteter eller data som är relaterade aktiviteter.

* Management-relaterade aktiviteter använder Azure Resource Manager API: er och är anslutna i hello Azure-portalen via granskningsloggar.
* Data som är relaterade aktiviteter använder WebHDFS REST API: er och är anslutna i hello Azure-portalen via diagnostikloggar.

### <a name="auditing-logs"></a>Granskningsloggar
toocomply bestämmelser en organisation kan kräva tillräcklig granskningshistorik om behöver toodig till specifika incidenter. Data Lake Store har inbyggd övervakning och granskning och loggar alla aktiviteter för hantering av kontot.

Visa och välj hello kolumner för granskningshistorik för kontot management som du vill toolog. Du kan också exportera granska loggarna tooAzure lagring.

![Granskningsloggar](./media/data-lake-store-security-overview/audit-logs.png "Granskningsloggar")

### <a name="diagnostic-logs"></a>Diagnostikloggar
Du kan ange dataåtkomst granskningshistorik i hello Azure-portalen (i diagnostikinställningar) och skapa ett Azure Blob storage-konto där hello loggar lagras.

![Diagnostikloggar](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostikloggar")

När du har konfigurerat diagnostikinställningar du kan visa hello loggar på hello **diagnostikloggar** fliken.

Mer information om hur du arbetar med diagnostikloggar med Azure Data Lake Store finns [åtkomst till diagnostikloggarna för Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Sammanfattning
Enterprise-kunder kräver en analytics molnet dataplattform som är säkrare och lättare toouse. Azure Data Lake Store är utformat toohelp uppfylla dessa krav via Identitetshantering och autentisering via Azure Active Directory-integrering, ACL-baserad auktorisering, isolering av nätverk, kryptering under överföring och i vila (kommer i hello framtida), och granskning.

Om du vill toosee nya funktioner i Data Lake Store kan skicka oss din feedback i hello [Data Lake Store UserVoice forum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Kom igång med Data Lake Store](data-lake-store-get-started-portal.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

