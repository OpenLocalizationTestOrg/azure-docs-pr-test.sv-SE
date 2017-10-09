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
# <a name="troubleshoot-application-upgrades"></a><span data-ttu-id="b6b02-103">Felsök programuppgraderingar</span><span class="sxs-lookup"><span data-stu-id="b6b02-103">Troubleshoot application upgrades</span></span>
<span data-ttu-id="b6b02-104">Den här artikeln beskrivs några vanliga problem med hello runt uppgraderar ett Azure Service Fabric-program och hur tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="b6b02-104">This article covers some of hello common issues around upgrading an Azure Service Fabric application and how tooresolve them.</span></span>

## <a name="troubleshoot-a-failed-application-upgrade"></a><span data-ttu-id="b6b02-105">Felsöka en misslyckad Programuppgradering</span><span class="sxs-lookup"><span data-stu-id="b6b02-105">Troubleshoot a failed application upgrade</span></span>
<span data-ttu-id="b6b02-106">När uppgraderingen misslyckas, hello utdata från hello **Get-ServiceFabricApplicationUpgrade** kommando innehåller ytterligare information för felsökning hello-fel.</span><span class="sxs-lookup"><span data-stu-id="b6b02-106">When an upgrade fails, hello output of hello **Get-ServiceFabricApplicationUpgrade** command contains additional information for debugging hello failure.</span></span>  <span data-ttu-id="b6b02-107">hello anger följande lista hur hello ytterligare information kan användas:</span><span class="sxs-lookup"><span data-stu-id="b6b02-107">hello following list specifies how hello additional information can be used:</span></span>

1. <span data-ttu-id="b6b02-108">Identifiera hello feltyp.</span><span class="sxs-lookup"><span data-stu-id="b6b02-108">Identify hello failure type.</span></span>
2. <span data-ttu-id="b6b02-109">Identifiera hello orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="b6b02-109">Identify hello failure reason.</span></span>
3. <span data-ttu-id="b6b02-110">Isolera en eller flera misslyckas komponenter för ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="b6b02-110">Isolate one or more failing components for further investigation.</span></span>

<span data-ttu-id="b6b02-111">Den här informationen är tillgänglig när Service Fabric hittar hello oavsett om hello **FailureAction** är tooroll tillbaka eller pausar hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="b6b02-111">This information is available when Service Fabric detects hello failure regardless of whether hello **FailureAction** is tooroll back or suspend hello upgrade.</span></span>

### <a name="identify-hello-failure-type"></a><span data-ttu-id="b6b02-112">Identifiera hello feltyp</span><span class="sxs-lookup"><span data-stu-id="b6b02-112">Identify hello failure type</span></span>
<span data-ttu-id="b6b02-113">I hello utdata från **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifierar hello tidstämpeln (UTC) då en uppgradering fel upptäcktes av Service Fabric och  **FailureAction** utlöstes.</span><span class="sxs-lookup"><span data-stu-id="b6b02-113">In hello output of **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifies hello timestamp (in UTC) at which an upgrade failure was detected by Service Fabric and **FailureAction** was triggered.</span></span> <span data-ttu-id="b6b02-114">**FailureReason** identifierar en av tre möjliga övergripande orsakerna till hello-fel:</span><span class="sxs-lookup"><span data-stu-id="b6b02-114">**FailureReason** identifies one of three potential high-level causes of hello failure:</span></span>

1. <span data-ttu-id="b6b02-115">UpgradeDomainTimeout - anger att en viss domän tog för lång toocomplete och **UpgradeDomainTimeout** upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="b6b02-115">UpgradeDomainTimeout - Indicates that a particular upgrade domain took too long toocomplete and **UpgradeDomainTimeout** expired.</span></span>
2. <span data-ttu-id="b6b02-116">OverallUpgradeTimeout - anger den övergripande uppgradera hello tog för lång toocomplete och **UpgradeTimeout** upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="b6b02-116">OverallUpgradeTimeout - Indicates that hello overall upgrade took too long toocomplete and **UpgradeTimeout** expired.</span></span>
3. <span data-ttu-id="b6b02-117">Hälsokontroll - anger att programmet hello när du har uppgraderat en uppdateringsdomän legat ohälsosamt enligt toohello angetts hälsoprinciper och **HealthCheckRetryTimeout** upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="b6b02-117">HealthCheck - Indicates that after upgrading an update domain, hello application remained unhealthy according toohello specified health policies and **HealthCheckRetryTimeout** expired.</span></span>

