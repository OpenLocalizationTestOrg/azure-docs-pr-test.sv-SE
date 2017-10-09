---
title: "aaaWhat är programåtkomst och enkel inloggning med Azure Active Directory? | Microsoft Docs"
description: "Använd Azure Active Directory tooenable enkel inloggning tooall hello SaaS och webb-program som du behöver för företag."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 75d1a3fd-b3c5-4495-a5c8-c4c24145ff00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curyand
ms.openlocfilehash: 429522cbd570ab27359c4630c5a6d7b25b692ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Vad är programåtkomst och enkel inloggning med Azure Active Directory?
Enkel inloggning innebär att kan tooaccess alla hello program och resurser som du behöver toodo företag genom att logga in en gång med ett enda användarkonto. När du är inloggad du kan komma åt alla hello-program som du behöver, utan att nödvändiga tooauthenticate (t.ex. Ange ett lösenord) en andra gång.

Många organisationer förlita sig på programvara som en tjänst (SaaS)-program, till exempel Office 365, rutan och Salesforce för slutanvändarens produktivitet. Tidigare IT-personal måste tooindividually skapa och uppdatera användarkonton i varje SaaS-program och att användarna har tooremember ett lösenord för varje SaaS-program.

Azure Active Directory utökar lokala Active Directory i hello moln att aktivera användare toouse sina primära organisationskonto toonot bara logga in tootheir domänanslutna enheter och företagets resurser, men även alla hello webb- och SaaS-program behövs för sitt jobb.

Därför inte bara användare har inte toomanage flera uppsättningar av användarnamn och lösenord, sina program åtkomst kan etableras eller tas bort automatiskt baserat på deras organisationsmedlemmar och deras status som en medarbetare. Azure Active Directory introducerar säkerhet och styrning åtkomstkontroller som gör att du toocentrally hantera användarnas åtkomst i SaaS-program.

Azure AD gör det möjligt för enkel integrering toomany för dagens populära SaaS-program. Det tillhandahåller identitets- och åtkomsthantering, kan användare toosingle inloggning tooapplications direkt, eller identifiera och starta dem från en portal, till exempel Office 365 eller hello Azure AD-åtkomstpanelen.

hello-arkitekturen för hello integration består av följande fyra huvudsakliga byggblock hello:

* Enkel inloggning kan användare tooaccess sina SaaS-program baserat på deras organisationens konto i Azure AD. Enkel inloggning är vad gör det möjligt för användare tooauthenticate tooan program med hjälp av enkel organisationskontot.
* Användaretablering kan användaretablering och avetablering på målet SaaS baserat på ändringar i Windows Server Active Directory och/eller Azure AD. Ett etablerade konto är vad gör att en användare behörighet toobe toouse ett program när de har autentiserats via enkel inloggning.
* Centraliserad hantering av programåtkomst via i hello Azure-hanteringsportalen kan enskild punkt SaaS programåtkomst och hantering med hello möjlighet toodelegate programmet åtkomst beslut att skapa och godkännanden tooanyone i hello organisation
* Enhetlig rapportering och övervakning av användaraktivitet i Azure AD

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Hur fungerar enkel inloggning med Azure Active Directory?
När en användare ”loggar in” tooan program går via en autentiseringsprocess där de är obligatoriska tooprove som de är de som de säger att de är. Utan enkel inloggning, detta görs vanligtvis genom att ange ett lösenord som lagras på hello program och hello användaren är obligatoriska tooknow lösenordet.

Azure AD stöder tre olika sätt toosign i tooapplications:

* **Federerad enkel inloggning** gör att program tooredirect tooAzure AD för autentisering av användare i stället för att fråga efter eget lösenord. Detta stöds för program som stöd för protokoll, till exempel SAML 2.0, WS-Federation eller OpenID Connect och hello bra läget för enkel inloggning.
* **Lösenordsbaserade enkel inloggning** möjliggör säker lagring av lösenord för program- och replay-med hjälp av en webbläsare webbtillägg eller mobilapp. Detta utnyttjar hello befintliga inloggningsprocessen tillhandahålls av programmet hello, men kan en administratör toomanage hello lösenord och kräver inte hello tooknow hello lösenord.
* **Befintliga enkel inloggning** aktiverar Azure AD tooleverage eventuella befintliga enkel inloggning som har ställts in för hello program, men kan dessa program toobe länkade toohello Office 365 eller Azure AD åtkomst panelen portaler och kan också ytterligare rapportering i Azure AD när hello program det startas.

