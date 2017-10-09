---
title: "aaaScalability av Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver hur tooscale Service Fabric-tjänster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5af06f8f71ad5dee32ba115b922842684867e654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-in-service-fabric"></a>Skalning i Service Fabric
Azure Service Fabric gör det enkelt toobuild skalbara program genom att hantera hello tjänster, partitioner och repliker på hello noder i ett kluster. Många arbetsbelastningar som körs hello samma maskinvara aktiverar maximala resursutnyttjande, men ger även flexibilitet i hur du väljer tooscale dina arbetsbelastningar. 

Skalningen i Service Fabric ska ske på flera olika sätt:

1. Skalning genom att skapa eller ta bort instanser av tillståndslösa tjänsten
2. Skalning genom att skapa eller ta bort nya heter tjänster
3. Skalning genom att skapa eller ta bort nya namngivna instanser av programmet
4. Skalning med partitionerade tjänster
5. Skalning genom att lägga till och ta bort noder från hello kluster 
6. Skalning med hjälp av hanteraren för filserverresurser mått

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Skalning genom att skapa eller ta bort instanser av tillståndslösa tjänsten
En av hello enklaste sätt tooscale i Service Fabric fungerar med tillståndslösa tjänster. När du skapar en tillståndslösa tjänsten måste du få en chans toodefine en `InstanceCount`. `InstanceCount`definierar hur många kopior av den tjänsten kod som körs skapas när hello-tjänsten startas. Anta exempelvis att det finns 100 noder i klustret hello. Även anta att en tjänst har skapats med en `InstanceCount` 10. Under körning kan de 10 körs kopiorna av hello kod kan bli alla upptagen (eller kan inte vara tillräckligt upptagen). Enkelriktade tooscale att arbetsbelastningen är toochange hello antal instanser. Vissa kodavsnitt övervakning eller hantering kan till exempel ändra hello befintliga antalet instanser too50 eller too5, beroende på om hello arbetsbelastning behöver tooscale in eller ut utifrån hello ladda. 

C#:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Med dynamisk instanser
Service Fabric erbjuder instansantalet automatiskt sätt toochange hello specifikt för tillståndslösa tjänster. Detta gör att hello service tooscale dynamiskt med hello antalet noder som är tillgängliga. hello sätt tooopt i det här beteendet är tooset hello instanser = -1. InstanceCount = -1 är en instruktion tooService Fabric som säger ”kör tillståndslösa tjänsten på varje nod”. Om hello antalet noder ändras, ändras Service Fabric automatiskt hello instans antal toomatch säkerställer att hello-tjänsten körs på alla noder som giltig. 

C#:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Skalning genom att skapa eller ta bort nya heter tjänster
En namngiven tjänstinstans är en specifik instans av en typ (finns [livscykel för Service Fabric application](service-fabric-application-lifecycle.md)) inom vissa namngivna programinstansen i hello kluster. 

