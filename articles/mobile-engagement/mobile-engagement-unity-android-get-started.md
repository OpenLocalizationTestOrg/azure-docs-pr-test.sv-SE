---
title: "aaaGet igång med Azure Mobile Engagement för Unity Android-distribution"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Unity-appar som distribueras tooiOS enheter."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Kom igång med Azure Mobile Engagement för Unity Android-distribution
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend skicka meddelanden toosegmented användare i ett Unity-program när du distribuerar tooan Android-enhet.
Den här självstudiekursen använder hello klassiska kursen Unity Roll kulan självstudiekursen som hello som startpunkt. Du bör följa hello stegen i den här [kursen](mobile-engagement-unity-roll-a-ball.md) innan du fortsätter med hello Mobile Engagement-integreringen som visas i hello kursen nedan. 

Den här kursen kräver hello följande:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
### <a name="import-hello-unity-package"></a>Importera hello Unity-paketet
1. Hämta hello [Mobile Engagement Unity-paketet](https://aka.ms/azmeunitysdk) och spara den tooyour lokal dator. 
2. Gå för**tillgångar -> Importera paket -> anpassat paket** och välj hello-paket som du hämtade i hello senare steg. 
   
    ![][70] 
3. Kontrollera att alla filer har markerats och klicka på knappen **Import** (Importera). 
   
    ![][71] 
4. När importen är klar visas hello importerade SDK-filerna i projektet.  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a>Uppdatera hello EngagementConfiguration
1. Öppna hello **EngagementConfiguration** skriptfilen hello SDK-mappen och uppdatera hello **ANDROID\_anslutning\_sträng** med hello anslutningssträng som du hämtade tidigare från hello Azure-portalen.  
   
    ![][73]
2. Spara filen med hello 
3. Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest). Detta är hello plugin-program läggs till av hello Mobile Engagement SDK och klicka på det uppdateras automatiskt dina Projektinställningar. 
   
    ![][74]

> [!IMPORTANT]
> Se till att tooexecute detta varje gång du uppdaterar hello **EngagementConfiguration** filen annars inte visas ändringarna i hello app. 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a>Konfigurera hello appen för grundläggande spårning
1. Öppna hello **PlayerController** skript kopplat toohello Player-objektet för redigering. 
2. Lägg till hello följande med instruktionen:
   
        using Microsoft.Azure.Engagement.Unity;
3. Lägg till följande toohello hello `Start()` metod
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a>Distribuera och köra hello app
Kontrollera att du har Android SDK är installerat på datorn innan du försöker utföra toodeploy Unity app tooyour enheten. 

1. Anslut en Android-enhet tooyour dator. 
2. Öppna **File -> Build Settings** (Arkiv -> Inställningar för byggande). 
   
    ![][40]
3. Välj **Android** och klicka sedan på **Switch Platform** (Växla plattform).
   
    ![][51]
   
    ![][52]
4. Klicka på **Player settings** (Player-inställningar) och ange ett giltigt paket-ID. 
   
    ![][53]
5. Klicka slutligen på **Build And Run** (Bygg och kör).
   
    ![][54]
6. Du tillfrågas tooprovide en mapp namn toostore hello Android-paketet. 
7. Om allt går bra, hello paketet ska vara distribuerade tooyour anslutna bör enheter och du se Unity-spelet på din telefon! 

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a>Uppdatera hello EngagementConfiguration
1. Öppna hello **EngagementConfiguration** skriptfilen hello SDK-mappen och uppdatera hello **ANDROID\_GOOGLE\_nummer** med hello **Google-projekt Antal** du tidigare hämtade från hello Google Cloud Developer-portalen. Detta är en sträng som värde så se till att tooenclose den med dubbla citattecken. 
   
    ![][75]
2. Spara hello-filen. 
3. Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest). Detta är hello plugin-program läggs till av hello Mobile Engagement SDK och klicka på det uppdateras automatiskt dina Projektinställningar. 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a>Konfigurera hello app tooreceive meddelanden
1. Öppna hello **PlayerController** skript kopplat toohello Player-objektet för redigering. 
2. Lägg till följande toohello hello `Start()` metod
   
        EngagementReachAgent.Initialize();
3. Nu när hello appen har uppdaterats, distribuera och köra hello app på en enhet enligt hello anvisningarna nedan. 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