När en användare har autentiserats med ett program, måste de också toohave en kontoposten tillhandahållas på hello-program som visar hello programmet där det behörigheter och åtkomstnivå är i hello-program. hello etableringen av den här kontoposten kan antingen ske automatiskt eller det kan ske manuellt av en administratör innan hello användaren får åtkomst för enkel inloggning.

 Mer information om dessa lägen för enkel inloggning och etablerar nedan.

### <a name="federated-single-sign-on"></a>Federerad enkel inloggning
Federerad enkel inloggning kan inloggning kan hello användare i din organisation toobe loggas automatiskt in tooa från tredje part SaaS-program med Azure AD med hjälp av hello-kontoinformation från Azure AD.

I det här scenariot eliminerar federation hello behovet av att en användare toobe autentiseras igen när du redan har loggats i Azure AD och du vill tooaccess resurser som styrs av en tredje parts SaaS-program.

Azure AD har stöd för federerad enkel inloggning med program som stöder hello SAML 2.0 WS-Federation, eller OpenID connect protokoll.

Se även: [hantera certifikat för federerad enkel inloggning](active-directory-sso-certs.md)

### <a name="password-based-single-sign-on"></a>Lösenordsbaserade enkel inloggning
Konfigurera lösenordsbaserade enkel inloggning kan hello användare i din organisation toobe loggas automatiskt in tooa från tredje part SaaS-program med Azure AD med hjälp av hello-kontoinformation från hello från tredje part SaaS-program. När du aktiverar den här funktionen samlar in Azure AD och hello-kontoinformation och hello relaterade lösenord lagras på ett säkert sätt.

Azure AD stöder lösenordsbaserad enkel inloggning på för molnbaserade appar som har en HTML-baserad inloggningssidan. Genom att använda ett anpassat webbläsare plugin-program kan AAD automatiserar processen via på ett säkert sätt hämta autentiseringsuppgifter, till exempel hello användarnamn och lösenord för hello från hello directory hello användares inloggning och anger autentiseringsuppgifterna till inloggningssidan för hello program hello användarens vägnar. Det finns två användningsområden:

1. **Administratören hanterar autentiseringsuppgifter** – administratörer kan skapa och hantera autentiseringsuppgifter och tilldela dessa autentiseringsuppgifter toousers eller grupper som behöver åtkomst till toohello programmet. I dessa fall hello slutanvändaren behöver inte tooknow hello autentiseringsuppgifter, men fortfarande får åtkomst för enkel inloggning toohello program genom att klicka på den i deras åtkomstpanelen eller via en angiven länk. På så sätt kan både livscykelhantering hello autentiseringsuppgifter av Hej administratör samt bekvämlighet för slutanvändare där de inte behöver tooremember eller hantera app-specifik lösenord. hello autentiseringsuppgifter som har dolts från hello slutanvändaren under hello automatisk inloggning process. men de är tekniskt sett kan upptäckas av hello användare med hjälp av web-felsökning verktyg och användare och administratörer bör följa presenteras hello samma säkerhetsprinciper som om hello autentiseringsuppgifter direkt av hello användare. Autentiseringsuppgifter för administratören är mycket användbara när ge kontot tillgång som delas av många användare, till exempel sociala medier eller dokumentdelning program.
2. **Användaren hanterar autentiseringsuppgifter** – administratörer kan tilldela program tooend användare eller grupper och tillåta hello slutet användare tooenter sina egna autentiseringsuppgifter direkt vid åtkomst till hello program för hello första gången i sina åtkomstpanelen. Detta skapar i syfte att underlätta för slutanvändare där de inte behöver toocontinually ange hello app-specifik lösenord varje gång de vill använda hello program. Den här användningsfall kan också användas som en version bricka tooadministrative hantering av hello autentiseringsuppgifter, där Hej administratör kan ställa in nya autentiseringsuppgifter för programmet hello senare utan att ändra hello app åtkomst upplevelse av hello slutanvändaren.

