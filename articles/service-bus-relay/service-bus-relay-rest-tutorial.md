---
title: "aaaService Bus REST-självstudiekurs med hjälp av Azure Relay | Microsoft Docs"
description: "Skapa en enkel Azure Service Bus relay värdapp som visar ett REST-baserat gränssnitt."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: b68650993a0390e7cef891ccb4236095cd86d4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a>Självstudiekurs för Azure WCF Relay REST

Den här självstudiekursen beskrivs hur toobuild ett enkelt Azure-relä värd för program som visar ett REST-baserat gränssnitt. REST kan du ge en webbklient, till exempel en webbläsare, tooaccess hello Service Bus-API: er via HTTP-begäranden.

hello kursen använder hello Windows Communication Foundation (WCF) REST API modellen tooconstruct en REST-tjänst på Service Bus. Mer information finns i [programmeringsmodellen WCF REST](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) och [utforma och implementera tjänster](/dotnet/framework/wcf/designing-and-implementing-services) i hello WCF-dokumentationen.

## <a name="step-1-create-a-namespace"></a>Steg 1: Skapa ett namnområde

toobegin med Hej relay funktioner i Azure, måste du först skapa ett namnområde för tjänsten. Ett namnområde tillhandahåller en omfångsbehållare för adressering av Azure-resurser i ditt program. Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a>Steg 2: Definiera en REST-baserat WCF-tjänsten kontraktet toouse med Azure-relä

När du skapar en tjänst för WCF REST-format, måste du definiera hello kontraktet. hello kontraktet anger vilka åtgärder hello värden stöder. En tjänsteåtgärd kan anses vara en webbtjänstemetod. Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic. Varje metod i gränssnittet hello motsvarar tooa specifik tjänsteåtgärd. Hej [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribut måste vara tillämpade tooeach gränssnitt och hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attributet måste vara tillämpade tooeach igen. Om en metod i ett gränssnitt som har hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) saknar hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), inte exponeras metoden. hello-kod som används för dessa aktiviteter visas i hello-exemplet som följer hello proceduren.

