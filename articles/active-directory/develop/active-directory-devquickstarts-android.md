---
title: "aaaAzure AD Android komma igång | Microsoft Docs"
description: "Hur skyddade toobuild ett Android-program som kan integreras med Azure AD för inloggnings- och Azure AD-anrop API: er med hjälp av OAuth."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a>Integrera Azure AD i en Android-app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/Android), som hjälper dig att komma igång med Azure AD i bara några minuter. hello developer-portalen tar dig igenom hello process för att registrera en app och integrera Azure AD i din kod. När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen.
>
>

Om du utvecklar ett skrivbordsprogram gör Azure Active Directory (AD Azure) det lätt för tooauthenticate du dina användare genom att använda sina lokala Active Directory-konton. Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.

Azure AD innehåller hello Active Directory Authentication Library (ADAL) för Android-klienter som behöver tooaccess skyddade resurser. hello uteslutande av ADAL är toomake det enkelt för din app tooget åtkomst-token. toodemonstrate hur lätt det är vi ska skapa ett program för Android att göra-lista som:

* Hämtar åtkomst till token för att anropa API en att göra-lista med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Hämtar uppgiftslista för en användare.
* Loggar ut användare.

tooget igång, behöver du en Azure AD-klient som du kan skapa användare och registrera ett program. Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a>Steg 1: Hämta och köra hello Node.js REST API TODO exempelserver
Hej Node.js REST API TODO exempel skrivs specifikt toowork mot vår befintliga exemplet för att skapa en enskild klient uppgiften REST API för Azure AD. Det här är en förutsättning för hello Snabbstart.