I båda fallen autentiseringsuppgifter lagras i ett krypterat tillstånd i hello directory och skickas endast över HTTPS under hello automatisk inloggning. Azure AD erbjuder med lösenordsbaserade enkel inloggning på en smidig åtkomstlösning för Identitetshantering för appar som inte kan stödja federation protokoll.

Lösenordsbaserade SSO förlitar sig på en webbläsare tillägget toosecurely hämta hello program- och särskild information från Azure AD och tillämpa det toohello service. De flesta SaaS tredjepartsprogram som stöds av Azure AD stöder denna funktion.

För lösenordsbaserad SSO kan hello användarens webbläsare vara:

* Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare (Se även [IE tillägget Deployment Guide](active-directory-saas-ie-group-policy.md))
* Chrome--På Windows 7 eller senare, och i MacOS X eller senare
* Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare

**Obs:** hello lösenordsbaserade SSO-tillägget ska vara tillgänglig för kvalificerade i Windows 10 när webbläsartillägg blir stöd för kant.

### <a name="existing-single-sign-on"></a>Befintliga enkel inloggning
När du konfigurerar enkel inloggning för ett program innehåller ett tredje alternativ för ”befintliga enkel inloggning” hello Azure-hanteringsportalen. Det här alternativet bara tillåter Hej administratör toocreate ett länk tooan program och placera den på hello åtkomstpanelen för valda användare.

Om det finns ett program som är konfigurerade tooauthenticate användare som använder Active Directory Federation Services 2.0, kan en administratör till exempel använda hello ”befintliga enkel inloggning” alternativet toocreate tooit en länk på hello åtkomstpanelen. När användare använder hello länken kan autentiseras med hjälp av Active Directory Federation Services 2.0 eller den befintliga enkla inloggning lösningen tillhandahålls av programmet hello.

### <a name="user-provisioning"></a>Användaretablering
Azure AD gör automatisk användaretablering och avetablering för konton i tredje parts SaaS-program från inom hello Azure-hanteringsportalen, med information om din Windows Server Active Directory eller Azure AD identitet för utvalda program. När en användare ges behörighet i Azure AD för något av dessa program, kan ett konto skapas automatiskt (etablerade) i hello mål SaaS-program.

När en användare tas bort eller information om deras ändringar i Azure AD, återspeglas ändringarna i hello SaaS-program. Detta innebär konfigurera automatisk identitet livscykelhantering gör att administratörer toocontrol och ger Automatisk etablering och avetablering från SaaS-program. Denna automatisering av Identitetshantering livscykel är aktiverat som användaretablering i Azure AD.

Det finns fler toolearn [automatisk Användaretablering och avetablering tooSaaS program](active-directory-saas-app-provisioning.md)

## <a name="get-started-with-hello-azure-ad-application-gallery"></a>Kom igång med hello Azure AD application gallery
Redo tooget igång? toodeploy enkel inloggning mellan Azure AD och SaaS-program som används i din organisation, Följ dessa riktlinjer.

