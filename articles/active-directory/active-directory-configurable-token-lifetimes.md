---
title: "aaaConfigurable token livslängd i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooset livslängd för token som utfärdas av Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: 0d4c8545981c5463cc7c95f669167bbc38230123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurerbara token livslängd i Azure Active Directory (förhandsversion)
Du kan ange hello livslängden för en token som utfärdas av Azure Active Directory (AD Azure). Du kan ange token livslängd för alla program i din organisation, för ett program för flera innehavare (flera organisation) eller för en specifik tjänstens huvudnamn i din organisation.

> [!NOTE]
> Den här funktionen är för närvarande i förhandsversion. Förbereda toorevert eller ta bort alla ändringar. hello-funktionen är tillgänglig i alla Azure Active Directory-prenumeration under Public Preview. Men när hello funktionen blir allmänt tillgänglig, vissa aspekter av hello-funktionen kan kräva en [Azure Active Directory Premium](active-directory-get-started-premium.md) prenumeration.
>
>

I Azure AD representerar ett grupprincipobjekt en uppsättning regler som tillämpas på enskilda program eller på alla program i en organisation. Varje princip har en unik struktur med en uppsättning egenskaper som är kopplade tooobjects toowhich de tilldelas.

Du kan ange en princip som hello standardprincipen för din organisation. hello principen är tillämpade tooany program i hello organisation, så länge det inte åsidosätts av en princip med högre prioritet. Dessutom kan du tilldela en princip toospecific program. hello prioritetsordning varierar beroende på principtypen.


## <a name="token-types"></a>Typer av token

Du kan ange principer för livslängd för token för uppdaterings-tokens, åtkomsttoken, session token och ID-token.

### <a name="access-tokens"></a>Åtkomst-token
Klienter Använd åtkomst tokens tooaccess en skyddad resurs. En åtkomst-token kan användas endast för en specifik kombination av användare, klienten och resursen. Åtkomst-token kan inte återkallas och är giltiga tills de upphör att gälla. En skadlig aktören som har fått en åtkomst-token kan använda det för omfattningen av dess livslängd. Justera hello livstid av en åtkomst-token är en kompromiss mellan förbättra systemets prestanda och ökar hello tidsperiod som hello klient behåller åtkomst efter hello användarkontot har inaktiverats. Förbättrad prestanda uppnås genom att minska hello antalet gånger som en klient måste tooacquire en ny åtkomsttoken.

### <a name="refresh-tokens"></a>Uppdatera token
När en klient får en token tooaccess för åtkomst till en skyddad resurs, får hello klienten både en uppdateringstoken och en åtkomst-token. Hej uppdateringstoken är används tooobtain nya åtkomst/uppdatera token par när hello aktuella åtkomst-token upphör att gälla. En uppdateringstoken är bundna tooa kombination av användar- och klienten. En uppdateringstoken kan återkallas och hello token giltighet kontrolleras varje gång hello token används.

