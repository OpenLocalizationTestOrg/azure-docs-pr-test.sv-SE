---
title: "aaaGetting igång med Azure CDN | Microsoft Docs"
description: "Det här avsnittet visar hur tooenable hello Azure Content Delivery Network (CDN). hello kursen går igenom hello skapandet av en ny CDN-profil och en slutpunkt."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a>Komma igång med Azure CDN
Det här avsnittet beskriver steg för steg hur du aktiverar Azure CDN genom att skapa en ny profil och slutpunkt för CDN.

> [!IMPORTANT]
> En introduktion toohow CDN fungerar, samt en lista över funktioner finns hello [CDN-översikt](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Skapa en ny CDN-profil
En CDN-profil är en samling CDN-slutpunkter.  Varje profil innehåller en eller flera CDN-slutpunkter.  Du kanske vill toouse flera profiler tooorganize CDN-slutpunkter med internet-domän, webbprogram eller andra kriterier.

> [!NOTE]
> En enda Azure-prenumeration är som standard begränsad tooeight CDN profiler. Varje CDN-profilen är begränsad tooten CDN-slutpunkter.
> 
> CDN priser tillämpas på hello CDN-profilen nivå. Om du vill toouse en blandning av Azure CDN prisnivåer behöver flera CDN-profiler.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Skapa en ny CDN-slutpunkt
**toocreate en ny CDN-slutpunkt**

1. I hello [Azure Portal](https://portal.azure.com), navigera tooyour CDN-profilen.  Du kanske har Fäst det toohello instrumentpanelen i hello föregående steg.  Om du inte hittar du den genom att klicka på **Bläddra**, sedan **CDN profiler**, och klicka på hello profilen du planerar tooadd till slutpunkten.
   
    hello CDN-profilbladet visas.
   
    ![CDN-profil][cdn-profile-settings]
2. Klicka på hello **Lägg till slutpunkt** knappen.
   
    ![Knappen Lägg till slutpunkt][cdn-new-endpoint-button]
   
    Hej **lägga till en slutpunkt** bladet visas.
   
    ![Bladet Lägg till slutpunkt][cdn-add-endpoint]
3. Ange ett **namn** för den här CDN-slutpunkten.  Det här namnet kommer att använda tooaccess cachelagrade resurserna på hello domän `<endpointname>.azureedge.net`.
4. I hello **typ av ursprung** listrutan väljer du typen av ursprung.  Välj **Storage** för ett Azure Storage-konto, **Cloud Services** för ett Azure Cloud Services-konto, **Web Apps** för ett Azure Web Apps-konto eller **Anpassat ursprung** för ett annat ursprung i form av en offentligt tillgänglig webbserver (som finns i Azure eller någon annanstans).
   
    ![Ursprungstyp för CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. I hello **ursprungsvärdnamn** listrutan, Välj eller Skriv din ursprungliga domän.  hello listrutan visar en lista över alla tillgängliga ursprung av hello-typ som du angav i steg 4.  Om du har valt *anpassad ursprung* som din **typ av ursprung**, du kommer att ange hello ursprungsdomän din anpassade.
6. I hello **ursprungssökväg** text Ange hello sökvägen toohello resurser du vill att toocache eller lämna tomt tooallow cache någon resurs på hello domän du angav i steg 5.
7. I hello **ursprungsvärdadress**anger hello värdadressen önskade hello CDN toosend med varje begäran, eller lämna hello standard.
   
   > [!WARNING]
   > Vissa typer av ursprung, till exempel Azure Storage och Web Apps kräver hello värden huvud toomatch hello ursprungsdomän hello. Om du inte har ett ursprung som kräver ett värdhuvud som skiljer sig från sin domän, bör du lämna hello standardvärdet.
   > 
   > 
8. För **protokollet** och **ursprung port**anger hello protokoll och portar används tooaccess dina resurser vid hello ursprung.  Du måste välja minst ett protokoll (HTTP eller HTTPS).
   
   > [!NOTE]
   > Hej **ursprung port** påverkar endast vilken port hello slutpunkten använder tooretrieve information från hello ursprung.  hello endpoint själva kommer bara att tillgängliga tooend klienter på hello standard HTTP och HTTPS-portar (80 och 443), oavsett hello **ursprung port**.  
   > 
   > **Azure CDN från Akamai** slutpunkter inte tillåter hello fullständig TCP-portintervallet för ursprung.  En lista över ursprungsportar som inte tillåts finns i [Azure CDN från Akamai-tillåtna ursprungsportar](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > Åtkomst till CDN har innehåll med hjälp av HTTPS hello följande begränsningar:
   > 
   > * Du måste använda hello SSL-certifikat som tillhandahålls av hello CDN. Certifikat från tredje part stöds inte.
   > * Du måste använda hello angivna CDN domän (`<endpointname>.azureedge.net`) tooaccess HTTPS-innehåll. HTTPS-stöd är inte tillgängligt för anpassade domännamn (skapa CNAME-poster) eftersom hello CDN inte stöder anpassade certifikat just nu.
   > 
   > 
9. Klicka på hello **Lägg till** knappen toocreate hello ny slutpunkt.
10. När hello slutpunkten har skapats visas den i en lista över slutpunkter för hello-profilen. hello listvyn visar hello URL toouse tooaccess cachelagrat innehåll, samt hello ursprungsdomän.
    
    ![CDN-slutpunkt][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > hello endpoint blir omedelbart inte tillgängliga för användning, som det tar tid för hello registrering toopropagate via hello CDN.  För profiler av typen <b>Azure CDN från Akamai</b> slutförs spridningen vanligtvis inom en minut.  För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.
    > 
    > Användare som försöker toouse hello CDN domännamn innan hello slutpunktskonfiguration har spridits toohello POP får HTTP 404 svarskoder.  Om det har gått flera timmar sedan du skapade slutpunkten och du fortfarande får 404-svar läser du [Felsöka CDN-slutpunkter som returnerar 404-status](cdn-troubleshoot-endpoint.md).
    > 
    > 

## <a name="see-also"></a>Se även
* [Kontrollera cachelagringsbeteendet för begäranden med frågesträngar](cdn-query-string.md)
* [Hur tooMap innehåll till CDN tooa anpassad domän](cdn-map-content-to-custom-domain.md)
* [Läsa in tillgångar för en Azure CDN-slutpunkt i förväg](cdn-preload-endpoint.md)
* [Rensa en Azure CDN-slutpunkt](cdn-purge-endpoint.md)
* [Felsöka CDN-slutpunkter som returnerar 404-status](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
