---
title: "Azure Notification Hubs: Vanliga frågor (FAQ) | Microsoft Docs"
description: "Vanliga frågor och svar om att designa/implementera lösningar på Notification Hubs"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: "push-meddelande, push-meddelanden, push-meddelanden för iOS, android push-meddelanden, push ios, android push"
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.openlocfilehash: a70efa7fc5954966847d5a173e9b10accf4b737e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Push-meddelanden med Azure Notification Hubs: vanliga frågor och svar
## <a name="general"></a>Allmänt
### <a name="what-is-hello-resource-structure-of-notification-hubs"></a>Vad är hello resurs strukturen för Meddelandehubbar?

Azure Notification Hub har två nivåer för resursen: NAV och namnområden. En hub är en enkel push-resurs som kan innehålla hello plattformsoberoende push-information för en app. En namnrymd är en samling av NAV i en region.

Rekommenderade mappning matchar ett namnområde med en app. Du kan ha en produktions-hubb som fungerar med din produktionsapp, en testnings-hubb som fungerar med din testning app och så vidare i ett namnområde.

### <a name="what-is-hello-price-model-for-notification-hubs"></a>Vad är hello Prismodell för Meddelandehubbar?
hello senaste prisinformation finns på hello [Notification Hub-priser] sidan. Notification Hubs faktureras på hello namnområde nivå. (Hello definition av en namnrymd, finns i ”vad är hello resurs strukturen för Notification Hubs”?) Notification Hubs ger tre nivåer:

* **Ledigt**: den här nivån är en bra utgångspunkt för att utforska push-funktioner. Det rekommenderas inte för produktion appar. Du får 500 enheter och 1 miljon push-meddelanden med per namnområde per månad, med ingen garanti för tjänsten servicenivåavtal (SLA).
* **Grundläggande**: den här nivån (eller hello standardnivån) rekommenderas för mindre produktion appar. Du får 200 000 enheter och 10 miljoner push-meddelanden med per namnområde per månad som utgångspunkt. Kvoten ingår tillväxt.
* **Standard**: den här nivån rekommenderas för medelstora toolarge produktion appar. Du får 10 miljoner enheter och 10 miljoner push-meddelanden med per namnområde per månad som utgångspunkt. Kvoten öka alternativ och omfattande telemetri funktioner ingår.

Standardnivån funktioner:
* **Effektiv telemetri**: du kan använda Notification Hubs Per meddelande telemetri tootrack någon skickar begäranden och Platform Notification System Feedback för felsökning.
* **Multitenancy**: du kan arbeta med Platform Notification System autentiseringsuppgifter på en nivå i namnområdet. Det här alternativet kan du tooeasily dela hyresgäster i hubbar inom hello samma namnområde.
* **Schemalagda push**: du kan schemalägga meddelanden toobe skickas när som helst.

### <a name="what-is-hello-notification-hubs-sla"></a>Vad är hello Notification Hubs SERVICENIVÅAVTAL?
För nivåerna Basic och Standard Notification Hubs, korrekt konfigurerade program skicka push-meddelanden eller utföra hanteringsåtgärder för registrering minst 99,9% hello tid. Mer om hello SLA, gå toohello toolearn [Notification Hubs SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) sidan.

> [!NOTE]
> Eftersom push-meddelanden är beroende av tredje parts plattformsspecifika meddelandesystem (till exempel Apple APNS och Google FCM), finns det inga SLA-garantin för hello leverans av meddelanden. När Meddelandehubbar skickar hello batchar tooPlatform meddelandesystem (SLA garanteras), är det hello ansvar hello plattformsspecifika meddelandesystem toodeliver hello push-meddelanden (ingen SLA garanteras).

### <a name="which-customers-are-using-notification-hubs"></a>Som kunder använder Notification Hubs?
Många kunder använder Notification Hubs. Vissa viktiga som finns här:

* Sochi 2014: Hundratals intressegrupper, 3 + miljoner enheter och 150 + miljoner meddelanden som skickas i två veckor. [Fallstudie: Sochi]
* Skanska: [Fallstudie: Skanska]
* Seattle tidpunkter: [Fallstudie: Seattle gånger]
* Mural.LY: [Fallstudie: Mural.ly]
* 7Digital: [Fallstudie: 7Digital]
* Bing-appar: Tiotal miljoner enheter skicka 3 miljoner meddelanden per dag.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-tooa-different-tier"></a>Hur uppgraderar jag eller nedgradera min nav eller -namnområde tooa annat skikt?
Gå toohello  **[Azure-portalen]** > **Notification Hubs namnområden** eller **Meddelandehubbar**. Välj hello resurs du vill tooupdate och gå för**prisnivån**. Observera hello följande krav:

* hello uppdaterade prisnivån gäller för*alla* hubbar i hello-namnområde som du arbetar med.
* Om enheten beräkningen är längre än hello hello-nivå som du Nedgradera till, måste toodelete enheter innan du Nedgradera.


## <a name="design-and-development"></a>Design och utveckling
### <a name="which-server-side-platforms-do-you-support"></a>Vilka plattformar som servern stöder?
Server-SDK: er är tillgängliga för .NET, Java, Node.js, PHP och Python. Notification Hubs API: er baseras på REST-gränssnitt, så att du kan arbeta direkt med REST API: er om du använder olika plattformar eller inte vill att extra beroende. Mer information finns i toohello [Notification Hub REST API: er] sidan.

### <a name="which-client-platforms-do-you-support"></a>Vilka klientplattformar stöd?
Push-meddelanden stöds för [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Kina (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) och [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome-appar](notification-hubs-chrome-push-notifications-get-started.md), och [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Mer information finns i toohello [Notification Hubs Kom igång-självstudierna] sidan.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Har du stöd för SMS, e-post eller webb-meddelanden?
Notification Hubs är främst avsedd toosend meddelanden toomobile appar. Den ger inte e-post eller text-funktioner. Dock tredjeparts-plattformar som innehåller dessa funktioner kan integreras med Notification Hubs toosend native push-meddelanden med hjälp av [Mobilappar].

Notification Hubs ger inte en i webbläsaren push leverans meddelandetjänsten out of box hello också. Kunder kan implementera den här funktionen med SignalR ovanpå hello stöds SSI-plattformar. Om du vill toosend meddelanden toobrowser appar i hello Chrome sandbox finns hello [Chrome-appar kursen].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Hur är Mobile Apps och Azure Notification Hubs relaterade och när används de?
Om du har en befintlig mobila app tillbaka avslutas och du vill tooadd endast hello kapaciteten toosend push-meddelanden, kan du använda Azure Notification Hubs. Om du vill tooset upp din serverdel för mobilappen från början, Överväg att använda hello Mobile Apps-funktionen i Azure App Service. En mobil app etablerar en meddelandehubb automatiskt så att du enkelt skicka push-meddelanden från serverdelen för mobilappen för hello. Priser för Mobile Apps innehåller hello grundläggande kostnader för en meddelandehubb. Du betalar bara när du överskrider hello med push-meddelanden. Mer information om kostnaderna gå toohello [priser för Apptjänst] sidan.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Hur många enheter kan jag använda om jag skicka push-meddelanden via Notification Hubs?
Se toohello [Notification Hub-priser] ha information om hello antalet enheter som stöds.

Om du behöver stöd för fler än 10 miljoner registrerade enheter, [kontaktar du oss](https://azure.microsoft.com/overview/contact-us/) direkt och vi hjälper dig att skala din lösning.

### <a name="how-many-push-notifications-can-i-send-out"></a>Hur många push-meddelanden kan skickas?
Beroende på markerade nivån hello skalas Azure Notification Hubs automatiskt baserat på hello antal meddelanden som passerar genom hello system.

> [!NOTE]
> hello kan övergripande kostnader för användning öka baserat på hello antal push-meddelanden som hanteras. Kontrollera att du är medveten om hello nivå gränser på hello [Notification Hub-priser] sidan.
> 
> 

Våra kunder använder Notification Hubs toosend miljoner för push-meddelanden varje dag. Du har inte toodo något särskilda tooscale hello nå med push-meddelanden så länge som du använder Azure Notification Hubs.

### <a name="how-long-does-it-take-for-sent-push-notifications-tooreach-my-device"></a>Hur lång tid tar det tar för skicka push-meddelanden tooreach min enhet?
I ett scenario med normal användning, där hello inkommande belastningen är konsekvent och även, Azure Notification Hubs kan bearbeta minst *1 miljon push-meddelanden skickas en minut*. Hastigheten kan variera beroende på hello antal taggar, hello uppbyggnad hello inkommande skickar och andra yttre faktorer.

Under hello beräknad leveranstid hello tjänsten beräknar hello per plattform och skickar meddelanden toohello Push Notification Service (PNS) baserat på hello registrerade taggar eller Tagguttryck. Det är hello ansvar för hello PNS toosend meddelanden toohello enhet.

Hej PNS garanterar inte någon SLA för att leverera meddelanden. Men levereras de flesta push-meddelanden tootarget enheter inom några minuter (normalt inom 10 minuter) från hello tid skickas de tooNotification Hubs. Några meddelanden kan ta längre tid.

> [!NOTE]
> Azure Notification Hub har en princip i plats toodrop push-meddelanden som inte levereras toohello PNS inom 30 minuter. Den här fördröjningen kan bero på ett antal orsaker, men de flesta ofta eftersom hello PNS begränsning är ditt program.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Finns det några garanti latens?
Det finns ingen garanti för svarstid på grund av hello uppbyggnad push-meddelanden (de levereras med en extern, plattformsspecifika PNS). Hello merparten av push-meddelanden skickas vanligtvis inom några minuter.

### <a name="what-do-i-need-tooconsider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Vad gör jag behöver tooconsider när du skapar en lösning med namnområden och notification hubs?
#### <a name="mobile-appenvironment"></a>Mobila appmiljön
* Använda en meddelandehubb per mobilappen per miljö.
* Varje klient ska ha en separat NAV i ett scenario med flera innehavare.
* Dela hello aldrig samma meddelandehubben för produktions- och testmiljöer. Den här övningen kan orsaka problem när du skickar meddelanden. (Apple erbjuder Sandbox och produktion Push slutpunkter med särskilda referenser.)
* Som standard kan du skicka test-meddelanden tooyour registrerade enheter via hello Azure-portalen eller hello Azure integrerad komponent i Visual Studio. hello tröskelvärde anges too10 enheter som väljs slumpmässigt från hello registrering poolen.

> [!NOTE]
> Om din hubb ursprungligen konfigurerades med ett certifikat för Apple sandbox och har omkonfigurerats toouse ett Apple-certifikat för produktion, är hello ursprungliga enhetstoken ogiltiga. Ogiltig token orsaka toofail push-meddelanden. Separata produktions- och testmiljöer och använda olika NAV för olika miljöer.
> 
> 

#### <a name="pns-credentials"></a>PNS-autentiseringsuppgifter
När en mobil app har registrerats med en plattform developer-portalen (till exempel Apple eller Google), skickas ett app-ID och säkerhetstoken. hello app serverdel tillhandahåller dessa token toohello plattform PNS så att push-meddelanden kan skickas toodevices. Säkerhetstoken kan vara i form av hello av certifikat (till exempel Apple iOS eller Windows Phone) eller säkerhetsnycklar (till exempel Google Android eller Windows). De måste konfigureras i meddelandehubbar. Konfigurationen görs vanligtvis på hello meddelandehubben nivå, men kan även utföras på hello namnområdesnivån i ett scenario med flera innehavare.

#### <a name="namespaces"></a>namnområden
Namnområden kan användas för distribution av gruppering. De kan även vara används toorepresent alla meddelandehubbar för alla klienter i hello samma app i ett scenario med flera innehavare.

#### <a name="geo-distribution"></a>GEO-distribution
GEO-distribution är inte alltid viktigt i scenarier för push-meddelande. Olika PNSes (till exempel APN eller GCM) som levererar toodevices för push-meddelanden är inte jämnt.

Om du har ett program som används för globalt skapa du hubbar i olika namnområden med hjälp av hello Meddelandehubbar service i olika Azure-regioner runt hello world.

> [!NOTE]
> Vi rekommenderar inte den här ordningen eftersom det ökar din management kostnader, särskilt för registreringar. Utfärdad endast om det behövs en explicit.
> 
> 

### <a name="should-i-do-registrations-from-hello-app-back-end-or-directly-through-client-devices"></a>Bör jag göra registreringar från hello app serverdelen eller direkt via klienten enheter?
Registreringar från hello app serverdel är användbara när du har tooauthenticate klienter innan du skapar hello registrering. De kan också användas när du har taggar som måste skapas eller ändras av hello app serverdel baserat på logiken i appen. Mer information finns i toohello [Backend registrering vägledning] och [Backend registrering vägledning 2] sidor.

### <a name="what-is-hello-push-notification-delivery-security-model"></a>Vad är hello push notification leverans säkerhetsmodell?
Azure Notification Hubs använder en [signatur för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-baserade säkerhetsmodell. Du kan använda hello delade åtkomsttoken signaturen på hello rotnivå namnområde eller på hello detaljerade notification hub nivå. Signatur för delad åtkomsttoken kan vara set toofollow olika auktoriseringsregler, till exempel toosend meddelande behörigheter eller toolisten för meddelande behörigheter. Mer information finns i hello [Meddelandehubbar säkerhetsmodell] dokumentet.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Hur får jag för att hantera känslig nyttolast i push-meddelanden?
Alla meddelanden levereras tootarget enheter av PNS hello-plattformen. När ett meddelande skickas tooAzure Notification Hubs, bearbetas och skickas toohello respektive PNS.

Alla anslutningar från hello avsändaren toohello Azure Notification Hubs toohello PNS, använder HTTPS.

> [!NOTE]
> Azure Notification Hub loggar inte hello nyttolasten för meddelanden på något sätt.
> 
> 

toosend känsliga nyttolaster, bör du använda en säker Push-mönster. hello avsändaren ger ett ping-meddelande med en meddelande-ID toohello enhet utan hello känsliga nyttolast. När hello appen på hello enhet tar emot hello nyttolast, anropar hello app säker API direkt toofetch hello meddelandeinformation. En guide på hur tooimplement det här mönstret gå toohello [Notification Hubs Secure Push kursen] sidan.

## <a name="operations"></a>Åtgärder
### <a name="what-support-is-provided-for-disaster-recovery"></a>Vilka support tillhandahålls för katastrofåterställning?
Vi ger metadata disaster recovery täckning hos oss (hello Meddelandehubbar namn, hello anslutningssträngen och annan viktig information). När en katastrofåterställning utlöses registreringsdata är hello *endast segmentera* hello Meddelandehubbar infrastruktur som går förlorad. Du behöver tooimplement en lösning toorepopulate dessa data till din nya hubb efter återställning:

1. Skapa en sekundär meddelandehubben i olika datacenter. Vi rekommenderar att du skapar en från hello början tooshield du från disaster recovery händelser som kan påverka dina hanteringsfunktioner. Du kan också skapa en efter hello hello disaster recovery-händelse.

2. Fyll i hello sekundära meddelandehubben med hello registreringar från din primära meddelandehubb. Vi inte rekommenderar försök toomaintain registreringar på båda NAV och hålla dem synkroniserade som registreringar finns i. Den här metoden fungerar inte bra på grund av hello inbyggd tendens av registreringar tooexpire på hello PNS-sida. Notification Hubs rensar dem som den tar emot PNS feedback om registreringar har upphört att gälla eller är ogiltig.  

Vi har två rekommendationer för app-servrar:

* Använda en app-serverdel som upprätthåller en given uppsättning registreringar med dess slut. Den kan utföra en massinfogning i hello sekundära meddelandehubben.

* Använda en app-serverdel som hämtar en vanlig dump registreringar från hello primära meddelandehubben som en säkerhetskopia. Den kan utföra en massinfogning i hello sekundära meddelandehubben.

> [!NOTE]
> Registreringar Export/Import-funktioner som är tillgängliga i hello standardnivån beskrivs i hello [registreringar Export/Import] dokumentet.
> 
> 

Om du inte har en serverdel när hello appen startas på målenheterna, kan de utföra en ny registrering i hello sekundära meddelandehubben. Slutligen har hello sekundära meddelandehubben alla aktiva hello enheter som har registrerats.

Det blir en tidsperiod när enheter med oöppnad appar inte ta emot meddelanden.

### <a name="is-there-audit-log-capability"></a>Granska loggen kapaciteten finns?
Alla Meddelandehubbar hanteringsåtgärder gå toooperation loggarna, vilket visas i hello [klassiska Azure-portalen].

## <a name="monitoring-and-troubleshooting"></a>Övervakning och felsökning
### <a name="what-troubleshooting-capabilities-are-available"></a>Vilka funktioner för felsökning är tillgängliga?
Azure Notification Hub innehåller flera funktioner för felsökning, särskilt för hello vanligaste scenariot för utelämnade aviseringar. Mer information finns i hello [Meddelandehubbar felsökning] vitboken.

### <a name="what-telemetry-features-are-available"></a>Vilka telemetri funktioner är tillgängliga?
Azure Notification Hubs kan visa telemetridata i hello [klassiska Azure-portalen]. Information om hello mått är tillgängliga på hello [Notification Hubs mått] sidan.

> [!NOTE]
> Lyckade meddelanden innebär helt enkelt att push-meddelanden har levererats toohello externa PNS (till exempel APNS för Apple) eller GCM för Google. Det är hello ansvar hello PNS toodeliver hello meddelanden tootarget enheter. Normalt visar inte hello PNS leverans mått toothird parter.  
> 
> 

Vi erbjuder även telemetridata för hello kapaciteten tooexport hello programmässigt (i hello standardnivån). Mer information finns i hello [Notification Hubs mått exempel].

[klassiska Azure-portalen]: https://manage.windowsazure.com
[Notification Hub-priser]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Fallstudie: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Fallstudie: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Fallstudie: Seattle gånger]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Fallstudie: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Fallstudie: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[Notification Hub REST API: er]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Notification Hubs Kom igång-självstudierna]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome-appar kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Backend registrering vägledning]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Backend registrering vägledning 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Meddelandehubbar säkerhetsmodell]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Notification Hubs Secure Push kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Meddelandehubbar felsökning]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Notification Hubs mått]: https://msdn.microsoft.com/library/dn458822.aspx
[Notification Hubs mått exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[registreringar Export/Import]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure-portalen]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobilappar]: https://azure.microsoft.com/services/app-service/mobile/
[priser för Apptjänst]: https://azure.microsoft.com/pricing/details/app-service/
