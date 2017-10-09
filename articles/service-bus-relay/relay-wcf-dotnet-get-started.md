---
title: "aaaGet igång med Azure Relay WCF-reläer i .NET | Microsoft Docs"
description: "Lär dig hur toouse Azure Relay WCF vidarebefordrar tooconnect två program som finns på olika platser."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a652617fc2e9b7c8d62d39fa914f77df6e3a1771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a>Hur toouse Azure Relay WCF vidarebefordrar med .NET
Den här artikeln beskriver hur toouse hello Azure vidarebefordrande tjänsten. hello exemplen är skrivna i C# och använder API för hello Windows Communication Foundation (WCF) med tillägg som finns i hello Service Bus-sammansättningen. Mer information om Azure relay finns hello [översikt över Azure Relay](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>Vad är WCF Relay?

hello Azure [ *vidarebefordrande WCF* ](relay-what-is-it.md) kan du toobuild hybridprogram som körs i ett Azure-datacenter och din egen lokala företagsmiljö. hello vidarebefordrande tjänsten förenklar detta genom att du toosecurely exponera tjänster för Windows Communication Foundation (WCF) som finns i ett företagsnätverk som kopplar nätverk toohello offentliga moln, utan med tooopen en brandväggsanslutning eller kräva störande ändringar tooa företagets nätverksinfrastruktur.

