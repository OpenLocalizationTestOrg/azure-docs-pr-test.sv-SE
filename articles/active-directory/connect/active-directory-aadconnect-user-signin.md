---
title: "Azure AD Connect: Användaren logga in | Microsoft Docs"
description: "Azure AD Connect användaren logga in för anpassade inställningar."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 7848b419f3855b25cfa074a46779d258bd534bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect användaren inloggningsalternativ
Azure Active Directory (AD Azure) Connect kan dina användare toosign i tooboth molnet och lokala resurser med hjälp av hello samma lösenord. Den här artikeln beskriver viktiga begrepp för varje identitet modellen toohelp du välja hello-identitet som du vill toouse för att logga in tooAzure AD.

Om du redan är bekant med hello Azure AD identitetsmodellen och vill ha mer information om en viss metod toolearn finns hello länken:

* [Lösenordssynkronisering](#password-synchronization) med [enkel inloggning (SSO)](active-directory-aadconnect-sso.md)
* [Direkt-autentisering](active-directory-aadconnect-pass-through-authentication.md)
* [Federerad enkel inloggning (med Active Directory Federation Services (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)

## <a name="choosing-hello-user-sign-in-method-for-your-organization"></a>Välja hello användaren logga in metod för din organisation
För de flesta organisationer som vill bara tooenable användaren logga in tooOffice 365, SaaS-program och andra Azure AD-baserade resurser, rekommenderar vi hello standardalternativet lösenord synkronisering. Vissa organisationer har dock en viss orsak till att de inte kan toouse det här alternativet. De kan välja antingen en federerad inloggningsalternativ, till exempel AD FS eller direktautentisering. Du kan använda hello efter tabellen toohelp du göra hello rätt val.

Jag behöver för| PS med enkel inloggning| PA med enkel inloggning| AD FS |
 --- | --- | --- | --- |
Synkronisera nya användarkonton, kontakta och gruppkonton i lokala Active Directory toohello molnet automatiskt.|x|x|x|
Konfigurera min klient för hybridscenarion för Office 365.|x|x|x|
Aktivera Mina användare toosign i och åtkomst molntjänster med sina lokala lösenord.|x|x|x|
Implementera enkel inloggning med företagets autentiseringsuppgifter.|x|x|x|
Se till att inga lösenord lagras i hello molnet.||x *|x|
Aktivera lokal multifaktorautentisering lösningar.|||x|

* Via en enkel anslutning.

>[!NOTE]
> Direkt-autentisering har för närvarande vissa begränsningar med omfattande klienter. Se [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) för mer information.

### <a name="password-synchronization"></a>Lösenordssynkronisering
Hash-värden för användarlösenord synkroniseras med Lösenordssynkronisering från lokala Active Directory tooAzure AD. När lösenord har ändrats eller återställa lokalt, hello nya lösenord är synkroniserade tooAzure AD omedelbart så att användarna kan alltid använda hello samma lösenord för molnresurser och lokala resurser. hello lösenord aldrig skickas tooAzure AD eller lagras i Azure AD i klartext. Du kan använda Lösenordssynkronisering tillsammans med lösenord återskrivning tooenable Självbetjäning för återställning av lösenord i Azure AD.

Dessutom kan du aktivera [SSO](active-directory-aadconnect-sso.md) för användare på domänanslutna datorer på hello företagsnätverket. Med enkel inloggning, aktiverade användare bara behöver du tooenter en användarnamn toohelp dem säker åtkomst till molnresurser.

![Lösenordssynkronisering](./media/active-directory-aadconnect-user-signin/passwordhash.png)

Mer information finns i hello [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) artikel.

### <a name="pass-through-authentication"></a>Direkt-autentisering
Med direktautentisering, har hello användarens lösenord verifierats mot hello lokala Active Directory-domänkontrollant. hello lösenord behöver inte toobe finns i Azure AD i någon form. Detta ger lokala principer, till exempel inloggning timme begränsningar toobe utvärderas under toocloud autentiseringstjänster.

Direkt-autentisering använder en enkel agent på en Windows Server 2012 R2-domänansluten dator i hello lokala miljö. Den här agenten lyssnar efter begäranden för verifiering av lösenord. Det kräver inte någon ingående portar toobe öppna toohello Internet.

Du kan dessutom också aktivera enkel inloggning för användare på domänanslutna datorer på hello företagsnätverket. Med enkel inloggning, aktiverade användare bara behöver du tooenter en användarnamn toohelp dem säker åtkomst till molnresurser.
![Direkt-autentisering](./media/active-directory-aadconnect-user-signin/pta.png)

Mer information finns i:
- [Direkt-autentisering](active-directory-aadconnect-pass-through-authentication.md)
- [Enkel inloggning](active-directory-aadconnect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Federation som använder en ny eller befintlig servergrupp med AD FS i Windows Server 2012 R2
Med federerad inloggning logga dina användare in tooAzure AD-baserade tjänster med sina lokala lösenord. När de är på företagsnätverket hello har de även inte tooenter sina lösenord. Med alternativet hello federation med AD FS, kan du distribuera en ny eller befintlig servergrupp med AD FS i Windows Server 2012 R2. Om du väljer toospecify en befintlig servergrupp, konfigurerar Azure AD Connect hello förtroende mellan din grupp och Azure AD så att användarna kan logga in.

<center>![Federation med AD FS i Windows Server 2012 R2](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Distribuera federation med AD FS i Windows Server 2012 R2

Om du distribuerar en ny servergrupp, behöver du:

* En Windows Server 2012 R2-server för federationsservern hello.
* En Windows Server 2012 R2-server för hello Web Application Proxy.
* En PFX-fil med ett SSL-certifikat för din avsedda federationstjänstens namn. Till exempel: fs.contoso.com.

Om du distribuerar en ny servergrupp eller använder en befintlig servergrupp, behöver du:

* Lokal administratörsbehörighet på dina federationsservrar.
* Lokal administratörsbehörighet på alla arbetsgruppsservrar (inte ansluten till domänen) du avser toodeploy hello Web Application Proxy-rollen på.
* hello datorn att du kör guiden hello toobe kan tooconnect tooany andra datorer som du vill tooinstall AD FS eller Web Application Proxy på med hjälp av Windows Remote Management.

Mer information finns i [Konfigurera enkel inloggning med AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Logga in med hjälp av en tidigare version av AD FS eller en lösning från tredje part
Om du redan har konfigurerat moln-inloggning med hjälp av en tidigare version av AD FS (till exempel AD FS 2.0) eller en tredje parts federationsleverantör kan du välja tooskip användaren logga in konfigurationen med hjälp av Azure AD Connect. Detta gör att du tooget hello senaste synkroniseringen och andra funktioner i Azure AD Connect medan du fortfarande använder din befintliga lösning för inloggning.

Mer information finns i hello [kompatibilitetslista för Azure AD från tredje part federation](active-directory-aadconnect-federation-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Logga in användaren och användarens huvudnamn
### <a name="understanding-user-principal-name"></a>Förstå användarens huvudnamn
I Active Directory är suffixet för hello standard användarens huvudnamn (UPN) hello DNS-namnet på hello domän där hello användarkonto har skapats. I de flesta fall är detta hello domännamn som är registrerad som hello företagsdomänen på hello Internet. Du kan dock lägga till flera UPN-suffix genom att använda Active Directory-domäner och förtroenden.

hello UPN för hello användaren har hello format username@domain. För en Active Directory-domän med namnet ”contoso.com” kan till exempel en användare med namnet John ha hello UPN ”john@contoso.com”. hello UPN för hello användaren baserat på RFC 822. Även om hello UPN och e-resursen hello samma format, hello värdet för hello UPN för en användare kan eller inte hello samma som hello hello användarens e-postadress.

### <a name="user-principal-name-in-azure-ad"></a>Användarens huvudnamn i Azure AD
hello Azure AD Connect-guiden använder hello userPrincipalName attribut eller låter dig ange hello attribut (i en anpassad installation) toobe användas från lokala som hello huvudnamn i Azure AD. Detta är hello-värde som används för att logga in tooAzure AD. Om hello värdet för attributet userPrincipalName för hello inte stämmer överens tooa verifierade domän i Azure AD och Azure AD ersätter det med ett standardvärde. onmicrosoft.com värde.

Varje katalog i Azure Active Directory innehåller en inbyggd domännamnet med hello format contoso.onmicrosoft.com, som kan du komma igång med Azure eller andra Microsoft-tjänster. Du kan förbättra och förenkla hello inloggningen genom att använda anpassade domäner. Mer information om anpassade domännamn i Azure AD och hur tooverify en domän, se [lägga till din anpassade domän namn tooAzure Active Directory](../add-custom-domain.md#add-your-custom-domain).

## <a name="azure-ad-sign-in-configuration"></a>Inloggningskonfiguration för Azure AD
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-inloggning konfiguration med Azure AD Connect
hello Azure AD-inloggningsupplevelsen beror på om Azure AD kan matcha hello UPN-suffix för en användare som synkroniserade tooone hello anpassade domäner som har verifierats i hello Azure AD-katalog. Azure AD Connect innehåller hjälp när du konfigurerar Azure AD-inloggning inställningar så att hello inloggning användarupplevelsen i hello molnet är liknande toohello lokalt inträffar.

Azure AD Connect visar hello UPN-suffix som har definierats för hello domäner och försöker toomatch dem med en anpassad domän i Azure AD. Sedan hjälper den dig med hello lämpliga åtgärder som måste vidtas toobe.
hello Azure AD-inloggningssida visar hello UPN-suffix som har definierats för lokala Active Directory och hello motsvarande status mot varje suffix. hello statusvärden kan vara något av följande hello:

| Status | Beskrivning | Åtgärd krävs |
|:--- |:--- |:--- |
| Verifiera |Azure AD Connect att hitta en matchande verifierade domän i Azure AD. Alla användare för den här domänen kan logga in med sina lokala autentiseringsuppgifter. |Ingen åtgärd krävs. |
| Inte verifieras |Azure AD Connect finns matchande anpassade domäner i Azure AD, men verifieras inte. hello UPN-suffix hello användare av den här domänen kommer att ändras toohello standard. onmicrosoft.com suffix efter synkroniseringen om hello domän inte verifieras. | [Kontrollera hello anpassade domäner i Azure AD.](../add-custom-domain.md#verify-the-domain-name-with-azure-ad) |
| Inte lägga till |Azure AD Connect hittade inte en anpassad domän som hade toohello UPN-suffix. hello UPN-suffix hello användare av den här domänen kommer att ändras toohello standard. onmicrosoft.com-suffix om hello domän inte lagts till och verifieras i Azure. | [Lägg till och verifiera en anpassad domän som motsvarar toohello UPN-suffix.](../add-custom-domain.md) |

hello Azure AD-inloggningssida visas hello UPN-suffix som är definierade för lokala Active Directory och hello motsvarande anpassade domäner i Azure AD med hello aktuell status för verifiering. I en anpassad installation kan du nu välja hello attribut för hello huvudnamn på hello **Azure AD-inloggning** sidan.

![Azure AD-inloggningssida](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Du kan klicka på hello uppdatera knappen toore fetch hello senaste statusen för hello anpassade domäner från Azure AD.

### <a name="selecting-hello-attribute-for-hello-user-principal-name-in-azure-ad"></a>Att välja hello attribut för hello huvudnamn i Azure AD
hello attributet userPrincipalName är hello-attribut som användare använder när de loggar in tooAzure AD och Office 365. Du bör kontrollera hello-domäner (även kallat UPN-suffix) som används i Azure AD innan hello användarna synkroniseras.

Vi rekommenderar starkt att du behåller hello standard attributet userPrincipalName. Om det här attributet är nonroutable och inte kan verifieras möjliga tooselect är ett annat attribut (e-post, till exempel) som hello-attribut som innehåller hello inloggnings-ID. Detta kallas hello alternativa-ID. hello Alternativt ID attributvärdet måste följa hello RFC 822 standard. Du kan använda ett alternativt ID med både lösenord SSO och federation enkel inloggning som hello inloggning lösning.

> [!NOTE]
> Använda ett alternativt ID är inte kompatibel med alla Office 365-arbetsbelastningar. Mer information finns i [konfigurera alternativa inloggnings-ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-hello-azure-sign-in-experience"></a>Anpassad domän för olika tillstånd och deras effekt på hello Azure inloggning upplevelse
Det är mycket viktigt toounderstand hello förhållandet mellan hello domänen tillstånd i Azure AD-katalogen och hello UPN-suffix som definierats lokalt. Vi gå igenom hello olika möjliga Azure inloggningen inträffar när du konfigurerar synkronisering med hjälp av Azure AD Connect.

I följande information hello, antar vi att vi kan berörda med hello UPN-suffixet contoso.com, som används i hello lokal katalog som en del av UPN – till exempel user@contoso.com.

###### <a name="express-settingspassword-synchronization"></a>Express-inställningar/Lösenordssynkronisering
| Status | Effekt på Azure-inloggning användarupplevelse |
|:---:|:--- |
| Inte lägga till |I det här fallet har ingen anpassad domän för contoso.com lagts till i hello Azure AD-katalog. Användare som har UPN lokalt med hello suffix @contoso.com kommer inte att kunna toouse sina lokala UPN-toosign i tooAzure. De måste i stället toouse nya UPN som har angetts toothem av Azure AD genom att lägga till hello suffix för hello standard Azure AD-katalog. Till exempel om du synkroniserar användare toohello Azure AD directory azurecontoso.onmicrosoft.com sedan hello lokala användare user@contoso.com kommer att få ett UPN för user@azurecontoso.onmicrosoft.com. |
| Inte verifieras |I detta fall har vi en domänen contoso.com som har lagts till i hello Azure AD-katalog. Men har ännu inte kontrolleras. Om du går vidare med synkroniserar användare utan att verifiera domänen hello hello användare kommer att tilldelas nya UPN av Azure AD, som precis i hello ”inte lägga till” scenario. |
| Verifiera |I detta fall har vi en domänen contoso.com som redan har lagts till och verifieras i Azure AD för hello UPN-suffix. Användare kommer att kunna toouse sina lokala användarens huvudnamn, till exempel user@contoso.com, toosign i tooAzure när de har synkroniserats tooAzure AD. |

###### <a name="ad-fs-federation"></a>AD FS-federation
Du kan inte skapa en federation med standard-hello. onmicrosoft.com-domän i Azure AD eller en anpassad overifierade domän i Azure AD. När du kör hello Azure AD Connect-guiden om du väljer en overifierade domän toocreate en federation med och Azure AD Connect prompter du med hello nödvändiga registrerar toobe skapas där din DNS-Server är värd för hello domän. Mer information finns i [verifiera hello Azure AD-domän som valts för federation](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Om du har valt hello användaren inloggningsalternativ **Federation med AD FS**, du måste ha en anpassad domän toocontinue att skapa en federation i Azure AD. Vår beskrivning innebär det att vi ska ha en anpassad domän contoso.com som lagts till i hello Azure AD-katalog.

| Status | Effekt på hello Azure inloggning användarupplevelse |
|:---:|:--- |
| Inte lägga till |I det här fallet hittade Azure AD Connect en matchande anpassad domän för hello UPN-suffixet contoso.com i hello Azure AD-katalog. Du behöver tooadd en domänen contoso.com, om du behöver användare toosign i med hjälp av AD FS med deras lokala UPN (t.ex. user@contoso.com). |
| Inte verifieras |I det här fallet får Azure AD Connect du lämplig information om hur du kan verifiera din domän i ett senare skede. |
| Verifiera |I det här fallet kan du gå vidare med hello-konfiguration utan några ytterligare åtgärder. |

## <a name="changing-hello-user-sign-in-method"></a>Ändra hello användaren inloggningsmetod
Du kan ändra hello användaren inloggningsmetod från federationstjänsten, Lösenordssynkronisering eller direktautentisering med hjälp av hello uppgifter som är tillgängliga i Azure AD Connect efter hello inledande konfiguration av Azure AD Connect med hello guiden. Kör hello Azure AD Connect-guiden igen och visas en lista över aktiviteter som du kan utföra. Välj **ändra användarens inloggning** hello listan över aktiviteter.

![Ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

På nästa sida hello uppmanas du tooprovide hello autentiseringsuppgifter för Azure AD.

![Ansluta tooAzure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

På hello **användarinloggning** väljer hello önskade användare logga in.

![Ansluta tooAzure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Om du bara skapar en tillfällig växeln toopassword synkronisering, och välj sedan hello **konverterar inte användarkonton** kryssrutan. Kontrollerar inte hello alternativet konverterar toofederated för varje användare och det kan ta flera timmar.
>
>

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
- Lär dig mer om [designbegrepp för Azure AD Connect](active-directory-aadconnect-design-concepts.md).
