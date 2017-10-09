---
title: 'Uppgradering av programmet: Uppgraderingsparametrar | Microsoft Docs'
description: "Beskriver parametrar relaterade tooupgrading ett Service Fabric-program, inklusive hälsa kontrollerar tooperform och principer tooautomatically Ångra hello uppgraderingen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: abd0ba48c223be9aa0909c7a0100ba5986430db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-upgrade-parameters"></a>Programuppgraderingsparametrar
Den här artikeln beskriver hello olika parametrar som gäller under hello uppgradera ett Azure Service Fabric-program. hello parametrar inkluderar hello namn och version av programmet hello. De är rattar som styr hello timeout och hälsokontroller som tillämpas under hello uppgraderingen och de anger hello principer som måste tillämpas när uppgraderingen misslyckas.

<br>

| Parameter | Beskrivning |
| --- | --- |
| ApplicationName |Namnet på hello-program som ska uppgraderas. Exempel: fabric: / VisualObjects fabric: / ClusterMonitor |
| TargetApplicationTypeVersion |hello version av hello programtyp som hello uppgradera mål. |
| FailureAction |hello åtgärd som Service Fabric när hello uppgraderingen misslyckas. hello program kan återställas toohello före uppdateringen version (rollback) eller hello uppgraderingen kanske har stoppats på hello aktuell uppgraderingsdomän. Hello Uppgraderingsläge är också ändrade tooManual i hello senare fallet. Tillåtna värden är återställning och manuell. |
| HealthCheckWaitDurationSec |hello tid toowait (i sekunder) efter hello uppgraderingen har slutförts på hello uppgraderingsdomänen innan Service Fabric utvärderar hello hälsotillståndet för programmet hello. Varaktigheten kan också ses som hello tid som ett program ska köras innan den kan betraktas som felfritt. Om hello hälsokontroll klarat, fortsätter hello uppgraderingsprocessen toohello nästa uppgraderingsdomän.  Om hello hälsokontroll misslyckas, väntar Service Fabric på ett intervall (hello UpgradeHealthCheckInterval) innan du försöker hello hälsokontroll igen förrän hello HealthCheckRetryTimeout har uppnåtts. hello standard och rekommenderat värde är 0 sekunder. |
| HealthCheckRetryTimeoutSec |hello varaktighet (i sekunder) som Service Fabric fortsätter tooperform hälsoutvärderingen innan du deklarerar hello uppgraderingen som misslyckades. hello standardvärdet är 600 sekunder. Varaktigheten startar när HealthCheckWaitDuration har nåtts. I den här HealthCheckRetryTimeout kan Service Fabric utföra flera hälsokontroller av hello programmets hälsotillstånd. hello standardvärdet är 10 minuter och bör anpassas på lämpligt sätt för ditt program. |
| HealthCheckStableDurationSec |hello (i sekunder) tooverify som hello program är stabil innan glidande toohello nästa uppgraderingsdomän eller Slutför hello uppgraderingen. Vänta varaktigheten är används tooprevent hittades inte ändringar av hälsa rätt efter hello hälsokontroll utförs. hello standardvärdet är 120 sekunder och bör anpassas på lämpligt sätt för ditt program. |
| UpgradeDomainTimeoutSec |Längsta tid (i sekunder) för att uppgradera en uppgradering domän. Om den här timeout uppnås hello uppgraderingen stoppar och fortsätter baserat på hello inställning för UpgradeFailureAction. hello standardvärdet är aldrig (obegränsat) och bör anpassas på lämpligt sätt för programmet. |
| UpgradeTimeout |En timeout (i sekunder) som gäller för hela hello-uppgradering. Om den här timeout uppnås hello uppgradera stoppar och UpgradeFailureAction utlöses. hello standardvärdet är aldrig (obegränsat) och bör anpassas på lämpligt sätt för programmet. |
| UpgradeHealthCheckInterval |hello frekvens som hello hälsostatus kontrolleras. Den här parametern anges i hello ClusterManager avsnitt i hello *klustret* *manifest*, och inte har angetts som en del av uppgraderingen hello-cmdlet. hello standardvärdet är 60 sekunder. |

<br>

## <a name="service-health-evaluation-during-application-upgrade"></a>Tjänsten hälsoutvärderingen under uppgradering av programmet
<br>
hello hälsa utvärderingskriterierna är valfria. Om hello hälsa utvärderingskriterierna inte anges när en uppgradering startas, används Service Fabric hello programmet hälsoprinciper som anges i hello ApplicationManifest.xml av hello programinstansen.

