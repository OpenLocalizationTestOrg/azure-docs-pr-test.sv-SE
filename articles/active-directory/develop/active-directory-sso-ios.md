---
title: "aaaHow tooenable enkel inloggning mellan appar på iOS använder ADAL | Microsoft Docs"
description: 'Hur hello toouse hello funktioner i ADAL SDK tooenable enkel inloggning i ditt program. '
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a>Hur tooenable enkel inloggning mellan appar på iOS använder ADAL
Att tillhandahålla enkel inloggning (SSO) så att användarna bara behöver tooenter sina autentiseringsuppgifter en gång och har de autentiseringsuppgifterna som automatiskt fungerar över program förväntas nu av kunder. hello svårt att ange sina användarnamn och lösenord på en liten skärm, ofta gånger kombineras med ytterligare en faktor (2FA) som ett telefonsamtal eller en textläge kod leder till att snabbt klagomål om en användare har toodo detta mer än en gång till produkten.

Dessutom, om du använder en identity-plattform som andra program kan använda, till exempel Microsoft Accounts eller ett arbetskonto från Office365, kunder förväntar sig att dessa autentiseringsuppgifter toobe tillgängliga toouse i alla program, oavsett hello leverantör.

hello Microsoft Identity-plattformen, tillsammans med vår Microsoft Identity-SDK: er, har den här tunga arbetet för dig och ger du hello möjlighet toodelight dina kunder med enkel inloggning antingen inom din egen suite av program eller som med våra broker kapaciteten och autentiseraren program över hello hela enheten.

Den här genomgången får du veta hur tooconfigure våra SDK i ditt program tooprovide denna förmån tooyour kunder.

Den här genomgången gäller för:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Villkorsstyrd åtkomst med Azure Active Directory

hello dokumentet ovan förutsätter att du vet hur för[etablera program på hello äldre portalen för Azure Active Directory](active-directory-how-to-integrate.md) och integrerad ditt program med hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>SSO begrepp i hello Microsoft Identity-plattformen
### <a name="microsoft-identity-brokers"></a>Microsoft Identity mäklare
Microsoft tillhandahåller program för varje mobil plattform som gör att för hello bryggning av autentiseringsuppgifter i program från olika leverantörer och gör för särskilda funktioner som kräver en enda säker plats varifrån toovalidate autentiseringsuppgifter. Vi kallar dem **mäklare**. På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar vissa eller alla hello enhet för sina anställda. Dessa mäklare stöder Hantera säkerhet för vissa program eller hello hela enheten baserat på vad IT-administratörer som önskar. Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.

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
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
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
 
 hello måste tooensure hello identiteten för ett program anropet hello broker är avgörande toohello säkerhet som vi tillhandahåller broker stödd inloggningar. Varken iOS eller Android tillämpar unika identifierare är endast giltiga för ett visst program så att skadliga program kan ”förfalska” legitima program-ID och ta emot hello token är avsedda för hello legitima program. tooensure vi alltid kommunicerar med hello rätt program vid körning, ber vi hello developer tooprovide anpassade redirectURI när de registrerar sina program med Microsoft. **Hur utvecklare ska använda för att skapa den här omdirigerings-URI diskuteras i detalj nedan.** Den här anpassade redirectURI innehåller hello paket-ID för programmet hello och toobe unika toohello program säkerställs genom hello Apple App Store. När ett program anropar hello broker hello broker ber hello iOS fungerar system tooprovide med hello paket-ID som kallas hello broker. hello broker ger detta paket-ID tooMicrosoft i hello anropet tooour identitetssystem. Om inte hello paket-ID för programmet hello matchar hello paket-ID angavs toous av hello utvecklare under registreringen vi kommer att neka åtkomst toohello token för hello resurs hello program begär. Detta säkerställer att endast hello-program som är registrerat av hello utvecklare tar emot tokens.

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
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
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
Här använder vi hello ADAL iOS SDK till:

* Aktivera icke-förhandlad stödd enkel inloggning för en uppsättning appar
* Aktivera stöd för Service broker-stödd enkel inloggning

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aktivera enkel inloggning för icke-förhandlad stödd SSO
För icke-förhandlad assisterad SSO över program hantera hello Microsoft Identity SDK mycket hello komplexitet SSO du. Detta inkluderar att hitta hello rätt användare i hello cache och underhålla en lista över inloggade användare för du tooquery.

tooenable enkel inloggning för program som du äger måste toodo hello följande:

1. Se till att alla dina program användaren hello samma klient-ID eller program-ID.
2. Kontrollera att alla dina program dela hello samma signering av certifikat från Apple så att du kan dela nyckelringar
3. Begära hello samma nyckelringar rätt för var och en av dina program.
4. Hello Microsoft Identity SDK: er om hello delade nyckelringen när du vill ange oss toouse.

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>Med hjälp av hello samma klient-ID eller program-ID för alla hello program i din uppsättning appar
För att hello Microsoft Identity plattform tooknow att tillåts tooshare token för ditt program, behöver var och en av dina program tooshare hello samma klient-ID eller program-ID. Detta är hello Unik identifierare som du har fått tooyou när du har registrerat din första programmet hello-portalen.

Du kanske undrar hur du identifierar olika appar toohello Microsoft Identity-tjänsten om den använder hello samma program-ID. hello svaret är med hello **omdirigerings-URI: er**. Varje program kan ha flera omdirigerings-URI: er registrerade i hello onboarding-portalen. Varje app i din suite har en annan omdirigerings-URI. Ett exempel på hur detta ser ut understiger:

App1 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

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

#### <a name="create-keychain-sharing-between-applications"></a>Skapa mellan program för delning av nyckelringar
Aktivera delning av nyckelringar ligger utanför det här dokumentet hello och omfattas av Apple i dokumentet [lägga till funktioner](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Vad är viktigt är att du bestämma vad du vill att din nyckelringar toobe kallas och lägga till den här funktionen i alla program.

När du har rättigheter som är konfigurerat på rätt sätt bör du se en fil i projektkatalogen rätt `entitlements.plist` som innehåller något som ser ut som följande hello:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

När du har hello nyckelringar rätt aktiverat i var och en av dina program och du är redo toouse SSO, berätta hello Microsoft Identity SDK nyckelringen med hjälp av hello följande inställning i din `ADAuthenticationSettings` med hello följande inställning:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> När du delar en nyckelring över dina program kan alla program ta bort användare eller värre ta bort alla hello-token för ditt program. Detta är särskilt katastrofal om du har program som förlitar sig på hello token toodo bakgrundsjobbet. Dela en nyckelring remove innebär att du måste vara mycket försiktig i alla-åtgärder via hello Microsoft Identity SDK: er.
> 
> 

Klart! hello Microsoft Identity SDK kommer nu att dela autentiseringsuppgifter i alla program. hello användarlistan också delas mellan programinstanser.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aktivera enkel inloggning för broker stödd SSO
Hej möjligheten för ett program toouse alla broker som är installerade på hello enheten är **inaktiverad som standard**. Ordna toouse ditt program med hello broker måste du göra ytterligare konfigurering och lägga till vissa kod tooyour program.

hello steg toofollow är:

1. Aktivera Service broker-läget i din programkod anropet toohello MS SDK.
2. Upprätta en ny omdirigerings-URI och anger tooboth hello appen och appen registreringen.
3. Registrera en URL-schema.
4. Stöd för iOS9: lägga till en behörighet tooyour info.plist-fil.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Steg 1: Aktivera Service broker-läge i ditt program
hello möjligheten för ditt program toouse hello broker är aktiverat när du skapar hello ”kontexten” eller installationen av autentisering-objektet. Du kan göra detta genom att ange vilken typ av autentiseringsuppgifter i koden:

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Hej `AD_CREDENTIALS_AUTO` inställningen medger hello Microsoft Identity SDK tootry toocall ut toohello broker `AD_CREDENTIALS_EMBEDDED` förhindrar hello Microsoft Identity SDK anropar toohello broker.

#### <a name="step-2-registering-a-url-scheme"></a>Steg 2: Registrera en URL-schema
hello Microsoft Identity-plattformen använder URL: er tooinvoke hello broker och gå sedan tillbaka tooyour kontrollprogrammet. toofinish serveranrop som du behöver ett URL-schema registrerats för ditt program att hello Microsoft Identity plattform vet om. Detta kan vara dessutom tooany andra app-scheman som du kanske redan har registrerat med ditt program.

> [!WARNING]
> Vi rekommenderar att du gör hello URL-schema ganska unika toominimize hello risken för en annan app med hello samma URL-schema. Apple tillämpar inte hello unika URL-scheman som är registrerade i hello app store.
> 
> 

Nedan visas ett exempel på hur den visas i projektkonfigurationen av. Du kan också göra detta i XCode samt:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Steg 3: Upprätta en ny omdirigerings-URI med URL-schema
I ordning tooensure att vi alltid returnera hello autentiseringsuppgifter tokens toohello rätt program, måste vi toomake att vi kallar tillbaka tooyour programmet på ett sätt som hello iOS-operativsystemet kan verifiera. hello iOS operativsystemet rapporter toohello Microsoft broker program hello paket-ID för hello programmet anropas. Detta kan vara falsk en otillåtna program. Därför kan utnyttja vi detta tillsammans med hello URI för våra broker programmet tooensure hello token returneras toohello rätt program. Vi behöver du tooestablish denna unika omdirigerings-URI både i ditt program och anges som en omdirigerings-URI i vår developer-portalen.

Omdirigerings-URI måste vara i hello rätt form av:

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Den här omdirigerings-URI måste toobe som angetts i registreringen app med hello [Azure-portalen](https://portal.azure.com/). Mer information om registrering av Azure AD app finns [integrera med Azure Active Directory](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a>Steg 3a: lägga till en omdirigerings-URI i din app och dev portal toosupport certifikatbaserad autentisering
en andra ”msauth”-toosupport certifikatbaserad autentisering måste toobe som registrerats i programmet och hello [Azure-portalen](https://portal.azure.com/) toohandle certifikatautentisering om du inte vill tooadd som stöder i ditt program.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a>Steg 4: iOS9: Lägg till en konfiguration parametern tooyour app
ADAL använder – canOpenURL: toocheck om hello broker är installerad på hello enhet. I iOS låsta 9 Apple scheman ett program kan fråga efter. Du behöver tooadd ”msauth” toohello LSApplicationQueriesSchemes avsnitt i din `info.plist file`.

<key>LSApplicationQueriesSchemes</key>

<array><string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Du har konfigurerat SSO!
Nu hello Microsoft Identity SDK automatiskt både dela autentiseringsuppgifter i dina program och anropa hello broker om den finns på sin enhet.

