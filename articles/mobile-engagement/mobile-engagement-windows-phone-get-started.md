---
title: "aaaGet igång med Azure Mobile Engagement för Windows Phone Silverlight-appar"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för Windows Phone Silverlight-appar."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Kom igång med Azure Mobile Engagement för Windows Phone Silverlight-appar
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett Windows Phone Silverlight-program.
Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement. Du får skapa en tom Windows Phone Silverlight-app som samlar in grundläggande data och tar emot push-meddelanden med Microsoft Push Notification Service (MPNS).

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

> [!NOTE]
> Windows Phone 8.1 och projekt i tidigare versioner stöds inte i Visual Studio 2017.  Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).

> [!NOTE]
> Om du utvecklar för Windows Phone 8.1 (utan Silverlight), se toohello [universella Windows-kursen](mobile-engagement-windows-store-dotnet-get-started.md).
> 
> 

Den här kursen kräver hello följande:

* Visual Studio 2013
* [MicrosoftAzure.MobileEngagement] NuGet-paket

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din Windows Phone-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)

Vi skapar en grundläggande app i Visual Studio toodemonstrate hello-integration.

### <a name="create-a-new-windows-phone-silverlight-project"></a>Skapa ett nytt Windows Phone Silverlight-projekt
hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio. 

1. Starta Visual Studio och i hello **Start** väljer **nytt projekt**.
2. Välj i popup-fönstret hello **Windows 8** -> **Windows Phone** -> **tom App (Windows Phone Silverlight)**. Fyll i hello app **namn** och **lösningsnamn**, och klicka sedan på **OK**.
   
    ![][1]
3. Du kan antingen välja tootarget **Windows Phone 8.0** eller **Windows Phone 8.1**.

Du har nu skapat en ny Windows Phone Silverlight-app som vi integrerar hello Azure Mobile Engagement SDK.

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Ansluta appen toohello Mobile Engagement-serverdelen
1. Installera hello [MicrosoftAzure.MobileEngagement] nuget-paket i projektet.
2. Öppna `WMAppManifest.xml` (under egenskapsmappen hello) och kontrollera att följande hello har angetts (Lägg till dem om de inte) i hello `<Capabilities />` tagg:
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. Nu klistra in hello anslutningssträng som du kopierade tidigare för Mobile Engagement-appen och klistra in den i hello `Resources\EngagementConfiguration.xml` fil mellan hello `<connectionString>` och `</connectionString>` taggar:
   
    ![][3]
4. I hello `App.xaml.cs` fil:
   
    a. Lägg till hello `using` instruktionen:
   
            using Microsoft.Azure.Engagement;
   
    b. Initiera hello SDK i hello `Application_Launching` metoden:
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    c. Infoga följande hello i hello `Application_Activated`:
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <a id="monitor"></a>Aktivera realtidsövervakning
Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.

1. Lägga till hello hello MainPage.xaml.cs `using` instruktionen:
   
        using Microsoft.Azure.Engagement;
2. Ersätt hello basklassen **MainPage**, som kommer före **PhoneApplicationPage**, med **EngagementPage**.
   
        class MainPage : EngagementPage 
3. I filen `MainPage.xml`:
   
    a. Lägg till tooyour namnområden deklarationer:
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    b. Ersätt `phone:PhoneApplicationPage` i hello XML-taggnamnet med `engagement:EngagementPage`.

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract och användarna med Push-meddelanden och meddelanden i appen hello kontexten för kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a>Aktivera din app tooreceive Push-meddelanden med MPNS
Lägga till nya funktioner tooyour `WMAppManifest.xml` fil:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a>Initiera hello REACH SDK
1. I `App.xaml.cs`, anropa `EngagementReach.Instance.Init();` i hello **Application_Launching** funktion, direkt efter agentinitieringen hello:
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. I `App.xaml.cs`, anropa `EngagementReach.Instance.OnActivated(e);` i hello **Application_Activated** funktion, direkt efter agentinitieringen hello:
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Då var allt klart. Nu är det dags att kontrollera att den grundläggande integrationen har genomförts.

## <a id="send"></a>Skicka en avisering tooyour app
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Du bör nu se ett meddelande på enheten som visas som en avisering via app om hello appen är öppen, annars som ett popup-meddelande som hello nedan: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
