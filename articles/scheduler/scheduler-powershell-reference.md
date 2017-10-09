---
title: aaaScheduler PowerShell Cmdlets-referens
description: "PowerShell Cmdlets-referens för Schemaläggaren"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: a2b23bcd3e4493ffba1dbf21fbb87818be7c01e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-powershell-cmdlets-reference"></a><span data-ttu-id="0ee39-103">PowerShell Cmdlets-referens för Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="0ee39-103">Scheduler PowerShell Cmdlets Reference</span></span>
<span data-ttu-id="0ee39-104">hello i den följande tabellen beskrivs och länkar toohello referenssida för varje större hello-cmdletar i Azure Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="0ee39-104">hello following table describes and links toohello reference page of each of hello major cmdlets in Azure Scheduler.</span></span>

<span data-ttu-id="0ee39-105">tooinstall Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0ee39-105">tooinstall Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

<span data-ttu-id="0ee39-106">Mer information om [Azure Resource Manager cmdlets](/powershell/azure/overview), se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0ee39-106">For more information about [Azure Resource Manager cmdlets](/powershell/azure/overview), see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

| <span data-ttu-id="0ee39-107">Cmdlet</span><span class="sxs-lookup"><span data-stu-id="0ee39-107">Cmdlet</span></span> | <span data-ttu-id="0ee39-108">Cmdlet-beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee39-108">Cmdlet Description</span></span> |
| --- | --- |
| [<span data-ttu-id="0ee39-109">Inaktivera AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-109">Disable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-110">Inaktiverar en jobbsamling.</span><span class="sxs-lookup"><span data-stu-id="0ee39-110">Disables a job collection.</span></span> |
| [<span data-ttu-id="0ee39-111">Aktivera AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-111">Enable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-112">Gör en jobbsamling.</span><span class="sxs-lookup"><span data-stu-id="0ee39-112">Enables a job collection.</span></span> |
| [<span data-ttu-id="0ee39-113">Get-AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-113">Get-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |<span data-ttu-id="0ee39-114">Hämtar schemaläggare.</span><span class="sxs-lookup"><span data-stu-id="0ee39-114">Gets Scheduler jobs.</span></span> |
| [<span data-ttu-id="0ee39-115">Get-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-115">Get-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-116">Hämtar jobbet samlingar.</span><span class="sxs-lookup"><span data-stu-id="0ee39-116">Gets job collections.</span></span> |
| [<span data-ttu-id="0ee39-117">Get-AzureRmSchedulerJobHistory</span><span class="sxs-lookup"><span data-stu-id="0ee39-117">Get-AzureRmSchedulerJobHistory</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |<span data-ttu-id="0ee39-118">Hämtar jobbhistorik.</span><span class="sxs-lookup"><span data-stu-id="0ee39-118">Gets job history.</span></span> |
| [<span data-ttu-id="0ee39-119">Ny AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-119">New-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |<span data-ttu-id="0ee39-120">Skapar ett HTTP-jobb.</span><span class="sxs-lookup"><span data-stu-id="0ee39-120">Creates an HTTP job.</span></span> |
| [<span data-ttu-id="0ee39-121">Ny AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-121">New-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-122">Skapar en jobbsamling.</span><span class="sxs-lookup"><span data-stu-id="0ee39-122">Creates a job collection.</span></span> |
| [<span data-ttu-id="0ee39-123">Ny AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-123">New-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |<span data-ttu-id="0ee39-124">Skapar en service bus kön.</span><span class="sxs-lookup"><span data-stu-id="0ee39-124">Creates a service bus queue job.</span></span> |
| [<span data-ttu-id="0ee39-125">Ny AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-125">New-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |<span data-ttu-id="0ee39-126">Skapar ett service bus avsnittet jobb.</span><span class="sxs-lookup"><span data-stu-id="0ee39-126">Creates a service bus topic job.</span></span> |
| [<span data-ttu-id="0ee39-127">Ny AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-127">New-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |<span data-ttu-id="0ee39-128">Skapar ett jobb för storage-kö.</span><span class="sxs-lookup"><span data-stu-id="0ee39-128">Creates a storage queue job.</span></span> |
| [<span data-ttu-id="0ee39-129">Ta bort AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-129">Remove-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |<span data-ttu-id="0ee39-130">Tar bort ett jobb i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="0ee39-130">Removes a Scheduler job.</span></span> |
| [<span data-ttu-id="0ee39-131">Ta bort AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-131">Remove-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-132">Tar bort en jobbsamling.</span><span class="sxs-lookup"><span data-stu-id="0ee39-132">Removes a job collection.</span></span> |
| [<span data-ttu-id="0ee39-133">Ange AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-133">Set-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |<span data-ttu-id="0ee39-134">Ändrar ett Jobbschema HTTP-jobb.</span><span class="sxs-lookup"><span data-stu-id="0ee39-134">Modifies a Scheduler HTTP job.</span></span> |
| [<span data-ttu-id="0ee39-135">Ange AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="0ee39-135">Set-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |<span data-ttu-id="0ee39-136">Ändrar en jobbsamling.</span><span class="sxs-lookup"><span data-stu-id="0ee39-136">Modifies a job collection.</span></span> |
| [<span data-ttu-id="0ee39-137">Ange AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-137">Set-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |<span data-ttu-id="0ee39-138">Ändrar ett service bus kön jobb.</span><span class="sxs-lookup"><span data-stu-id="0ee39-138">Modifies a service bus queue job.</span></span> |
| [<span data-ttu-id="0ee39-139">Ange AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-139">Set-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |<span data-ttu-id="0ee39-140">Ändrar ett service bus avsnittet jobb.</span><span class="sxs-lookup"><span data-stu-id="0ee39-140">Modifies a service bus topic job.</span></span> |
| [<span data-ttu-id="0ee39-141">Ange AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="0ee39-141">Set-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |<span data-ttu-id="0ee39-142">Ändrar ett jobb för storage-kö.</span><span class="sxs-lookup"><span data-stu-id="0ee39-142">Modifies a storage queue job.</span></span> |

<span data-ttu-id="0ee39-143">Du kan köra något av följande cmdlet: ar hello mer detaljerad information:</span><span class="sxs-lookup"><span data-stu-id="0ee39-143">For more detailed information, you can run any of hello following cmdlets:</span></span> 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a><span data-ttu-id="0ee39-144">Se även</span><span class="sxs-lookup"><span data-stu-id="0ee39-144">See Also</span></span>
 [<span data-ttu-id="0ee39-145">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="0ee39-145">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="0ee39-146">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-146">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="0ee39-147">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ee39-147">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="0ee39-148">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-148">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="0ee39-149">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-149">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="0ee39-150">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-150">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="0ee39-151">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-151">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="0ee39-152">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0ee39-152">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

