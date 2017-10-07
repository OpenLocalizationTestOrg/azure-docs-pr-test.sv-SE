---
title: "aaaAzure AD Cordova komma igång | Microsoft Docs"
description: "Hur toobuild ett Cordova-program som kan integreras med Azure AD för inloggning och anropar Azure AD-skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrera Azure AD med en Apache Cordova-app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Du kan använda Apache Cordova toodevelop HTML5 eller JavaScript-program som kan köras på mobila enheter som fullständiga program. Du kan lägga till företagsklass autentisering funktioner tooyour Cordova program med Azure Active Directory (AD Azure).

Ett plugin-programmet Cordova radbryts Azure AD plattformsspecifika SDK: er på iOS, Android, Windows Store och Windows Phone. Genom att använda att plugin-program, kan du utöka din programmet toosupport inloggning med Windows Server Active Directory-konton för dina användare kan få åtkomst tooOffice 365 och Azure API: er och även skydda anrop tooyour egna anpassade-webb-API.

I den här självstudiekursen kommer använder vi hello Apache Cordova-pluginprogrammet för Active Directory Authentication Library (ADAL) tooimprove en enkel app genom att lägga till hello följande funktioner:

* Autentisera en användare och få en token med bara några få rader med kod.
* Använder den token tooinvoke hello Graph API tooquery katalogen och visa resultat som hello.  
* Använd hello ADAL token-cache toominimize autentisering efterfrågar hello användare.

toomake dessa förbättringar som du behöver:

1. Registrera ett program med Azure AD.
2. Lägg till kod tooyour toorequest apptoken.
3. Lägg till kod toouse hello token för att fråga efter hello Graph API och visa resultat.
4. Skapa hello Cordova-projekt för distribution med alla hello plattformar du vill tootarget, lägga till hello Cordova ADAL plugin-program och testa hello lösning i emulatorerna.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du:

* En Azure AD-klient där du har ett konto med app development rättigheter.
* En utvecklingsmiljö som har konfigurerats toouse Apache Cordova.  

Om du har redan skapat, fortsätta direkt toostep 1.

Om du inte har en Azure AD-klient kan använda hello [anvisningar om hur tooget en](active-directory-howto-tenant.md).

