---
title: "Spara filer i PowerShell Azure Cloud Shell (förhandsversion) | Microsoft Docs"
description: "Genomgång av hur Azure Cloud Shell kvarstår filer."
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: d0bc16bc951fce17235d8070012de44ebab89888
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/29/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Så här fungerar PowerShell Azure Cloud Shell (förhandsgranskning)
PowerShell i molnet Shell (förhandsgranskning) kvarstår filer via metoden följande: 
* Montera din angivna Azure-filresursen som `clouddrive` i din `$Home` katalogen för filresursen direkt interaktion.

## <a name="list-cloud-drive-azure-file-shares"></a>Lista molntjänster enhet Azure-filresurser
Den `Get-CloudDrive` kommando hämtar Azure resursen filinformationen monterad av molnet enheten i molnet-gränssnittet. <br>
![Kör Get-CloudDrive](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Demontera enhet i molnet
Du kan demontera en Azure-filresurs som är kopplad till molnet Shell när som helst. Om Azure-filresursen har tagits bort, uppmanas du att skapa och montera en ny Azure filresurs vid nästa session.

Den `Dismount-CloudDrive` kommando demonterar en Azure-filresursen från det aktuella lagringskontot. Demontera molnet enheten avbryter den aktuella sessionen. Användaren uppmanas att skapa och montera en ny Azure filresurs under nästa.
![Kör Dismount CloudDrive](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Nästa steg
[Snabbstart för PowerShell](quickstart-powershell.md) <br>
[Lär dig mer om Azure-filer](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Lär dig mer om lagring taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>