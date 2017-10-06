---
title: "aaaArticle Index för programhantering i Azure Active Directory | Microsoft Azure"
description: "Lär dig hur toocustomize hello utgångsdatumet för federationscertifikat och hur toorenew certifikat som snart upphör att gälla."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: f8a584baa94dc50e279899074f50160978256559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Artikelindex för programhantering i Azure Active Directory
Den här sidan innehåller en omfattande lista över alla dokument som skrivits om hello olika program-relaterade funktioner i Azure Active Directory (AD Azure).

Det finns en kort introduktion tooeach större funktionsområde, samt information om vilka artiklar tooread beroende på vilken information du söker efter.

## <a name="overview-articles"></a>Översikt över artiklar
hello artiklarna nedan är bra startpunkter för dem som för en kort beskrivning av Azure AD funktioner för programhantering.

| Artikel Guide |  |
|:---:| --- |
| En introduktion toohello management programproblem som löser Azure AD |[Hantera program med Azure Active Directory (AD)](active-directory-enable-sso-scenario.md) |
| En översikt över hello olika funktioner i Azure AD-relaterade tooenabling enkel inloggning, definiera vem som har åtkomst till tooapps och hur användare starta appar |[Programåtkomst och enkel inloggning i Azure Active Directory](active-directory-appssoaccess-whatis.md) |
| En titt på hello olika steg med att integrera appar i din Azure AD |[Integrera Azure Active Directory med program](active-directory-integrating-applications-getting-started.md)<br /><br />[Aktivera enkel inloggning tooSaaS appar](active-directory-sso-integrate-saas-apps.md)<br /><br />[Hantera åtkomst tooApps](active-directory-managing-access-to-apps.md) |
| En förklaring av hur appar representeras i Azure AD |[Hur och varför program läggs tooAzure AD](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Felsökning av artiklar
Det här avsnittet ger snabb åtkomst toorelevant felsökning guider. Mer information om varje funktionsområde finns på hello resten av den här sidan.

| Funktionsområde |  |
|:---:| --- |
| Federerad enkel inloggning |[Felsökning av SAML-baserade enkel inloggning](active-directory-saml-debugging.md) |
| Lösenordsbaserade enkel inloggning |[Felsöka hello Access panelen-tillägg för Internet Explorer](active-directory-saas-ie-troubleshooting.md) |
| Programproxy |[Felsökningsguide för App-Proxy](active-directory-application-proxy-troubleshoot.md) |
| Enkel inloggning mellan lokala AD och Azure AD |[Felsöka Lösenordssynkronisering](connect/active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)<br /><br />[Felsöka tillbakaskrivning av lösenord](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Dynamiska gruppmedlemskap |[Felsökning av dynamiska gruppmedlemskap](active-directory-accessmanagement-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Enkel inloggning (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federerad enkel inloggning: Logga in på många appar med hjälp av en identitet
Enkel inloggning kan användare tooaccess olika appar och tjänster med hjälp av endast en uppsättning autentiseringsuppgifter. Federation är en metod som du kan aktivera enkel inloggning. När en användare försöker toosign till federerade appar, får de omdirigerade tootheir organisation officiella inloggningssidan återges av Azure Active Directory, och är då omdirigerade tillbaka toohello app vid autentiseringen.

| Artikel Guide |  |
|:---:| --- |
| En introduktion toofederation och andra typer av inloggning |[Enkel inloggning med Azure AD](active-directory-appssoaccess-whatis.md) |
| SaaS-appar som redan är integrerade med Azure AD med tusentals förenklad enkel inloggning konfigurationssteg |[Komma igång med hello Azure AD application gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Fullständig lista över förintegrerade appar som har stöd för Federation](http://aka.ms/aadfederatedapps)<br /><br />[Hur tooAdd din App toohello Azure AD App-galleriet](active-directory-app-gallery-listing.md) |
| Fler än 150 app självstudier om hur tooconfigure enkel inloggning för appar som [Salesforce](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Workday](active-directory-saas-workday-tutorial.md), och mycket mer |[Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md) |
| Hur toomanually konfigurera och anpassa din konfiguration för enkel inloggning |[Hur tooConfigure tooApps federerad enkel inloggning som inte är i hello Azure Active Directory-Programgalleriet](active-directory-saas-custom-apps.md)<br /><br />[Hur tooCustomize anspråk som utfärdats i hello SAML-Token för Pre-Integrated appar](active-directory-saml-claims-customization.md) |
| Felsökningsguide för federerade appar som använder hello SAML-protokoll |[Felsökning av SAML-baserade enkel inloggning](active-directory-saml-debugging.md) |
| Hur tooconfigure appens certifikatets förfallodatum och hur toorenew certifikat |[Hantera certifikat för federerad enkel inloggning i Azure Active Directory](active-directory-sso-certs.md) |

Federerad enkel inloggning är tillgänglig för alla utgåvor av Azure AD för in tooten appar per användare. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) har stöd för obegränsade program. Om din organisation har [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) eller [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), sedan kan du [Använd grupper tooassign åtkomst toofederated program](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Lösenordsbaserade enkel inloggning: kontot delning och enkel inloggning för icke-federerade appar
tooenable enkel inloggning tooapplications som inte stöder federation, Azure AD erbjudanden lösenord management-funktioner som kan lagras säkert lösenord tooSaaS appar och logga in användare automatiskt dessa appar. Du kan enkelt distribuera autentiseringsuppgifterna för nyligen skapade konton och dela team konton med flera personer. Användarna behöver inte nödvändigtvis tooknow hello autentiseringsuppgifter toohello konton som de får åtkomst till.

| Artikel Guide |  |
|:---:| --- |
| En introduktion toohow lösenordsbaserade SSO fungerar och en kortfattad teknisk översikt |[Lösenordsbaserade enkel inloggning med Azure AD](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| En sammanfattning av hello scenarier relaterade tooaccount delning och hur de här problemen lösas av Azure AD |[Dela konton med Azure AD](active-directory-sharing-accounts.md) |
| Ändra hello lösenord för vissa appar automatiskt med jämna mellanrum |[Automatisk förnyelse av lösenord (förhandsgranskning)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Distribution och felsökning av hello Internet Explorer version av hello Azure AD-lösenord management-tillägget för |[Hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md)<br /><br />[Felsöka hello Access panelen-tillägg för Internet Explorer](active-directory-saas-ie-troubleshooting.md) |

Lösenordsbaserade enkel inloggning är tillgänglig för alla utgåvor av Azure AD för in tooten appar per användare. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) har stöd för obegränsade program. Om din organisation har [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) eller [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), sedan kan du [Använd grupper tooassign åtkomst tooapplications](#managing-access-to-applications). Lösenord för automatisk förnyelse är en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funktion.

### <a name="app-proxy-single-sign-on-and-remote-access-tooon-premises-applications"></a>App-Proxy: Enkel inloggning och remote access tooon lokala program
Om du har program i ditt privata nätverk som behöver toobe användas av användare och enheter utanför hello nätverk, kan du använda Azure AD Application Proxy tooenable säker åtkomst toothose appar.

| Artikel Guide |  |
|:---:| --- |
| Översikt över Azure AD Application Proxy och hur det fungerar |[Säker fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md) |
| Självstudier om hur tooconfigure Application Proxy och hur toopublish din första app |[Hur tooSet upp Azure AD App-Proxy](active-directory-application-proxy-enable.md)<br /><br />[Hur tooSilently installera hello App Proxy Connector](active-directory-application-proxy-silent-installation.md)<br /><br />[Hur tooPublish program med App-Proxy](active-directory-application-proxy-publish.md)<br /><br />[Hur tooUse ditt eget domännamn](active-directory-application-proxy-custom-domains.md) |
| Hur tooenable enkel inloggning och villkorlig åtkomst för appar publiceras med App-Proxy |[Single-sign-on med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Villkorlig åtkomst och Application Proxy](active-directory-application-proxy-conditional-access.md) |
| Anvisningar om hur toouse programproxy för hello följande scenarier |[Hur tooSupport Native Client-program](active-directory-application-proxy-native-client.md)<br /><br />[Hur tooSupport anspråksmedvetna program](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Hur tooSupport program som publicerats i separata nätverk och platser](active-directory-application-proxy-connectors.md) |
| Felsökningsguide för Application Proxy |[Felsökningsguide för App-Proxy](active-directory-application-proxy-troubleshoot.md) |

Application Proxy är tillgänglig för alla utgåvor av Azure AD för in tooten appar per användare. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) har stöd för obegränsade program. Om din organisation har [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) eller [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), sedan kan du [Använd grupper tooassign åtkomst tooapplications](#managing-access-to-applications).

Du kan också vara intresserad av [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), vilket gör att du toomigrate dina lokala program tooAzure när fortfarande uppfyller hello identitet måste dessa program.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Aktivera enkel inloggning mellan Azure AD och lokala AD
Om organisationen upprätthåller en Windows Server Active Directory lokalt tillsammans med Azure Active Directory i hello molnet, kommer du förmodligen vill tooenable enkel inloggning mellan dessa två system. Azure AD Connect (hello verktyg som tillsammans integrerar dessa två system) innehåller flera alternativ för att konfigurera enkel inloggning: upprätta federation med AD FS eller en annan provider för federation eller aktivera Lösenordssynkronisering.

| Artikel Guide |  |
|:---:| --- |
| En översikt över hello enkel inloggning i Azure AD Connect, samt information om hur du hanterar hybridmiljöer |[Användaren loggar in alternativ i Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Allmänna riktlinjer för att hantera miljöer med både lokala Active Directory och Azure Active Directory |[Överväganden vid utformning av Azure AD-Hybrid](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md) |
| Anvisningar om hur du använder Lösenordssynkronisering tooenable enkel inloggning |[Implementera Lösenordssynkronisering med Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Felsöka Lösenordssynkronisering](https://support.microsoft.com/en-us/kb/2855271) |
| Anvisningar om hur du använder tillbakaskrivning av lösenord tooenable SSO |[Komma igång med lösenordshantering i Azure AD](active-directory-passwords-getting-started.md)<br /><br />[Felsök tillbakaskrivning av lösenord](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Vägledning om hur du använder från tredje part identitet providers tooenable enkel inloggning |[Lista över kompatibla från tredje part identitet Providers som kan användas tooEnable enkel inloggning](https://aka.ms/ssoproviders) |
| Hur Windows 10-användare kan få hello fördelarna med enkel inloggning via Azure AD Join |[Utöka Molnfunktioner tooWindows 10 enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md) |

Azure AD Connect är tillgängliga för [alla utgåvor av Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD Självbetjäning för återställning av lösenord är tillgängligt för [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) och [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Lösenord tillbakaskrivning tooon lokal AD är en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funktion.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Villkorlig åtkomst: Tillämpa ytterligare säkerhetskrav för hög risk appar
När du ställer in enkel inloggning tooyour appar och resurser kan du sedan ytterligare skydda känsliga program genom att tillämpa specifika säkerhetskrav på varje inloggning toothat-app. Du kan exempelvis använda Azure AD toodemand att alla åtkomst tooa viss app alltid kräva multifaktorautentisering, oavsett om huruvida appen stöder funktionen innately. Ett annat vanligt exempel av villkorlig åtkomst är att användarna vara anslutna toohello organisation toorequire betrodda nätverk i ordning tooaccess ett särskilt känsliga program.

| Artikel Guide |  |
|:---:| --- |
| En introduktion toohello villkorlig åtkomstfunktioner som erbjuds i Azure AD och Office 365 och Intune |[Hantera risker med villkorlig åtkomst](active-directory-conditional-access.md) |
| Hur tooenable villkorlig åtkomst för hello följande typer av resurser |[Villkorlig åtkomst för SaaS-appar](active-directory-conditional-access-azuread-connected-apps.md)<br /><br />[Villkorlig åtkomst för Office 365-tjänster](active-directory-conditional-access-device-policies.md)<br /><br />[Villkorlig åtkomst för lokala program](active-directory-conditional-access.md)<br /><br />[Villkorlig åtkomst för lokala program som publicerats via Azure AD App-Proxy](active-directory-application-proxy-conditional-access.md) |

| Hur beställa tooregister enheter med Azure Active Directory på tooenable principer för enhetsbaserad villkorlig åtkomst | [Översikt över Azure Active Directory Device Registration](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Hur tooEnable automatisk Enhetsregistrering för domänanslutna Windowsenheter](active-directory-conditional-access-automatic-device-registration.md)<br />– [Steg för Windows 8.1-enheter](active-directory-conditional-access-automatic-device-registration-setup.md)<br />– [Steg för Windows 7-enheter](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Hur toouse hello Microsoft Authenticator-appen för tvåstegsverifiering | [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

Villkorlig åtkomst är en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funktion.

## <a name="apps--azure-ad"></a>Appar och Azure AD
### <a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud App Discovery: Hitta vilka SaaS-appar som används i din organisation
Cloud App Discovery hjälper IT-avdelningar information om vilka SaaS-appar används i hela hello organisation. Det kan Mät appanvändning och dess popularitet så att den kan fastställa vilka appar kommer att ha nytta hello mest från att vara online under IT-kontroll och integreras med Azure AD.

| Artikel Guide |  |
|:---:| --- |
| En allmän översikt över hur det fungerar |[Hitta ej sanktionerad molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md) |
| En mer grundlig genomgång i hur det fungerar med svar tooquestions på sekretess |[Säkerhet och överväganden för sekretess](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Vanliga frågor och svar |[Vanliga frågor och svar för Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| Självstudier för distribution av Cloud App Discovery |[Handboken för distribution](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[Distributionsguiden för System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[Installera på proxyservrar med anpassade portar](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| hello Ändringslogg för toohello Cloud App Discovery-agenten för programuppdateringar |[Ändringslogg](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

Cloud App Discovery är en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funktion.

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automatiskt etablera och ta bort etableringen av användarkonton i SaaS-appar
Automatisera hello skapande, underhållet och borttagningen av användaridentiteter i SaaS-program, till exempel Dropbox, Salesforce, ServiceNow med mera. Matchar och synkronisera befintliga identiteter mellan Azure AD och din SaaS-appar och kontrollera åtkomsten genom att automatiskt inaktivera konton när användarna lämnar organisationen hello.

| Artikel Guide |  |
|:---:| --- |
| Läs mer om hur det fungerar och få svar toocommon frågor |[Automatisera Användaretablering och avetablering tooSaaS appar](active-directory-saas-app-provisioning.md) |
| Konfigurera hur information mappas mellan Azure AD och SaaS-appar |[Anpassa attributmappning](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Hur tooenable Automatisk etablering tooany app som stöder hello SCIM protokoll |[Konfigurera automatisk Användaretablering tooany SCIM-Enabled App](active-directory-scim-provisioning.md) |
| Hur tooreport på och felsöka användaretablering |[Rapportering om automatisk användaretablering](active-directory-saas-provisioning-reporting.md)<br><br>[Etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)<br><br>[Felsökning av användaretablering](active-directory-application-provisioning-content-map.md) |
| Begränsa vem som hämtar etablerade tooan program baserat på deras attributvärden |[Omfångsfilter](active-directory-saas-scoping-filters.md) |

Automatisk användaretablering är tillgänglig för alla utgåvor av Azure AD för in tooten appar per användare. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) har stöd för obegränsade program. Om din organisation har [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) eller [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), sedan kan du [använda grupper toomanage vilka användare få etableras](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Skapa program som integreras med Azure AD
Om din organisation utvecklar eller underhålla line-of-business (LoB)-program, eller om du utvecklar en app med kunder som använder Azure Active Directory hello följande kurser hjälper dig att integrera dina program med Azure AD.

| Artikel Guide |  |
|:---:| --- |
| Riktlinjer för både IT-proffs och programutvecklare på integrera appar med Azure AD |[hello IT-proffs Guide för hur du utvecklar program för Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[hello Developer's Guide för Azure Active Directory](active-directory-developers-guide.md) |
| Hur tooapplication leverantörer kan lägga till sina appar toohello Azure AD App-galleriet |[Visar en lista över ditt program i hello Azure Active Directory-Programgalleriet](active-directory-app-gallery-listing.md) |
| Hur toomanage åt toodeveloped program med Azure Active Directory |[Hur tooEnable Användartilldelning för utvecklade program](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Tilldela användare tooyour App](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Tilldela gruppen tooyour App](active-directory-applications-guiding-developers-assigning-groups.md) |

Om du utvecklar konsumentinriktade program du vill utnyttja [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) så att du inte har toodevelop egna identitet system toomanage användarna. [Läs mer](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-tooapplications"></a>Hantera åtkomst tooApplications
### <a name="using-groups-and-self-service-toomanage-who-has-access-toowhich-apps"></a>Med hjälp av grupper och självbetjäning toomanage som har åtkomst toowhich appar
toohelp som du hanterar som ska ha åtkomst toowhich resurser, Azure Active Directory kan du tooset tilldelningar och behörigheter i stor skala med hjälp av grupper. IT kan välja tooenable självbetjäningsfunktioner så att användare kan bara begära tillstånd när det behövs.

| Artikel Guide |  |
|:---:| --- |
| En översikt över Azure AD access management-funktioner |[Introduktion tooManaging åtkomst tooApps](active-directory-managing-access-to-apps.md)<br /><br />[Hur Access Management arbetar i Azure AD](active-directory-manage-groups.md)<br /><br />[Hur tooUse grupper tooManage åtkomst tooSaaS program](active-directory-accessmanagement-group-saasapps.md) |
| Aktivera tjänsten hantering av appar och grupper |[Självbetjäning programhantering](active-directory-self-service-application-access.md)<br /><br />[Self-Service Group Management](active-directory-accessmanagement-self-service-group-management.md) |
| Instruktioner för att konfigurera dina grupper i Azure AD |[Hur tooCreate säkerhetsgrupper](active-directory-accessmanagement-manage-groups.md)<br /><br />[Hur tooDesignate ägare för en grupp](active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Hur tooUse hello ”alla användare” gruppera](active-directory-accessmanagement-dedicated-groups.md) |
| Använd dynamiska grupper tooautomatically fylla i gruppmedlemskap med attributbaserad medlemskapsregler |[Dynamiska gruppmedlemskap: Avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md)<br /><br />[Felsökning av dynamiska gruppmedlemskap](active-directory-accessmanagement-troubleshooting.md) |

Hantering av grupp-baserade program är tillgängligt för [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) och [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Grupphantering via självbetjäning, självbetjäning programhantering och dynamiska grupper är [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funktioner.

### <a name="b2b-collaboration-enable-partner-access-tooapplications"></a>B2B-samarbete: Aktivera partner åtkomst tooapplications
Om ditt företag samarbetar med andra företag, är det troligt att du behöver toomanage partner åtkomst tooyour företagets program. Azure Active Directory B2B-samarbete ger ett enkelt och säkert sätt tooshare dina appar med partners.

| Artikel Guide |  |
|:---:| --- |
| En översikt över Azure AD för olika funktioner som kan hjälper dig att hantera externa användare som partners, kunder, osv. |[Jämförelse av funktioner för att hantera externa identiteter i Azure AD](active-directory-b2b-compare-external-identities.md) |
| En introduktion tooB2B samarbete och hur tooget igång |[Enkel, säker, molnet Partner integrering med Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure Active Directory B2B-samarbete](active-directory-b2b-collaboration-overview.md) |
| En mer grundlig genomgång i Azure AD B2B-samarbete och hur toouse den |[B2B-samarbete: Så här fungerar det](active-directory-b2b-how-it-works.md)<br /><br />[Aktuella begränsningar i Azure AD B2B-samarbete](active-directory-b2b-current-limitations.md)<br /><br />[Detaljerad genomgång av Azure AD B2B-samarbete](active-directory-b2b-detailed-walkthrough.md) |
| För referensartiklar med teknisk information om hur fungerar Azure AD B2B-samarbete |[CSV-filformat för att lägga till Partneranvändarna](active-directory-b2b-references-csv-file-format.md)<br /><br />[Användarattribut som påverkas av Azure AD B2B-samarbete](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Tokenformat för användare för Partner användare](active-directory-b2b-references-external-user-token-format.md) |

B2B-samarbete är tillgänglig för [alla utgåvor av Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Åtkomstpanelen: En portal för att komma åt appar och självbetjäningsfunktioner
hello Azure AD-åtkomstpanelen är där slutanvändarna kan starta sina appar och åtkomst hello självbetjäning funktioner som gör dem toomanage sina appar och gruppmedlemskap. Dessutom ingår toohello åtkomstpanelen, andra alternativ för att komma åt SSO-aktiverade appar i hello listan nedan.

| Artikel Guide |  |
|:---:| --- |
| En jämförelse av hello alternativ som är tillgängliga för distribution av appar för enkel inloggning toousers |[Distribuera Azure AD-integrerade program tooUsers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| En översikt över hello åtkomstpanelen och dess mobila motsvarande MyApps |[Introduktion tooAccess panelen och MyApps](active-directory-saas-access-panel-introduction.md)<br />– [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Hur tooaccess Azure AD-appar från hello Office 365-webbplatsen |[Med hjälp av hello Office 365 App programstart](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Hur tooaccess Azure AD-appar från hello mobila Intune Managed Browser-appen |[Intune Managed Browser](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />– [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Hur tooaccess Azure AD-appar som använder djuplänkar tooinitiate enkel inloggning |[Hämtar inloggning Direktlänkar tooYour appar](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Åtkomstpanelen är tillgänglig för [alla utgåvor av Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-tooapps"></a>Rapporter: Enkelt granska ändringar i appen och övervaka inloggningar tooapps
Azure Active Directory innehåller flera rapporter och aviseringar toohelp du övervaka din organisation åtkomst tooapplications. Du kan ta emot aviseringar om avvikande inloggningar tooyour appar och du kan följa när och varför en användare åtkomst tooan programmet har ändrats.

| Artikel Guide |  |
|:---:| --- |
| En översikt över hello rapportfunktionerna i Azure Active Directory |[Komma igång med Azure AD-rapportering](active-directory-reporting-getting-started.md) |
| Hur toomonitor hello inloggningar och app-användning av dina användare |[Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md) |
| Spåra ändringar som gjorts toowho kan komma åt ett visst program |[Azure Active Directory-Granskningsrapporthändelser](active-directory-reporting-audit-events.md) |
| Exportera hello data med hjälp av rapporter tooyour önskade verktyg hello Reporting API |[Komma igång med hello Azure AD Reporting API](active-directory-reporting-api-getting-started.md) |

vilka rapporter inkluderas i olika versioner av Azure Active Directory, toosee [Klicka här](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Se även
[Vad är Azure Active Directory?](active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/)

[Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/)
