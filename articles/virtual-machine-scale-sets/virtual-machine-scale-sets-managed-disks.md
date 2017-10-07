---
title: "aaaUsing hanterade diskar med Azure Virtual Machine Skaluppsättningar | Microsoft Docs"
description: "Lär dig hur och varför toouse hanterade diskar med virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Skalningsuppsättningar för virtuella Azure-datorer och hanterade diskar

[Skalningsuppsättningar för virtuella Azure-datorer](/azure/virtual-machine-scale-sets/) stöder nu virtuella datorer med hanterade diskar. Det finns många fördelar med att använda hanterade diskar med skalningsuppsättningar, däribland:

* Du behöver inte längre toopre-skapa och hantera konton toostore hello OS diskar med lagringsutrymme för hello skaluppsättning för virtuella datorer.

* Du kan koppla hanterade diskar toohello skala datauppsättning.

* En skalningsuppsättning med en hanterad disk kan ha en kapacitet så hög som 1 000 virtuella datorer om den är baserad på en plattformsavbildning eller 100 virtuella datorer om den är baserad på en anpassad avbildning.

## <a name="get-started"></a>Kom igång

Ett enkelt sätt tooget igång med skaluppsättningar för hanterade diskar är toodeploy en från hello Azure-portalen. Mer information finns i [den här artikeln](./virtual-machine-scale-sets-portal-create.md). Ett annat enkelt sätt igång tooget är toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy en skaluppsättning. hello följande exempel visas hur toocreate en Ubuntu baserade skaluppsättningen med 10 virtuella datorer, var och en med en datadisk på 50 GB och 100 GB:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Alternativt kan du titta i hello [Azure Quickstart mallar GitHub-repo](https://github.com/Azure/azure-quickstart-templates) för mappar som innehåller `vmss` toosee exempel på mallar som distribuerar skaluppsättningar som fördefinierade. tootell vilka mallar redan använder hanterade diskar, kan du läsa för[listan](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="next-steps"></a>Nästa steg

Du hittar mer information om hanterade diskar i allmänhet i [den här artikeln](../virtual-machines/windows/managed-disks-overview.md).

toosee hur tooconvert som en Resource Manager mallen tooprovision skala uppsättningar med hanterade diskar, finns i [i den här artikeln](./virtual-machine-scale-sets-convert-template-to-md.md). hello gäller samma ändringar toohello Resource Manager-mallar även toohello Azure REST API.

toolearn mer information om hur du använder hanterade datadiskar med skalningsuppsättningar, se [i den här artikeln](./virtual-machine-scale-sets-attached-disks.md).

toobegin arbetar med stor skala anger finns för[i den här artikeln](./virtual-machine-scale-sets-placement-groups.md).


