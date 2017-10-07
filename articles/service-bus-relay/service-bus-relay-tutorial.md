---
title: aaaAzure Service Bus WCF Relay kursen | Microsoft Docs
description: "Skapa en Service Bus-klient och en tjänst med hjälp av WCF Relay."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: sethm
ms.openlocfilehash: 78cd52ef51e9fcfcda2f13ec54bde3af50d76476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-tutorial"></a>Azure vidarebefordrande WCF-självstudier

Den här självstudiekursen beskriver hur toobuild en enkel WCF-relä klientprogram och en tjänst med Azure Relay. För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Gå igenom den här kursen får du förstå hello steg som är nödvändiga toocreate vidarebefordrande WCF-klienten och tjänsten program. En tjänst är en konstruktion som Exponerar en eller flera slutpunkter som Exponerar en eller flera tjänsteåtgärder som deras ursprungliga motsvarigheter i WCF. hello-slutpunkten för en tjänst anger en adress där tjänsten hello hittar du en bindning som innehåller hello information som en klient måste kommunicera med hello-tjänsten och ett kontrakt som definierar hello funktionalitet som tillhandahålls av hello service tooits klienter. hello största skillnaden mellan WCF- och WCF Relay är att hello slutpunkten exponeras i hello molnet i stället för lokalt på datorn.

När du arbetar med hello sekvens av rubriker i den här kursen har du en aktiv tjänst och en klient som kan anropa hello åtgärder av hello-tjänsten. hello det första avsnittet beskriver hur tooset ett konto. hello nästa steg beskriver hur toodefine en tjänst som använder ett kontrakt, hur tooimplement hello tjänsten och hur tooconfigure hello tjänsten i kod. Här beskrivs också hur toohost och kör hello-tjänsten. hello tjänsten som skapas är själva värdbaserade och hello klienten och tjänsten körs på hello samma dator. Du kan konfigurera hello tjänsten med hjälp av koden eller en konfigurationsfil.

hello de tre sista stegen beskriver hur toocreate ett klientprogram, konfigurera hello klientprogrammet och skapar och använder en klient som kan komma åt hello funktionerna i hello värden.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen behöver du hello följande:

* [Microsoft Visual Studio 2015 eller senare](http://visualstudio.com). Den här självstudiekursen använder Visual Studio 2017.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).

## <a name="create-a-service-namespace"></a>Skapa ett namnområde för tjänsten

hello första steget är toocreate ett namnområde och tooobtain en [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) nyckel. Ett namnområde ger en programgräns för varje program som exponeras via hello vidarebefordrande tjänsten. SAS-nyckeln genereras automatiskt av hello systemet när ett namnområde för tjänsten har skapats. hello kombinationen av namnområdet för tjänsten och SAS-nyckeln ger hello autentiseringsuppgifter för Azure tooauthenticate åtkomst tooan program. Följ hello [anvisningarna här](relay-create-namespace-portal.md) toocreate en Relay-namnrymd.

## <a name="define-a-wcf-service-contract"></a>Definiera ett WCF-tjänstekontrakt

hello tjänstekontraktet anger vilka åtgärder (hello webbserviceterminologin för metoder eller funktioner) hello tjänsten stöder. Kontrakt skapas genom att definiera ett gränssnitt för C++, C# eller Visual Basic. Varje metod i gränssnittet hello motsvarar tooa specifik tjänsteåtgärd. Varje gränssnitt måste ha hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) använda attributet tooit och varje åtgärd måste ha hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) tooit-attribut som används. Om en metod i ett gränssnitt som har hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) har ingen hälsningspaket [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attributet, inte den metoden är exponerad. hello-koden för dessa uppgifter finns i hello-exemplet som följer hello proceduren. En utförlig beskrivning av kontrakt och tjänster finns [utforma och implementera tjänster](https://msdn.microsoft.com/library/ms729746.aspx) i hello WCF-dokumentationen.

### <a name="create-a-relay-contract-with-an-interface"></a>Skapa en relay-kontrakt med ett gränssnitt

1. Öppna Visual Studio som administratör genom att högerklicka på programmet hello i hello **starta** menyn och välja **kör som administratör**.
2. Skapa ett nytt konsolappsrojekt. Klicka på hello **filen** -menyn och välj **ny**, klicka sedan på **projekt**. I hello **nytt projekt** dialogrutan klickar du på **Visual C#** (om **Visual C#** inte visas, tittar du under **andra språk**). Klicka på hello **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoService**. Klicka på **OK** toocreate hello projektet.

    ![][2]

3. Installera hello Service Bus NuGet-paketet. Det här paketet lägger automatiskt till referenser toohello Service Bus-bibliotek, samt hello WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello-namnområde som du kan använda tooprogrammatically åtkomst hello grundläggande funktioner i WCF. Service Bus använder många av hello objekt och attribut för WCF toodefine tjänstekontrakt.

    Högerklicka på hello projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket...** . Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Se till att hello projektnamnet är markerat i hello **versioner** rutan. Klicka på **installera**, och Godkänn hello villkor för användning.

    ![][3]
4. I Solution Explorer dubbelklickar du på hello Program.cs-filen tooopen den i Redigeraren för hello, om den inte redan är öppen.
5. Lägg till hello följande using-instruktioner överst hello i hello-filen:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Ändra hello namnområdesnamnet från standardnamnet **EchoService** för**Microsoft.ServiceBus.Samples**.

   > [!IMPORTANT]
   > Den här kursen använder hello C#-namnområdet **Microsoft.ServiceBus.Samples**, som är hello namnområdet för hello kontrakt-baserade hanterad typ som används i hello konfigurationsfilen i hello [konfigurera hello WCF-klient](#configure-the-wcf-client) steg. Du kan ange vilken namnområden du vill när du skapar det här exemplet; hello kursen fungerar inte om du ändrar hello namnområden hello kontrakt- och därför i hello programmets konfigurationsfil. hello namnområdet som angetts i hello App.config-filen måste vara hello samma som hello namnområdet som angetts i C#-filerna.
   >
   >
7. Direkt efter hello `Microsoft.ServiceBus.Samples` namnområdesdeklaration, men i hello-namnområdet, definierar du ett nytt gränssnitt med namnet `IEchoContract` och tillämpa hello `ServiceContractAttribute` attributet toohello gränssnittet med ett namnområdesvärde på `http://samples.microsoft.com/ServiceModel/Relay/`. Hej namnområdesvärdet skiljer sig från hello-namnområde som du använder under hela hello omfånget för din kod. I stället används hello namnområdesvärdet som en unik identifierare för det här kontraktet. Att ange hello namnområdet uttryckligen förhindrar att hello standardvärdet för namnområdet som läggs till toohello kontraktnamn. Klistra in följande kod efter hello namnområdesdeklaration hello:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > Hello namnområdet för tjänstekontraktet innehåller vanligtvis ett namngivningsschema som inkluderar information om versionen. Med versionsinformation i namnområdet för tjänstekontraktet hello kan tjänster tooisolate större ändringar genom att definiera ett nytt tjänstkontrakt med ett nytt namnområde och exponera det på en ny slutpunkt. På så sätt kan klienter fortsätta toouse hello gamla tjänstkontraktet utan toobe uppdateras. Versionsinformation kan bestå av ett datum eller ett build-nummer. Mer information finns i [Versionshantering för tjänster](http://go.microsoft.com/fwlink/?LinkID=180498). För hello i den här kursen är innehåller hello naming schemat för namnområdet för tjänstekontraktet hello inte versionsinformation.
   >
   >
8. Inom hello `IEchoContract` gränssnitt, deklarera en metod för hello gång hello `IEchoContract` kontrakt visar i hello gränssnitt och tillämpa hello `OperationContractAttribute` attributet toohello metod som du vill tooexpose som en del av hello offentliga WCF-relä minimera, enligt följande:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. Direkt efter hello `IEchoContract` deklarerar du en kanal som ärver från både `IEchoContract` och även toohello `IClientChannel` gränssnitt som visas här:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    En kanal är hello WCF-objekt via vilken hello värden och klienten skickar information tooeach andra. Senare kan skriva du kod mot hello kanal tooecho information mellan två hello-program.
10. Från hello **skapa** -menyn klickar du på **skapa lösning** eller tryck på **Ctrl + Skift + B** tooconfirm hello noggrannhet arbete hittills.

### <a name="example"></a>Exempel

hello följande kod visar ett grundläggande gränssnitt som definierar ett vidarebefordrande WCF-kontrakt.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nu när hello gränssnittet har skapats kan implementera du hello-gränssnittet.

## <a name="implement-hello-wcf-contract"></a>Implementera hello WCF-tjänstekontrakt

Skapa ett Azure relay måste du först skapa hello kontrakt som definieras med hjälp av ett gränssnitt. Mer information om hur du skapar hello-gränssnittet finns hello föregående steg. hello nästa steg är tooimplement hello gränssnitt. Detta innebär att du skapar en klass som heter `EchoService` som implementerar hello användardefinierade `IEchoContract` gränssnitt. När du implementerar gränssnittet hello, konfigurerar du sedan hello-gränssnittet genom att använda konfigurationsfilen App.config. hello konfigurationsfilen innehåller information som behövs för hello program, till exempel hello namnet på hello tjänst, hello namn hello kontraktet och hello typ av protokoll som används toocommunicate med hello vidarebefordrande tjänsten. hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren. En mer allmän diskussion om hur tooimplement en tjänst minimera finns [implementera tjänstekontrakt](https://msdn.microsoft.com/library/ms733764.aspx) i hello WCF-dokumentationen.

1. Skapa en ny klass med namnet `EchoService` direkt efter hello definition av hello `IEchoContract` gränssnitt. Hej `EchoService` klassen implementerar hello `IEchoContract` gränssnitt.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Liknande tooother gränssnittsimplementationer du kan implementera hello definition i en annan fil. Men i den här självstudien hello implementeringen i samma fil som gränssnittsdefinitionen hello och hello hello `Main` metod.
2. Tillämpa hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attributet toohello `IEchoContract` gränssnitt. hello attributet anger hello tjänstens namn och namnområde. När du har gjort det, hello `EchoService` klass visas på följande sätt:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. Implementera hello `Echo` metod som definieras i hello `IEchoContract` gränssnittet i hello `EchoService` klass.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Klicka på **skapa**, klicka på **skapa lösning** tooconfirm hello korrektheten i ditt arbete.

### <a name="define-hello-configuration-for-hello-service-host"></a>Definiera hello konfiguration för hello tjänstvärden

1. hello-konfigurationsfilen är mycket lik tooa WCF-konfigurationsfil. Den innehåller hello namnet på tjänsten, slutpunkten (det vill säga hello plats som Azure Relay visar för klienter och värdar toocommunicate med varandra) och hello bindning (hello typ av protokoll som används toocommunicate). hello största skillnaden är att den här konfigurerade tjänstslutpunkten refererar tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) bindning som inte är en del av hello .NET Framework. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) är en av hello bindningar som definierats av hello-tjänsten.
2. I **Solution Explorer**, dubbelklicka på hello App.config-filen tooopen i hello Visual Studio-redigeraren.
3. I hello `<appSettings>` element, Ersätt hello-platshållare med hello namnet på namnområdet för tjänsten och hello SAS-nyckel som du kopierade tidigare.
4. Inom hello `<system.serviceModel>` taggar, lägga till en `<services>` element. Du kan definiera flera relay-program i en enda konfigurationsfil. I den här självstudiekursen definieras dock bara en.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. Inom hello `<services>` element, lägga till en `<service>` toodefine hello elementnamnet hello-tjänsten.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. Inom hello `<service>` element definiera hello platsen för slutpunktskontraktet hello och även hello typen av bindning för hello slutpunkt.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    hello slutpunkten definierar var hello klienten ska söka efter värdprogrammet hello. Senare använder hello kursen det här steget toocreate en URI som visar hello värd via Azure Relay. hello bindningen deklarerar att vi använder TCP som hello protokollet toocommunicate med hello vidarebefordrande tjänsten.
7. Från hello **skapa** -menyn klickar du på **skapa lösning** tooconfirm hello korrektheten i ditt arbete.

### <a name="example"></a>Exempel

hello visar följande kod hello implementering av hello tjänstkontrakt.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

hello visar följande kod hello grundläggande formatet för hello App.config-fil som är associerad med hello tjänstvärden.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a>Värd för och köra en grundläggande web service tooregister med hello vidarebefordrande tjänsten

Det här steget beskriver hur toorun en Azure-relä service.

### <a name="create-hello-relay-credentials"></a>Skapa hello relay autentiseringsuppgifter

1. I `Main()`, skapa två variabler i vilka toostore hello namnområdet och SAS-nyckeln som läses från konsolfönstret hello hello.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    hello SAS-nyckeln kommer att användas senare tooaccess projektet. hello namnområdet skickas som en parameter för`CreateServiceUri` toocreate en URI för tjänsten.
2. Med hjälp av en [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objekt, deklarera att du kommer att använda en SAS-nyckel som autentiseringstyp hello. Lägg till följande kod direkt efter hello lade till i hello sista steget hello.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a>Skapa en basadress för hello-tjänsten

När hello-kod som du lade till i hello sista steget, skapa en `Uri` hello service-instans för hello basadress. Den här URI anger hello Service Bus-schemat, hello namnområde och hello sökväg för hello service-gränssnittet.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

”sb” är en förkortning av hello Service Bus-schemat och anger att vi använder TCP som protokoll hello. Detta har även tidigare angetts i konfigurationsfilen för hello när [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) har angetts som hello bindning.

Den här självstudien hello URI är `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-hello-service-host"></a>Skapa och konfigurera hello tjänstvärden

1. Ange hello anslutningsläget för`AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    hello anslutningsläget beskriver hello protokollet hello tjänsten använder toocommunicate med hello vidarebefordrande tjänsten; antingen HTTP eller TCP. Med hjälp av hello standardinställningen `AutoDetect`, hello om försöker tjänsten tooconnect tooAzure Relay över TCP, om den är tillgänglig och HTTP om TCP inte är tillgänglig. Observera att detta skiljer sig från hello hello-protokolltjänsten anger för klientkommunikation. Det här protokollet bestäms av hello bindningen som används. En tjänst kan till exempel använda hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) bindning som anger att dess slutpunkt kommunicerar med klienterna via HTTP. Att samma tjänst skulle kunna ange **ConnectivityMode.AutoDetect** så att hello service kommunicerar med Azure Relay via TCP.
2. Skapa hello tjänstvärden med hello URI skapade tidigare i det här avsnittet.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    hello tjänstvärden är hello WCF-objekt som instantierar hello-tjänsten. Här kan du skicka den hello typen tjänsten som du vill toocreate (en `EchoService` typ), och även toohello adress som du vill tooexpose hello-tjänsten.
3. Hello över hello Program.cs-filen lägger du till referenser för[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) och [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. Tillbaka i `Main()`, konfigurera hello endpoint tooenable offentlig åtkomst.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Det här steget meddelar hello vidarebefordrande tjänsten att programmet kan hittas offentligt genom att undersöka hello ATOM-flödet för projektet. Om du ställer in **DiscoveryType** för**privata**, en klient fortfarande att kunna tooaccess hello-tjänsten. Hello-tjänsten skulle dock inte visas när den söker hello Relay namnområde. I stället hello klienten måste då tooknow hello slutpunktsökväg i förväg.
5. Tillämpa hello service autentiseringsuppgifter toohello slutpunkter definieras i hello App.config-fil:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Som anges i föregående steg i hello ha du deklarerat flera tjänster och slutpunkter i hello konfigurationsfil. Om du har den här koden skulle passerar hello konfigurationsfil och sökning för varje slutpunkt toowhich ska den tillämpas dina autentiseringsuppgifter. För den här självstudiekursen har konfigurationsfilen hello dock endast en slutpunkt.

### <a name="open-hello-service-host"></a>Öppna hello tjänstvärden

1. Öppna hello-tjänsten.

    ```csharp
    host.Open();
    ```
2. Informera hello användare som hello tjänsten körs och förklara hur tooshut ned hello-tjänsten.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. När du är klar stänger du hello tjänstvärden.

    ```csharp
    host.Close();
    ```
4. Tryck på **Ctrl + Skift + B** toobuild hello projektet.

### <a name="example"></a>Exempel

Koden för slutförda tjänsten ska visas på följande sätt. hello kod innehåller hello tjänstekontraktet och implementeringen från föregående steg i självstudiekursen hello och värdar hello tjänst i ett konsolprogram.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create hello credentials object for hello endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create hello service URI based on hello service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create hello service host reading hello configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create hello ServiceRegistrySettings behavior for hello endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add hello Relay credentials tooall endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open hello service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            // Close hello service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-hello-service-contract"></a>Skapa en WCF-klient för tjänstekontraktet hello

hello nästa steg är toocreate ett klientprogram och definiera hello-tjänstekontrakt som du kommer att implementera i senare steg. Observera att många av de här stegen liknar hello steg använda toocreate en tjänst: definiera ett kontrakt, redigera en App.config-fil, använder autentiseringsuppgifter tooconnect toohello vidarebefordrande tjänst, och så vidare. hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.

1. Skapa ett nytt projekt i hello aktuella Visual Studio-lösning för hello klient hello följande:

   1. I Solution Explorer i hello samma lösning som innehåller hello-tjänsten och högerklicka på hello aktuella lösningen (inte hello projekt), klickar du på **Lägg till**. Klicka sedan på **Nytt projekt**.
   2. I hello **Lägg till nytt projekt** dialogrutan klickar du på **Visual C#** (om **Visual C#** inte visas, tittar du under **andra språk**), Välj hello **Konsolapp (.NET Framework)** mall, och ger den namnet **EchoClient**.
   3. Klicka på **OK**.
      <br />
2. I Solution Explorer dubbelklickar du på hello Program.cs-filen i hello **EchoClient** projektet tooopen den i Redigeraren för hello, om den inte redan är öppen.
3. Ändra hello namnområdesnamnet från standardnamnet `EchoClient` för`Microsoft.ServiceBus.Samples`.
4. Installera hello [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): i Solution Explorer högerklickar du på hello **EchoClient** projektet och klicka sedan på **hantera NuGet-paket**. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Klicka på **installera**, och Godkänn hello villkor för användning.

    ![][3]
5. Lägg till en `using` -instruktion för hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namnområde i hello Program.cs-filen.

    ```csharp
    using System.ServiceModel;
    ```
6. Lägg till hello definition toohello namnområdet för tjänstekontraktet, som visas i följande exempel hello. Observera att den här definitionen är identiska toohello definitionen i hello **Service** projekt. Du bör lägga till den här koden hello överst i hello `Microsoft.ServiceBus.Samples` namnområde.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. Tryck på **Ctrl + Skift + B** toobuild hello-klienten.

### <a name="example"></a>Exempel

hello följande kod visar hello aktuell status för hello Program.cs-filen i hello **EchoClient** projekt.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-hello-wcf-client"></a>Konfigurera hello WCF-klient

I det här steget skapar du en App.config-fil för ett grundläggande klientprogrammet som ansluter till hello-tjänst som skapades tidigare i den här självstudiekursen. Den här App.config-filen definierar hello kontraktet, bindningen och namnet på hello slutpunkt. hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.

1. I Solution Explorer i hello **EchoClient** projektet genom att dubbelklicka på **App.config** tooopen hello-filen i hello Visual Studio-redigeraren.
2. I hello `<appSettings>` element, Ersätt hello-platshållare med hello namnet på namnområdet för tjänsten och hello SAS-nyckel som du kopierade tidigare.
3. Inom elementet system.serviceModel hello lägger du till en `<client>` element.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    I det här steget anger du att du definierar ett klientprogram i WCF-format.
4. Inom hello `client` element definiera hello namnet, kontraktet och bindningstypen för slutpunkten hello.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Det här steget definierar hello slutpunkt, hello kontrakt som definierats i hello-tjänsten och hello faktum att hello klientprogrammet använder TCP toocommunicate med Azure Relay hello namn. hello namnet på slutpunkten används i hello nästa steg toolink denna slutpunktskonfiguration med URI för hello-tjänsten.
5. Klicka på **filen**, klicka på **spara alla**.

## <a name="example"></a>Exempel

hello visar följande kod hello App.config-fil för hello Echo-klienten.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-hello-wcf-client"></a>Implementera hello WCF-klient
I det här steget kan implementera du ett grundläggande klientprogrammet som ansluter till hello-tjänst som du skapade tidigare i den här självstudiekursen. Liknande toohello service hello utför klienten många av hello samma åtgärder tooaccess Azure Relay:

1. Anger hello anslutningsläget.
2. Skapar hello URI som lokaliserar värdtjänsten hello.
3. Definierar hello säkerhetsreferenser.
4. Gäller hello autentiseringsuppgifter toohello anslutning.
5. Öppnar hello-anslutning.
6. Utför hello programspecifika uppgifterna.
7. Stänger hello anslutningen.

En av hello viktigaste skillnaderna är dock att hello klientprogrammet använder en kanal tooconnect toohello vidarebefordrande tjänst, medan hello tjänsten använder ett anrop för**ServiceHost**. hello-kod som används för dessa uppgifter finns i hello-exemplet som följer hello proceduren.

### <a name="implement-a-client-application"></a>Implementera ett klientprogram
1. Ange hello anslutningsläget för**AutoDetect**. Lägg till följande kod i hello hello `Main()` metod för hello **EchoClient** program.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Definiera variabler toohold hello värden för hello tjänstens namnområde och SAS-nyckeln som läses från hello-konsolen.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Skapa hello URI som definierar hello platsen för hello värden i projektet Relay.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Skapa hello-autentiseringsobjekt för tjänstens namnområde-slutpunkt.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. Skapa hello kanalfabrik som läser in hello konfigurationen som beskrivs i hello App.config-fil.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    En kanalfabriken är ett WCF-objekt som skapar en kanal via vilken hello tjänsten och klienten kan kommunicera.
6. Tillämpa hello autentiseringsuppgifter.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Skapa och öppna hello kanal toohello service.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Skriva hello grundläggande gränssnittet och funktionerna för hello echo.

    ```csharp
    Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Observera att hello koden använder hello instans av hello kanal-objektet som en proxy för hello-tjänsten.
9. Stäng hello kanalen och Stäng hello fabriken.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a>Exempel

Den färdiga koden ska se ut så här visar hur toocreate ett klientprogram, hur toocall hello drift av hello-tjänsten och hur tooclose hello klienten efter hello åtgärden anropa är klar.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-hello-applications"></a>Köra hello program

1. Tryck på **Ctrl + Skift + B** toobuild hello lösning. Detta skapar både hello klientprojektet och hello service-projekt som du skapade i föregående steg i hello.
2. Innan du kör hello-klientprogrammet måste du se till att programmet hello körs. I Solution Explorer i Visual Studio högerklickar du på hello **EchoService** lösning, klicka på **egenskaper**.
3. I egenskapsdialogrutan för hello lösning, klickar du på **Startprojekt**, klicka på hello **flera Startprojekt** knappen. Kontrollera att **EchoService** visas först i hello-listan.
4. Ange hello **åtgärd** för båda hello **EchoService** och **EchoClient** projekt för**starta**.

    ![][5]
5. Klicka på **Projektberoenden**. I hello **projekt** väljer **EchoClient**. I hello **beror på** Kontrollera **EchoService** är markerad.

    ![][6]
6. Klicka på **OK** toodismiss hello **egenskaper** dialogrutan.
7. Tryck på **F5** toorun båda projekten.
8. Båda konsolfönstren öppnas och du uppmanas att ange hello namnområdesnamnet. hello-tjänsten måste köras först, så i hello **EchoService** konsolfönstret, ange hello namnområde och tryck sedan på **RETUR**.
9. Därefter uppmanas du ange din SAS-nyckel. Ange hello SAS-nyckeln och tryck på RETUR.

    Här är exempel på utdata från hello konsolfönstret. Observera att hello värdena som anges här är till exempel endast.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    hello Tjänsteprogrammet skriver toohello fönstret hello adressen som lyssnar, som visas i följande exempel hello.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`
10. I hello **EchoClient** konsolfönstret, ange hello samma information som du angav tidigare för hello-tjänstprogrammet. Följ hello föregående steg tooenter hello samma namnområde för tjänsten och SAS-nyckeln värden för hello-klientprogrammet.
11. När du har angett dessa värden hello klienten öppnar en kanal toohello tjänst och frågar du tooenter lite text i hello följande utmatningsexempel för konsolen.

    `Enter text tooecho (or [Enter] tooexit):`

    Ange vissa text toosend toohello-tjänstprogrammet och tryck på RETUR. Den här texten skickas toohello tjänsten via hello tjänståtgärd Echo och visas i hello tjänstekonsolfönstret hello följande exempel på utdata.

    `Echoing: My sample text`

    hello klientprogrammet får hello returvärdet för hello `Echo` åtgärden som hello ursprungliga texten och skriver ut det tooits konsolfönstret. hello följande är exempel på utdata från hello klientens konsolfönster.

    `Server echoed: My sample text`
12. Du kan fortsätta att skicka textmeddelanden från hello toohello-klienttjänsten i det här sättet. När du är klar, tryck på RETUR i hello klienten och tjänsten konsolen windows tooend båda programmen.

## <a name="next-steps"></a>Nästa steg

Den här självstudiekursen visades hur toobuild ett Azure Relay-klientprogram och tjänsten använder hello vidarebefordrande WCF-funktionerna i Service Bus. För en liknande självstudiekurs som använder [Service Bus-meddelanden](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), se [komma igång med Service Bus-köer](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

toolearn mer om Azure Relay finns hello följande avsnitt.

* [Azure Service Bus-Arkitekturöversikt](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Översikt över Azure Relay](relay-what-is-it.md)
* [Hur toouse hello WCF vidarebefordrande tjänsten med .NET](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
