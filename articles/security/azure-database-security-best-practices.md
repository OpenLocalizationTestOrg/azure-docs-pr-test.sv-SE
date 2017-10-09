---
title: "aaaAzure databasen säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning Metodtips för säkerhet i Azure-databas."
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
ms.date: 07/21/2017
ms.author: tomsh
ms.openlocfilehash: 78984291e8f74879c7f738e2ce3176c4be18d154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-best-practices"></a>Säkerhetsmetoder för Azure-databas

Säkerhet är ett viktigt mål vid hantering av databaser och det har alltid en prioritet för Azure SQL Database. Databasen kan vara nära skyddad toohelp uppfyller de flesta reglerande eller säkerhetskrav, bland annat HIPAA, ISO 27001/27002 och PCI DSS nivå 1. En lista över efterlevnad Säkerhetscertifieringar är tillgänglig på hello [Microsoft Trust Center plats](http://azure.microsoft.com/support/trust-center/services/). Du kan också välja tooplace dina databaser i specifika Azure-Datacenter baserat på föreskrifter.

I den här artikeln diskuteras en samling Azure-databas säkerhetsmetoder. Följande rekommendationer härleds från våra upplevelse med säkerhet på Azure-databas och hello erfarenheter från kunder som dig själv.

För varje bästa praxis förklarar vi:

-   Vilka hello bra
-   Varför du vill tooenable som bästa praxis
-   Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
-   Hur du kan lära dig tooenable hello bästa praxis

Den här artikeln säkerhetsmetoder för Azure-databas är baserad på en konsensus åsikt och Azure plattformsfunktioner och funktionerna som de finns på hello tid som den här artikeln skrevs. Åsikter och tekniker ändras med tiden och den här artikeln kommer att uppdateras på ett regelbundet tooreflect ändringarna.

Azure-databas Metodtips om säkerhet i den här artikeln omfattar:

-   Använd brandväggen regler toorestrict databasåtkomst
-   Aktivera Databasautentisering
-   Skydda dina data med hjälp av kryptering
-   Skydda data under överföring
-   Aktivera granskning för databasen
-   Aktivera hotidentifiering för databasen


## <a name="use-firewall-rules-toorestrict-database-access"></a>Använd brandväggen regler toorestrict databasåtkomst

Microsoft Azure SQL Database tillhandahåller en relationsdatabastjänst för Azure och andra Internetbaserade program. tooprovide åtkomstsäkerhet för SQL-databas kontrollerar åtkomsten med brandväggsregler begränsa anslutningen IP-adressen autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering mekanismer för att begränsa användare toospecific åtgärder och data. Brandväggar förhindrar alla åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet. hello brandväggen beviljar åtkomst toodatabases baserat på hello kommer IP-adressen för varje begäran.

![Brandväggsregler](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

hello Azure SQL Database-tjänsten är bara tillgängliga via TCP-port 1433. tooaccess en SQL-databas från datorn, se till att klienten datorn brandväggen tillåter att utgående TCP-kommunikation på TCP-port 1433. Om det behövs inte för andra program, blockera inkommande anslutningar på TCP-port 1433 med hjälp av brandväggsregler.

Som en del av hello anslutningsprocessen är anslutningar från Azure virtuella datorer omdirigerade tooa annan IP-adress och port, unikt för varje worker-rollen. hello portnumret är hello mellan 11000 too11999. Mer information om TCP-portar finns [portar utöver 1433 för ADO.NET 4.5 och SQL Database2](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> Mer information om brandväggsregler i SQL Database finns [SQL Database-brandväggsregler](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="enable-database-authentication"></a>Aktivera Databasautentisering
SQL Database stöder två typer av autentisering, SQL-autentisering och Azure Active Directory-autentisering (Azure AD-autentisering).

**SQL-autentisering** rekommenderas i följande fall:

-   Det gör SQL Azure toosupport miljöer med blandad operativsystem där inte alla användare autentiseras med en Windows-domän.
-   Tillåter SQL Azure toosupport äldre program och program som tillhandahålls av tredje part som kräver SQL Server-autentisering.
-   Tillåter användare tooconnect från okänt eller ej betrodda domäner. Till exempel statusen ett program där etablerade kunder ansluter med SQL Server-inloggningar tooreceive hello operation.
-   Tillåter SQL Azure toosupport webbaserade program där användare kan skapa sina egna identiteter.
-   Kan program som utvecklare toodistribute sina program med hjälp av en komplex behörighet hierarki baserat på kända förinställningen SQL Server-inloggningar.

> [!Note]
> SQL Server-autentisering kan dock använda Kerberos-säkerhetsprotokoll.

Om du använder **SQL-autentisering** måste du:

-   Hantera hello starka autentiseringsuppgifter själv.
-   Skydda hello autentiseringsuppgifter i anslutningssträngen för hello.
-   (Möjligen) skydda hello autentiseringsuppgifter skickas över nätverket hello från hello Web server toohello-databasen. Mer information finns i [så här: ansluta tooSQL Server använder SQL-autentisering i ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx).

**Azure Active Directory-autentisering** är en mekanism för anslutning tooMicrosoft Azure SQL Database och [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) med hjälp av identiteter i Azure Active Directory (AD Azure). Med Azure AD-autentisering kan du centralt hantera hello identiteter för databasanvändare och andra Microsoft-tjänster på en central plats. Central ID-hantering och erbjuder en enda plats toomanage databasanvändare och förenklar hantering av behörighet. Hello följande fördelar:

-   Det ger ett alternativ tooSQL Server-autentisering.
-   Hello ökande mängden av användaridentiteter stoppar över databasservrar.
-   Tillåter lösenord rotation i en enda plats.
-   Kunder kan hantera databasen med hjälp av externa (AAD)-grupper.
-   Men du kan inte lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure Active Directory.
-   Azure AD-autentisering används innesluten databas användare tooauthenticate identiteter på hello databasnivå.
-   Azure AD stöder tokenbaserad autentisering för program som ansluter tooSQL databas.
-   Azure AD authentication stöder AD FS (domänfederation) eller intern användarlösenord autentisering för en lokal Active Directory med Azure utan domänsynkronisering.
-   Azure AD stöder anslutningar från SQL Server Management Studio som använder Active Directory Universal-autentisering, vilket innefattar Multi-Factor Authentication (MFA). MFA ingår stark autentisering med ett antal alternativ för enkel verifiering – telefonsamtal, textmeddelande, smartkort och PIN-kod eller meddelande i mobilappen. Mer information finns i [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

hello konfigurationssteg omfattar följande procedurer tooconfigure hello och använda Azure Active Directory-autentisering.

-   Skapa och fylla i Azure AD.
-   Valfritt: Koppla eller ändra hello active directory som associeras med din Azure-prenumeration.
-   Skapa en Azure Active Directory-administratör för Azure SQL server eller [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- Konfigurera klientdatorer.
-   Skapa oberoende databasanvändare i din databas mappas tooAzure AD identiteter.
-   Ansluta tooyour databasen med hjälp av Azure AD identiteter.

Du hittar information information [här](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Skydda dina data med hjälp av kryptering

[Azure SQL Database transparent datakryptering (TDE)](https://msdn.microsoft.com/library/dn948096.aspx) hjälper dig att skydda mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler utan vilande kräver ändringar toohello program. TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen.

Även när hello hela lagring krypteras, är det mycket viktigt tooalso kryptera din databas sig själv. Det här är en implementering av hello skydd på djupet metod för att skydda data. Om du använder Azure SQL Database och vill tooprotect känsliga data, till exempel kreditkort eller personnummer, kan du kryptera databaser med FIPS 140-2-validerade 256-bitars AES-kryptering som uppfyller hello av många branschstandarder (till exempel HIPAA, PCI).

Det är viktigt toounderstand filer relaterade för[bufferten pool-tillägg (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) krypteras inte när en databas är krypterat med TDE. Du måste använda filen kryptering på Systemverktyg som [BitLocker](https://technet.microsoft.com/library/cc732774) eller hello [EFS (ENCRYPTING File System)](https://technet.microsoft.com/library/cc700811.aspx) för BPE relaterade filer.

Eftersom en behörig användare som en administratör eller en databasadministratör kan komma åt hello data även om hello databasen är krypterad med TDE, bör du också följa hello rekommendationer nedan:

-   Aktivera SQL-autentisering på hello databasnivå.
-   Använda Azure AD för autentisering med [RBAC-roller](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Användare och program bör använda separata konton tooauthenticate. Det här sättet kan du begränsa hello behörigheterna toousers och program och minska hello risker på skadlig aktivitet.
-   Implementera databasen säkerhetsnivå via fasta databasroller (till exempel db_datareader eller db_datawriter) eller du kan skapa anpassade roller för programmet-toogrant uttryckliga behörigheter tooselected databasobjekt.

För andra sätt tooencrypt Överväg dina data

-   [Kryptering på cellnivå](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt specifika kolumner eller även celler med olika krypteringsnycklar.
-   Kryptering används med hjälp av Always Encrypted: [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) kan klienter tooencrypt känsliga data i klientprogram och aldrig avslöja hello kryptering nycklar toohello databasmotorn (SQL Database eller SQL Server). Därför Always Encrypted ger dig en åtskillnad mellan dem som äger hello data (och visa det) och de som hanterar hello data (men bör har ingen åtkomst).
-   Använder säkerhet på radnivå: säkerhet på radnivå gör det möjligt för kunder toocontrol åtkomst toorows i en databastabell baserat på hello egenskaper för hello användaren kör en fråga (t.ex. gruppen medlemskap eller köra sammanhang). Mer information finns i [Säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131).

## <a name="protect-data-in-transit"></a>Skydda data under överföring
Skydda data under överföringen ska väsentlig del av strategin för skydd av data. Eftersom data flyttar fram och tillbaka från många platser, rekommenderas hello allmänna att du alltid använda SSL/TLS-protokollen tooexchange data mellan olika platser. I vissa fall kanske du vill tooisolate hello hela kommunikationskanalen mellan din lokala och moln infrastruktur med hjälp av ett virtuellt privat nätverk (VPN).

För data som flyttas mellan din lokala infrastruktur och Azure, bör du lämpliga skyddsåtgärder, till exempel HTTPS eller VPN.

För organisationer som behöver toosecure åtkomst från flera arbetsstationer finnas lokalt tooAzure använder [Azure plats-till-plats-VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

För organisationer som behöver toosecure åtkomst från enskilda arbetsstationer som finns lokalt eller tooAzure till andra lokala, Överväg att använda [punkt-till-plats VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Större datauppsättningar kan flyttas dedikerade snabba WAN-länk som [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Om du väljer toouse ExpressRoute, du kan också kryptera hello data på hello på programnivå med hjälp av [SSL/TLS](https://support.microsoft.com/kb/257591) eller andra protokoll för extra skydd.

Om du interagerar med Azure Storage via hello Azure-portalen, alla transaktioner ska ske via HTTPS. [Storage REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan också vara används toointeract med [Azure Storage](https://azure.microsoft.com/services/storage/) och [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisationer som inte tooprotect data under överföring är mer känslig för [man-in-the-middle-attacker](https://technet.microsoft.com/library/gg195821.aspx), [avlyssning](https://technet.microsoft.com/library/gg195641.aspx) och sessionskapning. Dessa attacker kan vara hello första steget i att få åtkomst till tooconfidential data.

Mer om Azure VPN-alternativet genom att läsa hello artikel toolearn [planering och design för VPN-Gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Aktivera granskning för databasen
Granskning av en instans av hello SQL Server Database Engine eller en individuell databas innebär att spåra och loggning av händelser som inträffar på hello databasmotorn. SQL Server audit kan du skapa server granskningar som kan innehålla server audit specifikationer för serverhändelser och databasen audit specifikationer för databashändelser. Granskade händelser kan skrivas toohello händelseloggar eller tooaudit filer.

Det finns flera nivåer av granskning för SQL Server, beroende på myndigheter eller standarder dina krav. SQL Server Audit innehåller hello verktyg och processer som du måste ha tooenable, lagra och visa revisioner på olika objekt för server och databas.

[Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure Storage-konto.

Granskning kan hjälpa dig att upprätthålla regelefterlevnad, förstå databasaktiviteter och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.

Granskning kan och underlättar följer toocompliance standarder men garantera inte efterlevnad.

toolearn mer om granskning för databasen och hur tooenable, Läs hello artikeln [aktivera granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Aktivera hotidentifiering för databasen
SQL-Hotidentifiering kan du toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter. Du får en avisering vid misstänkt databasaktiviteter, potentiella säkerhetsproblem och SQL injection attacker samt avvikande databasen åtkomstmönster. SQL-Hotidentifiering aviseringar ger detaljerad information om misstänkt aktivitet och rekommenderar åtgärd på hur tooinvestigate och minimera hotet hello.

Till exempel är SQL injection en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program. Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet fält, brott mot eller ändra data i hello-databas.

toolearn om hur tooset in hotidentifiering för din databas i hello Azure portal finns [Hotidentifiering för SQL-databasen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Dessutom kan SQL Hotidentifiering integreras med [Azure Security Center](https://azure.microsoft.com/services/security-center/). Vi ber dig tootry det. i 60 dagar för ledig.

Mer information om databasen Hotidentifiering toolearn och hur tooenable, Läs hello artikeln [aktivera granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Slutsats
Azure-databas är en robust databasplattform, med en fullständig uppsättning funktioner som uppfyller kraven för många organisationens och regelmässig efterlevnad. Du kan skydda data genom att kontrollera hello fysisk åtkomst tooyour data och använda en mängd olika alternativ för datasäkerhet på hello fil-, kolumn- eller raden med Transparent datakryptering, cellnivå kryptering eller säkerhet på radnivå. Alltid gör krypterat också att åtgärder mot krypterade data, förenkla hello processen för programuppdateringar. I sin tur tooauditing åtkomstloggar för SQL Database-aktiviteten finns hello information du behöver, så att du tooknow hur och när data används.

## <a name="next-steps"></a>Nästa steg
- toolearn mer information om brandväggsregler, se [regler i brandväggen](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- toolearn om användare och inloggningar, se [hantera inloggningar](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).
- En självstudiekurs finns [skydda din Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
