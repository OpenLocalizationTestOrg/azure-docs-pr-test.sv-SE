---
title: "Azure PowerShell-skript exempel – distribuera program till ett kluster | Microsoft Docs"
description: "Azure PowerShell-skript exempel – distribuerar ett program till ett Service Fabric-kluster."
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
ms.openlocfilehash: 2863823205dbd70f63948ecd4af8898220fe1ff8
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a><span data-ttu-id="e528f-103">Distribuera ett program till ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="e528f-103">Deploy an application to a Service Fabric cluster</span></span>

<span data-ttu-id="e528f-104">Det här exempelskriptet kopierar ett programpaket till ett kluster avbildningsarkivet registrerar programtypen i klustret och skapar en instans av programmet från programtypen.</span><span class="sxs-lookup"><span data-stu-id="e528f-104">This sample script copies an application package to a cluster image store, registers the application type in the cluster, and creates an application instance from the application type.</span></span>  <span data-ttu-id="e528f-105">Om alla standardtjänster definierades i programmanifestet av programmet måltypen skapas tjänsterna just nu.</span><span class="sxs-lookup"><span data-stu-id="e528f-105">If any default services were defined in the application manifest of the target application type, then those services are created at this time.</span></span> <span data-ttu-id="e528f-106">Anpassa parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="e528f-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="e528f-107">Om det behövs installerar du Service Fabric PowerShell-modulen med den [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e528f-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e528f-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e528f-108">Sample script</span></span>

<span data-ttu-id="e528f-109">[!code-powershell[huvudsakliga](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "distribuerar ett program till ett kluster")]</span><span class="sxs-lookup"><span data-stu-id="e528f-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e528f-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="e528f-110">Clean up deployment</span></span> 

<span data-ttu-id="e528f-111">Efter skriptexempel har körts, skriptet [tar bort ett program](service-fabric-powershell-remove-application.md) kan användas för att ta bort programinstansen, avregistrera programtypen och ta bort programmet paketet från avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="e528f-111">After the script sample has been run, the script in [Remove an application](service-fabric-powershell-remove-application.md) can be used to remove the application instance, unregister the application type, and delete the application package from the image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="e528f-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e528f-112">Script explanation</span></span>

<span data-ttu-id="e528f-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="e528f-113">This script uses the following commands.</span></span> <span data-ttu-id="e528f-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e528f-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e528f-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="e528f-115">Command</span></span> | <span data-ttu-id="e528f-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e528f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e528f-117">Kopiera ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="e528f-117">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="e528f-118">Kopiera ett programpaket till klustret image store.</span><span class="sxs-lookup"><span data-stu-id="e528f-118">Copy an application package to the cluster image store.</span></span>  |
|[<span data-ttu-id="e528f-119">Registrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="e528f-119">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="e528f-120">Registrerar ett programtypen och versionen på klustret.</span><span class="sxs-lookup"><span data-stu-id="e528f-120">Registers an application type and version on the cluster.</span></span> |
|[<span data-ttu-id="e528f-121">Ny ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="e528f-121">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="e528f-122">Skapar ett program från en typ som registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="e528f-122">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e528f-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e528f-123">Next steps</span></span>

<span data-ttu-id="e528f-124">Mer information om Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="e528f-124">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="e528f-125">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i den [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e528f-125">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
