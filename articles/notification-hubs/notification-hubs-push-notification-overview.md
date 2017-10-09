---
title: aaaAzure Notification Hubs
description: "Lär dig hur tooadd push notification-funktioner med Azure Notification Hubs."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: 
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 1/17/2017
ms.author: yuaxu
ms.openlocfilehash: 78ce34b6b094b560c8002ab9652f7ba4563c5c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs"></a>Azure Notification Hubs
## <a name="overview"></a>Översikt
Azure Notification Hubs ger dig ett enkelt att använda flera plattformar, skalats ut push-motorn. Med en enda plattformsoberoende API-anrop kan du enkelt skicka riktade och anpassade push-meddelanden tooany mobila plattformar från alla serverdelar för molnet eller lokalt.

Notification Hubs fungerar bra för enterprise-och konsumentscenarier. Här följer några exempel som kunder använder Notification Hubs för:

* Skicka senaste nyheterna meddelanden toomillions med låg latens.
* Skicka platsbaserade kuponger toointerested användarsegment.
* Skicka händelse-relaterade meddelanden toousers eller grupper för media/sport/ekonomi/spelappar.
* Skicka erbjudanden innehållet tooapps tooengage och marknaden toocustomers.
* Meddela användare om företagshändelser, till exempel nya meddelanden och arbetsobjekt.
* Skicka koder för multifaktorautentisering.

## <a name="what-are-push-notifications"></a>Vad är push-meddelanden?
Push-meddelanden är en typ av app-till-användare kommunikation där användare av mobila appar meddelas av vissa önskad information, vanligtvis i ett popup-fönster eller dialogruta. Användare kan vanligtvis välja tooview eller stänga hello-meddelande och välja hello f.d. öppnas hello mobila appar som har kommunicerat hello-meddelande.

Push-meddelanden är mycket viktigt för konsumentappar öka användarna och användning och företagsappar vid kommunikationen uppdaterade affärsinformation. Det finns hello bästa app-till-användare kommunikation eftersom den är energi sparas för mobila enheter, flexibla för hello meddelanden avsändare och tillgänglig när motsvarande appar inte är aktiva.

Mer information om push-meddelanden för några populära plattformar:
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Så här fungerar push-meddelandena
Push-meddelanden levereras via plattformsspecifika infrastrukturer som kallas *plattformsspecifika meddelandesystem* (PNSes). De erbjuder barebone push funktioner toodelivery meddelandet tooa enhet med en angiven hanteras och har inga gemensamt gränssnitt. toosend ett meddelande tooall kunder över hello iOS, Android och Windows versioner av en app hello developer måste arbeta med APNS (Apple Push Notification Service), FCM (Firebase Cloud Messaging) och WNS (Windows Notification Service) när batchbearbetning hello skickar.

På en hög nivå fungerar här så push:

1. hello-klientappen beslutar den vill tooreceive skickar därför kontakter hello motsvarande PNS tooretrieve unikt och tillfällig push referensen. Hej handtagstypen varierar beroende på hello system (t.ex. WNS har URI: er när APN har token).
2. hello-klientappen lagrar detta handtag i hello appens serverdel eller leverantören.
3. toosend ett push-meddelande kontaktar hello appens serverdel hello PNS med hello referensen tootarget en viss klient-app.
4. Hej pns-systemet vidarebefordrar hello toohello meddelandeenhet anges av hello referensen.

![][0]

## <a name="hello-challenges-of-push-notifications"></a>hello utmaningar för Push-meddelanden
PNSes är kraftfulla, lämna de mycket arbete toohello apputvecklaren i ordning tooimplement även vanliga push notification scenarier, till exempel sända eller skicka push-meddelanden toosegmented användare.

Push är en av hello mest beställda funktioner i mobila molntjänster, eftersom dess fungerande kräver komplexa infrastrukturer som orelaterade toohello app huvudsakliga affärslogiken. Här är några hello infrastrukturella utmaningar:

* **Plattformsberoende**: 

  * hello backend måste toohave komplexa och svårt att underhålla plattformsberoende logik toosend meddelanden toodevices på olika plattformar som PNSes inte finns unified.
* **Skala**:

  * Enligt PNS-riktlinjerna måste enhetstoken uppdateras vid varje appen startas. Detta innebär hello backend gäller en stor mängd trafik och databasen åtkomst bara tookeep hello token uppdaterade. När hello antalet enheter växer toohundreds och tusentals miljoner är hello kostnaden för att skapa och underhålla infrastrukturen omfattande.
  * De flesta PNSes stöder inte broadcast toomultiple enheter. Detta innebär en enkel broadcast tooa miljoner enheter resulterar i en miljon anrop toohello PNSes. Den här mängden trafik skalning med minimal svarstid är apputvecklarna.
* **Routning**:
  
  * Även om PNSes ge ett sätt toosend meddelanden toodevices, riktas meddelanden för de flesta appar användare eller intressegrupper. Det innebär att hello backend måste upprätthålla ett register tooassociate enheter med intressegrupper, användare, egenskaper och så vidare. Det här arbetet gör toohello tid toomarket och underhåll kostnaderna för en app.

## <a name="why-use-notification-hubs"></a>Varför bör du använda Notification Hubs?
Notification Hubs eliminerar alla svårigheter som är associerade med att aktivera push din egen. Dess flera plattformar, skalats ut push-infrastruktur minskar push-relaterade koder och förenklar din serverdel. Med Notification Hubs är enheter bara ansvarar för att registrera sina PNS-handtag med ett nav, medan hello backend skickar meddelanden toousers eller intressegrupper, enligt följande bild hello:

