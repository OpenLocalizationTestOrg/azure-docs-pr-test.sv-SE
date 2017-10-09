---
title: "aaaRelated och länkade resurser i hello panelen galleri"
description: "Läs mer om relaterade och länkade resurser som visas i hello panelen galleriet med hello Azure preview portal."
services: azure-portal
documentationcenter: 
author: adamabdelhamed
manager: wpickett
editor: 
ms.assetid: dba96d29-f518-49c8-bfd2-f14cecb44cbf
ms.service: azure-portal
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2015
ms.author: adamab
ms.openlocfilehash: c8f99be8e23dc9641ec3cd3169604b33a4b049f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="related-and-linked-resources-in-hello-tile-gallery"></a>Relaterade och länkade resurser i hello sida vid sida-galleriet
hello panelen galleriet kan du toofind paneler för en viss resurs och drar dem till din aktuella bladet. Du kan skapa vyer som sträcker sig över resurser med hello sida vid sida-galleriet. För alla angivna resurser relaterade hello resurser omfattar alla hello resurser i dess resursgruppen och alla resurser som länkar tooor från hello resurs.

## <a name="linked-resources-in-resource-manager"></a>Länkade resurser i Resource Manager
Länka är en funktion i hello Resource Manager.  Du kan använda toodeclare relationerna mellan resurser även om de inte finns i hello samma resursgrupp. Länka har ingen inverkan på hello körtiden för dina resurser, utan inverkan på fakturering och utan inverkan på rollbaserad åtkomst.  Det är bara en mekanism som du kan använda toorepresent relationer så att verktyg som hello panelen galleriet kan tillhandahålla en omfattande hantering.  Din verktyg kan inspektera hello länkar med hello länkar API och tillhandahålla anpassade hantering inträffar också. 

## <a name="how-do-i-link-my-resources"></a>Hur länkar mina resurser?
När du skapar resurser via hello portalen eller genom att distribuera en mall med Azure PowerShell eller Azure CLI, skapas automatiskt länkar för vissa beroende resurser. Du kan också programmässigt länka resurser med hjälp av hello [länkade resurser REST API](/rest/api/resources/resourcelinks).

## <a name="next-steps"></a>Nästa steg
* Om du behöver en introduktion toowriting Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).
* toounderstand mer information om hur du arbetar med resursgrupper via hello portal finns [Using hello Azure portal toomanage resurserna i Azure](../azure-resource-manager/resource-group-portal.md).

