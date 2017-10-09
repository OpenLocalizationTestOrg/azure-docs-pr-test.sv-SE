---
title: "aaa.NET flernivåapp med hjälp av Azure Service Bus | Microsoft Docs"
description: "En .NET-självstudiekurs som hjälper dig att utveckla en flernivåapp i Azure som använder Service Bus-köer toocommunicate mellan nivåer."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET-flernivåapp med hjälp av Azure Service Bus-köer
## <a name="introduction"></a>Introduktion
Utveckla för Microsoft Azure är enkelt med Visual Studio och hello kostnadsfria Azure SDK för .NET. Den här självstudiekursen vägleder dig genom hello steg toocreate ett program som använder flera Azure-resurser som körs i din lokala miljö.

Får du lära dig hello följande:

* Hur tooenable datorn för Azure-utveckling med en enda hämta och installera.
* Hur toouse Visual Studio toodevelop för Azure.
* Hur toocreate en flernivåapp i Azure med hjälp av webb-och arbetsroller.
* Hur toocommunicate mellan nivåer med hjälp av Service Bus-köer.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

I den här självstudiekursen kommer du bygger och kör hello flernivåapp i en Azure-molntjänst. hello klientdelen är en ASP.NET MVC-webbroll och serverdelen hello är en arbetsroll som använder Service Bus-kö. Du kan skapa hello samma flernivåapp med klientdelen hello som ett webbprojekt som är distribuerade tooan Azure-webbplats i stället för en tjänst i molnet. Du kan också testa hello [.NET på lokalt/i molnet hybridprogram](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) kursen.

hello visar följande skärmbild som hello slutföra programmet.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Scenarioöversikt: kommunikation mellan roller
toosubmit en order för bearbetning, hello frontend UI-komponent, körs i hello webbrollen, måste interagera med hello mellannivålogik som körs i hello worker-rollen. Det här exemplet använder Service Bus-meddelanden för hello kommunikation mellan hello nivåer.

Med hjälp av Service Bus frikopplas-meddelanden mellan hello webben och mellannivåerna de båda komponenterna. Däremot toodirect messaging (d.v.s. TCP eller HTTP) hello webbnivå inte ansluta toohello mellannivå direkt; i stället skickar den arbetsenheter, som meddelanden, till Service Bus, som lagrar dem tills hello mellannivån är klar tooconsume och bearbeta dem på ett tillförlitligt sätt.

Service Bus innehåller två entiteter toosupport asynkrona meddelanden: köer och ämnen. Med köer skickas varje meddelande toohello kö används av en enda mottagare. Ämnena stödjer hello Publicera/prenumerera-mönster där varje publicerat meddelande görs tillgängligt tooa prenumeration som har registrerats med hello-avsnittet. Varje prenumeration underhåller logiskt nog sin egen meddelandekö. Prenumerationer kan också konfigureras med filterregler som begränsar hello uppsättning av meddelanden som skickas till hello prenumeration kön toothose som matchar filtret hello. hello används följande exempel Service Bus-köer.

![][1]

Denna kommunikationsmekanism har flera fördelar jämfört med funktioner för direkta meddelanden:

* **Tidsbestämd frikoppling.** Med hello mönstret för asynkrona meddelanden, producenter och konsumenter behöver inte vara online på hello samtidigt. Service Bus lagrar meddelanden på ett tillförlitligt sätt tills hello konsumerande parten är redo att ta emot dem.. Detta gör att hello komponenter i hello distribuerade program toobe frånkopplad, antingen frivilligt, till exempel för underhåll, eller på grund av tooa komponentkrasch, utan att påverka systemet som helhet. Dessutom kanske hello förbrukar program behöver bara toocome online vid vissa tidpunkter på dagen hello.
* **Belastningsutjämning.** I många program varierar systembelastningen över tiden, medan hello bearbetningstid som krävs för varje arbetsenhet vanligtvis är konstant. Medlingen mellan meddelandeproducenter och konsumenter med hjälp av en kö innebär att hello förbrukar program (hello arbetaren) endast måste toobe etableras tooaccommodate genomsnittlig belastning i stället för belastning. hello kön hello djup växer och dras samman allt eftersom hello inkommande belastningen varierar. Detta sparar direkt pengar beträffande hello mängden infrastruktur krävs tooservice hello programinläsning.
* **Belastningsutjämning.** Eftersom belastningen ökar kan läggas fler arbetsprocesser tooread från hello kö. Varje meddelande bearbetas av bara en av hello arbetsprocesser. Dessutom gör den här pull-baserade belastningsbalanseringen att optimal användning av arbetsdatorerna hello även om arbetsdatorerna skiljer sig åt vad gäller processorkraft: de hämtar meddelanden med sina egna högsta hastighet. Det här mönstret kallas ofta hello *konkurrerande konsument* mönster.
  
  ![][2]

