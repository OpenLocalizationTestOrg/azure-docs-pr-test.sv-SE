---
title: aaaCluster Resource Manager klustret beskrivning | Microsoft Docs
description: "Som beskriver ett Service Fabric-kluster genom att ange Feldomäner, uppgradera domäner, nodegenskaper och noden kapaciteter för hello klustret Resource Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f2822075976bd54402af5ad56991b5b360dfb1d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="describing-a-service-fabric-cluster"></a>Som beskriver ett service fabric-kluster
hello resurshanteraren för Service Fabric-kluster tillhandahåller mekanismer för att beskriva ett kluster. Hello klustret Resource Manager använder information tooensure hög tillgänglighet hello-tjänster som körs i hello kluster under körning. Medan reglerna viktigt, försöker den också toooptimize hello resursförbrukning inom hello kluster.

## <a name="key-concepts"></a>Viktiga begrepp
hello hanteraren för filserverresurser stöder flera funktioner som beskriver ett kluster:

* Feldomäner
* Uppgradera domäner
* Nodegenskaper
* Noden kapacitet

## <a name="fault-domains"></a>Feldomäner
En Feldomän är en del av samordnade fel. En enskild dator är en Feldomän (eftersom den kan misslyckas på en egen för olika skäl, från power ange fel toodrive fel toobad NIC firmware). Datorer som är anslutna toohello samma Ethernet-switch finns i hello samma Feldomän som är datorer som delar en enda källa ström eller på en enda plats. Eftersom det är naturligt för maskinvara fel toooverlap Feldomäner är natur hierarkisk och visas i form av URI: er i Service Fabric.

Det är viktigt att Feldomäner har ställts in korrekt eftersom Service Fabric använder denna information toosafely plats tjänster. Service Fabric vill inte tooplace services så att hello förlust av en Feldomän (orsakas av hello misslyckades för vissa komponenten) orsakar en service toogo ned. Konfigurera hello noder i klustret hello i hello Azure Service Fabric använder hello Feldomän information från hello miljö toocorrectly-miljön för din räkning. För Service Fabric fristående, Feldomäner definieras när hello har hello klustret konfigurerats 

