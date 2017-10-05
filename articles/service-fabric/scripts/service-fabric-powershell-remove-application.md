---
title: "Azure PowerShell skriptexempel - ta bort programmet från ett kluster | Microsoft Docs"
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
ms.openlocfilehash: 05851132c7e5e5877884d29f04bce6c0717ce411
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="48145-103">Ta bort ett program från ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="48145-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="48145-104">Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från klustret och tar bort programpaketet från avbildningsarkivet klustret.</span><span class="sxs-lookup"><span data-stu-id="48145-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from the cluster, and deletes the application package from the cluster image store.</span></span>  <span data-ttu-id="48145-105">Tar bort programinstansen också att tas bort alla körs tjänsten instanser som är associerade med programmet.</span><span class="sxs-lookup"><span data-stu-id="48145-105">Deleting the application instance also deletes all the running service instances associated with that application.</span></span> <span data-ttu-id="48145-106">Anpassa parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="48145-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="48145-107">Om det behövs installerar du Service Fabric PowerShell-modulen med den [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="48145-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="48145-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="48145-108">Sample script</span></span>

<span data-ttu-id="48145-109">[!code-powershell[huvudsakliga](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "ta bort ett program från ett kluster")]</span><span class="sxs-lookup"><span data-stu-id="48145-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="48145-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="48145-110">Script explanation</span></span>

<span data-ttu-id="48145-111">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="48145-111">This script uses the following commands.</span></span> <span data-ttu-id="48145-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="48145-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="48145-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="48145-113">Command</span></span> | <span data-ttu-id="48145-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="48145-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="48145-115">Ta bort ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="48145-115">Remove-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="48145-116">Tar bort en instans som körs Service Fabric-program från klustret.</span><span class="sxs-lookup"><span data-stu-id="48145-116">Removes a running Service Fabric application instance from the cluster.</span></span>  |
| [<span data-ttu-id="48145-117">Avregistrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="48145-117">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="48145-118">Avregistrerar en Service Fabric-programtypen och versionen från klustret.</span><span class="sxs-lookup"><span data-stu-id="48145-118">Unregisters a Service Fabric application type and version from the cluster.</span></span> |
| [<span data-ttu-id="48145-119">Ta bort ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="48145-119">Remove-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="48145-120">Tar bort ett Service Fabric-programpaket från avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="48145-120">Removes a Service Fabric application package from the image store.</span></span>|

## <a name="next-steps"></a><span data-ttu-id="48145-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48145-121">Next steps</span></span>

<span data-ttu-id="48145-122">Mer information om Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="48145-122">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="48145-123">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i den [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="48145-123">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
