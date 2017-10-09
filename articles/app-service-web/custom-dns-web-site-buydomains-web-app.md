---
title: "aaaBuy ett anpassat domännamn för Azure-Webbappar"
description: "Lär dig hur toobuy en anpassad domän namn med en webbapp i Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Köp ett anpassat domännamn för Azure-Webbappar

Apptjänst-domäner (förhandsversion) är toppnivådomäner som hanteras direkt i Azure. De gör det enkelt toomanage anpassade domäner för [Azure Web Apps](app-service-web-overview.md). Den här kursen visar hur toobuy en Apptjänst-domän och tilldela DNS-namn tooAzure Web Apps.

Den här artikeln är Azure App Service (Web Apps, API Apps, Mobilappar, Logic Apps). Azure Storage eller Azure VM finns [tilldela Apptjänst domän tooAzure VM eller Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Cloud Services, se [konfigurera ett anpassat domännamn för en Azure-molntjänst](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

* [Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.

## <a name="prepare-hello-app"></a>Förbereda hello appen

toouse anpassade domäner i Azure Web Apps ditt webbprograms [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller **Premium**). I det här steget kan se du till att hello-webbprogrammet i hello stöds prisnivån.

### <a name="sign-in-tooazure"></a>Logga in tooAzure

Öppna hello [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Navigera toohello app i hello Azure-portalen

Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på hello app.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/select-app.png)

Du ser sidan för hantering av hello av hello Apptjänst-app.  

### <a name="check-hello-pricing-tier"></a>Kontrollera hello prisnivån

Bläddra i hello vänster navigeringsfält hello app sidan, toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

aktuella hello app-nivå markeras med en blå kantlinje. Kontrollera toomake till hello appen inte hello **lediga** nivå. Anpassad DNS stöds inte i hello **lediga** nivå. 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Om hello programtjänstplanen är inte **lediga**Stäng hello **Välj din prisnivå** sidan och hoppa över för[köp hello domän](#buy-the-domain).

### <a name="scale-up-hello-app-service-plan"></a>Skala upp hello App Service-plan

Väljer du någon av hello icke kostnadsfria nivåer (**delade**, **grundläggande**, **Standard**, eller **Premium**). 

Klicka på **Välj**.

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

När du ser följande meddelande hello har hello skalningsåtgärden slutförts.

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a>Köpa hello domän

### <a name="sign-in-tooazure"></a>Logga in tooAzure
Öppna hello [Azure-portalen](https://portal.azure.com/) och logga in med ditt Azure-konto.

### <a name="launch-buy-domains"></a>Starta köp domäner
I hello **Web Apps** klickar du på hello namnet på ditt webbprogram, Välj **inställningar**, och välj sedan **anpassade domäner**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

I hello **anpassade domäner** klickar du på **köpa domäner**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a>Konfigurera hello domän köp

I hello **Service tillämpningsdomän** i hello sidan **Sök efter domän** rutan, typ hello domännamn som du vill toobuy och skriv `Enter`. hello visas föreslagna tillgängliga domäner under textrutan hello. Välj en eller flera domäner som du vill toobuy. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Klicka på hello **kontaktuppgifter** och fylla i hello domän kontaktinformation formulär. När du är klar klickar du på **OK** tooreturn toohello Service tillämpningsdomän sidan.
   
Det är viktigt att du fyller i alla obligatoriska fält med så mycket noggrannhet som möjligt. Felaktiga data kontaktinformation kan resultera i fel toopurchase domäner. 

Välj därefter hello önskade alternativ för din domän. Se följande tabell förklaringar hello:

| Inställning | Föreslaget värde | Beskrivning |
|-|-|-|
|Förnya automatiskt | **Aktivera** | Förnyar din App Service domän automatiskt varje år. Ditt kreditkort debiteras hello samma inköpspriset på hello tid för förnyelse. |
|Sekretesskydd | Aktivera | Välja för ”sekretesskydd”, som ingår i hello inköpspriset _gratis_ (utom toppnivådomäner vars register inte stöder sekretesskydd, t.ex _. co.in_, _. Co.uk_och så vidare). |
| Tilldela standard värdnamn | **www** och**@** | Välj hello önskade värdnamnsbindningar, om så önskas. När hello domän köp åtgärden är slutförd måste kan ditt webbprogram nås på hello valt värdnamn. Om hello webbprogram finns bakom [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), du inte ser hello alternativet tooassign hello rotdomänen (@), eftersom Traffic Manager har inte stöd för A-poster. Du kan ändra toohello hostname tilldelningar när hello domän köpet har slutförts. |

### <a name="accept-terms-and-purchase"></a>Acceptera villkoren och köpa

Klicka på **juridiska villkor** tooreview hello villkor och hello kostnader, klicka sedan på **köpa**.

> [!NOTE]
> Apptjänst domäner använder Azure DNS toohost hello domäner. Dessutom toohello domän registreringsavgift användningskostnader för Azure DNS tillämpas. Mer information finns i [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).
>
>

Tillbaka i hello **Service tillämpningsdomän** klickar du på **OK**. Medan hello pågår, kan du se hello följande meddelanden:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a>Testa hello värdnamn

Om du har tilldelat standard värdnamn tooyour webbprogrammet kan se du också ett fungerande meddelande för varje vald värdnamn. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Du också se hello valt värdnamn i hello **anpassade domäner** i hello sidan **värdnamn** avsnitt. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

tootest hello värdnamn navigera toohello visas värdnamn i hello webbläsare. I exemplet hello i hello föregående skärmbilden försök navigera too_kontoso.net_ och _www.kontoso.net_.

## <a name="assign-hostnames-tooweb-app"></a>Tilldela värdnamn tooweb app

Om du väljer att inte tooassign en eller flera standard värdnamn tooyour webbapp under hello köpa process eller om du inte behöver tooassign ett värdnamn i listan, kan du tilldela ett värdnamn när som helst.

Du kan också tilldela värdnamn i hello Service tillämpningsdomän tooany andra webbprogram. hello steg beror på om hello programdomänen för tjänsten och hello webbapp hör toohello samma prenumeration.

- Annan prenumeration: mappa anpassade DNS-poster från hello Service tillämpningsdomän toohello webbapp som en externt köpta domän. För information om att lägga till anpassade DNS namn tooan programdomänen för tjänsten, se [hantera anpassade DNS-poster](#custom). toomap en extern köpta domän tooa webbapp, se [mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md). 
- Samma prenumeration: Använd hello följande steg.

### <a name="launch-add-hostname"></a>Starta Lägg till värdnamn
I hello **Apptjänster** sidan, Välj hello namnet på ditt webbprogram som du vill tooassign värdnamn, Välj **inställningar**, och välj sedan **anpassade domäner**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Kontrollera att dina köpta domän visas i hello **App Service domäner** avsnittet, men inte välja den. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Alla App Service-domäner i samma prenumeration visas i hello webbapp hello **anpassade domäner** sidan. Om din domän finns i hello webbapp prenumeration, men du kan se den i hello webbapp **anpassade domäner** och prova att öppna hello **anpassade domäner** sidan eller uppdatera hello webbsida. Kontrollera också hello notification bell hello överst i hello Azure-portalen för pågår eller skapa fel.
>
>

Välj **lägga till värdnamnet**.

### <a name="configure-hostname"></a>Konfigurera värdnamnet
I hello **lägga till värdnamnet** dialogrutan, typen hello fullständigt kvalificerade domännamnet för din App Service-domän eller en underdomän. Exempel:

- kontoso.NET
- www.kontoso.NET
- ABC.kontoso.NET

När du är klar väljer **verifiera**. hello hostname-posttypen väljs automatiskt för dig.

Välj **lägga till värdnamnet**.

När hello åtgärden är slutförd måste se du ett lyckade meddelande för hello tilldelade värdnamn.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Stäng lägga till värdnamnet
I hello **lägga till värdnamnet** anger du eventuella andra hostname tooyour webbapp som du vill. När du är klar stänger du hello **lägga till värdnamnet** sidan.

Du bör nu se hello som tilldelats nyligen hostname(s) i din app **anpassade domäner** sidan.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a>Testa hello värdnamn

Navigera toohello visas värdnamn i hello webbläsare. Försök navigera too_abc.kontoso.net_ i hello exemplet i hello föregående skärmbild.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Hantera anpassade DNS-poster

I Azure, DNS-poster för en tillämpningsdomän Service hanteras med hjälp av [Azure DNS](https://azure.microsoft.com/services/dns/). Du kan lägga till, ta bort, och uppdatera DNS-poster, precis som för en externt köpta domän.

### <a name="open-app-service-domain"></a>Öppna App Service-domän

Välj i hello Azure-portalen hello vänstra menyn **fler tjänster** > **App Service domäner**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Välj hello domän toomanage. 

### <a name="access-dns-zone"></a>Åtkomst till DNS-zonen

Välj hello domän vänstra menyn **DNS-zonen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Den här åtgärden öppnar hello [DNS-zonen](../dns/dns-zones-records.md) sidan i din App Service-domän i Azure DNS. Mer information om hur tooedit DNS-poster finns [hur toomanage DNS-zoner i hello Azure-portalen](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Avbryt inköp (ta bort domän)

När du har köpt hello App Service-domän har du fem dagar toocancel köpet för en full återbetalning. Efter fem dagar du kan ta bort hello programdomänen för tjänsten, men det går inte att få en återbetalning.

### <a name="open-app-service-domain"></a>Öppna App Service-domän

Välj i hello Azure-portalen hello vänstra menyn **fler tjänster** > **App Service domäner**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Välj hello domän tooyou vill toocancel eller ta bort. 

### <a name="delete-hostname-bindings"></a>Ta bort värdnamnsbindningar

Välj hello domän vänstra menyn **värdnamnsbindningar**. Hej värdnamnsbindningar från alla Azure-tjänster i den här listan.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Du kan inte ta bort hello Service tillämpningsdomän förrän alla värdnamnsbindningar har tagits bort.

Ta bort varje värdnamn bindning genom att välja **...**   >  **Ta bort**. När alla hello bindningar tas bort, Välj **spara**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Avbryt eller ta bort

Välj hello domän vänstra menyn **översikt**. 

Om hello annullering hello köpt domän inte har gått, väljer **Avbryt inköp**. Annars kan du se en **ta bort** knappen i stället. toodelete hello domän utan bidrag, Välj **ta bort**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Välj **OK** tooconfirm hello igen. Om du inte vill tooproceed Klicka utanför hello bekräftelsedialogruta.

När hello åtgärd har slutförts, hello domänen är utgivna från din prenumeration och är tillgängliga för alla toopurchase igen. 
