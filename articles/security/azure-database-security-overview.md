---
title: "Översikt över säkerheten i aaaAzure databasen | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över säkerhetsfunktionerna i hello Azure-databas."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: TomSh
ms.openlocfilehash: 13f14b99d15800e85e9906a9d167eb0adf2135de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-overview"></a>Översikt över säkerheten i Azure-databas

Säkerhet är ett viktigt mål vid hantering av databaser och det har alltid en prioritet för Azure SQL Database. Azure SQL Database stöder anslutningssäkerhet med brandväggsregler och krypterad anslutning. Den stöder autentisering med användarnamn och lösenord och Azure Active Directory-autentisering, som använder identiteter som hanteras av Azure Active Directory. Auktorisering använder rollbaserad åtkomstkontroll.

Azure SQL Database stöder kryptering genom att utföra realtid kryptering och dekryptering av databaser, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello program.

Microsoft tillhandahåller många möjligheter tooencrypt företagsdata:

-   Cellnivå kryptering tooencrypt specifika kolumner eller även dataceller med olika krypteringsnycklar.
-   Om du behöver en maskinvarusäkerhetsmodul eller central hantering av hierarkin kryptering kan du använda Azure Key Vault med SQL Server i en Azure VM.
-   Alltid krypterat (för närvarande under förhandsgranskning) gör kryptering transparent tooapplications och ger klienter tooencrypt känsliga data i klientprogram utan att dela hello krypteringsnycklarna med SQL-databas.