> [!WARNING]
> Det är viktigt att hello Feldomän information tillhandahålls tooService Fabric är korrekt. Anta exempelvis att att din Service Fabric klusternoder körs inom 10 virtuella datorer som körs på fem fysiska värdar. I det här fallet, även om det finns 10 virtuella datorer, det finns endast 5 olika (toppnivå) fault-domäner. Dela hello som gör att samma fysiska värd VMs tooshare hello samma roten feldomän eftersom hello VMs upplevelse koordineras fel om inte deras fysiska värden.  
>
> Eftersom Service Fabric förväntar hello Feldomän för en nod inte toochange. Andra mekanismer för att säkerställa hög tillgänglighet för hello virtuella datorer, t.ex [hög tillgänglighet för virtuella datorer](https://technet.microsoft.com/en-us/library/cc967323.aspx), Använd transparent migrering av virtuella datorer från en värd tooanother. Dessa mekanismer inte konfigurera om eller meddela hello kör kod i hello VM. Därför är **stöds inte** som miljöer för att köra Service Fabric-kluster. Service Fabric ska hello endast hög tillgänglighet teknik anställda. Mekanismer som VM Direktmigrering, SAN-nätverk eller andra behövs inte. Om används tillsammans med Service Fabric metoderna _minska_ programmets tillgänglighet och tillförlitlighet eftersom de införa ytterligare komplexitet, lägga till centraliserad källor till felet och använda tillförlitlighet och tillgänglighet strategier som står i konflikt med de i Service Fabric. 
>
>

Vi färg alla hello entiteter som bidrar tooFault domäner och lista alla hello olika Feldomäner som resulterar i hello bilden nedan. I det här exemplet har vi datacenter (”DC”), rack (”R”) och blad (”B”). Möjligen, om varje bladet innehåller mer än en virtuell dator, kan det finnas ett annat skikt i hello Feldomän hierarki.

<center>
![Noder ordnade via Feldomäner][Image1]
</center>

Under körning hello resurshanteraren för Service Fabric-klustret anser hello Feldomäner i hello kluster och planerar layouter. Hej tillståndskänslig repliker eller tillståndslösa instanser för en viss tjänst distribueras så att de är i separata Feldomäner. Distribuerar hello-tjänsten över feldomäner blir hello tillgänglighet av hello-tjänsten inte osäker när en feldomän på alla nivåer i hello hierarki.

Resurshanteraren för Service Fabric-klustret hand inte hur många lager i hello Feldomän hierarki. Dock försöker programmet tooensure hello förlust av någon en del av hello hierarkin inte påverkar tjänster som körs i den. 

Det är bäst om det finns hello samma antal noder på varje nivå på djupet i hello Feldomän hierarki. Om hello ”träd” Feldomäner Obalanserat i klustret, gör det svårare för hello klustret Resource Manager toofigure ut hello bästa fördelning av tjänster. Imbalanced Feldomäner layouter innebär att hello förlust av vissa domäner påverkan hello tillgängligheten för tjänster som är mer än andra domäner. Därför hello klustret Resource Manager torn mellan två mål: den vill toouse hello datorer i domänen ”tunga” genom att placera tjänster på dem och det vill tooplace tjänster i andra domäner så att hello förlust av en domän inte orsaka problem. 

Hur ser imbalanced domäner ut? I hello diagrammet nedan visar vi två olika kluster layouter. I hello första exemplet jämnt hello noder över hello Feldomäner. I andra exemplet hello har en Feldomän fler noder än hello andra Feldomäner. 

<center>
![Två olika kluster layouter][Image2]
</center>

I Azure hanteras hello val som Feldomän innehåller en nod åt dig. Men, beroende på hello antalet noder som du etablerar du kan fortfarande få Feldomäner med flera noder i dem än andra. Anta exempelvis att du har fem Feldomäner i hello kluster men etablera sju noder för en given NodeType. I det här fallet få hello två första Feldomäner fler noder. Om du fortsätter toodeploy mer nodetypes får med bara några instanser hämtar hello problemet försämras. Därför rekommenderar vi att hello är antalet noder i varje nodtyp en multipel av hello antalet Feldomäner.

## <a name="upgrade-domains"></a>Uppgradera domäner
Uppgradera domäner är en annan funktion som hjälper hello resurshanteraren för Service Fabric-klustret förstå hello layout hello-klustret. Uppgraderingsdomäner definiera uppsättningar av noder som uppgraderas vid hello samtidigt. Uppgradera domäner hjälp hello klustret Resource Manager förstå och samordna hanteringsåtgärder som uppgraderingar.

Uppgradera domäner är mycket som Feldomäner, men med några viktiga skillnader. Först definierar delar av samordnade maskinvarufel Feldomäner. Uppgradera domäner, på andra sidan hello, definieras av principen. Du får toodecide hur många du vill att, i stället för den som anges av hello-miljö. Du kan ha valfritt antal uppgradera domäner som du har noder. En annan skillnaden mellan Feldomäner och uppgradera domäner är att uppgradera domäner inte är hierarkiska. De är däremot mer som en enkel tagg. 

hello följande diagram visar tre uppgradera domäner stripe över tre Feldomäner. Det visar också ett möjligt placering för tre olika repliker av tillståndskänsliga tjänster, där varje avslutas med olika fel- och uppgradera domäner. Den här placering kan hello förlust av en Feldomän i hello mitten av en uppgradering av tjänsten och fortfarande har en kopia av hello kod och data.  

<center>
![Placering med fel- och Uppgraderingsdomäner][Image3]
</center>

Det finns för- och nackdelar toohaving stort antal uppgradera domäner. Fler uppgradera domäner innebär varje steg i hello uppgraderingen är mer detaljerade och påverkar därför ett mindre antal noder eller tjänster. Detta innebär färre tjänsterna har toomove samtidigt, införa mindre omsättning i hello system. Detta brukar tooimprove tillförlitlighet, eftersom mindre hello service påverkas av eventuella problem som introduceras under hello uppgraderingen. Fler uppgradera domäner innebär också att du behöver mindre tillgängliga bufferten på andra noder toohandle hello effekten av hello uppgraderar. Till exempel om du har fem uppgradera domäner hello noder i varje hanterar ungefär 20 procent av trafiken. Om du behöver tootake ned uppgradera domänen för en uppgradering måste att belastningen vanligtvis toogo någonstans. Eftersom du har fyra återstående uppgradera domäner måste ha plats för cirka 5% av hello totala trafiken. Fler uppgradera domäner innebär att du behöver mindre buffert på hello noder i klustret hello. Anta till exempel att om du har haft 10 uppgradera domäner i stället. Varje UD hanterar i så fall endast cirka 10% av totalt antal hello-trafik. När en uppgradering via hello klustret måste varje domän endast toohave utrymme för ungefär 1.1% av totalt antal hello-trafik. Fler uppgradera domäner kan du vanligtvis toorun noderna med högre användning, eftersom du behöver mindre reserverad kapacitet. hello sak samma gäller för Feldomäner.  

hello Nackdelen med att ha många uppgradera domäner är att uppgraderingar tenderar tootake längre. Service Fabric väntar en kort tidsperiod när en uppgradering domän är klar och utför kontroller innan du startar tooupgrade hello nästa. Dessa fördröjningar kan identifiera problem som introducerades av hello uppgraderingen innan hello uppgraderingen fortsätter. hello förhållandet är acceptabelt eftersom den förhindrar att felaktiga ändringar påverkar för mycket av hello tjänst i taget.

För få uppgradera domäner har många negativa sidoeffekter – när varje enskild uppgradera domän är nere och uppgraderas en stor del av den totala kapaciteten är inte tillgänglig. Till exempel om du bara har tre uppgradera domäner tar du ned ungefär 1/3 av din övergripande tjänst eller ett kluster kapacitet i taget. På samma gång med stor del av din tjänst ned inte önskvärt eftersom du har toohave tillräckligt med kapacitet i hello resten av klustret toohandle hello arbetsbelastningen. Underhålla det buffert som innebär att under normal drift är de noderna mindre inlästa än de skulle vara annars. Detta ökar hello kostnaden för att köra din tjänst.

Det finns inget verkliga gränsen toohello Totalt antal fel eller uppgradera domäner i en miljö eller begränsningar för hur de överlappar varandra. Namn, det finns flera vanliga mönster:

- Feldomäner och uppgradera domäner mappas 1:1
- En uppgradering domän per nod (fysisk eller virtuell OS instans)
- En modell för ”stripe” eller ”matris” där hello Feldomäner och uppgradera domäner utgör en matris med datorer som körs vanligtvis ned hello diagonala linjer

<center>
![Fel- och Uppgraderingsdomänen layouter][Image4]
</center>

Det finns inga bästa besvara vilken layout toochoose, var och en har vissa- och nackdelar. Hej 1FD:1UD modellen är till exempel enkel tooset upp. hello är 1 Uppgraderingsdomänen per nod modell de flesta som vad användare används för att. Under uppgraderingar uppdateras varje nod oberoende av varandra. Detta påminner om toohow små mängder datorer har uppgraderats manuellt i hello tidigare. 

hello vanligaste modellen är hello FD/UD matris, där hello FDs och UDs utgör en tabell och noder placeras startar längs hello diagonal. Detta är hello-modell som används som standard i Service Fabric-kluster i Azure. Kluster med flera noder hamnar allt ser ut som hello kompakta matris mönster ovan.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Fel- och uppgradera domän begränsningar och resultatet
hello klustret Resource Manager behandlar hello önskan tookeep en tjänst belastningsutjämnade över fel och uppgradera domäner som en begränsning. Du hittar mer information om begränsningar i [i den här artikeln](service-fabric-cluster-resource-manager-management-integration.md). Hej fel- och uppgradera domän begränsningar tillstånd ”: för en viss tjänst partition det ska aldrig vara skillnad *större än ett* i hello antal tjänstobjekt (tillståndslös tjänstinstanser eller tillståndskänslig service repliker) mellan två domäner ”. Detta förhindrar att vissa flyttar eller åtgärder som strider mot den här begränsningen.

Nu ska vi titta på ett exempel. Anta att vi har ett kluster med sex noder är konfigurerade med fem Feldomäner och fem uppgradera domäner.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Nu anta att vi skapa en tjänst med en TargetReplicaSetSize (eller för den tillståndslösa tjänsten en InstanceCount) fem. hello repliker mark på N1 N5. I själva verket används N6 aldrig oavsett hur många tjänster så här du skapar. Men varför? Nu ska vi titta på hello skillnaden mellan hello aktuell layout och vad som skulle hända om N6 väljs.

Här är hello layout vi har fått och hello totala antalet repliker per fel- och uppgradera domän:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Den här layouten balanseras vad gäller noder per fel och uppgradera domänen. Det är också balanserad vad gäller hello antal repliker per fel- och uppgradera domän. Varje domän har hello samma antal noder och hello samma antal repliker.

Nu ska vi titta på vad som skulle hända om vi hade använt N6 i stället för N2. Hur skulle hello repliker att distribueras sedan?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Den här layouten strider mot vår definition för hello Feldomän begränsning. FD0 har två repliker medan FD1 är noll, ringa hello skillnaden mellan FD0 och FD1 totalt två. hello klustret Resource Manager tillåter inte den här ordningen. På liknande sätt om vi plockats N2 och N6 (i stället för N1 och N2) skulle vi få:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Den här layouten balanseras vad gäller Feldomäner. Men nu den bryter mot hello uppgradera domänbegränsningar. Det beror på att UD0 har noll repliker medan UD1 har två. Därför den här layouten också är ogiltigt och nå inte hello klustret Resource Manager. 

## <a name="configuring-fault-and-upgrade-domains"></a>Konfigurera fel- och uppgradera domäner
Definiera Feldomäner och uppgradera domäner görs automatiskt i Azure värdbaserade Service Fabric-distributioner. Service Fabric hämtar och använder hello miljöinformation från Azure.

Om du skapar ditt eget kluster (eller vill toorun en viss topologi under utveckling) kan du ange hello fel och uppgradera domänen information själv. I det här exemplet definierar vi nio lokal utveckling kluster med en nod som omfattar tre ”datacenter” (var och en med tre rack). Det här klustret har också tre uppgradera domäner stripe över de tre datacenter. Ett exempel på hello configuration understiger: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

via ClusterConfig.json för fristående distributioner

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> När du definierar kluster via Azure Resource Manager tilldelas Feldomäner och uppgradera domäner av Azure. Därför innehåller hello definition av din nodtyper och virtuella datorer i Azure Resource Manager-mallen inte Feldomän eller uppgradera domäninformation.
>

## <a name="node-properties-and-placement-constraints"></a>Egenskaper för en nod och placeringen
Ibland (faktum för hello mesta) ska toowant tooensure som vissa arbetsbelastningar som körs bara på vissa typer av noder i klustret hello. Exempelvis kanske vissa arbetsbelastning kräver GPU-kort eller SSD och andra inte. Ett bra exempel på mål maskinvara tooparticular arbetsbelastningar är i stort sett alla där n-nivå-arkitektur. Vissa datorer fungera som hello klientdelen eller betjänar sida av programmet hello-API och är utsatta toohello klienter eller hello internet. Olika datorer, ofta med olika maskinvaruresurser hantera hello arbete hello beräkning eller lagring lager. Dessa är ofta _inte_ direkt exponerade tooclients eller hello internet. Service Fabric förväntar att det finns fall där viss arbetsbelastning behöver toorun på specifika maskinvarukonfigurationer. Exempel:

* har ”lyfts och flyttat” i ett befintligt program i flera nivåer i en miljö med Service Fabric
* en arbetsbelastning vill toorun på maskinvara för prestanda, skala och isolering av säkerhetsskäl
* En arbetsbelastning isoleras från andra arbetsbelastningar för princip- eller förbrukning orsaker

toosupport dessa typer av konfigurationer för Service Fabric har förstklassigt begreppet taggar som kan vara tillämpade toonodes. Dessa taggar kallas **nodegenskaper**. **Placeringen** är hello instruktioner kopplade tooindividual tjänster väljer för en eller flera egenskaper i noden. Placeringen definiera där tjänsterna ska köras. hello uppsättning begränsningar kan utökas - alla nyckel/värde-par fungerar. 

<center>
![Klustret Layout olika arbetsbelastningar][Image5]
</center>

### <a name="built-in-node-properties"></a>Inbyggda nodegenskaper
Service Fabric definierar vissa standardegenskaper nod som automatiskt kan användas utan hello användaren har toodefine dem. hello standardegenskaperna som definierats vid varje nod är hello **NodeType** och hello **nodnamn**. Exempelvis så du kan skriva ett placering villkor som `"(NodeType == NodeType03)"`. Vi har vanligtvis hittat NodeType toobe en av de vanligaste hello egenskaper. Det är användbart eftersom det motsvarar en typ av en dator 1:1. Varje typ av dator motsvarar tooa typ av arbetsbelastning i ett program med traditionella n-nivå.

<center>
![Placeringen och nodegenskaper][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Begränsning för placering och noden egenskapssyntax 
hello-värdet som anges i hello nodegenskap kan vara en sträng, bool, eller signerat länge. hello-instruktion på hello tjänsten kallas en placering *begränsningen* eftersom den begränsar där hello kan köras i hello kluster. hello-begränsningen kan vara alla booleskt uttryck som körs på hello olika nodegenskaper i hello kluster. hello giltig väljare i dessa booleskt uttryck är:

1) villkorliga kontroller för att skapa specifika instruktioner

| Instruktionen | Syntax |
| --- |:---:|
| ”lika med” | "==" |
| ”inte lika med” | "!=" |
| ”större än” | ">" |
| ”större än eller lika med” | ">=" |
| ”mindre än” | "<" |
| ”mindre än eller lika med” | "<=" |

2) Booleskt uttryck för gruppering och logiska åtgärder