Mer information om hur tooset detta, se vår befintliga prover i [Microsoft Azure Active Directory exempel REST API-tjänsten för Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Steg 2: Registrera ditt webb-API med Azure AD-klient
Active Directory har stöd för att lägga till två typer av program:

- Web API: er som erbjuder tjänster toousers
- Program (som körs på hello web eller på en enhet) som har åtkomst till de webb-API: er

I det här steget du registrera hello webb-API som du kör lokalt för att testa det här exemplet. Den här webb-API är normalt en REST-tjänst som är en funktion för erbjudande som du vill att en app tooaccess. Azure AD kan du skydda valfri slutpunkt.

Förutsätter vi att att du registrera hello TODO REST-API som refererar till tidigare. Men det här fungerar för alla webb-API som du vill skydda toohelp för Azure Active Directory.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Ange ett eget namn för programmet hello (till exempel **TodoListService**), Välj **webbprogram och/eller webb-API**, och klicka på **nästa**.
6. Ange hello bas-URL för hello inloggnings-URL, hello exempel. Detta är som standard `https://localhost:8080`.
7. Klicka på **OK** toocomplete hello registrering.
8. Gå tooyour program sida fortfarande i hello Azure-portalen, hitta hello program-ID-värde och kopiera den. Du behöver det senare när du konfigurerar ditt program.
9. Från hello **inställningar** -> **egenskaper** sidan, uppdatera hello app-ID URI - ange `https://<your_tenant_name>/TodoListService`. Ersätt `<your_tenant_name>` med hello namn för din Azure AD-klient.

## <a name="step-3-register-hello-sample-android-native-client-application"></a>Steg 3: Registrera hello exempelprogrammet Android Native Client
I det här exemplet måste du registrera ditt webbprogram. Detta gör att dina program toocommunicate med hello just registrerade webb-API. Azure AD att neka tooeven bevilja ditt program tooask för inloggning om den är registrerad. Som är del av hello säker hello modellen.

Förutsätter vi att att du registrerar hello exempelprogrammet refererar till tidigare. Men den här metoden fungerar för alla appar som du utvecklar.

> [!NOTE]
> Du kanske undrar varför du ska lägga ett program och ett webb-API i en klient. Du kan skapa en app som ansluter till en extern API som är registrerad i Azure AD från en annan klient som du kan ha gissa. Om du gör det kan kunderna blir ombedd tooconsent toohello användning av hello API i hello program. Active Directory Authentication Library för iOS hand tar om detta godkännande för dig. Får du veta mer avancerade funktioner, visas detta är en viktig del av hello arbete krävs tooaccess hello uppsättning Microsoft APIs från Azure och Office, samt-leverantör. Just nu eftersom du har registrerat både webb-API och ditt program under hello samma klient visas inte några uppmaningar om samtycke. Detta är vanligtvis hello fallet om du utvecklar ett program för egna företagets toouse.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Ange ett eget namn för programmet hello (till exempel **TodoListClient Android**), Välj **internt klientprogram**, och klicka på **nästa**.
6. För hello omdirigerings-URI, ange `http://TodoListClient`. Klicka på **Slutför**.
7. Hitta hello program-ID-värde från hello program sida och kopiera den. Du behöver det senare när du konfigurerar ditt program.
8. Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**.  Leta upp och välj TodoListService, Lägg till hello **åtkomst TodoListService** behörighet under **delegerade behörigheter**, och klicka på **klar**.

toobuild med Maven kan du använda pom.xml toppnivå hello:

1. Klona den här lagringsplatsen i en katalog som du väljer:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Åtgärderna i hello hello [krav tooset konfigurera Maven miljön för Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Ställ in hello emulator med SDK-19.
4. Gå toohello rotmappen där du har klonat hello lagringsplatsen.
5. Kör det här kommandot:`mvn clean install`
6. Ändra hello directory toohello Snabbstart exempel:`cd samples\hello`
7. Kör det här kommandot:`mvn android:deploy android:run`

   Du bör se hello app startar.
8. Ange test användarens autentiseringsuppgifter tootry.

JAR paket skickas bredvid hello AAR paketet.

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a>Steg 4: Hämta hello Android ADAL och lägga till den tooyour Eclipse arbetsytan
Vi har gjort det enkelt för toohave du flera alternativ toouse ADAL i din Android-projekt:

* Du kan använda hello källa kod tooimport det här biblioteket i Eclipse och länka tooyour program.
* Om du använder Android Studio kan du använda hello AAR paketet format och referens hello binärfiler.

### <a name="option-1-source-zip"></a>Alternativ 1: Källa Zip
toodownload en kopia av hello källkoden klickar du på **hämta ZIP** hello höger på sidan hello. Du kan också [ladda ned från GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Alternativ 2: Källa via Git
tooget hello källkoden för hello SDK via Git, skriver du:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Alternativ 3: Binärfiler via Gradle
Du kan hämta hello binärfiler från hello Maven centrala lagringsplatsen. Hej AAR paketet kan ingå i ditt projekt i Android Studio enligt följande:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Alternativ 4: AAR via Maven
Om du använder hello M2Eclipse plugin-program kan du ange hello beroende i filen pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a>Alternativ 5: JAR-paketet i hello biblioteksmappen
Du kan hämta hello JAR-filen från hello Maven lagringsplatsen och släpp hello **libs** mappen i projektet. Du behöver toocopy hello nödvändiga resurser tooyour projekt, eftersom hello JAR paket inte inkluderas.

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a>Steg 5: Lägg till referenser tooAndroid ADAL tooyour projekt
1. Lägga till en referens tooyour projekt och ange den som en Android-biblioteket. Om du är osäker på hur toodo, du kan få mer information om hello [Android Studio plats](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Lägg till hello projektet beroende för felsökning i dina Projektinställningar.
3. Uppdatera ditt projekt AndroidManifest.xml filen tooinclude:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Skapa en instans av AuthenticationContext på din huvudaktiviteten. hello information om det här anropet är utanför hello i det här avsnittet, men du får en bra start genom att titta på hello [Android Native Client exempel](https://github.com/AzureADSamples/NativeClient-Android). I följande exempel hello, SharedPreferences är hello standard cache och utfärdare är i hello form av `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Kopiera den här koden block toohandle hello slutet av AuthenticationActivity när hello användaren anger autentiseringsuppgifter och tar emot en Auktoriseringskoden:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. tooask för en token definierar ett motanropskontrakt:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Slutligen be om en token med hjälp av att motringning:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Här följer en förklaring av hello parametrar:

* *resursen* krävs och är hello-resurs som du försöker tooaccess.
* *clientid* krävs och kommer från Azure AD.
* *RedirectUri* är inte obligatoriska toobe för hello acquireToken anrop. Du kan konfigurera den som ditt paketnamn.
* *PromptBehavior* hjälper tooask för autentiseringsuppgifter tooskip hello cache och cookie.
* *motringning* anropas efter hello auktoriseringskod byts ut mot en token. Den har ett objekt av AuthenticationResult som har åtkomst-token, datum har upphört att gälla och ID tokeninformation.
* *acquireTokenSilent* är valfritt. Du kan anropa den toohandle cachelagring och uppdatera token. Det ger också hello synkroniserade versionen. Den godkänner *userId* som en parameter.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Du bör ha vad du behöver toosuccessfully integrera med Azure Active Directory med hjälp av den här genomgången. Fler exempel på detta fungerar finns i hello AzureADSamples / databasen på GitHub.

## <a name="important-information"></a>Viktig information
### <a name="customization"></a>Anpassning
Programresurser kan skriva över biblioteksresurser för projektet. Detta händer när din app skapas. Därför kan du anpassa autentisering aktivitet layout hello önskemål. Vara säker på att tookeep hello ID hello kontroller använder som ADAL (Webbvy).

### <a name="broker"></a>Service Broker
hello Microsoft Intunes företagsportalapp innehåller hello broker komponent. hello-konto har skapats i AccountManager. hello kontotypen är ”com.microsoft.workaccount”. AccountManager kan bara ett enda SSO-konto. En SSO-cookie för hello användare skapas när du har slutfört hello enheten utmaning för någon av hello appar.

ADAL använder hello broker konto om ett användarkonto skapas vid denna autentiserare och du väljer att inte tooskip den. Du kan hoppa över hello broker användare med:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Du behöver tooregister särskilda RedirectUri för broker användning. RedirectUri har formatet hello av `msauth://packagename/Base64UrlencodedSignature`. Du kan hämta din RedirectUri för din app med hjälp av hello skriptet brokerRedirectPrint.ps1 eller mContext.getBrokerRedirectUri för hello API-anrop. hello signaturen är relaterade tooyour signera certifikat.

hello aktuella broker modellen är för en användare. AuthenticationContext ger hello API-metoden tooget hello broker användare.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

App-manifest ska ha följande behörigheter toouse AccountManager konton hello. Mer information finns i hello [AccountManager information på webbplatsen för Android hello](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>URL: en utfärdare och AD FS
Active Directory Federation Services (AD FS) identifieras inte som produktion STS, så du behöver tooturn för identifiering av instansen och ange parametern false på hello AuthenticationContext konstruktor.

hello myndigheten URL måste en STS-instans och en [klientnamn](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Fråga cacheobjekt
ADAL tillhandahåller en standard-cache i SharedPreferences med några enkla cache frågan funktioner. Du kan hämta hello aktuell cache från AuthenticationContext med hjälp av:

    ITokenCacheStore cache = mContext.getCache();

Du kan också tillhandahålla implementeringen cachen om du vill toocustomize den.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Fråga beteende
ADAL ger ett alternativ toospecify fråga beteende. PromptBehavior.Auto visar hello Användargränssnittet om hello uppdateringstoken är ogiltig och autentiseringsuppgifter krävs. PromptBehavior.Always hoppa över hello cache användning och Visa alltid hello Användargränssnittet.

### <a name="silent-token-request-from-cache-and-refresh"></a>Tyst tokenbegäran från cache och uppdatera
En tyst tokenbegäran använder inte hello popup-Användargränssnittet och kräver inte en aktivitet. Den returnerar en token från hello cachen om de är tillgängliga. Om hello token har upphört att gälla den här metoden försöker toorefresh den. Om hello uppdateringstoken har upphört att gälla eller inte fungerar, returnerar AuthenticationException.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Du kan också göra en synkronisering anropet med hjälp av den här metoden. Du kan ange null toocallback eller använda acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostik
Dessa är hello primära informationskällor för att diagnostisera problem:

* Undantag
* Logs
* Nätverksspår

Observera att Korrelations-ID: N är central toohello diagnostik i hello-biblioteket. Du kan ange dina Korrelations-ID: N på grundval av per begäran om du vill toocorrelate en ADAL begäran med andra åtgärder i koden. Om du inte anger en Korrelations-ID, att ADAL generera ett slumpmässigt. Alla loggar meddelanden och samtal nätverket kommer sedan stämplad med hello Korrelations-ID Hej självgenererat ID ändringar för varje begäran.

#### <a name="exceptions"></a>Undantag
Undantag är först hello diagnostik. Vi försök tooprovide användbara felmeddelanden. Om du hittar en som inte är användbara du filen ett problem och berätta för oss. Innehåller enheten modellen och SDK-nummer.

#### <a name="logs"></a>Logs
Du kan konfigurera hello biblioteket toogenerate loggmeddelanden som du kan använda toohelp diagnostisera problem. Du kan konfigurera loggning genom att göra hello följande anropa tooconfigure återanrop ADAL som använder toohand av varje loggmeddelande som genereras.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Meddelanden kan skrivas tooa anpassade loggfil som visas i följande kod hello. Tyvärr, det går inte standard för att få loggar från en enhet. Det finns vissa tjänster som kan hjälpa dig med detta. Du kan också Skriv ditt eget, exempelvis skicka hello tooa filserver.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Dessa är hello loggningsnivåer:
* Fel (undantag)
* Varna (varning)
* Info (kännedom)
* Utförlig (Mer information)

Du kan ange hello loggningsnivån så här:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Alla loggmeddelanden skickas toologcat, tillägg tooany anpassad logg återanrop.
Du kan få en loggfil tooa från logcat på följande sätt:

    adb logcat > "C:\logmsg\logfile.txt"

 Mer information om GDB kommandon finns hello [logcat information på webbplatsen för Android hello](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Nätverksspår
Du kan använda olika verktyg toocapture hello HTTP-trafik som genereras av ADAL.  Detta är mest användbart om du är bekant med hello OAuth-protokollet eller om du behöver tooprovide diagnostikinformation tooMicrosoft eller andra supportkanaler.

Fiddler är hello enklaste HTTP spårning verktyg. Använd hello följande länkar tooset den upp toocorrectly post ADAL nätverkstrafik. För ett spårning verktyg som Fiddler eller Charles toobe användbar måste du konfigurera den toorecord okrypterade SSL-trafik.  

> [!NOTE]
> Spår som skapats på det här sättet kan innehålla mycket Privilegierade information, till exempel åtkomsttoken, användarnamn och lösenord. Om du använder Produktionskonton, dela inte de med tredje part. Om du behöver toosupply en trace toosomeone i ordning tooget stöd kan återskapa hello problemet med hjälp av ett tillfälligt konto med användarnamn och lösenord som du inte gör något delning.

* Från hello Telerik webbplats: [ställa in Fiddler för Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Från GitHub: [konfigurera Fiddler regler för ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Dialogrutan läge
Hej acquireToken metoden utan aktivitet stöder Kommandotolken dialogrutan.

### <a name="encryption"></a>Kryptering
ADAL krypterar hello token och lagra i SharedPreferences som standard. Du kan titta på hello StorageHelper toosee hello information. Android introducerade Android Keystore för 4.3 (API-18) säker lagring av privata nycklar. ADAL används för API-18 och högre. Om du vill använda toouse ADAL för lägre SDK-versioner måste tooprovide en hemlig nyckel på AuthenticationSettings.INSTANCE.setSecretKey.

### <a name="oauth2-bearer-challenge"></a>OAuth2 ägar utmaning
Hej AuthenticationParameters klassen innehåller funktioner tooget authorization_uri från hello OAuth2 ägar-anrop.

### <a name="session-cookies-in-webview"></a>Cookies i webbvy
Android webbvy rensas inte sessionscookies när hello appen är stängd. Du kan hantera som med hjälp av den här exempelkoden:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Mer information om cookies finns hello [CookieSyncManager information på webbplatsen för Android hello](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Resursen åsidosättningar
Hej ADAL-biblioteket innehåller engelska strängar för ProgressDialog meddelanden. Ditt program ska skriva över om du vill använda lokaliserade strängar.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>Dialogrutan för NTLM
ADAL version 1.1.0 stöder en dialogruta för NTLM som bearbetas via hello onReceivedHttpAuthRequest händelse från WebViewClient. Du kan anpassa hello layout och strängar för hello dialogruta.

### <a name="cross-app-sso"></a>Enkel inloggning mellan appar
Läs [hur tooenable enkel inloggning mellan appar på Android med hjälp av ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
