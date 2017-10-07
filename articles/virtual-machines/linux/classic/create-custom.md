---
title: "aaaCreate en klassisk virtuell Linux-dator med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate en Linux-dator med hello Azure CLI 1.0 med hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a>Hur tooCreate en klassisk virtuell Linux-dator med hello Azure CLI 1.0
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Hello Resource Manager version finns [här](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Det här avsnittet beskrivs hur toocreate en Linux-dator (VM) hello Azure CLI 1.0 med hello klassiska distributionsmodellen. Vi använder en Linux-avbildning från hello tillgängliga **bilder** på Azure. hello Azure CLI 1.0 kommandon ge hello följande konfigurationsalternativ, bland annat:

* Ansluta hello VM tooa virtuella nätverk
* Lägga till hello VM tooan befintlig molntjänst
* Lägga till hello VM tooan befintligt lagringskonto
* Lägger till hello VM tooan tillgänglighetsuppsättning eller plats

> [!IMPORTANT]
> Om du vill att din VM toouse ett virtuellt nätverk så att du kan ansluta tooit direkt av värdnamn eller konfigurera anslutningar mellan platser, se till att ange hello virtuellt nätverk när du skapar hello VM. En virtuell dator kan vara konfigurerade toojoin ett virtuellt nätverk endast när du skapar hello VM. Mer information om virtuella nätverk finns [Azure översikt över virtuella nätverk](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a>Hur toocreate en Linux VM som använder hello klassiska distributionsmodellen
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

