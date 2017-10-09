---
title: "aaaHow tooenable enkel inloggning mellan appar på Android använder ADAL | Microsoft Docs"
description: 'Hur hello toouse hello funktioner i ADAL SDK tooenable enkel inloggning i ditt program. '
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 3867e15030e5516464e4dbd92ba35894430daf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-android-using-adal"></a>Hur tooenable enkel inloggning mellan appar på Android använder ADAL
Att tillhandahålla enkel inloggning (SSO) så att användarna bara behöver tooenter sina autentiseringsuppgifter en gång och har de autentiseringsuppgifterna som automatiskt fungerar över program förväntas nu av kunder. hello svårt att ange sina användarnamn och lösenord på en liten skärm, ofta gånger kombineras med ytterligare en faktor (2FA) som ett telefonsamtal eller en textläge kod leder till att snabbt klagomål om en användare har toodo detta mer än en gång till produkten.

Dessutom, om du använder en identity-plattform som andra program kan använda, till exempel Microsoft Accounts eller ett arbetskonto från Office365, kunder förväntar sig att dessa autentiseringsuppgifter toobe tillgängliga toouse i alla program, oavsett hello leverantör.

hello Microsoft Identity-plattformen, tillsammans med vår Microsoft Identity-SDK: er, har den här tunga arbetet för dig och ger du hello möjlighet toodelight dina kunder med enkel inloggning antingen inom din egen suite av program eller som med våra broker kapaciteten och autentiseraren program över hello hela enheten.

Den här genomgången får du veta hur tooconfigure våra SDK i ditt program tooprovide denna förmån tooyour kunder.

Den här genomgången gäller för:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Villkorsstyrd åtkomst med Azure Active Directory

hello dokumentet ovan förutsätter att du vet hur för[etablera program på hello äldre portalen för Azure Active Directory](active-directory-how-to-integrate.md) och integrerad ditt program med hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) .

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>SSO begrepp i hello Microsoft Identity-plattformen
### <a name="microsoft-identity-brokers"></a>Microsoft Identity mäklare
Microsoft tillhandahåller program för varje mobil plattform som gör att för hello bryggning av autentiseringsuppgifter i program från olika leverantörer och gör för särskilda funktioner som kräver en enda säker plats varifrån toovalidate autentiseringsuppgifter. Vi kallar dem **mäklare**. På iOS och Android tillhandahålls dessa via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar vissa eller alla hello enhet för sina anställda. Dessa mäklare stöder Hantera säkerhet för vissa program eller hello hela enheten baserat på vad IT-administratörer som önskar. Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.

Mer information om hur använder vi dessa mäklare och hur dina kunder kan se dem i sina inloggningen flödet för hello Microsoft Identity plattform läsas på.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mönster för att logga in på mobila enheter
Åtkomst toocredentials på enheter följer två grundläggande mönster för hello Microsoft Identity plattform:

* Icke-förhandlad assisterad inloggningar
* Broker assisterad inloggningar

#### <a name="non-broker-assisted-logins"></a>Icke-förhandlad assisterad inloggningar
Icke-förhandlad assisterad inloggningar är inloggning upplevelser som inträffa infogade med hello program och Använd hello lokal lagring på hello enhet för programmet. Lagringen kan delas mellan program men hello autentiseringsuppgifterna är tätt bundna toohello app eller en uppsättning appar som använder denna autentiseringsuppgift. Du har förmodligen fått det i många mobila program när du anger ett användarnamn och lösenord i själva hello-programmet.

Dessa inloggningar har hello följande fördelar:

* Användarupplevelsen finns helt i hello-programmet.
* Autentiseringsuppgifter kan delas mellan program som är signerade av hello samma certifikat, vilket ger en enkel inloggning tooyour uppsättning program.
* Kontrollen runt hello upplevelse av loggning i finns toohello program före och efter inloggning.

Dessa inloggningar har följande nackdelar hello:

* Enkel inloggning på kan inte användarupplevelse över alla appar som använder Microsoft-Identity enbart över de Microsoft-Identities som programmet har konfigurerats.
* Programmet kan inte användas med mer avancerade funktioner för företag till exempel villkorlig åtkomst eller Använd hello InTune-produkter.
* Programmet stöder inte certifikatbaserad autentisering för företagsanvändare.

Här är en representation av hur hello Microsoft Identity SDK fungerar med hello delad lagring av ditt program tooenable enkel inloggning:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Broker assisterad inloggningar
Service Broker-stödd inloggningar är inloggning upplevelser som sker inom hello broker program och använder hello lagring och säkerhet för hello broker tooshare autentiseringsuppgifter för alla program på hello enhet som gäller hello Microsoft Identity-plattformen. Detta innebär att dina program som förlitar sig på hello broker toosign användare i. På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar hello enheten för sina användare. Ett exempel på den här typen av program är hello Microsoft Authenticator programmet på iOS. Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.
hello upplevelse varierar efter plattform och ibland kan vara störande toousers om de inte hanteras på rätt sätt. Du är mest förmodligen är bekant med det här mönstret om du har installerat hello Facebook-program och använder Facebook ansluta från ett annat program. hello hello Microsoft Identity-plattformen använder samma mönster.

