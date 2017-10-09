---
title: "aaaAzure säkerhetsfunktioner som hjälper med identity management | Microsoft Docs"
description: " Den här artikeln innehåller en översikt över hello Azure säkerhetsfunktionerna som hjälper till med Identitetshantering. Microsoft identitets- och lösningar hjälp IT skydda åtkomst tooapplications och resurser över hello företagets datacenter och i hello moln aktivera nivåer av verifiering, till exempel multifaktorautentisering och villkorlig åtkomstprinciper. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: terrylan
ms.openlocfilehash: f08e4f6cf2e48e455a16858b7fee08b53d5aa585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-security-overview"></a>Översikt över säkerheten i Azure identity management
Microsoft identitets- och lösningar hjälp IT skydda åtkomst tooapplications och resurser över hello företagets datacenter och i hello moln aktivera nivåer av verifiering, till exempel multifaktorautentisering och villkorlig åtkomstprinciper. Misstänkt aktivitet med avancerad säkerhet rapportering, granskning och aviseringar hjälper dig att minimera potentiella säkerhetsproblem. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) ger enkel inloggning toothousands molnappar (SaaS) och åtkomst tooweb program som du kör lokalt.

Säkerhet fördelar för Azure Active Directory (AD) hello möjlighet att:

* Skapa och hantera en enstaka identitet för varje användare inom företaget hybrid synkronisera användare, grupper och enheter
* Ge åtkomst för enkel inloggning tooyour program, bland annat tusentals förintegrerade SaaS-appar
* Aktivera åtkomst programsäkerhet genom att tillämpa regelbaserad Multifaktorautentisering för både lokala program och molnprogram
* Etablera säker fjärråtkomst tooon lokala webbprogram via Azure AD Application Proxy

hello syftet med den här artikeln är tooprovide en översikt över hello Azure säkerhetsfunktionerna som hjälper till med Identitetshantering. Vi erbjuder även länkar tooarticles som ger information om varje funktion så kan du läsa mer.  

Hej artikeln fokuserar på hello följande kärnfunktionerna Azure Identity management:

* Enkel inloggning
* Omvänd proxy
* Multi-Factor Authentication
* Säkerhetsövervakning, aviseringar och machine learning-baserade rapporter
* Hantering av konsumentidentitet och åtkomst
* Enhetsregistrering
* Privileged identity management
* Identitetsskydd
* Hybrididentitetshantering

## <a name="single-sign-on"></a>Enkel inloggning
Enkel inloggning (SSO) innebär att som kan tooaccess alla hello program och resurser som du behöver toodo företag genom att logga in en gång med ett enda användarkonto. När du är inloggad du kan komma åt alla hello-program som du behöver, utan att nödvändiga tooauthenticate (till exempel ange ett lösenord) en andra gång.

Många organisationer förlita sig på programvara som en tjänst (SaaS)-program, till exempel Office 365, rutan och Salesforce för slutanvändarens produktivitet. Tidigare tooindividually för IT-personal som behövs för skapa och uppdatera användarkonton i varje SaaS-program och var användarna tvungna tooremember ett lösenord för varje SaaS-program.

Azure AD utökar lokala Active Directory-miljöer i hello moln att aktivera användare toouse sina primära organisationskonto toonot bara logga in tootheir domänanslutna enheter och företagets resurser, men även alla hello webb- och SaaS-program behövs för sitt jobb.

Inte bara behöver användare inte toomanage flera uppsättningar av användarnamn och lösenord, programåtkomst kan vara automatiskt etablerade eller frigör etablerade utifrån organisationens grupper och deras status som en medarbetare. Azure AD introducerar säkerhet och styrning åtkomstkontroller som gör att du toocentrally hantera användarnas åtkomst i SaaS-program.

Läs mer:

* [Översikt över enkel inloggning](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Integrera Azure Active Directory enkel inloggning med SaaS-appar](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Omvänd proxy
Azure AD Application Proxy kan du publicera lokala program som [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) platser [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), och [IIS](http://www.iis.net/)-baserade appar i ditt privata nätverk och ger säker åtkomst toousers utanför nätverket. Programmet Proxy ger fjärråtkomst och enkel inloggning (SSO) för många typer av lokala webbprogram med hello tusentalsavgränsare för SaaS-program som har stöd för Azure AD. Anställda kan logga in tooyour appar från hem på sina egna enheter och autentisera via denna molnbaserade proxy.

Läs mer:

* [Aktivera Azure AD Application Proxy](../active-directory/active-directory-application-proxy-enable.md)
* [Publicera program med Azure AD Application Proxy](../active-directory/active-directory-application-proxy-publish.md)
* [Single-sign-on med Application Proxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Arbeta med villkorlig åtkomst](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure Multi-Factor authentication (MFA) är en autentiseringsmetod som kräver hello använder mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner. MFA hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd alternativ för verifiering – telefonsamtal, SMS och mobilappar meddelande eller verifiering kod och från tredje part OAuth-token.

Läs mer:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Hur Azure Multi-Factor Authentication fungerar](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Säkerhetsövervakning, aviseringar och machine learning-baserade rapporter
Säkerhetsövervakning och aviseringar och machine learning-baserade rapporter som identifierar inkonsekventa åtkomstmönster kan hjälpa dig att skydda din verksamhet. Du kan använda Azure Active Directory-åtkomst och användning rapporter toogain insyn i hello integritet och säkerhet för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.

I hello klassiska Azure-portalen kategoriseras rapporter i hello följande sätt:

* Avvikelseidentifiering rapporter – innehålla händelser att påträffades toobe avvikande inloggning. Vårt mål är toomake du känner till denna aktivitet och aktivera toobe kan toomake fastställa om en händelse är misstänkt.
* Integrerade program rapporter – tillhandahåller insikter om hur molnprogram används i din organisation. Azure Active Directory möjliggör integrering med tusentals molnprogram.
* Felrapporter – indikera fel som kan uppstå vid etablering av konton tooexternal program.
* Användarspecifika rapporter – visa enheten/logga i aktivitetsdata för en viss användare.
* Aktivitetsloggar – innehåller en post på alla granskade händelser inom hello senaste 24 timmarna, senaste 7 dagarna, eller senaste 30 dagarna, och ändringar av aktiviteten och aktiviteten för återställning och registrering av lösenord.

Läs mer:

* [Visa åtkomst- och användningsrapporterna](../active-directory/active-directory-view-access-usage-reports.md)
* [Komma igång med Azure Active Directory Reporting](../active-directory/active-directory-reporting-getting-started.md)
* [Guide för Azure Active Directory Reporting](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Hantering av konsumentidentitet och åtkomst
Azure Active Directory B2C är en hög tillgänglighet, globala identity management-tjänsten för konsumentinriktade program som kan skalas toohundreds miljoner identiteter. Den kan integreras över mobila plattformar och webbaserade plattformar. Dina användare kan logga in tooall programmen via anpassningsbara upplevelser genom att använda sina befintliga sociala konton eller genom att skapa nya autentiseringsuppgifter.

I tidigare hello, programutvecklare som ville ha toosign in och logga in användare i sina program tvungna att skriva egen kod. Och de använde lokala databaser eller system toostore användarnamn och lösenord. Azure Active Directory B2C erbjuder organisationen ett bättre sätt toointegrate identitetshanteringen i program med hjälp av hello av en säker, standardbaserad plattform och en stor uppsättning utökningsbara principer.

När du använder Azure Active Directory B2C kan kan dina användare registrera dig för ditt program med hjälp av sina befintliga sociala konton (Facebook, Google, Amazon, LinkedIn) eller genom att skapa nya autentiseringsuppgifter (e-postadress och lösenord eller användarnamn och lösenord).

Läs mer:

* [Vad är Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C preview: registrera och logga in användare i dina program](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C Preview: Typer av program](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Enhetsregistrering
Azure AD Device Registration är grunden för hello för enhetsbaserad [villkorlig åtkomst](../active-directory/active-directory-conditional-access-device-registration-overview.md) scenarier. När en enhet är registrerad ger Azure Active Directory Device Registration en identitet som används tooauthenticate hello när hello användare loggar in hello enheten. hello autentiserade enheten och hello attribut för hello enhet, kan du sedan vara används tooenforce villkorliga åtkomstprinciper för program som finns i hello molnet och lokalt.

När den kombineras med en mobil enhet (MDM) lösning, till exempel Intune, uppdateras hello attributen i Azure Active Directory med ytterligare information om hello enhet. Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad.

Läs mer:

* [Kom igång med Azure Active Directory Device Registration](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatisk enhetsregistrering med Azure Active Directory för Windows-domänanslutna enheter](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Konfigurera automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Privileged identity management
Azure Active Directory (AD) Privileged Identity Management kan du hantera, styr och övervaka Privilegierade identiteter och åtkomst tooresources i Azure AD samt andra Microsoft online services som Office 365 eller Microsoft Intune.

Ibland behöver användare toocarry Privilegierade åtgärder i Azure eller Office 365 resurser eller andra SaaS-appar. Detta innebär ofta organisationer har toogive dem permanent privilegierad åtkomst i Azure AD. Detta är en allt större säkerhetsrisk för molnbaserade resurser eftersom organisationer inte tillräckligt övervaka vad användarna gör med deras administratörsrättigheter. Om ett användarkonto med privilegierad åtkomst äventyras, kan dessutom som en överträdelse påverka deras övergripande säkerheten för molnet. Azure AD Privileged Identity Management hjälper tooresolve denna risk.

Azure AD Privileged Identity Management kan du:

* Se vilka användare som är administratörer för Azure AD
* Aktivera på begäran, just-in-time ”administrativ åtkomst tooMicrosoft onlinetjänster som Office 365 och Intune
* Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar
* Få meddelanden om åtkomst tooa Privilegierade roller

Läs mer:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Roller i Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Hur tooadd eller ta bort en användarroll](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identitetsskydd
Azure AD Identity Protection är en security-tjänst som tillhandahåller en samlad vy riskhändelser och potentiella säkerhetsproblem som påverkar din organisations identiteter. Identitetsskydd utnyttjar befintlig Azure Active Directorys avvikelseidentifiering identifieringsfunktionerna (tillgängligt genom Azure AD avvikande aktivitetsrapporter) och ger nya risk händelsetyper som kan identifiera avvikelser i realtid.

Läs mer:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD och Identity visa: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybrididentitetshantering
Microsofts strategi tooidentity spänner över lokalt och hello molnet, och skapa en enda användaridentitet för autentisering och auktorisering tooall resurser, oavsett plats.

Läs mer:

* [Vitboken för hybrid identity](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Active Directory-teamets blogg](https://blogs.technet.microsoft.com/ad/)
