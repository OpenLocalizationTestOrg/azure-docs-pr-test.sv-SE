---
title: "aaaAzure Active Directory bevis på koncept playbook byggblock | Microsoft Docs"
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: e54148330a123baf27d7e0f73469ff2a24c0efcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory som bevis på koncept playbook: byggblock

## <a name="catalog-of-roles"></a>Katalog över roller

| Roll | Beskrivning | Bevis på koncept (PoC) ansvar |
| --- | --- | --- |
| **Identity-arkitektur / Utvecklingsteamet** | De är vanligtvis hello något som designerna hello lösning, implementerar prototyper, godkännanden-enheter och slutligen lämnar toooperations | De innehåller hello miljöer och hello de utvärdering hello olika scenarier hello hanterbarhet ur |
| **Lokal identitet driftteamet** | Hanterar hello annan identitet källor lokalt: Active Directory-skogar, LDAP-kataloger, HR-system och Federation identitetsleverantörer. | Ge åtkomst tooon lokala resurser som behövs för hello PoC scenarier.<br/>De bör vara inblandade så lite som möjligt|
| **Tekniska ägare** | Tekniska ägare av hello olika molnappar och tjänster som ska integreras med Azure AD | Innehåller detaljerad information om SaaS-program (potentiellt instanser för att testa) |
| **Global administratör för Azure AD** | Hanterar hello Azure AD-konfiguration | Ange autentiseringsuppgifter tooconfigure hello-synkroniseringstjänsten. Vanligtvis hello samma team som identitet arkitektur under PoC men avgränsa hello operations fas|
| **Databas-teamet** | Ägarna av hello databasinfrastruktur | Ge åtkomst tooSQL miljö (AD FS eller Azure AD Connect) för specifik situation förberedelser.<br/>De bör vara inblandade så lite som möjligt |
| **Nätverk-teamet** | Ägare av hello nätverksinfrastrukturen | Ger nödvändiga åtkomst på hello nätverksnivån för hello synkronisering servrar tooproperly åtkomst hello-datakällor och cloud services (brandväggsregler, öppnade portar, IPSec-regler osv.) |
| **Säkerhetsteamet** | Definierar hello säkerhetsstrategi, analyserar säkerhetsrapporter från olika källor och följer på resultat. | Ange mål säkerhet utvärdering scenarier |

## <a name="common-prerequisites-for-all-building-blocks"></a>Gemensamma krav för alla byggblock

Följande är några förutsättningar för alla POC med Azure AD Premium.

