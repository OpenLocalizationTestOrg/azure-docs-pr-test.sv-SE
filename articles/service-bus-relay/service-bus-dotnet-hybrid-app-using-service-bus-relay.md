---
title: "aaaAzure vidarebefordrande WCF hybridprogram på lokalt/i molnet (.NET) | Microsoft Docs"
description: "Lär dig hur toocreate en .NET på lokalt/i molnet hybridprogram med hjälp av Azure WCF Relay."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: sethm
ms.openlocfilehash: aab8b1dbdc85c4edf7b0ccef0921b69524b2d306
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a>Lokalt eller molnbaserat .NET-hybridprogram med hjälp av Azure WCF Relay
## <a name="introduction"></a>Introduktion

Den här artikeln visar hur toobuild en hybrid cloud-program med Microsoft Azure och Visual Studio. hello kursen förutsätter att du har några tidigare erfarenheter av att använda Azure. På mindre än 30 minuter har du ett program som använder flera Azure-resurser och körs i hello moln.

Du kommer att lära dig:

* Hur toocreate eller anpassa en befintlig webbtjänst för användning av en webblösning.
* Hur toouse hello Azure WCF Relay service tooshare data mellan ett Azure-program och en webbtjänst värdbaserad någon annanstans.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Så här hjälper Azure Relay dig med hybridlösningar

Företagslösningar består normalt av en kombination av anpassad kod som skrivs tootackle nya och unika affärsbehov och befintliga funktioner som tillhandahålls av lösningar och system som redan finns på plats.

Lösningsarkitekter har börjat toouse hello molnet för enklare hantering av skalkrav och lägre driftskostnader. På så sätt kan hitta de befintliga tjänstetillgångarna som de vill ha tooleverage som byggblock för sina lösningar är innanför företagets brandvägg för hello och utanför enkelt nå för av hello molnlösning. Många interna tjänster är inte inbyggda eller värdbaserade på ett sätt att de enkelt kan exponeras vid företagets nätverksgräns för hello.

