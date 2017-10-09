---
title: "aaaPrevent oväntat kostnader, hantera fakturerings - Azure | Microsoft Docs"
description: "Lär dig hur tooavoid oväntat avgifter på fakturan Azure. Använda funktionerna för hantering och uppföljning av kostnaden för en Microsoft Azure-prenumeration."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: tonguyen
experimental_id: a2b2579c-cd2e-41
ms.openlocfilehash: 4827c65a55fe953c329ab26cc4e882266073c60a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prevent-unexpected-costs-with-azure-billing-and-cost-management"></a>Förhindra att oväntade kostnader med Azure fakturerings- och kostnaden management

När du registrerar dig för Azure finns det flera saker du kan göra tooget en bättre uppfattning av dina utgifter. I hello [Azure-portalen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), när du väljer hello prenumeration, du kan se din aktuella kostnaden nedbrytning och bränna hastighet. Du kan också [hämta gamla fakturor och användningsfiler i detalj](billing-download-azure-invoice-daily-usage-date.md). Om du vill toogroup kostnader för resurser som används för olika projekt eller team kan titta på [resurs taggning](../azure-resource-manager/resource-group-using-tags.md). Om din organisation har ett rapporteringssystem som du föredrar toouse, kolla hello [fakturering API: er](billing-usage-rate-card-overview.md). 

Mer information om det dagliga arbetet finns [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md).

