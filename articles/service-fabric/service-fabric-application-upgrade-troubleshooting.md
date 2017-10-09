---
title: aaaTroubleshooting programuppgraderingar | Microsoft Docs
description: "Den här artikeln beskriver några vanliga problem runt att uppgradera ett Service Fabric-program och hur tooresolve dem."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 0f56fa61db9b4e32824623f162dc1bfe7fda0f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-upgrades"></a>Felsök programuppgraderingar
Den här artikeln beskrivs några vanliga problem med hello runt uppgraderar ett Azure Service Fabric-program och hur tooresolve dem.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Felsöka en misslyckad Programuppgradering
När uppgraderingen misslyckas, hello utdata från hello **Get-ServiceFabricApplicationUpgrade** kommando innehåller ytterligare information för felsökning hello-fel.  hello anger följande lista hur hello ytterligare information kan användas:

1. Identifiera hello feltyp.
2. Identifiera hello orsaken till felet.
3. Isolera en eller flera misslyckas komponenter för ytterligare undersökning.

Den här informationen är tillgänglig när Service Fabric hittar hello oavsett om hello **FailureAction** är tooroll tillbaka eller pausar hello uppgraderingen.

### <a name="identify-hello-failure-type"></a>Identifiera hello feltyp
I hello utdata från **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifierar hello tidstämpeln (UTC) då en uppgradering fel upptäcktes av Service Fabric och  **FailureAction** utlöstes. **FailureReason** identifierar en av tre möjliga övergripande orsakerna till hello-fel:

1. UpgradeDomainTimeout - anger att en viss domän tog för lång toocomplete och **UpgradeDomainTimeout** upphört att gälla.
2. OverallUpgradeTimeout - anger den övergripande uppgradera hello tog för lång toocomplete och **UpgradeTimeout** upphört att gälla.
3. Hälsokontroll - anger att programmet hello när du har uppgraderat en uppdateringsdomän legat ohälsosamt enligt toohello angetts hälsoprinciper och **HealthCheckRetryTimeout** upphört att gälla.

Dessa poster endast visas i hello utdata när hello uppgraderingen misslyckas och startar återställs. Ytterligare information visas beroende på hello typ av hello-fel.

### <a name="investigate-upgrade-timeouts"></a>Undersök uppgradera tidsgränser
Uppgradera timeout fel orsakas oftast av problem med tjänsters tillgänglighet. hello utdata följande stycke är typiska för uppgraderingar där tjänsten repliker eller instanser misslyckas toostart i hello nya code-versionen. Hej **UpgradeDomainProgressAtFailure** fältet samlar in en ögonblicksbild av alla väntande uppgradera arbete på hello tiden för felet.

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

I det här exemplet hello gick inte att uppgradera på uppgraderingsdomänen *MYUD1* och två partitioner (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* och *4b43f4d8-b26b-424e-9307-7a7a62e79750*) har fastnat. hello partitioner har fastnat eftersom hello körning var tooplace primära repliker (*WaitForPrimaryPlacement*) på målnoder *Node1* och *nod4*.

Hej **Get-ServiceFabricNode** kommandot kan det vara används tooverify som dessa två noder i uppgraderingsdomänen *MYUD1*. Hej *UpgradePhase* står *PostUpgradeSafetyCheck*, vilket innebär att kontrollerna säkerhet sker när alla noder i hello uppgraderingsdomänen är klar för uppgradering. Den här informationen pekar tooa potentiella problem med hello ny version av hello programkod. hello de flesta vanliga problem är service-fel i hello öppna eller befordran tooprimary kodsökvägar.

En *UpgradePhase* av *PreUpgradeSafetyCheck* innebär det fanns problem förbereder hello uppgraderingsdomänen innan den har utförts. hello de flesta vanliga problem finns i det här fallet service-fel i hello Stäng eller degraderingen från primära kodsökvägar.

hello aktuella **UpgradeState** är *RollingBackCompleted*, så hello ursprungliga uppgraderingen måste ha utförts med en återställning **FailureAction**, som automatiskt Återställde hello uppgradering vid fel. Om hello ursprungliga uppgraderingen har utförts med en manuell **FailureAction**, och sedan hello uppgraderingen i stället är i ett pausat tillstånd tooallow live-felsökning av programmet hello.

### <a name="investigate-health-check-failures"></a>Undersök health check-fel
Health check fel kan utlösas av olika problem som kan inträffa när alla noder i en uppgraderingsdomän Slutför uppgraderingen och skicka alla säkerhet kontroller. hello utdata följande stycke är typiska för en uppgraderingen skulle misslyckas på grund av toofailed hälsokontroller. Hej **UnhealthyEvaluations** fältet samlar in en ögonblicksbild av hälsokontroller som misslyckades när hello hello uppgraderingen enligt angivna toohello [hälsoprincip](service-fabric-health-introduction.md).

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Undersöka health check fel kräver att först förstå hello Service Fabric-hälsomodell. Men även om ingen sådan en djupgående förståelse kan vi se att två tjänster är felfria: *fabric: / DemoApp/Svc3* och *fabric: / DemoApp/Svc2*, tillsammans med hello fel hälsorapporter (”InjectedFault ”i detta fall). I det här exemplet två av de fyra tjänster är felaktig, vilket är under hello standard målet 0% ohälsosamt (*MaxPercentUnhealthyServices*).

hello uppgraderingen pausades på misslyckas genom att ange en **FailureAction** av manuella när startar hello uppgraderingen. Det här läget kan vi tooinvestigate hello live system i hello misslyckades tillstånd innan du tar några fler åtgärder.

### <a name="recover-from-a-suspended-upgrade"></a>Återställa från en pausade uppgradering
Med en återställning **FailureAction**, det finns ingen återställning behövs eftersom hello uppgraderingen automatiskt återställs vid misslyckas. Med en manuell **FailureAction**, det finns flera återställningsalternativ:

1.  Starta en återställning
2. Gå igenom hello resten av hello uppgraderingen manuellt
3. Återuppta hello övervakas uppgradering

Hej **Start ServiceFabricApplicationRollback** kommando kan användas på alla tid toostart återställer hello program. När hello kommandot returnerar har hello Återställningsbegäran har registrerats i hello system och startar strax därefter.

Hej **återuppta ServiceFabricApplicationUpgrade** kan använda kommandot tooproceed via hello resten av hello manuellt, uppgradera en domän i taget. I det här läget utförs endast säkerhet kontroller av hello system. Inga fler systemhälsokontroller utförs. Det här kommandot kan endast användas när hello *UpgradeState* visar *RollingForwardPending*, vilket innebär att den aktuella uppgraderingsdomänen hello har uppgraderats men hello bredvid en har inte startats (väntar).

Hej **uppdatering ServiceFabricApplicationUpgrade** kommando kan vara används tooresume hello övervakas uppgradering med både hälsa och kontroller som utförs.

```
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
```

hello uppgraderingen fortsätter från hello uppgraderingsdomänen där det pausades senast och Använd hello samma uppgradera parametrar och hälsoprinciper som innan. Om det behövs, ändras hello uppgradera parametrar och hälsoprinciper som visas i föregående utdata hello i hello samma kommando när hello uppgraderingen återställs. I det här exemplet återupptogs hello uppgraderingen i övervakade-läge med hello parametrar och hello hälsoprinciper oförändrade.

## <a name="further-troubleshooting"></a>Felsökningsinformation
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a>Service Fabric inte följer hello angetts hälsoprinciper
Möjlig orsak 1:

Service Fabric översätter alla procenttal till faktiska antalet enheter (till exempel repliker, partitioner och tjänster) för utvärdering av hälsotillstånd och alltid Avrundar uppåt toowhole entiteter. Till exempel, om hello maximala *MaxPercentUnhealthyReplicasPerPartition* är 21% och det finns fem repliker, och sedan Service Fabric kan tootwo ohälsosamt repliker (det vill säga`Math.Ceiling (5*0.21)`). Därför ska hälsoprinciper fastställas.

Möjlig orsak 2:

Hälsoprinciper anges som procent av totalt antal tjänster och inte är specifika tjänstinstanser. Till exempel före en uppgradering, om ett program har fyra tjänsten instanser A, B, C och D, där tjänsten D är felfri men med liten inverkan toohello program. Vi vill tooignore hello kända ohälsosamt service D under uppgraderingen och ange hello parametern *MaxPercentUnhealthyServices* toobe 25%, under förutsättning att endast A, B och C måste toobe felfritt.

Men vid uppgradering hello bli D felfri medan C blir ohälsosamt. hello uppgraderingen kan ändå lyckas eftersom endast 25% av hello tjänster är felfria. Men kan det resultera i oväntade fel på grund av att tooC oväntat feltillstånd i stället för D. I det här fallet bör D modelleras som en annan typ från A och B och C. Eftersom hälsoprinciper anges per typ av tjänst, vara annan ohälsosamt procentsats tröskelvärden tillämpade toodifferent tjänster. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Jag har inte angett en hälsoprincip för uppgradering av programmet, men hello fortfarande går inte att uppgradera för vissa timeout som jag aldrig har angetts
När hälsoprinciper inte toohello uppgraderingsbegäran kan hämtas från hello *ApplicationManifest.xml* av hello aktuella programversionen. Om du uppgraderar programmet X från version 1.0 tooversion 2.0 används till exempel hälsoprinciper för program som angetts för version 1.0. Om en annan hälsoprincip ska användas för hello uppgraderingen, måste hello princip toobe som angetts som en del av hello programmet uppgradera API-anrop. hello gäller principer som har angetts som en del av hello API-anrop endast under hello uppgraderingen. När hello uppgraderingen är klar hello principer som angetts i hello *ApplicationManifest.xml* används.

### <a name="incorrect-time-outs-are-specified"></a>Felaktig timeout har angetts
Du har funderat över om vad som händer när för timeout är inkonsekvent. Du kan till exempel ha en *UpgradeTimeout* som är mindre än hello *UpgradeDomainTimeout*. hello svaret är att ett fel returneras. Fel returneras om hello *UpgradeDomainTimeout* är mindre än hello summan av *HealthCheckWaitDuration* och *HealthCheckRetryTimeout*, eller om  *UpgradeDomainTimeout* är mindre än hello summan av *HealthCheckWaitDuration* och *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Min uppgraderingar tar för lång
hello tiden för en uppgradering toocomplete beror på hello hälsokontroller och timeout anges. Hälsokontroller och tidsgränser beror på hur lång tid det tar att toocopy, distribuera och hålla hello program. Som för aggressivt med timeout kan betyda flera misslyckade uppgraderingar, så vi rekommenderar att du börjar restriktiva med längre timeout.

Här är en snabb uppdaterare på hur hello timeout interagera med hello uppgradera gånger:

Uppgraderar för en uppgraderingsdomän inte kan slutföra snabbare än *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Du uppgraderar kan inte ske snabbare än *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

hello tiden för uppgraderingen för en uppgraderingsdomän begränsas av *UpgradeDomainTimeout*.  Om *HealthCheckRetryTimeout* och *HealthCheckStableDuration* är båda inte är noll och hello hälsotillståndet för programmet hello håller växla fram och tillbaka och sedan hello uppgraderingen slutligen timeout på  *UpgradeDomainTimeout*. *UpgradeDomainTimeout* startar räknat en gång hello uppgradering för hello aktuell uppgraderingsdomän börjar.

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).
