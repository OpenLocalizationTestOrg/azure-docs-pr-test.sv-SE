---
title: "aaaGet igång med Windows Universal-appar Azure Mobile Engagement"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för universella Windows-appar."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Kom igång med Azure Mobile Engagement för universella Windows-appar
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett universella Windows-program.
Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement. Du skapar en tom universell Windows-app som samlar in grundläggande appanvändningsdata och tar emot push-meddelanden med Windows Notification Service (WNS).

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>Konfigurera Mobile Engagement för din universella Windows-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).

Du kan skapa en grundläggande app med Visual Studio toodemonstrate hello-integration.

### <a name="create-a-windows-universal-app-project"></a>Skapa ett universellt Windows-approjekt
hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio.

1. Starta Visual Studio och i hello **Start** väljer **nytt projekt**.
2. Välj i popup-fönstret hello **Windows** -> **Universal** -> **tom App (Universal Windows)**. Fyll i hello app **namn** och **lösningsnamn**, och klicka sedan på **OK**.

    ![][1]

Nu har du skapat en universell Windows-App-projekt där du sedan integreras hello Azure Mobile Engagement SDK.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Installera hello [MicrosoftAzure.MobileEngagement] Nuget-paket i projektet. Om du utvecklar för både Windows och Windows Phone-plattformar, behöver du toodo detta för båda projekten. För Windows 8.x och Windows Phone 8.1 hello samma Nuget-paketet platser hello rätt plattformsspecifika binärfiler i varje projekt.
2. Öppna **Package.appxmanifest** och se till att hello följande funktion har lagts till:

        Internet (Client)

    ![][2]
3. Nu kopiera hello anslutningssträng som du kopierade tidigare för Mobile Engagement-appen och klistra in den i hello `Resources\EngagementConfiguration.xml` fil mellan hello `<connectionString>` och `</connectionString>` taggar:

    ![][3]

    > [!TIP]
    > Om appen används på både Windows- och Windows Phone-plattformen ska du fortfarande skapa två Mobile Engagement-program – ett för varje plattform som stöds. Med två appar som gör att du kan skapa rätt segmentering av målgruppen hello och kan skicka lämpliga målanpassade meddelanden för varje plattform.

    > [!IMPORTANT]
    > NuGet kopierar inte automatiskt hello SDK-resurser i din Windows 10 UWP-App. Det har toodo manuellt följande hello som visas (viktigt.txt) när hello Nuget-paketet har installerats.  

1. I hello `App.xaml.cs` fil:

    a. Lägg till hello `using` instruktionen:

            using Microsoft.Azure.Engagement;

    b. Lägg till en metod som initierar hello Engagement:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    c. Initiera hello SDK i hello **OnLaunched** metod:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    c. Infoga följande hello i hello **OnActivated** metod och Lägg till hello-metoden om den inte redan finns:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <a id="monitor"></a>Aktivera realtidsövervakning
toostart skicka data och säkerställer att hello användarna är aktiva, måste du skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen.

1. I hello **MainPage.xaml.cs**, Lägg till följande hello `using` instruktionen:

    using Microsoft.Azure.Engagement.Overlay;
2. Ändra hello basklassen **MainPage** från **sidan** för**EngagementPageOverlay**:

        class MainPage : EngagementPageOverlay
3. I hello `MainPage.xaml` fil:

    a. Lägg till tooyour namnområden deklarationer:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. Ersätt hello **sidan** i hello XML-taggnamnet med **engagement: EngagementPageOverlay**

> [!IMPORTANT]
> Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`. Annars rapporteras inte aktiviteten hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod). Detta är särskilt viktigt i ett Windows Phone-projekt där standardmallen för hello har en `OnNavigatedTo` metod.
>
> För **Windows 10 Universal-appar**, använda hello-metod som rekommenderas i hello ”rekommenderad metod: överlagra sida-klasser” avsnittet av [avancerad rapportering med hello Windows Universal-appar Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md) , i stället för hello en nämns ovan.

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract och användarna med push-meddelanden och appmeddelanden i hello kontext kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a>Aktivera din app tooreceive Push-meddelanden med WNS
1. I hello `Package.appxmanifest` -fil hello **programmet** fliken, under **meddelanden**, ange **popup-kompatibel:** för**Ja**

    ![][5]

### <a name="initialize-hello-reach-sdk"></a>Initiera hello REACH SDK
I `App.xaml.cs`, anropa **EngagementReach.Instance.Init(e);** i hello **InitEngagement** direkt efter agentinitieringen hello:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Du är klar toosend ett popup-meddelande. Sedan kontrollerar vi att den grundläggande integrationen har genomförts.

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a>Bevilja åtkomst tooMobile Engagement toosend meddelanden
1. Öppna [Windows Store Dev Center] i webbläsaren, logga in och skapa ett konto om det behövs.
2. Klicka på **instrumentpanelen** hörn hello upp till höger och klicka sedan på **skapar en ny app** hello vänsterpanelen-menyn.

    ![][9]
3. Skapa din app genom att reservera dess namn.

    ![][10]
4. När hello app har skapats, navigera för**tjänster -> Push-meddelanden** hello vänstra menyn.

    ![][11]
5. I hello Push-meddelanden genom att klicka på hello **webbplatsen Live-tjänster** länk.

    ![][12]
6. Du kan navigera toohello Push-autentiseringsuppgifter avsnitt. Kontrollera att du befinner dig i hello **Appinställningar** avsnitt och kopiera ditt **paket-SID** och **klienthemlighet**

    ![][13]
7. Navigera toohello **inställningar** av Mobile Engagement-portalen och klicka på hello **Native Push** avsnittet hello vänster. Klicka på hello **redigera** knappen tooenter din **säkerhetsidentifierare (SID) för paketet** och **hemlig nyckel** som visas:

    ![][6]
8. Kontrollera slutligen att du har associerat Visual Studio-appen med den här skapade appen i hello App store. Klicka på**Associate App with Store** (Associera appen med Store) i Visual Studio.

    ![][7]

## <a id="send"></a>Skicka en avisering tooyour app
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Om hello appen körs visas ett meddelande i appen. Annars om hello appen är stängd visas ett popup-meddelande.
Om du ser en avisering via app men inte ett popup-meddelande, och du kör hello appen i felsökningsläge i Visual Studio, försök **Livscykelhändelser -> Suspend** i hello verktygsfältet tooensure hello appen har pausats. Om du klickade på hello startsideknappen när du felsöker programmet hello i Visual Studio sedan det alltid uppehåll inte och när du ser hello-meddelande via appen, den visas inte som ett popup-meddelande.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows Store Dev Center]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