<br>

| Parameter | Beskrivning |
| --- | --- |
| ConsiderWarningAsError |Standardvärdet är False. Behandla hello hälsa varningshändelser för programmet hello som fel vid utvärdering av hello hälsotillståndet för programmet hello under uppgraderingen. Som standard utvärderas Service Fabric inte varning hälsa händelser toobe fel (fel), så hello uppgraderingen kan fortsätta även om det finns händelser. |
| MaxPercentUnhealthyDeployedApplications |Rekommenderade standardvärdet är 0. Ange hello maximalt antal distribuerade program (se hello [hälsa avsnittet](service-fabric-health-introduction.md)) som kan vara felaktiga innan programmet hello anses vara felaktig och misslyckas hello uppgraderingen. Den här parametern anger hello programmets hälsotillstånd på hello nod och hjälper dig identifiera problem under uppgraderingen. Normalt hämta hello repliker av programmet hello belastningsutjämnade toohello andra nod där hello programmet tooappear felfri, vilket ger hello uppgradera tooproceed. Genom att ange en strikt MaxPercentUnhealthyDeployedApplications health Service Fabric identifiera problem med hello programpaket snabbt och kunna skapa en snabb uppgraderingen misslyckas. |
| MaxPercentUnhealthyServices |Rekommenderade standardvärdet är 0. Ange hello maximalt antal tjänster i hello programinstansen som kan vara felaktiga innan programmet hello anses vara felaktig och misslyckas hello uppgraderingen. |
| MaxPercentUnhealthyPartitionsPerService |Rekommenderade standardvärdet är 0. Ange hello högsta tillåtna antalet partitioner i en tjänst som kan vara felaktiga innan hello service anses vara felaktig. |
| MaxPercentUnhealthyReplicasPerPartition |Rekommenderade standardvärdet är 0. Ange hello maximalt antal repliker i partitionen kan vara felaktiga innan hello partition anses vara felaktig. |
| UpgradeReplicaSetCheckTimeout |**Tillståndslösa tjänsten**--inom en enskild uppgraderingsdomänen Service Fabric försöker tooensure att det finns ytterligare instanser av hello-tjänst. Om hello mål-instanser är mer än en, väntar Service Fabric på mer än en instans toobe tillgängliga in tooa maximala timeout-värdet. Timeout har angetts med hello UpgradeReplicaSetCheckTimeout-egenskapen. Om hello tidsgränsen går ut, fortsätter Service Fabric med uppgraderingen av hello, oavsett hello antalet instanser av tjänsten. Om hello mål-instanser är en, Service Fabric vänta inte och omedelbart fortsätter med uppgraderingen av hello. **Tillståndskänslig service**--inom en enskild uppgraderingsdomänen Service Fabric försöker tooensure som hello replik uppsättningen innehåller ett kvorum. Service Fabric väntar på att ett kvorum toobe tillgängliga in tooa maximala timeout-värdet (anges av egenskapen för hello UpgradeReplicaSetCheckTimeout). Om hello tidsgränsen går ut, fortsätter Service Fabric med hello uppgraderingen, oavsett kvorum. Den här inställningen är uppsättningen som aldrig (oändligt) när du återställer och 900 sekunder när du återställer. |
| ForceRestart |Om du uppdaterar en konfiguration eller datapaketet utan att uppdatera hello-Tjänstkod startas hello-tjänsten bara om hello ForceRestart egenskapen har värdet tootrue. När hello uppdateringen är klar, meddelar Service Fabric hello-tjänsten och att en ny konfigurationspaket eller datapaketet är tillgänglig. hello tjänsten ansvarar för att tillämpa hello ändringar. Om det behövs hello-tjänsten kan starta om sig själv. |

<br>
<br>
Hej MaxPercentUnhealthyServices och MaxPercentUnhealthyPartitionsPerService MaxPercentUnhealthyReplicasPerPartition villkor kan anges per service-typen för en instans av programmet. Dessa parametrar per-tjänst kan för en toocontain olika programtjänster typer med principer för olika utvärdering. En typ av tillståndslösa gateway kan till exempel ha en MaxPercentUnhealthyPartitionsPerService som skiljer sig från en tillståndskänslig engine service-typen för en programinstans.

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

[Uppgradera ditt program med hjälp av Service Fabric CLI på Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) vägleder dig genom en uppgradering av programmet med hjälp av Service Fabric CLI.

[Uppgradera ditt program med hjälp av Service Fabric Eclipse plugin-program](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).
