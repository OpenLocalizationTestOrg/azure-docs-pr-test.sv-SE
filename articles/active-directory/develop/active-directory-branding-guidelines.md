---
title: "aaaBranding riktlinjer för program | Microsoft Docs"
description: "En omfattande guide toodeveloper indatavärdena resurser för Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: e43f884c736a0dcb2e6e51293962ef1e2636ad70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="branding-guidelines-for-applications"></a>Riktlinjer för varumärkesanpassning för program
Det här avsnittet beskrivs hello företagsanpassning riktlinjer som du ska använda när du utvecklar program med Azure Active Directory (AD Azure). Dessa riktlinjer hjälper dig att dirigera dina kunder när de vill toouse sina arbets- eller skolkonto, hanteras i Azure AD- eller sitt personliga konto för registrering och inloggning tooyour program.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Personliga konton eller arbets- eller skolkonton från Microsoft
Microsoft hanterar två typer av användarkonton:

* **Personliga konton** (tidigare kallade Windows Live ID). Dessa konton representerar hello förhållandet mellan *enskilda* användare och Microsoft, och används för tooaccess konsumentenheter och tjänster från Microsoft. Dessa konton är avsedda för personligt bruk.
* **Arbets- eller skolkonto konton.** Dessa konton hanteras av Microsoft för organisationer som använder Azure Active Directory. Dessa konton är används toosign i tooOffice 365 och andra företagstjänster från Microsoft.

Microsoft arbets-eller skolkonton är vanligtvis tilldelade tooend användare (anställda, studenter, statligt anställda) av deras organisationer (företag, skola, myndighet). Dessa konton är antingen lärt dig direkt i hello moln i hello Azure AD-plattformen eller synkroniserade tooAzure AD från en lokal katalog, t.ex Windows Server Active Directory. Microsoft är hello *skyddar* av hello arbets- eller skolkonton, men hello konton ägs och styrs av hello organisation.

## <a name="referring-tooazure-ad-accounts-in-your-application"></a>Refererande tooAzure AD-konton i ditt program
Microsoft saknar en explicit slutanvändare toohello Azure eller hello Active Directory namn och ingen bör du.

* När användare loggar in, bör du använda hello organisationens namn och logotyp så mycket som möjligt. Det här är bättre än att använda allmänna villkor som ”din organisation”.
* När användare inte är inloggad bör du läsa tootheir konton som ”arbets- eller skolkonton” och Använd hello Microsoft logotypen tooconvey att dessa konton hanteras av Microsoft. Använd inte termer som ”enterprise-konto”, ”företag-konto” eller ”företagskontot” som skapar förvirring för användaren.

## <a name="user-account-pictogram"></a>Användarens konto grafik
I en tidigare version av dessa riktlinjer rekommenderar vi använder en ”blå Aktivitetsikon” grafik. Baserat på användare och utvecklare feedback nu rekommenderar vi hello användning av hello Microsoft-logotyp i stället. Detta hjälper användarna att förstå att de kan återanvända hello konto de använder med Office 365 eller andra Microsoft business services toosign i tooyour app.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Logga och logga in med Azure AD
Din app kan utgöra separata sökvägar för registrering och inloggning och hello följande avsnitt ger visual riktlinjer för båda scenarierna.

**Om din app har stöd för slutanvändare (t.ex. ledigt tootrial eller freemium model)-registrering**: du kan visa en **inloggning** knapp som gör att användare tooaccess appen med sitt arbetskonto eller ett personligt konto. Azure AD visas en fråga hello medgivande första gången de använder din app.

**Om din app krävs behörigheter som endast administratörer kan godkänna eller om din app kräver organisationens licensiering**: måste du avgränsa admin förvärv från användaren logga in. Hej **”hämta den här appen” knappen** kommer att omdirigera administratörer toosign i sedan be dem toogrant medgivande åt användare i organisationen. Detta har hello extra förmån för slutanvändare medgivande prompter tooyour app.

## <a name="visual-guidance-for-app-acquisition"></a>Visual vägledning för app
Länken ”Hämta hello app” omdirigera hello toohello Azure AD bevilja åtkomst (auktorisera) sida, tooallow en organisations administratör tooauthorize app-toohave dataåtkomst tootheir organisation som tillhandahålls av Microsoft. Information om hur toorequest åtkomst diskuteras i hello [integrera program med Azure Active Directory](active-directory-integrating-applications.md) artikel.

När administratörer godkänna tooyour app, kan de välja tooadd den tootheir Office 365 app programstart användarupplevelsen (tillgänglig från hello waffle och [https://portal.office.com/myapps](https://portal.office.com/myapps)). Om du vill tooadvertise den här funktionen kan använda du villkor som ”Lägg till den här appen tooyour organisationen” och visa en knapp så här:

![Programtyper och scenarier](./media/active-directory-branding-guidelines/add-to-my-org.png)

Vi rekommenderar dock att du skriver förklarande text i stället för knappar. Exempel:

> *Du kan bara bevilja < your_app_name > åtkomst tooyour organisations data om du redan använder Office 365 eller andra företagstjänst från Microsoft. Detta gör att dina användare tooaccess < your_app_name > med sina befintliga arbetskonton.*
> 
> 

## <a name="visual-guidance-for-sign-in"></a>Visual vägledning för inloggning
Din app ska innehålla tecknet i knapp som dirigerar om användare toohello inloggning slutpunkt som motsvarar toohello protokoll som du använder toointegrate med Azure AD. hello följande avsnitt innehåller information om hur knappen ska se ut.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Grafik och ”logga in med Microsoft”
Det är hello associering av hello Microsoft-logotyp och hello ”logga in med Microsoft” villkor som unikt representerar Azure AD bland andra identitetsleverantörer får stöd för din app. Om du inte har tillräckligt med utrymme för ”logga in med Microsoft”, är det ok tooshorten den för ”logga in”.

![Programtyper och scenarier](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Programtyper och scenarier](./media/active-directory-branding-guidelines/sign-in-light.png)

Du kan också använda ett mörkt färgschema hello knappar.

![Programtyper och scenarier](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Programtyper och scenarier](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Anpassning bra att veta
**GÖR** användning ”arbets- eller skolkonto konto” i kombination med hello ”logga in med Microsoft” knappen tooprovide ytterligare förklaring toohelp slutanvändare känner igen om de kan använda den. **Inte** använda andra villkor, till exempel ”enterprise-konto”, ”företag-konto” eller ”företagskonto”.

**Inte** använda ”Office 365-ID” eller ”Azure-ID”. Office 365 är också hello namn konsumenter erbjudande från Microsoft som inte använder Azure AD för autentisering.

**Inte** alter hello Microsoft-logotypen.

**Inte** exponera slutanvändare toohello Azure eller Active Directory varumärken. Det är ok men toouse dessa villkor med utvecklare, IT-proffs och administratörer.

## <a name="navigation-dos-and-donts"></a>Navigering ska och tänka på
**GÖR** ge användare toosign ut och växla tooanother användarkonto. Även om de flesta har ett enda personligt konto från Microsoft/Facebook/Google/Twitter, är personer ofta kopplade till mer än en organisation. Stöd för flera inloggade användare kommer snart.

