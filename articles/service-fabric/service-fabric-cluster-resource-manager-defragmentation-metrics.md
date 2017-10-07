---
title: "aaaDefragmentation av mätvärden i Azure Service Fabric | Microsoft Docs"
description: "En översikt över använder defragmentering eller packa som en strategi för mått i Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d09045a6cf196d2771f1a0794637f4579d3eb96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentering av mätvärden och belastningen i Service Fabric
hello Service Fabric klustret Resource Manager-standard strategi för att hantera belastningen mått i hello kluster är toodistribute hello belastningen. Se till att noderna är jämnt utnyttjade undviker varm eller kall punkter som leda tooboth konkurrens och oanvänt resurser. Distribution av arbetsbelastningar i hello klustret är också hello säkraste vad gäller kvarvarande fel eftersom det garanterar att fel inte ta ut en stor del av en viss arbetsbelastning. 

hello resurshanteraren för Service Fabric-klustret har stöd för en annan strategi för att hantera belastningen som är defragmentering. Defragmentering innebär att i stället för försök toodistribute hello användning av ett mått över hello kluster den konsolideras. Konsolidering är bara en inverteras hello standard strategi – i stället för att minimera hello genomsnittlig standardavvikelsen för mått load balancing, hello klustret Resource Manager försöker tooincrease den.

## <a name="when-toouse-defragmentation"></a>När toouse defragmentering
Distribuera belastning i hello kluster förbrukar hello resurser på varje nod. Vissa arbetsbelastningar skapa tjänster som är ovanligt stort och använda de flesta eller alla av en nod. I dessa fall är det möjligt att när det finns stora arbetsbelastningar komma skapas som det finns inte tillräckligt med utrymme på någon nod toorun dem. Stora arbetsbelastningar är inte ett problem i Service Fabric; i dessa fall hello fastställer klustret Resource Manager att den behöver tooreorganize hello klustret toomake utrymme för stora arbetsbelastningen. Men i hello har tiden att arbetsbelastningar toowait toobe schemalagda i hello kluster.

Om det finns många tjänster och tillstånd toomove runt kan ta det lång tid för hello stora arbetsbelastning toobe placeras i hello kluster. Detta är mer troligt om andra arbetsbelastningar i hello kluster är också stora och så ta längre tooreorganize. hello Service Fabric-teamet mätt skapa gånger under simulering av det här scenariot. Påträffades att skapa stora services tog mycket längre så snart klusteranvändning fick ovan mellan 30 och 50%. toohandle i det här scenariot vi har fört defragmentering som en strategi för belastningsutjämning. Påträffades att för stora arbetsbelastningar, särskilt de som där skapelsetid var viktigt defragmentering verkligen hjälpt dessa nya arbetsbelastningar få schemalagda i hello kluster.

Du kan konfigurera defragmentering mått toohave hello klustret Resource Manager tooproactively försök toocondense hello belastningen på hello tjänster i färre noder. Detta säkerställer att det finns nästan alltid plats för stora tjänster utan att organisera hello klustret. Inte har tooreorganize hello klustret kan du snabbt skapa stora arbetsbelastningar.

De flesta användare behöver inte defragmentering. Tjänster är vanligtvis vara liten, så det inte är hårda toofind utrymme för dem i hello kluster. När det är möjligt om, går snabbt igen eftersom de flesta tjänster är små och kan flyttas snabbt och parallellt. Om du har stora tjänster och behöver dem. skapas snabbt sedan hello är defragmentering strategi för dig. Diskuterar vi hello fördelar med defragmentering nästa. 

## <a name="defragmentation-tradeoffs"></a>Defragmentering kompromisser
Defragmentering kan öka impactfulness fel, eftersom flera tjänster som körs på noder som misslyckas. Defragmentering kan också öka kostnader, eftersom resurser i hello klustret måste hållas i reserv, väntar hello skapandet av stora arbetsbelastningar.

hello ger följande diagram en bild av två kluster, ett som är defragmenteras och ett som inte är. 

<center>
![Jämföra belastningsutjämnade och defragmenteras kluster][Image1]
</center>

Överväg att hello antalet förflyttningar som skulle vara nödvändiga tooplace ett hello största serviceobjekt i hello belastningsutjämnade fallet. I hello defragmenteras kluster, kan hello stora arbetsbelastning placeras på noder fyra eller fem utan toowait för andra tjänster-toomove.

## <a name="defragmentation-pros-and-cons"></a>Defragmentering- och nackdelar
Vad är så de konceptuella kompromisser? Här är en snabb tabell med saker toothink om:

| Defragmentering tekniker | Defragmentering nackdelar |
| --- | --- |
| Tillåter snabbare skapandet av stora tjänster |Koncentrat laddas till färre noder, öka konkurrens |
| Aktiverar lägre dataflyttning under skapande av |Fel kan påverka flera tjänster och orsaka mer omsättning |
| Tillåter omfattande beskrivning av kraven och frigöring av utrymme |Mer komplexa övergripande resurshantering-konfiguration |

Du kan blanda defragmenteras och normal mått i hello samma kluster. hello klustret Resource Manager försöker tooconsolidate hello defragmentering mått så mycket som möjligt och samtidigt sprider ut hello andra. hello resultaten av blanda defragmentering och belastningsutjämning strategier beror på flera faktorer, inklusive:
  - hello antalet NLB mått kontra hello antalet defragmentering mått
  - Om alla tjänster som använder båda typerna av mått 
  - hello mått vikterna
  - aktuella måttet läses in
  
Experiment är obligatoriska toodetermine hello exakt konfiguration behövs. Vi rekommenderar noggrann mätning av dina arbetsbelastningar innan du aktiverar defragmentering mått i produktion. Detta gäller särskilt när blanda defragmentering och belastningsutjämnade mått i hello samma tjänst. 

## <a name="configuring-defragmentation-metrics"></a>Konfigurera defragmentering mått
Konfigurera defragmentering mått är en global beslut i hello kluster och enskilda mått kan väljas för defragmentering. hello följande kodavsnitt config visar hur tooconfigure mätvärden för defragmenteringen. I det här fallet är ”Metric1” konfigurerad som en defragmentering mått ”Metric2” kommer att fortsätta toobe belastningsutjämnade normalt. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Nästa steg
- hello klustret Resource Manager har man alternativ för att beskriva hello klustret. toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret. Mer om mått toolearn och hur tooconfigure dem, kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
