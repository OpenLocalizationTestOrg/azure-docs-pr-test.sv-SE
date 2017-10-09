---
title: "tekniska aaaAzure säkerhetsfunktioner | Microsoft Docs"
description: "Läs mer om molnbaserad databearbetning tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise."
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
ms.date: 05/26/2017
ms.author: TomSh
ms.openlocfilehash: a0ef17883be54dab4cb6b597204f3197dc05c28c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-technical-capabilities"></a>De tekniska funktionerna i Azure-säkerhet

tooassist aktuella och framtida Azure-kunder att förstå och använda hello olika säkerhetsrelaterade funktioner tillgängliga i och omgivande hello Azure-plattformen, Microsoft har utvecklat en serie faktablad, säkerhet översikter, bästa praxis och Checklistor. Hej avsnitt angivet bredd och djup och uppdateras regelbundet. Det här dokumentet är en del av serien som sammanfattas i hello abstrakt nedan. Mer information om den här serien i Azure-säkerhet finns i (URL).

## <a name="azure-platform"></a>Azure-plattformen

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) en molnplattform som består av infrastruktur och programtjänster, med integrerad datatjänster och avancerade analyser och utvecklingsverktyg och tjänster, som finns i Microsofts offentligt molndata datacenter. Kunder kan använder Azure för många olika kapacitet och scenarier, från grundläggande beräkning, nätverk och lagring, toomobile och webb-app-tjänster, toofull scenarier som Sakernas Internet, och användas med öppen källkod tekniker och distribueras som hybrid molnet eller finns inom en kund datacenter. Azure tillhandahåller molnteknik som byggblock toohelp företag spara kostnader, förnya snabbt och hantera system proaktivt. När du bygger på, eller migrera IT tillgångar tooa molntjänstleverantör, måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Microsoft Azure är hello endast databehandling molnprovider som erbjuder en säker och konsekvent tillämpning plattform och infrastructure-as-a-service för team toowork inom sina kunskaper i annat moln och nivåer av projektet komplexiteten med integrerad data tjänster och analys som upptäcka intelligence från data oavsett var den finns i både Microsoft och icke-Microsoft-plattformar, öppna ramverk och verktyg för att tillhandahålla alternativ för att integrera moln med lokala samt distribuera Azure-molntjänster i lokalt datacenter. Som en del av hello Microsoft betrodda Cloud beroende kunder Azure för branschledande säkerhet, tillförlitlighet, kompatibilitet, sekretess och hello stora nätverk av personer, partners och processer toosupport organisationer i hello molnet.

Med Microsoft Azure kan du:

- Accelerera innovation hello moln.

- Power affärsbeslut och appar med insikter.

- Skapa fritt och distribuera var som helst.

- Skydda verksamheten.

## <a name="scope"></a>Omfång