| Instruktionen | Syntax |
| --- |:---:|
| ”och” | "&&" |
| ”eller” | "&#124;&#124;" |
| ”inte” | "!" |
| ”gruppen som enskild instruktion” | "()" |

Här följer några exempel på grundläggande begränsning instruktioner.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Endast noder där hello övergripande placering begränsningen uttryck utvärderas för ”True” kan ha hello-tjänst som placeras på den. Noder som inte har en definierad egenskap matchar inte någon placering begränsningen som innehåller egenskapen.

Anta att hello följande egenskaper har definierats för en viss nod nod:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

värdbaserad kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure. 

> [!NOTE]
> I noden Azure Resource Manager mallen hello parametriserade vanligtvis typen. Det skulle se ut som ”[parameters('vmNodeType1Name')]” i stället för ”NodeType01”.
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Du kan skapa tjänsten placering *begränsningar* för en tjänst som på följande sätt:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Om alla noder i NodeType01 är giltiga, du kan också välja att nodtypen med hello begränsningen ”(NodeType == NodeType01)”.

En av hello bästa sakerna placeringen för en tjänst är att de kan uppdateras dynamiskt vid körning. Så om du behöver kan du flytta en tjänst i hello kluster, lägga till och ta bort krav, osv. Service Fabric hand tar om garanterar säkerheten hello service in och är tillgänglig även när dessa typer av ändringar görs.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Placeringen har angetts för varje olika namngivna service-instans. Uppdateringar alltid ske hello av (Skriv över) det tidigare har angetts.

