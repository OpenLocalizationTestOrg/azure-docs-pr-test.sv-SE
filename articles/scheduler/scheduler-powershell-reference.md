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
# <a name="scheduler-powershell-cmdlets-reference"></a>PowerShell Cmdlets-referens för Schemaläggaren
hello i den följande tabellen beskrivs och länkar toohello referenssida för varje större hello-cmdletar i Azure Schemaläggaren.

tooinstall Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). 

Mer information om [Azure Resource Manager cmdlets](/powershell/azure/overview), se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Cmdlet-beskrivning |
| --- | --- |
| [Inaktivera AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Inaktiverar en jobbsamling. |
| [Aktivera AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Gör en jobbsamling. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Hämtar schemaläggare. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Hämtar jobbet samlingar. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Hämtar jobbhistorik. |
| [Ny AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Skapar ett HTTP-jobb. |
| [Ny AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Skapar en jobbsamling. |
| [Ny AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |Skapar en service bus kön. |
| [Ny AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Skapar ett service bus avsnittet jobb. |
| [Ny AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Skapar ett jobb för storage-kö. |
| [Ta bort AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Tar bort ett jobb i Schemaläggaren. |
| [Ta bort AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Tar bort en jobbsamling. |
| [Ange AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Ändrar ett Jobbschema HTTP-jobb. |
| [Ange AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Ändrar en jobbsamling. |
| [Ange AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Ändrar ett service bus kön jobb. |
| [Ange AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Ändrar ett service bus avsnittet jobb. |
| [Ange AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Ändrar ett jobb för storage-kö. |

Du kan köra något av följande cmdlet: ar hello mer detaljerad information: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

