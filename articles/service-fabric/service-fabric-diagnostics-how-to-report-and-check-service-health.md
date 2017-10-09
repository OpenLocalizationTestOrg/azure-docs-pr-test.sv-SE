---
title: "hälsa för aaaReport och kontrollera med Azure Service Fabric | Microsoft Docs"
description: "Lär dig hur toosend hälsorapporter från koden för tjänsten och hur toocheck hello hälsotillståndet för din tjänst med hjälp av hello hälsoövervakning verktyg som Azure Service Fabric tillhandahåller."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: bcb838fefe3f2054447e1731d709e455560260e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="report-and-check-service-health"></a><span data-ttu-id="691b6-103">Rapportera och kontrollera hälsan hos tjänster</span><span class="sxs-lookup"><span data-stu-id="691b6-103">Report and check service health</span></span>
<span data-ttu-id="691b6-104">När dina tjänster har problem, beror din möjlighet toorespond tooand korrigering incidenter och avbrott på din möjlighet toodetect hello problem snabbt.</span><span class="sxs-lookup"><span data-stu-id="691b6-104">When your services encounter problems, your ability toorespond tooand fix incidents and outages depends on your ability toodetect hello issues quickly.</span></span> <span data-ttu-id="691b6-105">Om du rapportera problem och fel toohello Azure Service Fabric health manager från service koden använder du standard hälsoövervakning verktyg för att Service Fabric tillhandahåller toocheck hello hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="691b6-105">If you report problems and failures toohello Azure Service Fabric health manager from your service code, you can use standard health monitoring tools that Service Fabric provides toocheck hello health status.</span></span>

<span data-ttu-id="691b6-106">Det finns tre sätt att du kan rapportera hälsa från hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="691b6-106">There are three ways that you can report health from hello service:</span></span>

