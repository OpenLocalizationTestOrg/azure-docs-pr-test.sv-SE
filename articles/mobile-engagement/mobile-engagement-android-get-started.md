---
title: "aaaGet igång med Android-appar Azure Mobile Engagement"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för Android-appar."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Kom igång med Azure Mobile Engagement för Android-appar
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend push-meddelanden toosegmented användare av en Android-App.
Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement. I självstudiekursen skapar du en tom Android-app som samlar in grundläggande data och tar emot push-meddelanden med Google Cloud Messaging (GCM).

## <a name="prerequisites"></a>Krav
Den här kursen krävs hello [Android Developer Tools](https://developer.android.com/sdk/index.html), som innehåller hello Android Studio integrated development environment och hello senaste Android-plattformen.

Det kräver också hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Konfigurera Mobile Engagement för din Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. Du kan skapa en grundläggande app för Android Studio toodemonstrate hello-integration.

hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Skapa ett Android-projekt
1. Starta **Android Studio**, och välj i popup-fönstret hello **starta ett nytt Android Studio-projekt**.

    ![][1]
2. Ange ett namn för appen och en företagsdomän. Anteckna vad du fyller i eftersom du kommer att behöva samma uppgifter senare. Klicka på **Nästa**.

    ![][2]
3. Välj hello formfaktor för mål samt API-nivå och klicka på **nästa**.

   > [!NOTE]
   > Mobile Engagement kräver en API-nivå på minst 10 (Android 2.3.3).
   >
   >

    ![][3]
4. Välj **tom aktivitet** här, vilket är endast hello-skärmen för den här appen och klicka på **nästa**.

    ![][4]
5. Låt hello standardvärdena vara och klicka på **Slutför**.

    ![][5]

Android Studio skapar nu hello demoappen där Mobile Engagement integreras.

### <a name="include-hello-sdk-library-in-your-project"></a>Inkludera hello SDK-biblioteket i ditt projekt
1. Hämta hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).
2. Extrahera hello filen tooa arkivmapp i datorn.
3. Identifiera hello .jar-biblioteket för hello aktuella versionen av detta SDK och kopiera den toohello Urklipp.

      ![][6]
4. Navigera toohello **projekt** avsnittet (1) och klistra in hello .jar i biblioteksmappen hello (2).

      ![][7]
5. tooload hello bibliotek, synkronisera hello projektet.

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a>Ansluta appen tooMobile Engagement-serverdelen med hello anslutningssträngen
1. Kopiera hello följande rader med kod till hello aktivitet skapades (måste endast göras på ett ställe i appen, vanligtvis hello huvudaktiviteten). Den här exempelappen öppna hello MainActivity under src -> main -> java-mappen och Lägg till hello följande:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. Lösa hello referenser genom att trycka på Alt + Retur eller lägga till följande importuttryck hello:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. Gå tillbaka toohello Azure klassiska Portal i din app **anslutningsinformation** och kopiera hello **anslutningssträngen**.

      ![][9]
4. Klistra in den i hello `setConnectionString` parameter, ersätter hello hela strängen som visas i följande kod hello:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Lägga till behörigheter och en tjänstedeklaration
1. Lägg till dessa behörigheter toohello Manifest.xml i ditt projekt omedelbart före eller efter hello `<application>` tagg:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. toodeclare Hej agent-tjänsten, Lägg till denna kod mellan hello `<application>` och `</application>` taggar:

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. Ersätt i hello kod du klistrade in, `"<Your application name>"` i hello etiketten som visas i hello **inställningar** menyn där du kan se tjänster som körs på hello enhet. Du kan lägga till hello ordet ”tjänst” i etiketten t.ex.

### <a name="send-a-screen-toomobile-engagement"></a>Skicka en skärm tooMobile Engagement
toostart skicka data och se till att hello användarna är aktiva, måste du skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen.

Gå för**MainActivity.java** och Lägg till hello följande tooreplace hello basklassen **MainActivity** för**EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> Om din basklass inte *aktiviteten*, se [avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md) för tooinherit från olika klasser.
>
>

Kommentera ut hello följande rad i detta enkla Exempelscenario:

    // setSupportActionBar(toolbar);

Om du vill tookeep hello `ActionBar` i din app Se [avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md).

## <a name="connect-app-with-real-time-monitoring"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Aktivera push-meddelanden och meddelanden i appen
Under en kampanj kan du med hjälp av Mobile Engagement samverka med och nå ut till användarna med push-meddelanden och meddelanden i appen. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello efter avsnittet ställer in din app tooreceive dem.

### <a name="copy-sdk-resources-in-your-project"></a>Kopiera SDK-resurser i ditt projekt
1. Gå tillbaka tooyour SDK ladda ned innehåll och kopierar hello **res** mapp.

    ![][10]
2. Gå tillbaka tooAndroid Studio, Välj hello **huvudsakliga** katalogen för dina projektfiler och klistra in den tooadd hello resurser tooyour projekt.

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Nästa steg
Gå för[Android SDK](mobile-engagement-android-sdk-overview.md) tooget detaljerade kunskaper om hello SDK-integration.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
