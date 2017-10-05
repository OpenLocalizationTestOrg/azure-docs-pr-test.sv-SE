---
title: "Felsöka din lokala konfiguration av Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: aa393f884b564cee81fcf75cc2eff895efea9471
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a><span data-ttu-id="9b152-103">Felsöka din konfiguration av lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="9b152-103">Troubleshoot your local development cluster setup</span></span>
<span data-ttu-id="9b152-104">Om du stöter på ett problem vid interaktion med din lokala Azure Service Fabric-klustret för utveckling, granska följande rekommendationer för möjliga lösningar.</span><span class="sxs-lookup"><span data-stu-id="9b152-104">If you run into an issue while interacting with your local Azure Service Fabric development cluster, review the following suggestions for potential solutions.</span></span>

## <a name="cluster-setup-failures"></a><span data-ttu-id="9b152-105">Kluster-installationsfel</span><span class="sxs-lookup"><span data-stu-id="9b152-105">Cluster setup failures</span></span>
### <a name="cannot-clean-up-service-fabric-logs"></a><span data-ttu-id="9b152-106">Det går inte att rensa Service Fabric-loggar</span><span class="sxs-lookup"><span data-stu-id="9b152-106">Cannot clean up Service Fabric logs</span></span>
#### <a name="problem"></a><span data-ttu-id="9b152-107">Problem</span><span class="sxs-lookup"><span data-stu-id="9b152-107">Problem</span></span>
<span data-ttu-id="9b152-108">När du kör skriptet DevClusterSetup, visas ett fel så här:</span><span class="sxs-lookup"><span data-stu-id="9b152-108">While running the DevClusterSetup script, you see an error like this:</span></span>

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a><span data-ttu-id="9b152-109">Lösning</span><span class="sxs-lookup"><span data-stu-id="9b152-109">Solution</span></span>
<span data-ttu-id="9b152-110">Stäng det nuvarande PowerShell-fönstret och öppna ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="9b152-110">Close the current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="9b152-111">Du bör nu kunna köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="9b152-111">You should now be able to successfully run the script.</span></span>

## <a name="cluster-connection-failures"></a><span data-ttu-id="9b152-112">Kluster-anslutningsfel</span><span class="sxs-lookup"><span data-stu-id="9b152-112">Cluster connection failures</span></span>
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a><span data-ttu-id="9b152-113">Service Fabric PowerShell cmdlets känns inte igen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b152-113">Service Fabric PowerShell cmdlets are not recognized in Azure PowerShell</span></span>
#### <a name="problem"></a><span data-ttu-id="9b152-114">Problem</span><span class="sxs-lookup"><span data-stu-id="9b152-114">Problem</span></span>
<span data-ttu-id="9b152-115">Om du försöker köra Service Fabric PowerShell-cmdlets som `Connect-ServiceFabricCluster` i ett Azure PowerShell-fönster misslyckas, säger att cmdleten inte känns igen.</span><span class="sxs-lookup"><span data-stu-id="9b152-115">If you try to run any of the Service Fabric PowerShell cmdlets, such as `Connect-ServiceFabricCluster` in an Azure PowerShell window, it fails, saying that the cmdlet is not recognized.</span></span> <span data-ttu-id="9b152-116">Anledningen är att Azure PowerShell använder 32-bitars version av Windows PowerShell (även på 64-bitars operativsystemversioner), medan Service Fabric-cmdlets fungerar endast i 64-bitars miljöer.</span><span class="sxs-lookup"><span data-stu-id="9b152-116">The reason for this is that Azure PowerShell uses the 32-bit version of Windows PowerShell (even on 64-bit OS versions), whereas the Service Fabric cmdlets only work in 64-bit environments.</span></span>

#### <a name="solution"></a><span data-ttu-id="9b152-117">Lösning</span><span class="sxs-lookup"><span data-stu-id="9b152-117">Solution</span></span>
<span data-ttu-id="9b152-118">Kör alltid Service Fabric-cmdlet: ar direkt från Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9b152-118">Always run Service Fabric cmdlets directly from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="9b152-119">Den senaste versionen av Azure PowerShell kan inte skapa en särskild genväg så att det inte längre ska ske.</span><span class="sxs-lookup"><span data-stu-id="9b152-119">The latest version of Azure PowerShell does not create a special shortcut, so this should no longer occur.</span></span>
> 
> 

### <a name="type-initialization-exception"></a><span data-ttu-id="9b152-120">Undantag för initiering av typen</span><span class="sxs-lookup"><span data-stu-id="9b152-120">Type Initialization exception</span></span>
#### <a name="problem"></a><span data-ttu-id="9b152-121">Problem</span><span class="sxs-lookup"><span data-stu-id="9b152-121">Problem</span></span>
<span data-ttu-id="9b152-122">När du ansluter till klustret i PowerShell kan se du felet TypeInitializationException för System.Fabric.Common.AppTrace.</span><span class="sxs-lookup"><span data-stu-id="9b152-122">When you are connecting to the cluster in PowerShell, you see the error TypeInitializationException for System.Fabric.Common.AppTrace.</span></span>