* <span data-ttu-id="691b6-107">Använd [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) eller [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objekt.</span><span class="sxs-lookup"><span data-stu-id="691b6-107">Use [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) or [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objects.</span></span>  
  <span data-ttu-id="691b6-108">Du kan använda hello `Partition` och `CodePackageActivationContext` objekt tooreport hello hälsotillståndet för element som ingår i hello aktuell kontext.</span><span class="sxs-lookup"><span data-stu-id="691b6-108">You can use hello `Partition` and `CodePackageActivationContext` objects tooreport hello health of elements that are part of hello current context.</span></span> <span data-ttu-id="691b6-109">Kod som körs som en del av en replik kan rapportera hälsa endast på den repliken och hello-partition som den tillhör hello-program som den är en del av.</span><span class="sxs-lookup"><span data-stu-id="691b6-109">For example, code that runs as part of a replica can report health only on that replica, hello partition that it belongs to, and hello application that it is a part of.</span></span>
* <span data-ttu-id="691b6-110">Använd `FabricClient`.</span><span class="sxs-lookup"><span data-stu-id="691b6-110">Use `FabricClient`.</span></span>   
  <span data-ttu-id="691b6-111">Du kan använda `FabricClient` tooreport hälsa från hello service-kod om hello klustret inte [säker](service-fabric-cluster-security.md) eller om hello-tjänsten körs med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="691b6-111">You can use `FabricClient` tooreport health from hello service code if hello cluster is not [secure](service-fabric-cluster-security.md) or if hello service is running with admin privileges.</span></span> <span data-ttu-id="691b6-112">De flesta verkliga scenarier inte använder oskyddade kluster, eller ange admin-privilegier.</span><span class="sxs-lookup"><span data-stu-id="691b6-112">Most real-world scenarios do not use unsecured clusters, or provide admin privileges.</span></span> <span data-ttu-id="691b6-113">Med `FabricClient`, kan du rapportera hälsotillstånd på en enhet som är en del av hello klustret.</span><span class="sxs-lookup"><span data-stu-id="691b6-113">With `FabricClient`, you can report health on any entity that is a part of hello cluster.</span></span> <span data-ttu-id="691b6-114">Vi rekommenderar dock bör Tjänstkod bara skicka rapporter som är relaterade tooits egen hälsa.</span><span class="sxs-lookup"><span data-stu-id="691b6-114">Ideally, however, service code should only send reports that are related tooits own health.</span></span>
* <span data-ttu-id="691b6-115">Använd hello REST API: er på hello kluster, program, distribuerat program, tjänst, servicepaket, partition, replik eller nod nivåer.</span><span class="sxs-lookup"><span data-stu-id="691b6-115">Use hello REST APIs at hello cluster, application, deployed application, service, service package, partition, replica, or node levels.</span></span> <span data-ttu-id="691b6-116">Detta kan vara används tooreport hälsa från i en behållare.</span><span class="sxs-lookup"><span data-stu-id="691b6-116">This can be used tooreport health from within a container.</span></span>

<span data-ttu-id="691b6-117">Den här artikeln vägleder dig genom ett exempel som rapporterar hälsotillståndet från hello Tjänstkod.</span><span class="sxs-lookup"><span data-stu-id="691b6-117">This article walks you through an example that reports health from hello service code.</span></span> <span data-ttu-id="691b6-118">hello exemplet visas hur hello verktyg som tillhandahålls av Service Fabric kan vara används toocheck hello hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="691b6-118">hello example also shows how hello tools provided by Service Fabric can be used toocheck hello health status.</span></span> <span data-ttu-id="691b6-119">Den här artikeln är avsedd toobe en snabb introduktion toohello hälsoövervakning funktionerna i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="691b6-119">This article is intended toobe a quick introduction toohello health monitoring capabilities of Service Fabric.</span></span> <span data-ttu-id="691b6-120">Mer detaljerad information kan du läsa hello serie djupgående artiklar om hälsa som börjar med hello länken hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="691b6-120">For more detailed information, you can read hello series of in-depth articles about health that start with hello link at hello end of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="691b6-121">Krav</span><span class="sxs-lookup"><span data-stu-id="691b6-121">Prerequisites</span></span>
<span data-ttu-id="691b6-122">Du måste ha hello följande installerat:</span><span class="sxs-lookup"><span data-stu-id="691b6-122">You must have hello following installed:</span></span>

* <span data-ttu-id="691b6-123">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="691b6-123">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="691b6-124">Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="691b6-124">Service Fabric SDK</span></span>

## <a name="toocreate-a-local-secure-dev-cluster"></a><span data-ttu-id="691b6-125">toocreate ett lokala säker dev-kluster</span><span class="sxs-lookup"><span data-stu-id="691b6-125">toocreate a local secure dev cluster</span></span>
* <span data-ttu-id="691b6-126">Öppna PowerShell med administratörsrättigheter och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="691b6-126">Open PowerShell with admin privileges, and run hello following commands:</span></span>

![Kommandon som visar hur toocreate ett säker dev-kluster](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a><span data-ttu-id="691b6-128">toodeploy ett program och kontrollera dess hälsa</span><span class="sxs-lookup"><span data-stu-id="691b6-128">toodeploy an application and check its health</span></span>
1. <span data-ttu-id="691b6-129">Öppna Visual Studio som administratör.</span><span class="sxs-lookup"><span data-stu-id="691b6-129">Open Visual Studio as an administrator.</span></span>
2. <span data-ttu-id="691b6-130">Skapa ett projekt med hjälp av hello **tillståndskänslig Service** mall.</span><span class="sxs-lookup"><span data-stu-id="691b6-130">Create a project by using hello **Stateful Service** template.</span></span>
   
    ![Skapa ett Service Fabric-program med tillståndskänslig Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. <span data-ttu-id="691b6-132">Tryck på **F5** toorun hello appen i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="691b6-132">Press **F5** toorun hello application in debug mode.</span></span> <span data-ttu-id="691b6-133">hello program är distribuerade toohello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="691b6-133">hello application is deployed toohello local cluster.</span></span>
4. <span data-ttu-id="691b6-134">När programmet hello körs, högerklicka på hello lokala Klusterhanterare ikon i meddelandefältet för hello och välj **hantera lokala klustret** från hello genvägen menyn tooopen Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="691b6-134">After hello application is running, right-click hello Local Cluster Manager icon in hello notification area and select **Manage Local Cluster** from hello shortcut menu tooopen Service Fabric Explorer.</span></span>
   
    ![Öppna Service Fabric Explorer från meddelandefältet](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. <span data-ttu-id="691b6-136">Hej programhälsan ska visas som i den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="691b6-136">hello application health should be displayed as in this image.</span></span> <span data-ttu-id="691b6-137">Hello programmet bör vara felfria utan fel för tillfället.</span><span class="sxs-lookup"><span data-stu-id="691b6-137">At this time, hello application should be healthy with no errors.</span></span>
   
    ![Felfri programmet i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. <span data-ttu-id="691b6-139">Du kan också kontrollera hello hälsotillstånd med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="691b6-139">You can also check hello health by using PowerShell.</span></span> <span data-ttu-id="691b6-140">Du kan använda ```Get-ServiceFabricApplicationHealth``` toocheck ett programs hälsa och du kan använda ```Get-ServiceFabricServiceHealth``` toocheck tjänstens hälsa.</span><span class="sxs-lookup"><span data-stu-id="691b6-140">You can use ```Get-ServiceFabricApplicationHealth``` toocheck an application's health, and you can use ```Get-ServiceFabricServiceHealth``` toocheck a service's health.</span></span> <span data-ttu-id="691b6-141">hello hälsorapport för hello samma program i PowerShell finns i den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="691b6-141">hello health report for hello same application in PowerShell is in this image.</span></span>
   
    ![Felfri program i PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a><span data-ttu-id="691b6-143">tooadd anpassade hälsa händelser tooyour service-kod</span><span class="sxs-lookup"><span data-stu-id="691b6-143">tooadd custom health events tooyour service code</span></span>
<span data-ttu-id="691b6-144">hello Service Fabric-projektmallar i Visual Studio innehåller exempelkod.</span><span class="sxs-lookup"><span data-stu-id="691b6-144">hello Service Fabric project templates in Visual Studio contain sample code.</span></span> <span data-ttu-id="691b6-145">hello följande steg visar hur du kan rapportera anpassade hälsa händelser från koden för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="691b6-145">hello following steps show how you can report custom health events from your service code.</span></span> <span data-ttu-id="691b6-146">Rapporterna visas automatiskt i hello standardverktyg för hälsoövervakning att Service Fabric innehåller till exempel Service Fabric Explorer, hälsotillståndsvy för Azure portal och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="691b6-146">Such reports show up automatically in hello standard tools for health monitoring that Service Fabric provides, such as Service Fabric Explorer, Azure portal health view, and PowerShell.</span></span>

1. <span data-ttu-id="691b6-147">Öppna hello-program som du skapade tidigare i Visual Studio eller skapa ett nytt program med hjälp av hello **tillståndskänslig Service** Visual Studio-mall.</span><span class="sxs-lookup"><span data-stu-id="691b6-147">Reopen hello application that you created previously in Visual Studio, or create a new application by using hello **Stateful Service** Visual Studio template.</span></span>
2. <span data-ttu-id="691b6-148">Öppna hello Stateful1.cs-filen och hitta hello `myDictionary.TryGetValueAsync` -anrop i hello `RunAsync` metod.</span><span class="sxs-lookup"><span data-stu-id="691b6-148">Open hello Stateful1.cs file, and find hello `myDictionary.TryGetValueAsync` call in hello `RunAsync` method.</span></span> <span data-ttu-id="691b6-149">Du kan se att den här metoden returnerar en `result` som innehåller hello aktuellt värde för hello räknaren eftersom hello logik i det här programmet är tookeep ett antal körs.</span><span class="sxs-lookup"><span data-stu-id="691b6-149">You can see that this method returns a `result` that holds hello current value of hello counter because hello key logic in this application is tookeep a count running.</span></span> <span data-ttu-id="691b6-150">Om detta har ett riktigt program och hello bristande resultat visas ett fel, skulle du tooflag händelsen.</span><span class="sxs-lookup"><span data-stu-id="691b6-150">If this were a real application, and if hello lack of result represented a failure, you would want tooflag that event.</span></span>
3. <span data-ttu-id="691b6-151">tooreport hälsohändelse när hello bristande resultat som representerar ett fel, Lägg till hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="691b6-151">tooreport a health event when hello lack of result represents a failure, add hello following steps.</span></span>
   
    <span data-ttu-id="691b6-152">a.</span><span class="sxs-lookup"><span data-stu-id="691b6-152">a.</span></span> <span data-ttu-id="691b6-153">Lägg till hello `System.Fabric.Health` namnområde toohello Stateful1.cs fil.</span><span class="sxs-lookup"><span data-stu-id="691b6-153">Add hello `System.Fabric.Health` namespace toohello Stateful1.cs file.</span></span>
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    <span data-ttu-id="691b6-154">b.</span><span class="sxs-lookup"><span data-stu-id="691b6-154">b.</span></span> <span data-ttu-id="691b6-155">Lägg till följande kod efter hello hello `myDictionary.TryGetValueAsync` anropa</span><span class="sxs-lookup"><span data-stu-id="691b6-155">Add hello following code after hello `myDictionary.TryGetValueAsync` call</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    <span data-ttu-id="691b6-156">Vi rapporterar replik hälsotillstånd eftersom rapporteras från en tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="691b6-156">We report replica health because it's being reported from a stateful service.</span></span> <span data-ttu-id="691b6-157">Hej `HealthInformation` parametern lagrar information om hello hälsa problemet som rapporteras.</span><span class="sxs-lookup"><span data-stu-id="691b6-157">hello `HealthInformation` parameter stores information about hello health issue that's being reported.</span></span>
   
    <span data-ttu-id="691b6-158">Om du har skapat en tillståndslösa tjänsten Använd hello följande kod</span><span class="sxs-lookup"><span data-stu-id="691b6-158">If you had created a stateless service, use hello following code</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. <span data-ttu-id="691b6-159">Om din tjänst körs med administratörsrättigheter inte eller om hello klustret [säker](service-fabric-cluster-security.md), du kan också använda `FabricClient` tooreport hälsa enligt hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="691b6-159">If your service is running with admin privileges or if hello cluster is not [secure](service-fabric-cluster-security.md), you can also use `FabricClient` tooreport health as shown in hello following steps.</span></span>  
   
    <span data-ttu-id="691b6-160">a.</span><span class="sxs-lookup"><span data-stu-id="691b6-160">a.</span></span> <span data-ttu-id="691b6-161">Skapa hello `FabricClient` instans efter hello `var myDictionary` deklaration.</span><span class="sxs-lookup"><span data-stu-id="691b6-161">Create hello `FabricClient` instance after hello `var myDictionary` declaration.</span></span>
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    <span data-ttu-id="691b6-162">b.</span><span class="sxs-lookup"><span data-stu-id="691b6-162">b.</span></span> <span data-ttu-id="691b6-163">Lägg till följande kod efter hello hello `myDictionary.TryGetValueAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="691b6-163">Add hello following code after hello `myDictionary.TryGetValueAsync` call.</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
5. <span data-ttu-id="691b6-164">Vi simulera felet och se den visas i hello hälsa övervakningsverktyg.</span><span class="sxs-lookup"><span data-stu-id="691b6-164">Let's simulate this failure and see it show up in hello health monitoring tools.</span></span> <span data-ttu-id="691b6-165">toosimulate hello fel, kommentera hello första raden i hello hälsotillståndsrapportering kod som du lade till tidigare.</span><span class="sxs-lookup"><span data-stu-id="691b6-165">toosimulate hello failure, comment out hello first line in hello health reporting code that you added earlier.</span></span> <span data-ttu-id="691b6-166">När du kommentera ut hello första raden ser hello koden ut så hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="691b6-166">After you comment out hello first line, hello code will look like hello following example.</span></span>
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   <span data-ttu-id="691b6-167">Den här koden utlöses hello hälsorapport varje gång `RunAsync` körs.</span><span class="sxs-lookup"><span data-stu-id="691b6-167">This code fires hello health report each time `RunAsync` executes.</span></span> <span data-ttu-id="691b6-168">När du har gjort hello ändra trycker du på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="691b6-168">After you make hello change, press **F5** toorun hello application.</span></span>
6. <span data-ttu-id="691b6-169">Öppna Service Fabric Explorer toocheck hello hälsotillståndet för programmet hello när hello programmet körs.</span><span class="sxs-lookup"><span data-stu-id="691b6-169">After hello application is running, open Service Fabric Explorer toocheck hello health of hello application.</span></span> <span data-ttu-id="691b6-170">Den här gången visar Service Fabric Explorer att hello program inte är felfritt.</span><span class="sxs-lookup"><span data-stu-id="691b6-170">This time, Service Fabric Explorer shows that hello application is unhealthy.</span></span> <span data-ttu-id="691b6-171">Detta är på grund av hello-fel som rapporterades från hello kod som vi har lagt till tidigare.</span><span class="sxs-lookup"><span data-stu-id="691b6-171">This is because of hello error that was reported from hello code that we added previously.</span></span>
   
    ![Feltillstånd programmet i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. <span data-ttu-id="691b6-173">Om du väljer hello primära repliken i hello trädstrukturen för Service Fabric Explorer, visas som **hälsotillstånd** indikerar ett fel för.</span><span class="sxs-lookup"><span data-stu-id="691b6-173">If you select hello primary replica in hello tree view of Service Fabric Explorer, you will see that **Health State** indicates an error, too.</span></span> <span data-ttu-id="691b6-174">Service Fabric Explorer visar även hello hälsorapport information som har lagts till toohello `HealthInformation` parameter i hello kod.</span><span class="sxs-lookup"><span data-stu-id="691b6-174">Service Fabric Explorer also displays hello health report details that were added toohello `HealthInformation` parameter in hello code.</span></span> <span data-ttu-id="691b6-175">Du kan se hello samma hälsorapporter i PowerShell och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="691b6-175">You can see hello same health reports in PowerShell and hello Azure portal.</span></span>
   
    ![Replik hälsa i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

<span data-ttu-id="691b6-177">Den här rapporten finns kvar i hello health manager förrän den har ersatts av en annan rapport eller tills replikeringen tas bort.</span><span class="sxs-lookup"><span data-stu-id="691b6-177">This report remains in hello health manager until it is replaced by another report or until this replica is deleted.</span></span> <span data-ttu-id="691b6-178">Eftersom vi inte har angetts `TimeToLive` för den här hälsorapport i hello `HealthInformation` objekt hello rapporten upphör aldrig att gälla.</span><span class="sxs-lookup"><span data-stu-id="691b6-178">Because we did not set `TimeToLive` for this health report in hello `HealthInformation` object, hello report never expires.</span></span>

<span data-ttu-id="691b6-179">Vi rekommenderar att hälsa ska rapporteras på hello mest detaljerad nivå, som i det här fallet är hello replik.</span><span class="sxs-lookup"><span data-stu-id="691b6-179">We recommend that health should be reported on hello most granular level, which in this case is hello replica.</span></span> <span data-ttu-id="691b6-180">Du kan också rapportera hälsa för `Partition`.</span><span class="sxs-lookup"><span data-stu-id="691b6-180">You can also report health on `Partition`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

<span data-ttu-id="691b6-181">tooreport hälsa för `Application`, `DeployedApplication`, och `DeployedServicePackage`, Använd `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="691b6-181">tooreport health on `Application`, `DeployedApplication`, and `DeployedServicePackage`, use  `CodePackageActivationContext`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a><span data-ttu-id="691b6-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="691b6-182">Next steps</span></span>
* [<span data-ttu-id="691b6-183">Ingående om hälsa för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="691b6-183">Deep dive on Service Fabric health</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="691b6-184">REST API för rapportering av tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="691b6-184">REST API for reporting service health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [<span data-ttu-id="691b6-185">REST API för rapportering av programmets hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="691b6-185">REST API for reporting application health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

