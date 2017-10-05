---
title: "Relaterade och länkade resurser i galleriet sida vid sida"
description: "Läs mer om relaterade och länkade resurser som visas i panelen galleriet med Azure preview portal."
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
ms.openlocfilehash: efa7bce26c2c4c153b083e0e34d689a11d27dd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Relaterade och länkade resurser i galleriet sida vid sida
Galleriet sida vid sida kan du hitta paneler för en viss resurs och drar dem till din aktuella bladet. Med galleriet sida vid sida kan skapa du vyer som sträcker sig över resurser. För alla angivna resurser innehåller relaterade resurser alla resurser i dess resursgruppen och alla resurser som länkar till och från resursen.

## <a name="linked-resources-in-resource-manager"></a>Länkade resurser i Resource Manager
Länka är en funktion av Resource Manager.  På så sätt kan du deklarera relationerna mellan resurser, även om de inte finnas i samma resursgrupp. Länka har ingen inverkan på körning av dina resurser, utan inverkan på fakturering och ingen inverkan på rollbaserad åtkomst.  Det är bara en mekanism som du kan använda för att representera relationer så att verktyg som sida vid sida-galleriet kan tillhandahålla en omfattande hantering.  Din verktyg kan inspektera länkar med hjälp av länkarna API och tillhandahålla anpassade hantering inträffar också. 

## <a name="how-do-i-link-my-resources"></a>Hur länkar mina resurser?
När du skapar resurser via portalen eller genom att distribuera en mall med Azure PowerShell eller Azure CLI, skapas automatiskt länkar för vissa beroende resurser. Du kan också programmässigt länka resurser med hjälp av den [länkade resurser REST API](/rest/api/resources/resourcelinks).

## <a name="next-steps"></a>Nästa steg
* Om du behöver en introduktion till skrivning Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).
* Om du vill veta mer om hur du arbetar med resursgrupper via portalen, se [hantera Azure-resurser med hjälp av Azure portal](../azure-resource-manager/resource-group-portal.md).

