---
title: "aaaAzure användargränssnitt för Mobile Engagement - Reach"
description: "Lär dig hur tooreach ut toohello användare på ditt program med push-meddelanden med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a>Hur tooreach ut toohello användare av ditt program med push-meddelanden
Den här artikeln beskriver hello **NÅ** för hello **Mobile Engagement** portal. Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar. Observera att toostart med hello-portalen måste du först toocreate en **Azure Mobile Engagement** konto. Mer information finns i [skapa ett Azure Mobile Engagement-konto](mobile-engagement-create.md).

hello nå avsnitt i Användargränssnittet är hello hello Push kampanj hanteringsverktyg som du kan skapa/redigera/aktivera/Slutför/Övervakare och få statistik på Push notification-kampanjer och funktioner som också kan komma åt via hello nå API (och vissa delar av hello låg nivå Push-API). Kom ihåg att om du använder hello API: er eller hello Användargränssnittet måste toointegrate räckvidd i programmet för varje plattform med hello SDK innan du kan använda och Azure Mobile Engagement nå kampanjer.

> [!NOTE]
> Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen. Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.
> 
> 

## <a name="four-types-of-push-notifications"></a>Fyra typer av Push-meddelanden
1. Meddelanden - Tillåt toosend reklam meddelanden toousers som omdirigerar dem tooanother plats i din app eller toosend dem tooa webbsidan eller lagras utanför din app. 
2. Omröstningar - Tillåt toogather information från slutanvändarna genom att ställa frågor till dem..
3. Data-push - Tillåt toosend en binär fil eller base64-datafil. hello informationen som finns i en data-push skickas tooyour programmet toomodify användarnas aktuella upplevelse i din app. Programmet måste toobe kan tooprocess hello data i en data-push.

## <a name="campaign-details"></a>Information om kampanjer
Du kan redigera, klona, ta bort eller aktivera kampanjer som inte har aktiverats ännu hovrar över deras namn och du kan klicka på tooopen dem. Du kan också klona kampanjer som redan har aktiverats av användaren håller muspekaren över deras namn eller klicka tooopen dem. Du kan inte ändra en kampanj när den väl har aktiverats.

![Reach1][18]

## <a name="reach-feedback"></a>Nå Feedback
Klicka på **statistik** toosee hello information om en Reach-kampanj. Hej **enkel** vyn innehåller en bild i hello form av stapeldiagram kolumn om vad som hänt efter en kampanj har aktiverats. Hej **Avancerat** vyn innehåller mer detaljerad information om hello push-kampanj. Dessa uppgifter är inte tillgängligt om du skickar ett testkampanj d.v.s. en push-skickade tooa testenhet. Här visas hur du ska tolka dessa uppgifter:

1. **Pushas** – detta anger hello antal meddelanden som pushas toohello enheter. Det här antalet beror på hello målgrupp du angav när du skapar hello push-kampanj. Om du inte anger någon målgrupp, skickas den här push ut tooall hello registrerade enheter. Precis som alla andra push-tjänster, vi inte push-meddelanden hello direkt toohello enheter men i stället push-installera dem toohello respektive plattform specifika Push Notification Services (PNS - WNS-APN/GCM) så att de kan tillhandahålla hello meddelanden toohello enheter. 
2. **Leverera** – detta anger hello antal meddelanden som har levererats av hello PNS toohello enhet och bekräftas som tagits emot av Mobile Engagement SDK. 
   
   *Orsaker till levererade antalet är mindre än antalet intryckt:*
   
   1. Om hello användaren har avinstallerats hello app från hello enhet men hello PNS inte känner till den för närvarande hello vi skicka hello push toohello PNS bort hello-meddelande.
   2. Om hello enhet har hello app men hello enheter själva var offline under långa perioder misslyckas hello PNS toodeliver hello meddelandet toohello enhet. 
   3. Om hello-meddelande levereras toohello enhet men hello Mobile Engagement SDK i hello app inte kan identifiera hello innehållet i hello-meddelande, släpper meddelandet. Detta kan inträffa om hello anpassning av hello-meddelande i hello app genererar ett undantag som vi fånga i hello SDK och släpp hello-meddelande. Detta kan också inträffa om hello appen på hello enhet med en version av hello Mobile Engagement SDK som inte kan toounderstand hello nyare version av hello push-meddelande skickas från hello plattform, men detta är endast när hello app uppgraderades efter hello-meddelande skickades från hello service plattform. Hej **Avancerat** fliken talar om hur många meddelanden har tagits bort. 
   4. På iOS-enheter meddelanden ibland inte levereras om antingen hello-enheten är för låg batterinivå eller om hello app förbrukar för mycket energi vid bearbetning av fjärr-meddelanden. Detta är en begränsning av hello iOS-enheter.   