Azure SQL Database Auditing kan företag toorecord händelser tooan audit inloggningen Azure Storage. SQL Database Auditing är integrerat med Microsoft Power BI toofacilitate nedåt rapporter och analys.

 SQL Azure-databaser kan vara väl skyddad toosatisfy mest reglerande eller säkerhetskrav, bland annat HIPAA, ISO 27001/27002 och PCI DSS nivå 1. En lista över efterlevnad Säkerhetscertifieringar är tillgänglig på hello [Microsoft Azure Trust Center plats](http://azure.microsoft.com/support/trust-center/services/).

Den här artikeln beskriver hur hello grunderna i att skydda Microsoft Azure SQL-databaser för Structured, tabeller och relationella Data. Den här artikeln i synnerhet, hjälper dig att komma igång med resurser för att skydda data, kontrollera åtkomst och proaktiv övervakning.

Översikt över säkerheten i Azure-databas artikeln fokuserar på hello följande områden:

-   Skydda data
-   Åtkomstkontroll
-   Proaktiv övervakning
-   Centraliserad hantering
-   Azure Marketplace

## <a name="protect-data"></a>Skydda data

SQL-databas skyddar dina data genom att tillhandahålla kryptering för data i rörelse med [Transport Layer Security](https://support.microsoft.com/kb/3135244)för rest-data med hjälp av [Transparent datakryptering](http://go.microsoft.com/fwlink/?LinkId=526242), och för data i användning med [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

I det här avsnittet pratar vi om:

-   Kryptering i rörelse
-   Vilande kryptering
-   Kryptering används (klient)

För andra sätt tooencrypt Överväg dina data

-   [Kryptering på cellnivå](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt specifika kolumner eller även celler med olika krypteringsnycklar.
-   Om du behöver en Hardware Security Module eller central hantering av krypteringsnyckelns hierarki, bör du använda [Azure Key Vault med SQL Server i en virtuell Azure-dator](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

### <a name="encryption-in-motion"></a>Kryptering i rörelse

Ett vanligt problem för alla klient/server-program behövs hello sekretess när data flyttas över offentliga och privata nätverk. Om data flyttas över ett nätverk inte är krypterade, är det hello chansen att den kan skapas och blir stulen av obehöriga användare. När du hanterar databastjänster, måste toomake att data som är krypterade mellan hello databasen klienten och servern och mellan database-servrar som kommunicerar med varandra och med mellannivå program.

Ett problem när du administrerar ett nätverk är att säkra data som skickas mellan program över ett osäkrat nätverk. Du kan använda [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) tooauthenticate servrar och klienter och sedan använda tooencrypt meddelanden mellan hello autentiserade parterna.

En TLS/SSL-klienten skickar en message tooa TLS/SSL-server i hello autentiseringsprocessen och hello servern svarar med hello information hello servern behöver tooauthenticate sig själv. hello-klienten och servern utför ytterligare ett utbyte av sessionsnycklar och hello autentisering dialogrutan slutförs. När autentiseringen är slutförd kan SSL-skyddad kommunikation startas mellan hello server och hello-klient som använder hello symmetriska krypteringsnycklar som fastställts under autentiseringsprocessen hello.

Alla anslutningar tooAzure SQL-databas Kräv kryptering (SSL/TLS) på alla tider när data är tooand ”under överföring” hello-databasen. SQL Azure använder TLS/SSL tooauthenticate servrar och klienter och sedan använda tooencrypt meddelanden mellan hello autentiserade parterna. I anslutningssträngen för ditt program måste du ange parametrarna tooencrypt hello anslutning och inte tootrust hello servercertifikat (det gör du om du kopierar anslutningssträngen utanför hello klassiska Azure-portalen), annars hello anslutningen kommer inte verifierar hello hello server och kommer att utsättas för ”man-in-the-middle”-attacker. För hello ADO.NET drivrutin, till exempel dessa anslutning string-parametrar är Encrypt = True och TrustServerCertificate = False.

### <a name="encryption-at-rest"></a>Vilande kryptering
Du kan utföra flera åtgärder toohelp säker hello databasen som en säker system utformas, kryptera konfidentiell tillgångar och skapa en brandvägg runt hello databasservrar. Dock i ett scenario där hello fysiska media (till exempel enheter eller band) blir stulna, kan en skadlig part bara återställa eller koppla hello databasen och bläddra hello data.

En lösning är tooencrypt hello känsliga data i hello-databasen och skyddar hello nycklar som används tooencrypt hello data med ett certifikat. Detta förhindrar att någon utan hello nycklar med hjälp av hello data, men den här typen av skydd måste planeras.

stöd för det här problemet, SQL Server och Azure SQL toosolve [Transparent Data kryptering (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). TDE krypterar SQL Server och Azure SQL Database-datafiler, kallas även krypteringsdata i vila.

Azure SQL Database transparent datakryptering skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello program.  

TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen. I SQL-databas skyddas hello databaskrypteringsnyckeln av inbyggda servercertifikat. hello inbyggda servercertifikatet är unikt för varje SQL Database-server.

Om en databas finns i någon GeoDR-relation, skyddas den av en annan nyckel på varje server. Om två databaser är anslutna toohello de delar samma server, hello samma inbyggda certifikat. Microsoft roteras automatiskt dessa certifikat minst var 90: e dag. En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Kryptering används (klient)

De flesta dataintrång involverar hello stöld av kritiska data, till exempel kreditkortsnummer eller personligt identifierbar information. Databaser kan vara skatt troves känslig information. De kan innehålla kundernas personliga data, konkurrenskraftiga konfidentiell information och immateriell egendom. Borttappade eller stulna data, särskilt kundinformation kan resultera i varumärken skador och konkurrenskraftiga nackdelen allvarliga böter – även rättegångar.

![Alltid krypterad](./media/azure-databse-security-overview/azure-database-fig1.png)

[Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) är en funktion som utformats tooprotect känsliga data, till exempel kreditkortsnummer eller nationella ID-nummer (till exempel USA personnummer), lagras i Azure SQL Database eller SQL Server-databaser. Alltid krypterat klienterna tooencrypt känsliga data i klientprogram och aldrig avslöja hello kryptering nycklar toohello databasmotorn (SQL Database eller SQL Server).

Krypterad ger alltid en åtskillnad mellan dem som äger hello data (och visa det) och de som hanterar hello data (men bör har ingen åtkomst). Genom att kontrollera lokal databasadministratörer molnet ansvariga för databasen eller andra privilegierad, men obehöriga användare kan inte komma åt hello krypterade data

Dessutom görs Always Encrypted transparent tooapplications för kryptering. En Always Encrypted-aktiverade installerade drivrutiner på hello klientdatorn så att den kan kryptera och dekryptera känsliga data i hello klientprogrammet automatiskt. hello drivrutinen krypterar hello data i känsliga kolumner innan informationen skickas hello data toohello Database Engine och skriver frågor automatiskt så att hello semantik toohello program bevaras. På liknande sätt dekrypterar hello drivrutinen transparent data som lagras i krypterade databaskolumner som ingår i frågeresultatet.

## <a name="access-control"></a>Åtkomstkontroll
tooprovide säkerhet, SQL-databas kontrollerar åtkomsten med brandväggsregler begränsa anslutningen IP-adressen autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering mekanismer för att begränsa användare toospecific åtgärder och data.

### <a name="database-access"></a>Åtkomst till databasen

Dataskydd börjar med att kontrollera åtkomst till tooyour data. hello datacenter som är värd för dina data hanterar fysisk åtkomst medan du kan konfigurera en brandvägg toomanage säkerhet på hello nätverksnivån. Du kan också styra åtkomsten genom att konfigurera inloggningar för autentisering och definiera rättigheter för servern och databasen roller.

I det här avsnittet pratar vi om:

-   Brandvägg och brandväggsregler
-   Autentisering
-   Auktorisering

#### <a name="firewall-and-firewall-rules"></a>Brandvägg och brandväggsregler

Microsoft Azure SQL Database tillhandahåller en relationsdatabastjänst för Azure och andra Internetbaserade program. toohelp skydda dina data, brandväggar förhindra all åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet. hello brandväggen beviljar åtkomst toodatabases baserat på hello kommer IP-adressen för varje begäran. Mer information finns i [Översikt över Azure SQL Database-brandväggsregler](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Hej [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) tjänsten finns endast tillgängliga via TCP-port 1433. tooaccess en SQL-databas från datorn, se till att klienten datorn brandväggen tillåter att utgående TCP-kommunikation på TCP-port 1433. Om det inte behövs för andra program, blockera inkommande anslutningar på TCP-port 1433.

#### <a name="authentication"></a>Autentisering

SQL-autentisering för databasen refererar toohow du styrka din identitet när du ansluter toohello databas. SQL Database stöder två typer av autentisering:

-   **SQL-autentisering:** en enkel inloggningskontot skapas när en logisk SQL-instans skapas, kallas hello prenumeranten konto för SQL-databasen. Det här kontot ansluter med hjälp av [SQL Server-autentisering](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (användarnamn och lösenord). Det här kontot är en administratör på hello logiska server-instansen och på alla användardatabaser ansluten toothat instans. hello behörigheter för hello prenumeranten konto kan inte begränsas. Endast ett av dessa konton kan finnas.
-   **Azure Active Directory-autentisering:** [Azure Active Directory-autentisering](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) är en mekanism för anslutning tooMicrosoft Azure SQL Database och SQL Data Warehouse med hjälp av identiteter i Azure Active Directory ( Azure AD). Det här kan du toocentrally Hantera identiteter för databasanvändare.

![Autentisering](./media/azure-databse-security-overview/azure-database-fig2.png)

 Fördelarna med Azure Active Directory-autentisering är:
  - Det ger ett alternativ tooSQL Server-autentisering.
  - Även stoppa hello ökande mängden av användaridentiteter över databasservrar & tillåter lösenord rotation i en enda plats.
  - Du kan hantera databasen med hjälp av grupper för externa (Azure Active Directory).
  - Men du kan inte lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure Active Directory.

#### <a name="authorization"></a>Auktorisering
[Auktorisering](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) toowhat refererar en användare kan utföra i en Azure SQL Database och detta styrs av ditt användarkonto databasen [rollmedlemskap](https://msdn.microsoft.com/library/ms189121) och [på objektnivå behörigheter](https://msdn.microsoft.com/library/ms191291.aspx). Auktorisering är hello process för att fastställa vilka skyddbara resurser som en huvudansvarig kan komma åt och vilka åtgärder tillåts för dessa resurser.

### <a name="application-access"></a>Programåtkomst

I det här avsnittet pratar vi om:

-   Dynamisk datamaskning
-   Säkerhet på radnivå

#### <a name="dynamic-data-masking"></a>Dynamisk datamaskning
En representant på ett Callcenter kan identifiera anropare med flera siffrorna i sina personnummer eller kreditkortsnummer, men dessa data objekt inte får vara helt exponeras toohello representant.

En maskningsregel kan definieras som masker alla men hello sista fyra siffrorna för personnummer eller kreditkortsnummer i hello resultatet av en fråga.

![Dynamisk datamaskning](./media/azure-databse-security-overview/azure-database-fig3.png)

Ett annat exempel är vara en lämplig mask definierade tooprotect personligt identifierbar information (PII) data, så att utvecklare kan fråga produktionsmiljöer i felsökningssyfte utan brott mot förordningar.

[SQL-databas dynamisk Datamaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare. Dynamisk datamaskning stöds för hello V12-versionen av Azure SQL Database.

[Dynamisk datamaskning](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) förhindrar obehörig åtkomst toosensitive data genom att du toodesignate hur mycket av hello känsliga data tooreveal med minimal påverkan på hello programnivå. Det är en funktion för principbaserad säkerhet som döljer hello känsliga data i hello resultatet av en fråga över avsedda databasfält medan hello data i hello databas inte ändras.


> [!Note]
> Dynamisk datamaskning kan konfigureras med hello Azure Database admin-administratören eller ansvarig säkerhetsroller.

#### <a name="row-level-security"></a>Säkerhet på radnivå
Ett annat vanliga säkerhetskrav för flera databaser är [säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx). Den här funktionen kan du toocontrol åtkomst toorows i en databastabell baserat på hello egenskaper för hello användaren kör en fråga (t.ex. gruppen medlemskap eller köra sammanhang).

![-Säkerhet på radnivå](./media/azure-databse-security-overview/azure-database-fig4.png)

hello åtkomst begränsning logik är finns i hello databasnivå i stället för avstånd från hello data i ett annat program skikt. hello databassystemet gäller hello åtkomstbegränsningar varje gång ett försök gjordes att dataåtkomst från alla skikt. Detta gör ditt säkerhetssystem mer tillförlitlig och stabil genom att minska hello ytan på ditt säkerhetssystem.

Säkerhet på radnivå introducerar predikat åtkomstkontroll. Den har en flexibel, centraliserad predikat-baserade utvärderingsversion som kan ta hänsyn metadata eller andra villkor Hej administratör avgör efter behov. hello predikat används som ett kriterium toodetermine hello användaren har hello åtkomstbehörighet toohello data baserat på användarattribut eller inte. Etikett-baserad åtkomstkontroll kan implementeras med hjälp av predikat-baserad åtkomstkontroll.

## <a name="proactive-monitoring"></a>Proaktiv övervakning
SQL-databas skyddar dina data genom att tillhandahålla **granskning** och **hotidentifiering** funktioner.

### <a name="auditing"></a>Granskning
SQL Database Auditing ökar din möjlighet toogain insyn i händelser och ändringar som sker inom hello-databasen, inklusive uppdateringar och frågor mot hello data.

[Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure Storage-konto. Granskning kan hjälpa dig att upprätthålla regelefterlevnad, förstå databasaktiviteter och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser. Granskning kan och underlättar följer toocompliance standarder men garantera inte efterlevnad.

SQL Database Auditing kan du:

-   **Behåll** redovisningsspårning markerade händelser. Du kan definiera kategorier av databasen åtgärder toobe granskas.
-   **Rapporten** på Databasaktivitet. Du kan använda förkonfigurerade rapporter och en instrumentpanel tooget snabbt igång med aktivitet och rapportera händelser.
-   **Analysera** rapporter. Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.

Det finns två metoder för granskning:

-   **Blobbgranskning** -loggarna skrivs tooAzure Blob Storage. Detta är en senare granskning metod som ger högre prestanda, stöder högre granularitet på objektnivå granskning och är mer kostnadseffektivt.
-   **Tabell granskning** -loggarna skrivs tooAzure Table Storage.

### <a name="threat-detection"></a>Hotidentifiering
[Azure SQL Database-hotidentifiering](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) upptäcker misstänkta aktiviteter som indikerar potentiella säkerhetshot. Hotidentifiering kan toorespond toosuspicious händelser i hello-databas, till exempel SQL injektioner när de inträffar. Den ger aviseringar och tillåter hello användning av Azure SQL Database Auditing tooexplore hello misstänkta händelser.

![Hotidentifiering](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Till exempel är SQL injection en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program. Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet fält, brott mot eller ändra data i hello-databas.

Säkerhet polis eller andra avsedda administratörer kan få en omedelbar avisering om misstänkt databasaktiviteter när de inträffar. Varje meddelande innehåller information om hello misstänkt aktivitet och rekommenderar hur toofurther undersöka och minimera hotet hello.        

## <a name="centralized-security-management"></a>Centraliserad hantering

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) hjälper dig att förebygga, upptäcka och åtgärda toothreats. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) kan du skydda data i SQL-databas genom att tillhandahålla insyn i hello säkerheten för servrar och databaser. Med Security Center kan du:

-   Definiera principer för kryptering av SQL-databas och granskning.
-   Övervaka hello säkerheten för SQL Database-resurser i alla dina prenumerationer.
-   Snabbt identifiera och åtgärda säkerhetsproblem.
-   Integrera aviseringar från [Azure SQL Database hotidentifiering](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
-   Security Center stöder rollbaserad åtkomst.

## <a name="azure-marketplace"></a>Azure Marketplace

hello Azure Marketplace är en online marketplace program och tjänster som gör att nystartade företag och oberoende leverantörer (ISV) toooffer kunderna lösningar tooAzure hello världen.
hello Azure Marketplace kombinerar ekosystem för Microsoft Azure-partner i en enhetlig plattform toobetter fungera våra kunder och partner. Klicka på [här](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) tooglance säkerhetsprodukter på databasen finns på Azure-marknadsplatsen.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [skydda din Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- Lär dig mer om [Azure Security Center och Azure SQL Database-tjänsten](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- toolearn mer om hotidentifiering, se [Hotidentifiering för SQL-databasen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- Det finns fler toolearn [förbättra SQL databasprestanda](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 
