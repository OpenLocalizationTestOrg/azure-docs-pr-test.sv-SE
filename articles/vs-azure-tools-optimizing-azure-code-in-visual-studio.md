---
title: aaaOptimizing din Azure kod i Visual Studio | Microsoft Docs
description: "Lär dig mer om hur Azure kod optimering verktyg i Visual Studio gör din kod mer stabilt och bättre prestanda."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a>Optimera din Azure kod
När du programmering appar som använder Microsoft Azure, det finns vissa kodningsexempel som du bör följa toohelp undvika problem med appen skalbarhet, funktioner och prestanda i en molnmiljö. Microsoft tillhandahåller ett analysverktyg i Azure kod som identifierar och identifierar de dessa ofta påträffade problem och hjälper dig att lösa dem. Du kan hämta hello verktyg i Visual Studio via NuGet.

## <a name="azure-code-analysis-rules"></a>Azure kod Analysis-regler
hello Azure kod analysverktyget använder hello enligt reglerna för tooautomatically flaggan Azure koden när den söker efter kända problem som påverkar prestanda. Identifierat problem visas som en varningar eller fel. Koden korrigeringar eller förslag tooresolve hello varnings- eller ofta sker via en lampikonen.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Undvik att använda sessionstillståndsläge för standard (pågående)
### <a name="id"></a>ID
AP0000

### <a name="description"></a>Beskrivning
Om du använder hello standard (pågående) sessionstillståndsläge för molnprogram, förlorar du sessionstillstånd.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Som standard är hello sessionstillståndsläge som angetts i web.config-filen för hello i processen. Även om ingen post som angetts i konfigurationsfilen för hello hello sessionstillstånd läge som standard tooin-processen. hello-läge i processen lagrar sessionstillstånd i minnet på hello webbserver. När en instans har startats om eller en ny instans används för belastningsutjämning eller stöd för redundans, sparas inte hello sessionstillstånd lagras i minnet på hello webbserver. Detta förhindrar att hello program som skalbara på hello moln.