### <a name="using-hello-azure-ad-application-gallery"></a>Använda hello Azure AD application gallery
Hej [Azure Active Directory-Programgalleriet](https://azure.microsoft.com/marketplace/active-directory/all/) innehåller en lista över program som är kända toosupport en form av enkel inloggning med Azure Active Directory.

![][1]

Här följer några tips för att hitta appar med vilka funktioner som de stöder:

* Azure AD stöder automatisk etablering och avetablering för alla ”aktuell” appar i hello [Azure Active Directory-Programgalleriet](https://azure.microsoft.com/marketplace/active-directory/all/).
* En lista över federerade program som uttryckligen har stöd för federerad enkel inloggning med ett protokoll, till exempel SAML WS-Federation, eller OpenID Connect finns [här](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

När du har hittat programmet, du kan komma igång med hello stegvisa anvisningar visas i hello app-galleriet och enkel inloggning i hello Azure management portal tooenable.

### <a name="application-not-in-hello-gallery"></a>Programmet inte i hello galleriet?
Om programmet inte hittas i hello Azure AD application gallery, finns följande alternativ:

* **Lägg till en ny app som du använder** -Använd hello anpassad kategori i hello app galleriet inom hello Azure management portal tooconnect ett olistade program som använder din organisation. Du kan lägga till alla program som stöder SAML 2.0 som en federerad app eller alla program som har en HTML-baserad-inloggningssida som lösenord SSO app. Mer information finns i den här artikeln på [lägga till egna program](active-directory-saas-custom-apps.md).
* **Lägg till dina egna app som du utvecklar** - om du har utvecklat programmet hello själv, följ hello riktlinjerna i hello Azure AD-utvecklare dokumentationen tooimplement federerad enkel inloggning eller etablering med hjälp av hello Azure AD graph API. Mer information finns i följande resurser:
  
  * [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Begär en appintegrering** -begär support för hello-program som du behöver använda hello [Azure AD-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-hello-azure-management-portal"></a>Med hjälp av hello Azure-hanteringsportalen
Du kan använda hello Active Directory-tillägget i hello Azure-hanteringsportalen tooconfigure hello program enkel inloggning. Som ett första steg behöver du tooselect en katalog från hello Active Directory-avsnittet i hello portal:

![][2]

toomanage från tredje part SaaS-program kan du växla till hello program fliken hello valda katalogen. Den här vyn kan administratörer:

* Lägga till nya program från hello Azure AD-galleriet som du utvecklar appar
* Ta bort integrerade program
* Hantera hello-program som de redan har integrerat

Vanliga administrativa uppgifter för ett SaaS-program från tredje part är:

* Aktivera enkel inloggning med Azure AD med lösenord SSO eller, om det finns för hello mål SaaS federerad enkel inloggning
* Du kan också aktivera användaretablering för användaren etablering och avetablering (livscykelhantering identitet)
* Att välja vilka användare som har åtkomst för program där användaretablering är aktiverat, toothat program

För galleriet appar som har stöd för federerad enkel inloggning, kräver konfigurationen normalt du tooprovide ytterligare konfigurationsinställningar som t.ex certifikat och metadata toocreate ett federerat förtroende mellan hello tredjeparts-appen och Azure AD. hello konfigurationsguiden vägleder dig genom hello information och ger dig med enkel åtkomst toohello specifika data för SaaS-program och anvisningar.

För galleriet appar som stöder automatisk användaretablering, kräver detta du toogive Azure AD behörigheter toomanage dina konton i hello SaaS-program. Som minimum måste tooprovide autentiseringsuppgifter för Azure AD ska användas vid autentisering via toohello målprogrammet. Om ytterligare konfigurationsinställningar måste toobe tillhandahålls beror på hello programmet hello krav.

## <a name="deploying-azure-ad-integrated-applications-toousers"></a>Distribuera Azure AD integrerade program toousers
Azure AD innehåller flera anpassningsbara sätt toodeploy program tooend-användare i organisationen:

* Azure AD-åtkomstpanelen
* Startprogrammet för Office 365
* Direkt inloggning toofederated appar
* Djup länkar toofederated, lösenordsbaserade eller befintliga appar

Vilken metod du väljer toodeploy i din organisation är så önskar.

### <a name="azure-ad-access-panel"></a>Azure AD-åtkomstpanelen
Hej åtkomstpanelen på https://myapps.microsoft.com är en webbaserad portal som tillåter en användare med ett organisationskonto i Azure Active Directory tooview och starta molnbaserade program toowhich de har beviljats åtkomst av hello Azure AD administratören. Om du är en slutanvändare med [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), du kan också använda självbetjäning hanteringsfunktioner via hello åtkomstpanelen.

![][3]

Hej åtkomstpanelen skiljer sig från hello Azure-hanteringsportalen och kräver inte användare toohave en Azure-prenumeration eller Office 365-prenumeration.

Mer information om hello Azure AD-åtkomstpanelen finns hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Startprogrammet för Office 365
Program som tilldelats toousers via Azure AD visas också i hello Office 365-portalen på https://portal.office.com/myapps för organisationer som har distribuerat Office 365. Detta gör det enkelt och för användare i en organisation toolaunch sina appar utan att behöva toouse en andra portal och rekommenderas hello appen startas lösning för organisationer som använder Office 365.

![][4]

Läs mer om hello Office 365 startprogrammet [har appen visas i hello Office 365 app starta](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-toofederated-apps"></a>Direkt inloggning toofederated appar
Mest federerade program som stöder SAML 2.0, WS-Federation eller OpenID Anslut även stöd hello möjligheten för användare toostart på hello program och sedan hämta loggat in via Azure AD genom automatisk omdirigering eller genom att klicka på en länk toosign i. Detta kallas tjänstleverantör-initieras inloggning och mest federerade program i hello Azure AD application gallery stöder (se hello-dokumentationen som länkas från hello app enkel inloggning konfigurationsguiden i hello Azure-hanteringsportalen för information).

![][5]

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Inloggning direktlänkar för federerade, lösenordsbaserade eller befintliga appar
Azure AD stöder också enkel inloggning Direktlänkar tooindividual program som stöder lösenordsbaserad enkel inloggning, befintlig enkel inloggning och någon form av federerad enkel inloggning.

Dessa länkar är särskilt utformad URL: er som skickar en användare via hello Azure AD-inloggning i processen för ett visst program utan hello användare starta dem från hello Azure AD-åtkomstpanelen eller Office 365. Enkel inloggning webbadresserna finns under fliken instrumentpanelen i hello av redan integrerade program i hello Active Directory-delen av hello Azure-hanteringsportalen enligt hello skärmbilden nedan.

![][6]

Dessa länkar kan kopieras och klistras in var du vill tooprovide en inloggning länken toohello valt program. Detta kan bero på ett e-postmeddelande eller i alla anpassade webbaserad portal som du har skapat för programåtkomst för användaren. Här är ett exempel på en Azure AD direkt enkel inloggnings-URL för Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Liknande tooorganization-specifika URL: er för hello åtkomstpanelen, du kan anpassa den här URL: en ytterligare genom att lägga till någon av hello active eller verifierade domäner för din katalog hello myapps.microsoft.com domän. Detta säkerställer att alla organisationens företagsanpassning har lästs in omedelbart på inloggningssidan för hello utan hello användare behöver tooenter sitt användar-ID först:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

När en behörig användare klickar på en av länkarna programspecifika data, de först finns i deras organisationens inloggningssida (förutsatt att de inte är redan inloggad) och när inloggningen är omdirigerade tootheir app utan att först stoppa på hello åtkomstpanelen. Om hello användaren saknar förutsättningar tooaccess hello program, till exempel hello webbläsartillägget för lösenordsbaserad enkel inloggning, uppmanar hello användaren tooinstall hello saknas tillägget hello länk. hello länk-URL förblir konstant om hello enkel inloggning för programmet hello konfigurationsändringar.

Dessa länkar använder hello samma kontrollmekanismer som hello åtkomst till Kontrollpanelen och Office 365 och endast dessa användare eller grupper som har tilldelats toohello programmet hello Azure-hanteringsportalen ska kunna autentisera toosuccessfully. Alla användare som inte är auktoriserad visas ett meddelande om att de inte har beviljats åtkomst och ges en länk tooload hello åtkomst panelen tooview tillgängliga program som de har åtkomst.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Hitta ej sanktionerad molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)
* [Introduktion tooManaging åtkomst tooApps](active-directory-managing-access-to-apps.md)
* [Jämförelse av funktioner för att hantera externa identiteter i Azure AD](active-directory-b2b-compare-external-identities.md)

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png
