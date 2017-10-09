---
title: "aaaService Fabric klustret Resource Manager - tillhörighet | Microsoft Docs"
description: "Översikt över hur du konfigurerar tillhörighet för Service Fabric-tjänster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7dc9b6d9c18d9d615d39cff7de9d7cba1c040474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurera och använda tjänsten tillhörighet i Service Fabric
Tillhörighet är en kontroll som tillhandahålls huvudsakligen toohelp enkel hello övergången av större gigantiska program i molnet och mikrotjänster hälsningsmeddelande. Det används också som en optimering för att förbättra hello prestanda för tjänster, även om detta kan ha sidoeffekter.

Anta att du tillämpar en större app eller en som bara inte utformad med mikrotjänster i åtanke, tooService Fabric (eller distribuerad miljö). Den här typen av övergången är vanligt. Börja med att lyfta hello hela appen till hello miljö, paketera den och gör att den körs utan problem. Du startar sedan dela upp frågan i olika mindre tjänster att alla prata tooeach andra.

Slutligen kan du programmet hello har några problem. hello problem indelas vanligtvis i dessa kategorier:

1. Vissa komponenten X i hello monolitisk app hade ett odokumenterade beroende på komponenten Y och du har aktiverat precis dessa komponenter till separata tjänster. Eftersom dessa tjänster körs nu på olika noder i klustret hello, är de bruten.
2. Dessa komponenter kommunicerar (lokal namngivna pipes | delat minne | filer på disk) och de verkligen behöver toobe kan toowrite tooa delade lokal resurs av prestandaskäl just nu. Hårda sambandet hämtar togs bort senare, kanske.
3. Allt är bra, men det visar sig att dessa två komponenter är faktiskt chatty och prestanda känslig. När de flyttas dem till olika tjänster övergripande programprestanda tanked eller latens ökade. Därför uppfyller hello övergripande inte förväntningar.

I dessa fall kan vi vill inte toolose våra refaktoriseringsaktiviteter arbete och vill inte toogo tillbaka toohello monolitvolym. hello senaste villkor kan även vara önskvärt som en vanlig optimering. Dock vi tills vi bearbeta hello komponenter toowork naturligt som tjänster (eller kan vi lösa hello prestandaförväntningarna något annat sätt) tooneed vissa uppfattning om ort.

Vilka toodo? Dessutom kan du aktivera tillhörighet.

## <a name="how-tooconfigure-affinity"></a>Hur tooconfigure tillhörighet
tooset in tillhörighet du definiera en tillhörigheten mellan två olika tjänster. Du kan tänka dig tillhörighet som ”pekar” en-tjänst på en annan och säger ”den här tjänsten kan endast köras var att tjänsten körs”. Ibland refererar vi tooaffinity som en överordnad-underordnad relation (där du peka hello underordnade på hello överordnade). Det innebär att hello repliker eller instanser av en tjänst är placerade på hello samma noder som en annan tjänst.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> En underordnad tjänst kan bara ingå i en enda tillhörigheten. Om du vill ha hello underordnade toobe tillhör tootwo överordnade tjänster samtidigt har flera olika alternativ:
> - Omvänd hello relationer (har parentService1 och parentService2 som pekar på hello aktuella underordnade tjänster), eller
> - Utse en hello överordnade som ett nav enligt konventionen och alla tjänster som pekar på den tjänsten. 
>
> hello resulterande placeringen i hello kluster bör hello samma.
>

## <a name="different-affinity-options"></a>Alternativ för olika tillhörighet
Tillhörighet representeras via en av flera scheman för korrelation och har två olika lägen. hello vanligaste läget för mappning mellan engelskans NonAlignedAffinity. Hej repliker i NonAlignedAffinity, eller instanser av hello olika tjänster är placerad hello samma noder. hello är andra läge AlignedAffinity. Justerade tillhörighet är användbar bara med tillståndskänsliga tjänster. Konfigurera två stateful services toohave justerad innebär att hello primärfärgerna av tjänsterna är placerade på hello samma noder som varje andra. Det gör även varje par av sekundärservrar för dessa tjänster toobe placerad hello samma noder. Det är också möjligt (dock mindre vanliga) tooconfigure NonAlignedAffinity för tillståndskänsliga tjänster. Hello olika repliker av hello två tillståndskänsliga tjänster körs på hello samma noder för NonAlignedAffinity, men deras primärfärgerna kan hamna på olika noder.

