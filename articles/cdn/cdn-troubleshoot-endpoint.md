---
title: aaaTroubleshooting Azure CDN-slutpunkter som returnerar status 404 | Microsoft Docs
description: "Felsöka 404 svarskoder med Azure CDN-slutpunkter."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 450bfbd641c869cfd88169a12c4b69819eaa7c26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Felsöka CDN-slutpunkter som returnerar 404-status
Den här artikeln hjälper dig att felsöka problem med [CDN-slutpunkter](cdn-create-new-endpoint.md) returnerar 404-fel.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.

## <a name="symptom"></a>Symtom
Du har skapat en CDN-profil och en slutpunkt, men innehållet verkar inte vara tillgängliga på hello CDN toobe.  Användare som försöker tooaccess ditt innehåll via hello CDN URL: en tar emot HTTP 404-statuskoder. 

## <a name="cause"></a>Orsak
Det finns flera möjliga orsaker, bland annat:

* hello filens ursprung är inte synliga toohello CDN
* hello-slutpunkten är felaktigt konfigurerad orsakar hello CDN toolook hello fel plats
* hello värden tar inte emot hello värdadressen från hello CDN
* hello-slutpunkten har inte varit tid toopropagate i hela hello CDN

## <a name="troubleshooting-steps"></a>Felsökningssteg
> [!IMPORTANT]
> När du har skapat en CDN-slutpunkt blir den omedelbart inte tillgängliga för användning, som det tar tid för hello registrering toopropagate via hello CDN.  För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis slutförs inom en minut.  För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.  Om du har slutfört hello stegen i det här dokumentet och du håller fortfarande på att 404 svar, Överväg att vänta några timmar toocheck igen innan du öppnar ett supportärende.
> 
> 

### <a name="check-hello-origin-file"></a>Kontrollera hello ursprungsfilen
Kontrollera först att vi ska hello hello filen vi vill cachelagrade finns på vår ursprung och är allmänt tillgänglig.  Hej snabbaste sättet toodo som tillhör en session i privat eller Incognito tooopen en webbläsare och gå direkt toohello filen.  Bara ange eller klistra in hello URL i adressfältet hello och se om som resulterar i hello-filen som du förväntar dig.  I det här exemplet ska vi toouse en fil som jag har på ett Azure Storage-konto, tillgängliga `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Som du ser över har hello test.

![Lyckades!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> När detta är hello snabbaste och enklaste sättet tooverify filen är offentligt tillgänglig, vissa nätverkskonfigurationer i din organisation kan ge hello du illusionen att den här filen är offentligt tillgänglig när det som faktiskt är bara synliga toousers ditt nätverk ( även om den finns i Azure).  Om du har en extern webbläsare som du kan testa, t.ex. en mobil enhet som inte är ansluten tooyour organisations nätverk eller en virtuell dator i Azure som skulle vara bäst.
> 
> 

### <a name="check-hello-origin-settings"></a>Kontrollera inställningarna för hello ursprung
Nu när vi har bekräftat att hello-filen är offentligt tillgänglig på Hej internet, vi ska verifiera vår origo.  I hello [Azure Portal](https://portal.azure.com), bläddra tooyour CDN-profilen och klickar på hello-slutpunkt som du felsöker.  I hello resulterande **Endpoint** bladet, klickar du på hello ursprung.  

![Slutpunkten bladet med ursprung markerat](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Hej **ursprung** bladet visas. 

![Ursprung bladet](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ av ursprung och värdnamn
Kontrollera hello **typ av ursprung** är korrekt och kontrollera hello **ursprungsvärdnamn**.  I vårt exempel `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, hello hostname-delen av hello URL: en är `cdndocdemo.blob.core.windows.net`.  Som du ser i skärmbilden hello detta är korrekt.  För Azure Storage, webbprogram och Molntjänsten ursprung hello **ursprungsvärdnamn** fältet är listrutan, så vi inte behöver tooworry om stavning den korrekt.  Om du använder en anpassad ursprung, det är dock *absolut nödvändigt* värdnamnet är rätt stavat!

#### <a name="http-and-https-ports"></a>HTTP och HTTPS-portar
hello andra sak toocheck här är din **HTTP** och **HTTPS-portar**.  I de flesta fall 80 och 443 är korrekta och du behöver inga ändringar.  Men om hello ursprungsservern lyssnar på en annan port, som behöver toobe som visas här.  Om du inte är säker på att bara titta på hello URL för din ursprungsfilen.  Ange portarna 80 och 443 som hello standardvärden hello HTTP och HTTPS-specifikationer. I min URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, en port har angetts, så antas hello standardvärdet 443 och inställningarna är korrekta.  