Nya instanser av tjänsten kan skapas (eller tas bort) som tjänster blir mer eller mindre upptagen. Detta gör att begäranden toobe fördelade på flera instanser av tjänsten, vanligtvis så att belastningen på befintliga tjänster toodecrease. När du skapar tjänster, placerar hello resurshanteraren för Service Fabric-klustret hello tjänster i hello kluster i en distribuerad sätt. hello exakt beslut regleras av hello [mått](service-fabric-cluster-resource-manager-metrics.md) i hello klustret och andra regler för placering. Tjänster kan skapas på flera olika sätt, men hello vanligaste är antingen genom att utföra administrativa åtgärder som någon anropar [ `New-ServiceFabricService` ](https://docs.microsoft.com/en-us/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), eller genom att koden anropar [ `CreateServiceAsync` ](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync`kan även anropas från inom andra tjänster som körs i hello kluster.

Skapa tjänster dynamiskt kan användas i alla typer av scenarier och är ett vanligt mönster. Anta till exempel att en tillståndskänslig tjänst som representerar ett visst arbetsflöde. Anrop som representerar resurser ska tooshow toothis tjänsten och den här tjänsten är pågående tooexecute hello steg toothat arbetsflödet och registrera pågår. 

Hur vill du göra viss tjänst skalans? hello service kan vara flera innehavare i någon form och acceptera anrop och startar stegen för många olika instanser av hello samma arbetsflöde på en gång. Dock som kan göra hello kod mer komplexa, eftersom nu den har tooworry om många olika instanser av hello samma arbetsflöde alla vid olika faser och från olika kunder. Dessutom hantering av flera olika arbetsflöden på hello samtidigt inte löser hello skala problem. Det beror på att någon gång den här tjänsten kommer att använda den för många resurser toofit på en viss dator. Många tjänster inte byggda för det här mönstret hello första plats kan även uppstå problem på grund av toosome inbyggd flaskhals eller långsammare i koden. Dessa typer av frågor orsaka hello-tjänsten inte toowork samt när hello antalet samtidiga arbetsflöden som den för att spåra hämtar större.  

En lösning är toocreate en instans av tjänsten för alla olika instanser av hello arbetsflöde du vill tootrack. Detta är ett bra mönster och fungerar om hello-tjänsten är tillståndslösa och tillståndskänsliga. För det här mönstret toowork finns vanligtvis en annan tjänst som fungerar som en ”Workload Manager-tjänst”. hello jobb för den här tjänsten är tooreceive begäranden och tooroute dessa begäranden tooother tjänster. hello manager kan skapa en instans av hello arbetsbelastning tjänsten dynamiskt när den tar emot hello-meddelande, och sedan på begäranden toothose tjänster. hello manager-tjänsten kan också få återanrop när en tjänst för angivet arbetsflödesinstans har slutförts sitt jobb. När hello manager tar emot dessa återanrop kan det ta bort instansen av hello arbetsflödestjänsten eller lämna den om fler anrop förväntades. 

Avancerade versioner av den här typen av manager kan även skapa pooler hello-tjänster som den hanterar. hello poolen bidrar till att den inte har toowait för hello service toospin när en ny begäran kommer. I stället kan hello manager bara välja en arbetsflödestjänst som inte är för tillfället upptagen från hello pool eller vidarebefordra slumpmässigt. Att hålla en pool med tillgängliga tjänster gör hantering nya begäranden snabbare, eftersom det är mindre troligt att hello-begäran har toowait för en ny service toobe efter en redundansväxling. Skapa nya tjänster är snabb, men inte ledigt eller omedelbar. hello pool kan minimera hello tid hello begäran har toowait innan genomgår underhåll. Det här mönstret manager och pool visas ofta när svarstider viktigaste hello. Meddelandeköer hello-begäran och skapar hello-tjänsten i hello bakgrund och _sedan_ skicka vidare är också ett populärt manager mönster som skapar och tar bort tjänster baserat på vissa spårningstjänst hello mängd arbete som för närvarande har väntande. 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Skalning genom att skapa eller ta bort nya namngivna instanser av programmet
Skapa och ta bort hela programmet instanser är liknande toohello mönster för att skapa och ta bort tjänster. Det finns vissa manager-tjänsten som beslutet hello baserat på hello-begäranden som den ser för det här mönstret och hello information som den tar emot från hello andra tjänster i hello kluster. 

När ska skapa en ny programinstans av namngivna användas istället för att skapa en ny namngiven tjänstinstanser i ett redan existerande program? Det finns några fall:

  * hello nya programinstansen är för en kund vars kod måste toorun under en viss identitet eller säkerhetsinställningar.
    * Service Fabric kan du definiera olika kod paket toorun under viss identiteter. I ordning toolaunch hello samma kodpaketet under olika identiteter, hello aktiveringar måste toooccur i olika programinstanser. Överväg att fall där du har en befintlig kunds arbetsbelastningar som har distribuerats. Dessa kan köras under en viss identitet så att du kan övervaka och kontrollera deras åtkomst tooother resurser, till exempel remote databaser eller andra system. I det här fallet när en ny kund loggar, vill du förmodligen inte tooactivate koden i hello samma kontext (processen diskutrymme). När du gick gör det det svårare för din tjänst kod tooact inom hello kontexten för en viss identitet. Du måste vanligtvis ha mer säkerhet, isolering och identity management-kod. Istället för att använda olika med namnet service instanser i samma instans av programmet hello och därför hello samma processutrymme kan du använda olika namngivna instanser i Fabric-tjänstprogrammet. Detta gör det enklare toodefine annan identitet sammanhang.
  * hello nya programinstansen fungerar också som ett sätt att konfiguration
    * Som standard körs alla hello namngivna instanser av tjänsten av en viss typ i en instans av programmet i samma process på en viss nod hello. Det innebär att när du kan konfigurera varje tjänstinstans på olika sätt, göra så är komplicerad. Tjänster måste ha en token som de använder toolook in sina config inom ett konfigurationspaket. Detta är vanligtvis bara hello tjänstens namn. Detta fungerar bra, men det kombinerar hello toohello konfigurationsnamn hello enskilda tjänsten instanser inom den programinstansen. Detta kan vara förvirrande hårda toomanage eftersom konfigurationen är normalt en design tid artefakt med specifika värden för program-instans. När du skapar fler tjänster alltid innebär mer programuppgraderingar toochange hello information inom hello config paket eller toodeploy nya så att hello nya tjänster kan slå upp sina specifik information. Det är ofta enklare toocreate en helt ny namngiven programinstansen. Du kan använda hello programmet parametrar tooset oavsett konfiguration krävs för hello-tjänster. Det här sättet alla hello-tjänster som är äldre än som heter programinstansen kan ärva inställningar för konfiguration. Till exempel i stället för att en enda konfigurationsfil med hello inställningar och anpassningar för varje kund, till exempel hemligheter eller per gränserna för kunden, måste du i stället en annan programinstansen för varje kund med dessa inställningar åsidosätts. 
  * hello nya programmet fungerar som en gräns för uppgradering
    * Olika namngivna programinstanser fungera som gränser för uppgradering i Service Fabric. En uppgradering av en namngiven programinstansen påverkar inte hello-kod som en annan programinstans av namngivna körs. hello olika program kommer att få kör olika versioner av samma code hello på hello samma noder. Detta kan vara en faktor när du behöver toomake skalning beslut som du kan välja om hello ny kod bör följa hello samma uppgraderingar som en annan tjänst eller inte. Anta till exempel att ett anrop når hello manager-tjänsten som ansvarar för att skala en viss kund arbetsbelastningar genom att skapa och ta bort tjänster dynamiskt. I det här fallet hello anrop är dock för en arbetsbelastning som är associerade med en _nya_ kunden. De flesta kunder vill som de isoleras från varandra inte bara för hello säkerhet och konfiguration orsaker som angavs tidigare, men eftersom det ger mer flexibla kör vissa versioner av hello programvara och välja när de uppgraderas. Du kan också skapa en ny instans av programmet och skapa hello tjänst det bara toofurther partition hello mängden dina tjänster som berör en uppgraderar. Separata programinstanser ger större precision när du utför programuppgraderingar och även aktivera A / B-testning och blå/grön distributioner. 
  * hello befintliga programinstansen är full
    * I Service Fabric [programmet kapacitet](service-fabric-cluster-resource-manager-application-groups.md) är ett begrepp som du kan använda toocontrol hello mängden tillgängliga resurser för viss programinstanser. Du kan till exempel bestämma att en viss tjänst måste toohave på en annan instans som skapats i ordning tooscale. Den här programinstansen är dock utanför kapacitet för ett viss mått. Om den här viss kund eller arbetsbelastning bör fortfarande beviljas mer resurser, kan sedan du antingen öka hello befintliga kapaciteten för programmet eller skapa ett nytt program. 

## <a name="scaling-at-hello-partition-level"></a>Skalning på hello partition nivå
Service Fabric stöder partitionering. Partitionering delar en tjänst i flera logiska och fysiska avsnitt, där varje fungerar oberoende av varandra. Detta är användbart med tillståndskänsliga tjänster, eftersom ingen har angetts av repliker har toohandle alla hello-anrop och ändra alla hello tillstånd på samma gång. Hej [partitionering översikt](service-fabric-concepts-partitioning.md) innehåller information om hello typer av partitionering scheman som stöds. hello repliker för varje partition sprids ut bland hello noder i ett kluster, distribuerar tjänstens belastning och se till att varken hello service hela eller en partition har en enskild felpunkt. 

Överväg att en tjänst som använder en ranged partitioneringsschema med en låg nyckel 0, en hög nyckel 99 och ett partitionsantal på 4. I ett kluster med tre noder kan hello service ta med fyra repliker som delar hello resurser på varje nod som visas här:

<center>
![Partitionslayout med tre noder](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Om du ökar hello antalet noder, ska Service Fabric flytta en del av hello befintliga replikeringar det. Anta att till exempel hello antalet noder ökar toofour och hello repliker hämta omdistribueras. Nu hello-tjänsten har nu tre repliker som körs på varje nod, var och en tillhörande toodifferent partitioner. Detta gör att bättre resursutnyttjande eftersom hello ny nod inte kalla. Normalt även förbättras prestanda som har mer resurser tillgängliga tooit för varje tjänst.

<center>
![Partitionslayout med fyra noder](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-hello-service-fabric-cluster-resource-manager-and-metrics"></a>Skalning med hjälp av hello resurshanteraren för Service Fabric-kluster och mått
[Mått](service-fabric-cluster-resource-manager-metrics.md) är hur tjänster express sina resurs förbrukning tooService Fabric. Med hjälp av mätvärden och ger hello klustret Resource Manager en möjlighet tooreorganize och optimera hello layout hello-klustret. T.ex, det kan finnas tillräckligt med resurser i hello kluster, men de kan inte tilldelas toohello tjänster som för närvarande gör arbete. Med hjälp av mått kan hello klustret Resource Manager tooreorganize hello klustret tooensure behörighet services toohello tillgängliga resurser. 


## <a name="scaling-by-adding-and-removing-nodes-from-hello-cluster"></a>Skalning genom att lägga till och ta bort noder från hello kluster 
Ett annat alternativ för skalning med Service Fabric är toochange hello storleken på hello kluster. Ändra hello storlek på hello klustret innebär att lägga till eller ta bort noder för en eller flera hello nodtyper i hello kluster. Tänk dig ett fall där alla hello noder i klustret hello är aktivt. Det innebär att hello klusterresurserna är nästan alla används. I det här fallet är att lägga till fler noder toohello klustret hello bästa sätt tooscale. När nya noder för hello ansluta hello klustret hello flyttar resurshanteraren för Service Fabric-kluster services toothem vilket resulterar i mindre totala belastningen på befintliga hello-noder. För tillståndslösa tjänster med instansantal =-1, mer service instanser skapas automatiskt. Detta gör att vissa anrop toomove från hello befintliga noder toohello nya noder. 

Lägga till och ta bort noder toohello kan klustret utföras via hello Service Fabric Azure Resource Manager PowerShell-modulen.

```posh
Add-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName  -NumberOfNodesToAdd 5 
Remove-AzureRmServiceFabricNode -ResourceGroupName $resourceGroupName -Name $clusterResourceName -NodeType $nodeTypeName -NumberOfNodesToRemove 5
```

## <a name="putting-it-all-together"></a>Alltihop
Låt oss ta alla hello idéer som vi nämnt här och prata via ett exempel. Överväg följande service hello: du försöker toobuild en tjänst som fungerar som en adressbok, hålla på toonames-och kontaktinformation. 

Höger direkt du har flera frågor relaterade tooscale: hur många användare kan du gå toohave? Hur många kontakter lagrar varje användare? När du Ständiga installera tjänsten för hello första gången är svårt att försöker toofigure alla ut. Anta att du tänkte toogo med en enda statisk tjänst med en specifik partition count. hello konsekvenserna av plocka hello fel partitionsantal kan orsaka toohave skala problem senare. På liknande sätt, även om du väljer hello rätt antal som du kanske inte alla hello information du behöver. Till exempel har du också toodecide hello storleken på hello kluster direkt, både vad gäller hello antalet noder och deras storlek. Vanligtvis hårddisken toopredict hur många resurser som en tjänst kommer tooconsume under sin livslängd. Det kan också vara hårda tooknow i tid hello trafik mönster som faktiskt ser hello-tjänsten. Exempelvis kanske personer lägga till och ta bort deras kontakter endast först öppna hello morgonen eller kanske det jämnt över hello loppet av hello dag. Utifrån detta måste du kanske tooscale ut och in dynamiskt. Kanske kan du läsa toopredict när du ska tooneed tooscale ut och in, men oavsett hur du kommer tooneed tooreact toochanging resurser som används av tjänsten. Detta kan omfatta ändra hello storlek på hello-kluster i ordning tooprovide mer resurser när organisera om användning av befintliga resurser inte är tillräckligt. 

Men även försök varför toopick ett enda partitionsschema ut för alla användare? Varför begränsa själv tooone service och en statisk kluster? hello verkliga situationen är vanligtvis mer dynamiskt. 

När du skapar för att skala, Överväg hello följande dynamiska mönster. Du kan behöva tooadapt den tooyour situation:

1. Skapa en ”manager-tjänst” i stället för försök toopick en partitioneringsschema för alla direkt.
2. hello jobb av hello manager-tjänsten är toolook på kundinformation när de registrerar sig för din tjänst. Beroende på den informationen hello manager-tjänsten för att skapa en instans av din _faktiska_ kontakta lagringstjänsten _för kunden_. Om de kräver specifik konfiguration, isolering eller uppgraderingar, kan du också bestämma toospin in en instans av programmet för kunden. 

Den här skapas dynamiskt mönstret många fördelar:

  - Du försöker inte tooguess hello rätt partitionsantal för alla användare direkt eller skapa en enskild tjänst som är oändligt skalbara på sin egen. 
  - Olika användare inte har toohave hello samma partitions antal replikantalet, placeringen, mått, standard belastning, tjänstnamn, dns-inställningarna eller någon av hello andra egenskaper anges på hello service- eller programnivå. 
  - Du får ytterligare data segmentering. Varje kund har sina egna kopia av hello-tjänsten
    - Varje kund-tjänsten kan konfigureras på olika sätt och beviljas fler eller färre resurser med fler eller färre partitioner eller repliker efter behov baserat på deras förväntade skala.
      - Anta till exempel hello kunden betalat för hello ”guld” nivå - kunde vara mer replikeringar eller större partitionsantal och potentiellt resurser dedikerade tootheir tjänster via mått och programmet kapacitet.
      - Eller Säg de angivna information som visar hello antalet kontakter de behövs var ”liten” - de kommer bara några partitioner eller kan även användas i en pool för delade tjänster med andra kunder.
  - Du kör inte flera instanser av tjänsten eller repliker medan du väntar kunder tooshow in
  - Om en kund någonsin lämnar är sedan ta bort deras information från din tjänst så enkelt som att ha hello manager ta bort den tjänsten eller programmet som skapats.

## <a name="next-steps"></a>Nästa steg
Mer information om Service Fabric-begrepp finns hello följande artiklar:

* [Tillgänglighet för Service Fabric-tjänster](service-fabric-availability-services.md)
* [Partitionering Service Fabric-tjänster](service-fabric-concepts-partitioning.md)