<span data-ttu-id="b6b02-118">Dessa poster endast visas i hello utdata när hello uppgraderingen misslyckas och startar återställs.</span><span class="sxs-lookup"><span data-stu-id="b6b02-118">These entries only show up in hello output when hello upgrade fails and starts rolling back.</span></span> <span data-ttu-id="b6b02-119">Ytterligare information visas beroende på hello typ av hello-fel.</span><span class="sxs-lookup"><span data-stu-id="b6b02-119">Further information is displayed depending on hello type of hello failure.</span></span>

### <a name="investigate-upgrade-timeouts"></a><span data-ttu-id="b6b02-120">Undersök uppgradera tidsgränser</span><span class="sxs-lookup"><span data-stu-id="b6b02-120">Investigate upgrade timeouts</span></span>
<span data-ttu-id="b6b02-121">Uppgradera timeout fel orsakas oftast av problem med tjänsters tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="b6b02-121">Upgrade timeout failures are most commonly caused by service availability issues.</span></span> <span data-ttu-id="b6b02-122">hello utdata följande stycke är typiska för uppgraderingar där tjänsten repliker eller instanser misslyckas toostart i hello nya code-versionen.</span><span class="sxs-lookup"><span data-stu-id="b6b02-122">hello output following this paragraph is typical of upgrades where service replicas or instances fail toostart in hello new code version.</span></span> <span data-ttu-id="b6b02-123">Hej **UpgradeDomainProgressAtFailure** fältet samlar in en ögonblicksbild av alla väntande uppgradera arbete på hello tiden för felet.</span><span class="sxs-lookup"><span data-stu-id="b6b02-123">hello **UpgradeDomainProgressAtFailure** field captures a snapshot of any pending upgrade work at hello time of failure.</span></span>

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

<span data-ttu-id="b6b02-124">I det här exemplet hello gick inte att uppgradera på uppgraderingsdomänen *MYUD1* och två partitioner (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* och *4b43f4d8-b26b-424e-9307-7a7a62e79750*) har fastnat.</span><span class="sxs-lookup"><span data-stu-id="b6b02-124">In this example, hello upgrade failed at upgrade domain *MYUD1* and two partitions (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* and *4b43f4d8-b26b-424e-9307-7a7a62e79750*) were stuck.</span></span> <span data-ttu-id="b6b02-125">hello partitioner har fastnat eftersom hello körning var tooplace primära repliker (*WaitForPrimaryPlacement*) på målnoder *Node1* och *nod4*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-125">hello partitions were stuck because hello runtime was unable tooplace primary replicas (*WaitForPrimaryPlacement*) on target nodes *Node1* and *Node4*.</span></span>

<span data-ttu-id="b6b02-126">Hej **Get-ServiceFabricNode** kommandot kan det vara används tooverify som dessa två noder i uppgraderingsdomänen *MYUD1*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-126">hello **Get-ServiceFabricNode** command can be used tooverify that these two nodes are in upgrade domain *MYUD1*.</span></span> <span data-ttu-id="b6b02-127">Hej *UpgradePhase* står *PostUpgradeSafetyCheck*, vilket innebär att kontrollerna säkerhet sker när alla noder i hello uppgraderingsdomänen är klar för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="b6b02-127">hello *UpgradePhase* says *PostUpgradeSafetyCheck*, which means that these safety checks are occurring after all nodes in hello upgrade domain have finished upgrading.</span></span> <span data-ttu-id="b6b02-128">Den här informationen pekar tooa potentiella problem med hello ny version av hello programkod.</span><span class="sxs-lookup"><span data-stu-id="b6b02-128">All this information points tooa potential issue with hello new version of hello application code.</span></span> <span data-ttu-id="b6b02-129">hello de flesta vanliga problem är service-fel i hello öppna eller befordran tooprimary kodsökvägar.</span><span class="sxs-lookup"><span data-stu-id="b6b02-129">hello most common issues are service errors in hello open or promotion tooprimary code paths.</span></span>

