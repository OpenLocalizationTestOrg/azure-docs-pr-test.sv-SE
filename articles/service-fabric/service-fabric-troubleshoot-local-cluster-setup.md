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
# <a name="troubleshoot-your-local-development-cluster-setup"></a><span data-ttu-id="cc856-103">Felsöka din konfiguration av lokal utveckling</span><span class="sxs-lookup"><span data-stu-id="cc856-103">Troubleshoot your local development cluster setup</span></span>
<span data-ttu-id="cc856-104">Om du stöter på ett problem vid interaktion med din lokala Azure Service Fabric-klustret för utveckling, granska hello följande förslag på möjliga lösningar.</span><span class="sxs-lookup"><span data-stu-id="cc856-104">If you run into an issue while interacting with your local Azure Service Fabric development cluster, review hello following suggestions for potential solutions.</span></span>

## <a name="cluster-setup-failures"></a><span data-ttu-id="cc856-105">Kluster-installationsfel</span><span class="sxs-lookup"><span data-stu-id="cc856-105">Cluster setup failures</span></span>
### <a name="cannot-clean-up-service-fabric-logs"></a><span data-ttu-id="cc856-106">Det går inte att rensa Service Fabric-loggar</span><span class="sxs-lookup"><span data-stu-id="cc856-106">Cannot clean up Service Fabric logs</span></span>
#### <a name="problem"></a><span data-ttu-id="cc856-107">Problem</span><span class="sxs-lookup"><span data-stu-id="cc856-107">Problem</span></span>
<span data-ttu-id="cc856-108">När du kör hello DevClusterSetup skript, visas ett fel så här:</span><span class="sxs-lookup"><span data-stu-id="cc856-108">While running hello DevClusterSetup script, you see an error like this:</span></span>

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held tooitems in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a><span data-ttu-id="cc856-109">Lösning</span><span class="sxs-lookup"><span data-stu-id="cc856-109">Solution</span></span>
<span data-ttu-id="cc856-110">Stäng hello aktuella PowerShell-fönster och öppna ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="cc856-110">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="cc856-111">Nu bör du kunna toosuccessfully kör hello-skript.</span><span class="sxs-lookup"><span data-stu-id="cc856-111">You should now be able toosuccessfully run hello script.</span></span>

## <a name="cluster-connection-failures"></a><span data-ttu-id="cc856-112">Kluster-anslutningsfel</span><span class="sxs-lookup"><span data-stu-id="cc856-112">Cluster connection failures</span></span>
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a><span data-ttu-id="cc856-113">Service Fabric PowerShell cmdlets känns inte igen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cc856-113">Service Fabric PowerShell cmdlets are not recognized in Azure PowerShell</span></span>
#### <a name="problem"></a><span data-ttu-id="cc856-114">Problem</span><span class="sxs-lookup"><span data-stu-id="cc856-114">Problem</span></span>
<span data-ttu-id="cc856-115">Om du försöker du toorun med hello Service Fabric PowerShell-cmdlets som `Connect-ServiceFabricCluster` i ett Azure PowerShell-fönster misslyckas, säger hello cmdleten är okänd.</span><span class="sxs-lookup"><span data-stu-id="cc856-115">If you try toorun any of hello Service Fabric PowerShell cmdlets, such as `Connect-ServiceFabricCluster` in an Azure PowerShell window, it fails, saying that hello cmdlet is not recognized.</span></span> <span data-ttu-id="cc856-116">hello anledningen är att Azure PowerShell använder hello 32-bitars version av Windows PowerShell (även på 64-bitars operativsystemversioner), medan hello Service Fabric-cmdlets fungerar endast i 64-bitars miljöer.</span><span class="sxs-lookup"><span data-stu-id="cc856-116">hello reason for this is that Azure PowerShell uses hello 32-bit version of Windows PowerShell (even on 64-bit OS versions), whereas hello Service Fabric cmdlets only work in 64-bit environments.</span></span>

#### <a name="solution"></a><span data-ttu-id="cc856-117">Lösning</span><span class="sxs-lookup"><span data-stu-id="cc856-117">Solution</span></span>
<span data-ttu-id="cc856-118">Kör alltid Service Fabric-cmdlet: ar direkt från Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc856-118">Always run Service Fabric cmdlets directly from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="cc856-119">hello senaste versionen av Azure PowerShell kan inte skapa en särskild genväg så att det inte längre ska ske.</span><span class="sxs-lookup"><span data-stu-id="cc856-119">hello latest version of Azure PowerShell does not create a special shortcut, so this should no longer occur.</span></span>
> 
> 

### <a name="type-initialization-exception"></a><span data-ttu-id="cc856-120">Undantag för initiering av typen</span><span class="sxs-lookup"><span data-stu-id="cc856-120">Type Initialization exception</span></span>
#### <a name="problem"></a><span data-ttu-id="cc856-121">Problem</span><span class="sxs-lookup"><span data-stu-id="cc856-121">Problem</span></span>
<span data-ttu-id="cc856-122">När du ansluter toohello klustret i PowerShell kan se du hello fel TypeInitializationException för System.Fabric.Common.AppTrace.</span><span class="sxs-lookup"><span data-stu-id="cc856-122">When you are connecting toohello cluster in PowerShell, you see hello error TypeInitializationException for System.Fabric.Common.AppTrace.</span></span>

