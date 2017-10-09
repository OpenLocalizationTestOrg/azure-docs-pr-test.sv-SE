---
title: "aaaPartitioning Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur toopartition Service Fabric tillståndskänsliga tjänster. Partitioner möjliggör datalagring på hello lokala datorer så att data och beräkning kan skalas tillsammans."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a>Partitionen Service Fabric reliable services
Den här artikeln innehåller en introduktion toohello grundläggande begrepp för partitionering tillförlitliga Azure Service Fabric-tjänster. hello källkoden som används i hello artikeln finns också på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Partitionering
Partitionering är inte unikt tooService Fabric. Det är faktiskt en core mönster för att skapa skalbara tjänster. I en bredare mening vi tror om partitionering som ett koncept för att dela tillstånd (data) och beräkna i mindre tillgänglig enheter tooimprove skalbarhet och prestanda. Ett välkänt formulär partitionering är [Datapartitionering][wikipartition], även kallad horisontell partitionering.

### <a name="partition-service-fabric-stateless-services"></a>Tillståndslösa tjänster för Service Fabric för partition
Du kan se om en partition är en logisk enhet som innehåller en eller flera instanser av en tjänst för tillståndslösa tjänster. Bild 1 visar tillståndslösa tjänsten med fem instanser distribuerade i ett kluster med en partition.

![Tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Det finns två typer av tillståndslösa tjänstelösningar verkligen. hello först är en en tjänst som kvarstår dess tillstånd externt, till exempel i en Azure SQL database (till exempel en webbplats som lagrar hello sessionsinformation och data). hello är andra endast beräkning-tjänster (till exempel Kalkylatorn eller image thumbnailing) som inte hanterar några beständiga tillstånd.

I antingen fallet partitionering tillståndslösa tjänsten är ett mycket sällsynt scenario--skalbarhet och tillgänglighet uppnås normalt genom att lägga till flera instanser. hello endast-tid som du vill tooconsider flera partitioner för tillståndslösa tjänstinstanser är när du behöver toomeet särskilda routning begäranden.

Exempelvis bör du fall där användare med ID: N i ett visst intervall endast hanteras av en viss tjänst-instans. Ett annat exempel på när du gick partitionera en tillståndslös tjänst är när du har en verkligen partitionerade serverdel (t.ex. ett delat SQL database) och du vill toocontrol vilken tjänstinstans ska skriva toohello databasen Fragmentera-- eller utföra andra förberedelser arbete i hello tillståndslösa tjänsten som kräver hello samma partitionering information som används i hello backend. Dessa typer av scenarier kan också lösas på olika sätt och kräver inte nödvändigtvis service partitionering.

hello resten av den här genomgången fokuserar på tillståndskänsliga tjänster.

### <a name="partition-service-fabric-stateful-services"></a>Tillståndskänsliga tjänster för Service Fabric för partition
Service Fabric gör det enkelt toodevelop skalbara tillståndskänsliga tjänster genom att erbjuda förstklassig sätt toopartition tillstånd (data). Begreppsmässigt, du kan se om en partition av en tillståndskänslig service som en skalningsenhet som är mycket pålitlig via [repliker](service-fabric-availability-services.md) som distribueras och balanserat över hello noder i ett kluster.

Partitionering hello gäller Service Fabric tillståndskänsliga tjänster refererar toohello process för att fastställa att en viss tjänst partition är ansvarig för en del av hello hello tjänstens fullständiga tillstånd. (Som tidigare nämnts, en partition är en uppsättning [repliker](service-fabric-availability-services.md)). En fantastiska med Service Fabric är att den placerar hello partitioner på olika noder. Detta innebär att de toogrow tooa nod resursgräns. När hello data behöver växa, partitioner växer och Service Fabric balanserar partitioner mellan noder. Detta säkerställer att hello fortsatt effektiv användning av maskinvaruresurser.

toogive du exempelvis säger att du börjar med ett kluster med 5 och en tjänst som är konfigurerade toohave 10 partitioner och ett mål för tre repliker. I det här fallet Service Fabric skulle balansera och distribuera hello repliker över hello kluster – och du skulle hamna med två primära [repliker](service-fabric-availability-services.md) per nod.
Om du behöver nu tooscale ut hello too10 klusternoder, Service Fabric skulle balansera hello primära [repliker](service-fabric-availability-services.md) alla 10 noder. Likaså om du skala tillbaka too5 noder skulle Service Fabric balansera om alla hello repliker över hello 5 noder.  

Bild 2 visar hello distribution av 10 partitioner före och efter skalning hello klustret.

![Tillståndskänslig service](./media/service-fabric-concepts-partitioning/partitions.png)

Därför kan uppnås hello skalbar eftersom förfrågningar från klienter distribueras mellan datorer, bättre prestanda i programmet hello och minskar konkurrens på toochunks för åtkomst av data.

## <a name="plan-for-partitioning"></a>Planera för partitionering
Innan du implementerar en tjänst bör du alltid hello strategi som är nödvändiga tooscale out partitionering. Det finns olika sätt, men alla fokusera på vad hello-programmet måste tooachieve. Hello-kontexten för den här artikeln ska vi tänka på hello fler viktiga aspekter.

En bra metod är toothink om hello struktur hello tillstånd som behöver toobe partitioneras som hello första steget.

Låt oss ta ett enkelt exempel. Om du toobuild en tjänst för en countywide avsökning, kan du skapa en partition för varje ort i hello region. Sedan kan du lagra hello röster för varje person i hello stad i hello-partition som motsvarar toothat stad. Bild 3 illustrerar en uppsättning personer och hello stad där de finns.

![Enkel partition](./media/service-fabric-concepts-partitioning/cities.png)

Eftersom hello ifyllning av städer varierar mycket, kan det sluta med några partitioner som innehåller stora mängder data (till exempel Seattle) och andra partitioner med mycket lite tillstånd (t.ex. Kirkland). Vad är hello effekten av med partitioner med en ojämn mängder tillstånd?

Om du tycker om hello exempel igen kan kan du enkelt se att hello-partition som innehåller hello röster för Seattle kommer mer trafik än hello Kirkland en. Som standard Service Fabric ser till att det handlar om hello samma antal primära och sekundära repliker på varje nod. Därför kan det sluta med noder som har repliker som fungerar mer nätverkstrafik och andra som hanterar mindre trafik. Helst bör tooavoid hot och kalla platser som detta i ett kluster.

I ordning tooavoid detta bör du göra två saker från en partitionering synsätt:

* Försök toopartition hello tillstånd så att det är jämnt fördelad över alla partitioner.
* Rapportera belastning från varje hello repliker för hello-tjänsten. (Mer information om hur checka ut den här artikeln på [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric ger hello kapaciteten tooreport belastning som används av tjänster, till exempel minne eller antal poster. Baserat på hello mått rapporterade upptäcker Service Fabric att vissa partitioner hanterar högre belastning än andra och balanserar hello klustret genom att flytta repliker toomore lämplig noder, så att övergripande ingen nod är överbelastad.

Du kan ibland vet hur mycket data visas i en given partition. Så att en allmän rekommendation är toodo båda--först sprids genom användning av en partitioneringsstrategi som hello data jämnt över hello partitioner och andra, med reporting belastningen.  hello första metoden förhindrar situationer som beskrivs i hello röstning exempelvis medan hello andra hjälper jämna ut temporära skillnader i åtkomst eller belastningsutjämning över tid.

En annan aspekt av partition planering är toochoose hello rätt antal partitioner toobegin med.
Från ett Service Fabric-perspektiv finns det inget som förhindrar börjat med ett högre antal partitioner än förväntat för ditt scenario.
I själva verket förutsatt att hello högsta tillåtna antalet partitioner är en giltig metod.

I sällsynta fall kan du få behöver fler partitioner än du ursprungligen har valt. Du inte kan ändra antalet partitioner för hello efter hello fakta, behöver du tooapply vissa avancerade partition metoder, till exempel skapa en ny service-instans av hello samma typen tjänst. Du måste också tooimplement vissa klientsidan logik som dirigerar hello begär toohello rätt tjänstinstans, baserat på klientsidan kunskap som din klientkod måste underhåll.

Ett annat övervägande för partitionering av planering är hello tillgängliga datorresurser. Eftersom hello tillstånd måste toobe nås och lagras, är bundna toofollow:

* Gränser för bandbredd i nätverket
* System minnesgränserna
* Disk Lagringsgränser

Så vad händer om du stöter på resursen begränsningar i ett kluster som körs? hello svaret är att du kan helt enkelt skala ut hello tooaccommodate hello nya krav.

[Hej kapacitetsplaneringsguiden](service-fabric-capacity-planning.md) ger vägledning för hur toodetermine hur många noder i klustret måste.

## <a name="get-started-with-partitioning"></a>Kom igång med partitionering
Det här avsnittet beskrivs hur tooget igång med partitionering din tjänst.

Service Fabric kan välja mellan tre partitionsscheman:

* Låg partitionering (kallas även UniformInt64Partition).
* Namngivna partitionering. Program med hjälp av den här modellen normalt har data som kan vara bucketed, inom en begränsad mängd. Några vanliga exempel datafält som används som namngivna partitionsnycklar skulle vara regioner, postnummer, kundgrupper eller andra företag gränser.
* Singleton partitioneras. Singleton-partitioner används vanligtvis när hello-tjänsten inte kräver ytterligare routning. Till exempel använder tillståndslösa tjänster den här partitioneringsschema som standard.

Namnet och scheman som Singleton partitionering är särskilda typer av låg partitioner. Som standard låg hello Visual Studio mallar för Service Fabric partitionering, eftersom den är hello vanligaste en. hello resten av den här artikeln fokuserar på hello låg partitioneringsschema.

### <a name="ranged-partitioning-scheme"></a>Låg partitioneringsschema
Detta är att använda toospecify heltal intervallet (som identifieras med en nyckel för låg och hög nyckel) och ett antal partitioner (n). Den skapar n partitioner, varje ansvarar för en icke-överlappande underintervall av hello övergripande partitions viktiga intervall. Till exempel en ranged partitioneringsschema med en låg nyckel 0, övre nyckel 99 och antalet 4 skulle skapa fyra partitioner, enligt nedan.

![Intervallet partitionering](./media/service-fabric-concepts-partitioning/range-partitioning.png)

En vanlig metod är toocreate ett hash-värde baserat på en unik nyckel i hello datauppsättning. Några vanliga exempel nycklar skulle vara vehicle ID-nummer (VIN), en anställnings-ID eller en unik sträng. Med hjälp av den här Unik nyckel skulle du sedan skapa en hash-kod, modulus hello viktiga omfång, toouse som din nyckel. Du kan ange hello övre och nedre gränser för hello tillåtet viktiga intervall.

### <a name="select-a-hash-algorithm"></a>Välj en hash-algoritm
En viktig del av hashing är att välja hash-algoritm. En faktor är om hello målet är toogroup liknande nycklar nära varandra (ort känsliga hashing)-- eller om aktiviteten ska distribueras brett för alla partitioner (hash-distribution), vilket är mer vanligt.

hello-egenskaperna hos en fungerande distribution hash-algoritm är att det är enkelt toocompute, den har några kollisioner och distribuerar hello nycklar jämnt. Ett bra exempel på en effektiv hash-algoritm är hello [FNV 1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash-algoritm.

Mer allmän hash-kod algoritmen alternativ är hello [Wikipedia sida på hash-funktioner](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Skapa en tillståndskänslig tjänst med flera partitioner
Nu ska vi skapa din första tillförlitliga tillståndskänslig tjänst med flera partitioner. I det här exemplet skapar du ett enkelt program där du vill att toostore alla efternamn som börjar med samma enhetsbokstaven i hello hello samma partition.

Innan du skriva någon kod måste toothink om hello- och partitionsnycklar. Du behöver 26 partitioner (en för varje bokstav i alfabetet hello) men vad om hello låg och hög nycklar?
Vi vill bokstavligt toohave en partition per brev, kan vi använder 0 som hello låg nyckel och 25 som hello hög nyckel, som varje bokstav är egen.

> [!NOTE]
> Detta är en förenklad scenariot i verkligheten hello distribution är ojämnt. Senaste namn som börjar med hello bokstäver ”S” eller ”M” är vanligare än hello som börjar med ”X” eller ”Y”.
> 
> 

1. Öppna **Visual Studio** > **filen** > **nya** > **projekt**.
2. I hello **nytt projekt** dialogrutan Välj hello Service Fabric-programmet.
3. Anropa hello projektet ”AlphabetPartitions”.
4. I hello **skapar du en tjänst** dialogrutan Välj **Stateful** tjänst och anropa den ”Alphabet.Processing” enligt hello bilden nedan.
       ![Dialogrutan Ny tjänst i Visual Studio][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. Ange hello antalet partitioner. Öppna hello Applicationmanifest.xml filen finns i hello ApplicationPackageRoot mapp för hello AlphabetPartitions projekt och uppdatera hello parametern Processing_PartitionCount too26 enligt nedan.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Du måste också tooupdate hello LowKey och HighKey egenskaper för hello StatefulService element i hello ApplicationManifest.xml enligt nedan.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. För hello service toobe tillgänglig, öppna en slutpunkt på en port genom att lägga till hello endpoint element av ServiceManifest.xml (finns i hello PackageRoot mappen) för hello Alphabet.Processing service enligt nedan:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Hello-tjänsten är nu konfigurerad toolisten tooan intern slutpunkt med 26 partitioner.
7. Sedan måste toooverride hello `CreateServiceReplicaListeners()` -metoden i klassen för hello-bearbetning.
   
   > [!NOTE]
   > För det här exemplet förutsätter vi att du använder en enkel HttpCommunicationListener. Mer information om tillförlitlig kommunikation finns [hello tillförlitlig kommunikation tjänstmodell](service-fabric-reliable-services-communication.md).
   > 
   > 
8. En rekommenderad mönstret för hello URL: en som en replik som avlyssnar är hello följande format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Så du tooconfigure din kommunikation lyssnare toolisten på hello rätt slutpunkter och med det här mönstret.
   
    Flera repliker av den här tjänsten kan finnas på hello samma dator, så den här adressen måste toobe unika toohello repliken. Det är därför partitions-ID + replik-ID är i hello-URL. HttpListener kan lyssna på flera adresser på hello samma port som hello URL-prefixet är unikt.
   
    hello extra GUID finns det för ett avancerade fall där sekundära repliker också lyssna efter begäranden i skrivskyddat läge. När så är fallet hello vill toomake till att en ny unik adress används vid övergång från primära toosecondary tooforce klienter toore Lös hello adress. ”+” används som hello adress här så att hello repliken lyssnar på alla tillgängliga värdar (IP, FQDM, localhost, etc.) hello koden nedan visar ett exempel.
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Det är också värt att nämna att hello publicerade URL är något annorlunda än hello lyssnande URL-prefix.
    hello lyssnande URL anges tooHttpListener. hello publicerade URL: en är hello-URL som är publicerade toohello Service Fabric Naming Service, som används för identifiering av tjänst. Klienter begär den här adressen genom att discovery-tjänsten. hello adress att klienter får behov toohave hello faktiska IP eller FQDN för hello nod i ordning tooconnect. Så du måste tooreplace '+' med hello nodens IP eller FQDN som visas ovan.
9. hello sista steget är tooadd hello bearbetning logik toohello service enligt nedan.
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest`läser hello värdena för hello frågan sträng parameter används toocall hello partition och anrop `AddUserAsync` tooadd hello efternamn toohello tillförlitliga ordlista `dictionary`.
10. Lägg till en tillståndslösa tjänsten toohello projekt toosee hur du kan anropa en viss partition.
    
    Den här tjänsten fungerar som ett enkelt webbgränssnitt som accepterar hello efternamn som en frågesträngsparameter anger hello partitionsnyckel och skickar den toohello Alphabet.Processing service för bearbetning.
11. I hello **skapar du en tjänst** dialogrutan Välj **Stateless** tjänst och kalla den ”Alphabet.Web” enligt nedan.
    
    ![Skärmbild av tillståndslösa tjänsten](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Uppdatera hello endpoint informationen i hello ServiceManifest.xml hello Alphabet.WebApi service tooopen in en port som visas nedan.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Du måste tooreturn en mängd ServiceInstanceListeners i hello klass Web. Igen, kan du välja tooimplement en enkel HttpCommunicationListener.
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. Du måste nu tooimplement hello bearbetning logik. Hej HttpCommunicationListener anrop `ProcessInputRequest` när en begäran kommer. Så Låt oss och Lägg till hello koden nedan.
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Nu ska vi gå igenom den steg för steg. hello koden läser hello första bokstaven i hello frågesträngparametern `lastname` till char. Sedan den avgör hello partitionsnyckel för den här bokstaven genom att subtrahera hello hexadecimalt värde på `A` från hello hexadecimalt värde för hello efternamn med versal.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Kom ihåg att det här exemplet använder vi 26 partitioner med en partitionsnyckel per partition.
    Nu ska vi får hello service partition `partition` för den här nyckeln med hjälp av hello `ResolveAsync` metod på hello `servicePartitionResolver` objekt. `servicePartitionResolver`definieras som
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    Hej `ResolveAsync` hello partitionsnyckel, metoden tar hello tjänsten URI och en annullering token som parametrar. Hej URI för tjänsten för hello bearbetning av tjänsten är `fabric:/AlphabetPartitions/Processing`. Kan gå vi hello slutpunkten för hello partition.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Slutligen vi skapar hello slutpunkts-URL plus hello querystring och anropa hello bearbetning av tjänsten.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    När hello bearbetning görs skriva vi hello utdata tillbaka.
15. hello sista steget är tootest hello tjänst. Visual Studio använder programmet parametrar för lokal och distribution. tootest hello tjänsten lokalt 26 partitioner, behöver du tooupdate hello `Local.xml` filen i hello ApplicationParameters mappen för hello AlphabetPartitions projektet enligt nedan:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Du kan kontrollera hello-tjänsten och alla dess partitioner i hello Service Fabric Explorer när du är klar med distributionen.
    
    ![Service Fabric Explorer skärmbild](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. I en webbläsare, kan du testa hello partitionering logik genom att ange `http://localhost:8081/?lastname=somename`. Du ser att varje efternamn som börjar med samma enhetsbeteckning som lagras i hello hello samma partition.
    
    ![Skärmbild för webbläsare](./media/service-fabric-concepts-partitioning/samplerunning.png)

hello hela källkoden hello exemplet finns på [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="next-steps"></a>Nästa steg
Information om Service Fabric-begrepp finns hello följande:

* [Tillgänglighet för Service Fabric-tjänster](service-fabric-availability-services.md)
* [Skalbarheten för Service Fabric-tjänster](service-fabric-concepts-scalability.md)
* [Kapacitetsplanering för Service Fabric-program](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png