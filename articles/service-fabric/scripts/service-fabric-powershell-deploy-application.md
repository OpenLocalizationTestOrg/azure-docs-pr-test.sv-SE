---
title: aaaAzure PowerShell skriptexempel - distribuera programmet tooa kluster | Microsoft Docs
description: "Azure PowerShell-skript exempel – distribuera programmet tooa Service Fabric-klustret."
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
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="64adf-103">Distribuera ett program tooa Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="64adf-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="64adf-104">Det här exempelskriptet kopierar ett paket tooa klustret avbildningen programarkiv registrerar hello programtyp i hello kluster och skapar en instans av programmet från hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="64adf-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span>  <span data-ttu-id="64adf-105">Om alla standardtjänster definierades i hello programmanifestet av hello Målprogramstyp skapas tjänsterna just nu.</span><span class="sxs-lookup"><span data-stu-id="64adf-105">If any default services were defined in hello application manifest of hello target application type, then those services are created at this time.</span></span> <span data-ttu-id="64adf-106">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="64adf-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="64adf-107">Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64adf-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="64adf-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="64adf-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="64adf-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="64adf-109">Clean up deployment</span></span> 

<span data-ttu-id="64adf-110">Efter hello skriptexempel har körts hello skriptet i [tar bort ett program](service-fabric-powershell-remove-application.md) kan använda tooremove hello programinstansen, avregistrering av programtyp hello och ta bort hello programpaketet från avbildningsarkivet hello.</span><span class="sxs-lookup"><span data-stu-id="64adf-110">After hello script sample has been run, hello script in [Remove an application](service-fabric-powershell-remove-application.md) can be used tooremove hello application instance, unregister hello application type, and delete hello application package from hello image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="64adf-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="64adf-111">Script explanation</span></span>

<span data-ttu-id="64adf-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="64adf-112">This script uses hello following commands.</span></span> <span data-ttu-id="64adf-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="64adf-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="64adf-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="64adf-114">Command</span></span> | <span data-ttu-id="64adf-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="64adf-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="64adf-116">Kopiera ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="64adf-116">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="64adf-117">Kopiera ett paket toohello klustret avbildningen programarkiv.</span><span class="sxs-lookup"><span data-stu-id="64adf-117">Copy an application package toohello cluster image store.</span></span>  |
|[<span data-ttu-id="64adf-118">Registrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="64adf-118">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="64adf-119">Registrerar ett programtypen och versionen på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="64adf-119">Registers an application type and version on hello cluster.</span></span> |
|[<span data-ttu-id="64adf-120">Ny ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="64adf-120">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="64adf-121">Skapar ett program från en typ som registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="64adf-121">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="64adf-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64adf-122">Next steps</span></span>

<span data-ttu-id="64adf-123">Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="64adf-123">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="64adf-124">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="64adf-124">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
