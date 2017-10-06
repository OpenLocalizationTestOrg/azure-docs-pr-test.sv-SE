---
title: "aaaMigrate en aktiv DNS-namnet tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur toomigrate ett anpassade DNS-namn som redan har tilldelats tooa live plats tooAzure Apptjänst utan någon avbrottstid."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a>Migrera en aktiv DNS-namnet tooAzure Apptjänst

Den här artikeln visar hur toomigrate en aktiv DNS namn för[Azure App Service](../app-service/app-service-value-prop-what-is.md) utan driftavbrott.

När du migrerar en levande plats och dess DNS-domänen namnet tooApp tjänsten fungerar det DNS-namnet redan live trafik. Du kan undvika avbrott i DNS-matchning under hello migreringen genom att binda hello aktiva DNS-namnet tooyour Apptjänst-app förebyggande syfte.

Om du inte oroar avbrott i DNS-matchning, se [mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Krav

Det här anvisningar toocomplete:

- [Kontrollera att din Apptjänst-app inte är kostnadsfria nivån](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-hello-domain-name-preemptively"></a>Binda hello domännamn förebyggande syfte

När du binder en anpassad domän förebyggande syfte uppnå både hello följande innan du gör några ändringar i DNS-posterna:

- Verifiera ditt ägarskap för domänen
- Aktivera hello domännamn för din app

När du migrerar slutligen din anpassade DNS-namnet från hello gamla platsen toohello Apptjänst-app måste finnas det utan avbrott i DNS-matchning.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Skapa post för verifiering av domän

tooverify domän ägarskap, Lägg till en TXT registrera. hello TXT-posten mappar från _awverify.&lt; underdomän >_ too_&lt;appname >. azurewebsites.net_. 

hello TXT-posten som du behöver beror på hello DNS-posten som du vill toomigrate. Exempel finns i följande tabell hello (`@` vanligtvis representerar hello rotdomänen):  

| DNS-post-exempel | TXT-värden | TXT-värde |
| - | - | - |
| @ (rot) | _awverify_ | _&lt;programnamn >. azurewebsites.net_ |
| www (underordnade) | _awverify.www_ | _&lt;programnamn >. azurewebsites.net_ |
| \*(jokertecken) | _awverify.\*_ | _&lt;programnamn >. azurewebsites.net_ |

Observera hello posttyp för hello DNS-namn som du vill använda toomigrate på sidan DNS-poster. Apptjänst stöder mappningar från CNAME- och A-poster.

### <a name="enable-hello-domain-for-your-app"></a>Aktivera hello domän för din app

I hello [Azure-portalen](https://portal.azure.com), i hello vänstra navigeringsfönstret hello app sidan, Välj **anpassade domäner**. 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

I hello **anpassade domäner** sidan, Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typen hello fullständigt kvalificerat domännamn som du har lagt till hello TXT-posten för, exempelvis `www.contoso.com`. För en jokerteckendomän med (t.ex. \*. contoso.com), du kan använda DNS-namn som matchar hello jokerteckendomän. 

Välj **Validera**.

Hej **lägga till värdnamnet** knappen aktiveras. 

Se till att **Hostname posttyp** toohello DNS-posttyp du vill toomigrate anges.

Välj **lägga till värdnamnet**.

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan. Försök att uppdatera hello webbläsare tooupdate hello data.

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Din anpassade DNS-namn är nu aktiverad i din Azure-app. 

## <a name="remap-hello-active-dns-name"></a>Mappa om hello aktiva DNS-namn

hello endast sak vänstra toodo ommappning din aktiva DNS-poster toopoint tooApp Service. Höger nu kan den fortfarande pekar tooyour gamla platsen.

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a>Kopiera hello app IP-adress (endast en post)

Om du mappa en CNAME-post, hoppar du över det här avsnittet. 

tooremap en A-post och du behöver hello Apptjänst appens externa IP-adress som visas i hello **anpassade domäner** sidan.

Stäng hello **lägga till värdnamnet** sidan genom att välja **X** i hello övre högra hörnet. 

I hello **anpassade domäner** sidan, kopiera hello app IP-adress.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a>Uppdatera hello DNS-post

Tillbaka på hello DNS-poster sidan i din domänleverantör, väljer du hello DNS-poster tooremap.

För hello `contoso.com` rot domän exempel, mappa om hello A- eller CNAME-post som hello exemplen i följande tabell hello: 

| FQDN-exempel | Typ av post | Värd | Värde |
| - | - | - | - |
| Contoso.com (rot) | A | `@` | IP-adress från [kopiera hello app IP-adress](#info) |
| www.contoso.com (underordnade) | CNAME | `www` | _&lt;programnamn >. azurewebsites.net_ |
| \*. contoso.com (jokertecken) | CNAME | _\*_ | _&lt;programnamn >. azurewebsites.net_ |

Spara dina inställningar.

DNS-frågor ska starta lösa tooyour Apptjänst-app omedelbart efter DNS-spridningen händer.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toobind anpassat SSL-certifikatet tooApp Service.

> [!div class="nextstepaction"]
> [Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)