hello främsta skillnaden mellan en WCF-kontrakt och ett kontrakt i REST-format är hello lägga till en egenskap toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Den här egenskapen gör toomap en metod i gränssnittet tooa metoden på hello andra sidan av hello-gränssnittet. I det här fallet vi använder [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink metod-tooHTTP GET. Detta gör att Service Bus tooaccurately hämta och tolka kommandon som skickas toohello gränssnitt.

### <a name="toocreate-a-contract-with-an-interface"></a>toocreate ett kontrakt med ett gränssnitt

1. Öppna Visual Studio som administratör: Högerklicka på hello programmet i hello **starta** -menyn och klicka sedan på **kör som administratör**.
2. Skapa ett nytt konsolappsrojekt. Klicka på hello **filen** -menyn och välj **ny**och välj **projekt**. I hello **nytt projekt** dialogrutan klickar du på **Visual C#**väljer hello **konsolprogram** mall, och ger den namnet **ImageListener**. Använd hello standard **plats**. Klicka på **OK** toocreate hello projektet.
3. Visual Studio skapar en `Program.cs`-fil för ett C#-projekt. Den här klassen innehåller en tom `Main()` metod som krävs för en konsol programmet projektet toobuild korrekt.
4. Lägg till referenser tooService Bus och **System.ServiceModel.dll** toohello projektet genom att installera hello Service Bus NuGet-paketet. Det här paketet lägger automatiskt till referenser toohello Service Bus-bibliotek, samt hello WCF **System.ServiceModel**. I Solution Explorer högerklickar du på hello **ImageListener** projektet och klicka sedan på **hantera NuGet-paket**. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Klicka på **installera**, och Godkänn hello villkor för användning.
5. Du måste uttryckligen lägga till en referens för**System.ServiceModel.Web.dll** toohello projektet:
   
    a. I Solution Explorer högerklickar du på hello **referenser** mapp under hello projektmappen, och klicka sedan på **Lägg till referens**.
   
    b. I hello **Lägg till referens** dialogrutan klickar du på hello **Framework** fliken hello vänster och hello **Sök** skriver **System.ServiceModel.Web** . Välj hello **System.ServiceModel.Web** kryssrutan och klicka sedan på **OK**.
6. Lägg till följande hello `using` instruktioner överst hello i hello Program.cs-filen.
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) är hello-namnområde som ger programmatisk åtkomst toobasic funktioner i WCF. Vidarebefordrande WCF använder många av hello objekt och attribut för WCF toodefine tjänstekontrakt. Du använder det här namnområdet i de flesta relay-appar. På liknande sätt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) hjälper dig att definiera hello kanalen, vilket är hello-objekt som du kommunicera med Azure Relay och hello klientens webbläsare. Slutligen [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) innehåller hello-typer som gör att du toocreate webbaserade program.
7. Byt namn på hello `ImageListener` namnområde för**Microsoft.ServiceBus.Samples**.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Direkt efter hello öppna klammerparentesen för namnområdesdeklarationen hello, definiera ett nytt gränssnitt med namnet **IImageContract** och tillämpa hello **ServiceContractAttribute** attributet toohello gränssnitt med en värdet för `http://samples.microsoft.com/ServiceModel/Relay/`. Hej namnområdesvärdet skiljer sig från hello-namnområde som du använder under hela hello omfånget för din kod. hello namnområdesvärdet används som en unik identifierare för det här kontraktet och bör innehålla versionsinformation. Mer information finns i [Versionshantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498). Att ange hello namnområdet uttryckligen förhindrar att hello standardvärdet för namnområdet som läggs till toohello kontraktnamn.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. Inom hello `IImageContract` gränssnitt, deklarera en metod för hello gång hello `IImageContract` kontrakt visar i hello gränssnitt och tillämpa hello `OperationContractAttribute` attributet toohello metod som du vill tooexpose som en del av hello offentliga Service Bus kontrakt.
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. I hello **OperationContract** attribut, lägga till hello **WebGet** värde.
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    Om du gör det möjliggör hello vidarebefordrande tjänsten tooroute HTTP GET-begäranden för`GetImage`, och tootranslate hello returnera värden av `GetImage` till en HTTP GETRESPONSE svaret. Senare i hello självstudiekursen ska du använda en web webbläsare tooaccess den här metoden och toodisplay hello avbildningen i hello webbläsare.
11. Direkt efter hello `IImageContract` definition, deklarerar du en kanal som ärver från både hello `IImageContract` och `IClientChannel` gränssnitt.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    En kanal är hello WCF-objekt via vilken hello-tjänsten och klienten skickar information tooeach andra. Senare skapar du hello kanalen i din värdapp. Azure Relay använder sedan den här kanalen toopass hello HTTP GET-förfrågningar från hello webbläsare tooyour **GetImage** implementering. hello relay använder också hello kanal tootake hello **GetImage** returvärde och omvandla det till en HTTP GETRESPONSE för hello klientens webbläsare.
12. Från hello **skapa** -menyn klickar du på **skapa lösning** tooconfirm hello noggrannhet arbete hittills.

### <a name="example"></a>Exempel
hello följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a>Steg 3: Implementera ett kontrakt toouse för REST-baserat WCF-tjänsten Service Bus
Skapa ett REST-format WCF vidarebefordrande tjänsten måste du först skapa hello kontrakt som definieras med hjälp av ett gränssnitt. hello nästa steg är tooimplement hello gränssnitt. Detta innebär att du skapar en klass som heter **ImageService** som implementerar hello användardefinierade **IImageContract** gränssnitt. När du implementerar hello kontraktet, konfigurerar du sedan hello-gränssnittet genom att använda en App.config-fil. hello konfigurationsfilen innehåller information som behövs för hello program, till exempel hello namnet på hello tjänst, hello namn hello kontraktet och hello typ av protokoll som används toocommunicate med hello vidarebefordrande tjänsten. hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.

Som med hello föregående steg, det är mycket lite skillnad mellan att implementera ett kontrakt i REST-format och ett vidarebefordrande WCF-kontrakt.

