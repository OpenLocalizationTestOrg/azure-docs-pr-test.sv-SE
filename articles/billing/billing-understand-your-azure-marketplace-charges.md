---
title: aaaUnderstand Azure externa avgifterna | Microsoft Docs
description: "Läs mer om fakturering för externa tjänster, tidigare känt som Marketplace, avgifter i Azure."
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d2cb28319e2ab4eff753177220993cbf94c96ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Förstå Azure faktureringen för externa serviceavgifter
Externa tjänster används toobe kallas Azure Marketplace. I allmänhet de är tjänster som publicerats av tillgängliga för tredje part för Azure men är helt integrerade i Azure. Till exempel är ClearDB och SendGrid externa tjänster som du kan köpa i Azure, men inte har publicerats av Microsoft.

När du etablerar en ny extern tjänst eller en resurs, visas en varning:

![Marketplace köpa varning](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Externa tjänster publiceras av företag som inte är Microsoft, men ibland också Microsoft-produkter kategoriseras som externa tjänster.
> 
> 

## <a name="how-external-services-are-billed"></a>Hur faktureras externa tjänster
- Externa tjänster faktureras separat. De behandlas som enskilda order i din Azure-prenumeration. hello faktureringsperioden för varje tjänst anges när du köper hello-tjänsten. Inte toobe förväxlas med hello faktureringsperiod hello prenumeration som du har handlat. Du får också separata växlar och ditt kreditkort debiteras separat.
- Varje extern tjänst har en annan fakturering modell. Vissa tjänster debiteras i en betalning per användning sätt medan andra använder en månatlig baserat betalning modell. Du behöver ett kreditkort för externa Azure-tjänster, du kan inte köpa externa tjänster med faktura lön.
- Du kan inte använda ledigt månadskrediter för externa tjänster. Om du använder en Azure-prenumeration som innehåller [ledigt krediter](https://azure.microsoft.com/pricing/spending-limits/), de kan inte vara tillämpade tooexternal tjänsten växlar. Använda ett kreditkort toopurchase externa tjänster.


## <a name="view-external-service-spending-and-history-in-hello-azure-portal"></a>Visa extern tjänsteutgifter och historik i hello Azure-portalen
Du kan visa en lista över hello externa tjänster som finns på varje prenumeration inom hello [Azure-portalen](https://portal.azure.com/): 

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello kontoadministratör.
2. I hello navmenyn väljer **prenumerationer**.
   
    ![Välj prenumerationer i hello hubbmenyn](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. I hello **prenumerationer** bladet, Välj hello prenumeration som du vill tooview och välj sedan **externa tjänster**.
   
    ![Välj en prenumeration i hello fakturering bladet](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. Du bör se alla dina externa tjänsten order, hello utgivarens namn, tjänstnivå som du har köpt, namn du gav hello resurs och aktuell status för hello. toosee tidigare räkningar, väljer du en extern tjänst.
   
    ![Välj en extern tjänst](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Härifrån kan visa du tidigare faktura belopp som inkluderar hello skatt uppdelning.
   
    ![Visa faktureringshistorik externa tjänster](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Visa extern tjänstens utgifter för kunder med Enterprise-avtal (EA)
EA kunder kan se externa tjänsteutgifter och hämta rapporter i hello EA-portalen. Se [Azure Marketplace för EA kunder](https://ea.azure.com/helpdocs/azureMarketplace) tooget igång.

## <a name="manage-payment-methods-for-external-service-orders"></a>Hantera betalningssätt för extern tjänst order
Uppdatera betalningsmetoden för extern tjänst order från hello [Kontocenter](https://account.windowsazure.com/).

> [!NOTE]
> Om du har köpt din prenumeration med ett arbets- eller Skol-konto [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake ändras tooyour betalningsmetod.
> 
> 

1. Logga in toohello [Kontocenter](https://account.windowsazure.com/) och [navigera toohello **marketplace** fliken](https://account.windowsazure.com/Store)
   
    ![Välj marketplace i hello kontocenter](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Välj hello externa tjänster du vill toomanage
   
    ![Välj hello externa tjänster du vill toomanage](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Klicka på **Byt betalningsmetod** hello höger på sidan hello. Den här länken öppnar du tooa olika portal toomanage betalningsmetoden.
   
    ![Ordning sammanfattning](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Klicka på **redigera info** och följ instruktionerna tooupdate din betalningsinformation.
   
    ![Välj Redigera info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Avbryta en extern tjänst ordning
Om du vill toocancel beställningen extern tjänst, ta bort hello resurs i hello [Azure-portalen](https://portal.azure.com).

![Ta bort resurs](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande har frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.

