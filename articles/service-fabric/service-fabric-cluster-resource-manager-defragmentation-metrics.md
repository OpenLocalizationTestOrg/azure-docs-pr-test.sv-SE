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
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a><span data-ttu-id="96efc-103">Defragmentering av mätvärden och belastningen i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="96efc-103">Defragmentation of metrics and load in Service Fabric</span></span>
<span data-ttu-id="96efc-104">hello Service Fabric klustret Resource Manager-standard strategi för att hantera belastningen mått i hello kluster är toodistribute hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="96efc-104">hello Service Fabric Cluster Resource Manager's default strategy for managing load metrics in hello cluster is toodistribute hello load.</span></span> <span data-ttu-id="96efc-105">Se till att noderna är jämnt utnyttjade undviker varm eller kall punkter som leda tooboth konkurrens och oanvänt resurser.</span><span class="sxs-lookup"><span data-stu-id="96efc-105">Ensuring that nodes are evenly utilized avoids hot and cold spots that lead tooboth contention and wasted resources.</span></span> <span data-ttu-id="96efc-106">Distribution av arbetsbelastningar i hello klustret är också hello säkraste vad gäller kvarvarande fel eftersom det garanterar att fel inte ta ut en stor del av en viss arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="96efc-106">Distributing workloads in hello cluster is also hello safest in terms of surviving failures since it ensures that a failure doesn’t take out a large percentage of a given workload.</span></span> 

<span data-ttu-id="96efc-107">hello resurshanteraren för Service Fabric-klustret har stöd för en annan strategi för att hantera belastningen som är defragmentering.</span><span class="sxs-lookup"><span data-stu-id="96efc-107">hello Service Fabric Cluster Resource Manager does support a different strategy for managing load, which is defragmentation.</span></span> <span data-ttu-id="96efc-108">Defragmentering innebär att i stället för försök toodistribute hello användning av ett mått över hello kluster den konsolideras.</span><span class="sxs-lookup"><span data-stu-id="96efc-108">Defragmentation means that instead of trying toodistribute hello utilization of a metric across hello cluster, it is consolidated.</span></span> <span data-ttu-id="96efc-109">Konsolidering är bara en inverteras hello standard strategi – i stället för att minimera hello genomsnittlig standardavvikelsen för mått load balancing, hello klustret Resource Manager försöker tooincrease den.</span><span class="sxs-lookup"><span data-stu-id="96efc-109">Consolidation is just an inversion of hello default balancing strategy – instead of minimizing hello average standard deviation of metric load, hello Cluster Resource Manager tries tooincrease it.</span></span>

## <a name="when-toouse-defragmentation"></a><span data-ttu-id="96efc-110">När toouse defragmentering</span><span class="sxs-lookup"><span data-stu-id="96efc-110">When toouse defragmentation</span></span>
<span data-ttu-id="96efc-111">Distribuera belastning i hello kluster förbrukar hello resurser på varje nod.</span><span class="sxs-lookup"><span data-stu-id="96efc-111">Distributing load in hello cluster consumes some of hello resources on each node.</span></span> <span data-ttu-id="96efc-112">Vissa arbetsbelastningar skapa tjänster som är ovanligt stort och använda de flesta eller alla av en nod.</span><span class="sxs-lookup"><span data-stu-id="96efc-112">Some workloads create services that are exceptionally large and consume most or all of a node.</span></span> <span data-ttu-id="96efc-113">I dessa fall är det möjligt att när det finns stora arbetsbelastningar komma skapas som det finns inte tillräckligt med utrymme på någon nod toorun dem.</span><span class="sxs-lookup"><span data-stu-id="96efc-113">In these cases, it's possible that when there are large workloads getting created that there isn't enough space on any node toorun them.</span></span> <span data-ttu-id="96efc-114">Stora arbetsbelastningar är inte ett problem i Service Fabric; i dessa fall hello fastställer klustret Resource Manager att den behöver tooreorganize hello klustret toomake utrymme för stora arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="96efc-114">Large workloads aren't a problem in Service Fabric; in these cases hello Cluster Resource Manager determines that it needs tooreorganize hello cluster toomake room for this large workload.</span></span> <span data-ttu-id="96efc-115">Men i hello har tiden att arbetsbelastningar toowait toobe schemalagda i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96efc-115">However, in hello meantime that workload has toowait toobe scheduled in hello cluster.</span></span>

