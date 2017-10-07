---
title: aaaHow toouse Meddelandehubbar med Java
description: "Lär dig hur toouse Azure Notification Hubs från Java backend-."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: afcf305b1acd9ee28ee4889040ece59d9399d29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-java"></a>Hur toouse Notification Hubs från Java
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Det här avsnittet beskrivs hello viktiga funktioner i officiella hello nya fullt stöd för Azure Notification Hub Java SDK. Det här är ett projekt med öppen källkod och du kan visa hello hela SDK-koden i [Java SDK]. 

I allmänhet kan du komma åt alla funktioner i Notification Hubs från Java/PHP/Python/Ruby backend-gränssnittet hello Notification Hub REST enligt beskrivningen i avsnittet om MSDN hello [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx). Den här Java SDK ger en tunn omslutning över dessa REST-gränssnitt i Java. 

hello stöder SDK för närvarande:

* CRUD på Notification hub 
* CRUD på registreringar
* Hantering av installation
* Importera och exportera registreringar
* Vanliga skickar
* Schemalagda skickar
* Asynkrona åtgärder via Java NIO
* Plattformar som stöds: APN (iOS), GCM (Android), WNS (Windows Store-appar), MPNS (Windows Phone), ADM (Amazon Kindle brand), Baidu (Android utan Google services) 

## <a name="sdk-usage"></a>SDK-användning
### <a name="compile-and-build"></a>Kompilera och bygga
Använd [Maven]

toobuild:

    mvn package

## <a name="code"></a>Kod
### <a name="notification-hub-cruds"></a>Notification Hub CRUDs
**Skapa en NamespaceManager:**

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Skapa Notification Hub:**

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 ELLER

    hub = new NotificationHub("connection string", "hubname");

**Hämta Meddelandehubben:**

    hub = namespaceManager.getNotificationHub("hubname");

**Uppdatera Meddelandehubben:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Ta bort Meddelandehubben:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Registrering CRUDs
**Skapa en Notification Hub-klient:**

    hub = new NotificationHub("connection string", "hubname");

**Skapa Windows-registrering:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**Skapa iOS-registrering:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

På liknande sätt kan du skapa registreringar för Android (GCM), Windows Phone (MPNS) och Kindle brand (ADM).

**Skapa mallen registreringar:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Skapa registreringar med skapa registrationid + upsert mönster**

Tar bort dubbletter på grund av förlorad tooany svar om ID: n som lagras på hello enhet:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Uppdatera registreringar:**

    hub.updateRegistration(reg);

**Ta bort registreringar:**

    hub.deleteRegistration(regid);

**Fråga registreringar:**

* **Hämta enkel registrering:**
  
    hub.getRegistration(regid);
* **Visa alla registreringar hubb:**
  
    hub.getRegistrations();
* **Hämta registreringar med tagg:**
  
    hub.getRegistrationsByTag("myTag");
* **Hämta registreringar per kanal:**
  
    hub.getRegistrationsByChannel("devicetoken");

Alla samlingsfrågor stöder $top och fortsättning token.

### <a name="installation-api-usage"></a>Installation av API-användning
API-installationen är en alternativ metod för hantering av registreringen. I stället för att underhålla flera registreringar som inte är trivial och enkelt kan göras felaktigt eller ineffektiv är det nu möjligt toouse ett enda Installation-objekt. Installationen innehåller allt du behöver: push-kanal (enhetstoken), taggar, mallar, sekundära paneler (för WNS och APN). Du inte behöver toocall hello service tooget Id längre – bara generera GUID eller andra identifierare, hålla på enheten och skicka tooyour backend tillsammans med push-kanal (enhetstoken). På hello serverdel bör du bara göra en enda anrop: CreateOrUpdateInstallation, är det fullständigt idempotent, så upplevs ledigt tooretry behövs.

Exempel för Amazon Kindle brand ser ut så här:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Om du vill tooupdate den: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

För avancerade scenarier har vi deluppdatering funktion som gör att toomodify endast särskilda egenskaper hello installationen objekt. I princip är deluppdatering delmängd av JSON-korrigering åtgärder du kan köra mot objektet för Installation.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Ta bort installationen:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, korrigeringsfil och ta bort är överensstämmelse med Get. Den begärda åtgärden bara stängs toohello system kön under hello anrop och körs i bakgrunden. Observera att hämta inte är avsedd för huvudsakliga runtime scenariot men bara för felsökning och felsökning, begränsas nära av hello-tjänsten.

Skicka flödet för installationer är hello samma som för registreringar. Vi har precis introducerat en alternativet tootarget meddelande toohello installationen - bara använda taggen ”InstallationId: {desired-id}”. För ovanstående fall det skulle se ut så här:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

För en av flera mallar:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Schemalägga meddelanden (tillgängligt för standardnivån)
Hej samma sak som regelbundet skicka men med en ytterligare parameter - scheduledTime som säger när meddelanden ska levereras. Tjänsten accepterar helst mellan nu + 5 minuter och nu + 7 dagar.

**Schemalägg en intern Windows-avisering:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>Importera och exportera (tillgängligt för standardnivån)
Ibland är det obligatoriska tooperform massåtgärd mot registreringar. Det är vanligtvis för integrering med ett annat system eller bara en omfattande lösning toosay uppdatering hello taggar. Det rekommenderas starkt inte toouse Get/uppdatera flödet om vi pratar om tusentals registreringar. Import/Export-kapaciteten är utformad toocover hello scenario. I praktiken ger åtkomst toosome blob-behållare under ditt lagringskonto som en källa för inkommande data och plats för utdata.

**Skicka exportjobb:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**Skicka importjobbet:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**Vänta tills jobbet är klar:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Hämta alla jobb:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**URI: N med SAS-signatur:** detta är hello URL för vissa blob-fil eller blob-behållaren plus uppsättning parametrar som behörigheter och förfallotid plus signaturen för dessa saker som skapats med hjälp av kontots SAS-nyckel. Azure Storage Java SDK: N har omfattande funktioner inklusive skapandet av denna typ av URI: er. Du kan ta en titt på ImportExportE2E TestKlass (från hello github plats) som har en mycket enkel och compact implementering av Signeringsalgoritm som enkelt alternativ.

### <a name="send-notifications"></a>Skicka meddelanden
hello meddelandeobjekt är helt enkelt en brödtext med rubriker, vissa metoder att skapa objekt för hello intern och mallen för meddelanden.

* **Windows Store och Windows Phone 8.1 (utan Silverlight)**
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* **iOS**
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* **Android**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* **Windows Phone 8.0 och 8.1 Silverlight**
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* **Kindle brand**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **Skicka tooTags**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Skicka tootag uttryck**       
  
        hub.sendNotification(n, "foo && ! bar");
* **Skicka meddelande för mallen**
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Kör Java-kod ska nu skapa ett meddelande som visas på målenheten.

## <a name="next-steps"></a>Nästa steg
I det här avsnittet visar hur toocreate en enkel Java REST-klienten för Notification Hubs. Härifrån kan du:

* Hämta hello fullständig [Java SDK], som innehåller hello hela SDK-kod. 
* Spela upp med hello exempel:
  * [Kom igång med Notification Hubs]
  * [Skicka de senaste nyheterna]
  * [Skicka lokaliserade senaste nyheterna]
  * [Skicka meddelanden tooauthenticated användare]
  * [Skicka plattformsoberoende meddelanden tooauthenticated användare]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Kom igång med Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Skicka de senaste nyheterna]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Skicka lokaliserade senaste nyheterna]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Skicka meddelanden tooauthenticated användare]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Skicka plattformsoberoende meddelanden tooauthenticated användare]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