Om din prenumeration via en Enterprise-avtal (EA), Cloud Solution Providers (CSP) eller Azure sponsring gäller inte många funktioner i den här artikeln tooyou. I stället har vi en annan uppsättning av verktyg som du kan använda för hantering av kostnaden. Se [ytterligare resurser för EA och CSP sponsring](#other-offers).

Om din prenumeration är en kostnadsfri utvärderingsversion [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure i Open (AIO) eller BizSpark, Lär dig sedan [utgiftsgränser](#spending-limit) tooavoid med din prenumeration unexpectantly inaktiverad. 

## <a name="day-0-before-you-add-azure-services"></a>Dag 0: Innan du lägger till Azure-tjänster

### <a name="estimate-cost-online-using-hello-pricing-calculator"></a>Uppskattade kostnaden online med hello prisnivå Kalkylatorn

Kolla in hello [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/) och [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator) tooget en uppskattning hello månadskostnaden för hello-tjänsten som du är intresserad av. A1 Windows virtuell dator (VM) är till exempel uppskattade toocost $66.96 USD per månad i compute timmar om du låter rutan vara igång hela tiden för hello:

![Skärmbild av hello prisnivå Kalkylatorn visar att en A1 Windows VM är uppskattade toocost $66.96 USD per månad](./media/billing-getting-started/pricing-calcVM.png)

Mer information finns i [priser vanliga frågor och svar](https://azure.microsoft.com/pricing/faq/). Eller om du vill tootalk tooa person kan anropa 1-800-867-1389.

### <a name="check-your-subscription-and-access"></a>Kontrollera din prenumeration och åtkomst

Visa kostnaderna kräver [prenumerationer behörighet toobilling information](billing-manage-access.md), men endast hello kontoadministratören kan komma åt hello [Kontocenter](https://account.windowsazure.com/Home/Index), ändra faktureringsinformationen och hantera prenumerationer. hello kontoadministratören är hello person som har gått igenom hello registreringsprocessen. Mer information finns i [lägga till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster](billing-add-change-azure-subscription-administrator.md).

toosee om du använder hello kontoadministratören, gå toohello [prenumerationer bladet i hello Azure-portalen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) och titta på hello listan över prenumerationer som du har åtkomst till. Tittar du under **min roll**. Om det står *kontoadministratören*, och du är ok. Om det står något annat som *ägare*, och du inte har fullständig behörighet.

![Skärmbild av din roll i hello Prenumerationsvy i hello Azure-portalen](./media/billing-getting-started/sub-blade-view.PNG)

Om du inte är hello kontoadministratören och sedan någon förmodligen gav dig delvis åtkomst via [Azure Active Directory-rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md) (RBAC). toomanage prenumerationer och ändrar fakturering info, [hitta hello kontoadministratören](billing-subscription-transfer.md#whoisaa) och be dem tooperform hello uppgifter eller [överför hello prenumeration tooyou](billing-subscription-transfer.md).

Om din kontoadministratör är inte längre med din organisation och du behöver toomanage fakturering [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

### <a name="spending-limit"></a>Kontrollera att du har en utgiftsgräns

Om du har en prenumeration som använder krediter sedan aktiveras hello utgiftsgräns du som standard. Det här sättet när du lägger till alla dina krediter ditt kreditkort inte hämta debiteras. Se hello [fullständig lista över Azure erbjudanden och hello tillgängligheten för utgiftsgräns](https://azure.microsoft.com/support/legal/offer-details/).

Om du har nått din utgiftsgräns inaktiveras dock dina tjänster. Det innebär att dina virtuella datorer har frigjorts. tooavoid stilleståndstiden, måste du inaktivera hello utgiftsgräns. Alla överförbrukning hämtar debiteras till kreditkortet på filen. 

toosee om du har stött på utgiftsgräns, gå toohello [prenumerationer visa i hello Kontocenter](https://account.windowsazure.com/Subscriptions). En banderoll visas om din utgiftsgräns finns på:

![Skärmbild som visar en varning om utgifter gränsen som på i hello Account Center](./media/billing-getting-started/spending-limit-banner.PNG)

Klicka på hello banderoll och följ anvisningarna för tooremove hello utgiftsgräns. Om du inte ange kreditkortsinformation när du registrerade dig, måste du ange den tooremove hello utgiftsgräns. Mer information finns i [Azure utgiftsgränsen – hur det fungerar och hur tooenable eller ta bort den](https://azure.microsoft.com/pricing/spending-limits/).

### <a name="set-up-billing-alerts"></a>Ställa in faktureringsvarningar

Ställ in fakturaaviseringar tooget e-postmeddelanden när förbrukningskostnader överskrider ett belopp som du anger. Om du har månatliga krediter ställa in aviseringar för när du använder upp en angiven mängd. Mer information finns i [konfigurera fakturering aviseringar för Microsoft Azure-prenumerationer](billing-set-up-alerts.md).

![Skärmbild av e-post för fakturering avisering](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Den här funktionen är fortfarande under förhandsgranskning så du bör regelbundet kontrollera din användning.

Du kanske vill toouse hello kostnadsuppskattning från hello priskalkylator som riktlinje för din första aviseringen.

### <a name="understand-limits-and-quotas-for-your-subscription"></a>Förstå begränsningar och kvoter för din prenumeration

Det finns begränsningar för standard tooeach prenumerationen för sådant som hello antalet processorkärnor och IP-adresser. Tänk också på dessa begränsningar. Mer information finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md). Du kan begära en ökad tooyour gräns eller kvot genom [att kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="day-1-as-you-add-services"></a>Dag 1: När du lägger till tjänster

### <a name="review-hello-estimated-cost-in-hello-portal"></a>Granska hello uppskattade kostnaden i hello-portalen

När du lägger till en tjänst i hello Azure-portalen, är det vanligtvis en vy som visar en liknande uppskattade kostnaden per månad. Till exempel när du väljer hello storleken på din Windows-VM visas hello beräknade månadskostnaden för hello beräkningstimmar:

![Exempel: en A1 Windows VM är uppskattade toocost $66.96 USD per månad](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="tags"></a>Lägga till taggar tooyour resurser toogroup din faktureringsinformation

Du kan använda taggar toogroup faktureringsinformation för tjänster som stöds. Till exempel om du kör flera virtuella datorer för olika grupper kan du använda taggar toocategorize kostnader per kostnadsställe (HR, marknadsföring, ekonomi) eller miljö (produktion, Förproduktion, test). 

![Skärmbild som visar hur du konfigurerar taggar i hello-portalen](./media/billing-getting-started/tags.PNG)

hello taggar visas i olika kostnaden reporting vyer. Till exempel de som är synliga i din [kostnad analysen](#costs) direkt och [innehåller information om användning .csv](#invoice-and-usage) efter din första faktureringsperioden.

Mer information finns i [med hjälp av taggar tooorganize resurserna i Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Överväg att aktivera kostnaden skärande funktioner som automatisk avstängning för virtuella datorer

Beroende på ditt scenario kan du konfigurera automatisk avstängning för dina virtuella datorer i hello Azure-portalen. Mer information finns i [automatisk avstängning för virtuella datorer med Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Skärmbild av automatisk avstängning alternativet hello-portalen](./media/billing-getting-started/auto-shutdown.PNG)

Automatisk avstängning hello inte samtidigt som när du avslutar inom hello VM med Energialternativ. Automatisk avstängning stoppar och tar bort VMs-toostop extra kostnader. Mer information finns i frågor och svar om prissättning [virtuella Linux-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) och [virtuella Windows-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) om VM-tillstånd.

För mer kostnaden skärande funktioner för utvecklings- och testmiljöer, kolla [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

## <a name="cost-reporting"></a>Dag 2 +: När du använder tjänster, visa användningsinformation

### <a name="costs"></a>Regelbundet kontrollera hello portal för kostnadsuppdelning och bränna hastighet

När du har hämtat dina tjänster som körs regelbundet kontrollera hur mycket de kostnadshantering du. Du kan se hello aktuella tillbringar och bränna hastighet på Azure-portalen. 

1. Besök hello [prenumerationer bladet i Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Välj din prenumeration som du vill toosee. Du kan bara ha en tooselect.

3. Du bör se hello kostnaden nedbrytning och bränna hastighet hello popup-bladet. Den stöds inte för erbjudandet (en varning visas hello övre delen). Vänta 24 timmar efter att du lägger till en tjänst för hello data toopopulate.
    
    ![Skärmbild av bränna hastighet och analys på detaljnivå i hello Azure-portalen](./media/billing-getting-started/burn-rate.PNG)

4. Du kanske vill toopin hello visa tooyour instrumentpanelen.

    ![Skärmbild av fästa en vy toohello instrumentpanel](./media/billing-getting-started/pin.PNG)

5. Klicka på **kostnad analysis** i hello listan toohello vänstra toosee hello kostnadsuppdelning av resursen.

    ![Skärmbild av hello kostnaden analys i Azure-portalen](./media/billing-getting-started/cost-analysis.PNG)

6. Du kan filtrera efter andra egenskaper som [taggar](#tags), resursgrupp och timespan. Klicka på **tillämpa** tooconfirm hello filter och **hämta** tooexport hello visa tooa Comma-Separated fil (.csv).

7. Klicka på en resurs toosee tillbringar historik och hur mycket den kostnadshantering du varje dag.

    ![Skärmbild av hello tillbringar historikvyn i Azure-portalen](./media/billing-getting-started/costhistory.PNG)

Vi rekommenderar att du kontrollerar hello kostnader visas med hello beräknar du såg när du har valt hello-tjänster. Om hello kostnader wildly skiljer sig från beräknar, hello dubbelkolla prisavtal (A1 vs A0 VM, till exempel) som du har valt för dina resurser. 

#### <a name="view-costs-for-all-your-subscriptions-in-hello-billing-blade"></a>Visa kostnader för alla prenumerationer hello fakturerings-bladet

Om du hanterar flera prenumerationer som hello kontoadministratören kan du kan se hello sammanställd faktura belopp och sammanställning för alla prenumerationer i hello [bladet fakturering](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade). 

<!-- Add screenshots of multiple subs each with billed usage -->

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Aktivera och checka ut Azure Advisor-rekommendationer

[Azure Advisor](../advisor/advisor-overview.md) är en förhandsvisningsfunktion som hjälper dig att minska kostnaderna genom att identifiera resurser med låg belastning. Aktivera den i hello Azure-portalen:

![Skärmbild av Azure Advisor-knappen i Azure-portalen](./media/billing-getting-started/advisor-button.PNG)

Sedan kan du hämta rekommenderade åtgärder i hello **kostnaden** fliken hello Advisor instrumentpanelen:

![Skärmbild av Advisor kostnaden rekommendation exempel](./media/billing-getting-started/advisor-action.PNG)

Mer information finns i [kostnaden för Advisor-rekommendationer](../advisor/advisor-cost-recommendations.md).

### <a name="invoice-and-usage"></a>Hämta din faktura och information om användning efter din första faktureringsperioden

Du kan hämta din faktura Portable Document Format (PDF) och användningsinformation Comma-Separated värden (CSV) efter den första faktureringsperioden. Du kan också välja toohave fakturan tooyou med e-post. Dessa filer med hjälp av toounderstand vad är slutligen fakturerade tooyou efter skatt, rabatter och krediter. Om du inte har en prenumeration med betalning metoden kopplade tooyour kanske dessa filer inte tillgänglig för dig. Mer information finns i [hur tooget Azure faktureringen faktura och dagliga användningsdata](billing-download-azure-invoice-daily-usage-date.md) och [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md).

![Skärmbild av en PDF-faktura](./media/billing-getting-started/invoice.png)

hello-taggar som du ställer in tidigare visas i hello detalj användning CSV-filer:

![Skärmbild som visar taggar i hello användning CSV](./media/billing-getting-started/csv.png)

### <a name="billing-api"></a>Fakturerings-API

Använd våra API tooprogrammatically get användning faktureringsinformation. Använd hello RateCard API och hello API för användning tillsammans tooget fakturerade förbrukningen. Mer information finns i [få insikter om dina Microsoft Azure-resursförbrukning](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a>Ytterligare resurser för EA CSP och sponsring

Prata tooyour Kontoansvariga eller Azure partner tooget igång.

| Erbjudande | Resurser |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise-avtal (EA) | [EA portal](https://ea.azure.com/), [hjälp docs](https://ea.azure.com/helpdocs), och [Power BI-rapport](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Kontakta tooyour provider |
| Sponsring av Azure | [Sponsring portal](https://www.microsoftazuresponsorships.com/) |

Om du hanterar IT för stora organisationer rekommenderar vi att läsa [Azure enterprise kodskelett](../azure-resource-manager/resource-manager-subscription-governance.md) och hello [enterprise IT vitboken](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF hämtning endast på engelska).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten

Om du behöver hjälp, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
