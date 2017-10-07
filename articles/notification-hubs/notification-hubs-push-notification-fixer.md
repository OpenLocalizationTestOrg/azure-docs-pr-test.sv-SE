---
title: "aaaAzure Notification Hubs – diagnos riktlinjer"
description: "Riktlinjer för hur toodiagnose vanliga problem med Azure Notification Hubs."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: e374278f2bfdfad36ba091e8846059cd184c17ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hub - diagnos riktlinjer
## <a name="overview"></a>Översikt
Ett av vanligaste hello frågor som vi får höra från Azure Notification Hubs kunder är hur toofigure reda på varför de ser ett meddelande som skickas från sina serverdelens program visas på klientenheten hello - där och varför meddelanden har tagits bort och hur toofix detta. I den här artikeln lär du dig hello olika orsaker till varför meddelanden kan hämta bort eller inte hamnar på hello-enheter. Vi kommer också titta igenom sätt som du kan analysera och ta reda på orsaken hello. 

Det är först och främst kritiska toounderstand hur Azure Notification Hubs skickar meddelanden toohello enheter.
![][0]

I en typisk skicka meddelande flöde hello-meddelande skickas från hello **programmet backend** för**Azure Notification Hub (NH)** som i sin tur har vissa bearbetning på alla hello registreringar med hänsyn till kontot hello konfigurerad taggar & taggen uttryck toodetermine ”mål” d.v.s. alla hello registreringar som behöver tooreceive hello push-meddelande. Dessa registreringar kan sträcka sig över allt eller något av våra plattformar som stöds – iOS, Google, Windows, Windows Phone Kindle och Baidu för Kina Android. När hello mål upprättas NH sedan push-meddelanden ut meddelanden, dela upp på flera batch registreringar toohello enheten plattformsspecifika **Push Notification Service (PNS)** -t ex APN för Apple, GCM för Google osv. NH autentiserar med hello respektive PNS utifrån hello autentiseringsuppgifter du angett i hello klassiska Azure-portalen på hello konfigurera Meddelandehubben sidan. Hej pns-systemet vidarebefordrar hello meddelanden toohello respektive **klientenheter**. Detta är hello plattform rekommenderat sätt toodeliver push-meddelanden och Observera att hello sista delen av Meddelandeleveransen sker mellan hello plattform PNS och hello enhet. Därför har vi fyra viktiga komponenter - *klienten*, *programmet backend*, *Azure Notification Hubs (NH)* och *Push Notification Services (PNS)*  och någon av dessa kan orsaka meddelanden avbryts. Mer information om den här arkitekturen finns på [översikt över Notification Hubs].

Fel toodeliver meddelanden kan inträffa under inledande hälsning/testning fas som kan tyda på ett problem, eller så kan inträffa i produktionen där alla eller vissa av hello meddelanden kan komma släppas som indikerar ett djupare program eller messaging mönster problemet. Hello under beskrivs nedan olika ignorerade meddelanden scenarier från vanliga toohello sällsynta typ, vilket kan vara uppenbara och vissa andra inte så mycket. 

## <a name="azure-notifications-hub-mis-configuration"></a>Azure felkonfigurerad för Meddelandehubben
Azure Notification Hub måste tooauthenticate sig själv i hello kontexten av hello developer program toobe kan toosuccessfully skicka meddelanden toohello respektive PNS. Detta är möjligt genom hello utvecklare att skapa ett utvecklarkonto med hello respektive plattforms (Google, Apple, Windows osv) och sedan registrera sina program där de kan hämta autentiseringsuppgifter som behöver toobe som konfigurerats i hello portal under varning NAV-konfigurationsavsnittet. Om inga meddelanden gör via första steg bör tooensure att hello rätt autentiseringsuppgifter har konfigurerats på hello Notification Hub matchar dem med programmet hello skapas under deras specifika utvecklarkonto plattform. Du hittar vår [komma igång-självstudierna] användbara toogo via den här processen på ett sätt som steg för steg. Här följer några vanliga konfigurationer för att:

1. **Allmänt**
   
    en) se till att namnet på din meddelandehubb (utan stavfel) är hello samma:
   
   * Om du registrerar hello-klient 
   * Om du skickar meddelanden från hello serverdel  
   * Om du har konfigurerat hello PNS-autentiseringsuppgifter och 
   * Vars SAS-autentiseringsuppgifter som du har konfigurerat på hello klienten och hello backend. 
     
     b) se till att du använder hello rätt SAS configuration strängar på hello klienten och hello programmet backend. Som en tumregel måste du använda hello **DefaultListenSharedAccessSignature** på hello-klienten och **DefaultFullSharedAccessSignature** på hello programmet serverdel (som ger behörighet toobe kan toosend meddelande toohello NH)
