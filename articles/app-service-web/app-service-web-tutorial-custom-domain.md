---
title: aaaMap en befintlig anpassad DNS-namnet tooAzure Web Apps | Microsoft Docs
description: "Lär dig hur tooadd en befintlig domän för anpassade DNS-namn (alternativa domän) tooa webbapp, mobilappsserverdel eller API-app i Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a>Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps

Med [Azure Web Apps](app-service-web-overview.md) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst. Den här kursen visar hur toomap en befintlig anpassad DNS namn tooAzure Web Apps.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Mappa en underdomän (till exempel `www.contoso.com`) med hjälp av en CNAME-post
> * Mappa en rotdomän (till exempel `contoso.com`) med hjälp av en A-post
> * Mappa en jokerteckendomän med (till exempel `*.contoso.com`) med hjälp av en CNAME-post
> * Automatisera domänmappning med skript

Du kan använda antingen en **CNAME-post** eller en **en post** toomap anpassade DNS-namnet tooApp Service. 

> [!NOTE]
> Vi rekommenderar att du använder en CNAME-post för alla anpassade DNS-namn utom en rotdomän (till exempel `contoso.com`).

toomigrate en levande plats och dess DNS-domänen namnet tooApp tjänsten, se [migrera en aktiv DNS-namnet tooAzure Apptjänst](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

* [Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.
* Köpa ett domännamn och kontrollera att du har åtkomst toohello DNS-registret för domänleverantören (till exempel GoDaddy).

  Till exempel tooadd DNS-poster för `contoso.com` och `www.contoso.com`, måste du vara kan tooconfigure hello DNS-inställningarna för hello `contoso.com` rotdomänen.

  > [!NOTE]
  > Om du inte har en befintlig domän namn bör du överväga att [köper en domän med hjälp av hello Azure-portalen](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-hello-app"></a>Förbereda hello appen

toomap en anpassad DNS-namnet tooa webbprogram, hello webbprogrammet [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller  **Premium**). I det här steget kan se du till att hello Apptjänst appar är i hello stöds prisnivå.

### <a name="sign-in-tooazure"></a>Logga in tooAzure

Öppna hello [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Navigera toohello app i hello Azure-portalen

Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på hello app.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/select-app.png)

Du ser sidan för hantering av hello av hello Apptjänst-app.  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a>Kontrollera hello prisnivån

Bläddra i hello vänster navigeringsfält hello app sidan, toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

aktuella hello app-nivå markeras med en blå kantlinje. Kontrollera toomake till hello appen inte hello **lediga** nivå. Anpassad DNS stöds inte i hello **lediga** nivå. 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Om hello programtjänstplanen är inte **lediga**Stäng hello **Välj din prisnivå** sidan och hoppa över för[mappa en CNAME-post](#cname).

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a>Skala upp hello App Service-plan

Väljer du någon av hello icke kostnadsfria nivåer (**delade**, **grundläggande**, **Standard**, eller **Premium**). 

Klicka på **Välj**.

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

När du ser följande meddelande hello har hello skalningsåtgärden slutförts.

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Mappa en CNAME-post

Hello självstudiekursen exempelvis du lägga till en CNAME-post för hello `www` underdomänen (till exempel `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Skapa hello CNAME-post

Lägg till en CNAME-post toomap en underdomän toohello app standard värdnamn (`<app_name>.azurewebsites.net`).

För hello `www.contoso.com` domän exempelvis lägga till en CNAME-post som mappar hello namn `www` för`<app_name>.azurewebsites.net`.

När du har lagt till hello CNAME ser hello DNS-poster sidan ut hello följande exempel:

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a>Aktivera mappning av hello CNAME-post i Azure

Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**. 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

I hello **anpassade domäner** sidan hello appen, lägga till hello fullständigt kvalificerade DNS-namn (`www.contoso.com`) toohello lista.

Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typen hello fullständigt kvalificerat domännamn som du har lagt till en CNAME-post för, exempelvis `www.contoso.com`. 

Välj **Validera**.

Hej **lägga till värdnamnet** knappen aktiveras. 

Se till att **Hostname posttyp** har angetts för**CNAME (www.example.com eller alla underdomäner)**.

Välj **lägga till värdnamnet**.

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan. Försök att uppdatera hello webbläsare tooupdate hello data.

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel på hello hello sidans nederkant.

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Mappa en A-post

Hello självstudiekursen exempelvis du lägga till en A-post för hello rotdomän (till exempel `contoso.com`). 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a>Kopiera hello app IP-adress

toomap en A-post måste hello app extern IP-adress. Du hittar den här IP-adressen i hello app **anpassade domäner** sida i hello Azure-portalen.

Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**. 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

I hello **anpassade domäner** sidan, kopiera hello app IP-adress.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a>Skapa hello A-post

toomap ett A poster tooan program kräver att Apptjänst **två** DNS-poster:

- En **A** registrera toomap toohello app IP-adressen.
- En **TXT** registrera toomap toohello app standard hostname `<app_name>.azurewebsites.net`. Apptjänst använder den här posten endast vid konfigurationen, tooverify som du äger hello anpassade domäner. När din anpassade domän har verifierats och konfigurerat i App Service kan du ta bort TXT-posten. 

För hello `contoso.com` domän exempelvis skapa hello-A och TXT-poster som enligt följande tabell toohello (`@` vanligtvis representerar hello rotdomän). 

| Typ av post | Värd | Värde |
| - | - | - |
| A | `@` | IP-adress från [kopiera hello app IP-adress](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

När hello-poster läggs hello DNS-poster sidan ser ut så hello följande exempel:

![DNS-poster sidan](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a>Aktivera hello en post mappning i hello app

Tillbaka i hello app **anpassade domäner** i hello Azure-portalen, Lägg till hello fullständigt kvalificerade DNS-namn (till exempel `contoso.com`) toohello lista.

Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Typen hello fullständigt kvalificerat domännamn som du har konfigurerat hello A-post för, exempelvis `contoso.com`.

Välj **Validera**.

Hej **lägga till värdnamnet** knappen aktiveras. 

Se till att **Hostname posttyp** har angetts för**en post (example.com)**.

Välj **lägga till värdnamnet**.

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan. Försök att uppdatera hello webbläsare tooupdate hello data.

![En post som lagts till](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel på hello hello sidans nederkant.

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Mappa en jokerteckendomän med

I kursen hello-exempel du mappa en [jokertecken DNS-namnet](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (till exempel `*.contoso.com`) toohello Apptjänst-app genom att lägga till en CNAME-post. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Skapa hello CNAME-post

Lägg till en CNAME-post toomap ett jokertecken namn toohello appens standard värdnamn (`<app_name>.azurewebsites.net`).

För hello `*.contoso.com` domän exempelvis hello CNAME-post mappas hello namnet `*` för`<app_name>.azurewebsites.net`.

När hello CNAME läggs hello DNS-poster sidan ser ut så hello följande exempel:

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a>Aktivera hello CNAME-post mappning i hello app

Nu kan du lägga till alla underdomäner som matchar hello jokertecken namn toohello app (till exempel `sub1.contoso.com` och `sub2.contoso.com` matchar `*.contoso.com`). 

Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**. 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Ange ett fullständigt kvalificerade domännamn som matchar hello jokerteckendomän (till exempel `sub1.contoso.com`), och välj sedan **verifiera**.

Hej **lägga till värdnamnet** knappen aktiveras. 

Se till att **Hostname posttyp** har angetts för**CNAME-post (www.example.com eller alla underdomäner)**.

Välj **lägga till värdnamnet**.

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan. Försök att uppdatera hello webbläsare tooupdate hello data.

Välj hello  **+**  ikonen igen tooadd ett annat värdnamn som matchar hello jokerteckendomän. Lägg exempelvis till `sub2.contoso.com`.

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testa i webbläsaren

Bläddra toohello DNS-namn som du tidigare har konfigurerat (till exempel `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, och `sub2.contoso.com`).

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Automatisera med skript

Du kan automatisera hanteringen av anpassade domäner med skript, med hjälp av hello [Azure CLI](/cli/azure/install-azure-cli) eller [Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure CLI 

hello följande kommando lägger till en konfigurerad anpassade DNS-namnet tooan Apptjänst-app. 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

Mer information finns i [mappa en anpassad domän tooa webbapp](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

hello följande kommando lägger till en konfigurerad anpassade DNS-namnet tooan Apptjänst-app. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Mer information finns i [tilldela en anpassad domän tooa webbapp](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Mappa en underdomän genom att använda en CNAME-post
> * Mappa en rotdomän med en A-post
> * Mappa en jokerteckendomän med genom att använda en CNAME-post
> * Automatisera domänmappning med skript

I förväg toohello nästa självstudiekurs toolearn hur toobind anpassat SSL-certifikatet tooa webbprogram.

> [!div class="nextstepaction"]
> [Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)