<span data-ttu-id="b6b02-130">En *UpgradePhase* av *PreUpgradeSafetyCheck* innebär det fanns problem förbereder hello uppgraderingsdomänen innan den har utförts.</span><span class="sxs-lookup"><span data-stu-id="b6b02-130">An *UpgradePhase* of *PreUpgradeSafetyCheck* means there were issues preparing hello upgrade domain before it was performed.</span></span> <span data-ttu-id="b6b02-131">hello de flesta vanliga problem finns i det här fallet service-fel i hello Stäng eller degraderingen från primära kodsökvägar.</span><span class="sxs-lookup"><span data-stu-id="b6b02-131">hello most common issues in this case are service errors in hello close or demotion from primary code paths.</span></span>

<span data-ttu-id="b6b02-132">hello aktuella **UpgradeState** är *RollingBackCompleted*, så hello ursprungliga uppgraderingen måste ha utförts med en återställning **FailureAction**, som automatiskt Återställde hello uppgradering vid fel.</span><span class="sxs-lookup"><span data-stu-id="b6b02-132">hello current **UpgradeState** is *RollingBackCompleted*, so hello original upgrade must have been performed with a rollback **FailureAction**, which automatically rolled back hello upgrade upon failure.</span></span> <span data-ttu-id="b6b02-133">Om hello ursprungliga uppgraderingen har utförts med en manuell **FailureAction**, och sedan hello uppgraderingen i stället är i ett pausat tillstånd tooallow live-felsökning av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="b6b02-133">If hello original upgrade was performed with a manual **FailureAction**, then hello upgrade would instead be in a suspended state tooallow live debugging of hello application.</span></span>

