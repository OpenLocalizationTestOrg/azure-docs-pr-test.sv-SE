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
# <a name="report-and-check-service-health"></a>Rapportera och kontrollera hälsan hos tjänster
När dina tjänster har problem, beror din möjlighet toorespond tooand korrigering incidenter och avbrott på din möjlighet toodetect hello problem snabbt. Om du rapportera problem och fel toohello Azure Service Fabric health manager från service koden använder du standard hälsoövervakning verktyg för att Service Fabric tillhandahåller toocheck hello hälsostatus.

Det finns tre sätt att du kan rapportera hälsa från hello-tjänsten:

* Använd [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) eller [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objekt.  
  Du kan använda hello `Partition` och `CodePackageActivationContext` objekt tooreport hello hälsotillståndet för element som ingår i hello aktuell kontext. Kod som körs som en del av en replik kan rapportera hälsa endast på den repliken och hello-partition som den tillhör hello-program som den är en del av.
* Använd `FabricClient`.   
  Du kan använda `FabricClient` tooreport hälsa från hello service-kod om hello klustret inte [säker](service-fabric-cluster-security.md) eller om hello-tjänsten körs med administratörsrättigheter. De flesta verkliga scenarier inte använder oskyddade kluster, eller ange admin-privilegier. Med `FabricClient`, kan du rapportera hälsotillstånd på en enhet som är en del av hello klustret. Vi rekommenderar dock bör Tjänstkod bara skicka rapporter som är relaterade tooits egen hälsa.
* Använd hello REST API: er på hello kluster, program, distribuerat program, tjänst, servicepaket, partition, replik eller nod nivåer. Detta kan vara används tooreport hälsa från i en behållare.

Den här artikeln vägleder dig genom ett exempel som rapporterar hälsotillståndet från hello Tjänstkod. hello exemplet visas hur hello verktyg som tillhandahålls av Service Fabric kan vara används toocheck hello hälsostatus. Den här artikeln är avsedd toobe en snabb introduktion toohello hälsoövervakning funktionerna i Service Fabric. Mer detaljerad information kan du läsa hello serie djupgående artiklar om hälsa som börjar med hello länken hello slutet av den här artikeln.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande installerat:

* Visual Studio 2015 eller Visual Studio 2017
* Service Fabric SDK

## <a name="toocreate-a-local-secure-dev-cluster"></a>toocreate ett lokala säker dev-kluster
* Öppna PowerShell med administratörsrättigheter och kör följande kommandon hello:

![Kommandon som visar hur toocreate ett säker dev-kluster](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a>toodeploy ett program och kontrollera dess hälsa
1. Öppna Visual Studio som administratör.
2. Skapa ett projekt med hjälp av hello **tillståndskänslig Service** mall.
   
    ![Skapa ett Service Fabric-program med tillståndskänslig Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. Tryck på **F5** toorun hello appen i felsökningsläge. hello program är distribuerade toohello lokala klustret.
4. När programmet hello körs, högerklicka på hello lokala Klusterhanterare ikon i meddelandefältet för hello och välj **hantera lokala klustret** från hello genvägen menyn tooopen Service Fabric Explorer.
   
    ![Öppna Service Fabric Explorer från meddelandefältet](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. Hej programhälsan ska visas som i den här avbildningen. Hello programmet bör vara felfria utan fel för tillfället.
   
    ![Felfri programmet i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. Du kan också kontrollera hello hälsotillstånd med hjälp av PowerShell. Du kan använda ```Get-ServiceFabricApplicationHealth``` toocheck ett programs hälsa och du kan använda ```Get-ServiceFabricServiceHealth``` toocheck tjänstens hälsa. hello hälsorapport för hello samma program i PowerShell finns i den här avbildningen.
   
    ![Felfri program i PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a>tooadd anpassade hälsa händelser tooyour service-kod
hello Service Fabric-projektmallar i Visual Studio innehåller exempelkod. hello följande steg visar hur du kan rapportera anpassade hälsa händelser från koden för tjänsten. Rapporterna visas automatiskt i hello standardverktyg för hälsoövervakning att Service Fabric innehåller till exempel Service Fabric Explorer, hälsotillståndsvy för Azure portal och PowerShell.

1. Öppna hello-program som du skapade tidigare i Visual Studio eller skapa ett nytt program med hjälp av hello **tillståndskänslig Service** Visual Studio-mall.
2. Öppna hello Stateful1.cs-filen och hitta hello `myDictionary.TryGetValueAsync` -anrop i hello `RunAsync` metod. Du kan se att den här metoden returnerar en `result` som innehåller hello aktuellt värde för hello räknaren eftersom hello logik i det här programmet är tookeep ett antal körs. Om detta har ett riktigt program och hello bristande resultat visas ett fel, skulle du tooflag händelsen.
3. tooreport hälsohändelse när hello bristande resultat som representerar ett fel, Lägg till hello följande steg.
   
    a. Lägg till hello `System.Fabric.Health` namnområde toohello Stateful1.cs fil.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. Lägg till följande kod efter hello hello `myDictionary.TryGetValueAsync` anropa
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Vi rapporterar replik hälsotillstånd eftersom rapporteras från en tillståndskänslig service. Hej `HealthInformation` parametern lagrar information om hello hälsa problemet som rapporteras.
   
    Om du har skapat en tillståndslösa tjänsten Använd hello följande kod
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. Om din tjänst körs med administratörsrättigheter inte eller om hello klustret [säker](service-fabric-cluster-security.md), du kan också använda `FabricClient` tooreport hälsa enligt hello följande steg.  
   
    a. Skapa hello `FabricClient` instans efter hello `var myDictionary` deklaration.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. Lägg till följande kod efter hello hello `myDictionary.TryGetValueAsync` anropa.
   
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
5. Vi simulera felet och se den visas i hello hälsa övervakningsverktyg. toosimulate hello fel, kommentera hello första raden i hello hälsotillståndsrapportering kod som du lade till tidigare. När du kommentera ut hello första raden ser hello koden ut så hello följande exempel.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Den här koden utlöses hello hälsorapport varje gång `RunAsync` körs. När du har gjort hello ändra trycker du på **F5** toorun hello program.
6. Öppna Service Fabric Explorer toocheck hello hälsotillståndet för programmet hello när hello programmet körs. Den här gången visar Service Fabric Explorer att hello program inte är felfritt. Detta är på grund av hello-fel som rapporterades från hello kod som vi har lagt till tidigare.
   
    ![Feltillstånd programmet i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. Om du väljer hello primära repliken i hello trädstrukturen för Service Fabric Explorer, visas som **hälsotillstånd** indikerar ett fel för. Service Fabric Explorer visar även hello hälsorapport information som har lagts till toohello `HealthInformation` parameter i hello kod. Du kan se hello samma hälsorapporter i PowerShell och hello Azure-portalen.
   
    ![Replik hälsa i Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Den här rapporten finns kvar i hello health manager förrän den har ersatts av en annan rapport eller tills replikeringen tas bort. Eftersom vi inte har angetts `TimeToLive` för den här hälsorapport i hello `HealthInformation` objekt hello rapporten upphör aldrig att gälla.

Vi rekommenderar att hälsa ska rapporteras på hello mest detaljerad nivå, som i det här fallet är hello replik. Du kan också rapportera hälsa för `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

tooreport hälsa för `Application`, `DeployedApplication`, och `DeployedServicePackage`, Använd `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Nästa steg
* [Ingående om hälsa för Service Fabric](service-fabric-health-introduction.md)
* [REST API för rapportering av tjänstens hälsa](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [REST API för rapportering av programmets hälsotillstånd](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