<span data-ttu-id="96efc-116">Om det finns många tjänster och tillstånd toomove runt kan ta det lång tid för hello stora arbetsbelastning toobe placeras i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96efc-116">If there are many services and state toomove around, then it could take a long time for hello large workload toobe placed in hello cluster.</span></span> <span data-ttu-id="96efc-117">Detta är mer troligt om andra arbetsbelastningar i hello kluster är också stora och så ta längre tooreorganize.</span><span class="sxs-lookup"><span data-stu-id="96efc-117">This is more likely if other workloads in hello cluster are also large and so take longer tooreorganize.</span></span> <span data-ttu-id="96efc-118">hello Service Fabric-teamet mätt skapa gånger under simulering av det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="96efc-118">hello Service Fabric team measured creation times in simulations of this scenario.</span></span> <span data-ttu-id="96efc-119">Påträffades att skapa stora services tog mycket längre så snart klusteranvändning fick ovan mellan 30 och 50%.</span><span class="sxs-lookup"><span data-stu-id="96efc-119">We found that creating large services took much longer as soon as cluster utilization got above between 30% and 50%.</span></span> <span data-ttu-id="96efc-120">toohandle i det här scenariot vi har fört defragmentering som en strategi för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="96efc-120">toohandle this scenario, we introduced defragmentation as a balancing strategy.</span></span> <span data-ttu-id="96efc-121">Påträffades att för stora arbetsbelastningar, särskilt de som där skapelsetid var viktigt defragmentering verkligen hjälpt dessa nya arbetsbelastningar få schemalagda i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96efc-121">We found that for large workloads, especially ones where creation time was important, defragmentation really helped those new workloads get scheduled in hello cluster.</span></span>

<span data-ttu-id="96efc-122">Du kan konfigurera defragmentering mått toohave hello klustret Resource Manager tooproactively försök toocondense hello belastningen på hello tjänster i färre noder.</span><span class="sxs-lookup"><span data-stu-id="96efc-122">You can configure defragmentation metrics toohave hello Cluster Resource Manager tooproactively try toocondense hello load of hello services into fewer nodes.</span></span> <span data-ttu-id="96efc-123">Detta säkerställer att det finns nästan alltid plats för stora tjänster utan att organisera hello klustret.</span><span class="sxs-lookup"><span data-stu-id="96efc-123">This helps ensure that there is almost always room for large services without reorganizing hello cluster.</span></span> <span data-ttu-id="96efc-124">Inte har tooreorganize hello klustret kan du snabbt skapa stora arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="96efc-124">Not having tooreorganize hello cluster allows creating large workloads quickly.</span></span>

<span data-ttu-id="96efc-125">De flesta användare behöver inte defragmentering.</span><span class="sxs-lookup"><span data-stu-id="96efc-125">Most people don’t need defragmentation.</span></span> <span data-ttu-id="96efc-126">Tjänster är vanligtvis vara liten, så det inte är hårda toofind utrymme för dem i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96efc-126">Services are usually be small, so it’s not hard toofind room for them in hello cluster.</span></span> <span data-ttu-id="96efc-127">När det är möjligt om, går snabbt igen eftersom de flesta tjänster är små och kan flyttas snabbt och parallellt.</span><span class="sxs-lookup"><span data-stu-id="96efc-127">When reorganization is possible, it goes quickly, again because most services are small and can be moved quickly and in parallel.</span></span> <span data-ttu-id="96efc-128">Om du har stora tjänster och behöver dem. skapas snabbt sedan hello är defragmentering strategi för dig.</span><span class="sxs-lookup"><span data-stu-id="96efc-128">However, if you have large services and need them created quickly then hello defragmentation strategy is for you.</span></span> <span data-ttu-id="96efc-129">Diskuterar vi hello fördelar med defragmentering nästa.</span><span class="sxs-lookup"><span data-stu-id="96efc-129">We'll discuss hello tradeoffs of using defragmentation next.</span></span> 

