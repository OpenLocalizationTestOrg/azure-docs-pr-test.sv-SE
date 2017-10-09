---
title: aaaAzure PowerShell skriptexempel - skapa ett Service Fabric-kluster | Microsoft Docs
description: "Azure PowerShell-skript exempel – skapa ett Service Fabric-kluster."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 12fdc201bd51688cb850cd456b1e00442b79c22d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster"></a><span data-ttu-id="9ae74-103">Skapa ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="9ae74-103">Create a Service Fabric cluster</span></span>

<span data-ttu-id="9ae74-104">Det här exempelskriptet skapar Service Fabric-kluster ett kluster med fem noder skyddas med ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="9ae74-104">This sample script creates a Service Fabric cluster a five-node cluster secured with an X.509 certificate.</span></span>  <span data-ttu-id="9ae74-105">hello-kommando skapar ett självsignerat certifikat och överförs tooa nya nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9ae74-105">hello command creates a self-signed certificate and uploads it tooa new key vault.</span></span> <span data-ttu-id="9ae74-106">hello certifikat är också kopierade tooa lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="9ae74-106">hello certificate is also copied tooa local directory.</span></span>  <span data-ttu-id="9ae74-107">Ange hello *-OS* parametern toochoose hello version av Windows eller Linux som körs på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="9ae74-107">Set hello *-OS* parameter toochoose hello version of Windows or Linux that runs on hello cluster nodes.</span></span>  <span data-ttu-id="9ae74-108">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="9ae74-108">Customize hello parameters as needed.</span></span>

<span data-ttu-id="9ae74-109">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview) och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="9ae74-109">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9ae74-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="9ae74-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9ae74-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="9ae74-111">Clean up deployment</span></span> 

<span data-ttu-id="9ae74-112">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, kluster och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="9ae74-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, cluster, and all related resources.</span></span>

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="9ae74-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="9ae74-113">Script explanation</span></span>

<span data-ttu-id="9ae74-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="9ae74-114">This script uses hello following commands.</span></span> <span data-ttu-id="9ae74-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="9ae74-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9ae74-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="9ae74-116">Command</span></span> | <span data-ttu-id="9ae74-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9ae74-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9ae74-118">Ny AzureRmServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="9ae74-118">New-AzureRmServiceFabricCluster</span></span>](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | <span data-ttu-id="9ae74-119">Skapar ett nytt Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="9ae74-119">Creates a new Service Fabric cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9ae74-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ae74-120">Next steps</span></span>

<span data-ttu-id="9ae74-121">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9ae74-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9ae74-122">Ytterligare Azure Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9ae74-122">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