hello centrum i den här rapporten avser säkerhetsfunktioner och funktioner som stöder kärnkomponenterna i Microsoft Azure, nämligen [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL-databaser](https://docs.microsoft.com/azure/sql-database/), [Microsoft Azure virtuella modellen](https://docs.microsoft.com/azure/virtual-machines/  ), och hello verktyg och infrastruktur som hanterar allt. I det här dokumentet fokuserar på Microsoft Azure tekniska funktioner tillgängliga tooyou som kunder toofulfil deras roll för att skydda hello säkerhet och sekretess för sina data.

hello vikten av att förstå den här delade ansvar modellen är mycket viktigt för kunder som flyttar toohello moln. Molntjänstleverantörer erbjuder betydande fördelar för säkerhet och efterlevnad, men dessa fördelar dessutom inte hello kunden från att skydda sina användare, program och tjänster.

Hello kunden ansvarar för IaaS-lösningar eller har delat ansvar för att skydda och hantera hello operativsystem, konfiguration av nätverk, program, identitet, klienter och data.  PaaS-lösningar som bygger på IaaS-distributioner, hello kunden fortfarande ansvarig eller en delad ansvarar för att skydda och hantera program, identitet, klienter och data. För SaaS-lösningar, Nonetheless, fortsätter hello kunden toobe ansvariga. De måste säkerställa att data har klassificerats korrekt och de delar en ansvar toomanage sina användare och enheter för slutpunkt.

Det här dokumentet ger inte detaljerade täckning av hello relaterade Microsoft Azure platform-komponenter som Azure-webbplatser, Azure Active Directory, HDInsight, Media Services och andra tjänster som lager ovanpå hello kärnkomponenter. Även om det finns en miniminivå för allmän information, antas läsare bekant med Azure grundläggande koncept som beskrivs i andra referenser som tillhandahålls av Microsoft och som ingår i länkarna i detta dokument.


## <a name="available-security-technical-capabilities-toofulfil-user-customer-responsibility---big-picture"></a>Tillgängliga säkerhetsuppdateringar tekniska funktioner toofulfil användaren (kunden) ansvar - översikt

Microsoft Azure tillhandahåller tjänster som kan hjälpa kunderna uppfyller hello säkerhet, sekretess och uppfylla. Hej följande bild som hjälper dig att förklara olika Azure-tjänster tillgängliga för användare toobuild en infrastruktur för skyddade och kompatibla program baserat på branschstandarder.

![Tillgängliga säkerhetsuppdateringar tekniska funktioner Big-bild](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Hantera och styra identitets- och åtkomst (skydda)

Azure hjälper dig att skydda affärsdata och personlig information genom att aktivera du toomanage användaridentiteter och autentiseringsuppgifter och kontrollera åtkomst.

### <a name="azure-active-directory"></a>Azure active directory

Microsoft identitets- och lösningar hjälp IT skydda åtkomst tooapplications och resurser över hello företagets datacenter och i hello moln aktivera nivåer av verifiering, till exempel multifaktorautentisering och villkorlig åtkomstprinciper. Misstänkt aktivitet med avancerad säkerhet rapportering, granskning och aviseringar hjälper dig att minimera potentiella säkerhetsproblem. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) ger enkel inloggning toothousands molnappar (SaaS) och åtkomst tooweb program som du kör lokalt.

Säkerhet fördelar för Azure Active Directory (AD) hello möjlighet att:

- Skapa och hantera en enstaka identitet för varje användare inom företaget hybrid synkronisera användare, grupper och enheter.

- Ge åtkomst för enkel inloggning tooyour program, bland annat tusentals förintegrerade SaaS-appar.

- Aktivera åtkomst programsäkerhet genom att tillämpa regelbaserad Multifaktorautentisering för både lokala program och molnprogram.

- Etablera säker fjärråtkomst tooon lokala webbprogram via Azure AD Application Proxy.

[Azure active directory-portalen](http://aad.portal.azure.com/) finns en del av azure-portalen. Från den här instrumentpanelen kan du få en översikt över hello tillståndet för din organisation och enkelt fördjupa dig i Hantera hello directory, användare eller programåtkomst.

![Azure active directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Följande är kärnfunktioner Azure Identity management:

- Enkel inloggning

- Multi-Factor Authentication

- Säkerhetsövervakning, aviseringar och machine learning-baserade rapporter

- Hantering av konsumentidentitet och åtkomst

- Enhetsregistrering

- Privileged identity management

- Identitetsskydd

#### <a name="single-sign-on"></a>Enkel inloggning

[Enkel inloggning (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) innebär att kan tooaccess alla hello program och resurser som du behöver toodo företag genom att logga in en gång med ett enda användarkonto. När du är inloggad kan komma åt alla hello-program som du behöver, utan att nödvändiga tooauthenticate (till exempel ange ett lösenord) en andra gång.

Många organisationer förlita sig på programvara som en tjänst (SaaS)-program, till exempel Office 365, rutan och Salesforce för slutanvändarens produktivitet. Tidigare tooindividually för IT-personal som behövs för skapa och uppdatera användarkonton i varje SaaS-program och var användarna tvungna tooremember ett lösenord för varje SaaS-program.

[Azure AD utökar lokala Active Directory i hello moln](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), aktivera användare toouse sina primära organisationens konto toonot bara logga in tootheir domänanslutna enheter och företagets resurser, men även alla hello webb- och SaaS-program behövs för sitt jobb.

Inte bara behöver användare inte toomanage flera uppsättningar av användarnamn och lösenord, programåtkomst kan vara automatiskt etablerade eller frigör etablerade utifrån organisationens grupper och deras status som en medarbetare. [Azure AD introducerar säkerhets- och styrning kontroller](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) som gör att du toocentrally hantera användarnas åtkomst i SaaS-program.

#### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

[Azure Multi-Factor authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) är en autentiseringsmetod som kräver hello använder mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner. [MFA hjälper dig att skydda](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) åtkomst till toodata och applikationer samtidigt som du uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd alternativ för verifiering – telefonsamtal, textmeddelande eller mobilapp meddelande eller verifiering kod och tredje parts OAuth token.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Säkerhetsövervakning, aviseringar och machine learning-baserade rapporter

Säkerhetsövervakning och aviseringar och machine learning-baserade rapporter som identifierar inkonsekventa åtkomstmönster kan hjälpa dig att skydda din verksamhet. Du kan använda Azure Active Directory-åtkomst och användning rapporter toogain insyn i hello integritet och säkerhet för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.

I hello klassiska Azure-portalen eller via [Azure Active directory-portalen](http://aad.portal.azure.com/), [rapporter](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) kategoriseras i hello följande sätt:

- Avvikelseidentifiering rapporter – innehålla händelser att påträffades toobe avvikande inloggning. Vårt mål är toomake du känner till denna aktivitet och aktivera toobe kan toodecide om en händelse är misstänkt.

- Integrerade program rapporter – tillhandahåller insikter om hur molnprogram används i din organisation. Azure Active Directory möjliggör integrering med tusentals molnprogram.

- Felrapporter – indikera fel som kan uppstå vid etablering av konton tooexternal program.

- Användarspecifika rapporter – visa enheten/logga i aktivitetsdata för en viss användare.

- Aktivitetsloggar – innehåller en post på alla granskade händelser inom hello senaste 24 timmarna, senaste 7 dagarna, eller senaste 30 dagarna, och ändringar av aktiviteten och aktiviteten för återställning och registrering av lösenord.

#### <a name="consumer-identity-and-access-management"></a>Hantering av konsumentidentitet och åtkomst

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) är en hög tillgänglighet, globala identity management-tjänsten för konsumentinriktade program som kan skalas toohundreds miljoner identiteter. Den kan integreras över mobila plattformar och webbaserade plattformar. Dina användare kan logga in tooall programmen via anpassningsbara upplevelser genom att använda sina befintliga sociala konton eller genom att skapa nya autentiseringsuppgifter.

I hello tidigare programutvecklare som ville ha för[registrera och logga in användare](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) i sina program tvungna att skriva egen kod. Och de använde lokala databaser eller system toostore användarnamn och lösenord. Azure Active Directory B2C erbjuder organisationen ett bättre sätt toointegrate identitetshanteringen i program med hjälp av hello av en säker, standardbaserad plattform och en stor uppsättning utökningsbara principer.

När du använder Azure Active Directory B2C kan kan dina användare registrera dig för ditt program med hjälp av sina befintliga sociala konton (Facebook, Google, Amazon, LinkedIn) eller genom att skapa nya autentiseringsuppgifter (e-postadress och lösenord eller användarnamn och lösenord).

Enhetsregistrering

[Azure AD Device Registration](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) är hello grunden för enhetsbaserad [villkorlig åtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) scenarier. När en enhet är registrerad ger Azure Active Directory Device Registration en identitet som används tooauthenticate hello när hello användare loggar in hello enheten. hello autentiserade enheten och hello attribut för hello enhet, kan du sedan vara används tooenforce villkorliga åtkomstprinciper för program som finns i hello molnet och lokalt.

När den kombineras med en [hantering av mobila enheter (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) lösning, till exempel Intune, hello attributen i Azure Active Directory uppdateras med ytterligare information om hello enhet. Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad.

#### <a name="privileged-identity-management"></a>Privileged identity management

[Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) kan du hantera, styr, och övervaka Privilegierade identiteter och åtkomst till tooresources i Azure AD samt andra Microsoft-onlinetjänster som Office 365 eller Microsoft Intune.

Ibland behöver användare toocarry Privilegierade åtgärder i Azure eller Office 365 resurser eller andra SaaS-appar. Detta innebär ofta organisationer har toogive dem permanent privilegierad åtkomst i Azure AD. Detta är en allt större säkerhetsrisk för molnbaserade resurser eftersom organisationer inte tillräckligt övervaka vad användarna gör med deras administratörsrättigheter. Om ett användarkonto med privilegierad åtkomst äventyras, kan dessutom som en överträdelse påverka deras övergripande säkerheten för molnet. Azure AD Privileged Identity Management hjälper tooresolve denna risk.

Azure AD Privileged Identity Management kan du:

- Se vilka användare som är administratörer för Azure AD

- Aktivera på begäran, just-in-time ”administrativ åtkomst tooMicrosoft onlinetjänster som Office 365 och Intune

- Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar

- Få meddelanden om åtkomst tooa Privilegierade roller

#### <a name="identity-protection"></a>Identitetsskydd

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) är en security-tjänst som tillhandahåller en samlad vy riskhändelser och potentiella säkerhetsproblem som påverkar din organisations identiteter. Identity Protection använder befintliga Azure Active Directorys avvikelseidentifiering identifieringsfunktionerna (tillgängligt genom Azure AD avvikande aktivitetsrapporter) och ger nya risk händelsetyper som kan identifiera avvikelser i realtid.

## <a name="secured-resource-access-in-azure"></a>Åtkomst till skyddade resurser i Azure

Åtkomstkontroll i Azure startar ur faktureringsinformation. hello ägaren till ett Azure-konto som nås genom att besöka hello [Azure Accounts Center](https://account.windowsazure.com/subscriptions), är hello kontot Administratör (AA). Prenumerationer är en behållare för fakturering, men de fungerar som en säkerhetsgräns: varje prenumeration har en Service systemadministratörskontot (SA) som kan lägga till, ta bort och ändra Azure-resurser i den prenumerationen genom att använda hello [klassiska Azure-portalen](https://manage.windowsazure.com/). hello standard SA för en ny prenumeration är hello AA men hello AA kan ändra hello SA i hello Azure Accounts Center.

![Åtkomst till skyddade resurser i Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Prenumerationer har också en association med en katalog. hello directory definierar en uppsättning användare. Det kan vara användare från hello arbetet eller skolan som skapat hello katalog eller de kan vara externa användare (det vill säga Microsoft Accounts). Prenumerationer är tillgängliga för en delmängd av de directory-användare som har tilldelats som tjänsten systemadministratörskontot (SA) eller Medadministratör (CA); hello endast undantaget är att äldre skäl Microsoft Accounts (tidigare Windows Live ID) kan tilldelas som SA eller Certifikatutfärdare utan att vara närvarande i hello directory.

Säkerhet-inriktade företag bör fokusera på och ger anställda hello exakt behörigheter de behöver. För många behörigheter kan exponera en konto tooattackers. För få behörigheter innebär att anställda kan få arbetet gjort effektivt. [Azure rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) hjälper dig att lösa problemet genom att erbjuda detaljerad åtkomsthantering för Azure.

![Åtkomst till skyddade resurser ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Med RBAC kan du särskilja uppgifter i din grupp och ge endast hello mängd åtkomst toousers de behöver tooperform sitt arbete. Obegränsad istället för att ge alla behörigheter i din Azure-prenumeration eller resurser, kan du tillåta endast vissa åtgärder. Till exempel använda RBAC toolet en medarbetare hantera virtuella datorer i en prenumeration, medan en annan kan hantera SQL-databaser inom hello samma prenumeration.

![Åtkomst till skyddade resurser i Azure(RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Azure datasäkerhet och -kryptering (skydda)

En av hello nycklar toodata skydd i hello molnet redovisning för hello möjliga tillstånd som kan uppstå i dina data och vilka kontroller som är tillgängliga för det aktuella tillståndet. Rekommendationer om bästa praxis hello vara runt hello följande data tillstånd för Azure datasäkerhet och kryptering.

- I vila: Detta innehåller all information som lagringsobjekt, behållare och typer som finns statiskt på fysiska media som ska vara det magnetiska eller optical disk.

- Under överföring: När data överförs mellan komponenter, platser eller program, som hello nätverket mellan en service bus (från lokala toocloud och vice versa, inklusive hybridanslutningar, till exempel ExpressRoute), eller under en in-/ utdata process, den är ungefär som i rörelse.

### <a name="encryption--rest"></a>Kryptering @ rest

tooachieve kryptering i vila i hello följande:

Stöd för minst en av hello rekommenderas kryptering modeller som beskrivs i följande tooencrypt tabelldata hello.

| Kryptering modeller |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Krypteringen av Server | Krypteringen av Server | Krypteringen av Server | Klienten kryptering
| Kryptering för serversidan med tjänsten hanterade nycklar | Kryptering för serversidan med Customer-Managed nycklar i Azure Key Vault | Kryptering för serversidan med hjälp av lokal kunden hanterade nycklar |
| • Azure Resursproviders operationer hello kryptering och dekryptering <br> • Microsoft hanterar hello nycklar <br>• Fullständig molnet funktioner | • Azure Resursproviders operationer hello kryptering och dekryptering<br>• Kunden styr nycklar via Azure Key Vault<br>• Fullständig molnet funktioner | • Azure Resursproviders operationer hello kryptering och dekryptering <br>• Kunden styr nycklar lokal <br> • Fullständig molnet funktioner| • Azure-tjänster inte kan se dekrypterade data <br>• Kunder behålla nycklar på lokalt (eller i andra säkra lagrar). Nycklarna är inte tillgängliga tooAzure tjänster <br>• Minskas molnet funktioner|

### <a name="enabling-encryption-at-rest"></a>Aktivera kryptering i vila

**Identifiera alla platser lagrar-Data**

hello målet för kryptering i vila är tooencrypt alla data. Detta eliminerar hello möjligheten att viktiga data eller alla beständiga platser saknas. Räkna upp alla data som lagras av ditt program. 

> [!Note] 
> Inte bara ”application data” eller ”personligt identifierbar information, men alla data som rör tooapplication inklusive konto metadata (prenumeration mappningar kontraktet information, personligt identifierbar information).

Överväg att vad lagrar du använder toostore data. Exempel:

- Extern lagringsenhet (till exempel SQL Azure-dokumentet DB, HDInsights, Data Lake, etc.)

- Tillfälligt lagringsutrymme (alla lokala cacheminnet som innehåller klientdata)

- I cacheminnet (kan användas i hello växlingsfilen.)

### <a name="leverage-hello-existing-encryption-at-rest-support-in-azure"></a>Utnyttja befintliga hello-kryptering i rest-stöd i Azure

Utnyttja hello befintliga kryptering på Rest support för varje butik som du använder.

- Azure Storage: Se [Azure Storage Service-kryptering av vilande Data](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- SQL Azure: Finns [Transparent datakryptering (TDE), SQL Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx)

- Virtuella och lokala disklagring ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

Använd Azure Disk Encryption för lagring av Virtuella och lokala diskar där de stöds:

IaaS

Tjänster med IaaS-VM (Windows eller Linux) ska använda [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) tooencrypt volymer som innehåller kundens data.

PaaS v2

Tjänster som körs på PaaS v2 med hjälp av Service Fabric kan använda Azure disk encryption för Skaluppsättning för virtuell dator [VMSS] tooencrypt deras PaaS v2 virtuella datorer.

PaaS v1

Azure Disk Encryption stöds för närvarande inte på PaaS v1. Därför måste du använda programnivå kryptering tooencrypt sparade data i vila.  Detta inkluderar, men är inte begränsat till programdata, temporära filer, loggar och minnesdumpar krascher.

De flesta tjänster ska försöka tooleverage hello kryptering av en lagringsresursprovidern. Vissa tjänster ha toodo explicit kryptering, till exempel alla sparade nyckelmaterial (certifikat, root / master-nycklar) måste vara lagrad i Nyckelvalvet.

Om du stöder kryptering för tjänsten på klientsidan med kundhanterad nycklar måste det toobe ett sätt för hello kunden tooget hello nyckeln toous. hello stöds och rekommenderade sätt toodo som genom att integrera med Azure Key Vault (AKV). I det här fallet kan kunder lägga till och hantera sina nycklar i Azure Key Vault. En kund kan lära dig hur toouse AKV via [komma igång med Key Vault](http://go.microsoft.com/fwlink/?linkid=521402).

toointegrate med Azure Key Vault lägger du till kod toorequest en nyckel från AKV vid behov för dekryptering.

- Se [Azure Key Vault – steg för steg](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) information om hur toointegrate med AKV.

Om du stöder hanterade Kundnycklar, behöver du tooprovide en UX för hello kunden toospecify vilka toouse Key Vault (eller Key Vault-URI).

Som kryptering i vila innefattar hello kryptering av värden, infrastruktur- och data, innebär hello förlust av hello nycklar förfaller toosystem fel eller skadliga aktiviteter alla hello krypterade data går förlorade. Det är därför viktigt att krypteringen Rest-lösning har en omfattande katastrofåterställning artikel flexibel toosystem fel och skadlig aktivitet.

Tjänster som implementerar kryptering i vila är vanligtvis fortfarande sårbara toohello krypteringsnycklar eller data som lämnas okrypterad på hello värdenheten (till exempel i hello växlingsfilen för hello värdens operativsystem.) Tjänster måste du därför kontrollera hello värdvolym för sina tjänster krypteras. toofacilitate beräkning team har aktiverat hello distributionen av värden kryptering, som använder [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP och tillägg toohello DCM-tjänsten och agenten tooencrypt hello värdvolym.

De flesta tjänster implementeras på standard virtuella Azure-datorer. Sådana tjänster ska få [värden kryptering](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) när beräkning aktiveras automatiskt den. För tjänster som körs i beräkning är hanterade kluster värden kryptering aktiverat automatiskt som Windows Server 2016 lyfts.

### <a name="encryption-in-transit"></a>Kryptering under överföring

Skydda data under överföringen ska väsentlig del av strategin för skydd av data. Eftersom data flyttas fram och tillbaka från många platser, rekommenderas hello allmänna att du alltid använda SSL/TLS-protokollen tooexchange data mellan olika platser. I vissa fall kanske du vill tooisolate hello hela kommunikationskanalen mellan din lokala och moln infrastruktur med hjälp av ett virtuellt privat nätverk (VPN).

För data som flyttas mellan din lokala infrastruktur och Azure, bör du lämpliga skyddsåtgärder, till exempel HTTPS eller VPN.

För organisationer som behöver toosecure åtkomst från flera arbetsstationer finnas lokalt tooAzure använder [Azure plats-till-plats-VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

För organisationer som behöver toosecure åtkomst från en arbetsstation som finns lokalt tooAzure, Använd [punkt-till-plats VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Större datauppsättningar kan flyttas dedikerade snabba WAN-länk som [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Om du väljer toouse ExpressRoute, du kan också kryptera hello data på hello på programnivå med hjälp av [SSL/TLS](https://support.microsoft.com/kb/257591) eller andra protokoll för extra skydd.

Om du interagerar med Azure Storage via hello Azure-portalen, alla transaktioner ska ske via HTTPS. [Storage REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan också vara används toointeract med [Azure Storage](https://azure.microsoft.com/services/storage/) och [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisationer som inte tooprotect data under överföring är mer känslig för [man-in-the-middle-attacker](https://technet.microsoft.com/library/gg195821.aspx), [avlyssning](https://technet.microsoft.com/library/gg195641.aspx), och sessionskapning. Dessa attacker kan vara hello första steget i att få åtkomst till tooconfidential data.

Du kan lära dig mer om Azure VPN-alternativ genom att läsa hello artikel [planering och design för VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Tillämpa fil nivån datakryptering

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) använder kryptering, identitet och auktorisering principer toohelp skydda filer och e-post. Azure RMS fungerar över flera enheter – telefoner, surfplattor och datorer genom att skydda både inom organisationen och utanför organisationen. Den här funktionen är möjligt eftersom Azure RMS lägger till en skyddsnivå som finns kvar med hello data, även när de lämnar organisationens gränser.

När du använder Azure RMS tooprotect dina filer som används med branschstandardkryptografi fullständigt stöd för [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). När du använder Azure RMS för dataskydd, har hello garantier att hello skyddet kvar hello-fil, även om det är kopierade toostorage som inte är under hello kontroll av IT-avdelningen, till exempel en molntjänst för lagring. hello samma inträffar för filer som delas via e-post, skyddas hello-filen som en bifogad fil tooan e-postmeddelande, med instruktioner för hur tooopen hello skyddade bifogade filen.
När du planerar för införandet av Azure RMS rekommenderar vi hello följande:

- Installera hello [RMS-delningsappen](https://technet.microsoft.com/library/dn339006.aspx). Den här appen integreras med Office program genom att installera ett Office-tillägget så att användarna kan enkelt skydda filer direkt.

- Konfigurera program och tjänster toosupport Azure RMS

- Skapa [anpassade mallar](https://technet.microsoft.com/library/dn642472.aspx) som motsvarar dina affärsbehov. Exempel: en mall för övre hemlig information som ska användas i alla översta hemlighet relaterade e-postmeddelanden.

Organisationer som är beroende av på [dataklassificering](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) och Filskydd kan vara svårare toodata sprids. Organisationer inte utan rätt Filskydd vara kan tooobtain affärsinsikter, övervaka missbruk och förhindra obehörig åtkomst toofiles.

> [!Note]
> Du kan lära dig mer om Azure RMS genom att läsa hello artikel [komma igång med Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Skydda ditt program (skydda)
Azure är ansvarig för att skydda hello infrastruktur och plattform som tillämpningsprogrammet körs på, men det är ditt ansvar toosecure tillämpningsprogrammet sig själv. Med andra ord måste toodevelop, distribuera och hantera din programkod och innehåll på ett säkert sätt. Utan detta, kan din programkod eller innehåll fortfarande vara utsatt toothreats.

### <a name="web-application-firewall-waf"></a>Brandvägg för webbaserade program (WAF)
[Brandvägg för webbaserade program (Brandvägg)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) är en funktion i [Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) som tillhandahåller centraliserad skyddet av dina webbapplikationer från vanliga kryphål och säkerhetsproblem.

Brandvägg för webbaserade program baserat på regler från hello [OWASP core uppsättningar](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 eller 2.2.9. Webbprogram blir i allt större utsträckning föremål för attacker där kända svagheter i programmen utnyttjas. Vanliga bland dessa kryphål SQL injection attacker, över flera webbplatser attacker tooname några. Förhindrar sådana attacker i programkod kan vara svårt och kan kräva rigorösa underhåll, uppdatering och övervakning av flera lager av hello programmet topologi. En brandvägg för centraliserad webbaserade program gör säkerhetshantering mycket enklare och ger bättre säkerhet tooapplication administratörer mot hot och intrång. En Brandvägg-lösning kan också reagera tooa säkerhetshot snabbare med uppdatering av ett känt problem på en central plats jämfört med att skydda alla enskilda webbprogram. Befintliga programgatewayer kan enkelt vara Programgateway för konverterade tooa web application-brandväggen är aktiverad.

Vissa av hello vanliga web säkerhetsrisker som Brandvägg för webbaserade program som skyddar mot innehåller:

- Skydd mot SQL-inmatning

- Skydd mot skriptkörning över flera webbplatser

- Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil

- Skydd mot åtgärder som inte följer HTTP-protokollet

- Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas

- Skydd mot robotar, crawlers och skannrar

- Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)

> [!Note]
> En detaljerad lista över regler och deras skydd finns hello följande [Core uppsättningar](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure tillhandahåller också flera lätt att använda funktioner toohelp skydda både inkommande och utgående trafik för din app. Azure också hjälper kunder skyddas sina programkod genom ett externt tillhandahålls funktionen tooscan ditt webbprogram för säkerhetsproblem.

- [Säkra din webbapp med olika metoder för autentisering och auktorisering](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization)

    - [Konfigurera Azure Active Directory-autentisering för din app](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)


- [Skydda trafik tooyour app genom att aktivera Transport Layer Security (TLS/SSL) - HTTPS](https://docs.microsoft.com/azure/app-service-web/web-sites-configure-ssl-certificate)

    - [Tvinga all inkommande trafik via HTTPS-anslutning](http://microsoftazurewebsitescheatsheet.info/)

  - [Aktivera strikt transportsäkerhet (HSTS)](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)


- [Begränsa åtkomst tooyour app genom att klientens IP-adress](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Begränsa åtkomst tooyour app genom att klientens beteende - begäran frekvens och samtidighet](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Skanna web app-kod för säkerhetsproblem med Tinfoil Security Scanning](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Konfigurera TLS ömsesidig autentisering toorequire klienten certifikat tooconnect tooyour webbapp](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth)

- [Konfigurera ett certifikat för användning från din app toosecurely ansluta tooexternal resurser](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Ta bort standard huvuden tooavoid serververktyg från fingeravtryck din app](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [På ett säkert sätt ansluta din app med resurser i ett privat nätverk med punkt-till-plats VPN](https://docs.microsoft.com/azure/app-service-web/web-sites-integrate-with-vnet)

- [På ett säkert sätt ansluta din app med resurser i ett privat nätverk med Hybridanslutningar](https://docs.microsoft.com/azure/app-service-web/web-sites-hybrid-connection-get-started)

Azure Apptjänst använder hello samma program mot skadlig kod som används av Azure-molntjänster och virtuella datorer. toolearn mer information om detta finns tooour [program mot skadlig kod dokumentationen](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Skydda nätverket (skydda)
Microsoft Azure innehåller en robust nätverk infrastruktur toosupport programmet och service anslutningskrav. Nätverksanslutningen är möjlig mellan resurser i Azure, mellan lokala och Azure värdbaserade resurser, och tooand från hello Internet och Azure.

Hej [Azure nätverksinfrastruktur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) aktiverar du toosecurely ansluta Azure-resurser tooeach andra med [virtuella nätverk (Vnet)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour nätverksprenumerationen. Du kan ansluta Vnet tooyour lokala nätverk.

![Skydda nätverket (skydda)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Om du behöver grundläggande nivån nätverksåtkomstkontroll (baserat på IP-adress och hello TCP eller UDP-protokoll), så du kan använda [Nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). En Nätverkssäkerhetsgrupp (NSG) är ett grundläggande tillståndskänslig paket filtrering brandvägg och det gör att du toocontrol åtkomst baserat på en [5-tuppel](https://www.techopedia.com/definition/28190/5-tuple).

Azure nätverk stöder hello möjlighet toocustomize hello dirigeringsbeteendet för nätverkstrafik på dina virtuella Azure-nätverk. Du kan göra detta genom att konfigurera [användardefinierade vägar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) i Azure.

[Tvingad tunneltrafik](https://www.petri.com/azure-forced-tunneling) är en mekanism som du kan använda tooensure som dina tjänster inte är tillåtna tooinitiate en anslutning toodevices på hello Internet.

Har stöd för Azure dedikerad WAN-länken anslutningen tooyour lokala nätverk och ett Azure Virtual Network med [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). hello länken mellan Azure och din plats använder en dedikerad anslutning som inte går via hello offentliga Internet. Om din Azure-program körs i flera datacenter, kan du använda [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute begäranden från användare som använder det Intelligent mellan instanser av programmet hello. Du kan också dirigera trafik tooservices inte körs i Azure om de är tillgängliga från hello Internet.

## <a name="virtual-machine-security-protect"></a>Säkerhet för virtuell dator (skydda)

[Virtuella Azure-datorer](https://docs.microsoft.com/azure/virtual-machines/) kan du distribuera en mängd olika databehandling lösningar i en flexibel metod. Med support för Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP och Azure BizTalk Services kan du distribuera alla arbetsbelastningar och alla språk på nästan alla operativsystem.

Med Azure, kan du använda [program mot skadlig kod](https://docs.microsoft.com/azure/security/azure-security-antimalware) från säkerhet leverantörer, till exempel Microsoft, Symantec, Trend Micro och Kaspersky tooprotect dina virtuella datorer från skadliga filer, annonsprogram och andra hot.

Microsoft Antimalware för Azure-molntjänster och virtuella datorer är en realtidsskydd funktion som hjälper dig att identifiera och ta bort virus, spionprogram och annan skadlig programvara. Microsoft Antimalware innehåller konfigurerbara aviseringar när känd skadlig eller oönskad programvara försöker tooinstall själva eller köras på din Azure-system.

[Azure-säkerhetskopiering](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) är en skalbar lösning som skyddar dina programdata med noll kapitalinvesteringar och minimala driftskostnader. Programfel kan skada dina data och den mänskliga faktorn kan införa buggar i dina program. Dina virtuella datorer som kör Windows och Linux är skyddade med Azure Backup.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) hjälper till att samordna replikering, redundans och återställning av arbetsbelastningar och appar så att de är tillgängliga från en sekundär plats om den primära platsen kraschar.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Kontrollera efterlevnad: molntjänster på grund av fordringar checklista (skydda)

Microsoft har utvecklat [hello Cloud Services på grund av fordringar checklista](https://aka.ms/cloudchecklist.download) toohelp organisationer utöva på grund av fordringar som de anser vara ett flytta toohello moln. Det finns en struktur för en organisation av storlek och typ, privata företag och offentliga organisationer, inklusive myndigheter på alla nivåer och ideella föreningar – tooidentify sina egna prestanda, service, hantering och styrning mål och krav. Detta innebär att de toocompare hello erbjudanden för olika molntjänstleverantörer som slutligen utgör hello grunden för ett serviceavtal för molnet.

hello checklistan innehåller ett ramverk som justerar satsen av sats med en ny internationella standard för molnet serviceavtal ISO/IEC 19086. Standarden erbjuder enhetlig överväganden för organisationer toohelp dem fatta beslut om molnet införande och skapa en gemensamma grunden för att jämföra Tjänsterbjudanden i molnet.

Checklista för hello befordrar en noggrant granskade flytta toohello molnet, med strukturerade vägledning och ett konsekvent, repeterbara tillvägagångssätt för att välja en molntjänstleverantör.

Molnet antas inte längre bara teknik beslut. Eftersom checklista krav touch på alla aspekter av en organisation, de betjänar tooconvene alla viktiga interna-beslutsfattare – hello chef och CISO samt juridiska, riskerar tekniker för hantering, inköp och kompatibilitet. Detta ökar effektiviteten hello hello beslutsprocess och grunden beslut i ljud skäl, vilket minskar hello sannolikheten för oförutsedda förbi vägspärrar tooadoption.

Dessutom hello Checklista:

- Visar viktiga diskussionsämnen för beslutsfattare hello början av hello implementeringsprocessen för molnet.

- Stöder omfattande business diskussioner om föreskrifter och hello organisationens egen mål för sekretess och personligt identifierbar information (PII) datasäkerhet.

- Hjälper organisationer att identifiera eventuella problem som kan påverka cloud-projekt.

- Ger en enhetlig uppsättning frågor med hello samma villkor, definitioner, mått och leveranser för varje leverantör toosimplify hello processen med att jämföra erbjudanden från olika molntjänst-providers.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure-infrastrukturen och programmet säkerhetsvalidering (identifiera)

[Azure operativ säkerhet](https://docs.microsoft.com/azure/security/azure-operational-security) refererar toohello tjänster, kontroller och funktioner tillgängliga toousers för att skydda sina data, program och andra resurser i Microsoft Azure.

![Säkerhetsvalidering (identifiera)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Azure operativ säkerhet bygger på ett ramverk som inkluderar hello kunskap via en olika funktioner som är unika tooMicrosoft, inklusive hello Microsoft Security Development Lifecycle (SDL), hello Microsoft Security Response Center programmet och djup medvetenhet om hello cybersecurity hotbild.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft operations management suite(OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) är hello IT-hanteringslösning för hello hybridmoln. Ensamt eller tooextend din befintliga System Center-distribution, OMS ger dig hello maximal flexibilitet och kontroll för molnbaserad hantering av infrastrukturen.

![Microsoft operations management suite(OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

Du kan hantera valfri instans i något moln, inklusive lokalt, Azure, AWS, Windows Server, Linux, VMware och OpenStack, till en lägre kostnad än konkurrenskraftiga lösningar med OMS. OMS erbjuder byggt för molnet första hälsningsmeddelande en ny metod toomanaging företaget som är hello snabbaste och mest kostnadseffektiva sätt toomeet nya utmaningar och hantera nya arbetsbelastningar, program och miljöer i molnet.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) tillhandahåller övervakning för OMS genom att samla in data från hanterade resurser i en central databas. Dessa data kan omfatta händelser, prestandadata, eller anpassade data som tillhandahålls via hello API. När samlas in, är hello data tillgängliga för aviseringar, analys och export.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Den här metoden kan du tooconsolidate data från olika källor, så kan du kombinera data från Azure-tjänster med din befintliga lokala miljö. Den också tydligt skiljer hello samling hello data från hello åtgärd på dessa data så att alla åtgärder är tillgängliga tooall typer av data.

### <a name="azure-security-center"></a>Azure security center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kan du förebygga, upptäcka och svara toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser tooidentify potentiella säkerhetsproblem. En lista över rekommendationer hjälper dig att hello konfigureringen av nödvändiga kontroller.

Exempel:

- Etablera program mot skadlig kod toohelp identifiera och ta bort skadlig programvara

- Konfigurera network security grupper och regler toocontrol trafik tooVMs

- Etablering av web application brandväggar toohelp skydd mot angrepp riktade webbprogram

- genomföra alla systemuppdateringar som fattas

- OS-konfigurationer som inte matchar hello-adressering rekommenderade baslinjer

Security Center automatiskt samlar in, analyseras och integreras loggdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar. Om hot upptäcks skapas en säkerhetsavisering. Exempel på hot:

- Angripna virtuella datorer som kommunicerar med kända skadliga IP-adresser

- avancerad skadlig kod upptäckt genom felrapporteringen i Windows

- Brute force-attacker mot virtuella datorer

- säkerhetsaviseringar från integrerade brandväggar och program mot skadlig kod

### <a name="azure-monitor"></a>Övervakare för Azure

[Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) innehåller pekare tooinformation på specifika typer av resurser. Den erbjuder visualisering, fråga, routning, varningar, automatisk skalning och automation på data både från hello Azure-infrastrukturen (aktivitetsloggen) och varje enskild Azure resurs (diagnostikloggar).

Molnprogram är komplicerade med många rörliga delar. Övervakning innehåller data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd. Toostave av eventuella problem eller felsöka efter de som hjälper också till.

![Azure-monitor](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) du kan dessutom använda övervakning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.

Det är viktigt för att identifiera säkerhetsproblem i nätverket och att säkerställa kompatibilitet med din IT-säkerhet och regelverk styrning modellen granskning nätverkssäkerheten. Med säkerhetsgruppen vy du hämta hello konfigureras regler för Nätverkssäkerhetsgruppen och säkerhet, samt hello effektiva säkerhetsregler. Du kan fastställa hello-portar som är öppna och ss nätverks-säkerhetsproblem med hello listan över regler tillämpas.

### <a name="network-watcher"></a>Nätverksbevakaren

[Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på en nivå i till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure. Den här tjänsten innehåller paketinsamling, nästa hopp, IP-flöde Kontrollera NSG flödet loggar i gruppvyn säkerhet. Scenariot nivån övervakning innehåller en end tooend för nätverksresurser i kontrast tooindividual resurs nätverksövervakning.

### <a name="storage-analytics"></a>Lagringsanalys

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) kan lagra mått som innehåller aggregerade statistik och kapacitet transaktionsdata om begäranden tooa storage-tjänst. Transaktioner rapporteras på både åtgärden hello API-nivå samt på hello storage-servicenivå och kapacitet rapporteras vid hello storage-servicenivå. Mätvärdesdata kan vara används tooanalyze lagringskvoten på tjänsten, diagnostisera problem med begäranden som görs mot hello storage-tjänst och tooimprove hello prestanda för program som använder en tjänst.

### <a name="application-insights"></a>Programinsikter

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) är en utökningsbar Management-program (APM)-tjänst för webbutvecklare på flera plattformar. Använd den toomonitor live webbprogrammet. Den identifierar automatiskt prestandaavvikelser. Den innehåller kraftfulla analytics verktyg toohelp du diagnostisera problem och toounderstand vad användare göra med din app. Den är utformad toohelp kontinuerligt förbättra prestanda och användbarhet. Det fungerar för appar på en mängd olika plattformar, inklusive .NET, Node.js och J2EE finns lokalt eller i hello molnet. Den kan integreras med devOps-processen och har anslutning punkter tooa olika utvecklingsverktyg.

Tjänsten övervakar:

- **Begärandefrekvens, svarstider och felfrekvens** – Ta reda på vilka sidor som är mest populära, vid vilka tidpunkter på dagen och var dina användare finns. Se vilka sidor som fungerar bäst. Om svarstiden och felfrekvensen är hög när det finns många begäranden kan det bero på ett resurstilldelningsproblem.

- **Beroendefrekvens, svarstider och felfrekvens** – Ta reda på om externa tjänster gör systemet långsammare.

- **Undantag** – analysera hello samman statistik, eller välja specifika instanser och detaljerat hello stackspårning och relaterade begäranden. Både server- och webbläsarundantag rapporteras.

- **Sidvyer och inläsningsprestanda** – Rapporteras av användarnas webbläsare.

- **AJAX-anrop från webbsidorna** -priser svarstider och fel.

- **Användar- och antal.**

- **Prestandaräknare** från dina Windows- eller Linux-serverdatorer, till exempel processor, minne och nätverksanvändning.

- **Värddiagnostik** från Docker eller Azure.

- **Diagnostikspårningsloggar** från din app – så att du kan jämföra spårningshändelser med begäranden.

- **Anpassade händelser och mått** du skriva själv i hello klient eller server koden, tootrack affärshändelser, till exempel sålda artiklar eller vunna.
hello infrastrukturen för ditt program består normalt av många komponenter – kanske en virtuell dator, storage-konto och virtuella nätverk eller en webbapp, databas, databasserver och 3 tjänster. Du ser inte de här komponenterna som separata entiteter, utan som relaterade delar av samma enhet som är beroende av varandra. Du vill toodeploy, hantera och övervaka dem som en grupp. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kan du toowork med hello resurser i din lösning som en grupp.

Du kan distribuera, uppdatera eller ta bort alla hello resurser i lösningen i en enda, samordnad åtgärd. Du använder en mall för distributionen. Mallen kan användas i olika miljöer, till exempel för testning, mellanlagring och produktion. Resource Manager tillhandahåller säkerhets-, granskning och märkning funktioner toohelp hantera dina resurser efter distributionen.

**hello fördelarna med att använda Resource Manager**

Resource Manager har flera fördelar:

- Du kan distribuera, hantera och övervaka alla hello resurser för din lösning som en grupp i stället för att hantera resurserna separat.

- Du kan flera gånger distribuerar din lösning i hela hello utvecklingslivscykeln och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.

- Du kan hantera infrastrukturen med hjälp av deklarativa mallar i stället för skript.

- Du kan definiera hello beroenden mellan resurser, så att de distribueras i rätt ordning för hello.

- Du kan använda tjänster för åtkomstkontroll tooall i resursgruppen eftersom rollbaserad åtkomstkontroll (RBAC) är inbyggt i hello plattform.

- Du kan lägga till taggar tooresources toologically ordna alla hello resurser i din prenumeration.

- Du kan tydliggöra faktureringen för din organisation genom att visa kostnaderna för en grupp med resurser dela hello samma tagg.

> [!Note]
> Resource Manager tillhandahåller ett nytt sätt toodeploy och hantera dina lösningar. Om du använde hello tidigare distributionsmodellen och vill toolearn om hello ändringarna, se [förstå Resource Manager distribution och klassisk distribution](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om säkerheten genom att läsa in några av våra djupgående säkerhetsfrågor:

- [Granskning och loggning](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Cyberbrott](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Design- och operativ säkerhet](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Kryptering](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Identitets- och åtkomsthantering](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Nätverkssäkerhet](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Hothantering](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
