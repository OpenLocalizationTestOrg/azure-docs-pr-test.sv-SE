---
title: "aaaConnect virtuella Linux-datorer i en molntjänst | Microsoft Docs"
description: "Ansluta Linux virtuella datorer som skapats med hello klassisk distribution modellen tooan Azure-molntjänst eller ett virtuellt nätverk."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 323baf04390d53ffb2810e24a24d175c08e60cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-linux-virtual-machines-created-with-hello-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Ansluta Linux virtuella datorer som skapats med hello klassiska distributionsmodellen med virtuella nätverk eller moln-tjänsten
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Linux virtuella datorer som skapats med hello klassiska distributionsmodellen placeras alltid i en molntjänst. hello Molntjänsten fungerar som en behållare och ger ett unikt offentliga DNS-namn, en offentlig IP-adress och en uppsättning slutpunkter tooaccess hello virtuella datorn över hello Internet. Hej Molntjänsten kan vara i ett virtuellt nätverk, men som är inte ett krav. Du kan också [ansluta virtuella Windows-datorer med virtuella nätverk eller molnet](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Om en molnbaserad tjänst som inte är i ett virtuellt nätverk, kallas en *fristående* Molntjänsten. hello virtuella datorer i en fristående molntjänst kommunicerar med andra virtuella datorer med hjälp av hello offentliga DNS-namn för andra virtuella datorer och hello trafik färdas över hello Internet. Om en tjänst i molnet är i ett virtuellt nätverk hello virtuella datorer i den molntjänst som kan kommunicera med alla andra virtuella datorer i hello virtuellt nätverk utan att skicka all trafik via hello Internet.

Om du placerar dina virtuella datorer i hello samma fristående molnbaserad tjänst, du kan fortfarande använda belastningsutjämning och tillgänglighetsuppsättningar. Mer information finns i [virtuella datorer för belastningsutjämning](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [hantera hello tillgängligheten för virtuella datorer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du kan inte ordna hello virtuella datorer i undernät eller ansluta en fristående cloud service tooyour lokalt nätverk. Här är ett exempel:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Nästa steg
När du skapar en virtuell dator är det en bra idé för[lägger till en datadisk](attach-disk.md) så att dina tjänster och arbetsbelastningar har en plats toostore data.
