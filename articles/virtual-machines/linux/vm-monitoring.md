---
title: "aaaEnable eller inaktivera Azure VM-övervakning"
description: "Beskriver hur tooEnable eller inaktivera Azure VM-övervakning"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Aktivera eller inaktivera övervakning på Azure VM

Det här avsnittet beskrivs hur tooenable eller inaktivera övervakning på virtuella datorer som körs på Azure. Du kan aktivera eller inaktivera övervakning med hjälp av hello-portalen eller Azure-kommandoradsgränssnittet för Mac, Linux och Windows (hello Azure CLI).

> [!IMPORTANT]
> Det här dokumentet beskriver version 2.3 av hello diagnostiska Linux-tillägg, som har tagits bort. Version 2.3 kommer att stödjas tills 30 juni 2018.
>
> Version 3.0 av hello Linux diagnostiska tillägget kan aktiveras i stället. Mer information finns i [hello dokumentationen](./diagnostic-extension.md).

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a>Aktivera / inaktivera övervakning via hello Azure-portalen

Du kan aktivera övervakning av din Azure VM, som ger data om din instans under perioder på 1 minut. (storage ändringarna). Detaljerad diagnostikdata är sedan tillgängliga för hello VM i hello portal diagram eller via hello API. Som standard aktiverar Azure-portalen värdbaserad övervakning av en begränsad uppsättning mått. Du kan aktivera övervakning av mått på en virtuell dator när hello virtuella datorn körs eller i ett stoppat tillstånd.

* Öppna hello Azure portal på [https://portal.azure.com](https://portal.azure.com).
* I vänster navigeringsfält hello, klickar du på virtuella datorer.
* Välj en igång eller stoppad instans i hello lista över virtuella datorer. Hej ”virtuell dator” blad öppnas.
* Klicka på alla inställningar.
* Klicka på diagnostik.
* Ändra status tooOn eller Off. Du kan också hämta i det här bladet hello nivån för övervakning information som du vill använda tooenable för den virtuella datorn.

![Aktivera eller inaktivera övervakning via hello Azure-portalen.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Aktivera / inaktivera övervakning med Azure CLI

tooenable övervakning för en Azure VM.

* Skapa en fil (med namnet, till exempel PrivateConfig.json):

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* Aktivera hello tillägget via Azure CLI.

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

Mer information finns i [med Linux diagnostiska tillägget tooMonitor Linux VM prestanda och diagnostikdata](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