hello klustret definition definierar hello egenskaper på en nod. Ändra egenskaper för en nod kräver en uppgradering för konfiguration av klustret. Uppgradera en nodegenskaper kräver varje påverkade noden toorestart tooreport dess nya egenskaper. Dessa rullande uppgraderingar hanteras av Service Fabric.

## <a name="describing-and-managing-cluster-resources"></a>Som beskriver och hantera klusterresurser
En av de viktigaste jobb av alla orchestrator är toohelp hello hantera resursförbrukning i hello klustret. Hantera klusterresurser kan innebära ett antal olika saker. Först, det är att säkerställa att datorer inte är överbelastad. Det innebär att se till att datorer inte kör fler tjänster än de kan hantera. Det finns andra, belastningsutjämning och optimering som är kritiska toorunning services effektivt. Kostnad gällande eller prestanda känsliga Tjänsterbjudanden tillåta inte hot vissa noder toobe medan andra är Kall. Varm noder leda tooresource konkurrens och sämre prestanda och kalla noder representerar gått förlorat resurser och ökade kostnader. 

Service Fabric representerar resurser som `Metrics`. Mått är en logisk eller fysisk resurs som du vill toodescribe tooService Fabric. Exempel på mått är till exempel ”WorkQueueDepth” eller ”MemoryInMb”. Information om hello fysiska resurser som Service Fabric kan styra på noder finns [resurs styrning](service-fabric-resource-governance.md). Information om hur du konfigurerar anpassade mått och hur används finns i [i den här artikeln](service-fabric-cluster-resource-manager-metrics.md)