## <a name="defragmentation-tradeoffs"></a><span data-ttu-id="96efc-130">Defragmentering kompromisser</span><span class="sxs-lookup"><span data-stu-id="96efc-130">Defragmentation tradeoffs</span></span>
<span data-ttu-id="96efc-131">Defragmentering kan öka impactfulness fel, eftersom flera tjänster som körs på noder som misslyckas.</span><span class="sxs-lookup"><span data-stu-id="96efc-131">Defragmentation can increase impactfulness of failures, since more services are running on nodes that fail.</span></span> <span data-ttu-id="96efc-132">Defragmentering kan också öka kostnader, eftersom resurser i hello klustret måste hållas i reserv, väntar hello skapandet av stora arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="96efc-132">Defragmentation can also increase costs, since resources in hello cluster must be held in reserve, waiting for hello creation of large workloads.</span></span>

<span data-ttu-id="96efc-133">hello ger följande diagram en bild av två kluster, ett som är defragmenteras och ett som inte är.</span><span class="sxs-lookup"><span data-stu-id="96efc-133">hello following diagram gives a visual representation of two clusters, one that is defragmented and one that is not.</span></span> 

<span data-ttu-id="96efc-134"><center>
![Jämföra belastningsutjämnade och defragmenteras kluster][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="96efc-134"><center>
![Comparing Balanced and Defragmented Clusters][Image1]
</center></span></span>

<span data-ttu-id="96efc-135">Överväg att hello antalet förflyttningar som skulle vara nödvändiga tooplace ett hello största serviceobjekt i hello belastningsutjämnade fallet.</span><span class="sxs-lookup"><span data-stu-id="96efc-135">In hello balanced case, consider hello number of movements that would be necessary tooplace one of hello largest service objects.</span></span> <span data-ttu-id="96efc-136">I hello defragmenteras kluster, kan hello stora arbetsbelastning placeras på noder fyra eller fem utan toowait för andra tjänster-toomove.</span><span class="sxs-lookup"><span data-stu-id="96efc-136">In hello defragmented cluster, hello large workload could be placed on nodes four or five without having toowait for any other services toomove.</span></span>

## <a name="defragmentation-pros-and-cons"></a><span data-ttu-id="96efc-137">Defragmentering- och nackdelar</span><span class="sxs-lookup"><span data-stu-id="96efc-137">Defragmentation pros and cons</span></span>
<span data-ttu-id="96efc-138">Vad är så de konceptuella kompromisser?</span><span class="sxs-lookup"><span data-stu-id="96efc-138">So what are those other conceptual tradeoffs?</span></span> <span data-ttu-id="96efc-139">Här är en snabb tabell med saker toothink om:</span><span class="sxs-lookup"><span data-stu-id="96efc-139">Here’s a quick table of things toothink about:</span></span>

| <span data-ttu-id="96efc-140">Defragmentering tekniker</span><span class="sxs-lookup"><span data-stu-id="96efc-140">Defragmentation Pros</span></span> | <span data-ttu-id="96efc-141">Defragmentering nackdelar</span><span class="sxs-lookup"><span data-stu-id="96efc-141">Defragmentation Cons</span></span> |
| --- | --- |
| <span data-ttu-id="96efc-142">Tillåter snabbare skapandet av stora tjänster</span><span class="sxs-lookup"><span data-stu-id="96efc-142">Allows faster creation of large services</span></span> |<span data-ttu-id="96efc-143">Koncentrat laddas till färre noder, öka konkurrens</span><span class="sxs-lookup"><span data-stu-id="96efc-143">Concentrates load onto fewer nodes, increasing contention</span></span> |
| <span data-ttu-id="96efc-144">Aktiverar lägre dataflyttning under skapande av</span><span class="sxs-lookup"><span data-stu-id="96efc-144">Enables lower data movement during creation</span></span> |<span data-ttu-id="96efc-145">Fel kan påverka flera tjänster och orsaka mer omsättning</span><span class="sxs-lookup"><span data-stu-id="96efc-145">Failures can impact more services and cause more churn</span></span> |
| <span data-ttu-id="96efc-146">Tillåter omfattande beskrivning av kraven och frigöring av utrymme</span><span class="sxs-lookup"><span data-stu-id="96efc-146">Allows rich description of requirements and reclamation of space</span></span> |<span data-ttu-id="96efc-147">Mer komplexa övergripande resurshantering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="96efc-147">More complex overall Resource Management configuration</span></span> |

<span data-ttu-id="96efc-148">Du kan blanda defragmenteras och normal mått i hello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="96efc-148">You can mix defragmented and normal metrics in hello same cluster.</span></span> <span data-ttu-id="96efc-149">hello klustret Resource Manager försöker tooconsolidate hello defragmentering mått så mycket som möjligt och samtidigt sprider ut hello andra.</span><span class="sxs-lookup"><span data-stu-id="96efc-149">hello Cluster Resource Manager tries tooconsolidate hello defragmentation metrics as much as possible while spreading out hello others.</span></span> <span data-ttu-id="96efc-150">hello resultaten av blanda defragmentering och belastningsutjämning strategier beror på flera faktorer, inklusive:</span><span class="sxs-lookup"><span data-stu-id="96efc-150">hello results of mixing defragmentation and balancing strategies depends on several factors, including:</span></span>
  - <span data-ttu-id="96efc-151">hello antalet NLB mått kontra hello antalet defragmentering mått</span><span class="sxs-lookup"><span data-stu-id="96efc-151">hello number of balancing metrics vs. hello number of defragmentation metrics</span></span>
  - <span data-ttu-id="96efc-152">Om alla tjänster som använder båda typerna av mått</span><span class="sxs-lookup"><span data-stu-id="96efc-152">Whether any service uses both types of metrics</span></span> 
  - <span data-ttu-id="96efc-153">hello mått vikterna</span><span class="sxs-lookup"><span data-stu-id="96efc-153">hello metric weights</span></span>
  - <span data-ttu-id="96efc-154">aktuella måttet läses in</span><span class="sxs-lookup"><span data-stu-id="96efc-154">current metric loads</span></span>
  
<span data-ttu-id="96efc-155">Experiment är obligatoriska toodetermine hello exakt konfiguration behövs.</span><span class="sxs-lookup"><span data-stu-id="96efc-155">Experimentation is required toodetermine hello exact configuration necessary.</span></span> <span data-ttu-id="96efc-156">Vi rekommenderar noggrann mätning av dina arbetsbelastningar innan du aktiverar defragmentering mått i produktion.</span><span class="sxs-lookup"><span data-stu-id="96efc-156">We recommend thorough measurement of your workloads before you enable defragmentation metrics in production.</span></span> <span data-ttu-id="96efc-157">Detta gäller särskilt när blanda defragmentering och belastningsutjämnade mått i hello samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="96efc-157">This is especially true when mixing defragmentation and balanced metrics within hello same service.</span></span> 

## <a name="configuring-defragmentation-metrics"></a><span data-ttu-id="96efc-158">Konfigurera defragmentering mått</span><span class="sxs-lookup"><span data-stu-id="96efc-158">Configuring defragmentation metrics</span></span>
<span data-ttu-id="96efc-159">Konfigurera defragmentering mått är en global beslut i hello kluster och enskilda mått kan väljas för defragmentering.</span><span class="sxs-lookup"><span data-stu-id="96efc-159">Configuring defragmentation metrics is a global decision in hello cluster, and individual metrics can be selected for defragmentation.</span></span> <span data-ttu-id="96efc-160">hello följande kodavsnitt config visar hur tooconfigure mätvärden för defragmenteringen.</span><span class="sxs-lookup"><span data-stu-id="96efc-160">hello following config snippets show how tooconfigure metrics for defragmentation.</span></span> <span data-ttu-id="96efc-161">I det här fallet är ”Metric1” konfigurerad som en defragmentering mått ”Metric2” kommer att fortsätta toobe belastningsutjämnade normalt.</span><span class="sxs-lookup"><span data-stu-id="96efc-161">In this case, "Metric1" is configured as a defragmentation metric, while "Metric2" will continue toobe balanced normally.</span></span> 

<span data-ttu-id="96efc-162">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="96efc-162">ClusterManifest.xml:</span></span>

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

<span data-ttu-id="96efc-163">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="96efc-163">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="96efc-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96efc-164">Next steps</span></span>
- <span data-ttu-id="96efc-165">hello klustret Resource Manager har man alternativ för att beskriva hello klustret.</span><span class="sxs-lookup"><span data-stu-id="96efc-165">hello Cluster Resource Manager has man options for describing hello cluster.</span></span> <span data-ttu-id="96efc-166">toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="96efc-166">toofind out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>
- <span data-ttu-id="96efc-167">Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="96efc-167">Metrics are how hello Service Fabric Cluster Resource Manger manages consumption and capacity in hello cluster.</span></span> <span data-ttu-id="96efc-168">Mer om mått toolearn och hur tooconfigure dem, kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="96efc-168">toolearn more about metrics and how tooconfigure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
