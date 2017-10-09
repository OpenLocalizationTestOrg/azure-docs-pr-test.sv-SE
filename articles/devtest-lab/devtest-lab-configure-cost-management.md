---
title: "aaaView hello månatliga beräknade labbkostnaden kostnadstrend i Azure DevTest Labs | Microsoft Docs"
description: "Läs mer om hello Azure DevTest Labs månatliga uppskattade kostnaden trenddiagram."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Visa hello månatliga beräknade labbkostnaden kostnadstrend i Azure DevTest Labs
hanteringsfunktionen för hello kostnaden för DevTest Labs hjälper dig att spåra hello kostnaden för ditt labb. Den här artikeln beskrivs hur toouse hello **månatlig uppskattad Kostnadstrend** diagram tooview hello aktuella kalendermånaden uppskattade kostnaden-till-date och hello planerade sista månad kostnaden för hello aktuella kalendermånaden. I den här artikeln lär du dig hur tooview hello månatliga uppskattade kostnaden trenddiagram i hello Azure-portalen.

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a>Visar hello månatlig uppskattad Kostnadstrend diagram
tooview hello månatlig uppskattad Kostnadstrend diagram, Följ dessa steg: 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.   
4. På bladet hello lab väljer **kostnad inställningar**.
5. På hello lab **kostnad inställningar** bladet väljer **Lab kostnadstrend**.
6. hello visar följande skärmbild ett exempel på ett diagram med kostnaden. 
   
    ![Kostnad diagram](./media/devtest-lab-configure-cost-management/graph.png)

Hej **uppskattade kostnaden** värde är hello aktuella kalendermånaden uppskattade kostnaden hittills. Hej **planerad kostnad** hello uppskattade kostnaden för hela hello aktuella kalendermånaden beräknas med hjälp av hello lab kostnaden för hello tidigare fem dagar.

hello kostnadsbelopp avrundas toohello närmaste heltal. Exempel: 

* 5.01 Avrundar uppåt too6 
* 5.50 Avrundar uppåt too6
* 5.99 Avrundar uppåt too6

Eftersom den anger över hello diagram hello kostnader som du ser i diagrammet hello är *uppskattade* kostnader med [betala per användning](https://azure.microsoft.com/offers/ms-azr-0003p/) erbjuda priser.
Dessutom hello följande är *inte* med i beräkningen hello:

* CSP och Dreamspark-prenumerationer stöds inte för närvarande eftersom Azure DevTest Labs använder hello [Azure fakturering API: er](../billing/billing-usage-rate-card-overview.md) toocalculate hello lab kostnad, som inte stöder CSP eller Dreamspark-prenumerationer.
* Allteftersom-taxan. Vi är för närvarande inte kan toouse din allteftersom-taxan (visas i din prenumeration) att du har förhandlats med Microsoft eller Microsoft partner. Vi använder betala per användning priser.
* Din skatter
* Din rabatt
* Valuta. För närvarande visas hello lab kostnaden endast i USD valuta.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Två flera saker tookeep dina kostnader på spår i DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Varför kostnad tröskelvärden?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Nästa steg
Här följer några saker tootry bredvid:

* [Definiera principer för labbet](devtest-lab-set-lab-policy.md) – Lär dig hur tooset hello olika principer används toogovern hur ditt labb och dess virtuella datorer används. 
* [Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning. Den här artikeln visar hur toocreate en anpassad avbildning från en VHD-fil.
* [Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) - DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder. Den här artikeln beskrivs hur toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb.
* [Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur toocreate en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur toowork med artefakter i den virtuella datorn.

