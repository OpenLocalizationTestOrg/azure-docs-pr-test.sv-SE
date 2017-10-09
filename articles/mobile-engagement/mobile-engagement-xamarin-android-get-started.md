---
title: "aaaGet igång med Azure Mobile Engagement för Xamarin.Android"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Xamarin.Android-appar."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 9d584fea8e8153d511258cf9b6f87f31dac6aeca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Kom igång med Azure Mobile Engagement för Xamarin.Android-appar
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend push-meddelanden toosegmented användare i ett Xamarin.Android-program.
Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement. I kursen får du skapa en tom Xamarin.Android-app som samlar in grundläggande data och tar emot push-meddelanden med hjälp av Google Cloud Messaging (GCM).

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Den här kursen kräver hello följande:

* [Xamarin Studio](http://xamarin.com/studio). Du kan även använda Visual Studio med Xamarin men i den här kursen används Xamarin Studio. Det finns installationsanvisningar i avsnittet om [konfiguration och installation för Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. 

Vi skapar en grundläggande app i Xamarin Studio toodemonstrate hello-integration.

### <a name="create-a-new-xamarinandroid-project"></a>Skapa ett nytt Xamarin.Android-projekt
1. Starta **Xamarin Studio** gå för**filen** -> **ny** -> **lösning** 
   
    ![][1]
2. Välj **Android-App** Kontrollera hello valt språk är **C#** och på **nästa**.
   
    ![][2]
3. Fyll i hello **Appnamn** och hello **organisations-ID**. Se till att toocheckmark **Google Play Services** och klicka sedan på **nästa**. 
   
    ![][3]
4. Uppdatera hello **projektnamn**, **lösningsnamn** och **plats** om krävs och klicka på **skapa**.
   
    ![][4]

Xamarin Studio skapar hello appen där Mobile Engagement ska integreras. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Högerklicka på hello **paket** mapp i hello Lösningsfönstret och välj **lägga till paket...**
   
    ![][5]
2. Sök efter hello **Microsoft Azure Mobile Engagement Xamarin SDK** och lägga till den tooyour lösning.  
   
    ![][6]
3. Öppna **MainActivity.cs** och Lägg till följande hello med-uttryck:
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. I hello `OnCreate` metoden Lägg till hello följande tooinitialize hello anslutningen till Mobile Engagement-serverdelen. Se till att tooadd din **ConnectionString**. 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a>Lägga till behörigheter och en tjänstedeklaration
1. Öppna hello **Manifest.xml** filen hello egenskaper i mappen. Välj fliken källa så att du direkt uppdatera hello XML-källa.
2. Lägg till dessa behörigheter toohello Manifest.xml (som finns under hello **egenskaper** mapp) i ditt projekt omedelbart före eller efter hello `<application>` tagg:
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. Lägg till följande hello mellan hello `<application>` och `</application>` taggar toodeclare hello agent-tjänsten:
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. Ersätt i hello kod inklistrade, `"<Your application name>"` i hello etikett. Detta visas i hello **inställningar** menyn där användare kan se vilka tjänster som körs på hello enhet. Du kan lägga till hello ordet ”tjänst” i etiketten t.ex.

### <a name="send-a-screen-toomobile-engagement"></a>Skicka en skärm tooMobile Engagement
Du måste skicka minst en skärm toohello Mobile Engagement-serverdelen i ordning toostart skicka data och säkerställa hello användarna är aktiva. För att göra detta – se till att hello `MainActivity` ärver från `EngagementActivity` i stället för `Activity`.

    public class MainActivity : EngagementActivity

Som alternativ, om du inte kan ärva från `EngagementActivity`, måste du lägga till `.StartActivity`- och `.EndActivity`-metoder i `OnResume` respektive `OnPause`.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract med användarna, och med push-meddelanden och appmeddelanden i hello kontext kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
följande avsnitt hello ställer in din app tooreceive dem.

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