För iOS leder tooa ”övergången” animering där programmet skickas toohello bakgrunden medan hello Microsoft Authenticator program kommer toohello förgrunden för hello användaren tooselect vilket konto som de vill ha toosign med.  

För Android och Windows hello konto visas Väljaren ovanpå ditt program som är mindre störande toohello användare.

#### <a name="how-hello-broker-gets-invoked"></a>Hur hello broker hämtar anropas
Om en kompatibel broker är installerad på hello enhet, som hello Microsoft Authenticator programmet hello Microsoft Identity SDK: er kommer automatiskt hello arbete anropar hello broker för dig när en användare anger de önskar toolog in med ett konto från hello Microsoft Identity plattform. Det här kontot kan vara ett personligt Microsoft-Account, ett arbets eller skolkonto, eller ett konto som du anger och värden i Azure med hjälp av vår B2C och B2B-produkter. 
 
 #### <a name="how-we-ensure-hello-application-is-valid"></a>Hur vi Kontrollera hello program är giltig
 
 hello måste tooensure hello identiteten för ett program anropet hello broker är avgörande toohello säkerhet som vi tillhandahåller broker stödd inloggningar. Varken iOS eller Android tillämpar unika identifierare är endast giltiga för ett visst program så att skadliga program kan ”förfalska” legitima program-ID och ta emot hello token är avsedda för hello legitima program. tooensure vi alltid kommunicerar med hello rätt program vid körning, ber vi hello developer tooprovide anpassade redirectURI när de registrerar sina program med Microsoft. **Hur utvecklare ska använda för att skapa den här omdirigerings-URI diskuteras i detalj nedan.** Den här anpassade redirectURI innehåller hello tumavtrycket för programmet hello och toobe unika toohello program säkerställs genom hello Google Play-butiken. När ett program anropar hello broker hello broker frågar hello operativsystemet Android tooprovide med hello tumavtryck för certifikat som kallas hello broker. hello broker ger tooMicrosoft detta certifikat-tumavtrycket i hello anropet tooour identitetssystem. Om hello certifikat tumavtryck hello programmet inte matchar hello certifikatets tumavtryck toous av hello utvecklare under registreringen kommer vi kommer att neka åtkomst toohello token för hello resurs hello program begär. Detta säkerställer att endast hello-program som är registrerat av hello utvecklare tar emot tokens.

**hello utvecklare har hello valet av om hello Microsoft Identity SDK-anrop hello broker eller använder hello icke-förhandlad assisterad flödet.** Men om hello utvecklare väljer inte toouse hello broker-stödd flödet de förlorar hello fördelen med att använda enkel inloggning autentiseringsuppgifter hello användaren kanske redan har lagt till på hello enhet och förhindrar att deras program som används med funktioner för företag Microsoft ger sina kunder som villkorlig åtkomst, Intune-hanteringsfunktioner och certifikatbaserad autentisering.

Dessa inloggningar har hello följande fördelar:

* Användaren upplever SSO över sina program oavsett hello leverantör.
* Programmet kan använda mer avancerade funktioner för företag som villkorlig åtkomst eller använda hello InTune-produkter.
* Programmet stöder certifikatbaserad autentisering för användare i verksamheten.
* Mycket säkrare inloggningen som hello identitet hello program- och Hej kontrolleras av hello broker program med ytterligare säkerhetsalgoritmer och kryptering.

Dessa inloggningar har följande nackdelar hello:

* I iOS övergick hello användare utanför din programupplevelse medan autentiseringsuppgifter är valt.
* Förlust av hello möjlighet toomanage hello inloggningen upplevelse för kunderna i ditt program.

Här är en representation av hur hello Microsoft Identity SDK fungerar med hello broker program tooenable enkel inloggning:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Tillsammans med den här bakgrundsinformation ska kunna toobetter att förstå och implementera enkel inloggning i ditt program med hjälp av hello Microsoft Identity plattform och SDK: er.

## <a name="enabling-cross-app-sso-using-adal"></a>Aktivera enkel inloggning mellan appar med hjälp av ADAL
Här använder vi hello ADAL Android SDK till:

* Aktivera icke-förhandlad stödd enkel inloggning för en uppsättning appar
* Aktivera stöd för Service broker-stödd enkel inloggning

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aktivera enkel inloggning för icke-förhandlad stödd SSO
För icke-förhandlad assisterad SSO över program hantera hello Microsoft Identity SDK mycket hello komplexitet SSO du. Detta inkluderar att hitta hello rätt användare i hello cache och underhålla en lista över inloggade användare för du tooquery.

tooenable enkel inloggning för program som du äger måste toodo hello följande:

