---
title: aaaManaging program med Azure Active Directory | Microsoft Docs
description: "Den här artikeln hello fördelarna med att integrera Azure Active Directory med din lokala, moln och SaaS-program."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 95b96f10-2d5c-4b78-8af8-d3657a24140f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 0016f8b433e101d8a150bc6d9be3931851578241
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-applications-with-azure-active-directory"></a>Hantera program med Azure Active Directory
Företag har två grundläggande krav för alla program utöver hello faktiska arbetsflödet eller innehåll:

1. tooincrease produktivitet program ska vara enkelt toodiscover och åtkomst
2. tooenable säkerhets- och styrning hello organisation behöver kontroll och tillsyn på vem som kan och faktiskt har åtkomst till varje program

I hello world av molnprogram detta uppnås bäst med identiteten toocontrol ”*som tillåts toodo vad*”.

Inom datorsammanhang terminologi:

* *Som* kallas *identitet* -hello hantering av användare och grupper
* *Vad* kallas *åtkomsthantering* – hello hantering av åtkomst tooprotected resurser

Båda komponenterna tillsammans kallas *identitet och åtkomst Management (IAM)*, som definieras av hello [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) som ”*hello säkerhet ämne som möjliggör hello höger enskilda användare tooaccess hello rätt resurser på hello Högerklicka gånger hello rätt skäl*”.

Vad är problemet hello okej? Om IAM *inte hanteras* på ett ställe med en integrerad lösning:

* Identity-administratörer har tooindividually skapa och uppdatera användarkonton i alla program separat, en redundant och tidskrävande aktivitet.
* Användare har toomemorize tooaccess hello av flera referenser-program som de behöver toowork med. Därför tenderar toowrite ned sina lösenord användare eller använda andra lösningar för hantering av lösenord som innehåller andra data säkerhetsrisker.
* Redundant tidskrävande aktiviteter minska hello tid användare och administratörer som arbetar på affärsverksamhet som ökar din verksamhet slutresultatet.

Så vad förhindrar normalt organisationer från att anta integrerad IAM-lösningar?

* De flesta tekniska lösningar baseras på programvaruplattformar som behöver toobe distribueras och av varje organisation för sina egna program.
* Molnprogram antas ofta till en högre nivå än IT-organisation kan integrera med befintliga IAM.
* Säkerhet och övervaka verktygsuppsättning kräver ytterligare anpassning och integrering tooachieve omfattande E2E scenarier.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory-integrerad med program
Azure Active Directory är Microsofts omfattande identitet som en tjänst (IDaaS) som:

* Aktiverar IAM som en tjänst i molnet 
* Tillhandahåller centrala hantering, enkel inloggning (SSO) och rapportering 
* Stöder integrerad åtkomsthantering för [tusentals program](https://azure.microsoft.com/marketplace/active-directory/) i hello programgalleriet, inklusive Salesforce, Google Apps, rutan, Concur och mycket mer. 

Med Azure Active Directory, alla program som du publicerar för partner och kunder (business eller konsumenten) har hello samma funktioner för hantering av identiteter och åtkomst.<br> Det här kan du toosignificantly minska din driftskostnader.

Vad händer om du behöver tooimplement ett program som ännu inte finns i hello programgalleriet? Här är lite mer tid än att konfigurera enkel inloggning för program från hello programgalleriet ger Azure AD dig en guide som hjälper dig med hello konfiguration.

hello-värdet för Azure AD är mer omfattande än ”bara” molnprogram. Du kan också använda den med lokala program genom att tillhandahålla säker fjärråtkomst. Du kan undvika hello hello behovet av VPN-anslutningar eller andra traditionella fjärråtkomst management-implementeringar med säker fjärråtkomst.

Genom att tillhandahålla central hantering och enkel inloggning (SSO) för alla program, Azure AD innehåller hello lösning toohello huvuddata problem med säkerhet och produktivitet.

* Användare kan komma åt flera program med en inloggning och ger mer tid tooincome genererar eller affärsenhet operations aktiviteter.
* Identity-administratörer kan hantera åtkomst tooapplications på ett ställe.

hello-förmån för hello användare och för ditt företag är uppenbara. Låt oss ta en närmare titt på hello fördelar för identitetsadministratör och hello organisation.

## <a name="integrated-application-benefits"></a>Fördelar med integrerat program
hello SSO-processen har två steg:

* Autentisering, hello processen att validera hello användarens identitet.
* Auktorisering, hello beslut tooenable eller blockera åtkomst tooa resurs med en åtkomstprincip.

När du använder Azure AD toomanage program och aktivera enkel inloggning:

* Autentisering görs på hello användarens lokala (t.ex. AD) eller Azure AD-kontot.
* Auktorisering kör på hello Azure AD tilldelning och skydd princip för att säkerställa konsekvent slutanvändarupplevelse och gör att du tooadd tilldelning, platser och MFA villkoren på alla program, oavsett dess interna funktioner.

Den viktiga toounderstand som hello sätt hello auktorisering trätt i kraft på hello målprogrammet varierar beroende på hur programmet hello har integrerat med Azure AD.

* **Redan integrerade program av tjänstleverantören** som Office 365 och Azure, de är program som skapats direkt på Azure AD och förlita dig på den för sina omfattande identitets- och hanteringsfunktioner. Åtkomst toothese program aktiveras via directory utfärdande av information och token.
* **Redan integrerade program från Microsoft och anpassade program** dessa är oberoende molnprogram som förlitar sig på ett internt programkatalogen och kan användas oberoende av Azure AD. Åtkomst toothese program aktiveras genom att utfärda ett program autentiseringsuppgifter som har mappats tooan programmet konto. Beroende på hello programfunktioner kan hello autentiseringsuppgifter vara en federation token eller användarnamn och lösenord för ett konto som tidigare var etablerad på hello program.
* **Lokala program** program som publicerats via hello Azure AD-programproxy främst att aktivera åtkomst tooon lokala program. Dessa program är beroende av en central lokal katalog som Windows Server Active Directory. Åtkomst toothese program aktiveras genom att utlösa hello proxy toodeliver hello programmet innehåll toohello slutanvändaren när respektera hello lokal inloggning krav.

Till exempel om en användare ansluter till din organisation, behöver du toocreate ett konto för hello användare i Azure AD för hello primära inloggning operationer. Om den här användaren kräver åtkomst tooa hanterade program, till exempel Salesforce kan du även behöver toocreate ett konto för den här användaren i Salesforce och länka det toohello Azure-konto toomake SSO arbete. När hello användare lämnar organisationen, är det lämpligt toodelete hello Azure AD-kontot och alla motsvarighet konton i hello IAM lagrar hello program hello användare har åtkomst till.

## <a name="access-detection"></a>Åtkomst-identifiering
IT-avdelningar är ofta inte medvetna om hello molnprogram som används i moderna företag. Tillsammans med Cloud App Discovery ger Azure AD dig en lösning toodetect dessa program.

## <a name="account-management"></a>Kontohantering
Traditionellt hantera konton i hello olika program är en manuell process som utförs av IT eller stöd för personal i hello organisation. Azure AD automatiserad helt kontohantering över alla service provider integrerade program och de program som redan integrerade Microsoft stöder automatisk användaretablering eller SAML JIT.

## <a name="automated-user-provisioning"></a>Automatisk användaretablering
Vissa program ange automation-gränssnitt för att skapa och ta bort (eller avaktivering) av konton. Om en provider erbjuder sådant gränssnitt kan utnyttjas den av Azure AD. Detta din driftskostnaderna minskar eftersom administrativa uppgifter sker automatiskt och förbättrar hello säkerheten för din miljö, eftersom det minskar hello risken för obehörig åtkomst.

## <a name="access-management"></a>Åtkomsthantering
Du kan använda Azure AD för att hantera tooapplications med enskilda eller regel drivs tilldelningar. Du kan också delegera åtkomst management toohello rätt personer i hello organisation att se till att hello bästa tillsyn och minska hello belastningen på supportavdelningen.

## <a name="on-premises-applications"></a>Lokala program
hello inbyggda programproxy kan du toopublish lokalt program tooyour användarna som resulterar i både konsekvent åtkomst till upplevelse med moderna molnet fördelar för programmet och hello från Azure AD-övervakning, rapportering och säkerhetsfunktioner .

## <a name="reporting-and-monitoring"></a>Övervakning och rapportering
Azure AD innehåller förintegrerade rapporterings- och övervakningsfunktionerna som gör att du tooknow vem som har åtkomst till tooapplications och när de faktiskt används dem.

## <a name="related-capabilities"></a>Relaterade funktioner
Du kan skydda dina program med detaljerade åtkomstprinciper och förintegrerade MFA med Azure AD. Mer om Azure MFA finns toolearn [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Komma igång
tooget igång integrera program med Azure AD ta en titt på hello [integrera Azure Active Directory med program komma igång](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Se även
[Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

