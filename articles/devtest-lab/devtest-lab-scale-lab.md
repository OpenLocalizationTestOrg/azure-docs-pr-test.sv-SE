---
title: "aaaScale kvoter och gränser i ditt labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooscale ett labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: tarcher
ms.openlocfilehash: 7fb429c0aabdfbce3a4a5aa6d9259daa2ee270d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Skala kvoter och gränser i DevTest Labs
När du arbetar i DevTest Labs märker du att det finns vissa standard gränser toosome Azure-resurser, vilket kan påverka hello DevTest Labs service. Dessa gränser är refererad tooas **kvoter**.

> [!NOTE]
> Hej DevTest Labs service införa inte kvoter. Kvoter som kan uppstå är standardbegränsningar av hello övergripande Azure-prenumeration.

Du kan använda varje Azure-resurs tills du når sin kvot. Varje prenumeration har separata kvoter och användning spåras per prenumeration.

Varje prenumeration har till exempel en standardkvot på 20 kärnor. Så om du skapar virtuella datorer i labbet med fyra kärnor, kan du bara skapa fem virtuella datorer. 

[Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits) listar några av de vanligaste hello-kvoter för Azure-resurser. hello resurser som används mest i ett labb och för vilket du kan stöta på kvoter, inkludera VM kärnor, offentliga IP-adresser, gränssnitt, hanterade diskar, RBAC rolltilldelning och ExpressRoute-kretsar.

## <a name="view-your-usage-and-quotas"></a>Visa användnings- och kvoter
Dessa steg visar hur tooview hello aktuella kvoter i din prenumeration för specifika Azure-resurser och toosee vilken procentandel av varje kvot som du har använt.

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **fakturering** hello-listan.
1. Välj en prenumeration i hello fakturerings-bladet.
4. Välj **användning + kvoter**.

   ![Knappen användning och kvoter](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Hej användning + kvoter bladet visas med olika resurser som är tillgängliga i den prenumerationen och hello procentandelen hello kvot som används per resurs.

   ![Kvoter och användning](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Begär fler resurser i din prenumeration
Om du når ett kvoten tak hello Standardgränsen för en resurs i en prenumeration kan du öka upp tooa gränsvärdet, enligt beskrivningen i [Azure-prenumeration och Tjänstbegränsningarna](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Dessa steg visar hur toorequest en kvot öka via hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**väljer **fakturering**, och välj sedan **användning + kvoter**.
1. Välj hello i hello användning + kvoter bladet **begära öka** knappen.

   ![Knappen för ökning av begäran](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. toocomplete och skicka begäran om hello, fylla hello krävs information om alla tre flikar hello **ny supportbegäran** formuläret.

   ![Öka formulär](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Förstå Azure gränser och ökar](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) innehåller mer information om att kontakta Azure-supporten toorequest en kvot ökning.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Nästa steg
* Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
