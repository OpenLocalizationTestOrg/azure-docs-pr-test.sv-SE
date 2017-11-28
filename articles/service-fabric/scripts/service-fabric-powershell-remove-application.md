---
title: "aaaAzure skriptexempel PowerShell - ta bort programmet från ett kluster | Microsoft Docs"
description: "Azure PowerShell skriptexempel - ta bort ett program från ett Service Fabric-kluster."
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
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="c0fc8-103">Ta bort ett program från ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="c0fc8-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="c0fc8-104">Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från hello klustret och tar bort hello programpaket från avbildningsarkivet för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster, and deletes hello application package from hello cluster image store.</span></span>  <span data-ttu-id="c0fc8-105">Ta bort hello programinstansen tas även bort alla hello kör instanser av tjänsten som är associerade med programmet.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="c0fc8-106">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="c0fc8-107">Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c0fc8-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c0fc8-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c0fc8-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a><span data-ttu-id="c0fc8-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c0fc8-109">Script explanation</span></span>

<span data-ttu-id="c0fc8-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-110">This script uses hello following commands.</span></span> <span data-ttu-id="c0fc8-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c0fc8-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="c0fc8-112">Command</span></span> | <span data-ttu-id="c0fc8-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c0fc8-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c0fc8-114">Ta bort ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="c0fc8-114">Remove-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="c0fc8-115">Tar bort en instans som körs Service Fabric-program från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-115">Removes a running Service Fabric application instance from hello cluster.</span></span>  |
| [<span data-ttu-id="c0fc8-116">Avregistrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="c0fc8-116">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="c0fc8-117">Avregistrerar en Service Fabric-programtypen och versionen från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-117">Unregisters a Service Fabric application type and version from hello cluster.</span></span> |
| [<span data-ttu-id="c0fc8-118">Ta bort ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="c0fc8-118">Remove-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="c0fc8-119">Tar bort ett Service Fabric-programpaket från avbildningsarkivet hello.</span><span class="sxs-lookup"><span data-stu-id="c0fc8-119">Removes a Service Fabric application package from hello image store.</span></span>|

## <a name="next-steps"></a><span data-ttu-id="c0fc8-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0fc8-120">Next steps</span></span>

<span data-ttu-id="c0fc8-121">Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="c0fc8-121">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="c0fc8-122">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c0fc8-122">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
