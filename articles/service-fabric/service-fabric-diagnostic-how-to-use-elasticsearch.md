---
title: "Med Elasticsearch som trace för Service Fabric programarkivet | Microsoft Docs"
description: "Beskriver hur Service Fabric-program kan använda Elasticsearch och Kibana att lagra indexet och söka igenom programspårningar (loggarna)"
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: adegeo
editor: 
ms.assetid: e59b0c39-e468-4d9e-b453-d5f2a8ad20d8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: karolz@microsoft.com
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: 2d2ceceea131b41ad1a1735aaa2a859d035ab098
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Använd Elasticsearch som en spårning i Service Fabric-programmet lagrar
## <a name="introduction"></a>Introduktion
Den här artikeln beskriver hur [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) program kan använda **Elasticsearch** och **Kibana** för programmet trace lagring, indexering och sökning. [Elasticsearch](https://www.elastic.co/guide/index.html) är en öppen källkod, distribuerade och skalbar realtid Sök och analytics-motor som lämpar sig väl för den här uppgiften. Den kan installeras på Windows- och Linux virtuella datorer som körs i Microsoft Azure. Elasticsearch effektivt kan bearbeta *strukturerade* spår som genereras med hjälp av teknik som **ETW Event Tracing for Windows ()**.

ETW används av Service Fabric runtime till källan diagnostisk information (spår). Det är den rekommenderade metoden för Service Fabric-program som källa för sina diagnostisk information. Använder samma metod kan korrelation mellan angivna runtime och tillämpningsprogrammet spår och gör felsökningen lättare. Mallar för Service Fabric-projekt i Visual Studio omfattar en loggning API: T (baserad på .NET **EventSource** klassen) som skickar ETW-spårning som standard. En allmän översikt över Service Fabric application spårning med ETW, se [övervakning och diagnos av tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

För spårning visas i Elasticsearch, behöver de hämtats vid Service Fabric klusternoderna i realtid (medan programmet körs) och skickas till en Elasticsearch slutpunkt. Det finns två huvudsakliga alternativ för spårning fånga in:

* **Processen spårning fånga in**  
  Programmet eller mer exakt tjänstprocessen ansvarar för att skicka ut diagnostikdata till arkivet trace (Elasticsearch).
* **Out-of-process spårning fånga in**  
  En separat agent fånga in spårningar från tjänstprocessen eller processer och skickar dem till arkivet spårningen.

Nedan, vi beskriver hur du ställer in Elasticsearch på Azure, diskutera tekniker och nackdelar för både avbilda alternativ och förklarar hur du konfigurerar ett Service Fabric-tjänsten för att skicka data till Elasticsearch.

## <a name="set-up-elasticsearch-on-azure"></a>Ställ in Elasticsearch på Azure
Det enklaste sättet att konfigurera tjänsten Elasticsearch på Azure är via [ **Azure Resource Manager-mallar**](../azure-resource-manager/resource-group-overview.md). En omfattande [Quickstart Azure Resource Manager-mall för Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) är tillgänglig från lagringsplatsen för Azure Quickstart-mallar. Den här mallen använder separata storage-konton för skalenheter (grupper av noder). Det kan också etablera separata klient och server-noder med olika konfigurationer och olika mängder datadiskar som är anslutna.

Här kan vi använder en annan mall, kallas **ES MultiNode** från den [Azure diagnostikverktyg](https://github.com/Azure/azure-diagnostics-tools). Den här mallen är enklare att använda och skapar ett Elasticsearch kluster som skyddas av grundläggande HTTP-autentisering. Innan du fortsätter, ska du hämta databasen från GitHub till din dator (genom att klona databasen eller hämtar en zip-fil). ES MultiNode mall finns i mappen med samma namn.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Förbered en dator som ska köra Elasticsearch installationsskript
Det enklaste sättet att använda mallen ES MultiNode är via ett tillhandahållna Azure PowerShell-skript som heter `CreateElasticSearchCluster`. Om du vill använda det här skriptet, måste du installera PowerShell-moduler och ett verktyg som kallas **openssl**. Dessa krävs för att skapa en SSH-nyckel som kan användas för att fjärradministrera Elasticsearch klustret.

`CreateElasticSearchCluster`skript är utformad för enkel användning med ES MultiNode mall från en Windows-dator. Det är möjligt att använda mallen på en icke-Windows-dator, men det scenariot är utanför omfattningen för den här artikeln.

1. Om du inte har installerat dem redan [ **Azure PowerShell-moduler**](http://aka.ms/webpi-azps). När du uppmanas, klickar du på **kör**, sedan **installera**. Azure PowerShell 1.3 eller senare krävs.
2. Den **openssl** tool ingår i distributionen av [ **Git för Windows**](http://www.git-scm.com/downloads). Om du redan inte har gjort det, installera [Git för Windows](http://www.git-scm.com/downloads) nu. (Standard installationsalternativ är OK.)
3. Förutsatt att Git har installerats men ingår inte i systemsökvägen, öppna ett Microsoft Azure PowerShell-fönster och kör följande kommandon:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Ersätt den `<Git installation folder>` med Git-plats på din dator; standardvärdet är **”C:\Program Files\Git”**. Observera semikolon i början av den första sökvägen.
4. Se till att du har loggat in till Azure (via [ `Add-AzureRmAccount` ](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) och att du har valt prenumerationen som ska användas för att skapa elastiska Sök-kluster. Du kan kontrollera att korrekt prenumeration har valts med hjälp av `Get-AzureRmContext` och `Get-AzureRmSubscription` cmdlets.
5. Om du inte redan gjort det, kan du ändra den aktuella katalogen till mappen ES MultiNode.

### <a name="run-the-createelasticsearchcluster-script"></a>Kör skriptet CreateElasticSearchCluster
Innan du kör skriptet, öppna den `azuredeploy-parameters.json` filen och verifiera eller ange värden för skriptparametrarna. Följande parametrar finns:

| Parameternamn | Beskrivning |
| --- | --- |
| dnsNameForLoadBalancerIP |Det namn som används för att skapa synligt offentligt DNS-namn för den elastiska sökningen kluster (genom att lägga till Azure-region domänen till det angivna namnet). Om det här parametervärdet är ”myBigCluster” och den valda Azure-regionen är USA, västra, är det resulterande DNS-namnet för klustret myBigCluster.westus.cloudapp.azure.com. <br /><br />Det här namnet fungerar också som ett namn för skogsrotsdomänen för många artefakter som är associerade med elastisk Sök-klustret, till exempel data nodnamn. |
| adminUsername |Namnet på administratörskontot för att hantera elastisk Sök-kluster (motsvarande SSH-nycklar genereras automatiskt). |
| dataNodeCount |Antalet noder i klustret elastisk sökning. Den aktuella versionen av skriptet skiljer inte mellan data och fråga noder. alla noder spela upp båda rollerna. Standardvärdet är 3 noder. |
| dataDiskSize |Storleken på diskar (i GB) som har allokerats för varje datanod. Varje nod som tar emot 4 datadiskar dedikerad enbart till elastisk Search-tjänsten. |
| Region |Namnet på Azure-region där elastisk Sök klustret ska finnas. |
| esUserName |Användarnamnet på användaren som har konfigurerats för åtkomst till ES-klustret (omfattas grundläggande HTTP-autentisering). Lösenordet ingår inte i parameterfilen och måste anges när `CreateElasticSearchCluster` anropas skriptet. |
| vmSizeDataNodes |Virtuella Azure-datorstorleken för elastiska Sök klusternoder. Standard_D2 standard. |

Du är nu redo att köra skriptet. Kör följande kommando:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

där 

| Parameternamn för skript | Beskrivning |
| --- | --- |
| `<es-group-name>` |Namnet på Azure-resursgrupp som innehåller alla elastisk Sök klusterresurser. |
| `<azure-region>` |Namnet på Azure-regionen där elastisk Sök klustret ska skapas. |
| `<es-password>` |Lösenordet för elastiska Sök användare. |

> [!NOTE]
> Om du får en NullReferenceException från cmdleten Test-AzureResourceGroup du har glömt att logga in på Azure (`Add-AzureRmAccount`).
> 
> 

Om du får ett felmeddelande från att köra skriptet och du bestämma att felet orsakades av ett fel mall parametervärde, åtgärda parameterfilen och köra skriptet igen med ett annat resursnamn för gruppen. Du kan också återanvända samma resursgruppens namn och har skriptet rensa den gamla servern genom att lägga till den `-RemoveExistingResourceGroup` parametern att anropa skript.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Resultatet av CreateElasticSearchCluster-skript
När du har kört den `CreateElasticSearchCluster` skript följande huvudsakliga artefakter kommer att skapas. Det här exemplet förutsätter vi att du har använt ”myBigCluster” som värde för den `dnsNameForLoadBalancerIP` parametern och att den region där du skapade klustret är västra USA.

| Artefakt | Namn, plats och kommentarer |
| --- | --- |
| SSH-nyckeln för fjärradministration |myBigCluster.key filen (i katalogen där CreateElasticSearchCluster kördes). <br /><br />Den här nyckelfilen kan användas för att ansluta till noden admin och (via noden admin) till datanoder i klustret. |
| Admin-nod |myBigCluster admin.westus.cloudapp.azure.com <br /><br />En särskild virtuell dator för Elasticsearch klustret fjärradministration--den enda som gör att externa SSH-anslutningar. Den körs på samma virtuella nätverk som alla klusternoder för Elasticsearch, men inga Elasticsearch tjänster körs inte. |
| Datanoder |myBigCluster1... myBigCluster*N* <br /><br />Datanoder som kör Elasticsearch och Kibana tjänster. Du kan ansluta via SSH till varje nod, men endast via noden Administration. |
| Elasticsearch kluster |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Primär slutpunkt för Elasticsearch klustret (Observera suffixet /es). Den är skyddad av grundläggande HTTP-autentisering (autentiseringsuppgifterna har de angivna parametrarna esUserName/esPassword av mallen ES MultiNode). Klustret har också huvudet plugin-program installerat (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) för grundläggande klusteradministration. |
| Kibana service |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Tjänsten Kibana ställs in för att visa data från skapade Elasticsearch klustret. Den är skyddad av samma autentiseringsuppgifter som själva klustret. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>I processen jämfört med out-of-process spårning fånga in
I inledningen nämndes två huvudsakliga sätt för att samla in diagnostikdata: i processen och out-of-process. Varje har styrkor och svagheter.

Fördelarna med den **pågående spårning fånga** omfattar:

1. *Enkel konfiguration och distribution*
   
   * Konfigurationen av diagnostikdata samlingen har bara en del av programkonfigurationen. Det är lätt att alltid synkronisera den ”” med resten av programmet.
   * Programspecifika eller per service configuration kan enkelt uppnås.
   * Out-of-process spårning fånga kräver vanligtvis en separat distributionen och konfigurationen av diagnostiska agenten som är en extra administrativa uppgifter och en tänkbar orsak till fel. Viss agent-teknik kan ofta endast en instans av agenten per virtuell dator (nod). Det innebär att konfigurationen för insamling av diagnostik-konfiguration delas med alla program och tjänster som körs på noden.
2. *Flexibilitet*
   
   * Programmet kan skicka data oavsett var den ska, så länge det finns ett klientbiblioteket som stöder måldata lagringssystemet. Nya sänkor kan läggas till på önskat sätt.
   * Komplexa capture, filtrering och Datasammanställning regler kan implementeras.
   * En out-of-process spårning fånga in begränsas ofta av data sänkor som har stöd för agenten. Vissa agenter är extensible.
3. *Åtkomst till interna programdata och kontext*
   
   * Undersystemet diagnostik körs i processen för program/tjänst kan enkelt utöka spårningar med relevant information.
   * I out-of-process-metod måste data skickas till en agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows. Den här mekanismen kan införa ytterligare begränsningar.

Fördelarna med den **out-of-process spårning fånga** omfattar:

1. *Möjlighet att övervaka programmet och samla in krascher Dumpar*
   
   * I processen spårning fånga kanske misslyckas om programmet inte startar eller kraschar. En oberoende agent har mycket större möjlighet att samla in viktig information för felsökning.<br /><br />
2. *Förfall, stabilitet och beprövade prestanda*
   
   * En agent som utvecklats av en plattform leverantör (till exempel en agent för Microsoft Azure-diagnostik) har undersökts rigorösa testning och slaget härdning.
   * Med pågående spårning fånga, måste vara försiktig så att aktiviteten för att skicka diagnostikdata från en process för programmet inte påverka programmets huvudsakliga aktiviteter eller införa tidsinställning eller prestanda. En separat körs agent är mindre risk för dessa problem och är specifikt utformade för att begränsa dess påverkan på systemet.

Det är möjligt att kombinera och dra nytta av båda metoderna. Faktiskt kan det vara den bästa lösningen för många program.

Här kan vi använda den **Microsoft.Diagnostic.Listeners biblioteket** och pågående spårningen avbildning för att skicka data från ett Service Fabric-program till ett Elasticsearch-kluster.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Använd lyssnare-biblioteket för att skicka diagnostikdata till Elasticsearch
Microsoft.Diagnostic.Listeners biblioteket är en del av PartyCluster exempelprogrammet Service Fabric. Du använder den:

1. Hämta [PartyCluster exempel](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) från GitHub.
2. Kopiera Microsoft.Diagnostics.Listeners och Microsoft.Diagnostics.Listeners.Fabric projekt (hela mappar) från katalogen PartyCluster exempel till mappen lösning på det program som ska skicka data till Elasticsearch.
3. Öppna mål-lösningen, högerklicka på noden lösningen i Solution Explorer och välj **lägga till befintliga projekt**. Lägger till Microsoft.Diagnostics.Listeners-projekt i lösningen. Upprepa samma för Microsoft.Diagnostics.Listeners.Fabric-projektet.
4. Lägg till en projektreferens från service-projektet till två projekt som lagts till. (Varje tjänst som ska skicka data till Elasticsearch ska referera till Microsoft.Diagnostics.EventListeners och Microsoft.Diagnostics.EventListeners.Fabric).
   
    ![Projektreferenser till Microsoft.Diagnostics.EventListeners och Microsoft.Diagnostics.EventListeners.Fabric bibliotek][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Versionen av Service Fabric allmän tillgänglighet och Microsoft.Diagnostics.Tracing Nuget-paketet
Program som skapats med versionen av Service Fabric allmän tillgänglighet (2.0.135, publicerat den 31 mars 2016) mål **.NET Framework 4.5.2**. Den här versionen är den högsta versionen av .NET Framework som stöds av Azure vid tidpunkten för GA-version. Den här versionen av framework saknar tyvärr vissa EventListener-APIs som behöver Microsoft.Diagnostics.Listeners-biblioteket. Eftersom EventSource (komponent som utgör grunden för loggning av API: er i Fabric-program) och EventListener nära tillsammans, måste varje projekt som använder Microsoft.Diagnostics.Listeners biblioteket använda en alternativ implementering av EventSource. Den här implementeringen tillhandahålls av den **Microsoft.Diagnostics.Tracing Nuget-paketet**, skapade av Microsoft. Paketet är fullt bakåtkompatibel med EventSource som ingår i framework, så inga kodändringar ska vara nödvändiga än refererade namnområde ändringar.

Om du vill börja använda Microsoft.Diagnostics.Tracing implementeringen av klassen EventSource, gör följande för varje service-projekt som ska skicka data till Elasticsearch:

1. Högerklicka på tjänsten projektet och välj **hantera Nuget-paket**.
2. Växla till nuget.org paketkällan (om den inte redan är markerad) och Sök efter ”**Microsoft.Diagnostics.Tracing**”.
3. Installera den `Microsoft.Diagnostics.Tracing.EventSource` paketet (och dess beroenden).
4. Öppna den **ServiceEventSource.cs** eller **ActorEventSource.cs** filen i projektet service och ersätter den `using System.Diagnostics.Tracing` direktivet ovanpå filen med den `using Microsoft.Diagnostics.Tracing` direktiv.

Dessa steg behövs inte när den **.NET Framework 4.6** stöds av Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch lyssnare instansiering och konfiguration
Det sista steget för att skicka diagnostikdata till Elasticsearch är att skapa en instans av `ElasticSearchListener` och konfigurera den med Elasticsearch anslutningsdata. Lyssnaren samlar automatiskt in alla händelser som samlats in via EventSource klasser som definieras i service-projekt. Den måste vara aktiv under livslängden för tjänsten, så den bästa platsen för att skapa den är i kod som initiering. Här är hur initieringskoden för den tillståndslösa tjänsten kan se ut efter ändringar (tillägg pekas i kommentarer som börjar med `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider, new FabricHealthReporter("ElasticSearchEventListener"));
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch anslutningsdata ska placeras i ett separat avsnitt i tjänstekonfigurationsfilen (**PackageRoot\Config\Settings.xml**). Namnet på avsnittet måste motsvara värdet som skickas till den `FabricConfigurationProvider` konstruktor, till exempel:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Värdena för `serviceUri`, `userName` och `password` parametrar motsvarar Elasticsearch klusteradress slutpunkt, Elasticsearch användarnamn och lösenord, respektive. `indexNamePrefix`är prefixet för Elasticsearch index. Microsoft.Diagnostics.Listeners biblioteket skapar ett nytt index för sina data varje dag.

### <a name="verification"></a>Verifieringen
Klart! Nu när tjänsten körs startar skicka spårningar till tjänsten Elasticsearch som angetts i konfigurationen. Du kan kontrollera detta genom att öppna Kibana UI som är associerade med Elasticsearch målinstansen. I vårt exempel är adressen http://myBigCluster.westus.cloudapp.azure.com/. Kontrollera att index med namnprefix valt för den `ElasticSearchListener` instans faktiskt har skapats och fylls med data.

![Kibana visar PartyCluster programhändelser][2]

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
