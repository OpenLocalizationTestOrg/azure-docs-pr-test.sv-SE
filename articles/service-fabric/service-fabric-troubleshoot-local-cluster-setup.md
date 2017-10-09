---
title: aaaTroubleshoot din lokala konfiguration av Service Fabric | Microsoft Docs
description: "Den här artikeln innehåller en uppsättning förslag för att felsöka lokal utveckling-kluster"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: ce36f62a4bc69d2cd5b6c3df4abda6ca88fa84f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Felsöka din konfiguration av lokal utveckling
Om du stöter på ett problem vid interaktion med din lokala Azure Service Fabric-klustret för utveckling, granska hello följande förslag på möjliga lösningar.

## <a name="cluster-setup-failures"></a>Kluster-installationsfel
### <a name="cannot-clean-up-service-fabric-logs"></a>Det går inte att rensa Service Fabric-loggar
#### <a name="problem"></a>Problem
När du kör hello DevClusterSetup skript, visas ett fel så här:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held tooitems in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Lösning
Stäng hello aktuella PowerShell-fönster och öppna ett nytt PowerShell-fönster som administratör. Nu bör du kunna toosuccessfully kör hello-skript.

## <a name="cluster-connection-failures"></a>Kluster-anslutningsfel
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric PowerShell cmdlets känns inte igen av Azure PowerShell
#### <a name="problem"></a>Problem
Om du försöker du toorun med hello Service Fabric PowerShell-cmdlets som `Connect-ServiceFabricCluster` i ett Azure PowerShell-fönster misslyckas, säger hello cmdleten är okänd. hello anledningen är att Azure PowerShell använder hello 32-bitars version av Windows PowerShell (även på 64-bitars operativsystemversioner), medan hello Service Fabric-cmdlets fungerar endast i 64-bitars miljöer.

#### <a name="solution"></a>Lösning
Kör alltid Service Fabric-cmdlet: ar direkt från Windows PowerShell.

> [!NOTE]
> hello senaste versionen av Azure PowerShell kan inte skapa en särskild genväg så att det inte längre ska ske.
> 
> 

### <a name="type-initialization-exception"></a>Undantag för initiering av typen
#### <a name="problem"></a>Problem
När du ansluter toohello klustret i PowerShell kan se du hello fel TypeInitializationException för System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Lösning
Path-variabeln har inte angetts på rätt sätt under installationen. Logga ut från Windows och logga in igen. Detta uppdaterar din sökväg.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Klustret anslutningen misslyckas med ”-objekt stängs”
#### <a name="problem"></a>Problem
Anropet tooConnect-ServiceFabricCluster misslyckas med felmeddelandet Så här:

    Connect-ServiceFabricCluster : hello object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Lösning
Stäng hello aktuella PowerShell-fönster och öppna ett nytt PowerShell-fönster som administratör. Du bör nu kunna ansluta toosuccessfully.

### <a name="fabric-connection-denied-exception"></a>Fabric-anslutningen nekades undantag
#### <a name="problem"></a>Problem
När du felsöker från Visual Studio kan du få ett FabricConnectionDeniedException fel.

#### <a name="solution"></a>Lösning
Det här felet uppstår vanligen när du försöker toostart en värdprocess för tjänsten manuellt, i stället för att tillåta hello Service Fabric runtime toostart det åt dig.

Se till att du inte har några serviceprojekt som Startprojekt i din lösning. Endast Service Fabric-programprojekt ska anges som Startprojekt.

> [!TIP]
> Om efter installationen, lokala klustret börjar toobehave onormalt, kan du återställa den med hjälp av hello lokala klustret manager system fack. Detta tar bort hello befintliga klustret och skapa en ny. Observera att alla distribuerade program och associerade data tas bort.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Förstå och felsöka klustret med systemet hälsorapporter](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

