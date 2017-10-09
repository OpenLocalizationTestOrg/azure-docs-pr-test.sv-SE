---
title: "aaaPlanning hello kapacitet för Service Fabric-kluster | Microsoft Docs"
description: "Service Fabric-kluster kapacitetsplaneringsöverväganden. Nodetypes får, hållbarhet och tillförlitlighet nivåer"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: chackdan
ms.openlocfilehash: 83272ce7fefe698eef755cf66493c2874cc3b120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-kluster kapacitetsplaneringsöverväganden
För alla Produktionsdistribution är kapacitetsplanering ett viktigt steg. Här är en del av hello objekten som du har tooconsider som en del av den här processen.

* hello antal typer toostart ditt kluster behov ut med
* hello egenskaperna för varje nodtyp (storlek, primära internetuppkopplad, antalet virtuella datorer, osv.)
* hello tillförlitlighet och hållbarhet egenskaper hello kluster

Låt oss gå igenom objekten.

## <a name="hello-number-of-node-types-your-cluster-needs-toostart-out-with"></a>hello antal typer toostart ditt kluster behov ut med
Du måste först toofigure reda på vilka hello-klustret som du skapar kommer toobe som används för och vilka typer av program som du planerar toodeploy i det här klustret. Om du inte radera på hello syftet hello klustret, du troligen ännu inte klar tooenter hello kapacitetsplanering processen.

Upprätta hello antalet nodtyper klustret måste toostart ut med.  Varje nodtyp är mappade tooa Skaluppsättning för virtuell dator. Varje nodtyp kan sedan skalas upp eller ned separat, har olika uppsättningar av öppna portar och kan ha olika kapacitetsdata. Så handlar hello beslut hello antalet nodtyper i stort sett om toohello följande överväganden:

* Har ditt program flera tjänster och behöver någon av dem toobe offentliga eller mot internet? Vanliga program innehåller en frontend-gateway-tjänst som tar emot indata från en klient och en eller flera backend-tjänster som kommunicerar med hello frontend-tjänster. Så i detta fall kan få du med minst två nodtyper.
* Har dina tjänster (som utgör ditt program) annan infrastrukturbehov som större RAM-minne eller högre CPU-cykler? Låt oss anta hello programmet som du vill toodeploy innehåller en frontend-tjänst och en backend tjänst. hello frontend-tjänsten kan köras på mindre virtuella datorer (VM-storlekar som D2) som har portar är öppna toohello internet.  hello backend-tjänst, men beräkning intensiva och behov toorun på större virtuella datorer (med VM-storlekar som D4 D6, D15) som inte är internet riktas.
  
  I det här exemplet, men du kan välja tooput alla Hej tjänster på en nodtyp, rekommenderar vi att du ska placera dem i ett kluster med två nodtyper.  Därmed kan varje nod toohave distinkta egenskaper, till exempel internet-anslutning eller VM-storlek. hello antal virtuella datorer kan skalas oberoende av varandra, samt.  
* Eftersom du inte förutsäga framtida hello med fakta som du känner av och besluta om hello antalet nodtyper som dina program behöver toostart med. Du kan alltid lägga till eller ta bort nodtyper senare. Service Fabric-klustret måste ha minst en nodtyp.

## <a name="hello-properties-of-each-node-type"></a>hello egenskaper för varje nodtyp
Hej **nodtypen** kan ses som motsvarande tooroles i molntjänster. Nodtyper definiera hello VM-storlekar, hello antal virtuella datorer och deras egenskaper. Varje nodtyp som definieras i Service Fabric-klustret har konfigurerats som en separat virtuella datorns skaluppsättning. Skaluppsättning för virtuell dator är en Azure compute-resurs som du kan använda toodeploy och hantera en samling med virtuella datorer som en uppsättning. Varje nodtyp som definieras som olika virtuella datorn sedan kan skalas upp eller ned separat, har olika uppsättningar av öppna portar och kan ha olika kapacitetsdata.

Läs [dokumentet](service-fabric-cluster-nodetypes.md) för mer information om hello relation för nodetypes får toovirtual datorns skaluppsättning hur tooRDP till en hello instanser öppna nya portar osv.

