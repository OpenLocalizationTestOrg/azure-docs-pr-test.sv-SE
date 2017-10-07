---
title: "aaaAzure användargränssnitt för Mobile Engagement - nå hur man"
description: "Användargränssnitt, översikt för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4b6dafd09d894214d4c386f5c6f157a77671606f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-using-and-managing-pushes-tooreach-out-tooyour-end-users"></a>Hur tooget igång med att använda och hantera push-meddelanden tooreach ut tooyour slutanvändare
När hello SDK är helt integrerat i din app, kan du börja använda hello hello Reach-avsnittet i hello UI tooPush meddelanden toohello användare i appen.  

## <a name="do-your-first-push-notification-campaign"></a>Gör din första Push-Meddelandekampanj
* Bekräfta att räckhåll är integrerat i din app med hello SDK. 
* Välj ditt program

![First1][1]

* Gå toohello ”Reach” avsnittet och klicka på ”nytt meddelande”

![First2][2]

* Skapa en ny kampanj och ge den namnet
  
![First3][3]

* Välj hur hello-meddelande ska skickas, som i appen bara

![First4][4]

* Skapa hello-meddelande som du vill toopush

![First5][5]

* Du kan skriva en titel på hello-meddelande (valfritt).
* Skriva innehållet för push-meddelandet.
* Du kan ladda upp en bild. Tänk på att hello storleken på hello-filen inte får överstiga 32 768 byte.
* Du har också hello möjlighet tooselect ytterligare alternativ, men för hello fokus för den här självstudiekursen, kommer vi att se som senare.
* Välj hello innehållstyp som meddelande endast

![First6][6]

* Skapa din push-kampanj och kommer att visas i listan kampanj.