1. Se till att alla dina program användaren hello samma klient-ID eller program-ID.
2. Se till att alla program som har samma SharedUserID ange hello.
3. Kontrollera att alla dina program dela hello samma signeringscertifikat från hello Google Play lagra så att du kan dela lagring.

#### <a name="step-1-using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>Steg 1: Använder hello samma klient-ID eller program-ID för alla hello program i din uppsättning appar
För att hello Microsoft Identity plattform tooknow att tillåts tooshare token för ditt program, behöver var och en av dina program tooshare hello samma klient-ID eller program-ID. Detta är hello Unik identifierare som du har fått tooyou när du har registrerat din första programmet hello-portalen.

Du kanske undrar hur du identifierar olika appar toohello Microsoft Identity-tjänsten om den använder hello samma program-ID. hello svaret är med hello **omdirigerings-URI: er**. Varje program kan ha flera omdirigerings-URI: er registrerade i hello onboarding-portalen. Varje app i din suite har en annan omdirigerings-URI. Ett exempel på hur detta ser ut understiger:

App1 omdirigerings-URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 omdirigerings-URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 omdirigerings-URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Dessa kapslas under hello samma klient-ID / program-ID och slås upp utifrån hello omdirigerings-URI som du kommer tillbaka toous i SDK-konfigurationen.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Observera att hello format dessa omdirigerings-URI: er beskrivs nedan. Du kan använda alla omdirigerings-URI om du vill toosupport hello broker, i vilket fall måste de se ut ungefär så hello ovan*

#### <a name="step-2-configuring-shared-storage-in-android"></a>Steg 2: Konfigurera delad lagring i Android
Inställningen hello `SharedUserID` ligger utanför det här dokumentet hello men kan hämtas genom att läsa hello Google Android dokumentation om hello [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Vad är viktigt är att du bestämmer vad du vill att din sharedUserID anropas och använda det i alla program.

När du har hello `SharedUserID` i dina program du är klar toouse enkel inloggning.

> [!WARNING]
> När du delar lagring över dina program kan alla program ta bort användare eller värre ta bort alla hello-token för ditt program. Detta är särskilt katastrofal om du har program som förlitar sig på hello token toodo bakgrundsjobbet. Dela lagring innebär att du måste vara mycket försiktig i alla remove-åtgärder via hello Microsoft Identity SDK: er.
> 
> 

Klart! hello Microsoft Identity SDK kommer nu att dela autentiseringsuppgifter i alla program. hello användarlistan också delas mellan programinstanser.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aktivera enkel inloggning för broker stödd SSO
Hej möjligheten för ett program toouse alla broker som är installerade på hello enheten är **inaktiverad som standard**. Ordna toouse ditt program med hello broker måste du göra ytterligare konfigurering och lägga till vissa kod tooyour program.

hello steg toofollow är:

1. Aktivera Service broker-läget i din programkod anropet toohello MS SDK
2. Upprätta en ny omdirigerings-URI och ange tooboth hello appen och appen registreringen
3. Ställa in hello rätt behörigheter i hello Android-manifest

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Steg 1: Aktivera Service broker-läge i ditt program
hello möjligheten för ditt program toouse hello broker är aktiverat när du skapar hello ”inställningar” eller installationen av autentisering-instans. Du kan göra detta genom att ange ApplicationSettings-typ i koden:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Steg 2: Skapa en ny omdirigerings-URI med URL-schema
I ordning tooensure att vi alltid returnera hello autentiseringsuppgifter tokens toohello rätt program, måste vi toomake att vi kallar tillbaka tooyour programmet på ett sätt som hello Android-operativsystemet kan verifiera. hello Android operativsystem använder hello-hash för certifikatet hello i hello Google Play store. Detta kan vara falsk en otillåtna program. Därför kan utnyttja vi detta tillsammans med hello URI för våra broker programmet tooensure hello token returneras toohello rätt program. Vi behöver du tooestablish denna unika omdirigerings-URI både i ditt program och anges som en omdirigerings-URI i vår developer-portalen.

Omdirigerings-URI måste vara i hello rätt form av:

`msauth://packagename/Base64UrlencodedSignature`

ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Den här omdirigerings-URI måste toobe som angetts i registreringen app med hello [Azure-portalen](https://portal.azure.com/). Mer information om registrering av Azure AD app finns [integrera med Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-hello-correct-permissions-in-your-application"></a>Steg 3: Ställ in hello behörigheter i ditt program
Vårt broker program i Android använder hello Accounts Manager funktion hello i Android OS toomanage autentiseringsuppgifter i program. I ordning toouse hello broker i Android måste app-manifest ha behörigheter toouse AccountManager konton. Det beskrivs i detalj i hello [Google-dokumentationen för hanteraren för kontosäkerhet här](http://developer.android.com/reference/android/accounts/AccountManager.html)

I synnerhet är dessa behörigheter:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Du har konfigurerat SSO!
Nu hello Microsoft Identity SDK automatiskt både dela autentiseringsuppgifter i dina program och anropa hello broker om den finns på sin enhet.