#### <a name="solution"></a><span data-ttu-id="cc856-123">Lösning</span><span class="sxs-lookup"><span data-stu-id="cc856-123">Solution</span></span>
<span data-ttu-id="cc856-124">Path-variabeln har inte angetts på rätt sätt under installationen.</span><span class="sxs-lookup"><span data-stu-id="cc856-124">Your path variable was not correctly set during installation.</span></span> <span data-ttu-id="cc856-125">Logga ut från Windows och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="cc856-125">Sign out of Windows and sign back in.</span></span> <span data-ttu-id="cc856-126">Detta uppdaterar din sökväg.</span><span class="sxs-lookup"><span data-stu-id="cc856-126">This refreshes your path.</span></span>

### <a name="cluster-connection-fails-with-object-is-closed"></a><span data-ttu-id="cc856-127">Klustret anslutningen misslyckas med ”-objekt stängs”</span><span class="sxs-lookup"><span data-stu-id="cc856-127">Cluster connection fails with "Object is closed"</span></span>
#### <a name="problem"></a><span data-ttu-id="cc856-128">Problem</span><span class="sxs-lookup"><span data-stu-id="cc856-128">Problem</span></span>
<span data-ttu-id="cc856-129">Anropet tooConnect-ServiceFabricCluster misslyckas med felmeddelandet Så här:</span><span class="sxs-lookup"><span data-stu-id="cc856-129">A call tooConnect-ServiceFabricCluster fails with an error like this:</span></span>

    Connect-ServiceFabricCluster : hello object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a><span data-ttu-id="cc856-130">Lösning</span><span class="sxs-lookup"><span data-stu-id="cc856-130">Solution</span></span>
<span data-ttu-id="cc856-131">Stäng hello aktuella PowerShell-fönster och öppna ett nytt PowerShell-fönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="cc856-131">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="cc856-132">Du bör nu kunna ansluta toosuccessfully.</span><span class="sxs-lookup"><span data-stu-id="cc856-132">You should now be able toosuccessfully connect.</span></span>

### <a name="fabric-connection-denied-exception"></a><span data-ttu-id="cc856-133">Fabric-anslutningen nekades undantag</span><span class="sxs-lookup"><span data-stu-id="cc856-133">Fabric Connection Denied exception</span></span>
#### <a name="problem"></a><span data-ttu-id="cc856-134">Problem</span><span class="sxs-lookup"><span data-stu-id="cc856-134">Problem</span></span>
<span data-ttu-id="cc856-135">När du felsöker från Visual Studio kan du få ett FabricConnectionDeniedException fel.</span><span class="sxs-lookup"><span data-stu-id="cc856-135">When debugging from Visual Studio, you get a FabricConnectionDeniedException error.</span></span>

#### <a name="solution"></a><span data-ttu-id="cc856-136">Lösning</span><span class="sxs-lookup"><span data-stu-id="cc856-136">Solution</span></span>
<span data-ttu-id="cc856-137">Det här felet uppstår vanligen när du försöker toostart en värdprocess för tjänsten manuellt, i stället för att tillåta hello Service Fabric runtime toostart det åt dig.</span><span class="sxs-lookup"><span data-stu-id="cc856-137">This error usually occurs when you try toostart a service host process manually, rather than allowing hello Service Fabric runtime toostart it for you.</span></span>

<span data-ttu-id="cc856-138">Se till att du inte har några serviceprojekt som Startprojekt i din lösning.</span><span class="sxs-lookup"><span data-stu-id="cc856-138">Ensure that you do not have any service projects set as startup projects in your solution.</span></span> <span data-ttu-id="cc856-139">Endast Service Fabric-programprojekt ska anges som Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="cc856-139">Only Service Fabric application projects should be set as startup projects.</span></span>

> [!TIP]
> <span data-ttu-id="cc856-140">Om efter installationen, lokala klustret börjar toobehave onormalt, kan du återställa den med hjälp av hello lokala klustret manager system fack.</span><span class="sxs-lookup"><span data-stu-id="cc856-140">If, following setup, your local cluster begins toobehave abnormally, you can reset it using hello local cluster manager system tray application.</span></span> <span data-ttu-id="cc856-141">Detta tar bort hello befintliga klustret och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="cc856-141">This removes hello existing cluster and set up a new one.</span></span> <span data-ttu-id="cc856-142">Observera att alla distribuerade program och associerade data tas bort.</span><span class="sxs-lookup"><span data-stu-id="cc856-142">Note that all deployed applications and associated data is removed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="cc856-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc856-143">Next steps</span></span>
* [<span data-ttu-id="cc856-144">Förstå och felsöka klustret med systemet hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="cc856-144">Understand and troubleshoot your cluster with system health reports</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [<span data-ttu-id="cc856-145">Visualisera ditt kluster med Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="cc856-145">Visualize your cluster with Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

