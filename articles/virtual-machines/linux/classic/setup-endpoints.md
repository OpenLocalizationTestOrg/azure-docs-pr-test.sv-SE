---
title: "aaaSet av slutpunkter på en klassisk Linux VM | Microsoft Docs"
description: "Läs tooset av slutpunkter för en Linux-VM i hello Azure klassiska portal tooallow kommunikation med en virtuell Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Hur tooset av slutpunkter på en Linux klassiska virtuell dator i Azure
Alla Linux virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen kan automatiskt kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma cloud service eller virtuellt nätverk. Datorer på hello Internet eller andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator. Den här artikeln är också tillgängligt för [virtuella Windows-datorer](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

I hello **Resource Manager** distributionsmodell, slutpunkter konfigureras med hjälp av **Nätverkssäkerhetsgrupper (NSG: er)**. Mer information finns i [öppna portar och slutpunkter](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

När du skapar en virtuell Linux-dator i hello Azure-portalen skapas en slutpunkt för SSH (Secure Shell) vanligtvis automatiskt. Du kan konfigurera ytterligare slutpunkter när du skapar virtuella hello eller senare efter behov.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nästa steg
* Du kan också skapa en VM-slutpunkt med hjälp av hello [Azure-kommandoradsgränssnittet](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Kör hello **azure vm endpoint skapa** kommando.
* Om du har skapat en virtuell dator i hello Resource Manager-distributionsmodellen kan du använda hello Azure CLI i Resource Manager-läget för[skapa nätverk säkerhetsgrupper](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol trafik toohello VM.