#### <a name="solution"></a><span data-ttu-id="9b152-123">Lösning</span><span class="sxs-lookup"><span data-stu-id="9b152-123">Solution</span></span>
<span data-ttu-id="9b152-124">Path-variabeln har inte angetts på rätt sätt under installationen.</span><span class="sxs-lookup"><span data-stu-id="9b152-124">Your path variable was not correctly set during installation.</span></span> <span data-ttu-id="9b152-125">Logga ut från Windows och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="9b152-125">Sign out of Windows and sign back in.</span></span> <span data-ttu-id="9b152-126">Detta uppdaterar din sökväg.</span><span class="sxs-lookup"><span data-stu-id="9b152-126">This refreshes your path.</span></span>

### <a name="cluster-connection-fails-with-object-is-closed"></a><span data-ttu-id="9b152-127">Klustret anslutningen misslyckas med ”-objekt stängs”</span><span class="sxs-lookup"><span data-stu-id="9b152-127">Cluster connection fails with "Object is closed"</span></span>
#### <a name="problem"></a><span data-ttu-id="9b152-128">Problem</span><span class="sxs-lookup"><span data-stu-id="9b152-128">Problem</span></span>
<span data-ttu-id="9b152-129">Ett anrop till Connect-ServiceFabricCluster misslyckas med felmeddelandet Så här:</span><span class="sxs-lookup"><span data-stu-id="9b152-129">A call to Connect-ServiceFabricCluster fails with an error like this:</span></span>

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a><span data-ttu-id="9b152-130">Lösning</span><span class="sxs-lookup"><span data-stu-id="9b152-130">Solution</span></span>
<span data-ttu-id="9b152-131">Stäng det nuvarande PowerShell-fönstret och öppna ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="9b152-131">Close the current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="9b152-132">Du ska nu kunna ansluta.</span><span class="sxs-lookup"><span data-stu-id="9b152-132">You should now be able to successfully connect.</span></span>

### <a name="fabric-connection-denied-exception"></a><span data-ttu-id="9b152-133">Fabric-anslutningen nekades undantag</span><span class="sxs-lookup"><span data-stu-id="9b152-133">Fabric Connection Denied exception</span></span>
#### <a name="problem"></a><span data-ttu-id="9b152-134">Problem</span><span class="sxs-lookup"><span data-stu-id="9b152-134">Problem</span></span>
<span data-ttu-id="9b152-135">När du felsöker från Visual Studio kan du få ett FabricConnectionDeniedException fel.</span><span class="sxs-lookup"><span data-stu-id="9b152-135">When debugging from Visual Studio, you get a FabricConnectionDeniedException error.</span></span>

#### <a name="solution"></a><span data-ttu-id="9b152-136">Lösning</span><span class="sxs-lookup"><span data-stu-id="9b152-136">Solution</span></span>
<span data-ttu-id="9b152-137">Det här felet uppstår vanligen när du försöker starta en serverprocess manuellt i stället för att tillåta Service Fabric runtime om du.</span><span class="sxs-lookup"><span data-stu-id="9b152-137">This error usually occurs when you try to start a service host process manually, rather than allowing the Service Fabric runtime to start it for you.</span></span>

<span data-ttu-id="9b152-138">Se till att du inte har några serviceprojekt som Startprojekt i din lösning.</span><span class="sxs-lookup"><span data-stu-id="9b152-138">Ensure that you do not have any service projects set as startup projects in your solution.</span></span> <span data-ttu-id="9b152-139">Endast Service Fabric-programprojekt ska anges som Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="9b152-139">Only Service Fabric application projects should be set as startup projects.</span></span>

> [!TIP]
> <span data-ttu-id="9b152-140">Om efter installationen, lokala klustret börjar fungera onormalt, kan du återställa den med hjälp av det lokala klustret manager fack systemprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9b152-140">If, following setup, your local cluster begins to behave abnormally, you can reset it using the local cluster manager system tray application.</span></span> <span data-ttu-id="9b152-141">Detta tar bort befintliga klustret och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="9b152-141">This removes the existing cluster and set up a new one.</span></span> <span data-ttu-id="9b152-142">Observera att alla distribuerade program och associerade data tas bort.</span><span class="sxs-lookup"><span data-stu-id="9b152-142">Note that all deployed applications and associated data is removed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="9b152-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b152-143">Next steps</span></span>
* [<span data-ttu-id="9b152-144">Förstå och felsöka klustret med systemet hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="9b152-144">Understand and troubleshoot your cluster with system health reports</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [<span data-ttu-id="9b152-145">Visualisera ditt kluster med Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="9b152-145">Visualize your cluster with Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