[Azure Relay](https://azure.microsoft.com/services/service-bus/) är avsedd för hello användningsfall där man tar befintliga Windows Communication Foundation (WCF) webbtjänster och göra de tjänster på ett säkert sätt komma åt toosolutions som finns utanför företagets perimeter för hello utan störande ändringar toohello företagets nätverksinfrastruktur. Sådana relay-tjänster är fortfarande inhysta i sin befintliga miljö men de delegerar lyssnandet efter inkommande sessioner och förfrågningar toohello molnbaserade vidarebefordrande tjänsten. Azure Relay skyddar även tjänsterna mot obehörig åtkomst med hjälp av [signatur för delad åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md)-autentisering.

## <a name="solution-scenario"></a>Lösningsscenario
I den här självstudiekursen skapar du en ASP.NET-webbplats som gör att toosee en lista över produkter på hello produktsidan för inventering.

![][0]

hello kursen förutsätter att du har produktinformation i ett befintligt lokalt system och använder Azure Relay tooreach till detta system. Det här simuleras av en webbtjänst som körs i ett enkelt konsolprogram och backas upp av en minnesintern uppsättning av produkter. Du kommer att kunna toorun detta konsolprogram på din dator och distribuera hello webbrollen till Azure. På så sätt, ser du hur hello-webbroll som körs i hello Azure-datacenter verkligen anropar din dator, även om datorn sannolikt ligger bakom en brandvägg och ett network address translation (NAT) lager.

## <a name="set-up-hello-development-environment"></a>Ställa in hello utvecklingsmiljö

Innan du kan börja utveckla Azure-program, hämta hello verktyg och ställa in din utvecklingsmiljö:

1. Installera hello Azure SDK för .NET från hello SDK [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. I hello **.NET** kolumn, och klicka på hello version av [Visual Studio](http://www.visualstudio.com) du använder. hello stegen i den här självstudiekursen används Visual Studio 2015, men de fungerar även med Visual Studio 2017.
3. När du uppmanas toorun eller spara hello installer, klickar du på **kör**.
4. I hello **installationsprogram för webbplattform**, klickar du på **installera** och fortsätta med installationen hello.
5. När hello installationen är klar har du allt nödvändigt toostart toodevelop hello app. hello SDK inkluderar verktyg som låter dig utveckla Azure-program i Visual Studio.

## <a name="create-a-namespace"></a>Skapa ett namnområde

toobegin med Hej relay funktioner i Azure, måste du först skapa ett namnområde för tjänsten. Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program. Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.

## <a name="create-an-on-premises-server"></a>Skapa en lokal server

Först ska du skapa ett lokalt produktkatalogsystem (ett fingerat sådant). Det kan vara ganska enkel; Du kan se detta som representerar ett faktiskt, lokalt produktkatalogsystem med en fullständig serviceyta som vi försöker toointegrate.

Det här projektet är ett konsolprogram i Visual Studio och använder hello [Azure Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus-bibliotek och konfigurationsinställningar.

### <a name="create-hello-project"></a>Skapa hello-projekt

1. Starta Microsoft Visual Studio med administratörsbehörighet. toodo så, högerklicka på programikonen för hello Visual Studio och klicka sedan på **kör som administratör**.
2. I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
3. Klicka på **Konsolprogram (.NET Framework** från **Installerade mallar** under **Visual C#**. I hello **namn** rutan, hello typnamn **ProductsServer**:

   ![][11]
4. Klicka på **OK** toocreate hello **ProductsServer** projekt.
5. Hoppa över toohello nästa steg om du redan har installerat hello NuGet package manager för Visual Studio. Annars går du till [NuGet][NuGet] och klickar på [Installera NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Följ hello prompter tooinstall hello NuGet-Pakethanteraren, och starta sedan om Visual Studio.
6. I Solution Explorer högerklickar du på hello **ProductsServer** projektet och klicka sedan på **hantera NuGet-paket**.
7. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Välj hello **WindowsAzure.ServiceBus** paketet.
8. Klicka på **installera**, och Godkänn hello villkor för användning.

   ![][13]

   Observera att hello krävs klientsammansättningarna nu refereras.
8. Lägg till en ny klass för ditt produktkontrakt. I Solution Explorer högerklickar du på hello **ProductsServer** projektet och klicka på **Lägg till**, och klicka sedan på **klassen**.
9. I hello **namn** rutan, hello typnamn **ProductsContract.cs**. Klicka sedan på **Lägg till**.
10. I **ProductsContract.cs**, Ersätt hello definitionen för namnområdet med följande kod, som definierar hello kontraktet för tjänsten hello hello.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define hello data contract for hello service
        [DataContract]
        // Declare hello serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define hello service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. Ersätt hello definitionen för namnområdet i Program.cs med hello följande kod, som lägger till hello profiltjänsten och hello värden för den.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement hello IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in hello service console application
            // when hello list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define hello Main() function in hello service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER tooclose");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. I Solution Explorer dubbelklickar du på hello **App.config** filen tooopen i hello Visual Studio-redigeraren. Längst ned hello hello `<system.ServiceModel>` element (men fortfarande inom `<system.ServiceModel>`), Lägg till hello följande XML-koden. Vara säker på att tooreplace *yourServiceNamespace* med hello namnet på ditt namnområdet och *yourKey* med hello SAS-nyckel som du tidigare hämtade från hello portal:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
13. Kvar i App.config i hello `<appSettings>` element, Ersätt hello anslutning strängvärde med hello anslutningssträng som du tidigare fick från hello-portalen.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. Tryck på **Ctrl + Skift + B** eller från hello **skapa** -menyn klickar du på **skapa lösning** toobuild hello program och kontrollera hello noggrannhet arbete hittills.

## <a name="create-an-aspnet-application"></a>Skapa ett ASP.NET-program

I det här avsnittet skapar du ett enkelt ASP.NET-program som visar data som hämtats från din produkttjänst.

### <a name="create-hello-project"></a>Skapa hello-projekt

1. Se till att Visual Studio körs med administratörsbehörighet.
2. I Visual Studio på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
3. Klicka på **ASP.NET webbapp (.NET Framwork)** från **Installerade mallar** under **Visual C#**. Namnet hello projektet **ProductsPortal**. Klicka sedan på **OK**.

   ![][15]

4. Från hello **ASP.NET mallar** listan i hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **MVC**.

   ![][16]

6. Klicka på hello **ändra autentisering** knappen. I hello **ändra autentisering** dialogrutan kontrollerar du att **ingen autentisering** är markerad och klicka sedan på **OK**. För den här självstudiekursen distribuerar du en app som inte kräver någon användarinloggning.

    ![][18]

7. Tillbaka i hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **OK** toocreate hello MVC-app.
8. Nu måste du konfigurera Azure-resurserna för en ny webbapp. Åtgärderna i hello hello [publicera tooAzure avsnitt i den här artikeln](../app-service-web/app-service-web-get-started-dotnet.md). Sedan returnera toothis självstudier och fortsätta toohello nästa steg.
10. I Solution Explorer högerklickar du på **Modeller**. Klicka sedan på **Lägg till** och på **Klass**. I hello **namn** rutan, hello typnamn **Product.cs**. Klicka sedan på **Lägg till**.

    ![][17]

### <a name="modify-hello-web-application"></a>Ändra hello-webbprogram

1. Ersätt hello befintliga definitionen för namnområdet med följande kod hello i hello Product.CS i Visual Studio.

   ```csharp
    // Declare properties for hello products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. I Solution Explorer expanderar du hello **domänkontrollanter** mapp, dubbelklicka på hello **HomeController.cs** filen tooopen den i Visual Studio.
3. I **HomeController.cs**, Ersätt hello befintliga definitionen för namnområdet med följande kod hello.

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of hello products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. Expandera hello-mappen Views\Shared i Solution Explorer och dubbelklicka sedan på **_Layout.cshtml** tooopen i hello Visual Studio-redigeraren.
5. Ändra alla förekomster av **My ASP.NET Application** för**LITWARE'S Products**.
6. Ta bort hello **Start**, **om**, och **Kontakta** länkar. Ta bort hello markerade koden i hello som följande exempel.

    ![][41]

7. Expandera hello-mappen Views\Home i Solution Explorer och dubbelklicka sedan på **Index.cshtml** tooopen i hello Visual Studio-redigeraren. Ersätt hello hela innehållet i filen hello med hello följande kod.

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. tooverify hello noggrannhet arbete så länge du kan trycka på **Ctrl + Skift + B** toobuild hello projektet.

### <a name="run-hello-app-locally"></a>Kör hello appen lokalt

Kör hello programmet tooverify att det fungerar.

1. Se till att **ProductsPortal** är hello aktiva projektet. Högerklicka på hello projektnamnet i Solution Explorer och markera **Set As Startup Project**.
2. I Visual Studio trycker du på **F5**.
3. Programmet bör visas och körs i en webbläsare.

   ![][21]

## <a name="put-hello-pieces-together"></a>Samla hello delar

hello nästa steg är toohook in hello lokala produktservern med hello ASP.NET-program.

1. Om den inte redan är öppen i Visual Studio öppnar den igen hello **ProductsPortal** projekt som du skapade i hello [skapa ett ASP.NET-program](#create-an-aspnet-application) avsnitt.
2. Liknande toohello steg i avsnittet ”Skapa en lokal Server” hello lägga till hello NuGet-paketet toohello projektreferenser. I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **hantera NuGet-paket**.
3. Sök efter ”Service Bus” och välj hello **WindowsAzure.ServiceBus** objekt. Sedan slutföra hello installationen och Stäng den här dialogrutan.
4. I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **Lägg till**, sedan **befintlig artikel**.
5. Navigera toohello **ProductsContract.cs** filen från hello **ProductsServer** console-projekt. Klicka på toohighlight ProductsContract.cs. Klicka på hello nedpilen bredvid för**Lägg till**, klicka på **Lägg till som länk**.

   ![][24]

6. Öppna hello **HomeController.cs** filen i hello Visual Studio-redigeraren och Ersätt hello definitionen för namnområdet med följande kod hello. Vara säker på att tooreplace *yourServiceNamespace* med hello namnet på namnområdet för tjänsten och *yourKey* med SAS-nyckel. Detta aktiverar hello klienten toocall hello lokala tjänsten och returnera hello resultatet av hello-anrop.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare hello channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of hello products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. I Solution Explorer högerklickar du på hello **ProductsPortal** lösning (se till att tooright klickar du på hello lösning, inte hello-projekt). Klicka på **Lägg till** och sedan på **Befintligt projekt**.
8. Navigera toohello **ProductsServer** projektet och sedan dubbelklicka på hello **ProductsServer.csproj** lösning filen tooadd den.
9. **ProductsServer** måste köras i ordning toodisplay hello data på **ProductsPortal**. I Solution Explorer högerklickar du på hello **ProductsPortal** lösningen och klicka på **egenskaper**. Hej **egenskapssidor** dialogrutan visas.
10. Klicka på vänster sida hello, **Startprojekt**. Klicka på hello höger **flera Startprojekt**. Se till att **ProductsServer** och **ProductsPortal** visas i den ordningen med **starta** som hello åtgärden för båda.

      ![][25]

11. Fortfarande i hello **egenskaper** dialogrutan klickar du på **projektberoenden** på hello vänster sida.
12. I hello **projekt** klickar du på **ProductsServer**. Se till att **ProductsPortal** inte är markerat.
13. I hello **projekt** klickar du på **ProductsPortal**. Se till att **ProductsServer** är markerat.

    ![][26]

14. Klicka på **OK** i hello **egenskapssidor** dialogrutan.

## <a name="run-hello-project-locally"></a>Kör hello projektet lokalt

tootest hello programmet lokalt, i Visual Studio trycker du på **F5**. hello lokal server (**ProductsServer**) bör starta först och sedan hello **ProductsPortal** bör programmet startas i ett webbläsarfönster. Den här gången ser du att hello produktinformation innehåller data som hämtats från hello produkten tjänsten lokalt system.

![][10]

Tryck på **uppdatera** på hello **ProductsPortal** sidan. Varje gång du uppdaterar hello sidan ser du hello appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.

Stänga båda programmen innan du fortsätter toohello nästa steg.

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a>Distribuera hello ProductsPortal-projektet tooan Azure-webbapp

hello nästa steg är toorepublish hello Azure Web app **ProductsPortal** klientdel. Hej du följande:

1. I Solution Explorer högerklickar du på hello **ProductsPortal** projektet och klicka på **publicera**. Klicka på **publicera** på hello **publicera** sidan.

  > [!NOTE]
  > Du kan se ett felmeddelande i webbläsarfönstret hello när hello **ProductsPortal** webbprojekt startas automatiskt efter hello-distribution. Detta är förväntat och beror på att hello **ProductsServer** programmet ännu inte körs.
>
>

2. Kopiera hello URL för hello distribueras webbapp som du behöver hello URL: en i hello nästa steg. Du kan även hämta denna URL från hello Azure App Service aktivitetsfönstret i Visual Studio:

  ![][9]

3. Stäng hello webbläsare fönstret toostop hello program körs.

### <a name="set-productsportal-as-web-app"></a>Ställa in ProductsPortal som en webbapp

Innan program som körs hello i hello molnet, måste du se till att **ProductsPortal** startas från Visual Studio som en webbapp.

1. I Visual Studio högerklickar du på hello **ProductsPortal** projektet och klicka sedan på **egenskaper**.
2. Klicka på hello vänstra kolumnen **Web**.
3. I hello **Starta åtgärden** klickar du på hello **starta URL** och i textrutan för hello anger hello URL för ditt tidigare distribuerade webbprogram, till exempel `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

4. Från hello **filen** -menyn i Visual Studio klickar du på **spara alla**.
5. Hello Build-menyn i Visual Studio klickar du på **återskapa lösning**.

## <a name="run-hello-application"></a>Kör programmet hello

1. Tryck på F5 toobuild och kör programmet hello. hello lokal server (hello **ProductsServer** konsolprogrammet) bör starta först och sedan hello **ProductsPortal** program ska starta i ett webbläsarfönster som visas i följande skärmbild hello bild. Observera igen att hello produktinformation visar data som hämtats från hello produkten tjänsten lokalt system och data i hello webbapp. Kontrollera hello URL toomake att som **ProductsPortal** körs i hello molnet, som en Azure-webbapp.

   ![][1]

   > [!IMPORTANT]
   > Hej **ProductsServer** konsolprogram måste vara körs och kan tooserve hello data toohello **ProductsPortal** program. Om ett felmeddelande visas i webbläsaren hello Vänta några sekunder för **ProductsServer** tooload och visa hello följande meddelande. Tryck på **uppdatera** i hello webbläsare.
   >
   >

   ![][37]
2. Tillbaka i hello webbläsaren trycker du på **uppdatera** på hello **ProductsPortal** sidan. Varje gång du uppdaterar hello sidan ser du hello appservern visar ett meddelande när `GetProducts()` från **ProductsServer** anropas.

    ![][38]

## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure Relay finns hello följande resurser:  

* [Vad är Azure Relay?](relay-what-is-it.md)  
* [Hur toouse relä](service-bus-dotnet-how-to-use-relay.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