| Förhandskrav | Resurser |
| --- | --- |
| Azure AD-klient som definierats med en giltig Azure-prenumeration | [Hur tooget ett Azure Active Directory-klient](active-directory-howto-tenant.md)<br/>**Obs:** om du redan har en miljö med Azure AD Premium-licenser kan du skaffa en noll cap-prenumeration genom att gå toohttps://aka.ms/accessaad <br/>Mer information finns i: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ och https://technet.microsoft.com/library/dn832618.aspx |
| Domäner som har definierats och verifierats | [Lägg till en anpassad domän namn tooAzure Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Obs:** vissa arbetsbelastningar som Power BI kunde har etablerat en azure AD-klient under hello omfattar. toocheck om en viss domän är associerade tooa klient navigerar toohttps://login.microsoftonline.com/ {domain}/v2.0/.well-known/openid-configuration. Om du får ett lyckat svar, och sedan hello domänen har redan tilldelats tooa klient och tar över kan behövas. I så fall, kan du kontakta Microsoft för ytterligare. Mer information om hello gäller alternativ på: [självbetjäningsregistrering för Azure?](active-directory-self-service-signup.md) |
| Azure AD Premium eller EMS utvärderingsversion aktiverad | [Azure Active Directory Premium gratis för en månad](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Du har tilldelat licenser för Azure AD Premium- eller EMS tooPoC användare | [Licens dig och dina användare i Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Autentiseringsuppgifter för Azure AD Global administratör | [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) |
| Valfritt men rekommenderas starkt: parallella laboratoriemiljö som reserv | [Förutsättningar för Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Directory synkronisering - Lösenordshashsynkronisering (PHS) - ny-Installation

Ungefärlig tid tooComplete: en timme för mindre än 1 000 PoC-användare

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Server tooRun Azure AD Connect | [Förutsättningar för Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| Rikta POC användare i hello samma domän och en del av en säkerhetsgrupp och Organisationsenhet | [Anpassad installation av Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect funktioner som behövs för hello POC identifieras | [Ansluta Active Directory med Azure Active Directory - Konfigurera synkronisering funktioner](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Du har krävs autentiseringsuppgifter för lokala och molnet miljöer  | [Azure AD Connect: Konton och behörigheter](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Hämta hello senaste versionen av Azure AD Connect | [Ladda ned Microsoft Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594) |
| Installera Azure AD Connect med hello enklaste sökvägen: Express <br/>1. Filtrera toohello OU toominimize hello synkronisering måltiden<br/>2. Välj Målet uppsättning användare i hello lokal grupp.<br/>3. Distribuera hello-funktioner som krävs av hello andra POC teman | [Azure AD Connect: Anpassad installation: domän och Organisationsenhet filtrering](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Anpassad installation: grupp baserad filtrering](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: Integrera dina lokala identiteter med Azure Active Directory: Konfigurera synkroniseringsfunktioner](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Öppna hello Azure AD Connect UI och se hello kör profiler slutförda (Import, synkronisering och export) | [Azure AD Connect sync: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Öppna hello [hanteringsportalen för Azure AD](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), gå toohello ”alla användare” bladet, Lägg till ”av” källkolumnen och se hello användare visas markerad korrekt som kommer från ”Windows Server AD” | [Hanteringsportalen för Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Överväganden

1. Titta på hello säkerhetsaspekter för lösenordshashsynkronisering [här](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Om hash-synkronisering för pilotprojekt produktionsanvändare slutgiltigt inte är ett alternativ, bör du hello följande alternativ:
   * Skapa testanvändare i hello produktionsdomänen. Kontrollera att du inte synkroniserar något annat konto
   * Flytta tooan UAT miljö
2.  Om du vill toopursue federation, det kan vara värt toounderstand hello kostnader associerade en federerad lösning med lokala identitetsprovider utöver hello POC och mått som mot hello fördelar du letar efter:
    * Det är i hello kritiska så att du har toodesign för hög tillgänglighet
    * Det är en lokal tjänst som du behöver toocapacity plan
    * Det är en lokal tjänst som du behöver toomonitor/Underhåll/korrigering

Läs mer: [identitet förstå Office 365 och Azure Active Directory - federerad identitet](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Anpassning

Ungefärlig tid tooComplete: 15 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Tillgångar (bilder, logotyper, etc.); Se till att hello tillgångar har hello rekommenderade storlekar för bästa visualiseringen. | [Lägga till företagsanpassning tooyour inloggningssidan i hello Azure Active Directory](active-directory-branding-custom-signon-azure-portal.md) |
| Valfritt: Om hello miljö har AD FS-server kan komma åt toohello server toocustomize webbtema | [AD FS-inloggning användaranpassning](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Inloggning för klienten datorn tooperform slutanvändarupplevelsen |  |
| Valfritt: Mobila enheter toovalidate upplevelse |  |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Gå tooAzure AD Management Portal | [Hanteringsportalen för Azure AD - företagsanpassning](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Överför hello tillgångar för hello inloggningssidan (hjälte logotyp, liten logotyp, etiketter, etc.). Du kan också om du har AD FS kan justera hello samma tillgångar med AD FS-inloggningssidor | [Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanelsidan: anpassningsbara element](active-directory-add-company-branding.md) |
| Vänta några minuter för hello ändra toofully börja gälla |  |
| Logga in med hello POC användarens autentiseringsuppgifter toohttps://myapps.microsoft.com |  |
| Bekräfta hello utseendet och känslan i webbläsare | [Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanelsidan](active-directory-add-company-branding.md) |
| Du kan också bekräfta hello utseendet och känslan i andra enheter |  |

### <a name="considerations"></a>Överväganden

Om hello gamla utseendet och känslan förblir efter hello anpassning Rensa klientcachen för hello webbläsaren och försök hello igen.

## <a name="group-based-licensing"></a>Grupp baserad licensiering

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Alla POC användare är en del av en säkerhetsgrupp (molnet eller lokalt) | [Skapa en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Gå toolicenses bladet i hanteringsportalen för Azure AD | [Azure AD-hanteringsportalen: licensiering](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Tilldela hello licenser toohello säkerhetsgrupp med POC användare. | [Tilldela licenser tooa grupp användare i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Överväganden

Vid eventuella problem går för[scenarier, begränsningar och kända problem med hjälp av grupper toomanage licensiering i Azure Active Directory](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>Konfiguration för SaaS-federerad enkel inloggning

Ungefärlig tid tooComplete: 60 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Testmiljö för hello SaaS-program som är tillgängliga. I den här guiden använder vi ServiceNow som exempel.<br/>Vi rekommenderar starkt toouse test instans toominimize friktion om navigering befintliga kvalitet och mappningar. | Gå toohttps://developer.servicenow.com/app.do#! / home toostart hello processen för att få en test-instans |
| Admin toohello ServiceNow hanteringskonsolen | [Självstudier: Azure Active Directory-integrering med ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Mål uppsättning användare tooassign hello programmet till. En säkerhetsgrupp som innehåller hello PoC användare rekommenderas. <br/>Om att skapa hello gruppen inte är möjligt sedan tilldela hello användare toodirectly toohello program för hello PoC | [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Dela hello självstudiekursen tooall aktörer från Microsoft Documentation  | [Självstudier: Azure Active Directory-integrering med ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Ange ett fungerande möte gör hello självstudiekurs med varje. | [Självstudier: Azure Active Directory-integrering med ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Tilldela hello app toohello gruppen som identifieras i hello krav. Om hello POC har villkorlig åtkomst i hello omfång, kan du besöker som senare och lägga till MFA och liknande. <br/>Observera att detta kommer startar i hello etableringsprocessen (om konfigurerad) |  [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Skapa en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Använda Azure AD management Portal tooadd ServiceNow program från galleriet| [Azure AD management Portal: företagsprogram](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Vad är nytt i Enterprise programhantering i Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| Aktivera ”SAML-baserade inloggning” i ”enkel inloggning” bladet ServiceNow-appen |  |
| Fyll i ”logga URL” och ”ID”-fält med ServiceNow-URL<br/>Hello kryssrutan för ”aktivera nytt certifikat”<br/>och spara inställningar |  |
| Öppna ”Konfigurera ServiceNow” bladet på hello längst ned på hello panelen tooview anpassade anvisningar för hur du tooconfigure ServiceNow |  |
| Följ anvisningarna tooconfigure ServiceNow |  |
| Bladet för ServiceNow-App i ”etablering” aktivera ”automatisk” etablering | [Hantera användarkonto etablering för företagsappar i hello nya Azure-portalen](active-directory-enterprise-apps-manage-provisioning.md) |
| Vänta några minuter medan etablering är slutförd.  I hello tiden kan du kontrollera på hello etablering rapporter |  |
| Logga in toohttps://myapps.microsoft.com/ som en användare som har åtkomst | [Vad är hello åtkomstpanelen?](active-directory-saas-access-panel-introduction.md) |
| Klicka på hello panelen för hello-program som just skapades. Bekräfta åtkomst |  |
| Du kan också kan du kontrollera hello programmet användningsrapporter. Observera att det finns vissa latens, så du måste toowait viss tid toosee hello trafik i hello rapporter. | [Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen: användning av hanterade program](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Kvarhållningsprinciper för rapporter i Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Överväganden

1. Ovan [kursen](active-directory-saas-servicenow-tutorial.md) refererar tooold Azure AD-hanteringen. Men PoC baseras på [Snabbstart](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away) upplevelse.
2. Om hello målprogrammet inte finns i hello-galleriet, kan du använda ”ta med din egen app”. Läs mer: [vad är nytt i Enterprise programhantering i Azure Active Directory: lägga till anpassade program från en plats](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS-lösenord SSO-konfiguration

Ungefärlig tid tooComplete: 15 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Testmiljö för SaaS-program. Ett exempel på lösenord SSO är HipChat och Twitter. För alla andra program måste du hello exakt URL till hello sida med inloggningen html-formulär. | [Twitter på Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat på Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Testa konton för hello program. | [Registrera dig för Twitter](https://twitter.com/signup?lang=en)<br/>[Registrera dig för gratis: HipChat](https://www.hipchat.com/sign_up) |
| Mål uppsättning användare tooassign hello programmet till. En innehöll hello säkerhetsgruppsanvändare rekommenderas. | [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokal administratör åtkomst tooa datorn toodeploy hello Access panelen-tillägg för Internet Explorer, Chrome eller Firefox | [Tillägget för åtkomst-panelen för Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Tillägget för åtkomst-panelen för Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Tillägget för åtkomst-panelen för Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Installera hello webbläsartillägg | [Tillägget för åtkomst-panelen för Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Tillägget för åtkomst-panelen för Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Tillägget för åtkomst-panelen för Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Konfigurera program från galleriet | [Vad är nytt i Enterprise programhantering i Azure Active Directory: hello nya och förbättrade programgalleriet](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurera enkel inloggning | [Hantera enkel inloggning för företagsappar i hello nya Azure-portalen: lösenordsbaserade inloggning](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Tilldela hello app toohello gruppen som identifieras i hello krav | [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Logga in toohttps://myapps.microsoft.com/ som en användare som har åtkomst |  |
| Klicka på hello panelen för hello-program som just skapades. | [Vad är hello åtkomstpanelen?: lösenordsbaserade SSO utan identitet etablering](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Ange hello programmet autentiseringsuppgifter | [Vad är hello åtkomstpanelen?: lösenordsbaserade SSO utan identitet etablering](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Stäng hello webbläsare och upprepa hello inloggningen. Den här gången runt bör hello användare se sömlös åtkomst toohello program. |  |
| Du kan också kan du kontrollera hello programmet användningsrapporter. Observera att det finns vissa latens, så du måste toowait viss tid toosee hello trafik i hello rapporter. | [Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen: användning av hanterade program](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Kvarhållningsprinciper för rapporter i Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Överväganden

Om hello målprogrammet inte finns i hello-galleriet, kan du använda ”ta med din egen app”. Läs mer: [vad är nytt i Enterprise programhantering i Azure Active Directory: lägga till anpassade program från en plats](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Kom ihåg hello följande krav:
   * Programmet måste ha en känd inloggnings-URL
   * hello-inloggningssida ska innehålla ett HTML-formulär med en mer textfält som hello webbläsartillägg kan automatisk ifyllning. Den ska innehålla användarnamn och lösenord på hello minsta.

## <a name="saas-shared-accounts-configuration"></a>SaaS delad konfiguration för konton

Ungefärlig tid tooComplete: 30 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Hej lista över program och hello exakt inloggning URL: er i förväg. Du kan exempelvis använda Twitter. | [Twitter på Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Registrera dig för Twitter](https://twitter.com/signup?lang=en) |
| Delade autentiseringsuppgifter för SaaS-programmet. | [Dela konton med hjälp av Azure AD](active-directory-sharing-accounts.md)<br/>[Azure AD automated lösenord förlängningen för Facebook, Twitter och LinkedIn nu i preview! -Bloggen Enterprise Mobility and Security] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Autentiseringsuppgifter för minst två gruppmedlemmar som kan komma åt hello samma konto. De måste vara en del av en säkerhetsgrupp. | [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokal administratör åtkomst tooa datorn toodeploy hello Access panelen-tillägg för Internet Explorer, Chrome eller Firefox | [Tillägget för åtkomst-panelen för Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Tillägget för åtkomst-panelen för Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Tillägget för åtkomst-panelen för Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Installera hello webbläsartillägg | [Tillägget för åtkomst-panelen för Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Tillägget för åtkomst-panelen för Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Tillägget för åtkomst-panelen för Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Konfigurera program från galleriet | [Vad är nytt i Enterprise programhantering i Azure Active Directory: hello nya och förbättrade programgalleriet](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurera enkel inloggning | [Hantera enkel inloggning för företagsappar i hello nya Azure-portalen: lösenordsbaserade inloggning](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Tilldela hello app toohello gruppen som identifieras i hello krav vid tilldelning av autentiseringsuppgifter | [Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Logga in som olika användare åtkomst till appen som hello **samma delade kontot.**  |  |
| Du kan också kan du kontrollera hello programmet användningsrapporter. Observera att det finns vissa latens, så du måste toowait viss tid toosee hello trafik i hello rapporter. | [Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen: användning av hanterade program](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Kvarhållningsprinciper för rapporter i Azure Active Directory](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Överväganden

Om hello målprogrammet inte finns i hello-galleriet, kan du använda ”ta med din egen app”. Läs mer: [vad är nytt i Enterprise programhantering i Azure Active Directory: lägga till anpassade program från en plats](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Kom ihåg hello följande krav:
   * Programmet måste ha en känd inloggnings-URL
   * hello-inloggningssida ska innehålla ett HTML-formulär med en mer textfält som hello webbläsartillägg kan automatisk ifyllning. Den ska innehålla användarnamn och lösenord på hello minsta.

## <a name="app-proxy-configuration"></a>Appen proxykonfiguration

Ungefärlig tid tooComplete: 20 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Ett Microsoft Azure AD basic eller premium-prenumeration och Azure AD-katalog som du är en global administratör | [Azure Active Directory-versioner](active-directory-editions.md) |
| Ett webbprogram finns lokalt som du vill att tooconfigure för fjärråtkomst |  |
| En server som kör Windows Server 2012 R2 eller Windows 8.1 eller senare, där du kan installera hello Application Proxy Connector | [Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md) |
| Om det finns en brandvägg i hello sökväg, kontrollerar du att den är öppen så att anslutningsverktyget kan göra HTTPS (TCP) hello begär toohello Application Proxy | [Aktivera Application Proxy i hello Azure-portalen: krav för Application Proxy](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Om din organisation använder proxy servrar tooconnect toohello internet, ta en titt på hello blogg efter arbeta med befintliga lokala proxyservrar för information om hur tooconfigure dem. | [Arbeta med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Installera en koppling på hello-server | [Aktivera Application Proxy i hello Azure-portalen: Installera och registrera hello Connector](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| Publicera hello lokala program i Azure AD som ett program med Application Proxy | [Publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md) |
| Tilldela testanvändare | [Publicera program med Azure AD Application Proxy: Lägg till en testanvändare](application-proxy-publish-azure-portal.md#add-a-test-user) |
| Alternativt kan du konfigurera en enkel inloggning för dina användare | [Tillhandahålla enkel inloggning med Azure AD Application Proxy](application-proxy-sso-azure-portal.md) |
| Testa appen genom att logga in tooMyApps portal som tilldelad användare | https://myapps.microsoft.com |

### <a name="considerations"></a>Överväganden

1. Vi rekommenderar att placera hello connector i företagets nätverk, men det finns fall när ser du bättre prestanda att placera det i hello molnet. Läs mer: [nätverk topologiöverväganden när du använder Azure Active Directory Application Proxy](application-proxy-network-topology-considerations.md)
2. För ytterligare säkerhetsinformation om och hur detta ger ett särskilt säker fjärråtkomst lösning genom att bara hantera utgående anslutningar finns: [säkerhetsaspekter för att komma åt appar från en fjärrdator med hjälp av Azure AD Application Proxy](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Allmän LDAP Connector-konfiguration

Ungefärlig tid tooComplete: 60 minuter

> [!IMPORTANT]
> Det här är en avancerad konfiguration kräver bekant med FIM/MIM. Om används i produktionen, rekommenderar vi frågor om den här konfigurationen gå igenom [Premier Support](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Azure AD Connect installeras och konfigureras | Byggblock: [katalogsynkronisering - Hash-synkronisering](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS instans möte krav | [Teknisk referens för allmän LDAP Connector: översikt över hello allmän LDAP Connector](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Lista över arbetsbelastningar som använder och attribut som är kopplade till dessa arbetsbelastningar | [Azure AD Connect-synkronisering: attribut synkroniserade tooAzure Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Lägg till allmän LDAP Connector | [Teknisk referens för allmän LDAP Connector: skapa en ny koppling](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| Skapa körningsprofiler för kopplingen som skapats (fullständig import, Deltaimport, fullständig synkronisering, Deltasynkronisering, export) | [Skapa en Management Agent kör profil](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Med hjälp av anslutningar med hello Azure AD Connect Sync Service Manager](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Kör fullständig import profil och kontrollera att det finns objekt i anslutarplats | [Sök efter ett utrymme Connector-objekt](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Med hjälp av anslutningar med hello Azure AD Connect Sync Service Manager: söka Anslutarplats](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Skapa Synkroniseringsregler, så att objekt i Metaversumet har attribut som krävs för arbetsbelastningar | [Azure AD Connect-synkronisering: bästa praxis för att ändra standardkonfigurationen för hello: ändras tooSynchronization regler](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect-synkronisering: Förstå deklarativ etablering](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect-synkronisering: Förstå uttryck för deklarativ etablering](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Starta fullständig synkroniseringscykel | [Azure AD Connect-synkronisering: Schemaläggaren: starta hello Schemaläggaren](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Gör eventuella fel felsökning | [Felsökning av ett objekt som inte synkroniseras tooAzure AD](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| Kontrollera att LDAP-användare kan logga in och komma åt programmet hello | https://myapps.microsoft.com |

### <a name="considerations"></a>Överväganden

> [!IMPORTANT]
> Det här är en avancerad konfiguration kräver bekant med FIM/MIM. Om används i produktionen, rekommenderar vi frågor om den här konfigurationen gå igenom [Premier Support](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Grupper – delegerad ägarskap

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| SaaS-program (federerad enkel inloggning eller lösenord SSO) har redan konfigurerats | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) |
| Moln-grupp som tilldelas åtkomst toohello program i #1 identifieras | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) <br/>[Skapa en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Autentiseringsuppgifterna för hello gruppägare är tillgängliga | [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md) |
| Autentiseringsuppgifter för hello information worker använder hello appar har identifierats | [Vad är hello åtkomstpanelen?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Identifiera hello-grupp som har beviljats åtkomst toohello program och konfigurera hello ägare av given grupp| [Hantera hello inställningar för en grupp i Azure Active Directory](active-directory-groups-settings-azure-portal.md) |
| Logga in som hello gruppägare, se hello medlemskap i grupper-fliken i åtkomstpanelen | [Azure Active Directory-grupper Management-sidan](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Lägg till hello informationsarbetare som du vill tootest |  |
| Logga in som hello informationsarbetare, bekräfta hello rutan är tillgänglig | [Vad är hello åtkomstpanelen?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Överväganden

Om programmet hello har etablering aktiverad, kanske du måste toowait några minuter för hello etablering toocomplete innan programmet hello som hello informationsarbetare.

## <a name="saas-and-identity-lifecycle"></a>SaaS och identitet livscykel

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| SaaS-program (federerad enkel inloggning eller lösenord SSO) har redan konfigurerats | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) |
| Moln-grupp som tilldelas åtkomst toohello program i #1 identifieras | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) <br/>[Skapa en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Autentiseringsuppgifter för hello information worker använder hello appar har identifierats | [Vad är hello åtkomstpanelen?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Ta bort hello användaren från hello grupp hello app har tilldelats för| [Hantera gruppmedlemskap för användare i din Azure Active Directory-klient](active-directory-groups-members-azure-portal.md) |
| Vänta några minuter för avetablering | [Automatisk Användaretablering för SaaS-App i Azure AD: hur fungerar automatisk etablering arbete?](active-directory-saas-app-provisioning.md#how-does-automated-provisioning-work) |
| Logga in som hello information worker toomy appar portal och bekräfta brickan saknas på en separat webbläsarsession | http://myapps.microsoft.com |


### <a name="considerations"></a>Överväganden

Dra slutsatser hello PoC scenariot tooleavers och/eller frånvaron scenarier. Om hello användare hämtar inaktiverad i lokala AD eller tas bort, finns det inte längre ett sätt toolog i toohello SaaS-program.

## <a name="self-service-access-tooapplication-management"></a>Self Service åtkomst tooApplication Management

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Identifiera POC användare som begär åtkomst toohello program som en del av hello säkerhetsgrupp | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) |
| Målprogrammet distribueras | Byggblock: [SaaS federerad enkel inloggning konfiguration](#saas-federated-sso-configuration) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Gå tooEnterprise program bladet i hanteringsportalen för Azure AD | [Azure AD-hanteringsportalen: Företagsprogram](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Konfigurera program från förutsättningar med självbetjäning | [Vad är nytt i Enterprise programhantering i Azure Active Directory: Konfigurera självbetjäning programåtkomst](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Logga in som hello information worker toomy appar portal | http://myapps.microsoft.com |
| Lägg märke till ”+ Lägg till app” knappen op hello-sidan. Använd den toohello tooget access-appen |  |

### <a name="considerations"></a>Överväganden

hello program valt kanske etablering krav, så kommer omedelbart toohello app kan orsaka fel. Du kan använda detta som en möjlighet tooshow hello hela dataflöde fungerar slutet tooend om hello programmet valt har stöd för etablering med azure ad och den är konfigurerad. Se hello byggblock för [SaaS federerad enkel inloggning Configuration](#saas-federated-sso-configuration) för ytterligare rekommendationer

## <a name="self-service-password-reset"></a>Lösenordsåterställning via självbetjäning

Ungefärlig tid tooComplete: 15 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Aktivera lösenordshantering av självbetjäning i din klient. | [Azure Active Directory återställning av lösenord för IT-administratörer](active-directory-passwords.md) |
| Aktivera lösenord återskrivning toomanage lösenord från lokala. Obs detta kräver vissa Azure AD Connect versioner | [Krav för tillbakaskrivning av lösenord](active-directory-passwords-writeback.md) |
| Identifiera hello PoC användare som kommer att använda den här funktionen och kontrollera att de är medlemmar i en säkerhetsgrupp. hello användare måste vara icke-administratörer toofully samlade hello kapaciteten | [Anpassa: Azure AD-lösenordshantering: begränsa åtkomst toopassword återställning](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Navigera tooAzure AD Management Portal: återställning av lösenord | [Hanteringsportalen för Azure AD: Lösenordsåterställning via](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Fastställa hello princip för lösenordsåterställning. POC ändamål, du kan använda för telefonsamtal och Q & A. Det rekommenderas tooenable registrering toobe krävs vid inloggning tooaccess panelen |  |
| Logga ut och logga in som en informationsanställd |  |
| Ange hello självbetjäning lösenordet för återställning av data som konfigurerats per steg 2 | http://aka.MS/ssprsetup |
| Stäng hello webbläsare |  |
| Börja om från början hello inloggningen som hello informationsarbetare som du använde i steg 4 |  |
| Återställ hello lösenord | [Uppdatera ditt eget lösenord: återställa mitt lösenord](active-directory-passwords-update-your-own-password.md) |
| Försök logga in med ditt nya lösenord tooAzure AD samt tooon lokala resurser |  |

### <a name="considerations"></a>Överväganden

1. Om uppgradering hello Azure AD Connect ska toocause friktion, Överväg att använda mot molnet konton eller göra det en demonstration mot en separat miljö
2. hello administratörer har en annan princip och med Hej administratör kan kontolösenord tooreset hello förorena hello PoC och orsaka förvirring. Kontrollera att du använder en vanlig användare konto tootest hello Återställ åtgärder


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure Multi-Factor Authentication med telefonsamtal

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Identifiera POC-användare som använder MFA  |  |
| Phone med mottagning för MFA-kontrollen  | [Vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Navigera för ”användare och grupper” bladet i hanteringsportalen för Azure AD | [Hanteringsportalen för Azure AD: Användare och grupper](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Välj ”alla användare” bladet |  |
| I hello översta Välj ”Multifaktorautentisering”-knapp | Direkt URL för Azure MFA-portalen: https://aka.ms/mfaportal |
| Välj hello PoC användare i hello ”användare” inställningar och aktivera dem för MFA | [Användartillstånd i Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| Logga in som hello PoC användar- och gå igenom hello bevis startprocessen  |  |

### <a name="considerations"></a>Överväganden

1. hello PoC stegen i den här inställningen uttryckligen MFA för en användare på alla inloggningar byggblock. Det finns andra verktyg, till exempel villkorlig åtkomst och identitetsskydd som engagera MFA på fler scenarier som mål. Detta blir något tooconsider när du flyttar från POC tooproduction.
2. hello PoC stegen i den här byggblock använder explicit telefonsamtal som hello MFA metod för expedience. När du flyttar från POC tooproduction bör du använda program, till exempel hello [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) som din andra faktor när det är möjligt.
Läs mer: [utkast NIST Special Publication 800 63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>MFA villkorlig åtkomst för SaaS-program

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Identifiera PoC användare tootarget hello princip. Dessa användare måste vara i en grupp tooscope hello villkorlig åtkomst säkerhetsprincip | [Konfiguration för SaaS-federerad enkel inloggning](#saas-federated-sso-configuration) |
| SaaS-program har redan konfigurerats |  |
| PoC användare har redan tilldelats toohello program |  |
| Autentiseringsuppgifter toohello POC användare är tillgängliga |  |
| POC användare är registrerad för Multifaktorautentisering. Använda en telefon med mottagning | http://aka.MS/ssprsetup |
| Enheten i hello interna nätverket. IP-adress som konfigurerats i hello interna adressintervall | Hitta ip-adress: https://www.bing.com/search?q=what%27s+my+ip |
| Enhet i hello externa nätverket (kan vara en telefon med hello operatör mobila nätverk) |  |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Gå tooAzure AD Management Portal: bladet för villkorlig åtkomst | [Hanteringsportalen för Azure AD: Villkorlig åtkomst](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Skapa princip för villkorlig åtkomst:<br/>-PoC målgruppsanvändare under ”användare och grupper”<br/>-PoC målprogrammet under ”molnappar”<br/>-Mål förutom betrodda relationer ”villkor” -> ”platser” alla platser **Obs:** tillförlitliga IP-adresser har konfigurerats i [MFA-portalen](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Kräv Multi-Factor authentication under ”bidrag” | [Kom igång med villkorlig åtkomst i Azure Active Directory: principen konfigurationssteg](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Åtkomst till program från företagsnätverk | [Kom igång med villkorlig åtkomst i Azure Active Directory: testar hello policy](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Åtkomst till programmet från offentliga nätverk | [Kom igång med villkorlig åtkomst i Azure Active Directory: testar hello policy](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Överväganden

Om du använder federation, kan du använda hello lokalt identitetsprovider (IdP) toocommunicate hello innanför/utanför företagsnätverket tillstånd med anspråk. Du kan använda den här tekniken utan toomanage hello lista över IP-adresser som kan vara komplicerade tooassess och hantera i stora organisationer. Du behöver för hello ”centrala” scenariot (en användare som loggar från hello interna nätverket och när inloggade växlar platser, till exempel ett kafé) och kontrollera att du förstår följderna hello i som installationsprogrammet. Läs mer: [skydda molnresurser med Azure Multi-Factor Authentication och AD FS: tillförlitliga IP-adresser för externa användare](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Ungefärlig tid tooComplete: 15 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Identifiera hello global administratör som ska ingå i hello POC för PIM | [Börja använda Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| Identifiera hello global administratör som ska bli hello säkerhetsadministratör | [Börja använda Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Olika administrativa roller i Azure Active Directory-PIM](active-directory-privileged-identity-management-roles.md) |
| Valfritt: Kontrollera om hello globala administratörer har e-post åt tooexercise e-postaviseringar i PIM | [Vad är Azure AD Privileged Identity Management?: Konfigurera hello produktaktivering rollinställningar](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Inloggningen toohttps://portal.azure.com som en global administratör (GA) och bootstrap hello PIM-bladet. hello Global administratör som utför det här steget dirigeras som hello säkerhetsadministratör.  Vi ska anropa den här aktören GA1 | [Med hjälp av guiden för hello säkerhet i Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Identifiera hello global administratör och flytta dem från permanent tooeligible. Detta bör vara en separat administratör från hello som används i steg 1 för tydlighetens skull. Vi ska anropa den här aktören GA2 | [Azure AD Privileged Identity Management: Hur tooadd eller ta bort en användarroll](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Vad är Azure AD Privileged Identity Management?: Konfigurera hello produktaktivering rollinställningar](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Nu logga in som GA2 toohttps://portal.azure.com och försök att ändra ”inställningar”. Observera att vissa alternativ är nedtonade. | |
| I en ny flik hello samma session som steg 3, Gå nu toohttps://portal.azure.com och lägga till hello PIM bladet toohello instrumentpanel. | [Hur tooactivate eller inaktivera roller i Azure AD Privileged Identity Management: lägga till hello Privileged Identity Management-program](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Begära aktivering toohello Global administratör | [Hur tooactivate eller inaktivera roller i Azure AD Privileged Identity Management: aktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Observera att om GA2 aldrig registrerat för Multifaktorautentisering, registrering för Azure MFA blir det nödvändigt |  |
| Gå tillbaka toohello ursprungliga fliken i steg 3 och klicka hello uppdatera i webbläsaren hello. Observera att du nu har åtkomst toochange ”inställningar” | |
| Du kan också om dina globala administratörer har e-post som är aktiverad, kan du kontrollera GA1 och GA2's inkorg och se hello-meddelande hello rollen aktiveras |  |
| 8 Kontrollera hello granskningshistorik och se hello rapporten tooconfirm hello höjning av GA2 visas. | [Vad är Azure AD Privileged Identity Management?: rollen granskningsaktivitet](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Överväganden

Den här funktionen är en del av Azure AD Premium P2 och/eller EMS E5

## <a name="discovering-risk-events"></a>Identifiering av riskhändelser

Ungefärlig tid tooComplete: 20 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Enhet med Tor webbläsaren hämtas och installeras | [Hämta Tor webbläsare](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Åtkomst tooPOC toodo hello logga in | [Azure Active Directory-identitetsskydd playbook](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Öppna tor webbläsare | [Hämta Tor webbläsare](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Logga in toohttps://myapps.microsoft.com med hello POC användarkonto | [Azure Active Directory-identitetsskydd playbook: simulering av riskhändelser](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Vänta 5-7 minuter |  |
| Logga in som en global administratör toohttps://portal.azure.com och öppna hello Identity Protection-blad | https://aka.MS/aadipgetstarted |
| Öppna hello risk händelser bladet. Du bör se en post under ”inloggningar från anonyma IP-adresser”  | [Azure Active Directory-identitetsskydd playbook: simulering av riskhändelser](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Överväganden

Den här funktionen är en del av Azure AD Premium P2 och/eller EMS E5

## <a name="deploying-sign-in-risk-policies"></a>Distribuera inloggning risk principer

Ungefärlig tid tooComplete: 10 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Enhet med Tor webbläsaren hämtas och installeras | [Hämta Tor webbläsare](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Åtkomst som en POC toodo hello användarinloggning för testning |  |
| POC användare är registrerad med MFA. Se till att toouse en telefon med mottagning | Byggblock: [Azure Multi-Factor Authentication med telefonsamtal](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Logga in som en global administratör toohttps://portal.azure.com bladet och öppna hello Identity Protection | https://aka.MS/aadipgetstarted |
| Aktivera inloggning riskprincipen på följande sätt:<br/>-Tilldelad till: POC användare<br/>-Villkor: Logga in risk medelhög eller högre (logga in från anonyma plats anses som en medelhög risknivå)<br/>-Kontroller: Kräva MFA | [Azure Active Directory-identitetsskydd playbook: Logga in risk](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Öppna tor webbläsare | [Hämta Tor webbläsare](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Logga in toohttps://myapps.microsoft.com med hello PoC användarkonto |  |
| Meddelande hello MFA-kontrollen | [Logga in som inträffar med Azure AD Identity Protection: riskfyllda inloggning återställning](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Överväganden

Den här funktionen är en del av Azure AD Premium P2 och/eller EMS E5. Mer om riskhändelser finns toolearn: [riskhändelser för Azure Active Directory](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Konfigurera certifikatbaserad autentisering

Ungefärlig tid toocomplete: 20 minuter

### <a name="pre-requisites"></a>Förutsättningar

| Förhandskrav | Resurser |
| --- | --- |
| Enheten användarcertifikat etablerats (Windows, iOS eller Android) från Företags-PKI | [Distribuera användarcertifikat](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD-domän federerade med AD FS | [Azure AD Connect och federation](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Översikt över Active Directory Certificate Services](https://technet.microsoft.com/library/hh831740.aspx)|
| Har Microsoft Authenticator-appen för iOS-enheter | [Kom igång med hello Microsoft Authenticator-appen](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Steg

| Steg | Resurser |
| --- | --- |
| Aktivera ”autentisering med användarcertifikat” på AD FS | [Konfigurera autentiseringsprinciper: tooconfigure primär autentisering globalt i Windows Server 2012 R2](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Valfritt: Aktivera certifikatautentisering i Azure AD för Exchange Active Sync-klienter | [Komma igång med certifikatbaserad autentisering i Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Navigera tooAccess panelen och autentisera med hjälp av användarcertifikat | https://myapps.microsoft.com |

### <a name="considerations"></a>Överväganden

Mer om varningar av den här distributionen finns toolearn: [ADFS: autentisering med datorcertifikat med Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Vara bör låst innehas av användarcertifikat. Antingen genom att hantera enheter eller med PIN-kod vid smartkort.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