Sessionstillståndet ASP.NET stöder flera olika lagringsalternativ för sessionstillståndsdata: InProc, StateServer, SQLServer, anpassad, och ut. Det rekommenderas att du använder anpassade läge toohost data på en extern butik sessionstillstånd, t.ex [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Lösning
En rekommenderad lösning är toostore sessionens tillstånd på en hanterad cache-tjänst. Lär dig hur toouse [Azure sessionstillståndsprovider för Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore sessionens tillstånd. Du kan också store-sessionen tillstånd i andra platser tooensure ditt program är skalbara på hello moln. Mer information om alternativa lösningar Läs toolearn [Session tillstånd lägen](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Kör metoden får inte vara asynkrona
### <a name="id"></a>ID
AP1000

### <a name="description"></a>Beskrivning
Skapa asynkrona metoder (som [await](https://msdn.microsoft.com/library/hh156528.aspx)) utanför hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden och sedan anropa hello asynkrona metoder från [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklarerar hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden som asynkron gör hello worker rollen tooenter en omstart loop.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Anropar asynkrona metoder i hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör hello cloud service runtime toorecycle hello worker-rollen. När en arbetsroll startar alla programkörningen äger rum inom hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod. Befintliga hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoden gör hello worker rollen toorestart. När hello worker-rollen runtime träffar hello async-metod, skickar alla åtgärder efter hello async-metod och returnerar sedan. Detta medför hello worker rollen tooexit från hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod och starta om. I hello nästa iteration av körningen träffar hello arbetsrollen hello asynkrona metoden igen och omstarter, orsakar hello worker rollen toorecycle samt igen.

### <a name="solution"></a>Lösning
Placera alla asynkrona åtgärder utanför hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod. Anropa sedan hello omstrukturerade async-metod från inuti hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod, till exempel RunAsync () .wait. hello Azure kod analysverktyget hjälper dig att åtgärda problemet.

hello följande kodfragment som visar hello kod korrigering för problemet:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Använda Service Bus signatur för delad åtkomst-autentisering
### <a name="id"></a>ID
AP2000

### <a name="description"></a>Beskrivning
Använda delade signatur åtkomst (SAS) för autentisering. Access Control Service (ACS) är inaktuell för service bus-autentisering.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
För förbättrad säkerhet ersätter Azure Active Directory ACS-autentisering med SAS-autentisering. Se [Azure Active Directory är hello framtida av ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) information om hello övergången plan.

### <a name="solution"></a>Lösning
Använd SAS-autentisering i dina appar. hello som följande exempel visar hur toouse en befintlig SAS-token tooaccess en service bus namnområde eller enhet.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Se hello följande avsnitt för mer information.

* En översikt finns [signatur autentisering för delad åtkomst med Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)
* [Hur toouse autentisering med signatur för delad åtkomst med Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)
* En exempelprojektet finns [autentisering med delad signatur åtkomst (SAS) med Service Bus-prenumerationer](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a>Överväg att använda OnMessage metoden tooavoid ”meddelandemottagning”
### <a name="id"></a>ID
AP2002

### <a name="description"></a>Beskrivning
tooavoid gå in på en ”meddelandemottagning”, anropa hello **OnMessage** metoden är en bättre lösning för att ta emot meddelanden än anropa hello **Receive** metod. Men om du måste använda hello **Receive** metod och ange väntetiden för en icke-standard-servern, kontrollera hello server väntetiden är mer än en minut.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
När du anropar **OnMessage**, hello klienten startar ett internt meddelande pump som ständigt avsöker hello kö eller prenumeration. Det här meddelandet pump innehåller en oändlig loop som utfärdar ett anrop tooreceive meddelanden. Om hello anropet uppnår tidsgränsen, utfärdas ett nytt anrop. hello timeoutintervall bestäms av hello värdet för hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) -egenskapen för hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)som används.

Hej fördelen med att använda **OnMessage** jämfört med för**Receive** är att användare inte har toomanually söka efter meddelanden, hantera undantag, bearbeta flera meddelanden parallellt och slutföra hello meddelanden.

Om du anropar **Receive** utan att använda standardvärdet vara säker på att hello *ServerWaitTime* värdet är mer än en minut. Ange *ServerWaitTime* toomore än en minut timeout innan hello helt meddelande förhindrar hello-servern.

### <a name="solution"></a>Lösning
Se följande kodexempel för rekommenderade användningsområden hello. Mer information finns i [QueueClient.OnMessage-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)och [QueueClient.Receive-metoden (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

tooimprove hello prestanda för hello Azure meddelandeinfrastruktur finns hello designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).

hello följande är ett exempel på hur du använder **OnMessage** tooreceive meddelanden.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

hello följande är ett exempel på hur du använder **ta emot** med hello standardserver väntetid.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

hello följande är ett exempel på hur du använder **Receive** väntetid med en server som inte är standard.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Överväg att använda asynkrona Service Bus-metoder
### <a name="id"></a>ID
AP2003

### <a name="description"></a>Beskrivning
Använda asynkrona Service Bus metoder tooimprove prestanda med asynkrona meddelanden.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
När du använder asynkrona metoder kan programmet programmet samtidighet eftersom körs varje anrop inte blockerar hello huvudtråden. När du använder Service Bus messaging metoder, utför en åtgärd (skicka, ta emot, ta bort, etc.) tar tid. Nu innehåller hello bearbetningen av hello åtgärden av hello Service Bus-tjänst i tillägg toohello svarstiden för hello och hello svarsmeddelanden. tooincrease hello antalet åtgärder per tid, åtgärder måste köras samtidigt. Mer information finns för[bästa praxis för prestanda förbättringar med Service Bus asynkrona meddelanden](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Lösning
Se [QueueClient klass (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) information om hur toouse hello rekommenderad asynkron metod.

tooimprove hello prestanda för hello Azure meddelandeinfrastruktur finns hello designmönstret [asynkrona meddelanden introduktion](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Överväg att partitionering Service Bus-köer och ämnen
### <a name="id"></a>ID
AP2004

### <a name="description"></a>Beskrivning
Partitionen Service Bus-köer och ämnen för bättre prestanda med Service Bus-meddelanden.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Partitionering Service Bus-köer och ämnen ökar prestanda genomflöde och tjänsten tillgänglighet eftersom hello totala genomflödet av en partitionerad kö eller ett ämne begränsas inte längre av hello resultatet av ett enda meddelande broker eller meddelandearkiv. Dessutom kan spärra ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne. Mer information finns i [partitionering Meddelandeentiteter](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Lösning
Hej följande fragment visas hur toopartition meddelandeentiteter.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Mer information finns i [partitionerad Service Bus-köer och ämnen | Microsoft Azure-blogg](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) och checka ut hello [Microsoft Azure Service Bus partitionerad-kö](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) exempel.

## <a name="do-not-set-sharedaccessstarttime"></a>Ange inte SharedAccessStartTime
### <a name="id"></a>ID
AP3001

### <a name="description"></a>Beskrivning
Du bör undvika att använda SharedAccessStartTimeset toohello klockslaget tooimmediately starta hello princip för delad åtkomst. Du behöver bara tooset den här egenskapen om du vill toostart hello delad åtkomstprincip vid ett senare tillfälle.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Synkronisering av datorklocka medför en lätt tidsskillnaden mellan datacenter. Du skulle till exempel logiskt se inställningen hello starttiden för lagring SAS-princip som hello aktuell tid med hjälp av DateTime.Now eller en liknande metod kommer hello SAS princip tootake gälla omedelbart. Hello mindre tidsskillnaderna mellan Datacenter kan emellertid orsaka problem med den här eftersom datacenter ibland kan vara lite senare än starttiden för hello, medan andra i den. Därför hello SAS-princip kan gälla snabbt (eller även omedelbart) om hello princip livstid är för kort.

Mer information om med signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdatera tooBlob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösning
Ta bort hello-uttryck som hello starttiden för hello delad åtkomstprincip. hello Azure kod analysverktyget är en lösning på problemet. Mer information om säkerhetshantering finns hello designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).

hello följande kodfragment som visar hello kod korrigering för problemet.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Delad åtkomstprincip förfallotiden måste vara fler än fem minuter
### <a name="id"></a>ID
AP3002

### <a name="description"></a>Beskrivning
Det kan finnas lika mycket som en fem minuters skillnad i klockor mellan Datacenter på olika platser på grund av tooa tillstånd som kallas ”klockavvikelser”. tooprevent hello SAS princip-token upphör att gälla ange tidigare än planerat, hello utgången tid toobe mer än fem minuter.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Datacenter på olika platser runt hello world synkronisera genom en signal klockan. Eftersom det tar tid för klockan signal tootravel toodifferent platser, kan det finnas en gång avvikelse mellan Datacenter på olika geografiska platser trots allt är synkroniserad. Den här tidsskillnaden kan påverka hello Shared Access start tid och förfallodatum avsökningsintervallet för klientprinciper. Därför tooensure princip för delad åtkomst träder i kraft omedelbart, anger inte hello starttid. Kontrollera dessutom att hello förfallotid är mer än 5 minuter tooprevent tidig timeout.

Mer information om hur du använder signatur för delad åtkomst på Azure storage finns [introduktion till tabellen SAS (signatur för delad åtkomst), Queue SAS och uppdatera tooBlob SAS - Microsoft Azure Storage-teamets blogg - platsen Start - MSDN-bloggarna](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösning
Mer information om säkerhetshantering finns hello designmönstret [Valet nyckeln mönster](https://msdn.microsoft.com/library/dn568102.aspx).

hello följande är ett exempel på inte att ange en starttid för delad åtkomst princip.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

hello följande är ett exempel på att ange en starttid för delad åtkomst principen med princip upphör att gälla mer än fem minuter.

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Mer information finns i [skapar och använder en signatur för delad åtkomst](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Använd CloudConfigurationManager
### <a name="id"></a>ID
AP4000

### <a name="description"></a>Beskrivning
Med hjälp av hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) klassen för projekt som Azure-webbplats och Azure mobile services inte introducerar runtime problem. Som bästa praxis, men det är en bra idé toouse moln[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) som ett enhetligt sätt att hantera konfigurationer för alla program i Azure-molnet.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
CloudConfigurationManager läser hello configuration file lämpliga toohello programmiljö.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Lösning
Refactor din kod toouse hello [CloudConfigurationManager-klassen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Hello Azure kod analysverktyg som en kod korrigering för problemet.

hello följande kodfragment som visar hello kod korrigering för problemet. Ersätt

`var settings = ConfigurationManager.AppSettings["mySettings"];`

med

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Här är ett exempel på hur toostore hello Konfigurationsinställningen i App.config eller Web.config-filen. Lägg till hello inställningar toohello AppSettings i konfigurationsfilen för hello. hello följer hello Web.config-filen för hello förra kodexemplet.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Undvik att använda hårdkodade anslutningssträngar
### <a name="id"></a>ID
AP4001

### <a name="description"></a>Beskrivning
Om du använder hårdkodade anslutningssträngar och du behöver tooupdate dem senare, du har toomake ändringar tooyour källkoden och omkompilera hello program. Men om du sparar din anslutningssträngar i en konfigurationsfil, kan du ändra dem senare genom att helt enkelt uppdatera hello konfigurationsfilen.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Hårdkoda anslutningssträngar är felaktigt eftersom problem när anslutningssträngar behöver toobe som ändras snabbt. Dessutom hello projektet måste toobe incheckad toosource kontroll, införa hårdkodade anslutningssträngar säkerhetsrisker eftersom hello strängar kan visas i hello källkod.

### <a name="solution"></a>Lösning
Lagrar anslutningssträngar i hello configuration-filer eller miljöer i Azure.

* För fristående program kan du använda inställningarna för app.config toostore-anslutningssträngar.
* Använd web.config toostore anslutningssträngar för IIS-värdbaserade webbprogram.
* Använd configuration.json toostore anslutningssträngar för ASP.NET-program vNext.

Information om hur du använder konfigurationer filer, till exempel web.config eller app.config finns [riktlinjer för konfiguration av ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Mer information om hur Azure miljö variabler arbete finns [Azure webbplatser: hur programmet strängar och anslutningen strängar fungerar](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Mer information om lagra anslutningssträngen i källkontrollen finns [undvika att sätta känslig information, till exempel anslutningssträngar i filer som lagras i koden källdatabasen](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Använd diagnostik-konfigurationsfil
### <a name="id"></a>ID
AP5000

### <a name="description"></a>Beskrivning
I stället för att konfigurera diagnostikinställningar för i din kod som genom att använda hello Microsoft.WindowsAzure.Diagnostics programmering API bör du konfigurera inställningarna för webbprogramdiagnostik i hello diagnostics.wadcfg-filen. (Eller diagnostics.wadcfgx om du använder Azure SDK 2.5). Du kan ändra inställningarna för webbprogramdiagnostik utan toorecompile din kod genom att göra.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
Innan Azure SDK 2,5 (som använder Azure diagnostics 1.3), Azure Diagnostics (BOMULLSTUSS) kan konfigureras med hjälp av flera olika metoder: lägga till den toohello configuration blob i lagring, med hjälp av tvingande koden, deklarativa konfiguration eller hello standard konfiguration. Dock rekommenderas hello sätt tooconfigure diagnostics är toouse en XML-konfigurationsfil (diagnostics.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare) i hello projektet. I den här metoden kan hello diagnostics.wadcfg filen helt definierar hello konfigurationen och uppdateras och omdistribueras när du vill. Blanda hello användning av hello diagnostics.wadcfg konfigurationsfilen med hello programmering för konfigurationer med hjälp av hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)eller [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) klasser kan leda tooconfusion. Se [initieras eller ändra Azure Diagnostics konfiguration](https://msdn.microsoft.com/library/azure/hh411537.aspx) för mer information.

Från och med BOMULLSTUSS 1.3 (ingår i Azure SDK 2.5) kan den inte längre möjligt toouse kod tooconfigure diagnostik. Därför kan du bara ange hello konfiguration när du tillämpar eller uppdaterar hello diagnostik tillägg.

### <a name="solution"></a>Lösning
Använda hello diagnostik configuration designer toomove diagnostikinställningar toohello diagnostik konfigurationsfil (diagnositcs.wadcfg eller diagnositcs.wadcfgx för SDK-2.5 och senare). Det rekommenderas också att du installerar [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) och använda hello senaste diagnostik.

1. Hello snabbmenyn för hello roll som du vill tooconfigure väljer Egenskaper och klicka på fliken för hello-konfiguration.
2. I hello **diagnostik** Kontrollera att hello **aktivera diagnostik** är markerad.
3. Välj hello **konfigurera** knappen.

   ![Åtkomst till hello aktivera diagnostik alternativet](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Se [konfigurera diagnostik för virtuella datorer och Azure Cloud Services](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) för mer information.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Undvik att deklarera DbContext-objekt som statisk
### <a name="id"></a>ID
AP6000

### <a name="description"></a>Beskrivning
toosave minne undvika deklarerar DBContext-objekt som statisk.

Du kan dela idéer och feedback på [Azure kod analys feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Orsak
DBContext-objekt innehåller hello frågeresultat från varje anrop. Statiska DBContext-objekt är inte bort förrän hello programdomänen tas bort. Därför kan ett statiskt DBContext-objekt förbruka stora mängder minne.

### <a name="solution"></a>Lösning
Deklarera DBContext som en lokal variabel eller icke-statisk instansfält, använda den för en aktivitet och låt den avyttras efter användning.

hello följande exempel MVC kontrollantklassen visar hur toouse hello DBContext-objektet.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om hur du optimerar och felsöka Azure apps finns [felsöka en webbapp i Azure App Service med Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