Mått skiljer sig från placeringar begränsningar och egenskaper för en nod. Nodegenskaper är statisk beskrivningar av själva hello noderna. Mått beskrivs resurser där noder och att tjänster ska använda när de körs på en nod. En nodegenskap kan vara ”HasSSD” och kan ställas in tootrue eller false. hello mängden ledigt utrymme på den SSD och hur mycket som används av tjänster är ett mått som ”DriveSpaceInMb”. 

Det är viktigt toonote som precis som för placeringen och egenskaper för en nod, hello resurshanteraren för Service Fabric-klustret inte förstå vilka hello namnen på hello mått medelvärdet. Tjänstmåttets namn är bara strängar. Det är ett bra toodeclare enheter som en del av hello mått som du skapar när det kan vara tvetydig.

## <a name="capacity"></a>Kapacitet
Om du har inaktiverat alla *NLB*, Service Fabric-klustret Resource Manager skulle fortfarande se till att ingen nod avslutas över sin kapacitet. Det är möjligt att hantera kapacitet loggens överskrids såvida hello klustret är full eller hello arbetsbelastningen är större än en nod. Kapaciteten är en annan *begränsningen* som hello hanteraren för filserverresurser använder toounderstand hur mycket av en resurs en nod har. Återstående kapacitet också spåras hello klustret som helhet. Både hello kapacitet och hello förbrukning i hello servicenivå uttrycks i mått. Exempelvis hello mått kan vara ”ClientConnections” och en viss nod kan ha en kapacitet för ”ClientConnections” på 32768. Andra noder kan har andra begränsningar vissa-tjänsten körs på den nod kan säga den för närvarande förbrukar 32256 av hello mått ”ClientConnections”.

