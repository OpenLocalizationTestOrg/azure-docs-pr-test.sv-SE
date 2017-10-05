---
title: Azure PowerShell skriptexempel - uppgradering ett Service Fabric-program | Microsoft Docs
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
ms.openlocfilehash: 454849f82ddb23ddb9d71459f86e3cf5a1589254
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="0e586-103">Uppgradera ett Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="0e586-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="0e586-104">Det här exempelskriptet uppgraderar en instans som körs Service Fabric-program till version 1.3.0.</span><span class="sxs-lookup"><span data-stu-id="0e586-104">This sample script upgrades a running Service Fabric application instance to version 1.3.0.</span></span> <span data-ttu-id="0e586-105">Skriptet kopierar nytt programpaket till klustret image store, registrerar programtypen, startar en övervakade uppgradering och kontrollerar uppgraderingsstatus kontinuerligt tills uppgraderingen har slutförts eller återställs.</span><span class="sxs-lookup"><span data-stu-id="0e586-105">The script copies the new application package to the cluster image store, registers the application type, starts a monitored upgrade, and continuously checks the upgrade status until the upgrade completes or rolls back.</span></span> <span data-ttu-id="0e586-106">Anpassa parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="0e586-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="0e586-107">Om det behövs installerar du Service Fabric PowerShell-modulen med den [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0e586-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="0e586-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0e586-108">Sample script</span></span>

<span data-ttu-id="0e586-109">[!code-powershell[huvudsakliga](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "uppgradera ett program")]</span><span class="sxs-lookup"><span data-stu-id="0e586-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="0e586-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0e586-110">Script explanation</span></span>

<span data-ttu-id="0e586-111">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0e586-111">This script uses the following commands.</span></span> <span data-ttu-id="0e586-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0e586-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="0e586-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="0e586-113">Command</span></span> | <span data-ttu-id="0e586-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0e586-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0e586-115">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="0e586-115">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="0e586-116">Hämtar alla program i Service Fabric-kluster eller ett visst program.</span><span class="sxs-lookup"><span data-stu-id="0e586-116">Gets all the applications in the Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="0e586-117">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="0e586-117">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="0e586-118">Hämtar status för en uppgradering av Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="0e586-118">Gets the status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="0e586-119">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="0e586-119">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="0e586-120">Hämtar Service Fabric-programtyper som registrerats för Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0e586-120">Gets the Service Fabric application types registered on the Service Fabric cluster.</span></span> |
| [<span data-ttu-id="0e586-121">Avregistrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="0e586-121">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="0e586-122">Avregistrerar en Service Fabric-programtyp.</span><span class="sxs-lookup"><span data-stu-id="0e586-122">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="0e586-123">Kopiera ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="0e586-123">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="0e586-124">Kopierar ett Service Fabric-programpaket till image store.</span><span class="sxs-lookup"><span data-stu-id="0e586-124">Copies a Service Fabric application package to the image store.</span></span>  |
| [<span data-ttu-id="0e586-125">Registrera ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="0e586-125">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="0e586-126">Registrerar en Service Fabric-programtyp.</span><span class="sxs-lookup"><span data-stu-id="0e586-126">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="0e586-127">Start-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="0e586-127">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="0e586-128">Uppgraderar ett Service Fabric-program till den angivna typ versionen.</span><span class="sxs-lookup"><span data-stu-id="0e586-128">Upgrades a Service Fabric application to the specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="0e586-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e586-129">Next steps</span></span>

<span data-ttu-id="0e586-130">Mer information om Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="0e586-130">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="0e586-131">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i den [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0e586-131">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
