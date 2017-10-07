---
title: aaaAzure Mobile Engagement demo-appen | Microsoft Docs
description: "Här beskrivs hur toodownload, hur toouse och hello fördelarna med Azure Mobile Engagement demonstration app"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
ms.openlocfilehash: 9ff0df0d21e1bad6aff573db49304a55593df1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement demo-appen
Vi har publicerat en Azure Mobile Engagement demo-appen för **iOS**, **Android**, och **Windows** plattformar toohelp du toofind användbara resurser och lär dig mer om Mobile Engagement.

hello app kan du:

* Enkelt hitta länkar tooMobile Engagement resurser som videor, dokumentation och hello Supportforum och där toogo tooraise funktionen begär.
* Upplevelse exempel meddelanden som stöds av Mobile Engagement tooget idéer för mobila program.
* Använd en referens implementering toostudy hur tooimplement Mobile Engagement i din egen app. Du kan lära dig att:
  
  * Samla in analysdata.
  * Implementera avancerade scenarier för meddelanden av typerna som *mellan enskilda muskler helskärmsläge* eller *popup-*.
  * Implementera undersökningar och avsökningar.
  * Implementera tysta push data och push-scenarier.   

## <a name="app-installation"></a>Installationen av appen
Den här appen finns tillgänglig på följande appbutiker hello:

* **Windows Universal demoappen**:
  
  * Hämta hello app på hello [App för Windows store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
  * hello app utvecklades som en Windows 10 Universal-app. hello källkoden finns på [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).
* **iOS demonstrera app**:
  
  * Hämta hello app på hello [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
  * hello app utvecklades iOS Swift. hello källkoden finns på [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).
* **Android demoappen**:
  
  * Hämta hello app på hello [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
  * hello källkoden finns på [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Universella Windows-demo-appen][1]

![iOS demonstrera app][2]
![Android demo-appen][3]

## <a name="usage"></a>Användning
Du kan använda den här appen i hello följande sätt:

**Hämta hello appen på enheten från hello programmet store länkarna (tidigare):**

> [!IMPORTANT]
> Du behöver ett Azure-konto eller behöver tooconnect hello app tooa serverdelsnätverk. hello appen fungerar oberoende av varandra.
> 
> 

* När du har hello appen på enheten, kan du gå igenom hello länkar i hello vänstra menyn toofind hello användbara resurser om Mobile Engagement.
* Vi har lagt till hello [tjänstens RSS-feed](https://aka.ms/azmerssfeed) i det här programmet så att du alltid är uppdaterad om hello produktuppdateringarna.
* Du kan också gå igenom hello exempel scenarier tooexperience hello meddelandetyp för meddelanden som stöds av Mobile Engagement för varje plattform. Dessa aviseringar kan vara erfarna lokalt – det vill säga du kan klicka på hello knappar på hello skärmar tooshow du hello meddelanden upplevelse, vilket är identiska toosending hello meddelanden från hello Mobile Engagement-plattformen.

![Appmenyn för Windows][4]

![För iOS-appmenyn][5]
![appmenyn för Android][6]

**Hämta hello källkod från hello GitHub länkarna (tidigare):**

* När du har hämtat hello källkod måste du öppna den i hello respektive utvecklingsmiljö--XCode för iOS, Android Studio för Android och Visual Studio för Windows.
* Du bör följa nästa vår [grundläggande SDK-integreringen](mobile-engagement-windows-store-dotnet-get-started.md) så att du kan tooconnect äger den här appen tooits Mobile Engagement backend-instans.
  * Du måste tooconfigure en anslutningssträng i hello app.
  * Du måste också tooconfigure hello push notification-plattformen för din app.
* Lägg märke till att själva appen hello instrumenterats med Mobile Engagement. När du öppnar hello app när du ansluter den toohello serverdel du kommer därför inte kan toosee hello användarens session, aktiviteter, händelser och så vidare på hello **övervakaren** fliken.
* Du måste också vara kan toosend meddelanden toothis app från din egen Mobile Engagement-instans i stället för lokala meddelanden.
  
  * Här du kan lägga till enheten som en testenhet genom att använda hello **Get hello enhets-ID** menyalternativet i hello app. Detta ger dig en enhets-ID som du sedan registrera som en testenhet för din plattform backend-instans.
    
    ![Enhets-ID i Windows][7]
    
    ![Enhets-ID på iOS][8]
    ![enhets-ID på Android][9]

## <a name="key-features-of-hello-demo-app"></a>Viktiga funktioner i hello demo-appen
* Som nämnts tidigare, med den här appen har du alla hello viktiga resurser för Mobile Engagement i din hand. Du kan gå igenom hello länkar på hello vänstra menyn.
* Du kan få ut av appen meddelanden för varje plattform. Dessa meddelanden kan skickas som **endast meddelande**, där om du klickar på hello-meddelande bara öppnas av en inbyggd skärm av programmet hello (med hjälp av **djup länka**)-- eller som en **Web meddelande**, där du kan ge ytterligare HTML-innehåll från hello Mobile Engagement tillbaka avslutas toobe som visas när användaren klickar på hello-meddelande.
  
    ![Out av appen meddelanden][29]
* På iOS har du tooclose hello app toosee hello out app eller system push-meddelanden. Du kan titta på hello implementering här för att lägga till **knappar**, t.ex hello som har lagts till toothis out app notification för *Feedback* och *resursen* (så att hello användaren kan utföra åtgärden direkt från själva hello meddelandet).
  
    ![Out av appen meddelanden på iOS][11] ![Out-appmeddelande visas på iOS][14]
* På Android hello-alternativ som stöds lägger till flera rader text (**stor Text**) eller en avbildning för meddelande (**översikt**) toohello meddelande, tillsammans med hello **knappar** (som stöds av iOS).
  
    ![Out av appen meddelanden på Android][12] ![Out-appmeddelande visas på Android][15]
* På Windows 10, kan du se hur hello meddelanden på hello PC. Det här meddelandet visar även i hello Windows 10 **Meddelandecentret**. Det finns inget stöd för att lägga till **knappar** av hello tidpunkt hello Windows SDK.
  
    ![Out av appen meddelanden i Windows][10] ![Out av programmet visas i Windows][13]
* Du kan uppleva standard ”app” meddelanden för varje plattform. Det här är en tvåstegsverifiering upplevelse där en **meddelande** fönster som visas först. När du klickar på den öppnas helskärm **meddelande**, som visas i följande skärmbild hello. hello innehållet i det här meddelandet kommer från din Mobile Engagement backend-instans. hello SDK har hello mallar för både aviseringar och meddelanden. Du kan enkelt anpassa dem, som visas i den här demon appen med hello lägga till våra logotyp och färgläggning.  
  
    ![Meddelanden i appen i Windows][16]
  
    ![I appen meddelanden på iOS][17]  ![Meddelanden i appen på Android][18]
  
    **iOS**, **Android**
* Du kan också använda hello **kategori** funktion i Mobile Engagement toocustomize standard upplevelsen. Vi har visas två vanliga sätt toochange hello upplevelse av hello meddelanden i hello demo-appen. Observera att hello kategori-funktionen inte stöds ännu i hello Windows SDK.
  
    **Helskärmsläge mellan enskilda muskler:**
  
    ![Avisering via app - mellan enskilda muskler kategori][30]
  
    ![Mellan enskilda muskler kategori på iOS][21]     ![Mellan enskilda muskler kategori på Android][22]
  
    **Popup-meddelande:**
  
    ![Avisering via app - popup-kategori][31]
  
    ![Popup-meddelande på iOS][19]    ![Popup-meddelande på Android][20]

**iOS**, **Android**

* Mobile Engagement stöder också en specialiserad typ av avisering via app kallas **Polls**. Detta gör att du toosend ut snabb undersökningar tooyour segmenterat appanvändare. Du kan lägga till frågor och alternativ för varje fråga som i följande skärmbild hello. Detta kommer sedan hämta visas som ett meddelande i appen toohello app användare.   
  
    ![Avsökningen meddelanden][32]
  
    ![Undersökning i Windows][26]
  
    ![En undersökning på iOS][27]   ![Undersökning på Android][28]

**iOS**, **Android**

* Mobile Engagement även stöd för att skicka tyst **Data Push** meddelanden. Med dessa aviseringar kan du skicka data från din tjänst (till exempel hello JSON-data i följande exempel hello), som du kan hantera i din app och vidta vissa åtgärder. Ett exempel är hur vi ändrar hello priset på en artikel selektivt med hjälp av data-push-meddelanden.
  
    ![Data-push-meddelanden][33]
  
    ![Data-push-meddelanden i Windows][23]
  
    ![Data-push-meddelanden på iOS][24]  ![Data-push-meddelande på Android][25]

**iOS**, **Android**

> [!NOTE]
> Du kan visa detaljerade stegvisa anvisningar för någon av dessa aviseringar genom att klicka på **Klicka här för instruktioner om hur toosend dessa meddelanden från Mobile Engagement-plattformen** på alla exempel anmälan skärm.
> 
> 

[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
