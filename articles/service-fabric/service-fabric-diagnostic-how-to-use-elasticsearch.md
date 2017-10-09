---
title: "aaaUsing Elasticsearch som trace för Service Fabric programarkivet | Microsoft Docs"
description: "Beskriver hur Service Fabric-program kan använda Elasticsearch och Kibana toostore, index och Sök igenom programspårningar (loggarna)"
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
ms.openlocfilehash: b5977c54e69319e3caa376e44a02f971b66a3254
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Använd Elasticsearch som en spårning i Service Fabric-programmet lagrar
## <a name="introduction"></a>Introduktion
Den här artikeln beskriver hur [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) program kan använda **Elasticsearch** och **Kibana** för programmet trace lagring, indexering och sökning. [Elasticsearch](https://www.elastic.co/guide/index.html) är en öppen källkod, distribuerade och skalbar realtid Sök och analytics-motor som lämpar sig väl för den här uppgiften. Den kan installeras på Windows- och Linux virtuella datorer som körs i Microsoft Azure. Elasticsearch effektivt kan bearbeta *strukturerade* spår som genereras med hjälp av teknik som **ETW Event Tracing for Windows ()**.

ETW används av Service Fabric runtime toosource diagnostisk information (spår). Det är hello rekommenderad metod för Service Fabric program toosource sina diagnostisk information för. Med hjälp av hello som gör att samma mekanism för korrelation mellan angivna runtime och tillämpningsprogrammet spår och gör felsökningen lättare. Mallar för Service Fabric-projekt i Visual Studio omfattar en loggning API: T (baserat på hello .NET **EventSource** klassen) som skickar ETW-spårning som standard. En allmän översikt över Service Fabric application spårning med ETW, se [övervakning och diagnos av tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

För hello spårningar tooshow in i Elasticsearch behöver de toobe hämtats vid hello Service Fabric-klusternoder i realtid (medan programmet hello körs) och skickas tooan Elasticsearch slutpunkt. Det finns två huvudsakliga alternativ för spårning fånga in:

* **Processen spårning fånga in**  
  hello program eller mer exakt tjänstprocessen ansvarar för att skicka ut hello diagnostikdata toohello trace store (Elasticsearch).
* **Out-of-process spårning fånga in**  
  En separat agent fånga in spårningar från hello tjänstprocessen eller processer och skickar dem toohello trace store.

Nedan, beskriver vi hur tooset in Elasticsearch på Azure, diskutera hello-tekniker och nackdelar för både avbilda alternativ och förklara hur tooconfigure ett Service Fabric-tjänsten toosend data tooElasticsearch.

## <a name="set-up-elasticsearch-on-azure"></a>Ställ in Elasticsearch på Azure
hello enklaste sättet tooset hello Elasticsearch tjänsten i Azure är via [ **Azure Resource Manager-mallar**](../azure-resource-manager/resource-group-overview.md). En omfattande [Quickstart Azure Resource Manager-mall för Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) är tillgänglig från lagringsplatsen för Azure Quickstart-mallar. Den här mallen använder separata storage-konton för skalenheter (grupper av noder). Det kan också etablera separata klient och server-noder med olika konfigurationer och olika mängder datadiskar som är anslutna.

Här kan vi använder en annan mall, kallas **ES MultiNode** från hello [Azure diagnostikverktyg](https://github.com/Azure/azure-diagnostics-tools). Den här mallen är enklare toouse och skapar ett Elasticsearch kluster som skyddas av grundläggande HTTP-autentisering. Innan du fortsätter måste du hämta hello databasen från GitHub tooyour dator (genom att antingen kloning hello databasen eller hämtar en zip-fil). hello ES MultiNode mall finns i hello mappen med hello samma namn.

### <a name="prepare-a-machine-toorun-elasticsearch-installation-scripts"></a>Förbered en dator toorun Elasticsearch installationsskript
hello enklaste sättet toouse hello ES MultiNode mallen är via ett tillhandahållna Azure PowerShell-skript som heter `CreateElasticSearchCluster`. toouse det här skriptet kan du behöver tooinstall PowerShell-moduler och ett verktyg som kallas **openssl**. hello senare krävs för att skapa en SSH-nyckel som kan använda tooadminister Elasticsearch klustret via fjärranslutning.

`CreateElasticSearchCluster`skript är utformad för enkel användning med hello ES MultiNode mall från en Windows-dator. Det är möjligt toouse hello mall på en icke-Windows-dator, men det scenariot ligger utanför hello i den här artikeln.

1. Om du inte har installerat dem redan [ **Azure PowerShell-moduler**](http://aka.ms/webpi-azps). När du uppmanas, klickar du på **kör**, sedan **installera**. Azure PowerShell 1.3 eller senare krävs.
2. Hej **openssl** tool ingår i hello fördelning av [ **Git för Windows**](http://www.git-scm.com/downloads). Om du redan inte har gjort det, installera [Git för Windows](http://www.git-scm.com/downloads) nu. (hello standard installationsalternativ är OK.)
3. Förutsatt att Git har installerats men ingår inte i systemsökvägen hello, öppna ett Microsoft Azure PowerShell-fönster och kör följande kommandon hello:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Ersätt hello `<Git installation folder>` med hello Git plats på din dator; hello standardvärdet är **”C:\Program Files\Git”**. Observera hello semikolon hello början av hello första sökväg.
4. Kontrollera att du är inloggad på tooAzure (via [ `Add-AzureRmAccount` ](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) och att du har valt hello-prenumeration som ska användas toocreate elastisk Sök klustret. Du kan kontrollera att korrekt prenumeration har valts med hjälp av `Get-AzureRmContext` och `Get-AzureRmSubscription` cmdlets.
5. Om du inte redan gjort det, kan du ändra hello aktuella toohello ES MultiNode katalogmapp.

### <a name="run-hello-createelasticsearchcluster-script"></a>Kör hello CreateElasticSearchCluster skript
Innan du kör skriptet hello öppna hello `azuredeploy-parameters.json` filen och verifiera eller ange värden för hello Skriptparametrar. hello följande parametrar finns:

| Parameternamn | Beskrivning |
| --- | --- |
| dnsNameForLoadBalancerIP |hello namn som används toocreate hello synligt offentligt DNS-namn för hello elastisk Sök-kluster (genom att lägga till hello Azure-region toohello tillhandahålls domännamn). Om det här parametervärdet är ”myBigCluster” och hello valt Azure-regionen är västra USA är hello resulterande DNS-namn för hello klustret myBigCluster.westus.cloudapp.azure.com. <br /><br />Det här namnet fungerar också som ett namn för skogsrotsdomänen för många artefakter som är associerade med hello elastisk Sök klustret, till exempel data nodnamn. |
| adminUsername |hello namnet på hello administratörskonto för att hantera hello elastisk Sök kluster (motsvarande SSH-nycklar genereras automatiskt). |
| dataNodeCount |hello antalet noder i klustret för hello elastisk sökning. hello nuvarande version av hello skript skiljer inte mellan data och fråga noder. alla noder spela upp båda rollerna. Standardvärden too3 noder. |
| dataDiskSize |hello storleken på diskar (i GB) som har allokerats för varje datanod. Varje nod som tar emot 4 datadiskar uteslutande dedikerade tooElastic Search-tjänsten. |
| Region |hello namnet på Azure-region där hello elastisk Sök klustret ska finnas. |
| esUserName |hello konfigurerat användarnamnet för hello-användare som är toohave åtkomst tooES kluster (ämne tooHTTP grundläggande autentisering). hello lösenord ingår inte i parameterfilen och måste anges när `CreateElasticSearchCluster` anropas skriptet. |
| vmSizeDataNodes |hello Azure virtuell datorstorlek för elastiska Sök klusternoder. Standardvärden tooStandard_D2. |

Nu är du redo toorun hello skript. Utfärda hello följande kommando:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

där 

| Parameternamn för skript | Beskrivning |
| --- | --- |
| `<es-group-name>` |hello namnet på hello Azure-resursgrupp som innehåller alla elastisk Sök klusterresurser. |
| `<azure-region>` |hello namnet på hello Azure-region där hello elastisk Sök klustret ska skapas. |
| `<es-password>` |hello lösenordet för hello elastisk Sök användare. |

> [!NOTE]
> Om du får en NullReferenceException från hello Test-AzureResourceGroup cmdlet du har glömt toolog på tooAzure (`Add-AzureRmAccount`).
> 
> 

Om du får ett felmeddelande från hello-skript och du bestämma att hello felet orsakades av ett fel mall parametervärde, korrigera hello parameterfilen och kör hello skriptet igen med ett annat resursnamn för gruppen. Du kan återanvända hello samma resursgruppens namn och har hello skript Rensa hello gamla genom att lägga till hello `-RemoveExistingResourceGroup` parametern toohello skript anrop.

### <a name="result-of-running-hello-createelasticsearchcluster-script"></a>Resultatet för hello CreateElasticSearchCluster skript som körs
När du har kört hello `CreateElasticSearchCluster` skript hello följande huvudsakliga artefakter kommer att skapas. I det här exemplet förutsätter vi att du har använt ”myBigCluster” som hello värde för hello `dnsNameForLoadBalancerIP` parametern och hello regionen där du skapade hello är västra USA.

| Artefakt | Namn, plats och kommentarer |
| --- | --- |
| SSH-nyckeln för fjärradministration |myBigCluster.key fil (i hello från vilka hello CreateElasticSearchCluster kördes). <br /><br />Nyckeln kan vara används tooconnect toohello Administratörsnod och (via hello Administratörsnod) toodata noder i klustret hello. |
| Admin-nod |myBigCluster admin.westus.cloudapp.azure.com <br /><br />En särskild virtuell dator för Elasticsearch klustret fjärradministration--hello endast en som gör att externa SSH-anslutningar. Den körs på hello har samma virtuella nätverk som alla klusternoder för hello Elasticsearch, men det inte att köra några Elasticsearch-tjänster. |
| Datanoder |myBigCluster1... myBigCluster*N* <br /><br />Datanoder som kör Elasticsearch och Kibana tjänster. Du kan ansluta via SSH tooeach noden, men endast via hello admin-nod. |
| Elasticsearch kluster |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />hello primär slutpunkt för hello Elasticsearch kluster (Obs hello /es suffix). Den är skyddad av grundläggande HTTP-autentisering (hello autentiseringsuppgifter var hello angivna esUserName/esPassword parametrar för hello ES MultiNode mall). hello-klustret har också hello head plugin-program installerat (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) för grundläggande klusteradministration. |
| Kibana service |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Hej Kibana tjänst har ställts in tooshow data från hello skapade Elasticsearch. Den är skyddad av hello samma autentiseringsuppgifter som hello kluster sig själv. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>I processen jämfört med out-of-process spårning fånga in
I hello introduktion nämndes två huvudsakliga sätt för att samla in diagnostikdata: i processen och out-of-process. Varje har styrkor och svagheter.

Fördelarna med hello **pågående spårning fånga** omfattar:

1. *Enkel konfiguration och distribution*
   
   * hello konfigurationen av diagnostikdata samlingen har bara en del av hello tillämpningsprogrammets konfiguration. Det är enkelt tooalways behålla det ”synkroniserad” med hello resten av programmet hello.
   * Programspecifika eller per service configuration kan enkelt uppnås.
   * Out-of-process spårning fånga kräver vanligtvis en separat distributionen och konfigurationen av hello diagnostikagenten som är en extra administrativa uppgifter och en tänkbar orsak till fel. hello viss agent teknologin tillåter ofta endast en instans av hello agent per virtuell dator (nod). Det innebär att konfigurationen för hello samling hello diagnostikkonfiguration delas med alla program och tjänster som körs på noden.
2. *Flexibilitet*
   
   * hello-program kan skicka hello data oavsett var den måste toogo, så länge det finns ett klientbiblioteket som stöder hello riktade Datalagringssystemet. Nya sänkor kan läggas till på önskat sätt.
   * Komplexa capture, filtrering och Datasammanställning regler kan implementeras.
   * En out-of-process spårning fånga in begränsas ofta av hello data sänkor som hello har stöd för agenten. Vissa agenter är extensible.
3. *Åtkomst toointernal programdata och kontext*
   
   * hello diagnostiska undersystemet körs i processen för hello-program/tjänst kan enkelt utöka hello spårningar med relevant information.
   * Hello out-of-process inriktning måste hello data skickas tooan agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows. Den här mekanismen kan införa ytterligare begränsningar.

Fördelarna med hello **out-of-process spårning fånga** omfattar:

1. *Hej möjlighet toomonitor hello program och samla in Dumpar krascher*
   
   * I processen spårning fånga kanske misslyckas om programmet hello misslyckas toostart eller kraschar. En oberoende agent har mycket större möjlighet att samla in viktig information för felsökning.<br /><br />
2. *Förfall, stabilitet och beprövade prestanda*
   
   * En agent som utvecklats av en plattform leverantör (till exempel en agent för Microsoft Azure-diagnostik) har ämne toorigorous testning och slaget härdning.
   * Med pågående spårning fånga, iakttas försiktighet tooensure att hello aktiviteten för att skicka diagnostikdata från en process för programmet inte påverka hello programmets huvudsakliga aktiviteter eller införa tidsinställning eller prestanda. En separat körs agent är mindre risk toothese problem och är särskilt utformad toolimit dess påverkan på hello system.

Det är möjligt toocombine och utnyttja båda metoderna. Faktiskt kanske hello bästa lösningen för många program.

Här kan vi använda hello **Microsoft.Diagnostic.Listeners biblioteket** och hello pågående spårning fånga toosend data från ett Service Fabric application tooan Elasticsearch kluster.

## <a name="use-hello-listeners-library-toosend-diagnostic-data-tooelasticsearch"></a>Använd hello lyssnare biblioteket toosend diagnostikdata tooElasticsearch
hello Microsoft.Diagnostic.Listeners biblioteket är en del av PartyCluster exempelprogrammet Service Fabric. toouse den:

1. Hämta [hello PartyCluster exempel](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) från GitHub.
2. Kopiera hello Microsoft.Diagnostics.Listeners och Microsoft.Diagnostics.Listeners.Fabric projekt (hela mappar) från hello PartyCluster directory toohello lösningsmapp för hello-program som ska toosend hello data tooElasticsearch .
3. Öppna hello mål lösningen, högerklickar du på hello lösning nod hello Solution Explorer och välj **lägga till befintliga projekt**. Lägg till hello Microsoft.Diagnostics.Listeners projekt toohello lösning. Upprepa hello samma för hello Microsoft.Diagnostics.Listeners.Fabric projekt.
4. Lägg till en projektreferens från tjänsten projektet toohello två tillagda projekt. (Varje tjänst som ska toosend data tooElasticsearch ska referera till Microsoft.Diagnostics.EventListeners och Microsoft.Diagnostics.EventListeners.Fabric).
   
    ![Projektet refererar till tooMicrosoft.Diagnostics.EventListeners och Microsoft.Diagnostics.EventListeners.Fabric bibliotek][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Versionen av Service Fabric allmän tillgänglighet och Microsoft.Diagnostics.Tracing Nuget-paketet
Program som skapats med versionen av Service Fabric allmän tillgänglighet (2.0.135, publicerat den 31 mars 2016) mål **.NET Framework 4.5.2**. Den här versionen är hello högsta version av hello .NET Framework som stöds av Azure när hello hello GA-versionen. Tyvärr kan saknar den här versionen av hello framework vissa EventListener APIs som hello Microsoft.Diagnostics.Listeners bibliotek måste. Eftersom EventSource (hello komponent som ligger hello grund för loggning av API: er i Fabric-program) och EventListener nära tillsammans, måste varje projekt som använder hello Microsoft.Diagnostics.Listeners bibliotek använda en alternativ implementering av EventSource. Den här implementeringen tillhandahålls av hello **Microsoft.Diagnostics.Tracing Nuget-paketet**, skapade av Microsoft. hello-paketet är fullt bakåtkompatibel med EventSource som ingår i hello framework, så inga kodändringar ska vara nödvändiga än refererade namnområde ändringar.

toostart med hello Microsoft.Diagnostics.Tracing implementering av hello EventSource klass, följer du dessa steg för varje service-projekt som behöver toosend data tooElasticsearch:

1. Högerklicka på hello service-projekt och välj **hantera Nuget-paket**.
2. Växla toohello nuget.org paketkällan (om den inte redan är markerad) och Sök efter ”**Microsoft.Diagnostics.Tracing**”.
3. Installera hello `Microsoft.Diagnostics.Tracing.EventSource` paketet (och dess beroenden).
4. Öppna hello **ServiceEventSource.cs** eller **ActorEventSource.cs** filen i projektet service och ersätter hello `using System.Diagnostics.Tracing` direktivet ovanpå hello-fil med hello `using Microsoft.Diagnostics.Tracing` direktiv.

Dessa steg behövs inte när hello **.NET Framework 4.6** stöds av Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch lyssnare instansiering och konfiguration
hello sista steget för att skicka diagnostikdata tooElasticsearch är toocreate en instans av `ElasticSearchListener` och konfigurera den med Elasticsearch anslutningsdata. hello lyssnare samlar automatiskt in alla händelser som samlats in via EventSource klasser som definieras i hello service-projekt. Den måste toobe alive under hello livstid hello service så hello bästa placera toocreate i hello service initieringskod. Här är hur hello initieringskoden för den tillståndslösa tjänsten kan du granska hello nödvändiga ändringar (tillägg pekas i kommentarer som börjar med `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add hello following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is hello entry point of hello service host process.
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

                // hello ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name tooa .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of hello class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that hello ElasticSearchListner instance is not garbage-collected prematurely
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

Elasticsearch anslutningsdata ska placeras i ett separat avsnitt i konfigurationsfilen för hello-tjänsten (**PackageRoot\Config\Settings.xml**). hello namnet hello avsnitt måste överensstämma med toohello-värdet som skickas toohello `FabricConfigurationProvider` konstruktor, till exempel:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Hej värdena för `serviceUri`, `userName` och `password` parametrar motsvaras toohello Elasticsearch klusteradress slutpunkt, Elasticsearch användarnamn och lösenord. `indexNamePrefix`hello-prefix för Elasticsearch index. Hej Microsoft.Diagnostics.Listeners biblioteket skapar ett nytt index för sina data varje dag.

### <a name="verification"></a>Verifieringen
Klart! Nu när hello-tjänsten körs startar skicka spårningar toohello Elasticsearch tjänst som anges i hello konfiguration. Du kan kontrollera detta genom att öppna hello Kibana UI som är associerade med hello Elasticsearch målinstansen. I vårt exempel är adressen hello http://myBigCluster.westus.cloudapp.azure.com/. Kontrollera att index med hello namnprefix valt för hello `ElasticSearchListener` instans faktiskt har skapats och fylls med data.

![Kibana visar PartyCluster programhändelser][2]

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
