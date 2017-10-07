---
title: aaaHow tootag en virtuell Azure Linux-dator | Microsoft Docs
description: "Mer information om taggar en Azure Linux-dator som skapats i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 0e99ea66a87b7e00eb21a2f72dd2bce8673778dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a>Hur tootag en Linux-dator i Azure
Den här artikeln beskrivs olika sätt tootag en Linux-dator i Azure via hello Resource Manager-modellen. Taggar är användardefinierade nyckel/värde-par som kan placeras direkt på en resurs eller en resursgrupp. Azure stöder för närvarande in too15 taggar per resurs och resursgruppen. Taggar kan placeras på en resurs för närvarande hello skapas eller lägga tooan befintlig resurs. Observera taggar stöds för resurser som skapats via hello Resource Manager distributionsmodellen.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Tagga med Azure CLI
toobegin, [installera och konfigurera hello Azure CLI](../../xplat-cli-azure-resource-manager.md) och kontrollera att du är i läget Resource Manager (`azure config mode arm`).

Du kan visa alla egenskaper för en viss virtuell dator, inklusive hello taggar, med det här kommandot:

        azure vm show -g MyResourceGroup -n MyTestVM

tooadd en ny VM-tagg via hello Azure CLI, kan du använda hello `azure vm set` kommandot tillsammans med hello taggen parametern **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

tooremove alla taggar, du kan använda hello **– T** parameter i hello `azure vm set` kommando.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nu när vi har använt taggar tooour resurser Azure CLI och hello Portal ska vi ta en titt på hello användning information toosee hello taggar i hello fakturering portal.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om resurserna i Azure-märkning finns [översikt över Azure Resource Manager] [ Azure Resource Manager Overview] och [med hjälp av taggar tooorganize dina Azure-resurser] [ Using Tags tooorganize your Azure Resources].
* toosee hur taggar kan hjälpa dig att hantera din användning av Azure-resurser, se [förstå fakturan Azure] [ Understanding your Azure Bill] och [få insikter om dina Microsoft Azure-resursförbrukning] [Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
