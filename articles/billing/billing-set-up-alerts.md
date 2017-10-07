---
title: "aaaSet in fakturerings- eller varningar för Azure-prenumerationer | Microsoft Docs"
description: "Beskriver hur du kan ställa in aviseringar på fakturan Azure så att du kan undvika fakturering överraskningar."
keywords: "avisering för kredit, fakturering avisering"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Ställ in fakturerings- eller varningar för Microsoft Azure-prenumerationer
Om du är hello kontoadministratören för en Azure-prenumeration kan använda du hello Azure Billing-aviseringstjänsten toocreate anpassade fakturering aviseringar som hjälper dig att övervaka och hantera fakturerings-aktivitet för Azure-konton.

Den här tjänsten är i förhandsgranskningen, så du måste tooenable den hello förhandsgranskningsfunktioner sidan först.

## <a name="set-hello-alert-threshold-and-email-recipients"></a>Ange hello tröskelvärde och e-post signalmottagare
1. Besök [hello förhandsgranskningsfunktioner sidan](https://account.windowsazure.com/PreviewFeatures) och aktivera **fakturering aviseringstjänsten**.

1. När du har fått hello e-postbekräftelse som hello faktureringen är aktiverad för din prenumeration besöker [hello prenumerationssidan](https://account.windowsazure.com/Subscriptions) hello-kontoportalen. Klicka på hello prenumeration du vill toomonitor och klicka sedan på **aviseringar**.

    ![Skärmbild av hello Prenumerationsvy över Azure kontocenter med aviseringar markerat][Image1]

2. Klicka sedan på **Lägg till avisering** toocreate din första en. Du kan ange en summa av fem fakturaaviseringar per prenumeration med ett annat tröskelvärde och uppåt tootwo e-postmottagare för varje avisering.

    ![Skärmbild av hello aviseringar visas där du kan lägga till avisering][Image2]

3. När du lägger till en avisering du ge det ett unikt namn väljer ett tröskelvärde för utgiftsgräns och välj hello e-postadresser som aviseringar skickas. När du ställer in hello tröskelvärdet, kan du välja antingen en **Billing Total** eller en **Penningkredit** från hello **avisering för** lista. Sammanlagt fakturering skickas en avisering när prenumerationen utgifter överskrider tröskelvärdet för hello. För en penningkredit skickas en avisering när penningkrediten sjunker under hello gränsen. Penningkrediten gäller vanligtvis tooFree utvärderingsversionen och Visual Studio-prenumerationer.

    ![Skärmbild av hello avisering tillägg view, där du kan konfigurera mottagare][Image3]

Azure har stöd för en e-postadress men inte kontrollera att hello e-postadressen fungerar, så dubbelkolla för skrivfel.

## <a name="check-on-your-alerts"></a>Kontrollera på aviseringar
När du har skapat aviseringar hello Account Center visas dem och hur många fler kan du ställa in. För varje avisering finns hello datum och tid som det har skickats, oavsett om det är en avisering om Billing Total eller kredit och hello gräns som du konfigurerar. hello format för datum och tid är 24 timmar universaltid samordna (UTC) och hello datum är åååå-mm-dd-formatet. Klicka på hello plus registrera för en avisering i hello listan tooedit den eller hello-papperskorg toodelete den.

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Fakturering aviseringar för kunder med Enterprise-avtal (EA)
EA kunder kan få aviseringar för varje avdelning under en registrering med inställningen utgifter kvoter. Se [avdelning utgifter kvoter](https://ea.azure.com/helpdocs/departmentSpendingQuotas) i hello EA portal tooget igång.

## <a name="learn-more-about-azure-cost-management"></a>Lär dig mer om hantering av Azure kostnad
- Uppskatta kostnader med hello [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/), [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator), och när du lägger till en tjänst.
- [Granska din användning och kostnader regelbundet i Azure-portalen](billing-getting-started.md#costs).
- Aktivera [Azure Advisor kostnad rekommendationer](../advisor/advisor-cost-recommendations.md).

Det finns fler toolearn [Azure kostnad management vägledning](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
