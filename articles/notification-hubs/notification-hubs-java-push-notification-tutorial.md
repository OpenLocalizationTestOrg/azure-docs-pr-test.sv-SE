---
title: "Hur du använder Notification Hubs med Java"
description: "Lär dig hur du använder Azure Notification Hubs från Java backend."
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
ms.openlocfilehash: 41f978750ddef9f7e878c65b0017e909720154aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-java"></a><span data-ttu-id="ee6a4-103">Hur du använder Notification Hubs från Java</span><span class="sxs-lookup"><span data-stu-id="ee6a4-103">How to use Notification Hubs from Java</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="ee6a4-104">Det här avsnittet beskrivs viktiga funktioner i den nya stöds fullt ut officiella Azure Notification Hub Java SDK.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-104">This topic describes the key features of the new fully supported official Azure Notification Hub Java SDK.</span></span> <span data-ttu-id="ee6a4-105">Det här är ett projekt med öppen källkod och du kan visa hela SDK-koden i [Java SDK].</span><span class="sxs-lookup"><span data-stu-id="ee6a4-105">This is an open source project and you can view the entire SDK code at [Java SDK].</span></span> 

<span data-ttu-id="ee6a4-106">I allmänhet kan du har åtkomst till alla funktioner i Notification Hubs från en Java/PHP/Python/Ruby serverdel med Notification Hub REST-gränssnitt som beskrivs i avsnittet MSDN [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee6a4-106">In general, you can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span> <span data-ttu-id="ee6a4-107">Den här Java SDK ger en tunn omslutning över dessa REST-gränssnitt i Java.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-107">This Java SDK provides a thin wrapper over these REST interfaces in Java.</span></span> 

<span data-ttu-id="ee6a4-108">SDK stöder för närvarande:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-108">The SDK supports currently:</span></span>

* <span data-ttu-id="ee6a4-109">CRUD på Notification hub</span><span class="sxs-lookup"><span data-stu-id="ee6a4-109">CRUD on Notification Hubs</span></span> 
* <span data-ttu-id="ee6a4-110">CRUD på registreringar</span><span class="sxs-lookup"><span data-stu-id="ee6a4-110">CRUD on Registrations</span></span>
* <span data-ttu-id="ee6a4-111">Hantering av installation</span><span class="sxs-lookup"><span data-stu-id="ee6a4-111">Installation Management</span></span>
* <span data-ttu-id="ee6a4-112">Importera och exportera registreringar</span><span class="sxs-lookup"><span data-stu-id="ee6a4-112">Import/Export Registrations</span></span>
* <span data-ttu-id="ee6a4-113">Vanliga skickar</span><span class="sxs-lookup"><span data-stu-id="ee6a4-113">Regular Sends</span></span>
* <span data-ttu-id="ee6a4-114">Schemalagda skickar</span><span class="sxs-lookup"><span data-stu-id="ee6a4-114">Scheduled Sends</span></span>
* <span data-ttu-id="ee6a4-115">Asynkrona åtgärder via Java NIO</span><span class="sxs-lookup"><span data-stu-id="ee6a4-115">Async operations via Java NIO</span></span>
* <span data-ttu-id="ee6a4-116">Plattformar som stöds: APN (iOS), GCM (Android), WNS (Windows Store-appar), MPNS (Windows Phone), ADM (Amazon Kindle brand), Baidu (Android utan Google services)</span><span class="sxs-lookup"><span data-stu-id="ee6a4-116">Supported platforms: APNS (iOS), GCM (Android), WNS (Windows Store apps), MPNS(Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android without Google services)</span></span> 

## <a name="sdk-usage"></a><span data-ttu-id="ee6a4-117">SDK-användning</span><span class="sxs-lookup"><span data-stu-id="ee6a4-117">SDK Usage</span></span>
### <a name="compile-and-build"></a><span data-ttu-id="ee6a4-118">Kompilera och bygga</span><span class="sxs-lookup"><span data-stu-id="ee6a4-118">Compile and build</span></span>
<span data-ttu-id="ee6a4-119">Använd [Maven]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-119">Use [Maven]</span></span>

<span data-ttu-id="ee6a4-120">Att skapa:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-120">To build:</span></span>

    mvn package

## <a name="code"></a><span data-ttu-id="ee6a4-121">Kod</span><span class="sxs-lookup"><span data-stu-id="ee6a4-121">Code</span></span>
### <a name="notification-hub-cruds"></a><span data-ttu-id="ee6a4-122">Notification Hub CRUDs</span><span class="sxs-lookup"><span data-stu-id="ee6a4-122">Notification Hub CRUDs</span></span>
<span data-ttu-id="ee6a4-123">**Skapa en NamespaceManager:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-123">**Create a NamespaceManager:**</span></span>

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

<span data-ttu-id="ee6a4-124">**Skapa Notification Hub:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-124">**Create Notification Hub:**</span></span>

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 <span data-ttu-id="ee6a4-125">ELLER</span><span class="sxs-lookup"><span data-stu-id="ee6a4-125">OR</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="ee6a4-126">**Hämta Meddelandehubben:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-126">**Get Notification Hub:**</span></span>

    hub = namespaceManager.getNotificationHub("hubname");

<span data-ttu-id="ee6a4-127">**Uppdatera Meddelandehubben:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-127">**Update Notification Hub:**</span></span>

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

<span data-ttu-id="ee6a4-128">**Ta bort Meddelandehubben:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-128">**Delete Notification Hub:**</span></span>

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a><span data-ttu-id="ee6a4-129">Registrering CRUDs</span><span class="sxs-lookup"><span data-stu-id="ee6a4-129">Registration CRUDs</span></span>
<span data-ttu-id="ee6a4-130">**Skapa en Notification Hub-klient:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-130">**Create a Notification Hub client:**</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="ee6a4-131">**Skapa Windows-registrering:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-131">**Create Windows registration:**</span></span>

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

<span data-ttu-id="ee6a4-132">**Skapa iOS-registrering:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-132">**Create iOS registration:**</span></span>

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

<span data-ttu-id="ee6a4-133">På liknande sätt kan du skapa registreringar för Android (GCM), Windows Phone (MPNS) och Kindle brand (ADM).</span><span class="sxs-lookup"><span data-stu-id="ee6a4-133">Similarly you can create registrations for Android (GCM), Windows Phone (MPNS), and Kindle Fire (ADM).</span></span>

<span data-ttu-id="ee6a4-134">**Skapa mallen registreringar:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-134">**Create template registrations:**</span></span>

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

<span data-ttu-id="ee6a4-135">**Skapa registreringar med skapa registrationid + upsert mönster**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-135">**Create registrations using create registrationid + upsert pattern**</span></span>

<span data-ttu-id="ee6a4-136">Tar bort dubbletter på grund av eventuella förlorade svar om ID: n som lagras på enheten:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-136">Removes duplicates due to any lost responses if storing registration ids on the device:</span></span>

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

<span data-ttu-id="ee6a4-137">**Uppdatera registreringar:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-137">**Update registrations:**</span></span>

    hub.updateRegistration(reg);

<span data-ttu-id="ee6a4-138">**Ta bort registreringar:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-138">**Delete registrations:**</span></span>

    hub.deleteRegistration(regid);

<span data-ttu-id="ee6a4-139">**Fråga registreringar:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-139">**Query registrations:**</span></span>

* <span data-ttu-id="ee6a4-140">**Hämta enkel registrering:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-140">**Get single registration:**</span></span>
  
    <span data-ttu-id="ee6a4-141">hub.getRegistration(regid);</span><span class="sxs-lookup"><span data-stu-id="ee6a4-141">hub.getRegistration(regid);</span></span>
* <span data-ttu-id="ee6a4-142">**Visa alla registreringar hubb:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-142">**Get all registrations in hub:**</span></span>
  
    <span data-ttu-id="ee6a4-143">hub.getRegistrations();</span><span class="sxs-lookup"><span data-stu-id="ee6a4-143">hub.getRegistrations();</span></span>
* <span data-ttu-id="ee6a4-144">**Hämta registreringar med tagg:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-144">**Get registrations with tag:**</span></span>
  
    <span data-ttu-id="ee6a4-145">hub.getRegistrationsByTag("myTag");</span><span class="sxs-lookup"><span data-stu-id="ee6a4-145">hub.getRegistrationsByTag("myTag");</span></span>
* <span data-ttu-id="ee6a4-146">**Hämta registreringar per kanal:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-146">**Get registrations by channel:**</span></span>
  
    <span data-ttu-id="ee6a4-147">hub.getRegistrationsByChannel("devicetoken");</span><span class="sxs-lookup"><span data-stu-id="ee6a4-147">hub.getRegistrationsByChannel("devicetoken");</span></span>

<span data-ttu-id="ee6a4-148">Alla samlingsfrågor stöder $top och fortsättning token.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-148">All collection queries support $top and continuation tokens.</span></span>

### <a name="installation-api-usage"></a><span data-ttu-id="ee6a4-149">Installation av API-användning</span><span class="sxs-lookup"><span data-stu-id="ee6a4-149">Installation API usage</span></span>
<span data-ttu-id="ee6a4-150">API-installationen är en alternativ metod för hantering av registreringen.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-150">Installation API is an alternative mechanism for registration management.</span></span> <span data-ttu-id="ee6a4-151">I stället för att underhålla flera registreringar som inte är trivial och enkelt kan göras felaktigt eller ineffektiv går nu att använda en enda Installation-objekt.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-151">Instead of maintaining multiple registrations which is not trivial and may be easily done wrongly or inefficiently, it is now possible to use a SINGLE Installation object.</span></span> <span data-ttu-id="ee6a4-152">Installationen innehåller allt du behöver: push-kanal (enhetstoken), taggar, mallar, sekundära paneler (för WNS och APN).</span><span class="sxs-lookup"><span data-stu-id="ee6a4-152">Installation contains everything you need: push channel (device token), tags, templates, secondary tiles (for WNS and APNS).</span></span> <span data-ttu-id="ee6a4-153">Behöver du inte att anropa tjänsten för att hämta Id längre – bara generera GUID eller andra identifierare, hålla på enheten och skicka till din serverdel tillsammans med push-kanal (enhetstoken).</span><span class="sxs-lookup"><span data-stu-id="ee6a4-153">You don't need to call the service to get Id anymore - just generate GUID or any other identifier, keep it on device and send to your backend together with push channel (device token).</span></span> <span data-ttu-id="ee6a4-154">På serverdelen ska du bara göra en enda anrop: CreateOrUpdateInstallation, är det fullständigt idempotent, så passa på att försöka igen om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-154">On the backend you should only do a single call: CreateOrUpdateInstallation, it is fully idempotent, so feel free to retry if needed.</span></span>

<span data-ttu-id="ee6a4-155">Exempel för Amazon Kindle brand ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-155">As example for Amazon Kindle Fire it looks like this:</span></span>

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="ee6a4-156">Om du vill uppdatera den:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-156">If you want to update it:</span></span> 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="ee6a4-157">För avancerade scenarier har vi deluppdatering funktion som gör för att ändra endast särskilda egenskaper i objektet installation.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-157">For advanced scenarios we have partial update capability which allows to modify only particular properties of the installation object.</span></span> <span data-ttu-id="ee6a4-158">I princip är deluppdatering delmängd av JSON-korrigering åtgärder du kan köra mot objektet för Installation.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-158">Basically partial update is subset of JSON Patch operations you can run against Installation object.</span></span>

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

<span data-ttu-id="ee6a4-159">Ta bort installationen:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-159">Delete Installation:</span></span>

    hub.deleteInstallation(installation.getInstallationId());

<span data-ttu-id="ee6a4-160">CreateOrUpdate, korrigeringsfil och ta bort är överensstämmelse med Get.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-160">CreateOrUpdate, Patch and Delete are eventually consistent with Get.</span></span> <span data-ttu-id="ee6a4-161">Den begärda åtgärden kan bara flyttas till kön systemet under anropet och körs i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-161">Your requested operation just goes to the system queue during the call and will be executed in background.</span></span> <span data-ttu-id="ee6a4-162">Observera att hämta inte är avsedd för huvudsakliga runtime scenariot men bara för felsökning och felsökning, begränsas nära av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-162">Note that Get is not designed for main runtime scenario but just for debug and troubleshooting purposes, it is tightly throttled by the service.</span></span>

<span data-ttu-id="ee6a4-163">Skicka flödet för installationer är desamma som för registreringar.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-163">Send flow for Installations is the same as for Registrations.</span></span> <span data-ttu-id="ee6a4-164">Vi har precis introducerat möjlighet till mål meddelande till viss installationen - bara använda taggen ”InstallationId: {desired-id}”.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-164">We've just introduced an option to target notification to the particular Installation - just use tag "InstallationId:{desired-id}".</span></span> <span data-ttu-id="ee6a4-165">För ovanstående fall det skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-165">For case above it would look like this:</span></span>

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

<span data-ttu-id="ee6a4-166">För en av flera mallar:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-166">For one of several templates:</span></span>

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a><span data-ttu-id="ee6a4-167">Schemalägga meddelanden (tillgängligt för standardnivån)</span><span class="sxs-lookup"><span data-stu-id="ee6a4-167">Schedule Notifications (available for STANDARD Tier)</span></span>
<span data-ttu-id="ee6a4-168">Samma som regelbundet skicka men med en ytterligare parameter - scheduledTime som säger när meddelanden ska levereras.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-168">The same as regular send but with one additional parameter - scheduledTime which says when notification should be delivered.</span></span> <span data-ttu-id="ee6a4-169">Tjänsten accepterar helst mellan nu + 5 minuter och nu + 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-169">Service accepts any point of time between now + 5 minutes and now + 7 days.</span></span>

<span data-ttu-id="ee6a4-170">**Schemalägg en intern Windows-avisering:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-170">**Schedule a Windows native notification:**</span></span>

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a><span data-ttu-id="ee6a4-171">Importera och exportera (tillgängligt för standardnivån)</span><span class="sxs-lookup"><span data-stu-id="ee6a4-171">Import/Export (available for STANDARD Tier)</span></span>
<span data-ttu-id="ee6a4-172">Ibland är det krävs för att utföra massredigering mot registreringar.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-172">Sometimes it is required to perform bulk operation against registrations.</span></span> <span data-ttu-id="ee6a4-173">Vanligtvis är den för integrering med en annan dator eller en omfattande lösning att säga uppdatera taggarna.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-173">Usually it is for integration with another system or just a massive fix to say update the tags.</span></span> <span data-ttu-id="ee6a4-174">Det rekommenderas starkt inte att använda Get/uppdatera flödet om vi pratar om tusentals registreringar.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-174">It is strongly not recommended to use Get/Update flow if we are talking about thousands of registrations.</span></span> <span data-ttu-id="ee6a4-175">Import/Export-funktionen är avsedd att täcka scenariot.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-175">Import/Export capability is designed to cover the scenario.</span></span> <span data-ttu-id="ee6a4-176">I praktiken ger en åtkomst till vissa blob-behållare under ditt lagringskonto som en källa för inkommande data och plats för utdata.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-176">Basically you provide an access to some blob container under your storage account as a source of incoming data and location for output.</span></span>

<span data-ttu-id="ee6a4-177">**Skicka exportjobb:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-177">**Submit export job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


<span data-ttu-id="ee6a4-178">**Skicka importjobbet:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-178">**Submit import job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

<span data-ttu-id="ee6a4-179">**Vänta tills jobbet är klar:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-179">**Wait until job is done:**</span></span>

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

<span data-ttu-id="ee6a4-180">**Hämta alla jobb:**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-180">**Get all jobs:**</span></span>

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

<span data-ttu-id="ee6a4-181">**URI: N med SAS-signatur:** detta är Webbadressen till vissa blob-fil eller blob-behållaren plus uppsättning parametrar som behörigheter och förfallotid plus signaturen för dessa saker som skapats med hjälp av kontots SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-181">**URI with SAS signature:** This is the URL of some blob file or blob container plus set of parameters like permissions and expiration time plus signature of all these things made using account's SAS key.</span></span> <span data-ttu-id="ee6a4-182">Azure Storage Java SDK: N har omfattande funktioner inklusive skapandet av denna typ av URI: er.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-182">Azure Storage Java SDK has rich capabilities including creation of such kind of URIs.</span></span> <span data-ttu-id="ee6a4-183">Du kan ta en titt på ImportExportE2E TestKlass (från platsen som github) som har en mycket enkel och compact implementering av Signeringsalgoritm som enkelt alternativ.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-183">As simple alternative you can take a look at ImportExportE2E test class (from the github location) which has very basic and compact implementation of signing algorithm.</span></span>

### <a name="send-notifications"></a><span data-ttu-id="ee6a4-184">Skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="ee6a4-184">Send Notifications</span></span>
<span data-ttu-id="ee6a4-185">Meddelande-objekt är helt enkelt en brödtext med rubriker, vissa metoder att skapa objekt för intern och mallen för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-185">The Notification object is simply a body with headers, some utility methods help in building the native and template notifications objects.</span></span>

* <span data-ttu-id="ee6a4-186">**Windows Store och Windows Phone 8.1 (utan Silverlight)**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-186">**Windows Store and Windows Phone 8.1 (non-Silverlight)**</span></span>
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="ee6a4-187">**iOS**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-187">**iOS**</span></span>
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* <span data-ttu-id="ee6a4-188">**Android**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-188">**Android**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="ee6a4-189">**Windows Phone 8.0 och 8.1 Silverlight**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-189">**Windows Phone 8.0 and 8.1 Silverlight**</span></span>
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="ee6a4-190">**Kindle brand**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-190">**Kindle Fire**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="ee6a4-191">**Skicka till taggar**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-191">**Send to Tags**</span></span>
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* <span data-ttu-id="ee6a4-192">**Skicka etikettuttrycket**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-192">**Send to tag expression**</span></span>       
  
        hub.sendNotification(n, "foo && ! bar");
* <span data-ttu-id="ee6a4-193">**Skicka meddelande för mallen**</span><span class="sxs-lookup"><span data-stu-id="ee6a4-193">**Send template notification**</span></span>
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

<span data-ttu-id="ee6a4-194">Kör Java-kod ska nu skapa ett meddelande som visas på målenheten.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-194">Running your Java code should now produce a notification appearing on your target device.</span></span>

## <span data-ttu-id="ee6a4-195"><a name="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee6a4-195"><a name="next-steps"></a>Next Steps</span></span>
<span data-ttu-id="ee6a4-196">Vi visar hur du skapar en enkel RESTEN av Java-klient för Meddelandehubbar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-196">In this topic we showed how to create a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="ee6a4-197">Härifrån kan du:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-197">From here you can:</span></span>

* <span data-ttu-id="ee6a4-198">Hämta fullständigt [Java SDK], som innehåller hela SDK-koden.</span><span class="sxs-lookup"><span data-stu-id="ee6a4-198">Download the full [Java SDK], which contains the entire SDK code.</span></span> 
* <span data-ttu-id="ee6a4-199">Spela upp med exemplen:</span><span class="sxs-lookup"><span data-stu-id="ee6a4-199">Play with the samples:</span></span>
  * <span data-ttu-id="ee6a4-200">[Kom igång med Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-200">[Get Started with Notification Hubs]</span></span>
  * <span data-ttu-id="ee6a4-201">[Skicka de senaste nyheterna]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-201">[Send breaking news]</span></span>
  * <span data-ttu-id="ee6a4-202">[Skicka lokaliserade senaste nyheterna]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-202">[Send localized breaking news]</span></span>
  * <span data-ttu-id="ee6a4-203">[Skicka meddelanden till autentiserade användare]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-203">[Send notifications to authenticated users]</span></span>
  * <span data-ttu-id="ee6a4-204">[Skicka plattformsoberoende meddelanden till autentiserade användare]</span><span class="sxs-lookup"><span data-stu-id="ee6a4-204">[Send cross-platform notifications to authenticated users]</span></span>

<span data-ttu-id="ee6a4-205">[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend</span><span class="sxs-lookup"><span data-stu-id="ee6a4-205">[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend</span></span>
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
<span data-ttu-id="ee6a4-206">[Kom igång med Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-206">[Get Started with Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/</span></span>
<span data-ttu-id="ee6a4-207">[Skicka de senaste nyheterna]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-207">[Send breaking news]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/</span></span>
<span data-ttu-id="ee6a4-208">[Skicka lokaliserade senaste nyheterna]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-208">[Send localized breaking news]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
<span data-ttu-id="ee6a4-209">[Skicka meddelanden till autentiserade användare]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-209">[Send notifications to authenticated users]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/</span></span>
<span data-ttu-id="ee6a4-210">[Skicka plattformsoberoende meddelanden till autentiserade användare]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-210">[Send cross-platform notifications to authenticated users]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/</span></span>
<span data-ttu-id="ee6a4-211">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="ee6a4-211">[Maven]: http://maven.apache.org/</span></span>