Klustret kan ha fler än en nodtyp, men hello primära nodtypen (hello förstnämnda som du definierar i hello portal) måste ha minst fem datorer för kluster som används för produktionsarbetsbelastningar (eller minst tre virtuella datorer för testkluster). Om du skapar hello-kluster med hjälp av en Resource Manager-mall, leta efter **är primära** attribut under hello nod typdefinition. hello primära nodtypen är hello nodtypen Service Fabric systemtjänster placering.  

### <a name="primary-node-type"></a>Primära nodtypen
För ett kluster med flera nodtyper måste toochoose en av dem toobe primära. Här följer primära nodtypen hello egenskaper:

* Hej **minsta storlek på virtuella datorer** för hello primära nodtypen bestäms av hello **hållbarhetsnivån** du väljer. hello standardvärdet för hello hållbarhetsnivån är Brons. Rulla nedåt för information om vilka hello hållbarhetsnivån är och hello värden kan det ta.  
* Hej **minsta antal virtuella datorer** för hello primära nodtypen bestäms av hello **tillförlitlighetsnivån** du väljer. hello standardvärdet för hello tillförlitlighetsnivån är Silver. Rulla nedåt för information om vilka hello tillförlitlighetsnivån är och hello värden kan det ta. 


* hello Service Fabric-systemtjänster (till exempel hello Klusterhanterare för tjänsten eller Image Store service) är placerad hello primära nodtypen och så hello tillförlitlighet och hållbarhet hello kluster bestäms av hello nivå värdet och hållbarhet tillförlitlighetsnivån värdet som du väljer för hello primära nodtypen.

![Skärmbild av ett kluster med två nodtyper ][SystemServices]

### <a name="non-primary-node-type"></a>Icke-primära nodtypen
Det finns en primära nodtypen och hello resten av dem är icke-primär för ett kluster med flera nodtyper. Här följer hello egenskaperna för en icke-primära nodtypen:

* hello minimistorleken på virtuella datorer för den här nodtypen bestäms av hello hållbarhetsnivån som du väljer. hello standardvärdet för hello hållbarhetsnivån är Brons. Rulla nedåt för information om vilka hello hållbarhetsnivån är och hello värden kan det ta.  
* hello minsta antal virtuella datorer för den här nodtypen kan vara en. Dock bör du välja det här antalet baseras på hello antal repliker av hello/programtjänster som du vill att toorun i den här nodtypen. hello antal virtuella datorer i en nodtyp kan ökas när du har distribuerat hello klustret.

## <a name="hello-durability-characteristics-of-hello-cluster"></a>hello hållbarhet egenskaper hello kluster
Hej hållbarhetsnivån är används tooindicate toohello system hello privilegier dina virtuella datorer har med hello underliggande Azure-infrastrukturen. I hello primära nodtypen kan den här behörigheten Service Fabric toopause varje VM nivån infrastruktur begäran (till exempel omstart VM, VM avbildningsåterställning eller VM-migrering) som påverkar hello kvorum kraven för hello systemtjänster och tillståndskänsliga tjänster. I hello icke-primär nodtyper kan den här behörigheten Service Fabric toopause alla VM nivån infrastruktur förfrågningar som VM omstart, avbildningsåterställning VM, VM-migrering osv., som påverkar hello kvorum kraven för din tillståndskänsliga tjänster som körs i den.

Den här behörigheten uttrycks i hello följande värden:

* Guld - jobb kan pausas under en period på två timmar per UD hello-infrastruktur. Guld hållbarhet kan endast aktiveras fullständig nod VM SKU: er som D15_V2 G5 osv.
* Silver - hello infrastruktur jobb kan pausas för en varaktighet på 10 minuter per UD och är tillgänglig på alla standard virtuella datorer med enkel kärna och senare.
* Brons - inga privilegier. Detta är standardvärdet för hello. Endast använda den här nivån av hållbarhet för nodtyper som kör _endast_ tillståndslösa arbetsbelastningar. 

> [!WARNING]
> Nodetypes får körs med Brons hållbarhet hämta _inga privilegier_. Det innebär att infrastrukturen jobb som påverkar din tillståndslösa arbetsbelastningar inte stoppats eller fördröjd. Det är möjligt att sådan jobb fortfarande kan påverka dina arbetsbelastningar, orsaka driftsavbrott eller andra problem. För alla slags produktion arbetsbelastning körs med minst Silver rekommenderas. 
> 

