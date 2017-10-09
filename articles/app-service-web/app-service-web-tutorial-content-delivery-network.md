---
title: aaaAdd CDN-tooan Azure App Service | Microsoft Docs
description: "Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service toocache och leverera de statiska filerna från servrar Stäng tooyour kunder runt hälsningsmeddelande."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a>Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service

[Azure innehåll innehållsleveransnätverk (CDN)](../cdn/cdn-overview.md) cachelagrar statiska webbinnehåll på strategiskt monterade platser tooprovide maximalt dataflöde för att leverera innehåll toousers. hello CDN minskar också belastningen på servern på ditt webbprogram. Den här kursen visar hur tooadd Azure CDN tooa [webbapp i Azure App Service](app-service-web-overview.md). 

Här är hello startsidan för hello exempel statiska HTML-webbplats som du ska arbeta med:

![Exempelstartsida för app](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Vad du lära dig:

> [!div class="checklist"]
> * Skapa en CDN-slutpunkt.
> * Uppdatera cachelagrade tillgångar.
> * Använd fråga strängar toocontrol cachelagras versioner.
> * Använda en anpassad domän för hello CDN-slutpunkten.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

- [Installera Git](https://git-scm.com/)
- [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a>Skapa hello-webbprogram

toocreate hello webbprogram som du ska arbeta med, följ hello [statiska HTML-Snabbstart](app-service-web-get-started-html.md) via hello **Bläddra toohello app** steg.

### <a name="have-a-custom-domain-ready"></a>Ha ett anpassat domän redo

toocomplete hello domänen steg i den här kursen behöver tooown en anpassad domän och har åtkomst tooyour DNS-registret för domänleverantören (till exempel GoDaddy). Till exempel tooadd DNS-poster för `contoso.com` och `www.contoso.com`, måste du ha åtkomst tooconfigure hello DNS-inställningarna för hello `contoso.com` rotdomänen.

Om du inte redan har ett domännamn kan du överväga följande hello [Apptjänst domän kursen](custom-dns-web-site-buydomains-web-app.md) toopurchase en domän med hjälp av hello Azure-portalen. 

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Öppna en webbläsare och gå toohello [Azure-portalen](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Skapa en CDN-profil och en slutpunkt

I vänster navigeringsfält hello, väljer **Apptjänster**, och välj sedan hello-app som du skapade i hello [statiska HTML-Snabbstart](app-service-web-get-started-html.md).

![Välj App Service-appen i hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

I hello **Apptjänst** i hello sidan **inställningar** väljer **nätverk > Konfigurera Azure CDN för din app**.

![Välj CDN hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

I hello **Azure Content Delivery Network** anger hello **ny slutpunkt** inställningar som anges i hello tabell.

![Skapa profil och slutpunkt i hello-portalen](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Inställning | Föreslaget värde | Beskrivning |
| ------- | --------------- | ----------- |
| **CDN-profil** | myCDNProfile | Välj **Skapa nytt** toocreate CDN-profilen. En CDN-profil är en samling av CDN-slutpunkter med hello samma prisnivån. |
| **prisnivå** | Standard Akamai | Hej [prisnivån](../cdn/cdn-overview.md#azure-cdn-features) anger hello-providern och funktioner som är tillgängliga. I den här kursen använder du Standard Akamai. |
| **CDN-slutpunktsnamn** | Alla namn som är unikt i hello azureedge.net domän | Du har åtkomst till dina cachelagrade resurser på hello domän  *\<endpointname >. azureedge.net*.

Välj **Skapa**.

Azure skapar hello profil och slutpunkt. hello ny slutpunkt visas i hello **slutpunkter** listan hello samma sida och när den har etablerats hello status är **kör**.

![Ny slutpunkt i listan](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a>Testa hello CDN-slutpunkten

Om du har valt Verizon-prisnivån tar det vanligtvis cirka 90 minuter för slutpunktsspridning. Det tar några minuter för spridning för Akamai

hello sample-appen har en `index.html` fil och *css*, *img*, och *js* mappar som innehåller andra statiska tillgångar. hello innehåll sökvägar för alla dessa filer är hello samma på hello CDN-slutpunkten. Till exempel åtkomst både hello följande URL: er hello *bootstrap.css* filen i hello *css* mapp:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Gå en webbläsare toohello följande URL:

```
http://<endpointname>.azureedge.net/index.html
```

![Exempelstartsida för app som hämtats från CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 Du kan se hello samma sidan du körde tidigare i en Azure-webbapp. Azure CDN har hämtats hello ursprung webbprogram tillgångar och används av dem från hello CDN-slutpunkten

tooensure som den här sidan sparas i cacheminnet i hello CDN uppdatera hello sidan. Två begäranden för hello samma tillgång krävs ibland för hello CDN toocache hello begärda innehållet.

Mer information om hur du skapar Azure CDN-profiler och slutpunkter finns i [Komma igång med Azure CDN](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-hello-cdn"></a>Rensa hello CDN

hello CDN uppdaterar regelbundet hur dess resurser från hello ursprung webbapp baserat på hello time to live (TTL) konfiguration. hello standard TTL-värdet är sju dagar.

Ibland kanske du måste toorefresh hello CDN innan hello TTL förfallodatum – exempelvis när du distribuerar uppdaterade innehållet toohello webbprogram. tootrigger en uppdatering kan du manuellt Rensa hello CDN resurser. 

I det här avsnittet av kursen hello distribuera en ändring toohello webbapp och rensa hello CDN tootrigger hello CDN toorefresh sin cache.

### <a name="deploy-a-change-toohello-web-app"></a>Distribuera en ändring toohello webbapp

Öppna hello `index.html` och Lägg till ”-V2” toohello H1 rubriken som visas i följande exempel hello: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Genomför ändringarna och distribuerar den toohello webbprogram.

```bash
git commit -am "version 2"
git push azure master
```

När distributionen är klar finns i Bläddra toohello web app-URL och du hello ändra.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 i rubriken i webbappen](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Bläddra toohello CDN-slutpunkten URL: en för hello startsidan och du inte ser hello ändra eftersom hello cachelagrade versionen i hello CDN inte har upphört att gälla ännu. 

```
http://<endpointname>.azureedge.net/index.html
```

![Ingen V2 i rubriken i CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a>Rensa hello CDN hello-portalen

tootrigger Hej CDN tooupdate den cachelagra versionen, rensa hello CDN.

I hello portalens vänstra navigeringsfönstret, Välj **resursgrupper**, och välj sedan hello resursgrupp som du har skapat för ditt webbprogram (myResourceGroup).

![Välj resursgrupp](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Välj CDN-slutpunkten i hello lista av resurser.

![Välj slutpunkt](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Hello överst i hello **Endpoint** klickar du på **Rensa**.

![Välj Rensa](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Ange hello sökvägar till innehåll du vill toopurge. Du kan skicka en fullständig sökväg toopurge en enskild fil eller en sökväg segment toopurge och uppdatera allt innehåll i en mapp. Eftersom du har ändrat `index.html`, se till att den en hello sökvägar.

Längst ned hello hello-sidan, Välj **Rensa**.

![Rensa sida](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a>Kontrollera att hello CDN uppdateras

Vänta tills hello begäran om rensning har bearbetat vanligtvis några minuter. toosee hello aktuell status, Välj hello klockikonen överst hello på hello-sidan. 

![Rensningsavisering](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Bläddra toohello CDN slutpunkts-URL för `index.html`, och nu visas hello V2 som du lagt till toohello rubrik på hello startsida. Detta visar hello CDN cachen har uppdaterats.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 i rubriken i CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Mer information finns i [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md) (Rensa en Azure CDN-slutpunkt). 

## <a name="use-query-strings-tooversion-content"></a>Använda frågan strängar tooversion innehåll

hello Azure CDN finns följande Beteendealternativ för cachelagring hello:

* Ignorera frågesträngar
* Kringgå cachelagring för frågesträngar
* Cachelagra varje unik URL 

Hej först är hello standard, vilket innebär att endast en cachelagrad version av en tillgång oavsett hello frågesträng i hello URL: en av dessa. 

I det här avsnittet av kursen hello ändra hello cachelagring beteende toocache varje unik URL.

### <a name="change-hello-cache-behavior"></a>Ändra hello cachebeteende

I hello Azure-portalen **CDN-slutpunkten** väljer **Cache**.

Välj **cachelagra varje unik URL** från hello **cachelagring av frågesträngar** listrutan.

Välj **Spara**.

![Välj beteende för cachelagring av frågesträngar](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Kontrollera att unika URL:er cachelagras separat

Navigera toohello startsidan på hello CDN-slutpunkten i en webbläsare, men innehåller en frågesträng: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

hello CDN returnerar hello aktuella app webbinnehåll, som innehåller ”V2” i hello rubrik. 

tooensure som den här sidan sparas i cacheminnet i hello CDN uppdatera hello sidan. 

Öppna `index.html` och ändra ”V2” för ”V3”, och distribuera hello ändringen. 

```bash
git commit -am "version 3"
git push azure master
```

I en webbläsare går toohello CDN slutpunkts-URL med en ny frågesträng som `q=2`. hello CDN hämtar hello aktuella `index.html` filen och visar ”V3”.  Men om du navigerar toohello CDN-slutpunkten med hello `q=1` frågesträng, visas ”V2”.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 i rubriken i CDN, frågesträng 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 i rubriken i CDN, frågesträng 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Den här visas varje frågesträngen behandlas på olika sätt:

* q = 1 användes innan, så cachelagrat innehåll returneras (V2).
* q = 2 är nytt, så hello senaste web app innehållet hämtas och returnerade (V3).

Mer information finns i [Kontrollera cachelagringsbeteendet med frågesträngar](../cdn/cdn-query-string.md).

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a>Mappa en anpassad domän tooa CDN-slutpunkt

Du måste mappa dina anpassade domäner tooyour CDN-slutpunkten genom att skapa en CNAME-post. En CNAME-post är en DNS-funktion som mappar en domän tooa mål källdomänen. Du kan till exempel mappa `cdn.contoso.com` eller `static.contoso.com` för`contoso.azureedge.net`.

Om du inte har en anpassad domän, bör följande hello [Apptjänst domän kursen](custom-dns-web-site-buydomains-web-app.md) toopurchase en domän med hjälp av hello Azure-portalen. 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a>Hitta hello hostname toouse med hello CNAME

I hello Azure-portalen **Endpoint** Kontrollera **översikt** har valts i vänster navigering och välj sedan hello hello **+ anpassad domän** knappen hello överst på hello sidan.

![Välj Lägg till en anpassad domän](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

I hello **lägga till en anpassad domän** kan du se hello endpoint värden namnet toouse för att skapa en CNAME-post. hello värdnamn är härledd från din CDN-slutpunktens URL:  **&lt;EndpointName >. azureedge.net**. 

![Lägg till domänsida](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a>Konfigurera hello CNAME-post hos din domänregistrator

Navigera tooyour domän register-webbplats och leta upp hello-avsnittet för att skapa DNS-poster. Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.

Hitta hello avsnitt för att hantera skapa CNAME-poster. Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord CNAME, Alias eller underdomäner.

Skapa en CNAME-post som mappar dina valda underdomän (till exempel **Statiska** eller **cdn**) toohello **Endpoint värdnamn** visas tidigare i hello-portalen. 

### <a name="enter-hello-custom-domain-in-azure"></a>Ange hello anpassade domäner i Azure

Returnera toohello **lägga till en anpassad domän** sidan och ange din domän, inklusive hello underdomän hello i dialogrutan. Ange till exempel `cdn.contoso.com`.   
   
Azure verifierar att det finns hello CNAME-post för hello domännamn som du har angett. Om hello CNAME stämmer har din anpassade domän verifierats.

Det kan ta tid för hello CNAME post toopropagate tooname servrar på hello Internet. Om din domän inte har verifierats omedelbart, vänta en stund och försök igen.

### <a name="test-hello-custom-domain"></a>Testa hello anpassad domän

I en webbläsare, navigera toohello `index.html` filen med hjälp av en anpassad domän (till exempel `cdn.contoso.com/index.html`) hello tooverify hello resultatet är samma som går direkt för`<endpointname>azureedge.net/index.html`.

![Exempelstartsidan för app med URL för en anpassad domän](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Mer information finns i [anpassad domän för kartan Azure CDN innehåll tooa](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nästa steg

Vad du lärt dig:

> [!div class="checklist"]
> * Skapa en CDN-slutpunkt.
> * Uppdatera cachelagrade tillgångar.
> * Använd fråga strängar toocontrol cachelagras versioner.
> * Använda en anpassad domän för hello CDN-slutpunkten.

Lär dig hur toooptimize CDN prestanda i hello följande artiklar:

> [!div class="nextstepaction"]
> [Förbättra prestandan genom att komprimera filer i Azure CDN](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Läsa in tillgångar för en Azure CDN-slutpunkt i förväg](../cdn/cdn-preload-endpoint.md)