Det är viktigt toomake åtskillnad mellan konfidentiell och offentliga klienter. Läs mer om olika typer av klienter, [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Token livslängd med konfidentiella klienten uppdatera token
Konfidentiell klienter är program som kan lagras säkert lösenord klienten (hemliga). De kan visa att förfrågningarna kommer från hello klientprogrammet och inte från en skadlig aktören. Ett webbprogram är till exempel en konfidentiell klient eftersom det kan lagra en klienthemlighet på hello webbserver. Exponeras inte. Eftersom dessa flöden är säkrare hello standard livslängd för uppdaterings-tokens som utfärdade toothese flöden är `until-revoked`, kan inte ändras med hjälp av Grupprincip och kommer inte att återkallas på frivilliga lösenordsåterställning.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Token livslängd med offentliga klienten uppdatera token

Offentliga klienter kan inte på ett säkert sätt att lagra ett klient-lösenord (hemliga). En iOS/Android-app obfuscate inte en hemlighet från hello resurs-ägare, så är det en offentlig klient. Du kan ange principer på resurser tooprevent uppdaterings-tokens från offentliga klienter som är äldre än en angiven period från att erhålla ett nytt åtkomst/uppdatera token par. (toodo detta, Använd hello uppdatera Token inaktiva Maxtid egenskap.) Du kan också använda principer tooset en tidsperiod efter vilken hello uppdaterings-tokens accepteras inte längre. (toodo detta, Använd hello uppdatera Token Max Age egenskap.) Du kan justera hello livslängden för en uppdatering token toocontrol när och hur ofta hello användaren är obligatoriska tooreenter autentiseringsuppgifter, i stället för att tyst återautentiseras, när du använder en offentlig klientprogrammet.

### <a name="id-tokens"></a>ID-token
ID-token skickas toowebsites och interna klienter. ID-token innehåller profilinformation om en användare. En ID-token är bundna tooa specifik kombination av användar- och klienten. ID-token anses giltiga tills de upphör att gälla. Vanligtvis ett webbprogram matchar en användare har sessioners livstid i hello programmet toohello livstid hello-ID-token som utfärdas för hello användare. Du kan justera hello livslängden för ett ID token toocontrol hur ofta hello webbprogrammet upphör att gälla hello program och hur ofta kräver hello användaren toobe autentisera med Azure AD (tyst eller interaktivt).

### <a name="single-sign-on-session-tokens"></a>Enkel inloggning session token
När en användare autentiserar med Azure AD och väljer hello **Håll mig inloggad** kryssrutan, en enkel inloggning (SSO) upprättas session med hello användarens webbläsare och Azure AD. hello SSO-token i hello form av en cookie representerar den här sessionen. Observera att hello SSO sessionstoken inte är bunden tooa specifika resurs/klientprogram. SSO session token kan återkallas och deras giltighet kontrolleras varje gång de används.

Azure AD använder två typer av token för SSO-session: beständiga och Uppdateringsvärdet. Beständiga session token lagras som beständiga cookies hello webbläsare. Uppdateringsvärdet session token lagras som sessions-cookies. (Sessionscookies förstörs när hello webbläsaren stängs.)

Uppdateringsvärdet session token har en livslängd på 24 timmar. Beständiga token har en livslängd på 180 dagar. Helst en SSO sessionstoken används inom sin giltighetstid förlängs hello giltighetstid annat 24 timmar eller 180 dagar, beroende på hello tokentyp. Om en SSO sessionstoken inte används inom sin giltighetstid, anses det upphört att gälla och är inte längre att accepteras.

Du kan använda en princip tooset hello tid när hello första sessionstoken utfärdades utöver vilka hello sessionstoken accepteras inte längre. (toodo detta, Använd hello Session Token Max Age egenskap.) Du kan justera hello livslängden för en session token toocontrol när och hur ofta en användare är obligatoriska tooreenter autentiseringsuppgifter, i stället för tyst verifierats när du använder ett webbprogram.

### <a name="token-lifetime-policy-properties"></a>Egenskaper för livslängd för token
En princip för livslängd för token är en typ av grupprincipobjekt som innehåller regler för livslängd för token. Använd hello egenskaper för hello princip toocontrol angetts livstid för token. Om ingen princip har angetts, tillämpar hello system hello standardvärdet livslängd.

### <a name="configurable-token-lifetime-properties"></a>Livslängd för token konfigurerbara egenskaper
| Egenskap | Princip för egenskapssträng | Påverkar | Standard | Minimum | Maximalt |
| --- | --- | --- | --- | --- | --- |
| Livslängd för åtkomst-Token |AccessTokenLifetime |Åtkomsttoken, ID-token, SAML2-token |1 timme |10 minuter |1 dag |
| Uppdatera Token inaktiva Maxtid |MaxInactiveTime |Uppdatera token |14 dagar |10 minuter |90 dagar |
| Enskild faktor uppdatera Token maximal ålder |MaxAgeSingleFactor |Uppdatera token (för alla användare) |Tills återkallats |10 minuter |Tills återkallas<sup>1</sup> |
| Multi-Factor uppdatera Token maximal ålder |MaxAgeMultiFactor |Uppdatera token (för alla användare) |Tills återkallats |10 minuter |Tills återkallas<sup>1</sup> |
| Enskild faktor Session Token maximal ålder |MaxAgeSessionSingleFactor<sup>2</sup> |Sessionen token (beständiga och Uppdateringsvärdet) |Tills återkallats |10 minuter |Tills återkallas<sup>1</sup> |
| Multi-Factor Session Token maximal ålder |MaxAgeSessionMultiFactor<sup>3</sup> |Sessionen token (beständiga och Uppdateringsvärdet) |Tills återkallats |10 minuter |Tills återkallas<sup>1</sup> |

* <sup>1</sup>365 dagar är hello explicit maxlängd som kan anges för dessa attribut.
* <sup>2</sup>om **MaxAgeSessionSingleFactor** är inte ange det här värdet tar hello **MaxAgeSingleFactor** värde. Om varken parametern anges tar hello egenskapen hello standardvärdet (förrän har återkallats).
* <sup>3</sup>om **MaxAgeSessionMultiFactor** är inte ange det här värdet tar hello **MaxAgeMultiFactor** värde. Om varken parametern anges tar hello egenskapen hello standardvärdet (förrän har återkallats).

### <a name="exceptions"></a>Undantag
| Egenskap | Påverkar | Standard |
| --- | --- | --- |
| Uppdatera Token Max Age (utfärdas för federerade användare har inte tillräckligt återkallningsinformation<sup>1</sup>) |Uppdatera token (utfärdas för federerade användare har inte tillräckligt återkallningsinformation<sup>1</sup>) |12 timmar |
| Uppdatera Token inaktiva Maxtid (utfärdats för konfidentiell klienter) |Uppdatera token (utfärdats för konfidentiell klienter) |90 dagar |
| Uppdatera Token Max Age (utfärdats för konfidentiell klienter) |Uppdatera token (utfärdats för konfidentiell klienter) |Tills återkallats |

* <sup>1</sup>externa användare har inte tillräckligt återkallningsinformation som innehåller alla användare som inte har hello ”LastPasswordChangeTimestamp”-attribut som synkroniseras. Dessa användare får den här korta Max Age eftersom AAD är tooverify när toorevoke token som är knutna tooan gamla autentiseringsuppgifter (till exempel ett lösenord som har ändrats) och måste kontrollera tillbaka i oftare tooensure hello användaren och associerade token är fortfarande i god position. tooimprove upplevelsen, klient som administratörer måste se till att de synkroniserar hello ”LastPasswordChangeTimestamp”-attribut (Detta kan ställas in på hello användarobjekt med hjälp av Powershell eller via AADSync).

### <a name="policy-evaluation-and-prioritization"></a>För principutvärdering och prioritering
Du kan skapa och tilldela ett specifikt program för livslängd för token princip tooa, tooyour organisation och tooservice säkerhetsobjekt. Flera principer kan tillämpas tooa specifika program. Dessa regler: hello livslängd för token-princip som påverkas

* Om en princip specifikt tilldelas toohello tjänstens huvudnamn, tillämpas.
* Om ingen princip är uttryckligen har tilldelats toohello tjänstens huvudnamn, tillämpas en princip som tilldelats toohello överordnad organisation för hello tjänstens huvudnamn.
* Om ingen princip uttryckligen har tilldelats toohello tjänstens huvudnamn eller toohello organisation, tillämpas hello tilldelat toohello program.
* Ingen princip har tilldelats toohello service principal, hello organisation eller hello programobjektet hello standardvärden framtvingas. (Se tabellen hello i [livslängd för token konfigurerbara egenskaper](#configurable-token-lifetime-properties).)

Läs mer om hello relation mellan program och tjänstens huvudnamn objekt [program och tjänstens huvudnamn objekt i Azure Active Directory](active-directory-application-objects.md).

En token giltigheten utvärderas när hello hello token används. hello-princip med hello högsta prioriteten på hello-program som används börjar gälla.

> [!NOTE]
> Här är ett exempel.
>
> En användare vill tooaccess två webbprogram: A-webbprogram och Web Application B.
> 
> Faktorer:
> * Både webbprogram finns i hello samma överordnade organisation.
> * Token livstid princip 1 med en Session Token Max Age åtta timmar har angetts som standard hello överordnade organisationen.
> * Webbprogrammet A är en vanlig användning webbprogram som är inte länkade tooany principer.
> * Web Application B används för mycket känslig processer. Tjänsten som är länkade tooToken livstid princip 2, som har en Session Token Max Age 30 minuter.
>
> Klockan 12:00, hello användaren startar en ny webbläsarsession och försöker tooaccess Web Application A. hello är omdirigerade tooAzure AD och begärs toosign i. Detta skapar en cookie som har en sessionstoken i hello webbläsare. hello användaren är omdirigerade tillbaka tooWeb ett program med en ID-token som gör att hello användaren tooaccess hello program.
>
> Klockan 12:15 försöker hello användare tooaccess Web Application B. hello webbläsare omdirigerar tooAzure AD, som identifierar hello sessions-cookie. Web Application B tjänstens huvudnamn är länkade tooToken livstid princip 2, men det är också en del av hello överordnade organisation standard Token livstid princip 1. Token livstid princip 2 börjar gälla eftersom principer länkade tooservice säkerhetsobjekt har högre prioritet än standardprinciperna för organisationen. hello sessionstoken ursprungligen utfärdades inom hello senaste 30 minuterna, så att det ska vara giltigt. hello användaren är omdirigerade tillbaka tooWeb programmet B med en ID-token som ger dem behörighet.
>
> Klockan 13:00 är hello försöker tooaccess Web Application A. hello användare omdirigerade tooAzure AD. Webbplatsen programmet är inte länkade tooany principer, men eftersom den är i en organisation med standard Token livstid princip 1 principen träder i kraft. hello sessions-cookie som ursprungligen utfärdades inom hello senaste åtta timmar har identifierats. hello användaren är tyst omdirigerade tillbaka tooWeb ett program med en ny ID-token. hello användaren är inte obligatoriska tooauthenticate.
>
> Omedelbart efteråt, hello användare försöker är tooaccess Web Application B. hello användaren omdirigerade tooAzure AD. Som börjar tidigare, Token livstid princip 2 gälla. Eftersom hello token utfärdas mer än 30 minuter sedan, hello användaren är begärd tooreenter sina inloggningsuppgifter. En helt ny sessionstoken och ID-token utfärdas. hello-användare kan sedan komma åt Web Application B.
>
>

## <a name="configurable-policy-property-details"></a>Information om konfigurerbar princip
### <a name="access-token-lifetime"></a>Livslängd för åtkomst-Token
**Sträng:** AccessTokenLifetime

**Påverkar:** åtkomsttoken, ID-token

**Sammanfattning:** den här principen styr hur länge åtkomst och ID-token för den här resursen är giltiga. Minska hello åtkomst livslängd för Token egenskapen minskar hello risken för en åtkomst-token eller en ID-token som används av en skadlig aktören under en längre tidsperiod. (Dessa token kan inte återkallas.) hello kompromiss är att prestanda påverkas negativt, eftersom hello token har toobe ersättas oftare.

### <a name="refresh-token-max-inactive-time"></a>Uppdatera Token inaktiva Maxtid
**Sträng:** MaxInactiveTime

**Påverkar:** Uppdateringstoken

**Sammanfattning:** den här principen styr hur gammal en uppdateringstoken kan vara innan en klient kan inte längre använda den tooretrieve ett nytt åtkomst/uppdatera token par vid försök tooaccess den här resursen. Eftersom en ny uppdateringstoken vanligtvis returneras när en uppdateringstoken används förhindrar den här principen åtkomst om hello klienten försöker tooaccess någon resurs med hjälp av hello aktuella uppdateringstoken under hello angiven tidsperiod.

Den här principen gör att användare som inte har varit aktivt på deras klient tooreauthenticate tooretrieve en ny uppdateringstoken.

hello uppdatera Token inaktiva Maxtid egenskapen måste anges tooa lägre värde än hello enskild faktor Token Max Age och hello Multi-Factor uppdatera Token Max Age egenskaper.

### <a name="single-factor-refresh-token-max-age"></a>Enskild faktor uppdatera Token maximal ålder
**Sträng:** MaxAgeSingleFactor

**Påverkar:** Uppdateringstoken

**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en uppdatering token tooget en ny åtkomst/uppdatera token par efter de senaste autentiseras korrekt med hjälp av bara en enda faktor. När en användare autentiseras och tar emot en ny uppdateringstoken, hello användaren kan använda hello uppdatering tokenflöde för hello angetts period tid. (Detta är SANT så länge hello aktuella uppdateringstoken inte har återkallats och det inte inte används längre än hello inaktiva tiden.) Vid den punkten är hello användaren framtvingad tooreauthenticate tooreceive en ny uppdateringstoken.

Minska maximal ålder för hello tvingar användare tooauthenticate oftare. Eftersom single-factor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du använder egenskapen tooa värdet som är lika tooor mindre än hello Multi-Factor uppdatera Token Max Age egenskapen.

### <a name="multi-factor-refresh-token-max-age"></a>Multi-Factor uppdatera Token maximal ålder
**Sträng:** MaxAgeMultiFactor

**Påverkar:** Uppdateringstoken

**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en uppdatering token tooget en ny åtkomst/uppdatera token par efter de senaste autentiseras korrekt med hjälp av flera faktorer. När en användare autentiseras och tar emot en ny uppdateringstoken, hello användaren kan använda hello uppdatering tokenflöde för hello angetts period tid. (Detta är SANT så länge hello aktuella uppdateringstoken inte har återkallats och är inte inte används längre än hello inaktiva tiden.) Då tvingas användarna tooreauthenticate tooreceive en ny uppdateringstoken.

Minska maximal ålder för hello tvingar användare tooauthenticate oftare. Eftersom single-factor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du har angett den här egenskapen tooa-värde som är lika tooor som är större än hello enskild faktor uppdatera Token Max Age egenskapen.

### <a name="single-factor-session-token-max-age"></a>Enskild faktor Session Token maximal ålder
**Sträng:** MaxAgeSessionSingleFactor

**Påverkar:** Session token (beständiga och Uppdateringsvärdet)

**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en session token tooget ett nytt-ID och session token när de senast autentiseras korrekt med bara en enda faktor. När en användare autentiseras och tar emot en ny session-token, hello användaren kan använda hello session tokenflöde för hello angetts period tid. (Detta är SANT så länge hello aktuella sessionstoken som inte har återkallats och inte har upphört att gälla.) När hello angett tidsperiod, är hello användaren framtvingad tooreauthenticate tooreceive en ny session-token.

Minska maximal ålder för hello tvingar användare tooauthenticate oftare. Eftersom single-factor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du har angett den här egenskapen tooa-värde som är lika tooor som är mindre än hello Multi-Factor Session Token Max Age egenskap.

### <a name="multi-factor-session-token-max-age"></a>Multi-Factor Session Token maximal ålder
**Sträng:** MaxAgeSessionMultiFactor

**Påverkar:** Session token (beständiga och Uppdateringsvärdet)

**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en session token tooget ett nytt-ID och session token efter hello senast som de har autentiseras med hjälp av flera faktorer. När en användare autentiseras och tar emot en ny session-token, hello användaren kan använda hello session tokenflöde för hello angetts period tid. (Detta är SANT så länge hello aktuella sessionstoken som inte har återkallats och inte har upphört att gälla.) När hello angett tidsperiod, är hello användaren framtvingad tooreauthenticate tooreceive en ny session-token.

Minska maximal ålder för hello tvingar användare tooauthenticate oftare. Eftersom single-factor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du har angett den här egenskapen tooa-värde som är lika tooor som är större än hello enskild faktor Session Token Max Age egenskapen.

## <a name="example-token-lifetime-policies"></a>Exempel på principer livslängd för token
Många scenarier är möjliga i Azure AD när du kan skapa och hantera token livslängd för appar, tjänstens huvudnamn och din organisation. I det här avsnittet går vi igenom några vanliga princip scenarier som kan hjälpa dig att införa nya regler för:

* Livslängd för token
* Token inaktiva Maxtid
* Token maximal ålder

I hello exempel kan du lära dig hur du:

* Hantera standardprincipen för en organisation
* Skapa en princip för web-inloggning
* Skapa en princip för en intern app som anropar ett webb-API
* Hantera en princip för Avancerat

### <a name="prerequisites"></a>Krav
I följande exempel hello, du skapa, uppdatera, länkar och ta bort principer för appar, tjänstens huvudnamn och din organisation. Om du är ny tooAzure AD, rekommenderar vi att du lär dig mer om [hur tooget en Azure AD-klient](active-directory-howto-tenant.md) innan du fortsätter med de här exemplen.  

tooget igång, hello följande steg:

1. Hämta senaste hello [Azure AD PowerShell-modulen offentliga förhandsversionen](https://www.powershellgallery.com/packages/AzureADPreview).
2. Kör hello `Connect` kommandot toosign i tooyour Azure AD-administratörskonto. Kör det här kommandot varje gång startar du en ny session.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. toosee alla principer som har skapats i din organisation, kör hello efter kommandot. Kör detta kommando efter de flesta åtgärder i hello följande scenarier. Kör kommandot hello också hjälper dig att börja hello ** ** dina principer.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Exempel: Hantera standardprincipen för en organisation
I det här exemplet skapar du en princip som gör att användare kan logga in mindre ofta i hela organisationen. toodo, skapa en livslängd för token-princip för enskild faktor uppdatera token som används inom organisationen. hello princip är tillämpade tooevery program i din organisation och tooeach tjänstens huvudnamn som inte redan har en princip.

1. Skapa en princip för livslängd för token.

    1.  Ange hello enskild faktor uppdatera Token för ”tills har återkallats”. hello-token gälla inte förrän åtkomst har återkallats. Skapa följande principdefinitionen hello:

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  toocreate hello princip kör hello följande kommando:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  toosee din nya principen och tooget hello princip **ObjectId**kör hello följande kommando:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Uppdatera hello-principen.

    Du kan välja hello första principen som du anger i det här exemplet inte är så strikta din tjänst kräver. tooset din enda faktor uppdatera Token tooexpire två dagar kör hello följande kommando:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Exempel: Skapa en princip för web-inloggning

I det här exemplet skapar du en princip som kräver att användare tooauthenticate oftare i ditt webbprogram. Den här principen anger hello livstid hello åtkomst-ID-token och Hej max ålder Multi-Factor session token toohello tjänstens huvudnamn för ditt webbprogram.

1. Skapa en princip för livslängd för token.

    Den här principen för web inloggning, anger livslängd för token hello åtkomst-ID och Hej max enskild faktor token ålder tootwo sessionstimmar.

    1.  toocreate hello princip köra det här kommandot:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee din nya principen och tooget hello princip **ObjectId**kör hello följande kommando:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  Tilldela hello princip tooyour tjänstens huvudnamn. Du måste också tooget hello **ObjectId** för tjänstens huvudnamn. 

    1.  toosee din organisations tjänstens huvudnamn som du kan fråga [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). I [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), logga in tooyour Azure AD-kontot.

    2.  När du har hello **ObjectId** huvudnamn för tjänsten köras hello följande kommando:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Exempel: Skapa en princip för en intern app som anropar ett webb-API
I det här exemplet skapar du en princip som kräver att användare tooauthenticate mindre ofta. hello princip förlängs också hello mängden tid som en användare kan vara inaktiv innan hello användaren måste autentiseras. hello principen är tillämpade toohello webb-API. När hello inbyggda appen begär hello webb-API som en resurs, används den här principen.

1. Skapa en princip för livslängd för token.

    1.  toocreate en strikt princip för en webb-API, kör hello följande kommando:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee din nya principen och tooget hello princip **ObjectId**kör hello följande kommando:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Tilldela hello princip tooyour webb-API. Du måste också tooget hello **ObjectId** för programmet. Hej bästa sätt toofind appens **ObjectId** är toouse hello [Azure-portalen](https://portal.azure.com/).

   När du har hello **ObjectId** för din app, kör hello följande kommando:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of hello Application> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Exempel: Hantera en princip för Avancerat
I det här exemplet skapar du några principer, toolearn hur hello prioritet system fungerar. Du kan också lära dig hur toomanage flera principer som är kopplade tooseveral objekt.

1. Skapa en princip för livslängd för token.

    1.  toocreate standardprincipen för en organisation som anger hello enskild faktor uppdatera Token livstid too30 dagar, kör följande kommando hello:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  toosee din nya principen och tooget hello princip **ObjectId**kör hello följande kommando:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Tilldela hello princip tooa tjänstens huvudnamn.

    Du har nu en princip som gäller toohello hela organisationen. Du kanske vill toopreserve 30-dagars principen för en specifik tjänstens huvudnamn men ändra hello organisation standard princip toohello övre gränsen för ”tills har återkallats”.

    1.  toosee din organisations tjänstens huvudnamn som du kan fråga [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). I [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), logga in med hjälp av Azure AD-kontot.

    2.  När du har hello **ObjectId** huvudnamn för tjänsten köras hello följande kommando:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
            ```
        
3. Ange hello `IsOrganizationDefault` flaggan toofalse:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Skapa en ny organisation standardprincip:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Nu har du hello ursprungliga princip länkade tooyour tjänstens huvudnamn och hello ny princip har angetts som din organisation standardprincipen. Det är viktigt tooremember att principer tillämpas tooservice säkerhetsobjekt har högre prioritet än standardprinciperna för organisationen.

## <a name="cmdlet-reference"></a>Cmdlet-referens

### <a name="manage-policies"></a>Hantera principer

Du kan använda följande cmdlet: ar toomanage principer hello.

#### <a name="new-azureadpolicy"></a>Ny AzureADPolicy

Skapar en ny princip.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Matris med stringified JSON som innehåller alla hello princip regler. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |Sträng med hello principnamn. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |Om värdet är true anger du hello principen som hello organisation standardprincipen. Om värdet är FALSKT får ingen effekt. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |Typen av princip. Token livslängd alltid använda ”TokenLifetimePolicy”. | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Valfritt] |Anger ett alternativt ID för hello princip. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Hämtar alla Azure AD-principer eller en angiven princip.

```PowerShell
Get-AzureADPolicy
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code>[Valfritt] |**Objekt-ID (Id)** av hello-princip som du vill. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Hämtar alla appar och tjänstens huvudnamn som är länkade tooa princip.

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av hello-princip som du vill. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Ange AzureADPolicy
Uppdaterar en befintlig princip.

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av hello-princip som du vill. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |Sträng med hello principnamn. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code>[Valfritt] |Matris med stringified JSON som innehåller alla hello princip regler. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code>[Valfritt] |Om värdet är true anger du hello principen som hello organisation standardprincipen. Om värdet är FALSKT får ingen effekt. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code>[Valfritt] |Typen av princip. Token livslängd alltid använda ”TokenLifetimePolicy”. |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Valfritt] |Anger ett alternativt ID för hello princip. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Ta bort AzureADPolicy
Tar bort hello angetts princip.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av hello-princip som du vill. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Användningsprinciper
Du kan använda hello följande cmdletar för principer för program.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Lägg till AzureADApplicationPolicy
Länkar hello angetts tooan tillämpning av principer.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** av hello princip. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Hämtar hello-princip som tilldelats tooan program.

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Ta bort AzureADApplicationPolicy
Tar bort en princip från ett program.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** av hello princip. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Tjänstens huvudnamn principer
Du kan använda hello följande cmdletar för tjänstens huvudnamn principer.

#### <a name="add-azureadserviceprincipalpolicy"></a>Lägg till AzureADServicePrincipalPolicy
Länkar hello principens tooa tjänstens huvudnamn.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** av hello princip. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Hämtar alla princip länkade toohello angivna tjänstens huvudnamn.

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Ta bort AzureADServicePrincipalPolicy
Tar bort hello princip från hello angivna tjänstens huvudnamn.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parametrar | Beskrivning | Exempel |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objekt-ID (Id)** av programmet hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** av hello princip. | `-PolicyId <ObjectId of Policy>` |