Under körning spårar hello klustret Resource Manager återstående kapacitet i hello klustret och på noder. I ordning tootrack subtraherar kapacitet hello klustret Resource Manager varje tjänst användning från nodens kapacitet där hello-tjänsten körs. Med den här informationen hello resurshanteraren för Service Fabric-klustret kan ta reda på var tooplace eller flytta repliker så att noderna inte gå igenom kapacitet.

<center>
![Klusternoder och kapacitet][Image7]
</center>

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Du kan se de kapaciteter som definierats i klustermanifestet hello:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

värdbaserad kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure. 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Ofta en tjänst att läsa in ändras dynamiskt. Anta att en replik belastningen på ”ClientConnections” har ändrats från 1024 too2048 men hello nod kördes på därefter endast hade 512 kapacitet kvar för att mått. Nu den repliken eller instansens placering är ogiltigt, eftersom det inte finns tillräckligt med utrymme på noden. hello klustret Resource Manager har tookick i och få hello nod under kapacitet. Det minskar belastningen på hello-nod som är överbelastade genom att flytta en eller flera av hello repliker eller instanser från de nod tooother noderna. När du flyttar repliker försöker hello klustret Resource Manager toominimize hello kostnaden för dessa transporter. Förflyttningskostnad beskrivs i [i den här artikeln](service-fabric-cluster-resource-manager-movement-cost.md) mer om hello klustret Resource Manager ombalansering strategier och regler beskrivs [här](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Kluster-kapacitet
Hur hello resurshanteraren för Service Fabric-klustret keep hello övergripande kluster från att vara full? Dessutom med dynamiska belastning som det inte finns mycket du kan göra. Tjänster kan ha sina belastningen topp oberoende av åtgärder som vidtas av hello klustret Resource Manager. Klustret med tillräckligt med utrymme i dag kan därför vara underpowered när du blir berömda i morgon. Namn, det finns vissa kontroller som är inbyggd i tooprevent problem. hello första vi kan göra är förhindra hello skapandet av nya arbetsbelastningar som skulle orsaka hello klustret toobecome fullständig.

Anta att du skapar en tillståndslös tjänst och den innehåller vissa som är kopplade till den. Anta att hello service mån om hello ”DiskSpaceInMb” mått. Anta också att det är pågående tooconsume fem enheter av ”DiskSpaceInMb” för varje instans av hello-tjänsten. Vill du toocreate tre instanser av hello-tjänsten. Bra! Så att innebär att vi behöver 15 enheter av ”DiskSpaceInMb” toobe i hello kluster för oss tooeven vara kan toocreate dessa instanser av tjänsten. hello klustret Resource Manager beräknar kontinuerligt hello kapacitet och användning av varje mått så att den kan se hello återstående kapacitet i hello klustret. Om det inte finns tillräckligt med utrymme, skapa hello klustret Resource Manager avvisar hello anropet till tjänsten.

Eftersom hello krävs endast att det finnas 15 enheter, kan detta utrymme allokeras många olika sätt. Till exempel, kan det finnas en återstående enhet av kapacitet på 15 olika noder eller tre återstående enheter av kapacitet på fem olika noder. Om hello klustret Resource Manager kan ordna om saker så att det finns fem enheter på tre noder, placerar hello-tjänsten. Ordna om hello-kluster är vanligtvis inte hello klustret är nästan full eller går inte att konsolidera hello befintliga tjänster av någon anledning.

## <a name="buffered-capacity"></a>Buffrat kapacitet
Buffrat kapacitet är en annan funktion i hello klustret Resource Manager. Det tillåter reservation av en del av hello övergripande nodens kapacitet. Den här kapaciteten bufferten är endast använda tooplace tjänster under uppgraderingar och nodfel. Buffrat kapacitet anges globalt per mått för alla noder. hello-värde som du väljer för hello reserverad kapacitet är en funktion av hello antal fel och uppgradera domäner som du har i hello kluster. Flera fel och uppgradera domäner innebär att du kan välja ett lägre värde för din buffrat kapacitet. Om du har flera domäner, du kan förvänta sig mindre mängder kluster-toobe otillgänglig under uppgraderingar och fel. Ange buffras kapacitet endast meningsfullt om du har angivna hello nod kapacitet för ett mått.

Här är ett exempel på hur toospecify buffras kapacitet:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

hello skapandet av nya tjänster misslyckas när hello klustret ligger utanför buffrat kapacitet för ett mått. Hindrar hello skapandet av nya tjänster toopreserve hello bufferten säkerställer att uppgraderingar och fel inte orsaka noder toogo överbelastade. Buffrat kapacitet är valfritt men rekommenderas i ett kluster som definierar kapaciteten för ett mått.

hello klustret Resource Manager visar informationen belastningen. Informationen omfattar för varje mått: 
  - hello buffras kapacitetsinställningarna
  - hello total kapacitet
  - hello strömförbrukningen
  - Anger om varje mått anses belastningsutjämnade eller inte
  - statistik om hello standardavvikelse
  - hello-noder som har hello belastning på de flesta och minst  
  
Nedan finns det ett exempel på dessa utdata:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Nästa steg
* Information om hello arkitektur och information om flödet i hello klustret Resource Manager kolla [den här artikeln](service-fabric-cluster-resource-manager-architecture.md)
* Definiera defragmentering mått är ett sätt tooconsolidate belastningen på noder i stället för att sprida. toolearn hur tooconfigure defragmenteringen finns för[i den här artikeln](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)
* toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
