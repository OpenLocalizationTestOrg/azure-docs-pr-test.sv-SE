---
title: "aaaSet av slutpunkter på en klassiska Windows VM | Microsoft Docs"
description: "Lär dig tooset av slutpunkter för en virtuell Windows-dator i hello Azure klassiska portal tooallow kommunikation med en Windows-dator i Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Hur tooset av slutpunkter för klassiska Windows-dator i Azure
Alla Windows virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen automatiskt kan kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma molntjänst eller ett virtuellt nätverk. Datorer på hello Internet eller andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator. Den här artikeln är också tillgängligt för [virtuella Linux-datorer](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

I hello **Resource Manager** distributionsmodell, slutpunkter konfigureras med hjälp av **Nätverkssäkerhetsgrupper (NSG: er)**. Mer information finns i [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

När du skapar en virtuell Windows-dator i hello Azure-portalen skapas vanliga slutpunkter som de för fjärrskrivbord och Windows PowerShell-fjärrkommunikation vanligtvis automatiskt. Du kan konfigurera ytterligare slutpunkter när du skapar virtuella hello eller senare efter behov.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nästa steg
* toouse en Azure PowerShell-cmdlet tooset in en VM-slutpunkt finns [Lägg till AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* toouse toomanage en Azure PowerShell-cmdlet: en ACL för en slutpunkt finns [hantera åtkomstkontrollistor (ACL) för slutpunkter med hjälp av PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Om du har skapat en virtuell dator i hello Resource Manager-distributionsmodellen kan du använda Azure PowerShell för[skapa nätverk säkerhetsgrupper](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol trafik toohello VM.
