---
title: "aaaAzure Mobile Engagement felsökning guider"
description: "Felsökningsguide för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 31134a29-a513-4e5e-b626-f6cf6fe04769
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: dd69bfd7019907c3e1da8df590db3b5f61606173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement, felsökningsguide
## <a name="introduction"></a>Introduktion
hello följande felsökningsguiden hjälper dig att förstå grundorsaken till vissa ofta upptäckta problem och kan aktivera tootroubleshoot på egen hand. 

## <a name="general"></a>Allmänt
I allmänhet bör du alltid kontrollera hello följande:

1. Se till att du har gått igenom alla hello steg som krävs för integration enligt beskrivningen i vår [komma igång Självstudier](mobile-engagement-windows-store-dotnet-get-started.md)
2. Du använder hello senaste versionen av hello plattform SDK. 
3. Testa både på en enhet och en emulator eftersom vissa problem är särskilda tooemulator. 
4. Du inte träffa alla gränser/begränsas från Mobile Engagement som finns dokumenterade [här](../azure-subscription-service-limits.md)
5. Om du inte kan tooconnect toohello Mobile Engagement service backend eller visa data som inte laddas kontinuerligt och sedan kontrollera att det finns ingen pågående service incidenter genom att kontrollera [här](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>Problem med '-Övervakaren
### <a name="i-am-not-seeing-my-device-showing-up-on-hello-monitor-tab"></a>Jag får inte min enhet visas på fliken för övervakning av hello
Övervakaren visar hello enheter anslutna tooyour Mobile Engagement-plattformen i realtid. Om du felsöker på en emulator och enheter, bör du se här minst en session. Om hello appen har distribuerats, ser du hello aktiva sessioner mätaren återspeglar hello-enheter som är anslutna toohello plattform i realtid. 

Om du inte kan se din enhet på fliken för övervakning av hello är det förmodligen ett problem för SDK-integration. Några vanliga steg tootake tootroubleshoot är följande:

1. Kontrollera att du använder rätt anslutningssträng för hello i hello mobilappar och från hello SDK nycklar och inte hello API nycklar avsnitt. hello anslutningssträng ansluter din mobila app toohello instans av hello Mobile Engagement-app som du ser enheten hello övervakaren på fliken. 
2. För Windows-plattformen - om sidan åsidosätter hello `OnNavigatedTo` metod, se till att toocall `base.OnNavigatedTo(e)`.
3. Om du integrerar Mobile Engagement i en befintlig mobila app så att du kan också se till att det inte saknas några steg genom att titta på hello avancerade integreringen [här](mobile-engagement-windows-store-integrate-engagement.md)
4. Se till att minst en skärmaktivitet skickas genom att åsidosätta hello sidan med EngagementActivity beroende på hello-plattform som du arbetar enligt beskrivningen i hello [Kom igång-självstudierna](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-hello-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Jag får hello övervakningsfliken visar en session även när jag har kopplats från eller stängd min app / emulatorn.
Om du är hello endast en ansluten toohello plattform nu och du använder en emulator tooopen hello app är det förmodligen på grund av tooemulator egenskaper. I allmänhet måste tooensure att du kommer tillbaka toohello startsidan på hello emulatorn för hello app session toodisconnect har. Dessutom på Windows-plattformen, när du felsöker med Visual Studio, du kan behöva tooensure i Visual Studio du gå toohello **Livscykelhändelser** menyraden och klicka på **pausa** tooreally Stäng hello-session. Se [Windows kursen](mobile-engagement-windows-store-dotnet-get-started.md) mer information. 

## <a name="analytics-issues"></a>'Analytics-problem
### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Jag ser inte några data / uppdateras data på fliken Analytics
Analysdata beräknas om regelbundet och det kan ta upp till 24 timmar för den här uppdateringen. Dessa data är inte standarddatakällan i realtid och du ser den uppdateras i den här 24-timmarsformat tidsperiod.
Kontrollera att dock att du skickar minst en skärm eller aktivitet toohello plattform backend med antingen övergripande minst en sida med `EngagementActivity` eller anropa `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-hello-analytics-tab"></a>Jag får felaktigt avbildade datum/tid för en enhet på fliken för hello Analytics
hello baseras tidsperiod för Analytics på hello datum från hello användares Enhetsinställningar. Kontrollera att hello enhet har hello datum är korrekt inställd. 

## <a name="segment-issues"></a>'Segment-problem
### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Jag har skapat ett segment och den visas nedtonat eller alla data visas inte
Segment skapas inte realtid hello tillfället. Den beräknas på hello samma tid som hello analysdata sammanställs och så att det kan ta upp till 24 timmar. Du bör Kom tillbaka senare men samtidigt bör du också kontrollera att dina mobilappar verkligen hello data skickas med utgångspunkt från hello som du som utgör hello segment. T.ex. Om en händelse säger ”foo” är inte som skickas av alla mobila enheter och det inte finnas några segmentdata för ett segment som skapats med EventName = foo som hello kriterium. Du bör också kontrollera din SDK integration tooensure din mobila app skickar hello data korrekt. 

## <a name="reach-or-push-notifications-issues"></a>Problem med 'Nå' eller Push-meddelanden
### <a name="my-push-messages-are-not-being-delivered"></a>Push-meddelanden levereras inte
1. Försök att skicka meddelanden tooa test enheten första tooensure att alla hello - mobilappar, SDK och hello service de är anslutna på rätt sätt och kan toodeliver push-meddelanden. 
2. Skicka alltid hello enklaste 'out app notification' först via en kampanj som inte har schemalagts och inte heller har alla målgruppskriterium som angetts. Detta är tooprove meddelande nätverksanslutningen fungerar korrekt. 
3. Om du har steget problem med att leverera meddelanden i appen och sedan är också en bra första tootry skickar en out appmeddelande först. 
4. Se till att hello 'Native Push' är korrekt konfigurerad för din mobila app. Beroende på plattform hello kommer den antingen inkluderar nycklar (Android, Windows) eller certifikat (iOS). Se [användargränssnittet - inställningar](mobile-engagement-user-interface-settings.md)
5. Användaren via hello mobila operativsystem så se till att detta är inte hello fallet utanför appen meddelanden kan också blockeras av hello. 
6. Se till att du inte anger hello *Ignorera målgrupp, push skickas toousers via hello API* alternativ i hello **kampanj** avsnitt i en Reach kampanj eftersom det garanterar att push meddelanden kan endast skickas via API: er. 
7. Se till att du testar din push-kampanj med både en enhet är ansluten via Wi-Fi och phone operatorn nätverket tooeliminate hello nätverksanslutning som en möjlig problem.
8. Se till att de hello tidsvärdet på din emulatorn är korrekt eftersom alla synkroniserad enheter även stör hello Push Notification Services möjlighet toodeliver meddelanden. 

Flera plattformsspecifika felsökning instruktionerna nedan:

1. **iOS** 
   
   * Se till att hello certifikat är giltiga och återstående för iOS Push-meddelanden. 
   * Se till att du korrekt konfigurerar en *produktion* certifikat i Mobile Engagement-appen. 
   * Se till att du testar på en *verkliga fysiska enheten.* hello iOS-simulatorn kan inte bearbeta push-meddelanden.
   * Se till att hello paket-ID är korrekt konfigurerad i hello mobila appar. Se hello instruktioner [här](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
   * När du testar, kan du använda ”Ad Hoc” distribution i din mobila etableringsprofil. Du kommer inte att kunna tooreceive meddelande om din app kompileras med ”Debug”
2. **Android**
   
   * Se till att du har angett rätt hello-projektnummer i din mobila app AndroidManifest.xml-filen som följs av \n tecken. 
     
           <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
   * Se till att du inte är saknas eller felaktigt konfigurerad alla behörigheter i hello Android-manifestfilen 
   * Se till att hello-projektnummer som du lägger till tooyour klientappen från hello samma konto där du har fått hello GCM-servernyckeln. En felaktig matchning mellan hello två förhindras din push-meddelanden från att gå ut. 
   * Om du tar emot system meddelanden, men inte i appen granska hello [ange en ikon för meddelanden](mobile-engagement-android-get-started.md) som troligen inte ange hello rätt ikon i hello Android-manifestfilen. 
   * Om du skickar ett meddelande om BigPicture: Kontrollera att om du har externa bilden servrar och sedan de behöver toobe kan toosupport HTTP ”GET” och ”chef”.
3. **Windows**
   
   * Se till att du har associerat hello app med en giltig Windows Store-app. I Visual Studio – har du tooright klickar hello projektet och väljer alternativet ”associera App med Store” och välj hello-app som du skapade i hello Windows Store. Windows Store-app ska hello samma från där du har fått hello interna push-autentiseringsuppgifter tooconfigure i hello Mobile Engagement-portalen.
   * Om du får ut app push-meddelanden, men inte i appen meddelanden med `EngagementOverlay` integrering sedan se till att det finns ett rotelement i rutnätet på sidan. EngagementOverlay använder hello första ”rutnätet” elementet som påträffas i xaml-filen tooadd två Webbvyer på sidan. Om du vill toolocate där Webbvyer ställs in, kan du definiera ett rutnät med namnet ”EngagementGrid” så här, men du måste tooensure finns tillräcklig höjd och bredd för hello två efterföljande web vyer som ska visa hello-meddelande och hello efter meddelandet som meddelande i appen:
     
           <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-hello-notification-it-is-showing-as-active-what-does-it-mean"></a>Jag har skapat en push-meddelanden/meddelande/kampanj och även när det skickas mig hello-meddelande, det visas som ”aktiv”. Vad innebär det?
Hej **kampanj** som du skapade i Mobile Engagement kallas så eftersom det är en tidskrävande push notification betydelse när nya enheter ansluter tooyour mobil plattform, de skickas automatiskt hello-meddelande du konfigurerar här, förutsatt att de uppfyller hello kriterium som du anger i hello kampanj. Detta är inte en en som enda Meddelandeinställningar. Du måste klicka på hello toomanually **Slutför** knappen tooterminate hello kampanjen så att det inte att skicka fler meddelanden. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-hello-app-i-get-hello-same-notification-even-when-i-had-actioned-it-before"></a>Jag har skapat en push-kampanj och jag kan ta emot meddelanden har dock när jag öppnar upp hello app visas hello samma meddelande även när jag hade åtgärdade det innan?
Detta är troligen toohappen under testningen och om du använder emulatorerna eller vissa Testramverk som TestFlight. Vad som händer här är att på varje app kör instans, den förvärva nya DeviceID och skicka det tooour backend som orsakar hello Mobile Engagement-plattformen tootreat den som en ny enhet och skicka hello-meddelande. 

## <a name="getting-support"></a>Få Support
Om du tooresolve hello problemet själv kan du:

1. Sök efter problemet i hello befintliga trådar på StackOverflow forum och [MSDN-forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) och om inte sedan ställa en fråga det. 
2. Om du hittar en funktion som saknas sedan lägga till/rösten för hello begäran på vår [UserVoice-forum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Om du har Microsoft stöder öppna en supportincident genom att tillhandahålla hello följande information: 
   * Azure prenumerations-ID
   * Plattform (t.ex. iOS, Android osv)
   * App-ID
   * Kampanj-ID (för problem med push notification)
   * Enhets-ID
   * Mobile Engagement SDK-version (t.ex. Android SDK v2.1.0)
   * Information om fel med felmeddelandet och scenario

