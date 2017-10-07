---
title: aaaCommunity-verktyg, flytta klassiska resurser tooAzure Resource Manager | Microsoft Docs
description: "Den här artikeln kataloger hello verktyg som tillhandahålls av hello community toohelp migrera IaaS-resurser från klassiska toohello Azure Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 67d5624a14d12a2d9eb46eb12aef461b706d4589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="community-tools-toomigrate-iaas-resources-from-classic-tooazure-resource-manager"></a>Gemenskapen verktygen toomigrate IaaS-resurser från klassiska tooAzure Resource Manager
Den här artikeln kataloger hello verktyg som tillhandahålls av hello community tooassist migreringen av IaaS-resurser från klassiska toohello Azure Resource Manager-distributionsmodellen.

> [!NOTE]
> Dessa verktyg officiellt stöds inte av Microsoft-supporten. Därför är öppen källkod på GitHub och vi är glada tooaccept fso för korrigeringar eller fler scenarier. tooreport ett problem med hello GitHub utfärdar funktionen.
> 
> Migrera med de här verktygen kommer driftstopp för dina klassiska virtuella datorer. Om du letar efter migrering plattform som stöds finns 
> 
>   * [Migrering av plattform som stöds av IaaS-resurser från klassiska tooAzure Resource Manager-stacken](migration-classic-resource-manager-overview.md)
>   * [Tekniska ingående om plattformen stöds migreringen från klassiskt tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migrera IaaS-resurser från klassiska tooAzure Resource Manager med Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Detta är en samling helper-verktyg som skapats som en del av enterprise migrering från Azure-tjänsthantering tooAzure Resource Manager. Det här verktyget kan du tooreplicate infrastrukturen till en annan prenumeration som kan användas för att testa migrering och Stryk eventuella problem innan du kör hello migrering på prenumerationen i produktion.

[Länken toohello verktyget dokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz är en ytterligare alternativ toomigrate en fullständig uppsättning klassiska IaaS resurser tooAzure Resource Manager IaaS-resurser. hello migrering kan ske inom hello samma prenumeration eller mellan olika prenumerationer och prenumerationstyper (ex: CSP prenumerationer).

[Länken toohello verktyget dokumentation](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Nästa steg

* [Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