### <a name="investigate-health-check-failures"></a><span data-ttu-id="b6b02-134">Undersök health check-fel</span><span class="sxs-lookup"><span data-stu-id="b6b02-134">Investigate health check failures</span></span>
<span data-ttu-id="b6b02-135">Health check fel kan utlösas av olika problem som kan inträffa när alla noder i en uppgraderingsdomän Slutför uppgraderingen och skicka alla säkerhet kontroller.</span><span class="sxs-lookup"><span data-stu-id="b6b02-135">Health check failures can be triggered by various issues that can happen after all nodes in an upgrade domain finish upgrading and passing all safety checks.</span></span> <span data-ttu-id="b6b02-136">hello utdata följande stycke är typiska för en uppgraderingen skulle misslyckas på grund av toofailed hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="b6b02-136">hello output following this paragraph is typical of an upgrade failure due toofailed health checks.</span></span> <span data-ttu-id="b6b02-137">Hej **UnhealthyEvaluations** fältet samlar in en ögonblicksbild av hälsokontroller som misslyckades när hello hello uppgraderingen enligt angivna toohello [hälsoprincip](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b6b02-137">hello **UnhealthyEvaluations** field captures a snapshot of health checks that failed at hello time of hello upgrade according toohello specified [health policy](service-fabric-health-introduction.md).</span></span>

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

<span data-ttu-id="b6b02-138">Undersöka health check fel kräver att först förstå hello Service Fabric-hälsomodell.</span><span class="sxs-lookup"><span data-stu-id="b6b02-138">Investigating health check failures first requires an understanding of hello Service Fabric health model.</span></span> <span data-ttu-id="b6b02-139">Men även om ingen sådan en djupgående förståelse kan vi se att två tjänster är felfria: *fabric: / DemoApp/Svc3* och *fabric: / DemoApp/Svc2*, tillsammans med hello fel hälsorapporter (”InjectedFault ”i detta fall).</span><span class="sxs-lookup"><span data-stu-id="b6b02-139">But even without such an in-depth understanding, we can see that two services are unhealthy: *fabric:/DemoApp/Svc3* and *fabric:/DemoApp/Svc2*, along with hello error health reports ("InjectedFault" in this case).</span></span> <span data-ttu-id="b6b02-140">I det här exemplet två av de fyra tjänster är felaktig, vilket är under hello standard målet 0% ohälsosamt (*MaxPercentUnhealthyServices*).</span><span class="sxs-lookup"><span data-stu-id="b6b02-140">In this example, two out of four services are unhealthy, which is below hello default target of 0% unhealthy (*MaxPercentUnhealthyServices*).</span></span>

<span data-ttu-id="b6b02-141">hello uppgraderingen pausades på misslyckas genom att ange en **FailureAction** av manuella när startar hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="b6b02-141">hello upgrade was suspended upon failing by specifying a **FailureAction** of manual when starting hello upgrade.</span></span> <span data-ttu-id="b6b02-142">Det här läget kan vi tooinvestigate hello live system i hello misslyckades tillstånd innan du tar några fler åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b6b02-142">This mode allows us tooinvestigate hello live system in hello failed state before taking any further action.</span></span>

### <a name="recover-from-a-suspended-upgrade"></a><span data-ttu-id="b6b02-143">Återställa från en pausade uppgradering</span><span class="sxs-lookup"><span data-stu-id="b6b02-143">Recover from a suspended upgrade</span></span>
<span data-ttu-id="b6b02-144">Med en återställning **FailureAction**, det finns ingen återställning behövs eftersom hello uppgraderingen automatiskt återställs vid misslyckas.</span><span class="sxs-lookup"><span data-stu-id="b6b02-144">With a rollback **FailureAction**, there is no recovery needed since hello upgrade automatically rolls back upon failing.</span></span> <span data-ttu-id="b6b02-145">Med en manuell **FailureAction**, det finns flera återställningsalternativ:</span><span class="sxs-lookup"><span data-stu-id="b6b02-145">With a manual **FailureAction**, there are several recovery options:</span></span>

1.  <span data-ttu-id="b6b02-146">Starta en återställning</span><span class="sxs-lookup"><span data-stu-id="b6b02-146">trigger a rollback</span></span>
2. <span data-ttu-id="b6b02-147">Gå igenom hello resten av hello uppgraderingen manuellt</span><span class="sxs-lookup"><span data-stu-id="b6b02-147">Proceed through hello remainder of hello upgrade manually</span></span>
3. <span data-ttu-id="b6b02-148">Återuppta hello övervakas uppgradering</span><span class="sxs-lookup"><span data-stu-id="b6b02-148">Resume hello monitored upgrade</span></span>

<span data-ttu-id="b6b02-149">Hej **Start ServiceFabricApplicationRollback** kommando kan användas på alla tid toostart återställer hello program.</span><span class="sxs-lookup"><span data-stu-id="b6b02-149">hello **Start-ServiceFabricApplicationRollback** command can be used at any time toostart rolling back hello application.</span></span> <span data-ttu-id="b6b02-150">När hello kommandot returnerar har hello Återställningsbegäran har registrerats i hello system och startar strax därefter.</span><span class="sxs-lookup"><span data-stu-id="b6b02-150">Once hello command returns successfully, hello rollback request has been registered in hello system and starts shortly thereafter.</span></span>

<span data-ttu-id="b6b02-151">Hej **återuppta ServiceFabricApplicationUpgrade** kan använda kommandot tooproceed via hello resten av hello manuellt, uppgradera en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="b6b02-151">hello **Resume-ServiceFabricApplicationUpgrade** command can be used tooproceed through hello remainder of hello upgrade manually, one upgrade domain at a time.</span></span> <span data-ttu-id="b6b02-152">I det här läget utförs endast säkerhet kontroller av hello system.</span><span class="sxs-lookup"><span data-stu-id="b6b02-152">In this mode, only safety checks are performed by hello system.</span></span> <span data-ttu-id="b6b02-153">Inga fler systemhälsokontroller utförs.</span><span class="sxs-lookup"><span data-stu-id="b6b02-153">No more health checks are performed.</span></span> <span data-ttu-id="b6b02-154">Det här kommandot kan endast användas när hello *UpgradeState* visar *RollingForwardPending*, vilket innebär att den aktuella uppgraderingsdomänen hello har uppgraderats men hello bredvid en har inte startats (väntar).</span><span class="sxs-lookup"><span data-stu-id="b6b02-154">This command can only be used when hello *UpgradeState* shows *RollingForwardPending*, which means that hello current upgrade domain has finished upgrading but hello next one has not started (pending).</span></span>

<span data-ttu-id="b6b02-155">Hej **uppdatering ServiceFabricApplicationUpgrade** kommando kan vara används tooresume hello övervakas uppgradering med både hälsa och kontroller som utförs.</span><span class="sxs-lookup"><span data-stu-id="b6b02-155">hello **Update-ServiceFabricApplicationUpgrade** command can be used tooresume hello monitored upgrade with both safety and health checks being performed.</span></span>

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

<span data-ttu-id="b6b02-156">hello uppgraderingen fortsätter från hello uppgraderingsdomänen där det pausades senast och Använd hello samma uppgradera parametrar och hälsoprinciper som innan.</span><span class="sxs-lookup"><span data-stu-id="b6b02-156">hello upgrade continues from hello upgrade domain where it was last suspended and use hello same upgrade parameters and health policies as before.</span></span> <span data-ttu-id="b6b02-157">Om det behövs, ändras hello uppgradera parametrar och hälsoprinciper som visas i föregående utdata hello i hello samma kommando när hello uppgraderingen återställs.</span><span class="sxs-lookup"><span data-stu-id="b6b02-157">If needed, any of hello upgrade parameters and health policies shown in hello preceding output can be changed in hello same command when hello upgrade resumes.</span></span> <span data-ttu-id="b6b02-158">I det här exemplet återupptogs hello uppgraderingen i övervakade-läge med hello parametrar och hello hälsoprinciper oförändrade.</span><span class="sxs-lookup"><span data-stu-id="b6b02-158">In this example, hello upgrade was resumed in Monitored mode, with hello parameters and hello health policies unchanged.</span></span>

## <a name="further-troubleshooting"></a><span data-ttu-id="b6b02-159">Felsökningsinformation</span><span class="sxs-lookup"><span data-stu-id="b6b02-159">Further troubleshooting</span></span>
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a><span data-ttu-id="b6b02-160">Service Fabric inte följer hello angetts hälsoprinciper</span><span class="sxs-lookup"><span data-stu-id="b6b02-160">Service Fabric is not following hello specified health policies</span></span>
<span data-ttu-id="b6b02-161">Möjlig orsak 1:</span><span class="sxs-lookup"><span data-stu-id="b6b02-161">Possible Cause 1:</span></span>

<span data-ttu-id="b6b02-162">Service Fabric översätter alla procenttal till faktiska antalet enheter (till exempel repliker, partitioner och tjänster) för utvärdering av hälsotillstånd och alltid Avrundar uppåt toowhole entiteter.</span><span class="sxs-lookup"><span data-stu-id="b6b02-162">Service Fabric translates all percentages into actual numbers of entities (for example, replicas, partitions, and services) for health evaluation and always rounds up toowhole entities.</span></span> <span data-ttu-id="b6b02-163">Till exempel, om hello maximala *MaxPercentUnhealthyReplicasPerPartition* är 21% och det finns fem repliker, och sedan Service Fabric kan tootwo ohälsosamt repliker (det vill säga`Math.Ceiling (5*0.21)`).</span><span class="sxs-lookup"><span data-stu-id="b6b02-163">For example, if hello maximum *MaxPercentUnhealthyReplicasPerPartition* is 21% and there are five replicas, then Service Fabric allows up tootwo unhealthy replicas (that is,`Math.Ceiling (5*0.21)`).</span></span> <span data-ttu-id="b6b02-164">Därför ska hälsoprinciper fastställas.</span><span class="sxs-lookup"><span data-stu-id="b6b02-164">Thus, health policies should be set accordingly.</span></span>

<span data-ttu-id="b6b02-165">Möjlig orsak 2:</span><span class="sxs-lookup"><span data-stu-id="b6b02-165">Possible Cause 2:</span></span>

<span data-ttu-id="b6b02-166">Hälsoprinciper anges som procent av totalt antal tjänster och inte är specifika tjänstinstanser.</span><span class="sxs-lookup"><span data-stu-id="b6b02-166">Health policies are specified in terms of percentages of total services and not specific service instances.</span></span> <span data-ttu-id="b6b02-167">Till exempel före en uppgradering, om ett program har fyra tjänsten instanser A, B, C och D, där tjänsten D är felfri men med liten inverkan toohello program.</span><span class="sxs-lookup"><span data-stu-id="b6b02-167">For example, before an upgrade, if an application has four service instances A, B, C, and D, where service D is unhealthy but with little impact toohello application.</span></span> <span data-ttu-id="b6b02-168">Vi vill tooignore hello kända ohälsosamt service D under uppgraderingen och ange hello parametern *MaxPercentUnhealthyServices* toobe 25%, under förutsättning att endast A, B och C måste toobe felfritt.</span><span class="sxs-lookup"><span data-stu-id="b6b02-168">We want tooignore hello known unhealthy service D during upgrade and set hello parameter *MaxPercentUnhealthyServices* toobe 25%, assuming only A, B, and C need toobe healthy.</span></span>

<span data-ttu-id="b6b02-169">Men vid uppgradering hello bli D felfri medan C blir ohälsosamt.</span><span class="sxs-lookup"><span data-stu-id="b6b02-169">However, during hello upgrade, D may become healthy while C becomes unhealthy.</span></span> <span data-ttu-id="b6b02-170">hello uppgraderingen kan ändå lyckas eftersom endast 25% av hello tjänster är felfria.</span><span class="sxs-lookup"><span data-stu-id="b6b02-170">hello upgrade would still succeed because only 25% of hello services are unhealthy.</span></span> <span data-ttu-id="b6b02-171">Men kan det resultera i oväntade fel på grund av att tooC oväntat feltillstånd i stället för D. I det här fallet bör D modelleras som en annan typ från A och B och C. Eftersom hälsoprinciper anges per typ av tjänst, vara annan ohälsosamt procentsats tröskelvärden tillämpade toodifferent tjänster.</span><span class="sxs-lookup"><span data-stu-id="b6b02-171">However, it might result in unanticipated errors due tooC being unexpectedly unhealthy instead of D. In this situation, D should be modeled as a different service type from A, B, and C. Since health policies are specified per service type, different unhealthy percentage thresholds can be applied toodifferent services.</span></span> 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a><span data-ttu-id="b6b02-172">Jag har inte angett en hälsoprincip för uppgradering av programmet, men hello fortfarande går inte att uppgradera för vissa timeout som jag aldrig har angetts</span><span class="sxs-lookup"><span data-stu-id="b6b02-172">I did not specify a health policy for application upgrade, but hello upgrade still fails for some time-outs that I never specified</span></span>
<span data-ttu-id="b6b02-173">När hälsoprinciper inte toohello uppgraderingsbegäran kan hämtas från hello *ApplicationManifest.xml* av hello aktuella programversionen.</span><span class="sxs-lookup"><span data-stu-id="b6b02-173">When health policies aren't provided toohello upgrade request, they are taken from hello *ApplicationManifest.xml* of hello current application version.</span></span> <span data-ttu-id="b6b02-174">Om du uppgraderar programmet X från version 1.0 tooversion 2.0 används till exempel hälsoprinciper för program som angetts för version 1.0.</span><span class="sxs-lookup"><span data-stu-id="b6b02-174">For example, if you're upgrading Application X from version 1.0 tooversion 2.0, application health policies specified for in version 1.0 are used.</span></span> <span data-ttu-id="b6b02-175">Om en annan hälsoprincip ska användas för hello uppgraderingen, måste hello princip toobe som angetts som en del av hello programmet uppgradera API-anrop.</span><span class="sxs-lookup"><span data-stu-id="b6b02-175">If a different health policy should be used for hello upgrade, then hello policy needs toobe specified as part of hello application upgrade API call.</span></span> <span data-ttu-id="b6b02-176">hello gäller principer som har angetts som en del av hello API-anrop endast under hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="b6b02-176">hello policies specified as part of hello API call only apply during hello upgrade.</span></span> <span data-ttu-id="b6b02-177">När hello uppgraderingen är klar hello principer som angetts i hello *ApplicationManifest.xml* används.</span><span class="sxs-lookup"><span data-stu-id="b6b02-177">Once hello upgrade is complete, hello policies specified in hello *ApplicationManifest.xml* are used.</span></span>

### <a name="incorrect-time-outs-are-specified"></a><span data-ttu-id="b6b02-178">Felaktig timeout har angetts</span><span class="sxs-lookup"><span data-stu-id="b6b02-178">Incorrect time-outs are specified</span></span>
<span data-ttu-id="b6b02-179">Du har funderat över om vad som händer när för timeout är inkonsekvent.</span><span class="sxs-lookup"><span data-stu-id="b6b02-179">You may have wondered about what happens when time-outs are set inconsistently.</span></span> <span data-ttu-id="b6b02-180">Du kan till exempel ha en *UpgradeTimeout* som är mindre än hello *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-180">For example, you may have an *UpgradeTimeout* that's less than hello *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="b6b02-181">hello svaret är att ett fel returneras.</span><span class="sxs-lookup"><span data-stu-id="b6b02-181">hello answer is that an error is returned.</span></span> <span data-ttu-id="b6b02-182">Fel returneras om hello *UpgradeDomainTimeout* är mindre än hello summan av *HealthCheckWaitDuration* och *HealthCheckRetryTimeout*, eller om  *UpgradeDomainTimeout* är mindre än hello summan av *HealthCheckWaitDuration* och *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-182">Errors are returned if hello *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckRetryTimeout*, or if *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckStableDuration*.</span></span>

### <a name="my-upgrades-are-taking-too-long"></a><span data-ttu-id="b6b02-183">Min uppgraderingar tar för lång</span><span class="sxs-lookup"><span data-stu-id="b6b02-183">My upgrades are taking too long</span></span>
<span data-ttu-id="b6b02-184">hello tiden för en uppgradering toocomplete beror på hello hälsokontroller och timeout anges.</span><span class="sxs-lookup"><span data-stu-id="b6b02-184">hello time for an upgrade toocomplete depends on hello health checks and time-outs specified.</span></span> <span data-ttu-id="b6b02-185">Hälsokontroller och tidsgränser beror på hur lång tid det tar att toocopy, distribuera och hålla hello program.</span><span class="sxs-lookup"><span data-stu-id="b6b02-185">Health checks and time-outs depend on how long it takes toocopy, deploy, and stabilize hello application.</span></span> <span data-ttu-id="b6b02-186">Som för aggressivt med timeout kan betyda flera misslyckade uppgraderingar, så vi rekommenderar att du börjar restriktiva med längre timeout.</span><span class="sxs-lookup"><span data-stu-id="b6b02-186">Being too aggressive with time-outs might mean more failed upgrades, so we recommend starting conservatively with longer time-outs.</span></span>

<span data-ttu-id="b6b02-187">Här är en snabb uppdaterare på hur hello timeout interagera med hello uppgradera gånger:</span><span class="sxs-lookup"><span data-stu-id="b6b02-187">Here's a quick refresher on how hello time-outs interact with hello upgrade times:</span></span>

<span data-ttu-id="b6b02-188">Uppgraderar för en uppgraderingsdomän inte kan slutföra snabbare än *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-188">Upgrades for an upgrade domain cannot complete faster than *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span></span>

<span data-ttu-id="b6b02-189">Du uppgraderar kan inte ske snabbare än *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-189">Upgrade failure cannot occur faster than *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span></span>

<span data-ttu-id="b6b02-190">hello tiden för uppgraderingen för en uppgraderingsdomän begränsas av *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-190">hello upgrade time for an upgrade domain is limited by *UpgradeDomainTimeout*.</span></span>  <span data-ttu-id="b6b02-191">Om *HealthCheckRetryTimeout* och *HealthCheckStableDuration* är båda inte är noll och hello hälsotillståndet för programmet hello håller växla fram och tillbaka och sedan hello uppgraderingen slutligen timeout på  *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="b6b02-191">If *HealthCheckRetryTimeout* and *HealthCheckStableDuration* are both non-zero and hello health of hello application keeps switching back and forth, then hello upgrade eventually times out on *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="b6b02-192">*UpgradeDomainTimeout* startar räknat en gång hello uppgradering för hello aktuell uppgraderingsdomän börjar.</span><span class="sxs-lookup"><span data-stu-id="b6b02-192">*UpgradeDomainTimeout* starts counting down once hello upgrade for hello current upgrade domain begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6b02-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6b02-193">Next steps</span></span>
<span data-ttu-id="b6b02-194">[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6b02-194">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="b6b02-195">[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b6b02-195">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="b6b02-196">Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="b6b02-196">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="b6b02-197">Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="b6b02-197">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="b6b02-198">Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="b6b02-198">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