![WCF Relay-begrepp](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Azure Relay gör toohost WCF-tjänster i din befintliga företagsmiljö. Du kan sedan delegera lyssningen för inkommande sessioner och förfrågningar toothese WCF services toohello relay-tjänsten körs i Azure. Detta gör du tooexpose dessa tjänster tooapplication kod som körs i Azure, eller toomobile arbetare eller miljöer för extranätpartner. Relay gör toosecurely styra vem som kan komma åt dessa tjänster på en detaljerad nivå. Det ger ett kraftfullt och säkert sätt tooexpose programfunktioner och data från dina befintliga företagslösningar och dra nytta av den från hello molnet.

Den här artikeln beskrivs hur toouse Azure Relay toocreate WCF-webbtjänst exponeras med hjälp av en TCP-kanalbindning och som implementerar en säker konversation mellan två parter.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a>Hämta hello Service Bus-NuGet-paket
Hej [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) är hello enklaste sättet tooget hello Service Bus-API och tooconfigure ditt program med alla hello Service Bus-beroenden. tooinstall hello NuGet-paketet i ditt projekt hello följande:

1. Högerklicka på **Referenser** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.
2. Sök efter ”Service Bus” och välj hello **Microsoft Azure Service Bus** objekt. Klicka på **installera** toocomplete hello installationen och sedan stänga hello följande dialogruta:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Exponera och använda en SOAP-webbtjänst med TCP
tooexpose en befintlig WCF SOAP-webbtjänst för extern användning, måste du göra ändringar toohello tjänstbindningar och adresser. Du kan behöva ändringar tooyour konfigurationsfilen eller kan det krävas kodändringar, beroende på hur du har skapat och konfigurerat dina WCF-tjänster. Observera att WCF gör du toohave flera nätverksslutpunkter över Hej samma tjänst, så du kan behålla hello befintliga interna slutpunkter när du lägger till relay slutpunkter för extern åtkomst på hello samma tid.

I den här uppgiften att skapa en enkel WCF-tjänst och Lägg till en relay-lyssnaren tooit. Den här övningen förutsätter bekant med Visual Studio och därför igenom inte alla hello information om hur du skapar ett projekt. Övningen fokuserar istället på hello-koden.

Slutför hello följa proceduren tooset konfigurera miljön innan du påbörjar de här stegen:

1. Skapa ett konsolprogram som innehåller två projekt, ”klient” och ”tjänst” i hello lösning i Visual Studio.
2. Lägg till hello Service Bus-NuGet-paketet tooboth projekt. Det här paketet lägger till alla hello nödvändiga sammansättningen referenser tooyour projekt.

### <a name="how-toocreate-hello-service"></a>Hur toocreate hello service
Först skapar du själva hello-tjänsten. Alla WCF-tjänster består av minst tre separata delar:

* Definition av ett kontrakt som beskriver vilka meddelanden som utbyts och vilka åtgärder som toobe anropas.
* Implementering av kontraktet.
* Värden som är värd hello WCF-tjänst och visar flera slutpunkter.

hello kodexempel i det här avsnittet adressen var och en av dessa komponenter.

hello kontraktet definierar en enda åtgärd `AddNumbers`, som adderar två tal och returnerar hello resultat. Hej `IProblemSolverChannel` gränssnittet aktiverar hello klienten toomore enkelt hantera hello proxylivslängden. Att skapa ett sådant gränssnitt anses vara bästa praxis. Det är en bra idé tooput det här kontraktet definition i en separat fil så att du kan referera till filen från både ”klient” och ”tjänst” projekt, men du kan också kopiera hello koden till båda projekten.

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Med hello kontrakt på plats är hello-implementeringen:

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Konfigurera en tjänstevärd programmässigt
Med hello kontraktet och implementering på plats kan värd du nu hello-tjänsten. Värdhanteringen utförs i en [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objekt, som tar hand om hanteringen av hello-tjänsten och värdar hello slutpunkter som lyssnar efter meddelanden. hello konfigurerar följande kod hello-tjänsten med både en vanlig, lokal slutpunkt och en relay endpoint tooillustrate hello utseende, sida vid sida av interna och externa slutpunkter. Ersätt hello sträng *namnområde* med ditt namnområdesnamn och *yourKey* med hello SAS-nyckel som du fick i hello förra steget av installationen.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();

sh.Close();
```

I exemplet hello skapar du två slutpunkter som finns på hello samma kontrakt implementering. En lokal och en projiceras via Azure Relay. hello viktigaste skillnaderna mellan dem är bindningarna hello: [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) för hello lokala punkten och [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) för hello relay slutpunkt och hello adresser. hello lokala slutpunkten har en lokal nätverksadress med en separat port. hello relay slutpunkten har en slutpunktsadress som består av hello sträng `sb`, ditt namnområdesnamn och hello sökvägen ”solver”. Detta resulterar i hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifierar hello tjänsteslutpunkten som en Service Bus (relay) TCP-slutpunkt med ett fullständigt kvalificerat externa DNS-namn. Om du placerar hello koden som ska ersätta platshållarna hello i hello `Main` funktion av hello **Service** program du har en fungerande tjänst. Om du vill att din tjänst toolisten enbart på hello relay bort hello lokala slutpunktsdeklarationen.

### <a name="configure-a-service-host-in-hello-appconfig-file"></a>Konfigurera en tjänstevärd i hello App.config-fil
Du kan också konfigurera hello värden med hjälp av hello App.config-fil. hello koden för tjänstevärden i det här fallet visas i hello nästa exempel.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

hello slutpunktsdefinitionerna flyttas till hello App.config-fil. hello NuGet-paketet har redan lagt till en uppsättning definitioner toohello App.config-filen som är hello krävs configuration tillägg för Azure Relay. Hej följande exempel, som är hello exakt motsvarighet till föregående kod hello bör visas direkt under hello **system.serviceModel** element. I det här kodexemplet förutsätter vi att namnområdet för ditt C#-projekt har namnet **Tjänst**.
Ersätt hello platshållarna med namnet för relay-namnområdet och SAS-nyckel.

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

När du har gjort ändringarna hello-tjänsten startar som förut men med två live-slutpunkter: en lokal och en som lyssnar i hello molnet.

### <a name="create-hello-client"></a>Skapa hello-klient
#### <a name="configure-a-client-programmatically"></a>Konfigurera en klient programmässigt
tooconsume hello tjänsten som du kan skapa en WCF-klient som använder en [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objekt. Service Bus använder en tokenbaserad säkerhetsmodell som implementeras med hjälp av SAS. Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer. hello följande exempel används hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoden toohandle hello förvärv av hello lämplig SAS-token. är de som erhålls från hello portal som beskrivs i föregående avsnitt i hello hello namn och nyckel.

Första, referens eller kopiering hello `IProblemSolver` minimera koden från hello-tjänsten till ditt klientprojekt.

Ersätt sedan hello koden i hello `Main` metod för hello-klienten, Ersätt återigen platshållartexten hello med relay namnområdet och SAS-nyckel.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Du kan nu skapa hello klienten och hello-tjänsten och köra dem (kör hello tjänsten först), och hello klienten anropar hello-tjänsten och skriver ut **9**. Du kan köra hello klienten och servern på olika datorer, även över nätverk och hello kommunikation fortfarande fungerar. hello klientkoden kan också köra hello molnet eller lokalt.

#### <a name="configure-a-client-in-hello-appconfig-file"></a>Konfigurera en klient i hello App.config-fil
hello följande kod visar hur tooconfigure hello klienten med hjälp av hello App.config-fil.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

hello slutpunktsdefinitionerna flyttas till hello App.config-fil. hello följande exempel, som är hello samma som hello koden som listades tidigare, bör visas direkt under hello `<system.serviceModel>` element. Här, som tidigare måste du ersätta hello platshållarna med relay namnområdet och SAS-nyckel.

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i Azure Relay följa dessa länkar toolearn mer.

* [Vad är Azure Relay?](relay-what-is-it.md)
* [Azure Service Bus-Arkitekturöversikt](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* Hämta Service Bus-exempel från [Azure-exempel] [ Azure samples] eller se hello [översikt över Service Bus-exempel][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