Du får toochoose hållbarhet nivå för var och en av dina nodtyper. Du kan välja en nodtyp toohave hållbarhet andelen guld eller silver och hello andra har Brons i hello samma kluster. **Måste du upprätthålla en minsta antal 5 noder för alla nodtyper som har en hållbarhet guld eller silver**. 

**Fördelarna med att använda Silver eller guld hållbarhet nivåer**
 
1. Minskar hello antal steg i en åtgärd för skalan (det vill säga nodinaktiveringen och ta bort ServiceFabricNodeState anropas automatiskt)
2. Minskar hello risk för dataförlust på grund av tooa kund-initierad lokalt VM SKU ändring eller de Azure-infrastrukturen åtgärder.
     
**Nackdelarna med att använda Silver eller guld hållbarhet nivåer**
 
1. Distributioner tooyour Skaluppsättning för virtuell dator och andra relaterade Azure-resurser) kan vara fördröjd, kan tar för lång tid eller blockeras helt efter problem i klustret eller på hello infrastrukturnivå. 
2. Ökar hello antalet [replik Livscykelhändelser](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (till exempel primära växlingar) på grund av tooautomated nod avaktiveringar under Azure-infrastrukturen.

### <a name="recommendations-on-when-toouse-silver-or-gold-durability-levels"></a>Rekommendationer för när toouse Silver eller guld hållbarhet nivåer

Använd Silver eller guld hållbarhet för alla nodtyper som värd stateful tjänster du förväntar dig tooscale i (minska VM-instanser) ofta, och du föredrar att fördröjas distributionsåtgärder för att förenkla dessa åtgärder för skalan. hello skalbar scenarier (lägga till instanser för virtuella datorer) play inte till valet av hållbarhetsnivån hello, bara att skala i har.

### <a name="operational-recommendations-for-hello-node-type-that-you-have-set-toosilver-or-gold-durability-level"></a>Operativa rekommendationer för hello nod skriver du att du har angett toosilver eller guld hållbarhet nivå.

1. Skydda klustret och program felfria vid alla tidpunkter och se till att program svarar tooall [tjänsten replik Livscykelhändelser](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (t.ex. replik i build har fastnat) inom rimlig tid.
2. Anta säkrare sätt toomake VM SKU-ändra (skala upp eller ned): ändra hello VM SKU Skalningsuppsättning en virtuell dator är natur osäkra transaktionen och det bör undvikas om möjligt. Här är hello process som du kan följa tooavoid vanliga problem.
    - **För icke-primära nodetypes får:** rekommenderas att du skapar nya Virtual Machine Scale Set, ändra hello service placering tooinclude hello nya virtuella datorn/noden Villkorstyp och sedan minska hello gamla Virtual Machine Scale Set instansen antal too0, en nod i taget (detta är toomake till att borttagningen av hello noder inte påverkar hello tillförlitlighet hello kluster).
    - **För hello primära nodetype:** vår rekommendation är att du inte ändrar VM SKU hello primära nodtypen. Om hello skäl för hello nya SKU är kapacitet, vi rekommenderar att lägga till flera instanser eller skapa ett nytt kluster. Om du inte välja hello se ändringar toohello virtuella skala ange Model definition tooreflect nya SKU. Om klustret har endast en nodetype, kontrollerar du att alla tillståndskänsliga program svarar tooall [tjänsten replik Livscykelhändelser](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (t.ex. replik i build har fastnat) i rimlig tid och att tjänsten repliken återskapa varaktighet är mindre än fem minuter (Silver hållbarhet nivå). 


> [!WARNING]
> Ändra hello VM SKU-storleken för Skalningsuppsättningar inte körs på minst Silver hållbarhet är inte rekommenderade. Ändra storlek på VM-SKU är skadliga data på plats-infrastruktur. Utan någon möjlighet toodelay eller övervaka den här ändringen är det möjligt att hello åtgärden kan orsaka dataloss för tillståndskänsliga tjänster eller göra andra oförutsebara operativa problem, även för tillståndslösa arbetsbelastningar. 
> 
    
3. Underhåll en minsta antal fem noder för en Skalningsuppsättning virtuell dator som har aktiverat MR
4. Inte ta bort slumpmässiga VM-instanser måste alltid använda Virtual Machine Scale Set skala ned funktionen. hello borttagning av slumpmässiga VM-instanser har en potentiell skapar obalans i sprids UD och FD hello VM-instans. Detta att sänka belastningsutjämning för hello system möjligheten tooproperly bland hello service instanser/Service repliker.
6. Om du använder Autoskala, anger du hello regler så att skala (att ta bort av VM-instanser) görs endast en nod i taget. Det är inte säkert att skala ned mer än en instans i taget.
7. Om skalning av primära nodtypen skala du aldrig den ned över vilka hello tillförlitlighetsnivån tillåter.


## <a name="hello-reliability-characteristics-of-hello-cluster"></a>hello tillförlitlighetsegenskaper hello kluster
hello tillförlitlighetsnivån används tooset hello antal repliker hello systemtjänster som du vill toorun i det här klustret på hello primära nodtypen. Hej hello fler repliker, hello mer tillförlitlig hello systemtjänsterna är i klustret.  

hello tillförlitlighetsnivån kan ta hello följande värden:

* Platina - kör hello systemtjänster med en mål-replik ange antal 9
* Guld - kör hello systemtjänster med en mål-replik ange antal 7
* Silver - kör hello systemtjänster med en replik set målantalet 5 
* Brons - kör hello systemtjänster med en replik set målantalet 3

> [!NOTE]
> hello tillförlitlighetsnivån som du väljer bestämmer hello minsta antalet noder som den primära nodtypen måste ha. 
> 
> 


### <a name="recommendations-for-hello-reliability-tier"></a>Rekommendationer för hello tillförlitlighetsnivån.

 När du ökar eller minskar hello storleken på ditt kluster (hello summan av VM-instanser i alla nodtyper), måste du uppdatera klustret från ett lager tooanother hello tillförlitlighet. Detta utlöser hello uppgraderingar nödvändiga toochange hello system services replik set antalet kluster. Vänta tills hello uppgradering i förloppet toocomplete innan du gör några andra ändringar toohello klustret, som att lägga till noder.  Du kan övervaka hello hello uppgraderingen på Service Fabric-Utforskaren eller genom att köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Här är hello rekommendation om att välja hello tillförlitlighetsnivån.

| **Klusterstorleken** | **Tillförlitlighetsnivån** |
| --- | --- |
| 1 |Ange inte Hej tillförlitlighetsnivån parametern, beräknar hello systemet |
| 3 |Brons |
| 5 eller 6|Silver |
| 7 eller 8 |Guld |
| 9 och senare |Platina |




## <a name="primary-node-type---capacity-guidance"></a>Primära nodtypen - kapacitet vägledning

Här är hello vägledning för planering av hello primära noden typen kapacitet

1. **Antal Virtuella instanser toorun produktion arbetsbelastningar i Azure:** måste du ange en primär nod typen minimistorleken på 5. 
2. **Antal Virtuella instanser toorun test arbetsbelastningar i Azure** du kan ange en minsta primära noden typen storlek på 1 eller 3. Hej ett kluster med noder, körs med en specialkonfiguration och, skala ut från det klustret stöds inte. hello ett kluster med noder, har inga tillförlitlighet och i Resource Manager-mall du måste ange denna konfiguration tooremove ej (inte inställningsvärde hello configuration räcker inte). Om du ställer in hello kluster med en nod ställa in via portalen och sedan hello configuration automatiskt har åtgärdats. kluster med 1 och 3 noder stöds inte för att köra produktionsarbetsbelastningar. 
3. **VM-SKU:** primära nodtypen är där hello systemtjänster kör så hello VM SKU du väljer det, måste vidta till kontot hello övergripande högsta Utjämning av du planera tooplace till hello-kluster. Här är en liknelsen tooillustrate vad avses här - Se hello primära nodtypen som din ”lungor”, är det den erbjuder kontroll tooyour hjärna och därför om hello hjärna inte får tillräckligt med kontroll din brödtext drabbas av. 

Eftersom hello kapacitetsbehov i ett kluster bestäms av arbetsbelastning du planerar toorun i hello kluster, vi kan inte tillhandahålla kvalitativa vägledning för en viss arbetsbelastning, men här är hello bred vägledning toohelp dig att komma igång

För produktionsarbetsbelastningar 


- hello bör VM SKU är Standard D3 Standard D3_V2 eller motsvarande med minst 14 GB lokal SSD.
- hello minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande med minst 14 GB lokal SSD. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte för produktionsarbetsbelastningar av prestandaskäl.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Icke-primära nodtypen - kapacitet vägledning för tillståndskänsliga arbetsbelastningar

Den här vägledningen är för tillståndskänsliga arbetsbelastningar med hjälp av Service fabric [tillförlitliga samlingar eller reliable Actors](service-fabric-choose-framework.md) som körs i hello icke-primära nodtypen.


**Antal VM-instanser:** för produktionsarbetsbelastningar som är tillståndskänslig rekommenderas att köra med det lägsta och mål replikantalet 5. Det innebär att i stabilt tillstånd du att få en replik (från en replikuppsättning) i varje feldomän och uppgraderingsdomänen. hello hela tillförlitlighet nivå konceptet för hello primära nodtypen är ett sätt toospecify inställningen för systemtjänster. Hej så samma gäller tooyour tillståndskänsliga tjänster samt.

För produktionsarbetsbelastningar är hello minsta rekommenderade icke - primära noden teckenstorlek därför 5, om du kör tillståndskänsliga arbetsbelastningar i den.


**VM-SKU:** detta är hello nodtypen där ditt program körs, så hello VM SKU du väljer för det, måste ske med kontot hello belastning du planera tooplace på varje nod. Hej kapacitetsbehov för hello nodetype, bestäms av arbetsbelastning som du planerar toorun i hello kluster, så vi inte kan tillhandahålla kvalitativa vägledning för din specifika arbetsbelastning, men det här är hello bred vägledning toohelp dig att komma igång

För produktionsarbetsbelastningar 

- hello bör VM SKU är Standard D3 Standard D3_V2 eller motsvarande med minst 14 GB lokal SSD.
- hello minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande med minst 14 GB lokal SSD. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte specifikt för produktionsarbetsbelastningar av prestandaskäl.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Icke-primära nodtypen - kapacitet vägledning för tillståndslösa arbetsbelastningar

Den här vägledningen tillståndslösa arbetsbelastningar som körs på hello icke-primär nodetype.

**Antal VM-instanser:** för produktionsarbetsbelastningar som är tillståndslös hello minsta stöd för icke - primära noden standardstorleken är 2. Detta gör att du toorun du två tillståndslös instanser av programmet, så att din tjänst toosurvive hello förlust av en VM-instans. 

> [!NOTE]
> Om klustret körs på en service fabric-version lägre än 5.6 körning (det här problemet löses i 5.6), skalning av en icke-primär nod typen tooless än 5, på grund av tooa defekt i hello resulterar i klustret hälsa aktivera feltillstånd, tills du anropa [ Ta bort ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) med hello lämpliga nodnamn. Läs [utföra Service Fabric-klustret in eller ut](service-fabric-cluster-scale-up-down.md) mer information
> 
>

**VM-SKU:** detta är hello nodtypen där ditt program körs, så hello VM SKU du väljer för det, måste ske med kontot hello belastning du planera tooplace på varje nod. Hej kapacitetsbehov för hello nodetype, bestäms av arbetsbelastning som du planerar toorun i hello kluster, så vi inte kan tillhandahålla kvalitativa vägledning för din specifika arbetsbelastning, men det här är hello bred vägledning toohelp dig att komma igång

För produktionsarbetsbelastningar 


- hello bör VM SKU är Standard D3 Standard D3_V2 eller motsvarande. 
- hello minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte för produktionsarbetsbelastningar av prestandaskäl.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Nästa steg
När du slutför din kapacitetsplanering och konfigurera ett kluster kan du läsa hello följande:

* [Säkerhet för Service Fabric-kluster](service-fabric-cluster-security.md)
* [Service Fabric hälsa modellen introduktion](service-fabric-health-introduction.md)
* [Ange relationen mellan nodetypes får tooVirtual datorn skala](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
