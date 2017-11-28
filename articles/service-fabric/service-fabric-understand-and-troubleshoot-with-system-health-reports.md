---
title: "aaaTroubleshoot med systemet hälsorapporter | Microsoft Docs"
description: "Beskriver hello hälsorapporter som skickas av Azure Service Fabric-komponenter och deras användning för felsökning kluster eller problem."
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
ms.openlocfilehash: c77a6cdd0440ce5d354cd8760f40151f674a3529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-system-health-reports-tootroubleshoot"></a><span data-ttu-id="3e896-103">Använd system hälsa rapporter tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="3e896-103">Use system health reports tootroubleshoot</span></span>
<span data-ttu-id="3e896-104">Azure Service Fabric-komponenter rapport out of box hello på alla entiteter i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3e896-104">Azure Service Fabric components report out of hello box on all entities in hello cluster.</span></span> <span data-ttu-id="3e896-105">Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) skapar och tar bort enheter baserat på hello systemrapporter.</span><span class="sxs-lookup"><span data-stu-id="3e896-105">hello [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on hello system reports.</span></span> <span data-ttu-id="3e896-106">Även ordnar dem i en hierarki som samlar in entiteten interaktioner.</span><span class="sxs-lookup"><span data-stu-id="3e896-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="3e896-107">toounderstand hälsorelaterade begrepp och Läs mer på [Service Fabric-hälsomodell](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3e896-107">toounderstand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="3e896-108">System hälsorapporter ger inblick i klustret och programfunktioner och flaggan problem med hjälp av hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e896-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="3e896-109">System hälsorapporter Kontrollera att entiteter implementeras och fungerar korrekt från hello Service Fabric perspektiv för program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e896-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from hello Service Fabric perspective.</span></span> <span data-ttu-id="3e896-110">hello-rapporter tillhandahåller inte någon hälsoövervakning av hello affärslogik hello service eller detekteringen av låsta processer.</span><span class="sxs-lookup"><span data-stu-id="3e896-110">hello reports do not provide any health monitoring of hello business logic of hello service or detection of hung processes.</span></span> <span data-ttu-id="3e896-111">Användaren tjänster kan utöka hello hälsa data med information specifik tootheir logik.</span><span class="sxs-lookup"><span data-stu-id="3e896-111">User services can enrich hello health data with information specific tootheir logic.</span></span>

> [!NOTE]
> <span data-ttu-id="3e896-112">Watchdogs hälsorapporter visas endast *när* hello systemkomponenter skapa en entitet.</span><span class="sxs-lookup"><span data-stu-id="3e896-112">Watchdogs health reports are visible only *after* hello system components create an entity.</span></span> <span data-ttu-id="3e896-113">När du tar bort en entitet bort hello health store automatiskt alla hälsorapporter som associeras med den.</span><span class="sxs-lookup"><span data-stu-id="3e896-113">When an entity is deleted, hello health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="3e896-114">hello samma sak gäller när en ny instans av entiteten hello skapas (till exempel skapas en ny tillståndskänslig beständiga replik tjänstinstans).</span><span class="sxs-lookup"><span data-stu-id="3e896-114">hello same is true when a new instance of hello entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="3e896-115">Alla rapporter som är kopplat till hello gamla instansen tas bort och rensa från hello store.</span><span class="sxs-lookup"><span data-stu-id="3e896-115">All reports associated with hello old instance are deleted and cleaned up from hello store.</span></span>
> 
> 

<span data-ttu-id="3e896-116">hello system komponenten rapporter identifieras av hello källa som börjar med hello ”**System.**”</span><span class="sxs-lookup"><span data-stu-id="3e896-116">hello system component reports are identified by hello source, which starts with hello "**System.**"</span></span> <span data-ttu-id="3e896-117">prefix.</span><span class="sxs-lookup"><span data-stu-id="3e896-117">prefix.</span></span> <span data-ttu-id="3e896-118">Watchdogs kan inte använda samma prefix för deras källor hello som avvisas rapporter med ogiltiga parametrar.</span><span class="sxs-lookup"><span data-stu-id="3e896-118">Watchdogs can't use hello same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="3e896-119">Nu ska vi titta på vissa system rapporterar toounderstand vad utlöser dem och hur toocorrect hello möjliga problem med de representerar.</span><span class="sxs-lookup"><span data-stu-id="3e896-119">Let's look at some system reports toounderstand what triggers them and how toocorrect hello possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="3e896-120">Service Fabric fortsätter tooadd rapporter på villkor intressanta som förbättrar inblick i vad som händer i hello kluster och program.</span><span class="sxs-lookup"><span data-stu-id="3e896-120">Service Fabric continues tooadd reports on conditions of interest that improve visibility into what is happening in hello cluster and application.</span></span> <span data-ttu-id="3e896-121">Befintliga rapporter kan förbättras med mer information toohelp felsöka hello problem snabbare.</span><span class="sxs-lookup"><span data-stu-id="3e896-121">Existing reports can also be enhanced with more details toohelp troubleshoot hello problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="3e896-122">Klustret system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-122">Cluster system health reports</span></span>
<span data-ttu-id="3e896-123">hello klustret hälsa entitet skapas automatiskt i hello health store.</span><span class="sxs-lookup"><span data-stu-id="3e896-123">hello cluster health entity is created automatically in hello health store.</span></span> <span data-ttu-id="3e896-124">Om allt fungerar korrekt, har den inte en rapport.</span><span class="sxs-lookup"><span data-stu-id="3e896-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="3e896-125">Förlust av nätverket</span><span class="sxs-lookup"><span data-stu-id="3e896-125">Neighborhood loss</span></span>
<span data-ttu-id="3e896-126">**System.Federation** rapporterar ett fel när den identifierar en förlust av nätverket.</span><span class="sxs-lookup"><span data-stu-id="3e896-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="3e896-127">hello rapporten är från enskilda noder och hello nod-ID ingår i hello egenskapsnamn.</span><span class="sxs-lookup"><span data-stu-id="3e896-127">hello report is from individual nodes, and hello node ID is included in hello property name.</span></span> <span data-ttu-id="3e896-128">Om en nätverket förlorat i hello hela Service Fabric ringen kan förvänta du vanligtvis två händelser (båda sidor av hello lucka rapporten).</span><span class="sxs-lookup"><span data-stu-id="3e896-128">If one neighborhood is lost in hello entire Service Fabric ring, you can typically expect two events (both sides of hello gap report).</span></span> <span data-ttu-id="3e896-129">Om flera neighborhoods går förlorade, finns det flera händelser.</span><span class="sxs-lookup"><span data-stu-id="3e896-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="3e896-130">hello rapporten anger hello globala kontraktstidsgräns som hello tid toolive.</span><span class="sxs-lookup"><span data-stu-id="3e896-130">hello report specifies hello global lease timeout as hello time toolive.</span></span> <span data-ttu-id="3e896-131">hello rapporten skickas varje hälften av hello TTL-tiden för så länge villkoret hello förblir aktiv.</span><span class="sxs-lookup"><span data-stu-id="3e896-131">hello report is resent every half of hello TTL duration for as long as hello condition remains active.</span></span> <span data-ttu-id="3e896-132">hello händelse tas bort automatiskt när den upphör.</span><span class="sxs-lookup"><span data-stu-id="3e896-132">hello event is automatically removed when it expires.</span></span> <span data-ttu-id="3e896-133">Ta bort när utgångna beteendet garanteras att hello rapporten rensas från hello health store korrekt, även om hello reporting nod är nere.</span><span class="sxs-lookup"><span data-stu-id="3e896-133">Remove when expired behavior ensures that hello report is cleaned up from hello health store correctly, even if hello reporting node is down.</span></span>

* <span data-ttu-id="3e896-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="3e896-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="3e896-135">**Egenskapen**: börjar med **nätverket** och innehåller information om noden</span><span class="sxs-lookup"><span data-stu-id="3e896-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="3e896-136">**Nästa steg**: undersöka varför hello nätverket går förlorad (till exempel kontrollera hello kommunikation mellan klusternoder).</span><span class="sxs-lookup"><span data-stu-id="3e896-136">**Next steps**: Investigate why hello neighborhood is lost (for example, check hello communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="3e896-137">Noden system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-137">Node system health reports</span></span>
<span data-ttu-id="3e896-138">**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="3e896-138">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about cluster nodes.</span></span> <span data-ttu-id="3e896-139">Varje nod bör ha en rapport från System.FM som visar sitt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e896-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="3e896-140">hello noden enheter tas bort när hello nodens tillstånd tas bort (se [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="3e896-140">hello node entities are removed when hello node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="3e896-141">Noden upp/ned</span><span class="sxs-lookup"><span data-stu-id="3e896-141">Node up/down</span></span>
<span data-ttu-id="3e896-142">System.FM rapporter som OK när hello nod ansluter till hello ringen (det är igång).</span><span class="sxs-lookup"><span data-stu-id="3e896-142">System.FM reports as OK when hello node joins hello ring (it's up and running).</span></span> <span data-ttu-id="3e896-143">Ett fel rapporteras när hello nod avgår hello ringen (den är nere, antingen för att uppgradera eller helt enkelt eftersom den inte kunde).</span><span class="sxs-lookup"><span data-stu-id="3e896-143">It reports an error when hello node departs hello ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="3e896-144">hello hälsa hierarki som skapats av hello health store vidtar åtgärder på distribuerade entiteter i samband med System.FM noden rapporter.</span><span class="sxs-lookup"><span data-stu-id="3e896-144">hello health hierarchy built by hello health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="3e896-145">Betraktar hello nod entiteter för alla distribuerade virtuella överordnad.</span><span class="sxs-lookup"><span data-stu-id="3e896-145">It considers hello node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="3e896-146">hello distribueras entiteter på noden är tillgängliga via frågor om hello nod rapporteras som upp av System.FM, med hello samma instans som hello-instans som är associerade med hello entiteter.</span><span class="sxs-lookup"><span data-stu-id="3e896-146">hello deployed entities on that node are exposed through queries if hello node is reported as up by System.FM, with hello same instance as hello instance associated with hello entities.</span></span> <span data-ttu-id="3e896-147">När System.FM rapporterar hello noden är nere eller startas om (en ny instans), rensas hello hälsoarkivet automatiskt hello distribueras entiteter som kan finnas endast på hello ned noden eller på hello tidigare instans av hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-147">When System.FM reports that hello node is down or restarted (a new instance), hello health store automatically cleans up hello deployed entities that can exist only on hello down node or on hello previous instance of hello node.</span></span>

* <span data-ttu-id="3e896-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="3e896-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="3e896-149">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-149">**Property**: State</span></span>
* <span data-ttu-id="3e896-150">**Nästa steg**: om hello nod avser en uppgradering, det ska gå tillbaka när den har uppgraderats.</span><span class="sxs-lookup"><span data-stu-id="3e896-150">**Next steps**: If hello node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="3e896-151">I det här fallet bör hello hälsotillstånd växla tillbaka tooOK.</span><span class="sxs-lookup"><span data-stu-id="3e896-151">In this case, hello health state should switch back tooOK.</span></span> <span data-ttu-id="3e896-152">Om hello noden inte gå tillbaka eller om det misslyckas måste hello problemet mer undersökning.</span><span class="sxs-lookup"><span data-stu-id="3e896-152">If hello node doesn't come back or it fails, hello problem needs more investigation.</span></span>

<span data-ttu-id="3e896-153">hello visas följande exempel hello System.FM händelse med ett hälsotillstånd OK för noden:</span><span class="sxs-lookup"><span data-stu-id="3e896-153">hello following example shows hello System.FM event with a health state of OK for node up:</span></span>

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


### <a name="certificate-expiration"></a><span data-ttu-id="3e896-154">Certifikatet upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="3e896-154">Certificate expiration</span></span>
<span data-ttu-id="3e896-155">**System.FabricNode** rapporterar en varning när certifikat som används av hello nod är nära förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="3e896-155">**System.FabricNode** reports a warning when certificates used by hello node are near expiration.</span></span> <span data-ttu-id="3e896-156">Det finns tre certifikat per nod: **Certificate_cluster**, **Certificate_server**, och **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="3e896-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="3e896-157">När hello förfallodatum är minst två veckor, är hälsotillståndet för hello rapporten OK.</span><span class="sxs-lookup"><span data-stu-id="3e896-157">When hello expiration is at least two weeks away, hello report health state is OK.</span></span> <span data-ttu-id="3e896-158">När hello upphör att gälla inom två veckor är hello rapporttypen en varning.</span><span class="sxs-lookup"><span data-stu-id="3e896-158">When hello expiration is within two weeks, hello report type is a warning.</span></span> <span data-ttu-id="3e896-159">TTL-värde på dessa händelser är oändligt och de tas bort när en nod lämnar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3e896-159">TTL of these events is infinite, and they are removed when a node leaves hello cluster.</span></span>

* <span data-ttu-id="3e896-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="3e896-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="3e896-161">**Egenskapen**: börjar med **certifikat** och innehåller mer information om hello certifikattyp</span><span class="sxs-lookup"><span data-stu-id="3e896-161">**Property**: Starts with **Certificate** and contains more information about hello certificate type</span></span>
* <span data-ttu-id="3e896-162">**Nästa steg**: uppdatera hello certifikat om de är nära förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="3e896-162">**Next steps**: Update hello certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="3e896-163">Läsa in kapacitetsöverträdelse</span><span class="sxs-lookup"><span data-stu-id="3e896-163">Load capacity violation</span></span>
<span data-ttu-id="3e896-164">hello Service Fabric-Belastningsutjämnarens rapporterar en varning när den identifierar en kapacitetsöverträdelse för noden.</span><span class="sxs-lookup"><span data-stu-id="3e896-164">hello Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="3e896-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="3e896-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="3e896-166">**Egenskapen**: börjar med **kapacitet**</span><span class="sxs-lookup"><span data-stu-id="3e896-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="3e896-167">**Nästa steg**: Kontrollera tillhandahålls mått och visa hello aktuella kapaciteten på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-167">**Next steps**: Check provided metrics and view hello current capacity on hello node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="3e896-168">Programmet system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-168">Application system health reports</span></span>
<span data-ttu-id="3e896-169">**System.CM**, som representerar hello Cluster Manager-tjänsten är hello utfärdare som hanterar information om ett program.</span><span class="sxs-lookup"><span data-stu-id="3e896-169">**System.CM**, which represents hello Cluster Manager service, is hello authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="3e896-170">Status</span><span class="sxs-lookup"><span data-stu-id="3e896-170">State</span></span>
<span data-ttu-id="3e896-171">System.CM rapporterar som OK när programmet hello har skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="3e896-171">System.CM reports as OK when hello application has been created or updated.</span></span> <span data-ttu-id="3e896-172">Informerar hello hälsoarkivet när programmet hello har tagits bort, så att den kan tas bort från store.</span><span class="sxs-lookup"><span data-stu-id="3e896-172">It informs hello health store when hello application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="3e896-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="3e896-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="3e896-174">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-174">**Property**: State</span></span>
* <span data-ttu-id="3e896-175">**Nästa steg**: om programmet hello har skapats eller uppdaterats, ska det finnas hello Klusterhanterare hälsorapport.</span><span class="sxs-lookup"><span data-stu-id="3e896-175">**Next steps**: If hello application has been created or updated, it should include hello Cluster Manager health report.</span></span> <span data-ttu-id="3e896-176">I annat fall söker hello tillståndet för hello-programmet genom att utfärda en fråga (till exempel hello PowerShell-cmdleten **ServiceFabricApplication Get - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="3e896-176">Otherwise, check hello state of hello application by issuing a query (for example, hello PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="3e896-177">hello följande exempel visar hello händelsen på hello **fabric: / WordCount** program:</span><span class="sxs-lookup"><span data-stu-id="3e896-177">hello following example shows hello state event on hello **fabric:/WordCount** application:</span></span>

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

## <a name="service-system-health-reports"></a><span data-ttu-id="3e896-178">Tjänsten system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-178">Service system health reports</span></span>
<span data-ttu-id="3e896-179">**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e896-179">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="3e896-180">Status</span><span class="sxs-lookup"><span data-stu-id="3e896-180">State</span></span>
<span data-ttu-id="3e896-181">System.FM rapporterar som OK när hello-tjänsten har skapats.</span><span class="sxs-lookup"><span data-stu-id="3e896-181">System.FM reports as OK when hello service has been created.</span></span> <span data-ttu-id="3e896-182">Det tar bort hello entitet från hello health store när hello-tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="3e896-182">It deletes hello entity from hello health store when hello service has been deleted.</span></span>

* <span data-ttu-id="3e896-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="3e896-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="3e896-184">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-184">**Property**: State</span></span>

<span data-ttu-id="3e896-185">hello följande exempel visar hello händelsen hello tjänsten **fabric: / WordCount/WordCountWebService**:</span><span class="sxs-lookup"><span data-stu-id="3e896-185">hello following example shows hello state event on hello service **fabric:/WordCount/WordCountWebService**:</span></span>

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

### <a name="service-correlation-error"></a><span data-ttu-id="3e896-186">Fel i tjänsten korrelation</span><span class="sxs-lookup"><span data-stu-id="3e896-186">Service correlation error</span></span>
<span data-ttu-id="3e896-187">**System.PLB** rapporterar ett fel när den identifierar att uppdatera en tjänst toobe korrelerade med en annan tjänst skapas en tillhörighetskedja.</span><span class="sxs-lookup"><span data-stu-id="3e896-187">**System.PLB** reports an error when it detects that updating a service toobe correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="3e896-188">hello rapporten rensas när lyckade uppdatering sker.</span><span class="sxs-lookup"><span data-stu-id="3e896-188">hello report is cleared when successful update happens.</span></span>

* <span data-ttu-id="3e896-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="3e896-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="3e896-190">**Egenskapen**: ServiceDescription</span><span class="sxs-lookup"><span data-stu-id="3e896-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="3e896-191">**Nästa steg**: Kontrollera hello korrelerade beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="3e896-191">**Next steps**: Check hello correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="3e896-192">Partitionen system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-192">Partition system health reports</span></span>
<span data-ttu-id="3e896-193">**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om tjänsten partitioner.</span><span class="sxs-lookup"><span data-stu-id="3e896-193">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="3e896-194">Status</span><span class="sxs-lookup"><span data-stu-id="3e896-194">State</span></span>
<span data-ttu-id="3e896-195">System.FM rapporterar som OK när hello partition har skapats och är felfri.</span><span class="sxs-lookup"><span data-stu-id="3e896-195">System.FM reports as OK when hello partition has been created and is healthy.</span></span> <span data-ttu-id="3e896-196">Det tar bort hello entitet från hello health store när hello partition tas bort.</span><span class="sxs-lookup"><span data-stu-id="3e896-196">It deletes hello entity from hello health store when hello partition is deleted.</span></span>

<span data-ttu-id="3e896-197">Om hello partitionen är mindre än hello minsta antal, rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-197">If hello partition is below hello minimum replica count, it reports an error.</span></span> <span data-ttu-id="3e896-198">Om hello partitionen är inte mindre än hello minsta antal, men det är mindre än hello mål replik antal, rapporterar en varning.</span><span class="sxs-lookup"><span data-stu-id="3e896-198">If hello partition is not below hello minimum replica count, but it is below hello target replica count, it reports a warning.</span></span> <span data-ttu-id="3e896-199">Om hello partitionen förlorar kvorum, rapporterar System.FM ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-199">If hello partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="3e896-200">Andra viktiga händelser inkludera en varning när hello omkonfiguration tar längre tid än förväntat och när hello build tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="3e896-200">Other important events include a warning when hello reconfiguration takes longer than expected and when hello build takes longer than expected.</span></span> <span data-ttu-id="3e896-201">hello förväntade tider för hello bygg- och omkonfiguration kan konfigureras baserat på tjänsten scenarier.</span><span class="sxs-lookup"><span data-stu-id="3e896-201">hello expected times for hello build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="3e896-202">Till exempel om en tjänst har en terabyte tillstånd, till exempel SQL-databas tar hello build längre tid än för en tjänst med en liten mängd tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e896-202">For example, if a service has a terabyte of state, such as SQL Database, hello build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="3e896-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="3e896-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="3e896-204">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-204">**Property**: State</span></span>
* <span data-ttu-id="3e896-205">**Nästa steg**: om hello hälsotillstånd inte är OK, är det möjligt att vissa repliker inte har skapats, öppna eller upphöjt tooprimary eller sekundär korrekt.</span><span class="sxs-lookup"><span data-stu-id="3e896-205">**Next steps**: If hello health state is not OK, it's possible that some replicas have not been created, opened, or promoted tooprimary or secondary correctly.</span></span> <span data-ttu-id="3e896-206">I många fall är hello orsaken ett service-fel i hello öppna eller ändra roll implementering.</span><span class="sxs-lookup"><span data-stu-id="3e896-206">In many instances, hello root cause is a service bug in hello open or change-role implementation.</span></span>

<span data-ttu-id="3e896-207">hello som följande exempel visar en felfri partition:</span><span class="sxs-lookup"><span data-stu-id="3e896-207">hello following example shows a healthy partition:</span></span>

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

<span data-ttu-id="3e896-208">hello visar följande exempel hello hälsotillståndet för en partition som är lägre än målet replikantalet.</span><span class="sxs-lookup"><span data-stu-id="3e896-208">hello following example shows hello health of a partition that is below target replica count.</span></span> <span data-ttu-id="3e896-209">hello nästa steg är tooget hello partition beskrivning, som visar hur den är konfigurerad: **MinReplicaSetSize** är tre och **TargetReplicaSetSize** sju.</span><span class="sxs-lookup"><span data-stu-id="3e896-209">hello next step is tooget hello partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="3e896-210">Hämta sedan hello antalet noder i klustret hello: fem.</span><span class="sxs-lookup"><span data-stu-id="3e896-210">Then get hello number of nodes in hello cluster: five.</span></span> <span data-ttu-id="3e896-211">Så i det här fallet två repliker kan inte placeras eftersom hello mål antal repliker är högre än hello antalet noder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="3e896-211">So in this case, two replicas can't be placed because hello target number of replicas is higher than hello number of nodes available.</span></span>

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
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
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

### <a name="replica-constraint-violation"></a><span data-ttu-id="3e896-212">Replik begränsningsfel</span><span class="sxs-lookup"><span data-stu-id="3e896-212">Replica constraint violation</span></span>
<span data-ttu-id="3e896-213">**System.PLB** rapporterar en varning om den identifierar en replik begränsningsfel och går inte att placera alla partition repliker.</span><span class="sxs-lookup"><span data-stu-id="3e896-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="3e896-214">hello rapportinformationen visar vilka begränsningar och egenskaper förhindra hello replik placering.</span><span class="sxs-lookup"><span data-stu-id="3e896-214">hello report details show which constraints and properties prevent hello replica placement.</span></span>

* <span data-ttu-id="3e896-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="3e896-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="3e896-216">**Egenskapen**: börjar med **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="3e896-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="3e896-217">Replik system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-217">Replica system health reports</span></span>
<span data-ttu-id="3e896-218">**System.RA**, som representerar hello omkonfiguration agent-komponenten är hello utfärdare för hello replikens tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e896-218">**System.RA**, which represents hello reconfiguration agent component, is hello authority for hello replica state.</span></span>

### <a name="state"></a><span data-ttu-id="3e896-219">Status</span><span class="sxs-lookup"><span data-stu-id="3e896-219">State</span></span>
<span data-ttu-id="3e896-220">**System.RA** rapporterar OK när hello repliken har skapats.</span><span class="sxs-lookup"><span data-stu-id="3e896-220">**System.RA** reports OK when hello replica has been created.</span></span>

* <span data-ttu-id="3e896-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="3e896-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="3e896-222">**Egenskapen**: tillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-222">**Property**: State</span></span>

<span data-ttu-id="3e896-223">hello som följande exempel visar en felfri replik:</span><span class="sxs-lookup"><span data-stu-id="3e896-223">hello following example shows a healthy replica:</span></span>

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

### <a name="replica-open-status"></a><span data-ttu-id="3e896-224">Replik öppen status</span><span class="sxs-lookup"><span data-stu-id="3e896-224">Replica open status</span></span>
<span data-ttu-id="3e896-225">hello beskrivning av den här hälsorapport innehåller hello Starttid (Coordinated Universal Time) när hello API-anrop anropades.</span><span class="sxs-lookup"><span data-stu-id="3e896-225">hello description of this health report contains hello start time (Coordinated Universal Time) when hello API call was invoked.</span></span>

<span data-ttu-id="3e896-226">**System.RA** rapporterar en varning om hello replik öppna tar längre tid än hello konfigurerats period (standard: 30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3e896-226">**System.RA** reports a warning if hello replica open takes longer than hello configured period (default: 30 minutes).</span></span> <span data-ttu-id="3e896-227">Om hello API påverkar tjänsttillgänglighet, utfärdas hello rapporten mycket snabbare (en konfigurerbar intervall, med en standard på 30 sekunder).</span><span class="sxs-lookup"><span data-stu-id="3e896-227">If hello API impacts service availability, hello report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="3e896-228">hello tid, mätt innehåller hello tidsåtgång för hello replikatorn öppna och hello-tjänst som är öppna.</span><span class="sxs-lookup"><span data-stu-id="3e896-228">hello time measured includes hello time taken for hello replicator open and hello service open.</span></span> <span data-ttu-id="3e896-229">hello egenskapen ändringar tooOK om hello öppnar har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3e896-229">hello property changes tooOK if hello open completes.</span></span>

* <span data-ttu-id="3e896-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="3e896-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="3e896-231">**Egenskapen**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="3e896-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="3e896-232">**Nästa steg**: om hello hälsotillstånd inte är OK, undersöka varför hello replik öppna tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="3e896-232">**Next steps**: If hello health state is not OK, investigate why hello replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="3e896-233">Långsam service API-anrop</span><span class="sxs-lookup"><span data-stu-id="3e896-233">Slow service API call</span></span>
<span data-ttu-id="3e896-234">**System.RAP** och **System.Replicator** rapportera en varning om en anropet toohello service användarkod tar längre tid än hello konfigurerad tid.</span><span class="sxs-lookup"><span data-stu-id="3e896-234">**System.RAP** and **System.Replicator** report a warning if a call toohello user service code takes longer than hello configured time.</span></span> <span data-ttu-id="3e896-235">hello varning rensas när hello anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3e896-235">hello warning is cleared when hello call completes.</span></span>

* <span data-ttu-id="3e896-236">**SourceId**: System.RAP eller System.Replicator</span><span class="sxs-lookup"><span data-stu-id="3e896-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="3e896-237">**Egenskapen**: hello namnet på hello långsam API.</span><span class="sxs-lookup"><span data-stu-id="3e896-237">**Property**: hello name of hello slow API.</span></span> <span data-ttu-id="3e896-238">hello beskrivningen innehåller mer information om hello tid hello API har väntande.</span><span class="sxs-lookup"><span data-stu-id="3e896-238">hello description provides more details about hello time hello API has been pending.</span></span>
* <span data-ttu-id="3e896-239">**Nästa steg**: undersöka varför hello anropet tar längre tid än förväntat.</span><span class="sxs-lookup"><span data-stu-id="3e896-239">**Next steps**: Investigate why hello call takes longer than expected.</span></span>

<span data-ttu-id="3e896-240">hello som följande exempel visar en partition i förlorar kvorum och hello undersökningen steg klar toofigure reda på varför.</span><span class="sxs-lookup"><span data-stu-id="3e896-240">hello following example shows a partition in quorum loss, and hello investigation steps done toofigure out why.</span></span> <span data-ttu-id="3e896-241">En av hello repliker har ett varningshälsotillstånd så att du får dess hälsa.</span><span class="sxs-lookup"><span data-stu-id="3e896-241">One of hello replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="3e896-242">Det visar att hello tjänståtgärd tar längre tid än väntat, en händelse som rapporterats av System.RAP.</span><span class="sxs-lookup"><span data-stu-id="3e896-242">It shows that hello service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="3e896-243">När informationen har tagits emot, hello nästa steg är toolook på hello service-koden och undersöka det.</span><span class="sxs-lookup"><span data-stu-id="3e896-243">After this information is received, hello next step is toolook at hello service code and investigate there.</span></span> <span data-ttu-id="3e896-244">Det här fallet hello **RunAsync** implementering av hello tillståndskänslig service utlöser ett undantag.</span><span class="sxs-lookup"><span data-stu-id="3e896-244">For this case, hello **RunAsync** implementation of hello stateful service throws an unhandled exception.</span></span> <span data-ttu-id="3e896-245">hello repliker omarbetning så att du inte kan se alla repliker i hello varningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e896-245">hello replicas are recycling, so you may not see any replicas in hello warning state.</span></span> <span data-ttu-id="3e896-246">Du kan försök hämtning hello hälsotillstånd och söka efter skillnader i hello replik-ID.</span><span class="sxs-lookup"><span data-stu-id="3e896-246">You can retry getting hello health state and look for any differences in hello replica ID.</span></span> <span data-ttu-id="3e896-247">I vissa fall kan hello återförsök ge ledtrådar.</span><span class="sxs-lookup"><span data-stu-id="3e896-247">In certain cases, hello retries can give you clues.</span></span>

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

<span data-ttu-id="3e896-248">När du startar hello felande program under hello felsökare visar hello diagnostiska händelser windows hello undantag utlöstes från RunAsync:</span><span class="sxs-lookup"><span data-stu-id="3e896-248">When you start hello faulty application under hello debugger, hello diagnostic events windows show hello exception thrown from RunAsync:</span></span>

![Visual Studio 2015 diagnostiska händelser: RunAsync fel i fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="3e896-250">Visual Studio 2015 diagnostiska händelser: RunAsync fel i **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="3e896-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="3e896-251">Replikeringskön är full</span><span class="sxs-lookup"><span data-stu-id="3e896-251">Replication queue full</span></span>
<span data-ttu-id="3e896-252">**System.Replicator** rapporterar en varning när hello Replikeringskön är full.</span><span class="sxs-lookup"><span data-stu-id="3e896-252">**System.Replicator** reports a warning when hello replication queue is full.</span></span> <span data-ttu-id="3e896-253">På primära hello blir Replikeringskön vanligtvis full eftersom en eller flera sekundära repliker är långsam tooacknowledge åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e896-253">On hello primary, replication queue usually becomes full because one or more secondary replicas are slow tooacknowledge operations.</span></span> <span data-ttu-id="3e896-254">På sekundära hello inträffar detta när hello-tjänsten är långsam tooapply hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e896-254">On hello secondary, this usually happens when hello service is slow tooapply hello operations.</span></span> <span data-ttu-id="3e896-255">hello varning rensas när hello kön är inte längre fullständig.</span><span class="sxs-lookup"><span data-stu-id="3e896-255">hello warning is cleared when hello queue is no longer full.</span></span>

* <span data-ttu-id="3e896-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="3e896-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="3e896-257">**Egenskapen**: **PrimaryReplicationQueueStatus** eller **SecondaryReplicationQueueStatus**, beroende på hello replikroll</span><span class="sxs-lookup"><span data-stu-id="3e896-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on hello replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="3e896-258">Långsam Naming åtgärder</span><span class="sxs-lookup"><span data-stu-id="3e896-258">Slow Naming operations</span></span>
<span data-ttu-id="3e896-259">**System.NamingService** rapporterar hälsotillståndet för dess primära repliken när en Naming åtgärden tar längre tid än godkända.</span><span class="sxs-lookup"><span data-stu-id="3e896-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="3e896-260">Exempel på Naming operations är [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) eller [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="3e896-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="3e896-261">Flera metoder kan hittas under FabricClient, till exempel [tjänsten metoder för hantering av](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) eller [metoder för hantering av egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="3e896-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="3e896-262">hello Naming service matchar namnen tooa plats i hello klustret och gör att användare toomanage Tjänstenamn och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3e896-262">hello Naming service resolves service names tooa location in hello cluster and enables users toomanage service names and properties.</span></span> <span data-ttu-id="3e896-263">Det är ett Service Fabric partitionerad beständiga tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e896-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="3e896-264">En av hello partitioner representerar hello Authority Owner som innehåller metadata om alla Service Fabric-namn och tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e896-264">One of hello partitions represents hello Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="3e896-265">hello Service Fabric-namn är mappade toodifferent partitioner, kallas Name Owner partitioner så hello-tjänsten kan utökas.</span><span class="sxs-lookup"><span data-stu-id="3e896-265">hello Service Fabric names are mapped toodifferent partitions, called Name Owner partitions, so hello service is extensible.</span></span> <span data-ttu-id="3e896-266">Läs mer om [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="3e896-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="3e896-267">När en Naming åtgärden tar längre tid än förväntat hello åtgärden har flaggats med en varning-rapport på hello *primära repliken av hello Naming service partition som fungerar hello åtgärden*.</span><span class="sxs-lookup"><span data-stu-id="3e896-267">When a Naming operation takes longer than expected, hello operation is flagged with a Warning report on hello *primary replica of hello Naming service partition that serves hello operation*.</span></span> <span data-ttu-id="3e896-268">Om hello-åtgärden har slutförts, är hello varning avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="3e896-268">If hello operation completes successfully, hello Warning is cleared.</span></span> <span data-ttu-id="3e896-269">Om hello-åtgärden har slutförts med ett fel, innehåller hello hälsa rapporten information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-269">If hello operation completes with an error, hello health report includes details about hello error.</span></span>

* <span data-ttu-id="3e896-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="3e896-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="3e896-271">**Egenskapen**: börjar med prefixet **Duration_** och identifierar hello går långsamt och hello Service Fabric-namnet på vilka hello åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="3e896-271">**Property**: Starts with prefix **Duration_** and identifies hello slow operation and hello Service Fabric name on which hello operation is applied.</span></span> <span data-ttu-id="3e896-272">Till exempel om skapar tjänst med namnet fabric: / MyApp/MyService tar för lång tid, hello-egenskapen är Duration_AOCreateService.fabric:/MyApp/MyService.</span><span class="sxs-lookup"><span data-stu-id="3e896-272">For example, if create service at name fabric:/MyApp/MyService takes too long, hello property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="3e896-273">AO punkter toohello roll hello Naming partition för det här namnet och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="3e896-273">AO points toohello role of hello Naming partition for this name and operation.</span></span>
* <span data-ttu-id="3e896-274">**Nästa steg**: Kontrollera varför hello Naming åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="3e896-274">**Next steps**: Check why hello Naming operation fails.</span></span> <span data-ttu-id="3e896-275">Varje åtgärd kan ha olika rotorsaker.</span><span class="sxs-lookup"><span data-stu-id="3e896-275">Each operation can have different root causes.</span></span> <span data-ttu-id="3e896-276">Till exempel ta bort tjänsten kan ha fastnat på en nod eftersom hello programvärd håller kraschar på en nod på grund av tooa användaren programfel i hello service-kod.</span><span class="sxs-lookup"><span data-stu-id="3e896-276">For example, delete service may be stuck on a node because hello application host keeps crashing on a node due tooa user bug in hello service code.</span></span>

<span data-ttu-id="3e896-277">hello som följande exempel visar en åtgärd för att skapa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e896-277">hello following example shows a create service operation.</span></span> <span data-ttu-id="3e896-278">hello-åtgärden tog längre tid än hello konfigurerats varaktighet.</span><span class="sxs-lookup"><span data-stu-id="3e896-278">hello operation took longer than hello configured duration.</span></span> <span data-ttu-id="3e896-279">AO återförsök och skickar tooNO arbete.</span><span class="sxs-lookup"><span data-stu-id="3e896-279">AO retries and sends work tooNO.</span></span> <span data-ttu-id="3e896-280">Inga senaste åtgärden slutförts hello med Timeout.</span><span class="sxs-lookup"><span data-stu-id="3e896-280">NO completed hello last operation with Timeout.</span></span> <span data-ttu-id="3e896-281">I det här fallet är hello samma replik primär för både hello AO och inga roller.</span><span class="sxs-lookup"><span data-stu-id="3e896-281">In this case, hello same replica is primary for both hello AO and NO roles.</span></span>

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
                        Description           : hello AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
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
                        Description           : hello NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="3e896-282">DeployedApplication system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="3e896-283">**System.Hosting** är hello auktoritet på distribuerade entiteter.</span><span class="sxs-lookup"><span data-stu-id="3e896-283">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="3e896-284">Aktivering</span><span class="sxs-lookup"><span data-stu-id="3e896-284">Activation</span></span>
<span data-ttu-id="3e896-285">System.Hosting rapporterar som OK när ett program har aktiverats på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-285">System.Hosting reports as OK when an application has been successfully activated on hello node.</span></span> <span data-ttu-id="3e896-286">Annars rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="3e896-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-288">**Egenskapen**: aktivering, inklusive hello distributionen version</span><span class="sxs-lookup"><span data-stu-id="3e896-288">**Property**: Activation, including hello rollout version</span></span>
* <span data-ttu-id="3e896-289">**Nästa steg**: om hello program inte är felfritt, undersöka varför hello-aktiveringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="3e896-289">**Next steps**: If hello application is unhealthy, investigate why hello activation failed.</span></span>

<span data-ttu-id="3e896-290">hello visar följande exempel lyckad aktivering:</span><span class="sxs-lookup"><span data-stu-id="3e896-290">hello following example shows successful activation:</span></span>

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
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="3e896-291">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="3e896-291">Download</span></span>
<span data-ttu-id="3e896-292">**System.Hosting** rapporterar ett fel om inte hello programmet paketet ned.</span><span class="sxs-lookup"><span data-stu-id="3e896-292">**System.Hosting** reports an error if hello application package download fails.</span></span>

* <span data-ttu-id="3e896-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-294">**Egenskapen**:  **hämta:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="3e896-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="3e896-295">**Nästa steg**: undersöka varför hello hämtning misslyckades på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-295">**Next steps**: Investigate why hello download failed on hello node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="3e896-296">DeployedServicePackage system hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="3e896-297">**System.Hosting** är hello auktoritet på distribuerade entiteter.</span><span class="sxs-lookup"><span data-stu-id="3e896-297">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="3e896-298">Aktivering av Service-paket</span><span class="sxs-lookup"><span data-stu-id="3e896-298">Service package activation</span></span>
<span data-ttu-id="3e896-299">System.Hosting rapporter som OK om hello service paketet aktiveringen på hello nod har genomförts.</span><span class="sxs-lookup"><span data-stu-id="3e896-299">System.Hosting reports as OK if hello service package activation on hello node is successful.</span></span> <span data-ttu-id="3e896-300">Annars rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="3e896-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-302">**Egenskapen**: aktivering</span><span class="sxs-lookup"><span data-stu-id="3e896-302">**Property**: Activation</span></span>
* <span data-ttu-id="3e896-303">**Nästa steg**: undersöka varför hello-aktiveringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="3e896-303">**Next steps**: Investigate why hello activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="3e896-304">Aktivering av kod paketet</span><span class="sxs-lookup"><span data-stu-id="3e896-304">Code package activation</span></span>
<span data-ttu-id="3e896-305">**System.Hosting** rapporter som OK för varje kodpaketet om hello aktiveringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="3e896-305">**System.Hosting** reports as OK for each code package if hello activation is successful.</span></span> <span data-ttu-id="3e896-306">Om hello aktiveringen misslyckas rapporterar en varning som konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="3e896-306">If hello activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="3e896-307">Om **CodePackage** misslyckas tooactivate eller avslutas med ett fel som är större än konfigurerad hello **CodePackageHealthErrorThreshold**, värd rapporterar ett fel.</span><span class="sxs-lookup"><span data-stu-id="3e896-307">If **CodePackage** fails tooactivate or terminates with an error greater than hello configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="3e896-308">Om en tjänstepaketet innehåller flera kod paket, genereras en aktivering-rapporten för varje kriterium.</span><span class="sxs-lookup"><span data-stu-id="3e896-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="3e896-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-310">**Egenskapen**: använder hello prefixet **CodePackageActivation** och innehåller hello namn hello kodpaketet och hello startpunkt som  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (till exempel **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="3e896-310">**Property**: Uses hello prefix **CodePackageActivation** and contains hello name of hello code package and hello entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="3e896-311">Typ av registrering</span><span class="sxs-lookup"><span data-stu-id="3e896-311">Service type registration</span></span>
<span data-ttu-id="3e896-312">**System.Hosting** rapporterar som OK om hello tjänsttypen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="3e896-312">**System.Hosting** reports as OK if hello service type has been registered successfully.</span></span> <span data-ttu-id="3e896-313">Ett fel rapporteras om hello inte registrerats i tid (som konfigurerats med hjälp av **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="3e896-313">It reports an error if hello registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="3e896-314">Om hello runtime är stängd hello service type har avregistrerats hello nod och värd rapporter en varning.</span><span class="sxs-lookup"><span data-stu-id="3e896-314">If hello runtime is closed, hello service type is unregistered from hello node and Hosting reports a warning.</span></span>

* <span data-ttu-id="3e896-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-316">**Egenskapen**: använder hello prefixet **ServiceTypeRegistration** och innehåller hello tjänstnamnet typen (till exempel **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="3e896-316">**Property**: Uses hello prefix **ServiceTypeRegistration** and contains hello service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="3e896-317">hello som följande exempel visar en felfri distribuerat tjänstpaket:</span><span class="sxs-lookup"><span data-stu-id="3e896-317">hello following example shows a healthy deployed service package:</span></span>

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
                             Description           : hello ServicePackage was activated successfully.
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
                             Description           : hello CodePackage was activated successfully.
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
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="3e896-318">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="3e896-318">Download</span></span>
<span data-ttu-id="3e896-319">**System.Hosting** rapporterar ett fel om inte hello service paketet ned.</span><span class="sxs-lookup"><span data-stu-id="3e896-319">**System.Hosting** reports an error if hello service package download fails.</span></span>

* <span data-ttu-id="3e896-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-321">**Egenskapen**:  **hämta:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="3e896-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="3e896-322">**Nästa steg**: undersöka varför hello hämtning misslyckades på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-322">**Next steps**: Investigate why hello download failed on hello node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="3e896-323">Validering av uppgradering</span><span class="sxs-lookup"><span data-stu-id="3e896-323">Upgrade validation</span></span>
<span data-ttu-id="3e896-324">**System.Hosting** rapporterar ett fel om valideringen under hello uppgraderingen misslyckas eller om hello uppgraderingen misslyckas på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="3e896-324">**System.Hosting** reports an error if validation during hello upgrade fails or if hello upgrade fails on hello node.</span></span>

* <span data-ttu-id="3e896-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="3e896-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="3e896-326">**Egenskapen**: använder hello prefixet **FabricUpgradeValidation** och innehåller hello uppgraderingsversion</span><span class="sxs-lookup"><span data-stu-id="3e896-326">**Property**: Uses hello prefix **FabricUpgradeValidation** and contains hello upgrade version</span></span>
* <span data-ttu-id="3e896-327">**Beskrivning**: pekar toohello fel uppstod</span><span class="sxs-lookup"><span data-stu-id="3e896-327">**Description**: Points toohello error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e896-328">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e896-328">Next steps</span></span>
[<span data-ttu-id="3e896-329">Visa Service Fabric-hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="3e896-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="3e896-330">Hur tooreport och kontrollera-tjänsten för hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="3e896-330">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="3e896-331">Övervaka och diagnostisera tjänster lokalt</span><span class="sxs-lookup"><span data-stu-id="3e896-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="3e896-332">Uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="3e896-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