![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testa din kampanj med Push-meddelanden
![test1][8]

* Registrera din enhet.
* Klicka på hello kryssrutan för hello enhet du vill toopush.
* Klicka på hello ”Test” knappen toosend hello push toohello enhet.

![Test2][9]

* Aktivera hello kampanj

![test3][10]

* Nu när du har skapat din kampanj behöver du bara tooactivate för hello meddelande toobe pushas tooyour användare.

## <a name="send-personalized-pushes"></a>Skicka push-meddelanden anpassade
* Det här exemplet skapar en distribution där en anpassad rabatt-kod har angetts i hello push-meddelande.

![Personalize1][11]

Anpassning fungerar genom att ersätta en markör av från en app-info tagg så har du toomake till hello användaren har hello rätt app-info definieras först. I det här exemplet hello har utvalda användare en app info-tagg med namnet rebate_code som definierats.
Som du ser ovan innehåller hello push notification-innehållet hello markör ${rebate_code} som visar att det är toobe ersättas med hello själva innehållet hello app-info tagg.

> [!WARNING]
> Om taggen för hello app-info inte har definierats för hello användaren får inte hello användaren hello push.

* Resultat

![Personalize2][12]

### <a name="you-can-further-personalize-hello-text-your-notification"></a>Du kan anpassa text hello-meddelande
![Personalize3][13]

* Inklusive hello rubrik av hello-meddelande
* Och hello innehåll hello-meddelande.
* Välj hello typ för meddelande (textvy eller webbvy)

![Personalize4][14]

### <a name="hello-body-of-an-announcement-may-also-be-personalized-with"></a>hello brödtexten i ett meddelande kan också anpassas med:
* hello åtgärdens URL Om du vill att toocustomize hello landningssida
* hello-rubrik
* hello brödtext hello-meddelande.

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Skilja mellan din Push Notification (i eller utanför appen)
* Välj hello typen av meddelande push, Välj ditt program, gå toohello ”Reach” avsnittet, väljer eller skapar en push-kampanj och gå toohello ”meddelanden” avsnittet.
* Klicka på hello ”leveransläget” som du vill.
* Klicka på hello ”begränsa aktiviteter” kryssrutan om du vill hello-meddelande som uppstår på specifika aktiviteter (skärmar).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>Leveransläget ”endast utanför appen
![Differentiate2][16]

”Endast utanför appen” leveransläge ger push-meddelanden när hello programmet stängs. Detta är hello standard push-meddelande.
När du väljer ”endast utanför appen” måste du har redan angett hello certifikat från hello-plattform som programmet skapar på (APN eller GCM).

### <a name="see-also"></a>Se även
* [Certifikat för Apple Push Notification Service –](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging-certifikat](http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>”i appen bara” leveransläge
![Differentiate3][17]

”I appen bara” leveransläge ger push-meddelanden när hello program körs.
För det här meddelandet behöver inte toogo via hello APN och GCM-system.
Du kan använda hello i appen leverans system tooreach dina slutanvändare.
Du kan helt anpassa hello-meddelande och bestämma i vilken aktivitet (skärm) hello-meddelande som ska visas.

### <a name="anytime-delivery-mode"></a>”När som helst” leveransläge
Du kan välja ett ”när som helst” leveransläge, säkerställer tooreach slutanvändaren hello om programmet körs eller inte.
När du väljer ”när som helst” måste du har redan angett hello certifikat från hello-plattform som programmet skapar (APN eller GCM). 

## <a name="schedule-a-push-campaign"></a>Schemalägga en Push-kampanj
### <a name="plan-toostart-a-campaign"></a>Planera tooStart en kampanj
![Shedule1][18]

Det är hello 21 mars och du har ett meddelande toomake och hyvlat för hello 22nd mars vid midnatt. Du har inte toostay framför hello gränssnittet toodo en push! Du kan planera i förväg hello exakt minut meddelanden ska skickas.

* Avmarkera hello ”None” kryssrutan och välj en starttid 
* Välj hello datum- och hello du vill toostart hello push-kampanj.

### <a name="plan-tooend-a-campaign"></a>Planera tooend en kampanj
![Shedule2][19]

Du vill din kampanj toostop på hello 25 mars 3:00 pm men du vet att du inte kan toodo den.
Du har inte toostay framför hello gränssnittet toopush! Du kan planera i förväg hello exakt minut kampanjen slutar.

* Klicka på hello ”None” kryssrutan eller välj en sluttid
* Välj hello datum- och hello du vill toofinish hello push-kampanj.

### <a name="end-a-campaign-manually"></a>Avsluta en kampanj manuellt
![Shedule3][20]

Hej ”None”-kryssrutorna är markerade som standard.
Detta innebär att hello kampanj startar så fort du aktivera den i hello nå avsnittet och uppnår slutet när du vill sluta på hello avsnitt.

> [!NOTE]
> Kampanjer som har skapats utan ett slutdatum lagra hello push lokalt på hello enhet och visar den hello nästa gång hello appen öppnas även om hello kampanj manuellt avslutas.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Förbättra ett Push-meddelande med en textvy
### <a name="what-is-a-text-view"></a>Vad är en textvy?
![TextView1][21]

En textvy som är ett popup-fönster med textinnehåll. Det här popup-fönstret visas när hello slutanvändaren har klickat på hello push-meddelande.
En textvy kan du toopresent mer innehåll tooyour slutanvändaren. Det är också hello möjlighet toopresent en tooaction anrop, till exempel hoppa tooa sidan för din app, omdirigera tooa Store, öppna en webbsida, e-post, startar en geolokaliserade sökning osv...

### <a name="example-text-view"></a>Exempel: Textvy
* Skapa din Push-meddelandekampanj i hello ”nå” och namnge din kampanj

![TextView2][22]

* Skriv hello-meddelande som ska visas på hello-meddelande.
* Välj hello-meddelande innehållstyp ”text”

![TextView3][23]

> [!NOTE]
> När du överför en textvy kommer alltid med ett meddelande först. 

* Definiera hello text (när du har valt hello för meddelande textinnehåll hello underavsnittet visas så att du toodefine hello text som du vill toobe visas.)

![TextView4][24]

* Skriva hello rubrik som visas överst hello i hello-meddelande.
* Skriva hello huvuddragen av hello textvy.
* Skriv hello innehåll som visas på hello åtgärdsknappen (en med åtgärdsknappen kan hello programmet toomake något, till exempel öppna en sida av programmet hello dirigera tooan App store eller alla typer av datakällor som du kan ange).
* Skriv hello innehåll som visas på hello avsluta-knappen (genom att klicka på hello avsluta-knappen, hello textvy försvinner.)
* Skapa din push-meddelandekampanj och visas i listan för hello kampanj.

![TextView5][25]

* Aktivera push notification kampanj toosend hello text visa tooyour användarna.

![TextView6][26]

* Resultat

![TextView7][27]

* hello användaren får hello-meddelande och klicka på det.
* hello textvy visas som ett popup-vilket gör att hello användaren toointeract med den.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Förbättra ett Push-meddelande med en webbvy
### <a name="what-is-a-web-view"></a>Vad är en webbvy?
![WebView1][28]

En webbvy är ett popup-fönster med webbinnehåll. Det här popup-fönster visas när hello slutanvändaren har klickat på hello push-meddelande.
En webbvy möjliggör toohave mer interaktion med hello slutanvändaren.
Det är också hello möjlighet toopresent en tooaction anrop, till exempel omdirigering av tooApp Store, öppna en webbsida, e-post, startar en geolokaliserade sökning osv...

### <a name="example-web-view"></a>Exempel: Webbvy
* Skapa din Push-kampanj i hello ”nå” och namnge din kampanj.

![WebView2][29]

* Skriv hello-meddelande som ska visas på hello-meddelande.
* Välj hello meddelande innehållstyp som ”web”

![WebView3][30]

### <a name="about-announcement-types"></a>Om meddelandet typer:
* Endast meddelande: det är ett enkelt standard meddelande. Vilket innebär att om en användare klickar på den, ingen extra vy visas, men endast hello åtgärd som är associerade tooit sker.
* SMS-meddelande: det är ett meddelande som snabbt tillkallar hello användaren toohave en titt på textvyn.
* Web meddelande: det är ett meddelande som snabbt tillkallar hello användaren toohave en titt på en webbvy.
  Välj hello ”Web meddelandet” innehåll.

> [!NOTE]
> När du överför en webbvy kommer alltid med ett meddelande först.

* Definiera hello webbinnehåll (när du har valt hello för meddelande webbinnehåll hello avsnitt visas så att du toodefine hello webbinnehåll du vill toobe visas.)

![WebView4][31]

* Skriva hello rubrik som visas överst hello i hello-meddelande (valfritt).
* Skriv in HTML-koden här.
* Klicka på hello läge knappen tooswitch edition för redigering och se hur den ser ut.
* Skriv hello innehåll som visas på hello åtgärdsknappen (en med åtgärdsknappen kan hello programmet toomake något, till exempel öppna en sida av programmet hello omdirigera tooa Store eller alla typer av datakällor som du kan ange).
* Skriv hello innehåll som visas på hello avsluta-knappen (genom att klicka på hello avsluta-knappen, hello webbvy försvinner).
* Resultat

![WebView5][32]

* hello användaren hello-meddelande och klicka på den.
* hello textvy visas som ett popup-vilket gör att hello användaren toointeract med den.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

