---
title: "aaaGet igång med Azure Mobile Engagement för Cordova/Phonegap"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Cordova/Phonegap-appar."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Komma igång med Azure Mobile Engagement för Cordova/Phonegap
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand användnings- och skicka push-meddelanden toosegmented användare av ditt program för mobila program som utvecklats med Cordova.

I den här självstudiekursen skapar vi en tom Cordova-app med Mac och integrerar sedan Mobile Engagement SDK. Den samlar in grundläggande analysdata och tar emot push-meddelanden via Apple Push Notification System (APNS) för iOS och Google Cloud Messaging (GCM) för Android. Vi distribuerar den här tooan iOS eller Android-enhet för testning. 

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).
> 
> 

Den här kursen kräver hello följande:

* XCode som du kan installera från Mac App Store (för att distribuera tooiOS)
* [Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (för att distribuera tooAndroid)
* Certifikat för push-meddelanden (.p12) som kan hämtas från Apple Dev Center för APNS
* Ett GCM-projektnummer som kan hämtas från Google Developer Console för GCM
* [Cordova-pluginprogrammet för Mobile Engagement](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> Du kan hitta hello källkoden och hello viktigt för hello Cordova-pluginprogrammet på [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din Cordova-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration” som hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. 

Vi skapar en grundläggande app i Cordova toodemonstrate hello integrering:

### <a name="create-a-new-cordova-project"></a>Skapa ett nytt Cordova-projekt
1. Starta *Terminal* fönster på din Mac-datorn och Skriv-hello efter som skapar ett nytt Cordova-projekt från hello standardmallen. Kontrollera att hello publicering profil du slutligen Använd toodeploy iOS-appen använder ”com.mycompany.myapp” som hello App-ID. 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. Köra hello följande tooconfigure ditt projekt för **iOS** och kör det i hello iOS-simulatorn:
   
        $ cordova platform add ios 
        $ cordova run ios
3. Köra hello följande tooconfigure ditt projekt för **Android** och kör det i hello Android-emulatorn. Se till att inställningarna för Android SDK Emulator har mål som Google APIs (Google Inc.) med hello CPU / ABI som Google APIs ARM.  
   
        $ cordova platform add android
        $ cordova run android
4. Lägg till hello Cordova Console plugin-programmet. 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Installera hello Azure Mobile Engagement Cordova-pluginprogrammet samtidigt som man hello variabelvärden tooconfigure hello plugin-program:
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android Reach-ikon* : måste vara hello namnet på hello resursen utan filtillägg eller drawable-prefix (exempel: mynotificationicon), och hello ikonfilen måste kopieras till din android-projekt (plattformar/android/res/drawable)

*iOS Reach-ikon* : måste vara hello namnet på hello resursen med filtillägg (exempel: mynotificationicon.png), och hello ikonfilen måste läggas till i ditt iOS-projekt med XCode (Använd hello Lägg till filer menyn)

## <a id="monitor"></a>Aktivera realtidsövervakning
1. Hej Cordova-projekt - redigera **www/js/index.js** tooadd hello anropet tooMobile Engagement toodeclare en ny aktivitet när hello *deviceReady* händelsen har tagits emot.
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. Kör programmet hello:
   
   * **För iOS**
     
       I `Terminal` fönstret starta din app i en ny instans av simulatorn genom att köra hello följande:
     
           cordova run ios
   * **För Android**
     
       I `Terminal` fönstret starta din app i en ny instans av emulatorn genom att köra hello följande:
     
           cordova run android
3. Du kan se hello följande i hello loggarna för konsolen:
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan toointeract med dina användare med hjälp av Push-meddelanden och appmeddelanden i hello kontext kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="configure-push-credentials-for-mobile-engagement"></a>Konfigurera push-autentiseringsuppgifter för Mobile Engagement
tooallow Mobile Engagement toosend Push-meddelanden för din räkning, behöver du toogrant komma åt tooyour Apple iOS-certifikat eller GCM Server API-nyckel. 

1. Navigera tooyour Mobile Engagement-portalen. Kontrollera att du befinner dig i hello app vi använder för det här projektet och klicka sedan på hello **starta** knappen längst ned hello:
   
    ![][1]
2. Du hamnar på hello inställningssidan i Engagement-portalen. Därifrån klickar du på hello **Native Push** avsnitt:
   
    ![][2]
3. Konfigurera iOS-certifikat/API-nyckel för GCM-server
   
    **[iOS]**
   
    a. Välj ditt .p12-certifikat, ladda upp det och ange ditt lösenord:
   
    ![][3]
   
    **[Android]**
   
    a. Klicka på hello redigeringsikonen framför **API-nyckel** hello GCM-inställningar och hello popup-fönster som visas, klistra in hello GCM-servernyckeln och på **OK**. 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a>Aktivera push-meddelanden i hello Cordova-app
Redigera **www/js/index.js** tooadd hello anropet tooMobile Engagement toorequest push-meddelanden och deklarerar en hanterare:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a>Kör hello-appen
**[iOS]**

1. Vi använder XCode toobuild och distribuera hello-appen på hello enheten tootest push-meddelanden eftersom iOS endast tillåter push-meddelanden tooan faktisk enhet. Gå toohello platsen där Cordova-projektet har skapats och navigera för**...\platforms\ios** plats. Öppna filen för hello interna .xcodeproj i XCode. 
2. Skapa och distribuera hello Cordova-app toohello iOS-enhet med hello-konto som har hello etableringsprofil som innehåller hello certifikat du just har överfört toohello Mobile Engagement-portalen och hello App-Id som matchar hello något du angav när du skapar Hej Cordova-app. Du kan checka ut hello *paketidentifierare* i din **resurser\*-info.plist** filen i XCode toomatch upp. 
3. Hello standard iOS popup på enheten om appen hello begär behörighet toosend meddelanden visas. Bevilja behörighet att hello. 

**[Android]**

Du behöver bara använda hello emulatorn toorun hello Android-app som GCM-meddelanden stöds hello Android-emulatorn. 

    cordova run android

## <a id="send"></a>Skicka en avisering tooyour app
Nu ska vi skapa en enkel Push-meddelandekampanj som skickar ett push tooyour app som körs på hello enhet:

1. Navigera toohello **nå** fliken i din Mobile Engagement-portal
2. Klicka på **nytt meddelande** toocreate push-kampanj
   
    ![][6]
3. Ange indata toocreate din kampanj **[Android]**
   
   * Ge kampanjen ett **namn**. 
   * Välj hello **Leveranstyp** som *Systemmeddelande* *enkel*
   * Välj hello **leveranstiden** som *”helst”*
   * Ange en **rubrik** som kommer att vara hello första raden i hello push-meddelande.
   * Ange en **meddelandet** som fungerar som meddelandetexten hello-meddelande. 
     
     ![][11]
4. Ange indata toocreate din kampanj **[iOS]**
   
   * Ge kampanjen ett **namn**. 
   * Välj hello **leveranstiden** som *”endast utanför appen”*
   * Ange en **rubrik** som kommer att vara hello första raden i hello push-meddelande.
   * Ange en **meddelandet** som fungerar som meddelandetexten hello-meddelande. 
     
     ![][12]
5. Bläddra nedåt och i hello innehållsavsnitt väljer **endast meddelande**
   
    ![][8]
6. [Valfritt] Du kan även ange åtgärdens URL-adress. Se till att den använder en URL-schema som hello plugin-programmets **AZME\_OMDIRIGERA\_URL** variabeln t.ex. *myapp://test*.  
7. Du är klar inställningen hello mest grundläggande kampanjen möjligt. Bläddra nedåt igen och klicka på hello **skapa** knappen toosave din kampanj.
8. Slutligen **aktiverar** du din kampanj
   
    ![][10]
9. Du bör nu se ett push-meddelande på enheten eller i emulatorn som en del av den här kampanjen. 

## <a id="next-steps"></a>Nästa steg
[Översikt över alla metoder som är tillgängliga med Cordova Mobile Engagement SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