<center>
![Tillhörighet och deras effekter][Image1]
</center>

### <a name="best-effort-desired-state"></a>Bästa prestanda önskad tillstånd
En mappningsrelation är bästa prestanda. Ger inte hello samma garantier för samplacering eller tillförlitlighet som körs i hello samma körbara process. hello-tjänster i ett tillhörigheten är helt olika enheter som kan misslyckas och flyttas oberoende av varandra. En tillhörigheten kan också dela, även om dessa radbrytningar är tillfällig. Till exempel innebär kapacitetsbegränsningar att endast en del av hello tjänstobjekt i hello tillhörigheten får plats på en viss nod. I dessa fall även om det finns en tillhörigheten på plats, den kan inte tillämpas på grund av toohello andra villkor. Om det är möjligt toodo så korrigeras hello överträdelse senare.

### <a name="chains-vs-stars"></a>Kedjor kontra stjärnor
Idag är hello klustret Resource Manager inte kan toomodel kedjor tillhörighet relationer. Det innebär som en tjänst som är underordnat i en tillhörigheten får inte vara en förälder i en annan tillhörigheten. Om du vill toomodel den här typen av relation kan du effektivt har toomodel den som en stjärna i stället för en kedja. toomove från en kedja tooa stjärna, hello lägsta underordnade skulle vara överordnad toohello första underordnade överordnade i stället. Beroende på hello placering av dina tjänster kanske toodo detta flera gånger. Om det finns ingen fysisk överordnade tjänst, kan du ha toocreate som fungerar som platshållare. Beroende på dina krav kan du kanske också vill toolook till [programgrupper](service-fabric-cluster-resource-manager-application-groups.md).

<center>
![Kedjor vs. Stjärnorna hello kontexten tillhörighet relationer][Image2]
</center>

En annan sak toonote om mappning mellan relationer är idag att de är riktat. Det innebär att hello tillhörighet regeln endast tillämpar den hello underordnade placeras med hello överordnad. Det garanterar inte att hello överordnade finns med hello underordnade. Det är också viktigt toonote som hello tillhörigheten inte kan perfekt eller direkt tillämpas eftersom olika tjänster har med olika livscykler kan misslyckas och flytta oberoende av varandra. Anta exempelvis att hello överordnade plötsligt att fungera över tooanother noden eftersom den har kraschat. hello klustret Resource Manager och Failover Manager referensen du hello redundans först, eftersom hello tjänster, konsekvent och tillgängliga är hello prioritet. När hello växling vid fel är klar hello tillhörigheten har brutits, men hello klustret Resource Manager tror att allt är bra tills den meddelanden den hello underordnat finns inte med hello överordnad. Dessa typer av kontroller som utförs med jämna mellanrum. Mer information om hur hello klustret Resource Manager utvärderar villkor finns i [i den här artikeln](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), och [den här](service-fabric-cluster-resource-manager-balancing.md) berättar mer om hur tooconfigure hello takt där dessa villkor är utvärdera.   


### <a name="partitioning-support"></a>Partitionering
hello sista åtgärd toonotice om mappning mellan är att tillhörighet relationer inte stöds där hello överordnade är partitionerad. Partitionerade överordnade tjänster kan stödjas förr eller senare, men idag är det inte tillåtet.

## <a name="next-steps"></a>Nästa steg
- Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)
- toolimit services tooa liten uppsättning datorer eller aggregering hello belastningsutjämning för tjänster, Använd [programgrupper](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png