Dock säger hello URL för din ursprung-fil som du tidigare har testat är `http://www.contoso.com:8080/file.txt`.  Obs hello `:8080` hello slutet av hello hostname segment.  Som talar om hello webbläsare toouse port `8080` tooconnect toohello webbserver på `www.contoso.com`, så du behöver tooenter 8080 i hello **HTTP-porten** fältet.  Det är viktigt toonote att portinställningarna påverkar bara vilken port hello slutpunkten använder tooretrieve information från hello ursprung.

> [!NOTE]
> **Azure CDN från Akamai** slutpunkter inte tillåter hello fullständig TCP-portintervallet för ursprung.  En lista över ursprungsportar som inte tillåts finns i [Azure CDN från Akamai-tillåtna ursprungsportar](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-hello-endpoint-settings"></a>Kontrollera inställningarna för hello-slutpunkt
Tillbaka på hello **Endpoint** bladet, klickar du på hello **konfigurera** knappen.

![Slutpunkten bladet med Konfigurera knappen markerad](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Hej slutpunktens **konfigurera** bladet visas.

![Konfigurera bladet](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoll
För **protokoll**, kontrollera att hello-protokollet som används av hello klienter har valts.  hello blir samma protokoll som används av hello klient hello ett används tooaccess hello ursprung, så det är viktigt toohave hello ursprung portar som konfigurerats på rätt sätt hello föregående avsnitt.  hello slutpunkten lyssnar bara på hello standard HTTP och HTTPS-portar (80 och 443), oavsett hello ursprung portar.

Vi ska returnera tooour hypotetiska exempel med `http://www.contoso.com:8080/file.txt`.  Som du kommer ihåg Contoso angetts `8080` sina HTTP-port, men vi förutsätter också de angivna `44300` som deras HTTPS-port.  Om de har skapat en slutpunkt med namnet `contoso`, deras CDN-slutpunktens värdnamn skulle vara `contoso.azureedge.net`.  En begäran om `http://contoso.azureedge.net/file.txt` är en HTTP-begäran så hello slutpunkten använder HTTP på port 8080 tooretrieve från hello ursprung.  En säker begäran via HTTPS, `https://contoso.azureedge.net/file.txt`, skulle orsaka hello endpoint toouse HTTPS på port 44300 när hämta hello filen från hello ursprung.

#### <a name="origin-host-header"></a>Ursprungsvärdadress
Hej **ursprungsvärdadress** är hello skickas toohello ursprunget med varje begäran.  I de flesta fall är detta bör vara hello samma som hello **ursprungsvärdnamn** vi verifierat tidigare.  Ett felaktigt värde i det här fältet normalt orsakar 404 status, men är sannolikt toocause andra 4xx statusar, beroende på vilka hello ursprung förväntas.

#### <a name="origin-path"></a>Sökväg
Till sist ska vi ska verifiera vår **ursprungssökväg**.  Detta är tomt som standard.  Du bör endast använda det här fältet om du vill toonarrow hello scope hello ursprung finns resurser som du vill toomake som är tillgängliga på hello CDN.  

Till exempel i min slutpunkt önskade alla resurser på toobe tillgängliga, min storage-konto så att jag lämnas **ursprungssökväg** tomt.  Detta innebär att en begäran för`https://cdndocdemo.azureedge.net/publicblob/lorem.txt` resulterar i en anslutning från min slutpunkten för`cdndocdemo.core.windows.net` som begär `/publicblob/lorem.txt`.  På samma sätt kan en begäran om `https://cdndocdemo.azureedge.net/donotcache/status.png` resulterar i hello endpoint begär `/donotcache/status.png` från hello ursprunget.

Men vad händer om jag vill inte toouse hello CDN för varje sökväg på min ursprung?  Säg I bara tänkte tooexpose hello `publicblob` sökväg.  Om jag ange */publicblob* i min **ursprungssökväg** fält som gör att hello endpoint tooinsert */publicblob* innan alla begäranden som gjorts toohello ursprung.  Det innebär att hello begäran om `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nu faktiskt tar hello begäran delen av hello URL `/publicblob/lorem.txt`, och Lägg till `/publicblob` toohello början. Detta resulterar i en begäran om `/publicblob/publicblob/lorem.txt` från hello ursprunget.  Om sökvägen inte går att lösa tooan själva filen, returnerar hello ursprung status 404.  hello rätt URL tooretrieve lorem.txt i det här exemplet skulle kunna vara `https://cdndocdemo.azureedge.net/lorem.txt`.  Observera att vi inte inkluderar hello */publicblob* sökvägen, eftersom hello begäran delen av hello URL är `/lorem.txt` och lägger till hello endpoint `/publicblob`, vilket resulterar i `/publicblob/lorem.txt` som hello-begäran skickas toohello ursprung .

