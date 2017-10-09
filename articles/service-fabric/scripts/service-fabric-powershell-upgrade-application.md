---
title: aaaAzure PowerShell skriptexempel - uppgraderar ett Service Fabric-program | Microsoft Docs
description: Azure PowerShell skriptexempel - uppgradering ett Service Fabric-program.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="10e54-103">Uppgradera ett Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="10e54-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="10e54-104">Det här exempelskriptet uppgraderar en aktiv Service Fabric application instans tooversion 1.3.0.</span><span class="sxs-lookup"><span data-stu-id="10e54-104">This sample script upgrades a running Service Fabric application instance tooversion 1.3.0.</span></span> <span data-ttu-id="10e54-105">kopierar hello nya program paketet toohello klustret avbildningsarkivet hello skript registrerar hello programtyp, startar en övervakade uppgradering och kontrollerar hello uppgraderingsstatus kontinuerligt tills hello uppgraderingen har slutförts eller återställs.</span><span class="sxs-lookup"><span data-stu-id="10e54-105">hello script copies hello new application package toohello cluster image store, registers hello application type, starts a monitored upgrade, and continuously checks hello upgrade status until hello upgrade completes or rolls back.</span></span> <span data-ttu-id="10e54-106">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="10e54-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="10e54-107">Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="10e54-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="10e54-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="10e54-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a><span data-ttu-id="10e54-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="10e54-109">Script explanation</span></span>

<span data-ttu-id="10e54-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="10e54-110">This script uses hello following commands.</span></span> <span data-ttu-id="10e54-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="10e54-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="10e54-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="10e54-112">Command</span></span> | <span data-ttu-id="10e54-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="10e54-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="10e54-114">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="10e54-114">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="10e54-115">Hämtar alla hello program i hello Service Fabric-kluster eller ett visst program.</span><span class="sxs-lookup"><span data-stu-id="10e54-115">Gets all hello applications in hello Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="10e54-116">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="10e54-116">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="10e54-117">Hämtar hello status för en uppgradering av Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="10e54-117">Gets hello status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="10e54-118">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="10e54-118">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="10e54-119">Hämtar hello Service Fabric application typer har registrerats på hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="10e54-119">Gets hello Service Fabric application types registered on hello Service Fabric cluster.</span></span> |
| [<span data-ttu-id="10e54-120">Avregistrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="10e54-120">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="10e54-121">Avregistrerar en Service Fabric-programtyp.</span><span class="sxs-lookup"><span data-stu-id="10e54-121">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="10e54-122">Kopiera ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="10e54-122">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="10e54-123">Kopierar en Service Fabric application package toohello avbildning arkivet.</span><span class="sxs-lookup"><span data-stu-id="10e54-123">Copies a Service Fabric application package toohello image store.</span></span>  |
| [<span data-ttu-id="10e54-124">Registrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="10e54-124">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="10e54-125">Registrerar en Service Fabric-programtyp.</span><span class="sxs-lookup"><span data-stu-id="10e54-125">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="10e54-126">Start-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="10e54-126">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="10e54-127">Uppgraderar en version Service Fabric-program för toohello angivet program.</span><span class="sxs-lookup"><span data-stu-id="10e54-127">Upgrades a Service Fabric application toohello specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="10e54-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10e54-128">Next steps</span></span>

<span data-ttu-id="10e54-129">Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="10e54-129">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="10e54-130">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="10e54-130">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
