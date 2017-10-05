---
title: "Felsöka med systemet hälsorapporter | Microsoft Docs"
description: "Beskriver hälsorapporter som skickas av Azure Service Fabric-komponenter och deras användning för felsökning kluster eller problem."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: 54e20146b2f1e0ca6153b66319be70c6f7c2fb59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a><span data-ttu-id="2ef7c-103">Felsök med hjälp av systemhälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-103">Use system health reports to troubleshoot</span></span>
<span data-ttu-id="2ef7c-104">Azure Service Fabric-komponenter rapporten direkt på alla entiteter i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-104">Azure Service Fabric components report out of the box on all entities in the cluster.</span></span> <span data-ttu-id="2ef7c-105">Den [hälsoarkivet](service-fabric-health-introduction.md#health-store) skapar och tar bort enheter baserat på systemrapporter.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-105">The [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on the system reports.</span></span> <span data-ttu-id="2ef7c-106">Även ordnar dem i en hierarki som samlar in entiteten interaktioner.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef7c-107">För att förstå hälsorelaterade begrepp, kan du läsa mer i [Service Fabric-hälsomodell](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-107">To understand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="2ef7c-108">System hälsorapporter ger inblick i klustret och programfunktioner och flaggan problem med hjälp av hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="2ef7c-109">För program och tjänster Kontrollera system hälsorapporter att entiteter implementeras och fungerar korrekt ur ett Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from the Service Fabric perspective.</span></span> <span data-ttu-id="2ef7c-110">Rapporterna ger inte några hälsoövervakning av affärslogiken för tjänsten eller detekteringen av låsta processer.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-110">The reports do not provide any health monitoring of the business logic of the service or detection of hung processes.</span></span> <span data-ttu-id="2ef7c-111">Användaren tjänster kan utöka hälsa-data med information som är specifika för sin logik.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-111">User services can enrich the health data with information specific to their logic.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef7c-112">Watchdogs hälsorapporter visas endast *när* systemkomponenter skapa en entitet.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-112">Watchdogs health reports are visible only *after* the system components create an entity.</span></span> <span data-ttu-id="2ef7c-113">När du tar bort en entitet bort health store automatiskt alla hälsorapporter som associeras med den.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-113">When an entity is deleted, the health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="2ef7c-114">Detsamma gäller när en ny instans av entiteten skapas (till exempel skapas en ny tillståndskänslig beständiga replik tjänstinstans).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-114">The same is true when a new instance of the entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="2ef7c-115">Alla rapporter som är associerade med den gamla instansen tas bort och rensa från store.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-115">All reports associated with the old instance are deleted and cleaned up from the store.</span></span>
> 
> 

<span data-ttu-id="2ef7c-116">Systemkomponenten rapporterar identifieras av källa som börjar med den ”**System.**”</span><span class="sxs-lookup"><span data-stu-id="2ef7c-116">The system component reports are identified by the source, which starts with the "**System.**"</span></span> <span data-ttu-id="2ef7c-117">prefix.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-117">prefix.</span></span> <span data-ttu-id="2ef7c-118">Watchdogs kan inte använda samma prefix för deras källor som avvisas rapporter med ogiltiga parametrar.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-118">Watchdogs can't use the same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="2ef7c-119">Nu ska vi titta på vissa systemrapporter att förstå vad utlöser dem och åtgärda eventuella problem som de representerar.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-119">Let's look at some system reports to understand what triggers them and how to correct the possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef7c-120">Service Fabric fortsätter att lägga till rapporter på villkor som förbättrar inblick i vad som händer i klustret och tillämpningen av intresse.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-120">Service Fabric continues to add reports on conditions of interest that improve visibility into what is happening in the cluster and application.</span></span> <span data-ttu-id="2ef7c-121">Befintliga rapporter kan även förbättras med mer information för att felsöka problemet snabbare.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-121">Existing reports can also be enhanced with more details to help troubleshoot the problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="2ef7c-122">Klustret system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-122">Cluster system health reports</span></span>
<span data-ttu-id="2ef7c-123">Entiteten hälsa kluster skapas automatiskt i health store.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-123">The cluster health entity is created automatically in the health store.</span></span> <span data-ttu-id="2ef7c-124">Om allt fungerar korrekt, har den inte en rapport.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="2ef7c-125">Förlust av nätverket</span><span class="sxs-lookup"><span data-stu-id="2ef7c-125">Neighborhood loss</span></span>
<span data-ttu-id="2ef7c-126">**System.Federation** rapporterar ett fel när den identifierar en förlust av nätverket.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="2ef7c-127">Rapporten är från enskilda noder och nod-ID ingår i namnet.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-127">The report is from individual nodes, and the node ID is included in the property name.</span></span> <span data-ttu-id="2ef7c-128">Om en nätverket förlorat i hela Service Fabric ringen kan förvänta du vanligtvis två händelser (båda sidor av rapporten lucka).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-128">If one neighborhood is lost in the entire Service Fabric ring, you can typically expect two events (both sides of the gap report).</span></span> <span data-ttu-id="2ef7c-129">Om flera neighborhoods går förlorade, finns det flera händelser.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="2ef7c-130">Rapporten anger globala kontraktstidsgräns som time to live.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-130">The report specifies the global lease timeout as the time to live.</span></span> <span data-ttu-id="2ef7c-131">Rapporten skickas varje hälften av TTL-tiden för så länge villkoret förblir aktiv.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-131">The report is resent every half of the TTL duration for as long as the condition remains active.</span></span> <span data-ttu-id="2ef7c-132">Händelsen tas bort automatiskt när den upphör.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-132">The event is automatically removed when it expires.</span></span> <span data-ttu-id="2ef7c-133">Ta bort när utgångna beteendet garanteras att rapporten har rensats bort från health store korrekt, även om noden reporting är nere.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-133">Remove when expired behavior ensures that the report is cleaned up from the health store correctly, even if the reporting node is down.</span></span>

* <span data-ttu-id="2ef7c-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="2ef7c-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="2ef7c-135">**Egenskapen**: börjar med **nätverket** och innehåller information om noden</span><span class="sxs-lookup"><span data-stu-id="2ef7c-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="2ef7c-136">**Nästa steg**: undersöka varför i nätverket går förlorad (till exempel Kontrollera kommunikationen mellan noder i klustret).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-136">**Next steps**: Investigate why the neighborhood is lost (for example, check the communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="2ef7c-137">Noden system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-137">Node system health reports</span></span>
<span data-ttu-id="2ef7c-138">**System.FM**, som representerar Failover Manager-tjänsten är utfärdaren som hanterar information om klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-138">**System.FM**, which represents the Failover Manager service, is the authority that manages information about cluster nodes.</span></span> <span data-ttu-id="2ef7c-139">Varje nod bör ha en rapport från System.FM som visar sitt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="2ef7c-140">Noden enheter tas bort när tillståndet noden tas bort (se [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-140">The node entities are removed when the node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="2ef7c-141">Noden upp/ned</span><span class="sxs-lookup"><span data-stu-id="2ef7c-141">Node up/down</span></span>
<span data-ttu-id="2ef7c-142">System.FM rapporter som OK när noden ansluter ringen (det är igång).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-142">System.FM reports as OK when the node joins the ring (it's up and running).</span></span> <span data-ttu-id="2ef7c-143">Ett fel rapporteras när noden avgår ringen (den är nere, antingen för att uppgradera eller helt enkelt eftersom den inte kunde).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-143">It reports an error when the node departs the ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="2ef7c-144">Hälsotillstånd hierarkin som skapats av health store vidtar åtgärder på distribuerade entiteter i samband med System.FM noden rapporter.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-144">The health hierarchy built by the health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="2ef7c-145">Noden anser entiteter för alla distribuerade virtuella överordnad.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-145">It considers the node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="2ef7c-146">Distribuerade enheterna på noden som är tillgängliga via frågor om noden rapporteras som drift av System.FM med samma instans som den instans som är associerade med entiteterna.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-146">The deployed entities on that node are exposed through queries if the node is reported as up by System.FM, with the same instance as the instance associated with the entities.</span></span> <span data-ttu-id="2ef7c-147">När System.FM rapporterar att noden inte är igång eller startas om (en ny instans), rensas health store automatiskt de distribuerade entiteter som kan finnas endast på noden nedåt eller på den föregående versionen av noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-147">When System.FM reports that the node is down or restarted (a new instance), the health store automatically cleans up the deployed entities that can exist only on the down node or on the previous instance of the node.</span></span>

* <span data-ttu-id="2ef7c-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ef7c-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ef7c-149">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ef7c-149">**Property**: State</span></span>
* <span data-ttu-id="2ef7c-150">**Nästa steg**: om noden avser en uppgradering, det ska gå tillbaka när den har uppgraderats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-150">**Next steps**: If the node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="2ef7c-151">I det här fallet bör hälsotillståndet växla tillbaka till OK.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-151">In this case, the health state should switch back to OK.</span></span> <span data-ttu-id="2ef7c-152">Om noden inte gå tillbaka eller om det misslyckas måste problemet mer undersökning.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-152">If the node doesn't come back or it fails, the problem needs more investigation.</span></span>

<span data-ttu-id="2ef7c-153">I följande exempel visas System.FM händelse med ett hälsotillstånd OK för noden:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-153">The following example shows the System.FM event with a health state of OK for node up:</span></span>

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a><span data-ttu-id="2ef7c-154">Certifikatet upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="2ef7c-154">Certificate expiration</span></span>
<span data-ttu-id="2ef7c-155">**System.FabricNode** rapporterar en varning när certifikat som används av noden närmar sig förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-155">**System.FabricNode** reports a warning when certificates used by the node are near expiration.</span></span> <span data-ttu-id="2ef7c-156">Det finns tre certifikat per nod: **Certificate_cluster**, **Certificate_server**, och **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="2ef7c-157">När upphör att gälla är minst två veckor, är hälsotillståndet för rapporten OK.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-157">When the expiration is at least two weeks away, the report health state is OK.</span></span> <span data-ttu-id="2ef7c-158">När upphör att gälla inom två veckor är rapporttypen en varning.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-158">When the expiration is within two weeks, the report type is a warning.</span></span> <span data-ttu-id="2ef7c-159">TTL-värde på dessa händelser är oändligt och de tas bort när en nod lämnar klustret.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-159">TTL of these events is infinite, and they are removed when a node leaves the cluster.</span></span>

* <span data-ttu-id="2ef7c-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="2ef7c-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="2ef7c-161">**Egenskapen**: börjar med **certifikat** och innehåller mer information om vilken certifikattyp</span><span class="sxs-lookup"><span data-stu-id="2ef7c-161">**Property**: Starts with **Certificate** and contains more information about the certificate type</span></span>
* <span data-ttu-id="2ef7c-162">**Nästa steg**: uppdatera certifikat om de är nära förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-162">**Next steps**: Update the certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="2ef7c-163">Läsa in kapacitetsöverträdelse</span><span class="sxs-lookup"><span data-stu-id="2ef7c-163">Load capacity violation</span></span>
<span data-ttu-id="2ef7c-164">Service Fabric-Belastningsutjämnarens rapporterar en varning när den identifierar en kapacitetsöverträdelse för noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-164">The Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="2ef7c-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ef7c-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ef7c-166">**Egenskapen**: börjar med **kapacitet**</span><span class="sxs-lookup"><span data-stu-id="2ef7c-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="2ef7c-167">**Nästa steg**: Kontrollera att mått har angetts och visa den aktuella kapaciteten på noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-167">**Next steps**: Check provided metrics and view the current capacity on the node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="2ef7c-168">Programmet system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-168">Application system health reports</span></span>
<span data-ttu-id="2ef7c-169">**System.CM**, som representerar klustret Manager-tjänsten är utfärdaren som hanterar information om ett program.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-169">**System.CM**, which represents the Cluster Manager service, is the authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="2ef7c-170">Status</span><span class="sxs-lookup"><span data-stu-id="2ef7c-170">State</span></span>
<span data-ttu-id="2ef7c-171">System.CM rapporterar som OK när programmet har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-171">System.CM reports as OK when the application has been created or updated.</span></span> <span data-ttu-id="2ef7c-172">Informerar arkivet hälsotillstånd när programmet har tagits bort, så att den kan tas bort från store.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-172">It informs the health store when the application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="2ef7c-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="2ef7c-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="2ef7c-174">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ef7c-174">**Property**: State</span></span>
* <span data-ttu-id="2ef7c-175">**Nästa steg**: om programmet har skapats eller uppdaterats, ska det finnas hälsorapport Klusterhanteraren.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-175">**Next steps**: If the application has been created or updated, it should include the Cluster Manager health report.</span></span> <span data-ttu-id="2ef7c-176">I annat fall söker tillståndet för programmet genom att utfärda en fråga (till exempel PowerShell-cmdleten **ServiceFabricApplication Get - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-176">Otherwise, check the state of the application by issuing a query (for example, the PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="2ef7c-177">I följande exempel visas händelsen på den **fabric: / WordCount** program:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-177">The following example shows the state event on the **fabric:/WordCount** application:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a><span data-ttu-id="2ef7c-178">Tjänsten system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-178">Service system health reports</span></span>
<span data-ttu-id="2ef7c-179">**System.FM**, som representerar Failover Manager-tjänsten är auktoritet som hanterar information om tjänster.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-179">**System.FM**, which represents the Failover Manager service, is the authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="2ef7c-180">Status</span><span class="sxs-lookup"><span data-stu-id="2ef7c-180">State</span></span>
<span data-ttu-id="2ef7c-181">System.FM rapporter som OK när tjänsten har skapats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-181">System.FM reports as OK when the service has been created.</span></span> <span data-ttu-id="2ef7c-182">Det tar bort enheten från health store när tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-182">It deletes the entity from the health store when the service has been deleted.</span></span>

* <span data-ttu-id="2ef7c-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ef7c-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ef7c-184">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ef7c-184">**Property**: State</span></span>

<span data-ttu-id="2ef7c-185">I följande exempel visas händelsen på tjänsten **fabric: / WordCount/WordCountWebService**:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-185">The following example shows the state event on the service **fabric:/WordCount/WordCountWebService**:</span></span>

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a><span data-ttu-id="2ef7c-186">Fel i tjänsten korrelation</span><span class="sxs-lookup"><span data-stu-id="2ef7c-186">Service correlation error</span></span>
<span data-ttu-id="2ef7c-187">**System.PLB** rapporterar ett fel när den identifierar att uppdatera en tjänst kan korreleras med en annan tjänst skapas en tillhörighetskedja.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-187">**System.PLB** reports an error when it detects that updating a service to be correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="2ef7c-188">Rapporten är avmarkerad när uppdateringen sker.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-188">The report is cleared when successful update happens.</span></span>

* <span data-ttu-id="2ef7c-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ef7c-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ef7c-190">**Egenskapen**: ServiceDescription</span><span class="sxs-lookup"><span data-stu-id="2ef7c-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="2ef7c-191">**Nästa steg**: Kontrollera korrelerade service beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-191">**Next steps**: Check the correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="2ef7c-192">Partitionen system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-192">Partition system health reports</span></span>
<span data-ttu-id="2ef7c-193">**System.FM**, som representerar Failover Manager-tjänsten är utfärdaren som hanterar information om tjänsten partitioner.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-193">**System.FM**, which represents the Failover Manager service, is the authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="2ef7c-194">Status</span><span class="sxs-lookup"><span data-stu-id="2ef7c-194">State</span></span>
<span data-ttu-id="2ef7c-195">System.FM rapporterar som OK när partitionen har skapats och är felfri.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-195">System.FM reports as OK when the partition has been created and is healthy.</span></span> <span data-ttu-id="2ef7c-196">Det tar bort enheten från health store när partitionen tas bort.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-196">It deletes the entity from the health store when the partition is deleted.</span></span>

<span data-ttu-id="2ef7c-197">Om partitionen är mindre än det minsta antalet, rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-197">If the partition is below the minimum replica count, it reports an error.</span></span> <span data-ttu-id="2ef7c-198">Om partitionen är inte mindre än det minsta antalet, men den är under målet replikantalet, rapporterar en varning.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-198">If the partition is not below the minimum replica count, but it is below the target replica count, it reports a warning.</span></span> <span data-ttu-id="2ef7c-199">Om partitionen förlorar kvorum, rapporterar System.FM ett fel.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-199">If the partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="2ef7c-200">Andra viktiga händelser inkludera en varning när omkonfiguration tar längre tid än förväntat och när bygga tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-200">Other important events include a warning when the reconfiguration takes longer than expected and when the build takes longer than expected.</span></span> <span data-ttu-id="2ef7c-201">Den förväntade gånger för att bygga och omkonfiguration kan konfigureras baserat på tjänsten scenarier.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-201">The expected times for the build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="2ef7c-202">Till exempel om en tjänst har en terabyte tillstånd, till exempel SQL-databas tar bygga längre tid än för en tjänst med en liten mängd tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-202">For example, if a service has a terabyte of state, such as SQL Database, the build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="2ef7c-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ef7c-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ef7c-204">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ef7c-204">**Property**: State</span></span>
* <span data-ttu-id="2ef7c-205">**Nästa steg**: om hälsotillståndet inte är OK, är det möjligt att repliker inte har skapats, öppnas, eller befordras till primär eller sekundär korrekt.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-205">**Next steps**: If the health state is not OK, it's possible that some replicas have not been created, opened, or promoted to primary or secondary correctly.</span></span> <span data-ttu-id="2ef7c-206">I många fall är den grundläggande orsaken ett service-fel i implementeringen öppna eller ändra roll.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-206">In many instances, the root cause is a service bug in the open or change-role implementation.</span></span>

<span data-ttu-id="2ef7c-207">I följande exempel visas en felfri partition:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-207">The following example shows a healthy partition:</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

<span data-ttu-id="2ef7c-208">I följande exempel visar hälsotillståndet för en partition som är lägre än målet replikantalet.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-208">The following example shows the health of a partition that is below target replica count.</span></span> <span data-ttu-id="2ef7c-209">Nästa steg är att hämta partitionen beskrivning, som visar hur den är konfigurerad: **MinReplicaSetSize** är tre och **TargetReplicaSetSize** sju.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-209">The next step is to get the partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="2ef7c-210">Hämta sedan antalet noder i klustret: fem.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-210">Then get the number of nodes in the cluster: five.</span></span> <span data-ttu-id="2ef7c-211">Så i det här fallet två repliker kan inte placeras eftersom målet antal repliker är högre än antalet noder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-211">So in this case, two replicas can't be placed because the target number of replicas is higher than the number of nodes available.</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/P RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a><span data-ttu-id="2ef7c-212">Replik begränsningsfel</span><span class="sxs-lookup"><span data-stu-id="2ef7c-212">Replica constraint violation</span></span>
<span data-ttu-id="2ef7c-213">**System.PLB** rapporterar en varning om den identifierar en replik begränsningsfel och går inte att placera alla partition repliker.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="2ef7c-214">Rapportdetaljer visar vilka villkor och egenskaper förhindra replik-placering.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-214">The report details show which constraints and properties prevent the replica placement.</span></span>

* <span data-ttu-id="2ef7c-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ef7c-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ef7c-216">**Egenskapen**: börjar med **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="2ef7c-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="2ef7c-217">Replik system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-217">Replica system health reports</span></span>
<span data-ttu-id="2ef7c-218">**System.RA**, som representerar komponenten omkonfiguration agent är auktoritet för replik-tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-218">**System.RA**, which represents the reconfiguration agent component, is the authority for the replica state.</span></span>

### <a name="state"></a><span data-ttu-id="2ef7c-219">Status</span><span class="sxs-lookup"><span data-stu-id="2ef7c-219">State</span></span>
<span data-ttu-id="2ef7c-220">**System.RA** rapporterar OK när repliken har skapats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-220">**System.RA** reports OK when the replica has been created.</span></span>

* <span data-ttu-id="2ef7c-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="2ef7c-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="2ef7c-222">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ef7c-222">**Property**: State</span></span>

<span data-ttu-id="2ef7c-223">I följande exempel visas en felfri replik:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-223">The following example shows a healthy replica:</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replica-open-status"></a><span data-ttu-id="2ef7c-224">Replik öppen status</span><span class="sxs-lookup"><span data-stu-id="2ef7c-224">Replica open status</span></span>
<span data-ttu-id="2ef7c-225">Beskrivning av den här hälsorapport innehåller starttiden (Coordinated Universal Time) när API-anrop anropades.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-225">The description of this health report contains the start time (Coordinated Universal Time) when the API call was invoked.</span></span>

<span data-ttu-id="2ef7c-226">**System.RA** rapporterar en varning om repliken öppna tar längre tid än den konfigurerade tidsperioden (standard: 30 minuter).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-226">**System.RA** reports a warning if the replica open takes longer than the configured period (default: 30 minutes).</span></span> <span data-ttu-id="2ef7c-227">Om API: et påverkar tjänsttillgänglighet, utfärdas rapporten mycket snabbare (en konfigurerbar intervall, med en standard på 30 sekunder).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-227">If the API impacts service availability, the report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="2ef7c-228">Tiden mäts innehåller den tid det tar för replikatorn öppen och öppna tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-228">The time measured includes the time taken for the replicator open and the service open.</span></span> <span data-ttu-id="2ef7c-229">Egenskapen ändras till OK om öppna har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-229">The property changes to OK if the open completes.</span></span>

* <span data-ttu-id="2ef7c-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="2ef7c-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="2ef7c-231">**Egenskapen**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="2ef7c-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="2ef7c-232">**Nästa steg**: om hälsotillståndet inte är OK, undersöka varför repliken öppna tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-232">**Next steps**: If the health state is not OK, investigate why the replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="2ef7c-233">Långsam service API-anrop</span><span class="sxs-lookup"><span data-stu-id="2ef7c-233">Slow service API call</span></span>
<span data-ttu-id="2ef7c-234">**System.RAP** och **System.Replicator** rapportera en varning om ett anrop till användarkod för tjänsten tar längre tid än den konfigurerade tidpunkten.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-234">**System.RAP** and **System.Replicator** report a warning if a call to the user service code takes longer than the configured time.</span></span> <span data-ttu-id="2ef7c-235">Varningen rensas när anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-235">The warning is cleared when the call completes.</span></span>

* <span data-ttu-id="2ef7c-236">**SourceId**: System.RAP eller System.Replicator</span><span class="sxs-lookup"><span data-stu-id="2ef7c-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="2ef7c-237">**Egenskapen**: namnet på den långsamma API.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-237">**Property**: The name of the slow API.</span></span> <span data-ttu-id="2ef7c-238">Beskrivningen ger mer information om den tid som API: et har väntande.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-238">The description provides more details about the time the API has been pending.</span></span>
* <span data-ttu-id="2ef7c-239">**Nästa steg**: undersöka varför anropet tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-239">**Next steps**: Investigate why the call takes longer than expected.</span></span>

<span data-ttu-id="2ef7c-240">I följande exempel visas en partition i förlorar kvorum och undersökning stegen för att ta reda på varför.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-240">The following example shows a partition in quorum loss, and the investigation steps done to figure out why.</span></span> <span data-ttu-id="2ef7c-241">En av replikerna har ett varningshälsotillstånd så att du får dess hälsa.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-241">One of the replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="2ef7c-242">Det visar att en åtgärd i tjänsten tar längre tid än väntat, en händelse som rapporterats av System.RAP.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-242">It shows that the service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="2ef7c-243">När informationen har tagits emot, är nästa steg att titta på kod och undersöka det.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-243">After this information is received, the next step is to look at the service code and investigate there.</span></span> <span data-ttu-id="2ef7c-244">För det här fallet den **RunAsync** implementeringen av tjänsten tillståndskänslig utlöser ett undantag.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-244">For this case, the **RunAsync** implementation of the stateful service throws an unhandled exception.</span></span> <span data-ttu-id="2ef7c-245">Replikerna omarbetning så att du inte kan se alla repliker i varningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-245">The replicas are recycling, so you may not see any replicas in the warning state.</span></span> <span data-ttu-id="2ef7c-246">Du kan försöka komma hälsotillståndet och söka efter skillnader i replik-ID.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-246">You can retry getting the health state and look for any differences in the replica ID.</span></span> <span data-ttu-id="2ef7c-247">I vissa fall kan återförsöken ge ledtrådar.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-247">In certain cases, the retries can give you clues.</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 3
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

<span data-ttu-id="2ef7c-248">När du startar det felande programmet under felsökaren kan visa diagnostik händelser windows undantag utlöstes från RunAsync:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-248">When you start the faulty application under the debugger, the diagnostic events windows show the exception thrown from RunAsync:</span></span>

![Visual Studio 2015 diagnostiska händelser: RunAsync fel i fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="2ef7c-250">Visual Studio 2015 diagnostiska händelser: RunAsync fel i **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="2ef7c-251">Replikeringskön är full</span><span class="sxs-lookup"><span data-stu-id="2ef7c-251">Replication queue full</span></span>
<span data-ttu-id="2ef7c-252">**System.Replicator** rapporterar en varning när Replikeringskön är full.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-252">**System.Replicator** reports a warning when the replication queue is full.</span></span> <span data-ttu-id="2ef7c-253">På primärt blir Replikeringskön vanligtvis full eftersom en eller flera sekundära repliker tar lång tid att godkänna åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-253">On the primary, replication queue usually becomes full because one or more secondary replicas are slow to acknowledge operations.</span></span> <span data-ttu-id="2ef7c-254">På sekundärt inträffar detta när tjänsten går långsamt att tillämpa åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-254">On the secondary, this usually happens when the service is slow to apply the operations.</span></span> <span data-ttu-id="2ef7c-255">Varningen rensas när kön är inte längre fullständig.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-255">The warning is cleared when the queue is no longer full.</span></span>

* <span data-ttu-id="2ef7c-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="2ef7c-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="2ef7c-257">**Egenskapen**: **PrimaryReplicationQueueStatus** eller **SecondaryReplicationQueueStatus**, beroende på rollen replik</span><span class="sxs-lookup"><span data-stu-id="2ef7c-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on the replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="2ef7c-258">Långsam Naming åtgärder</span><span class="sxs-lookup"><span data-stu-id="2ef7c-258">Slow Naming operations</span></span>
<span data-ttu-id="2ef7c-259">**System.NamingService** rapporterar hälsotillståndet för dess primära repliken när en Naming åtgärden tar längre tid än godkända.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="2ef7c-260">Exempel på Naming operations är [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) eller [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="2ef7c-261">Flera metoder kan hittas under FabricClient, till exempel [tjänsten metoder för hantering av](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) eller [metoder för hantering av egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="2ef7c-262">Tjänsten Naming matchar tjänstnamn till en plats i klustret och gör att användarna kan hantera tjänstnamn och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-262">The Naming service resolves service names to a location in the cluster and enables users to manage service names and properties.</span></span> <span data-ttu-id="2ef7c-263">Det är ett Service Fabric partitionerad beständiga tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="2ef7c-264">En av partitionerna representerar Authority Owner som innehåller metadata om alla Service Fabric-namn och tjänster.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-264">One of the partitions represents the Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="2ef7c-265">Service Fabric-namnen mappas till olika partitioner, kallas Name Owner partitioner, så att tjänsten kan utökas.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-265">The Service Fabric names are mapped to different partitions, called Name Owner partitions, so the service is extensible.</span></span> <span data-ttu-id="2ef7c-266">Läs mer om [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="2ef7c-267">När en Naming åtgärden tar längre tid än förväntat åtgärden har flaggats med en varning-rapport på den *primära repliken för Naming service-partition som fungerar igen*.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-267">When a Naming operation takes longer than expected, the operation is flagged with a Warning report on the *primary replica of the Naming service partition that serves the operation*.</span></span> <span data-ttu-id="2ef7c-268">Om åtgärden slutförs är varningen avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-268">If the operation completes successfully, the Warning is cleared.</span></span> <span data-ttu-id="2ef7c-269">Om åtgärden har slutförts med ett fel, innehåller hälsa rapporten information om felet.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-269">If the operation completes with an error, the health report includes details about the error.</span></span>

* <span data-ttu-id="2ef7c-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="2ef7c-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="2ef7c-271">**Egenskapen**: börjar med prefixet **Duration_** och identifierar långsam igen och Service Fabric-namnet som åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-271">**Property**: Starts with prefix **Duration_** and identifies the slow operation and the Service Fabric name on which the operation is applied.</span></span> <span data-ttu-id="2ef7c-272">Till exempel om skapar tjänst med namnet fabric: / MyApp/MyService tar för lång tid, egenskapen är Duration_AOCreateService.fabric:/MyApp/MyService.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-272">For example, if create service at name fabric:/MyApp/MyService takes too long, the property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="2ef7c-273">AO pekar på rollen för Naming partition för det här namnet och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-273">AO points to the role of the Naming partition for this name and operation.</span></span>
* <span data-ttu-id="2ef7c-274">**Nästa steg**: Kontrollera orsaken Naming åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-274">**Next steps**: Check why the Naming operation fails.</span></span> <span data-ttu-id="2ef7c-275">Varje åtgärd kan ha olika rotorsaker.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-275">Each operation can have different root causes.</span></span> <span data-ttu-id="2ef7c-276">Till exempel ta bort tjänsten kan ha fastnat på en nod eftersom programvärden håller kraschar på en nod på grund av ett programfel för användare i kod.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-276">For example, delete service may be stuck on a node because the application host keeps crashing on a node due to a user bug in the service code.</span></span>

<span data-ttu-id="2ef7c-277">I följande exempel visas en Skapa-åtgärd i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-277">The following example shows a create service operation.</span></span> <span data-ttu-id="2ef7c-278">Åtgärden tog längre tid än den konfigurerade varaktigheten.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-278">The operation took longer than the configured duration.</span></span> <span data-ttu-id="2ef7c-279">AO återförsök och skickar arbetsobjekt till Nej.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-279">AO retries and sends work to NO.</span></span> <span data-ttu-id="2ef7c-280">INTE slutföra den senaste åtgärden med Timeout.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-280">NO completed the last operation with Timeout.</span></span> <span data-ttu-id="2ef7c-281">I det här fallet är samma replik primär för både AO och inga roller.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-281">In this case, the same replica is primary for both the AO and NO roles.</span></span>

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="2ef7c-282">DeployedApplication system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="2ef7c-283">**System.Hosting** är auktoritet på distribuerade entiteter.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-283">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="2ef7c-284">Aktivering</span><span class="sxs-lookup"><span data-stu-id="2ef7c-284">Activation</span></span>
<span data-ttu-id="2ef7c-285">System.Hosting rapporterar som OK när ett program har aktiverats på noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-285">System.Hosting reports as OK when an application has been successfully activated on the node.</span></span> <span data-ttu-id="2ef7c-286">Annars rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="2ef7c-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-288">**Egenskapen**: aktivering, inklusive distribution-version</span><span class="sxs-lookup"><span data-stu-id="2ef7c-288">**Property**: Activation, including the rollout version</span></span>
* <span data-ttu-id="2ef7c-289">**Nästa steg**: om programmet är i feltillstånd, undersöka varför aktiveringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-289">**Next steps**: If the application is unhealthy, investigate why the activation failed.</span></span>

<span data-ttu-id="2ef7c-290">I följande exempel visar lyckad aktivering:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-290">The following example shows successful activation:</span></span>

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="2ef7c-291">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="2ef7c-291">Download</span></span>
<span data-ttu-id="2ef7c-292">**System.Hosting** rapporterar ett fel om inte paketet ned programmet.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-292">**System.Hosting** reports an error if the application package download fails.</span></span>

* <span data-ttu-id="2ef7c-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-294">**Egenskapen**:  **hämta:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="2ef7c-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="2ef7c-295">**Nästa steg**: undersöka varför hämtningen misslyckades på noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-295">**Next steps**: Investigate why the download failed on the node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="2ef7c-296">DeployedServicePackage system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="2ef7c-297">**System.Hosting** är auktoritet på distribuerade entiteter.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-297">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="2ef7c-298">Aktivering av Service-paket</span><span class="sxs-lookup"><span data-stu-id="2ef7c-298">Service package activation</span></span>
<span data-ttu-id="2ef7c-299">System.Hosting rapporter som OK om tjänsten paketet aktiveringen på noden har genomförts.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-299">System.Hosting reports as OK if the service package activation on the node is successful.</span></span> <span data-ttu-id="2ef7c-300">Annars rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="2ef7c-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-302">**Egenskapen**: aktivering</span><span class="sxs-lookup"><span data-stu-id="2ef7c-302">**Property**: Activation</span></span>
* <span data-ttu-id="2ef7c-303">**Nästa steg**: undersöka varför aktiveringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-303">**Next steps**: Investigate why the activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="2ef7c-304">Aktivering av kod paketet</span><span class="sxs-lookup"><span data-stu-id="2ef7c-304">Code package activation</span></span>
<span data-ttu-id="2ef7c-305">**System.Hosting** rapporter som OK för varje kodpaketet om aktiveringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-305">**System.Hosting** reports as OK for each code package if the activation is successful.</span></span> <span data-ttu-id="2ef7c-306">Om aktiveringen misslyckas rapporterar en varning som konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-306">If the activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="2ef7c-307">Om **CodePackage** inte kan aktivera eller avslutas med ett fel som är större än den konfigurerade **CodePackageHealthErrorThreshold**, värd rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-307">If **CodePackage** fails to activate or terminates with an error greater than the configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="2ef7c-308">Om en tjänstepaketet innehåller flera kod paket, genereras en aktivering-rapporten för varje kriterium.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="2ef7c-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-310">**Egenskapen**: namnområdesprefixet **CodePackageActivation** och innehåller namnet på kodpaketet och startpunkt som  **CodePackageActivation:*CodePackageName* :*SetupEntryPoint/EntryPoint*** (till exempel **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="2ef7c-310">**Property**: Uses the prefix **CodePackageActivation** and contains the name of the code package and the entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="2ef7c-311">Typ av registrering</span><span class="sxs-lookup"><span data-stu-id="2ef7c-311">Service type registration</span></span>
<span data-ttu-id="2ef7c-312">**System.Hosting** rapporterar som OK om tjänsttypen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-312">**System.Hosting** reports as OK if the service type has been registered successfully.</span></span> <span data-ttu-id="2ef7c-313">Ett fel rapporteras om den inte registrerats i tid (som konfigurerats med hjälp av **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="2ef7c-313">It reports an error if the registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="2ef7c-314">Om körningen är stängt, service-typen är avregistrera från noden och värd rapporterar en varning.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-314">If the runtime is closed, the service type is unregistered from the node and Hosting reports a warning.</span></span>

* <span data-ttu-id="2ef7c-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-316">**Egenskapen**: namnområdesprefixet **ServiceTypeRegistration** och innehåller namnet på tjänsten (till exempel **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="2ef7c-316">**Property**: Uses the prefix **ServiceTypeRegistration** and contains the service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="2ef7c-317">I följande exempel visas en felfri distribuerat tjänstpaket:</span><span class="sxs-lookup"><span data-stu-id="2ef7c-317">The following example shows a healthy deployed service package:</span></span>

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="2ef7c-318">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="2ef7c-318">Download</span></span>
<span data-ttu-id="2ef7c-319">**System.Hosting** rapporterar ett fel om inte paketet ned service.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-319">**System.Hosting** reports an error if the service package download fails.</span></span>

* <span data-ttu-id="2ef7c-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-321">**Egenskapen**:  **hämta:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="2ef7c-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="2ef7c-322">**Nästa steg**: undersöka varför hämtningen misslyckades på noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-322">**Next steps**: Investigate why the download failed on the node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="2ef7c-323">Validering av uppgradering</span><span class="sxs-lookup"><span data-stu-id="2ef7c-323">Upgrade validation</span></span>
<span data-ttu-id="2ef7c-324">**System.Hosting** rapporterar ett fel om valideringen under uppgraderingen misslyckas eller om uppgraderingen misslyckas på noden.</span><span class="sxs-lookup"><span data-stu-id="2ef7c-324">**System.Hosting** reports an error if validation during the upgrade fails or if the upgrade fails on the node.</span></span>

* <span data-ttu-id="2ef7c-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ef7c-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ef7c-326">**Egenskapen**: namnområdesprefixet **FabricUpgradeValidation** och innehåller den uppgradera versionen</span><span class="sxs-lookup"><span data-stu-id="2ef7c-326">**Property**: Uses the prefix **FabricUpgradeValidation** and contains the upgrade version</span></span>
* <span data-ttu-id="2ef7c-327">**Beskrivning**: pekar på fel</span><span class="sxs-lookup"><span data-stu-id="2ef7c-327">**Description**: Points to the error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ef7c-328">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ef7c-328">Next steps</span></span>
[<span data-ttu-id="2ef7c-329">Visa Service Fabric-hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="2ef7c-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="2ef7c-330">Hur du rapporten och kontrollera tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="2ef7c-330">How to report and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="2ef7c-331">Övervaka och diagnostisera tjänster lokalt</span><span class="sxs-lookup"><span data-stu-id="2ef7c-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="2ef7c-332">Uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="2ef7c-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

