---
title: aaaSending push-meddelanden med Azure Notification Hub och Node.js
description: "Lär dig hur toouse Meddelandehubbar toosend push-meddelanden från ett Node.js-program."
keywords: push-meddelande, push notifications,node.js push ios push
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Skicka push-meddelanden med Azure Notification Hubs och Node.js
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Översikt
> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

Den här guiden visar hur toosend push-meddelanden med hjälp av hello i Azure Notification Hubs direkt från ett Node.js-program. 

hello scenarier beskrivs skickar push-meddelanden tooapplications på hello följande plattformar:

* Android
* iOS
* Windows Phone
* Universella Windows-plattformen 

Mer information om notification hubs finns hello [nästa steg](#next) avsnitt.

## <a name="what-are-notification-hubs"></a>Vad är Notification Hub?
Azure Notification Hubs innehåller en lätt att använda flera plattformar, skalbar infrastruktur för att skicka push-meddelanden toomobile enheter. Mer information om hello-infrastruktur, finns hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sidan.

## <a name="create-a-nodejs-application"></a>Skapa en Node.js-program
hello första steget i den här kursen är att skapa en ny tom Node.js-program. Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera en Node.js-programmet tooAzure webbplats][nodejswebsite], [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell eller [webbplatsen med WebMatrix].

## <a name="configure-your-application-toouse-notification-hubs"></a>Konfigurera ditt program tooUse Notification Hubs
toouse Azure Notification Hubs, behöver du toodownload och Använd hello Node.js [azure-paketet](https://www.npmjs.com/package/azure), som innehåller en inbyggd grupp helper bibliotek som kommunicerar med hello push notification REST services.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Använd noden Package Manager (NPM) tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Linux) och gå toohello mappen där du skapade ditt tomt program .
2. Typen **npm installera azure-sb** i hello kommandofönstret.
3. Du kan köra hello manuellt **ls** eller **dir** kommandot tooverify som en **nod\_moduler** mappen har skapats. Hitta hello inuti den mappen **azure** paket som innehåller hello bibliotek som du behöver tooaccess hello Notification Hub.

> [!NOTE]
> Du kan lära dig mer om att installera NPM på hello officiella [NPM blogg](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-hello-module"></a>Importera hello-modul
Med hjälp av en textredigerare, Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Skapa en anslutning med Azure Notification Hub
Hej **NotificationHubService** objekt kan du arbeta med notification hubs. hello följande kod skapar en **NotificationHubService** objekt för hello nofication hubb med namnet **hubname**. Lägg till den hello övre delen av hello **server.js** filen efter hello instruktionen tooimport hello azure modulen:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Hej anslutning **connectionstring** värde kan hämtas från hello [Azure Portal] genom att utföra hello följande steg:

1. Hello vänstra navigeringsfönstret, klicka på **Bläddra**.
2. Välj **Meddelandehubbar**, och sedan hitta hello hubb gärna toouse för hello exemplet. Du kan se toohello [Windows Store komma igång-kursen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) om du behöver hjälp med att skapa en ny Meddelandehubb.
3. Välj **inställningar**.
4. Klicka på **åtkomstprinciper**. Båda anslutningssträngar för delade och fullständig åtkomst visas.

![Azure Portal – Notification hub](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Du kan också hämta hello anslutningssträngen med hello **Get-AzureSbNamespace** cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello **azure sb namnområde visa** med Hej [Azure-kommandoradsgränssnittet (Azure CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Allmän arkitektur
Hej **NotificationHubService** objekt visar hello följande objektinstanser för att skicka push-meddelanden toospecific enheter och program:

* **Android** -Använd hello **GcmService** -objekt som finns på **notificationHubService.gcm**
* **iOS** -Använd hello **ApnsService** -objekt som är tillgängliga i **notificationHubService.apns**
* **Windows Phone** -Använd hello **MpnsService** -objekt som finns på **notificationHubService.mpns**
* **Uwp** -Använd hello **WnsService** -objekt som finns på **notificationHubService.wns**

### <a name="how-to-send-push-notifications-tooandroid-applications"></a>Så här: skicka push-meddelanden tooAndroid program
Hej **GcmService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooAndroid program. Hej **skicka** metoden godkänner hello följande parametrar:

* **Taggar** -hello taggen identifierare. Om ingen tagg skickas hello-meddelande tooall klienter.
* **Nyttolasten** -hello-meddelande JSON eller raw sträng nyttolast.
* **Motringning** -hello Återanropsfunktionen.

Finns mer information om hello nyttolastformatet hello **nyttolast** avsnitt i hello [implementera GCM-Server](http://developer.android.com/google/gcm/server.html#payload) dokumentet.

hello följande kod använder hello **GcmService** instans som exponeras av hello **NotificationHubService** toosend tooall en push-meddelande registrerade klienter.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-tooios-applications"></a>Så här: skicka push-meddelanden tooiOS program
Samma som beskrivs ovan, Android-program hello **ApnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooiOS program. Hej **skicka** metoden godkänner hello följande parametrar:

* **Taggar** -hello taggen identifierare. Om ingen tagg skickas hello-meddelande tooall klienter.
* **Nyttolasten** – hello-meddelande JSON eller string nyttolast.
* **Motringning** -hello Återanropsfunktionen.

Mer information hello nyttolastformat, finns hello **meddelande nyttolast** avsnitt i hello [lokala Programmeringsguide och Push Notification](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentet.

hello följande kod använder hello **ApnsService** instans som exponeras av hello **NotificationHubService** toosend en avisering visas tooall klienter:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a>Så här: skicka push-meddelanden tooWindows Phone-program
Hej **MpnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooWindows Phone-program. Hej **skicka** metoden godkänner hello följande parametrar:

* **Taggar** -hello taggen identifierare. Om ingen tagg skickas hello-meddelande tooall klienter.
* **Nyttolasten** -hello-meddelande nyttolast för XML.
* **Målnamn**  -  `toast` för popup-meddelanden. `token`för sida vid sida-meddelanden.
* **NotificationClass** -hello prioritet av hello-meddelande. Se hello **HTTP-huvudet element** avsnitt i hello [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) dokument för giltiga värden.
* **Alternativ** – valfritt begärandehuvuden.
* **Motringning** -hello Återanropsfunktionen.

En lista över giltiga **TargetName**, **NotificationClass** och Rubrikalternativ, kolla hello [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) sidan.

Följande exempelkod hello använder hello **MpnsService** instans som exponeras av hello **NotificationHubService** toosend en push-meddelandensom:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a>Så här: skicka push-meddelanden tooUniversal Windowsplattformen (UWP) program
Hej **WnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooUniversal Windows Platform program.  Hej **skicka** metoden godkänner hello följande parametrar:

* **Taggar** -hello taggen identifierare. Om ingen tagg skickas hello-meddelande tooall registrerade klienter.
* **Nyttolasten** -hello nyttolast för XML-meddelande.
* **Typen** -hello meddelandetyp.
* **Alternativ** – valfritt begärandehuvuden.
* **Motringning** -hello Återanropsfunktionen.

En lista över giltiga typer och begärandehuvuden finns [Push notification service begärande- och svarshuvuden](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

hello följande kod använder hello **WnsService** instans som exponeras av hello **NotificationHubService** toosend en popup push notification tooa UWP-appen:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Nästa steg
hello exempel kodavsnitt ovan kan du tooeasily build service infrastruktur toodeliver push-meddelanden tooa flera olika enheter. Nu när du har lärt dig grunderna hello i Notification Hubs med node.js, följa dessa länkar toolearn mer om hur du kan utöka dessa ytterligare funktioner.

* Se hello MSDN-referens för [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Besök hello [Azure SDK för noden] lagringsplats på GitHub för fler exempel och implementeringsdetaljer.

[Azure SDK för noden]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[webbplatsen med WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