2. **Konfiguration för Apple Push-aviseringstjänsten (APNS)**
   
    Du måste upprätthålla två olika hubs – en för produktion och en annan för att testa syfte. Det innebär att du överför hello-certifikat som du kommer toouse i sandbox miljö tooa separat NAV och hello certifikat du ska toouse i produktion tooa separat hubb. Försök inte tooupload olika typer av certifikat toohello samma hubb som det kan orsaka fel för avisering hello rad nedåt. Om du i ett läge där du har överfört olika typer av certifikat toohello oavsiktligt samma hubb rekommenderas toodelete hello NAV- och starta ny. Om du av någon anledning du inte kan toodelete hello hubb vid hello mycket minst, måste du ta bort alla befintliga hello-registreringar från hello-hubben. 
3. **Google Cloud Messaging (GCM) konfiguration** 
   
    en) se till att du aktiverar ”Google Cloud Messaging för Android” under projektet molnet. 
   
    ![][2]
   
    b) se till att du skapar en ”servernyckel” hämta hello autentiseringsuppgifter vilka NH använda tooauthenticate med GCM. 
   
    ![][3]
   
    c) se till att du har konfigurerat ”projekt-ID” på hello-klient som är en helt numeriskt entitet som du kan hämta från hello instrumentpanelen:
   
    ![][1]

## <a name="application-issues"></a>Programproblem
1) **Taggar / tagg uttryck**

Om du använder taggar eller taggen uttryck toosegment målgruppen, är det alltid möjligt när du skickar hello-meddelande, det finns inget mål har hittats baserat på hello taggar/Tagguttryck du anger i send-anrop. Det är bäst tooreview din registreringar tooensure som finns taggar som matchar när du skickar meddelanden och sedan kontrollera hello leveranskvitto enbart från hello klienter med dessa registreringar. T.ex. Om din registreringar med NH har återställts säger taggen ”politik” och du skickar ett meddelande med taggen ”sport”, kommer den inte skickas tooany enhet. Komplexa fallet kan innebära Tagguttryck där du endast registrerade med ”tagg A” eller ”taggen B”, men vid sändning av meddelanden om du utvecklar ”taggen A & & taggen B”. I hello själv diagnosticera tips nedan finns det sätt som du kan granska din registreringar tillsammans med hello-taggar som de har. 

2) **Problem med mallar**

Om du använder mallar och sedan kontrollera att du följer hello riktlinjer som beskrivs i [mall vägledning]. 

3) **Ogiltig registreringar**

Under förutsättning att hello Notification Hub har konfigurerats på rätt sätt och taggar/Tagguttryck användes korrekt ledde hello hitta giltig mål toowhich hello meddelanden behöver toobe skickas, utlöses NH av flera bearbetning batchar parallellt - varje batch skicka meddelanden tooa uppsättning registreringar. 

> [!NOTE]
> Eftersom vi hello bearbetning parallellt, garantera vi inte hello ordning i vilken hello meddelanden levereras. 
> 
> 

Nu har Azure Meddelandehubben optimerats för en ”en gång i de flesta” leveransmodell för meddelandet. Det innebär att vi försöka inaktiveringen duplicering så att inga meddelanden levereras mer än en gång tooa enhet. tooensure detta vi titta igenom hello registreringar och se till att endast ett meddelande skickas per enhetsidentifierare innan du faktiskt skicka hello meddelandet toohello pns-systemet. Eftersom varje batch skickas toohello PNS, som i sin tur tar emot och validerar hello registreringar, är det möjligt att hello PNS upptäcker ett fel med en eller flera av hello registreringar i en batch returnerar ett fel tooAzure NH och avbryter bearbetningen vilket släppa som batch-helt. Detta gäller särskilt med APNS som använder ett protokoll för TCP-ström. Även om vi är optimerade för i de flesta när hello leverans i det här fallet vi ta bort arbetsbördan registrering från vår databas och försök Meddelandeleveransen hello resten av hello enheter i den gruppen.