hello följande avsnitt beskrivs hello-kod som implementerar denna arkitektur.

## <a name="set-up-hello-development-environment"></a>Ställa in hello utvecklingsmiljö
Innan du kan börja utveckla Azure-program, hämta hello verktyg och Ställ in din utvecklingsmiljö.

1. Installera hello Azure SDK för .NET från hello SDK [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. I hello **.NET** kolumn, och klicka på hello version av [Visual Studio](http://www.visualstudio.com) du använder. hello stegen i den här självstudiekursen används Visual Studio 2015, men de fungerar även med Visual Studio 2017.
3. När du uppmanas toorun eller spara hello installer, klickar du på **kör**.
4. I hello **installationsprogram för webbplattform**, klickar du på **installera** och fortsätta med installationen hello.
5. När hello installationen är klar har du allt nödvändigt toostart toodevelop hello app. hello SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio.

## <a name="create-a-namespace"></a>Skapa ett namnområde
hello nästa steg är toocreate ett namnområde för tjänsten och få en delad signatur åtkomst (SAS)-nyckel. Ett namnområde ger en appgräns för varje app som exponeras via Service Bus. SAS-nyckeln genereras av hello systemet när ett namnområde har skapats. hello kombinationen av namnområdet och SAS-nyckeln ger hello autentiseringsuppgifter för Service Bus tooauthenticate åtkomst tooan program.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Skapa en webbroll
I det här avsnittet skapar du hello klientdelen för ditt program. Först skapar du hello sidor som din app visar.
Efter det att lägga till kod som skickar objekt tooa Service Bus-kö och visar statusinformation om hello kö.

### <a name="create-hello-project"></a>Skapa hello-projekt
1. Starta Visual Studio med administratörsbehörighet: Högerklicka på hello **Visual Studio** programikonen och klicka sedan på **kör som administratör**. hello Azure compute emulator, som beskrivs senare i den här artikeln kräver att Visual Studio startas med administratörsbehörighet.
   
   I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
2. Från **Installerade mallar**, under **Visual C#**, klickar du på **Moln** och sedan på **Azure Cloud Service**. Namnet hello projektet **MultiTierApp**. Klicka sedan på **OK**.
   
   ![][9]
3. Dubbelklicka på **ASP.NET Web Role** från rollerna **.NET Framework 4.5**.
   
   ![][10]
4. Hovra över **WebRole1** under **Azure Cloud Service solution**och klicka på pennikonen hello Byt namn på hello webbroll för**FrontendWebRole**. Klicka sedan på **OK**. (Kontrollera att du anger ”Frontend” med ett litet ”e”, det vill säga, inte ”FrontEnd”).
   
   ![][11]
5. Från hello **nytt ASP.NET-projekt** i dialogrutan hello **Välj en mall** klickar du på **MVC**.
   
   ![][12]
6. Fortfarande i hello **nytt ASP.NET-projekt** dialogrutan klickar du på hello **ändra autentisering** knappen. I hello **ändra autentisering** dialogrutan klickar du på **ingen autentisering**, och klicka sedan på **OK**. För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.
   
    ![][16]
7. Tillbaka i hello **nytt ASP.NET-projekt** dialogrutan klickar du på **OK** toocreate hello projektet.
8. I **Solution Explorer**, i hello **FrontendWebRole** projekt, högerklicka på **referenser**, klicka på **hantera NuGet-paket**.
9. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Välj hello **WindowsAzure.ServiceBus** paketet genom att klicka på **installera**, och Godkänn hello villkor för användning.
   
   ![][13]
   
   Observera att hello krävs klientsammansättningarna nu refereras och vissa nya kodfiler har lagts till.
10. I **Solution Explorer** högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**. I hello **namn** rutan, hello typnamn **OnlineOrder.cs**. Klicka sedan på **Lägg till**.

### <a name="write-hello-code-for-your-web-role"></a>Skriva hello koden för webbrollen
I det här avsnittet skapar du hello olika sidor som din app visar.

1. Ersätt den befintliga definitionen för namnområdet med följande kod hello i hello OnlineOrder.cs-filen i Visual Studio:
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**. Lägg till följande hello **med** instruktioner överst hello i hello filen tooinclude hello namnområdena för den modell som du precis skapade, samt Service Bus.
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. Ersätt även den befintliga definitionen för namnområdet med följande kod hello i hello HomeController.cs filen i Visual Studio. Den här koden innehåller metoder för att hantera hello överföring av objekt toohello kö.
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. Från hello **skapa** -menyn klickar du på **skapa lösning** tootest hello noggrannhet arbete hittills.
5. Nu skapar hello vy för hello `Submit()` metod som du skapade tidigare. Högerklicka inne hello `Submit()` metod (hello överlagring för `Submit()` som tar några parametrar), och välj sedan **Lägg till vy**.
   
   ![][14]
6. En dialogruta för att skapa hello vyn. I hello **mallen** Välj **skapa**. I hello **Modellklass** klickar du på hello **OnlineOrder** klass.
   
   ![][15]
7. Klicka på **Lägg till**.
8. Nu ska du ändra hello visas namnet på ditt program. I **Solution Explorer**, dubbelklicka på den **Views\Shared\\_Layout.cshtml** filen tooopen i hello Visual Studio-redigeraren.
9. Ersätt alla förekomster av **My ASP.NET Application** med **LITWARE'S Products**.
10. Ta bort hello **Start**, **om**, och **Kontakta** länkar. Ta bort hello markerade koden:
    
    ![][28]
11. Slutligen ändra hello skickas sidan tooinclude viss information om hello kö. I **Solution Explorer**, dubbelklicka på den **Views\Home\Submit.cshtml** filen tooopen i hello Visual Studio-redigeraren. Lägg till följande rad efter hello `<h2>Submit</h2>`. Nu hello `ViewBag.MessageCount` är tom. Du kommer att fylla i denna senare.
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. Du har nu implementerat ditt användargränssnitt (UI). Du kan trycka på **F5** toorun programmet och bekräfta att det ser ut som förväntat.
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a>Skriva hello-kod för att skicka objekt tooa Service Bus-kö
Lägg nu till kod för att skicka objekt tooa kön. Först måste du skapa en klass som innehåller anslutningsinformation för Service Bus-kön. Sedan initierar du anslutningen från Global.aspx.cs. Slutligen uppdatera hello överföringskod som du skapade tidigare i HomeController.cs tooactually skicka objekt tooa Service Bus-kö.

1. I **Solution Explorer**, högerklicka på **FrontendWebRole** (Högerklicka på hello projektet, inte hello rollen). Klicka på **Lägg till** och sedan på **Klass**.
2. Namnge hello klassen **QueueConnector.cs**. Klicka på **Lägg till** toocreate hello-klassen.
3. Lägg nu till kod som kapslar in hello anslutningsinformationen och initierar hello anslutning tooa Service Bus-kö. Ersätt hello hela innehållet i QueueConnector.cs med följande kod hello och ange värden för `your Service Bus namespace` (namnet på ditt namnområde) och `yourKey`, vilket är hello **primärnyckel** du tidigare fick från hello Azure portalen.
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. Se nu till att din **initiera**-metod anropas. Dubbelklicka på **Global.asax\Global.asax.cs** i **Solution Explorer**.
5. Lägg till följande kodrad i slutet av hello av hello hello **Application_Start** metod.
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Slutligen uppdatera hello web-kod som du skapade tidigare, för att skicka objekt toohello kön. I **Solution Explorer** dubbelklickar du på **Controllers\HomeController.cs**.
7. Uppdatera hello `Submit()` metod (hello överlagring som inte tar några parametrar) enligt följande tooget hello-meddelande räkna hello kön.
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Uppdatera hello `Submit(OnlineOrder order)` metod (hello överlagring som tar några parametrar) enligt följande toosubmit ordning information toohello kön.
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Du kan nu köra hello programmet igen. Varje gång du skickar en order, ökar antal för hello-meddelande.
   
   ![][18]

## <a name="create-hello-worker-role"></a>Skapa hello worker-rollen
Nu ska du skapa hello arbetsroll som behandlar orderöverföringen hello. Det här exemplet används hello **Arbetsroll med Service Bus-kö** projektmall för Visual Studio. Du har redan fått hello krävs autentiseringsuppgifter från hello-portalen.

1. Kontrollera att du har anslutit Visual Studio tooyour Azure-konto.
2. I Visual Studio i **Solution Explorer** högerklickar du på den **roller** mapp under hello **MultiTierApp** projekt.
3. Klicka på **Lägg till** och sedan på **Nytt arbetsrollprojekt**. Hej **Lägg till nytt Rollprojekt** dialogrutan visas.
   
   ![][26]
4. I hello **Lägg till nytt Rollprojekt** dialogrutan klickar du på **Arbetsroll med Service Bus-kö**.
   
   ![][23]
5. I hello **namn** rutan, namnet hello projekt **OrderProcessingRole**. Klicka sedan på **Lägg till**.
6. Kopiera hello anslutningssträng som du hämtade i steg 9 av hello ”skapa ett namnområde för Service Bus” avsnittet toohello Urklipp.
7. I **Solution Explorer**, högerklicka på hello **OrderProcessingRole** du skapade i steg 5 (se till att du högerklickar på **OrderProcessingRole** under **Roller**, och inte hello klassen). Klicka sedan på **Egenskaper**.
8. På hello **inställningar** för hello **egenskaper** dialogrutan klickar du inuti hello **värdet** för **Microsoft.ServiceBus.ConnectionString**, och klistra in hello slutpunktsvärdet som du kopierade i steg 6.
   
   ![][25]
9. Skapa en **OnlineOrder** klassen toorepresent hello order som du behandlar dem från hello kö. Du kan återanvända en klass som du redan har skapat. I **Solution Explorer**, högerklicka på hello **OrderProcessingRole** klass (Högerklicka på hello klassikonen, inte hello rollen). Klicka på **Lägg till** och sedan på **Befintligt objekt**.
10. Bläddra toohello undermapp för **FrontendWebRole\Models**, och dubbelklicka sedan på **OnlineOrder.cs** tooadd den toothis projekt.
11. I **WorkerRole.cs**, ändra hello värdet på hello **könamn** variabel från `"ProcessingQueue"` för`"OrdersQueue"` enligt hello följande kod.
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Lägg till hello följande med instruktionen hello överst i hello WorkerRole.cs fil.
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. I hello `Run()` funktionen inuti hello `OnMessage()` anropa, ersätter hello innehållet i hello `try` -sats med hello följande kod.
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Du har slutfört hello program. Du kan testa hello fullständiga programmet genom att högerklicka på hello MultiTierApp-projektet i Solution Explorer välja **Set as Startup Project**, och sedan trycka på F5. Observera att antalet meddelanden inte ökar, eftersom hello arbetsrollen behandlar objekt från kön hello och markerar dem som slutförd. Du kan se hello spårningsutdata från arbetsrollen genom att visa hello Azure Compute Emulator UI. Du kan göra detta genom att högerklicka hello emulatorikonen i hello meddelandefältet i Aktivitetsfältet och välja **visa Compute Emulator UI**.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>Nästa steg
toolearn mer om Service Bus finns hello följande resurser:  

* [Dokumentation för Azure Service Bus][sbdocs]  
* [Tjänstesida för Service Bus][sbacom]  
* [Hur tooUse Service Bus-köer][sbacomqhowto]  

toolearn mer om flernivåscenarier, se:  

* [.NET-flernivåapp med hjälp av lagringstabeller, köer och blob-objekt][mutitierstorage]  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