3. **Visas** – detta anger hello antal meddelanden som har visas toohello app användaren på hello-enhet i hello form av en systemavisering push/out-för-app i hello meddelandecentret eller ett meddelande i appen i hello mobila App.  Hej **Avancerat** fliken talar om hur många har systemmeddelanden och hur många har meddelanden i appen. 
   
   *Orsaker till visas antalet är mindre än antalet levererade (väntar toobe visas)*
   
   1. Om hello meddelandekampanj hade ett slutdatum på det är möjligt att hello meddelandet levererades men när hello tid kommer tooopen och visa det toohello app användaren, har den gått så att det har aldrig visas.   
   2. Om hello-meddelande är ett meddelande i appen sedan visas hello-meddelande bara när hello app användare öppnar hello app. I fall där hello app användaren inte har öppnat hello app hello SDK rapporterar att hello-meddelande kunde levereras men har ännu inte visas tills hello appen öppnas. 
   3. Om hello-meddelande är ett meddelande i appen och konfigurerat toobe som visas på en specifik aktivitet/skärm och sedan också hello-meddelande kommer att rapporteras som levereras men ännu inte levereras till öppnar hello användare hello-appen på en viss skärm. 
4. **Användarinteraktioner** – detta anger hello antal meddelanden som hello app användare har har åtgärdat med och ta hälsningsmeddelande som har hanterats eller stängts. 
   
   * *hello app användaren kan åtgärda ett meddelande i något av följande sätt hello:*
     
     1. Om hello meddelande är ett system/out-för-app-meddelande eller ett meddelande i appen skickas som endast meddelande och hello app användaren klickar på hello-meddelande.
     2. Om hello-meddelande är ett meddelande i appen med en text eller en webbvy eller avsökningar hello sedan app användare klickar du på hello åtgärdsknappen i hello-meddelande.
     3. Om hello-meddelande är ett meddelande i appen med en webbvy sedan hello app användaren klickar på en URL i hello webbvy [Android endast]
   * *hello app användaren kan avsluta ett meddelande i något av följande sätt hello:*
     
     1. Klicka hello Stäng i hello-meddelande direkt. 
     2. Svepa in eller ta bort hello-meddelande. 
     3. Meddelanden i appen med text/webbinnehåll och avsökningar är vanligtvis visas toohello app användare i två steg. Ett meddelande visas först och när de klickar på den de se hello efterföljande text/web/avsökning innehåll. hello app användaren kan avsluta ett meddelande i någon av de här stegen och hello information i Avancerat läge för hello samlar in det här. 
5. **Åtgärdade** – detta anger hello antal meddelanden som uttryckligen åtgärdade av hello app användare. Detta är mest intressanta hello-nummer som det här visar hur många användare som appen har intresserad av du push-installeras i hello-meddelande hello-meddelande. 

> [!NOTE]
> På iOS & Windows-plattformar om hello användare har hello app öppna och hello kampanj har kampanj ”när som helst” är det möjligt att både från appen och i appen meddelanden visas på hello samtidigt. Detta kan orsaka en högre än hello levererade visas antalet. Om hello användaren interagerar eller åtgärder hello-meddelande, sedan även hello användaren interaktioner/Actioned antalet kan vara större än levererade. 
> 
> 

![Reach2][19]

## <a name="see-also"></a>Se även
* [Begrepp][Link 6]
* [Felsöka Guide Service][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