Du kan hämta information om fel för hello leverans av misslyckade försök mot en registrering med hello Azure Notification Hub REST API: er: [Per meddelande telemetri: hämta meddelanden meddelande telemetri](https://msdn.microsoft.com/library/azure/mt608135.aspx) och [PNS Feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). Se hello [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) exempelkod.

## <a name="pns-issues"></a>PNS-problem
När hello-meddelande har tagits emot av hello respektive PNS är dess ansvar toodeliver hello toohello meddelandeenhet. Azure Notification Hub ligger utanför hello bild och har ingen kontroll när eller om hello-meddelande kommer toobe levereras toohello enhet. Eftersom hello platform notification services är ganska robust, meddelanden tenderar tooreach hello enheter inom några sekunder från hello pns-systemet. Om hello PNS men begränsning är sedan Azure Notification Hubs gäller en exponentiell tillbaka av strategi och om hello PNS förblir kan nås för 30 minuter sedan har vi en princip i placera tooexpire och permanent ta bort meddelandena. 

Om en PNS försöker toodeliver ett meddelande men hello enheten är offline, hello-meddelande lagras av hello PNS under en begränsad tid, och levereras toohello enheten när den blir tillgänglig. Endast en senaste meddelande för en viss app lagras. Om flera meddelanden skickas när hello enheten är offline, gör varje nytt meddelande hello tidigare meddelande toobe ignoreras. Det här beteendet för att hålla endast hello senaste meddelande är refererad tooas mottagarsidan meddelanden i APN och komprimera i GCM (som använder en komprimera nyckel). Hello-enheten är offline under en längre tid, ignoreras de meddelanden som lagrats för den. Käll - [APN vägledning] & [GCM vägledning]

Med Azure Notification Hubs – du kan skicka en buffertsammanslagning nyckel via ett HTTP-huvud med hello generisk `SendNotification` API (t.ex. för .NET SDK – `SendNotificationAsync`) som tar också HTTP-huvuden som skickas är toohello respektive PNS. 

## <a name="self-diagnose-tips"></a>Själv diagnosticera tips
Här undersöker vi hello olika vägar toodiagnose och rot orsaka problemen Meddelandehubben:

### <a name="verify-credentials"></a>Verifiera autentiseringsuppgifter
1. **PNS developer-portalen**
   
    Kontrollera dem på hello respektive PNS developer portal (APN GCM, WNS osv) med hjälp av vår [komma igång-självstudierna].
2. **Klassiska Azure-portalen**
   
    Gå toohello konfigurera fliken tooreview och matchar hello autentiseringsuppgifter med de som erhålls från hello PNS developer-portalen. 
   
    ![][4]

### <a name="verify-registrations"></a>Kontrollera registreringar
1. **Visual Studio**
   
    Om du använder Visual Studio för utveckling kan du ansluta tooMicrosoft Azure och visa och hantera flera Azure-tjänster inklusive Meddelandehubben från ”Server Explorer”. Detta är främst användbart för din miljö för utveckling och testning. 
   
    ![][9]
   
    Du kan visa och hantera alla hello registreringar i din hubb som kategoriseras snyggt för plattformen, intern eller mallen registrering, alla taggar, PNS-identifierare, registrering id och hello upphör att gälla. Du kan också redigera en registrering på hello direkt - vilket är användbart säga om du vill tooedit alla taggar. 
   
    ![][8]
   
   > [!NOTE]
   > Visual Studio funktioner tooedit registreringar bör endast användas under utveckling och testning med begränsat antal registreringar. Om det uppstår ett behöver toofix din registreringar i grupp, Överväg att använda hello Export/Import registrering funktioner som beskrivs här - [Export/Import-registreringar](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Service Bus explorer**
   
    Många kunder använder ServiceBus explorer beskrivs här - [ServiceBus Explorer] för att visa och hantera sina meddelandehubben. Det är ett projekt med öppen källkod från code.microsoft.com - [ServiceBus Explorer-kod]

### <a name="verify-message-notifications"></a>Kontrollera meddelanden
1. **Klassisk Azure-portal**
   
    Du kan gå toohello ”Debug” fliken toosend meddelanden tooyour testklienter utan att behöva alla serverdelar service in och körs. 
   
    ![][7]
2. **Visual Studio**
   
    Du kan också skicka testmeddelanden från hello comforts av Visual Studio:
   
    ![][10]
   
    Du kan läsa mer i hello Visual Studio Notification Hub Azure explorer funktioner här- 
   
   * [Översikt över VS Server Explorer]
   * [VS Server Explorer blogginlägget - 1]
   * [VS Server Explorer blogginlägget - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Felsöka misslyckade meddelanden / granska meddelanderesultat
**Egenskapen EnableTestSend**

När du skickar en avisering via Notification Hubs ursprungligen den bara hämtar i kö för NH toodo bearbetning toofigure ut alla mål och slutligen NH skickas toohello pns-systemet. Det innebär att när du använder REST API eller någon av hello klient-SDK hello lyckas returneras skicka anrop betyder som hello meddelandet har tagits har i kö med Notification Hub. En inblick i vad som hänt när NH slutligen fick toosend hello meddelandet tooPNS får inte. Om ditt meddelande är inte anländer till hello klientenheten finns en möjlighet att när NH försökte toodeliver hello meddelande tooPNS, det gick inte att t.ex. hello nyttolastens storlek överskrider hello tillåtna av hello PNS eller hello autentiseringsuppgifterna som konfigurerats i NH är Ogiltig etc. tooget en inblick i hello PNS fel, har vi introducerades en egenskap som kallas [EnableTestSend funktionen]. Den här egenskapen aktiveras automatiskt när du skickar meddelanden från hello-portalen eller Visual Studio-klienten och därför kan du toosee detaljerad felsökningsinformation. Du kan använda den här via API: er som tar hello exempel på hello .NET SDK där den är tillgänglig nu och kommer att tillagda tooall klient-SDK: förr eller senare. toouse detta med hello REST-anrop, Lägg bara till en querystring parameter med namnet ”test” hello slutet av samtalet skicka t.ex. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Exempel (.NET SDK)*

Anta att du använder .NET SDK toosend ett enhetligt popup-meddelande:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`kommer bara tillstånd `Enqueued` hello slutet av hello körning utan någon inblick i vad hände tooyour push. Nu kan du använda hello `EnableTestSend` boolesk egenskap vid initiering av hello `NotificationHubClient` och kan få detaljerad statusinformation om hello PNS-fel uppstod vid sändning av hello-meddelande. här hello skicka anrop tar extra tid tooreturn eftersom det endast returnerar efter NH har levererats hello tooPNS toodetermine hello meddelanderesultat. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Exempel på utdata*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    hello Token obtained from hello Token Provider is wrong

Det här meddelandet anger ogiltiga autentiseringsuppgifter har konfigurerats i hello meddelandehubben eller ett problem med hello registreringar på hello NAV- och hello rekommenderas kursen skulle toodelete registreringen och låta hello klienten återskapa den innan du skickar hello meddelande. 

> [!NOTE]
> Observera att hello användning av den här egenskapen begränsas kraftigt och så du behöver bara använda detta i miljö för utveckling och testning med begränsad uppsättning registreringar. Vi bara skicka debug meddelanden too10 enheter. Vi har också en gräns för bearbetning av debug skickar toobe 10 per minut. 
> 
> 

### <a name="review-telemetry"></a>Granska telemetri
1. **Använda klassiska Azure-portalen**
   
    hello-portalen kan du tooget en snabb överblick över alla hello aktivitet på Meddelandehubben. 
   
    a) du kan visa en samlad vy av hello registreringar, meddelanden och fel per plattform från fliken för hello ”instrumentpanelen”. 
   
    ![][5]
   
    b) du kan också lägga till många andra plattform specifika mått från hello ”övervakaren” fliken tootake en djupare inblick särskilt på eventuella specifika PNS-fel returneras när NH försöker toosend hello meddelande toohello pns-systemet. 
   
    ![][6]
   
    c) du ska börja med att granska hello **inkommande meddelanden**, **registrering Operations**, **lyckade meddelanden** och gå sedan tooper plattform fliken tooreview hello PNS felen. 
   
    d) om du har ser hello notification hub felkonfigurerat med hello autentiseringsinställningar sedan du PNS-autentiseringsfel. Det här är en bra indikation på toocheck hello PNS-autentiseringsuppgifter. 

2) **Programmeringsåtkomst**

Mer information här- 

* [Programmässiga telemetri åtkomst]
* [Telemetri åtkomst via API: er exempel] 

> [!NOTE]
> Flera telemetri relaterade funktioner som **Export/Import registreringar**, **telemetri åtkomst via API: er** etc är bara tillgängliga i standardnivån. Om du försöker toouse får funktionerna om du är i kostnadsfri eller grundläggande nivån sedan du undantag meddelandet toothis gälla när du använder hello SDK och en HTTP 403 (förbjuden) när du använder dem direkt från hello REST API: er. Kontrollera att du har flyttat in tooStandard skikt via den klassiska Azure-portalen.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[översikt över Notification Hubs]: notification-hubs-push-notification-overview.md
[komma igång-självstudierna]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[mall vägledning]: https://msdn.microsoft.com/library/dn530748.aspx 
[APN vägledning]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM vägledning]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-kod]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Översikt över VS Server Explorer]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Server Explorer blogginlägget - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Server Explorer blogginlägget - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend funktionen]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programmässiga telemetri åtkomst]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetri åtkomst via API: er exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

