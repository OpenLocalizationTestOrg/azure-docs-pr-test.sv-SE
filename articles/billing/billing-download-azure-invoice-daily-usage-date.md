---
title: "aaaDownload Azure faktura och daglig användning faktureringsinformation | Microsoft Docs"
description: "Beskriver hur toodownload eller visa Azure faktureringen faktura och dagliga användningsdata."
keywords: "fakturering faktura, ladda ned faktura, azure faktura, azure användning"
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2826df10f39914fcaeb9985271dadde550c68dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Hämta eller visa din Azure fakturering faktura och dagliga användningsdata
Du kan hämta fakturan från hello [Azure-portalen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) eller låta skickas via e-post. toodownload det dagliga arbetet, gå toohello [Azure Kontocenter](https://account.windowsazure.com). Endast vissa roller har behörigheten tooget fakturering faktura och användning av information, t.ex. hello kontoadministratör. toolearn mer information om hur du får åtkomst toobilling information, se [hantera åtkomst tooAzure fakturering med roller](billing-manage-access.md).

## <a name="get-your-invoice-in-email-pdf"></a>Hämta fakturan i e-post (PDF)
Du kan välja och konfigurera ytterligare mottagare tooreceive din Azure fakturerar i ett e-postmeddelande. Den här funktionen är kanske inte tillgängliga för vissa prenumerationer som stöd för erbjudanden, Enterprise-avtal eller Azure i Open.

1. Välj din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Anmäl för varje prenumeration som du äger. Klicka på **fakturor** sedan **e-min faktura**. 

    ![Skärmbild som visar hello Anmäl flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klicka på **välja** och acceptera villkoren hello.

    ![Skärmbild som visar hello Anmäl flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. När du har accepterat avtalet hello, kan du konfigurera ytterligare mottagare.

    ![Skärmbild som visar hello Anmäl flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Om du inte får ett e-postmeddelande när du har följt stegen hello, kontrollera din e-postadress är korrekt i hello [kommunikationsinställningar i din profil](https://account.windowsazure.com/profile).

## <a name="download-invoice-from-azure-portal-pdf"></a>Ladda ned faktura från Azure-portalen (PDF)

1. Välj din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen som [en användare med åtkomst tooinvoices](billing-manage-access.md).

2. Välj **fakturor**. 

    ![Skärmbild som visar hello fakturerings- och alternativet](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Klicka på **ladda ned faktura** tooview en kopia av fakturan PDF. Om det står **inte tillgänglig**, se [Varför ser jag en faktura för hello senaste faktureringsperiod?](#noinvoice)

    ![Skärmbild som visar fakturering punkter, hello hämtningsalternativ och totala avgifter för varje faktureringsperioden](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Du kan också visa det dagliga arbetet genom att klicka på hello faktureringsperioden. 

Mer information om fakturan finns [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md). Hjälp med att hantera kostnader finns [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing-getting-started.md).

## <a name="download-usage-from-hello-account-center-csv"></a>Ladda ned användning från hello Account Center (.csv)

1. Logga in på hello [Azure Kontocenter](https://account.windowsazure.com/subscriptions) som hello kontoadministratör.

2. Välj hello prenumeration som du vill ha information om hello faktura och användning.

3. Välj **FAKTURERING HISTORIK**. 

    ![Skärmbild som visar historik betalningsalternativ](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Du kan se dina rapporter för hello senaste sex fakturering perioderna och hello aktuella ofakturerad tidsperiod. 

    ![Skärmbild som visar fakturering punkter, alternativ toodownload faktura och daglig användning och totala debiteringen för varje faktureringsperioden](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Välj **Visa aktuella instruktionen** toosee en uppskattning av dina debiteringar på hello tid hello uppskattning genererades. Den här informationen uppdateras bara dagligen och får inte innehålla din användning. Fakturan månatliga kan skilja sig från den här beräkningen.

    ![Skärmbild som visar hello Visa aktuella instruktionen alternativet](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Skärmbild som visar hello uppskattning av aktuella avgifter](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Välj **ladda ned användning** toodownload hello dagliga användningsdata som en CSV-fil. Om du ser två versioner som är tillgängliga, hämta version 2.

    ![Skärmbild som visar hello ladda ned användning alternativet](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Endast hello kontoadministratör kan komma åt hello Azure Kontocenter. Andra fakturering administratörer, till exempel en ägare kan hämta information om användning med hello [fakturerings-API: er](billing-usage-rate-card-overview.md).

Mer information om det dagliga arbetet finns [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md). Hjälp med att hantera kostnader finns [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing-getting-started.md).

## <a name="noinvoice"></a>Varför ser jag en faktura för hello senaste faktureringsperioden

Det kan finnas flera skäl till att du inte ser en faktura:

- Du har ett månatliga kreditbelopp med din prenumeration som du inte överskrider eller du har en kostnadsfri utvärderingsversion. En faktura skapas bara när du är skyldig pengar.

- Det är mindre än 30 dagar från hello dag som du prenumererar på tooAzure.

- hello fakturan är inte genererats ännu. Vänta tills hello slutet av hello faktureringsperioden.

- Om du inte är hello kontoadministratören kanske inte äldre fakturor avaialbe tooyou.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande har fler frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.