Om du inte har Apache Cordova ställa in på din dator, installera hello följande:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Cordova CLI](https://cordova.apache.org/) (enkelt kan installeras via NPM Pakethanteraren: `npm install -g cordova`)

hello före installationer bör fungera både på hello dator och på hello Mac.

Varje målplattform har olika förutsättningar:

* toobuild och kör en app för Windows Surfplattors eller Windows Phone:
  * Installera [Visual Studio 2013 för Windows med Update 2 eller senare](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express eller en annan version) eller [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* toobuild och kör en app för iOS:

  * Installera Xcode 6.x eller senare. Ladda ned det från hello [Apple-utvecklare plats](http://developer.apple.com/downloads) eller hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * Installera [ios-sim](https://www.npmjs.org/package/ios-sim). Du kan använda den toostart iOS-appar i iOS-simulatorn hello-kommandoraden. (Du kan installera den via hello terminal: `npm install -g ios-sim`.)
* toobuild och kör en app för Android:

  * Installera [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller senare. Kontrollera att `JAVA_HOME` (miljövariabeln) är korrekt inställd enligt toohello JDK-installationssökvägen (till exempel C:\Program Files\Java\jdk1.7.0_75).
  * Installera [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) och Lägg till hello `<android-sdk-location>\tools` plats (till exempel C:\tools\Android\android-sdk\tools) tooyour `PATH` miljövariabeln.
  * Öppna Android SDK Manager (till exempel via hello terminal: `android`) och installera:
    * *Android 5.0.1 (API 21)* platform SDK
    * *Android SDK Build Tools* version 19.1.0 eller senare
    * *Stöd för Android databasen* (tillägg)

  hello Android SDK ger inte några emulatorn standardinstansen. Skapa en genom att köra `android avd` från hello terminal och sedan välja **skapa**om du vill toorun hello Android-app på en emulator. Vi rekommenderar en API-nivå för 19 eller högre. Läs mer om alternativen för hello Android-emulatorn och skapa [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) på webbplatsen för Android hello.

## <a name="step-1-register-an-application-with-azure-ad"></a>Steg 1: Registrera ett program med Azure AD
Det här steget är valfritt. Den här självstudiekursen innehåller företablerad värden som du kan använda toosee hello exemplet i åtgärden utan att behöva göra några allokering i din egen klient. Vi rekommenderar dock att du utför det här steget och bekanta dig med hello process, eftersom den är obligatorisk när du skapar dina egna program.

Azure AD utfärdar token tooonly kända program. Innan du kan använda Azure AD från din app, behöver du toocreate en post för den i din klient. tooregister ett nytt program i din klient:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ anvisningarna för hello och skapa en **internt klientprogram**. (Men Cordova-appar är HTML-baserad, vi skapar en här native client-program. Hej **internt klientprogram** alternativet måste väljas eller hello program fungerar inte.)
  * **Namnet** beskriver toousers ditt program.
  * **Omdirigerings-URI** är hello URI som har använt tooreturn token tooyour app. Ange **http://MyDirectorySearcherApp**.

När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD. Du behöver det här värdet i hello nästa avsnitt. Du hittar den på hello programmet flik i hello nyskapad app.

toorun `DirSearchClient Sample`, bevilja hello nyligen skapade appen behörighet tooquery hello Azure AD Graph API:

1. Från hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.  
2. Hello Azure Active Directory-program, Välj **Microsoft Graph** som hello API och Lägg till hello **Access hello-katalogen som hello inloggade användare** behörighet under **delegerade Behörigheter**.  Detta gör att dina program tooquery hello Graph API för användare.

## <a name="step-2-clone-hello-sample-app-repository"></a>Steg 2: Klonar hello exempel app lagringsplatsen
Ange hello följande kommando från din gränssnitt eller en kommandorad:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a>Steg 3: Skapa hello Cordova-app
Det finns flera sätt toocreate Cordova program. I den här kursen använder vi hello Cordova-kommandoradsgränssnittet (CLI).

1. Ange hello följande kommando från din gränssnitt eller en kommandorad:

        cordova create DirSearchClient

   Detta kommando skapar hello mappstrukturen och scaffold-teknik för hello Cordova-projekt.

2. Flytta toohello ny DirSearchClient mapp:

        cd .\DirSearchClient

3. Kopiera hello innehållet hello starter Project i hello www undermapp med hjälp av en Filhanteraren eller hello följande kommando i gränssnittet:

  * Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. Lägg till hello godkända plugin-programmet. Detta är nödvändigt för att anropa hello Graph API.

        cordova plugin add cordova-plugin-whitelist

5. Lägg till alla hello plattformar som du vill toosupport. toohave ett exempel på en fungerande måste tooexecute minst en av följande kommandon hello. Observera att du inte kommer att kunna tooemulate iOS i Windows eller emulera Windows på en Mac.

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. Lägg till hello ADAL för Cordova-pluginprogrammet tooyour projekt:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a>Steg 4: Lägga till kod tooauthenticate användare och hämta token från Azure AD
hello-program som du utvecklar i den här kursen får du en enkel directory sökfunktionen. hello-användare kan sedan Skriv hello-alias för alla användare i katalogen, hello och visualisera vissa grundläggande attribut. hello starter projektet innehåller hello definition av hello grundläggande användargränssnitt hello-appen (i www/index.html) och hello scaffold-teknik som dra kablar in grundläggande app händelse cykler användaren gränssnittet bindningar och resultatet visas logik (i www/js/index.js). hello är endast uppgift kvar för du tooadd hello logik som implementerar identitet uppgifter.

hello måste du först toodo i koden är införa hello protokollet värden som Azure AD använder för att identifiera din app och hello resurser som du mål. Dessa värden kommer att använda tooconstruct hello tokenbegäranden vid ett senare tillfälle. Infoga följande fragment hello överst i hello index.js filen hello:

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Hej `redirectUri` och `clientId` värden ska matcha hello värden som beskriver din app i Azure AD. Du hittar dem från hello **konfigurera** fliken i hello Azure-portalen, enligt beskrivningen i steg 1 tidigare i den här kursen.

> [!NOTE]
> Om du valt för att registrera en ny app inte i en egen klient kan du bara klistra in hello fördefinierade värden som är. Sedan visas hello exempel körs, men du bör alltid skapa din egen post för dina appar som är avsedda för produktion.

Lägg till hello tokenbegäran kod. Infoga följande fragment mellan hello hello `search` och `renderData` definitioner:

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
Låt oss nu undersöka som fungerar genom att bryta ned den i sina två delar.
Det här exemplet är utformad toowork med innehavare, som skillnad från toobeing knutna tooa viss en. Den använder hello ”/ vanliga” slutpunkt som tillåter hello användaren tooenter något konto vid autentisering och dirigerar hello begäran toohello klient där den tillhör.

Den här första delen av hello metoden kontrollerar om hello ADAL cache toosee om det finns redan en token. I så fall, använder hello-metoden hello klienter där hello-token kommer från för att ADAL. Detta är nödvändigt tooavoid extra prompter eftersom hello användning av ”/ vanliga” alltid resulterar i ber hello användaren tooenter ett nytt konto.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
hello andra delen av hello metoden utför hello rätt tokenbegäran. Hej `acquireTokenSilentAsync` metoden ber ADAL tooreturn en token för hello angetts resursen utan att visa alla UX. Som kan inträffa om hello cache redan har en lämplig åtkomst-token som lagras, eller om en uppdateringstoken kan användas tooget en ny åtkomsttoken utan visar prompten. Om som försöker misslyckas vi återställs `acquireTokenAsync`--som uppmanas synligt hello användaren tooauthenticate.

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
Nu när vi har hello token kan vi slutligen anropa hello Graph API och utföra hello sökfråga som vi vill. Infoga följande fragment nedan hello hello `authenticate` definition:

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
hello startpunkt filer tillhandahålls en enkel UX för att ange alias för en användare i en textruta. Den här metoden används värdet tooconstruct en fråga, kombinera det med hello åtkomst-token, skickar den tooMicrosoft diagram och parsa hello resultat. Hej `renderData` -metoden finns redan i hello startpunkt filen tar hand om visualisera hello resultat.

## <a name="step-5-run-hello-app"></a>Steg 5: Kör hello-appen
Appen är redo slutligen toorun. Den är enkel: när hello app startar ange hello-alias för hello-användare som du vill att toolook och klicka sedan på hello. Du uppmanas för autentisering. Hello-attribut för hello genomsöks användare visas på lyckad autentisering och lyckad sökning.

Efterföljande körningar utför hello sökning utan att visa prompten, tack vare toohello förekomst av hello tidigare skaffat token i cacheminnet.

hello konkreta stegen för att köra hello app varierar beroende på plattform.

### <a name="windows-10"></a>Windows 10
   Surfplattors:`cordova run windows --archs=x64 -- --appx=uap`

   Mobile (kräver en Windows 10 Mobile-enhet som är ansluten tooa PC):`cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > Under hello kör först, kan du bli ombedd toosign i för utvecklarlicens. Mer information finns i [Developer licens](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1 Surfplattors
   `cordova run windows`

   > [!NOTE]
   > Under hello kör först, kan du bli ombedd toosign i för utvecklarlicens. Mer information finns i [Developer licens](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   toorun på en ansluten enhet:`cordova run windows --device -- --phone`

   toorun på hello standard-emulatorn:`cordova emulate windows -- --phone`

   Använd `cordova run windows --list -- --phone` toosee alla tillgängliga mål och `cordova run windows --target=<target_name> -- --phone` toorun hello program på en viss enhet eller emulator (till exempel `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   toorun på en ansluten enhet:`cordova run android --device`

   toorun på hello standard-emulatorn:`cordova emulate android`

   Kontrollera att du har skapat en instans av emulatorn med AVD Manager, som beskrivits tidigare i hello avsnittet ”förutsättningar”.

   Använd `cordova run android --list` toosee alla tillgängliga mål och `cordova run android --target=<target_name>` toorun hello program på en viss enhet eller emulator (till exempel `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   toorun på en ansluten enhet:`cordova run ios --device`

   toorun på hello standard-emulatorn:`cordova emulate ios`

   > [!NOTE]
   > Kontrollera att du har hello `ios-sim` package installerad toorun på hello-emulatorn. Mer information finns i ”hello” krav ”avsnittet.

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a>Nästa steg
Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).

Du kan nu flytta på toomore avancerade (och mer intressant) scenarier. Du kanske vill tootry: [skydda en Node.js-webb-API med Azure AD](active-directory-devquickstarts-webapi-nodejs.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