![][1]

Meddelandehubbar är färdiga att använda push-motorn med hello följande fördelar:

* **Mellan olika plattformar**

  * Stöd för alla större push-plattformar, inklusive iOS, Android, Windows, och Kindle och Baidu.
  * En gemensam gränssnittet toopush tooall plattformar i plattformsspecifika eller plattformsoberoende format med inget plattformsspecifika arbete.
  * Enheten hantera på ett ställe.
* **Mellan serverdelar**
  
  * Molnet eller lokalt
  * .NET, Node.js, Java och så vidare.
* **Stor uppsättning av leveransmönster**:

  * *Broadcast-tooone eller flera plattformar*: du kan direkt broadcast toomillions enheter på plattformar med ett enda API-anrop.
  * *Push-toodevice*: du kan rikta meddelanden tooindividual enheter.
  * *Push-toouser*: taggar och mallar funktionerna kan du nå alla plattformar enheter av en användare.
  * *Push-toosegment med dynamiska taggar*: taggfunktionen hjälper dig att segmentera enheter och push toothem enligt tooyour behov om du skickar tooone segment eller ett uttryck med segment (t.ex. active och liv i Seattle inte nya användare). I stället för att vara begränsade toopub-sub, kan du uppdatera enheten taggar var som helst och när som helst.
  * *Lokaliserade push*: mallfunktionen kan uppnå lokalisering utan att detta påverkar serverdelskoden.
  * *Tyst push*: gör hello push-pull-mönster genom att skicka tyst meddelanden toodevices och utlösa dem toocomplete vissa hämtar eller åtgärder.
  * *Schemalagda push*: du kan schemalägga toosend ut meddelanden när som helst.
  * *Direct push*: du kan hoppa över registrera enheter med vår tjänst och direkt batch push tooa lista över enhetshandtag.
  * *Anpassade push*: enheten push-variabler kan du skicka enhetsspecifika personliga push-meddelanden med anpassade nyckel-värdepar.
* **Effektiv telemetri**
  
  * Allmän push, enhet, fel och åtgärden telemetri är tillgängliga i hello Azure-portalen och genom programmering.
  * Per meddelande telemetri spårar skickar varje push från din första begäran anropet tooour tjänst har batchbearbetning hello.
  * Platform Notification System Feedback kommunicerar alla feedback från Platform Notification Systems tooassist vid felsökning.
* **Skalbarhet** 
  
  * Skicka snabb meddelanden toomillions enheter utan att behöva bygga om eller enhet horisontell partitionering.
* **Säkerhet**

  * Delad hemlighet av åtkomst (SAS) eller federerad autentisering.

## <a name="integration-with-app-service-mobile-apps"></a>Integrering med Mobilappar i Apptjänst
toofacilitate en sömlös och tillhandahåller konsekvent upplevelse mellan Azure-tjänster, [Apptjänst Mobilappar] har inbyggt stöd för push-meddelanden med Notification Hubs. [Apptjänst Mobilappar] är en mycket skalbar, globalt tillgänglig plattform för utvecklare av företagsprogram och systemintegrerare. den ger en omfattande uppsättning toomobile utvecklare.

Utvecklare av Mobilappar kan utnyttja Notification Hubs med följande arbetsflöde hello:

1. Hämta PNS-handtag för enheter
2. Registrera enheten med Notification Hub via lämplig Mobile Apps klient-SDK registrera API
   * Observera att Mobile Apps av säkerhetsskäl tar bort alla taggar vid registreringen. Arbeta med Notification Hubs från din serverdel direkt tooassociate taggar med enheter.
3. Skicka meddelanden från din apps serverdel med Notification Hubs

Här följer några fördelar tas toodevelopers med den här integreringen:

* **Mobile Apps klient-SDK:**: dessa SDK: er med flera plattformar ger enkla API: er för registrering och tala toohello meddelandehubben länkats med hello mobilappen automatiskt. Utvecklare behöver toodig via Notification Hubs autentiseringsuppgifter inte och arbeta med en ytterligare tjänst.

  * *Push-toouser*: hello SDK: er automatiskt tagga hello anges enheten med Mobile Apps autentiserat användar-ID tooenable push toouser scenario.
  * *Push-toodevice*: hello SDK använder automatiskt hello installations-ID för Mobile Apps som GUID tooregister med Notification Hubs vilket gör att utvecklare hello slipper upprätthålla flera olika GUID-tjänster.
* **Installationsmodell**: Mobile Apps fungerar med Notification Hubs senaste push modellen toorepresent alla push-egenskaper som är associerade med en enhet i en JSON-Installation som överensstämmer med Push Notification Services och är enkelt toouse.
* **Flexibilitet**: utvecklare kan alltid välja toowork direkt med Notification Hubs med hello integration på plats.
* **Integrerad upplevelse i [Azure-portalen]**: Push som en funktion är visuellt återgiven i Mobile Apps och utvecklarna kan enkelt arbeta med hello associerade meddelandehubben via Mobile Apps.

## <a name="next-steps"></a>Nästa steg
Du hittar mer information om Notification Hubs under följande ämnen:

* **[Hur kunder använder Notification Hubs]**
* **[Självstudiekurser och guider om Notification Hubs]**
* **Notification Hubs komma igång Självstudier**: [iOS], [Android], [Windows Universal], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android]

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png
[Hur kunder använder Notification Hubs]: http://azure.microsoft.com/services/notification-hubs
[Självstudiekurser och guider om Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[Apptjänst Mobilappar]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure-portalen]: https://portal.azure.com
[tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