### <a name="tooimplement-a-rest-style-service-bus-contract"></a>Service Bus-kontrakt tooimplement REST-format
1. Skapa en ny klass med namnet **ImageService** direkt efter hello definition av hello **IImageContract** gränssnitt. Hej **ImageService** klassen implementerar hello **IImageContract** gränssnitt.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Liknande tooother gränssnittsimplementationer du kan implementera hello definition i en annan fil. Men för den här självstudiekursen hello visas implementeringen i samma fil som gränssnittsdefinitionen hello hello och `Main()` metod.
2. Tillämpa hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attributet toohello **IImageService** klassen tooindicate som hello klassen är en implementering av ett WCF-kontrakt.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Som tidigare nämnts är det här namnområdet inte ett traditionellt namnområde. I stället är den del av hello WCF-arkitekturen som identifierar hello kontraktet. Mer information finns i hello [datakontraktnamn](https://msdn.microsoft.com/library/ms731045.aspx) -avsnittet i hello WCF-dokumentationen.
3. Lägg till en .jpg-bild tooyour projektet.  
   
    Det här är en bild som hello tjänsten visar i hello mottagning av webbläsaren. Högerklicka på ditt projekt och klicka sedan på **Lägg till**. Klicka därefter på **Befintligt objekt**. Använd hello **Lägg till befintligt objekt** dialogrutan rutan toobrowse tooan lämpliga .jpg och klicka sedan på **Lägg till**.
   
    När du lägger till hello-fil, se till att **alla filer** väljs i hello listrutan nästa toohello **filnamn:** fält. hello resten av den här kursen förutsätter att hello namn på hello bild är ”image.jpg”. Om du har en annan fil, kommer du ha toorename hello bild eller ändra toocompensate din kod.
4. toomake till att hello kör tjänsten hittar hello bildfil i **Solution Explorer** Högerklicka hello image-filen och klicka på **egenskaper**. I hello **egenskaper** ställer du in **kopiera tooOutput Directory** för**kopiera om nyare**.
5. Lägg till en referens toohello **System.Drawing.dll** sammansättningen toohello projekt och Lägg även till hello följande associerade `using` instruktioner.  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. I hello **ImageService** klassen, lägga till hello följande konstruktor att belastningar hello bitmappen och förbereder toosend den toohello klientens webbläsare.
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. Direkt efter föregående kod hello Lägg till följande hello **GetImage** metod i hello **ImageService** klassen tooreturn ett HTTP-meddelande som innehåller hello avbildningen.
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    Den här implementeringen använder **MemoryStream** tooretrieve hello bilden och förbereda den för strömning toohello webbläsare. Den börjar hello strömningspositionen vid noll, deklarerar hello ströminnehållet som jpeg och strömmar hello information.
8. Från hello **skapa** -menyn klickar du på **skapa lösning**.

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a>toodefine hello konfiguration för att köra hello webbtjänsten på Service Bus
1. I **Solution Explorer**, dubbelklicka på **App.config** tooopen i hello Visual Studio-redigeraren.
   
    Hej **App.config** filen innehåller hello tjänstnamn, slutpunkten (det vill säga hello plats som Azure Relay visar för klienter och värdar toocommunicate med varandra) och bindningen (hello typ av protokoll som används toocommunicate). hello största skillnaden här är den hello konfigurerade tjänstslutpunkten refererar tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning.
2. Hej `<system.serviceModel>` XML-elementet är ett WCF-element som definierar en eller flera tjänster. Det kan använda toodefine hello tjänstenamnet och slutpunkten. Längst ned hello hello `<system.serviceModel>` element (men fortfarande inom `<system.serviceModel>`), Lägg till en `<bindings>` element som har hello följande innehåll. Detta definierar hello bindningar som används i hello program. Du kan definiera flera bindningar men i den här kursen definierar du bara en.
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    hello föregående kod definierar en WCF-relä [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bindning med **relayClientAuthenticationType** ställa in också**ingen**. Den här inställningen anger att en slutpunkt som använder den här bindningen inte kräver autentiseringsuppgifter för klienten.
3. Efter hello `<bindings>` element, lägga till en `<services>` element. Liknande toohello bindningar kan du definiera flera tjänster i en enda konfigurationsfil. I den här självstudiekursen kommer du dock bara att definiera en.
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    Det här steget konfigurerar en tjänst som använder hello som tidigare definierats standard **webHttpRelayBinding**. Använder också hello standard **sbTokenProvider**, som har definierats i hello nästa steg.
4. Efter hello `<services>` elementet, skapar en `<behaviors>` element med hello efter innehåll, Ersätt ”SAS_KEY” med hello *signatur för delad åtkomst* (SAS)-nyckel som du tidigare fick från hello [Azure-portalen ][Azure portal].
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. Kvar i App.config i hello `<appSettings>` element, Ersätt hello hela anslutningen strängvärde med hello anslutningssträng som du tidigare fick från hello-portalen. 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. Från hello **skapa** -menyn klickar du på **skapa lösning** toobuild hello hela lösningen.

### <a name="example"></a>Exempel
hello följande kod visar hello kontrakt- och tjänsteimplementeringen för en REST-baserad tjänst som körs på Service Bus använder hello **WebHttpRelayBinding** bindning.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

hello visar följande exempel hello App.config-fil som är associerad med hello-tjänsten.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove hello ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a>Steg 4: Värdbasera hello REST-baserat WCF-tjänsten toouse Azure Relay
Det här steget beskriver hur toorun en tjänst med hjälp av ett konsolprogram med WCF Relay. En fullständig lista över hello-kod som skrivs i det här steget finns i hello-exemplet som följer hello proceduren.

### <a name="toocreate-a-base-address-for-hello-service"></a>toocreate en basadress för hello-tjänsten
1. I hello `Main()` funktionsdeklarationen, skapa en variabel toostore hello namnområdet för ditt projekt. Se till att tooreplace `yourNamespace` med hello namnet hello Relay-namnområde som du skapade tidigare.
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus använder hello namnet på ditt namnområde toocreate ett unikt URI.
2. Skapa en `Uri` hello service som baseras på namnområdet hello-instans för hello basadress.
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a>toocreate och konfigurera hello webbtjänstevärden
* Skapa hello webbtjänstevärden med hjälp av hello URI-adress som skapade tidigare i det här avsnittet.
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    hello tjänstvärden är hello WCF-objekt som instantierar värdprogrammet hello. Det här exemplet skickar den hello typen värden som du vill toocreate (en **ImageService**), och även hello adress som du vill tooexpose hello värdprogrammet.

### <a name="toorun-hello-web-service-host"></a>toorun hello webbtjänstevärden
1. Öppna hello-tjänsten.
   
    ```csharp
    host.Open();
    ```
    hello-tjänsten körs nu.
2. Visa ett meddelande som anger att hello-tjänsten körs och hur toostop hello service.
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. När du är klar stänger du hello tjänstvärden.
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a>Exempel
hello följande exempel innehåller hello tjänstekontraktet och implementeringen från föregående steg i hello självstudier och värdar hello tjänsten i ett konsolprogram. Kompilera hello följande kod i en körbar fil med namnet ImageListener.exe.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-hello-code"></a>Kompilera koden hello
När du har skapat hello lösningen hello följande toorun hello program:

1. Tryck på **F5**, eller bläddra toohello körbara filplatsen (ImageListener\bin\Debug\ImageListener.exe) toorun hello-tjänsten. Behåll hello app körs, eftersom det är nödvändigt tooperform hello nästa steg.
2. Kopiera och klistra in hello-adress från hello Kommandotolken i en webbläsare toosee hello avbildningen.
3. När du är klar trycker du på **RETUR** i hello kommandotolk-fönster tooclose hello app.

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett program som använder hello Service Bus relay-tjänst finns i följande artiklar toolearn mer om Azure Relay hello:

* [Azure Service Bus-Arkitekturöversikt](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Översikt över Azure Relay](relay-what-is-it.md)
* [Hur toouse hello WCF vidarebefordrande tjänsten med .NET](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
