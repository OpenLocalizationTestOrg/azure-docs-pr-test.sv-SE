---
title: "Felsöka Azure CDN-slutpunkter som returnerar status 404 | Microsoft Docs"
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
ms.openlocfilehash: f59fbd18413fb44026d8c92b7f6940ed2f8a00a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Felsöka slutpunkter för innehållsleveransnätverk som returnerar status 404
Den här artikeln hjälper dig att felsöka problem med [CDN-slutpunkter](cdn-create-new-endpoint.md) returnerar 404-fel.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.

## <a name="symptom"></a>Symtom
Du har skapat en CDN-profil och en slutpunkt, men innehållet verkar inte vara tillgängliga på CDN.  Användare som försöker få åtkomst till ditt innehåll via CDN-URL: en får HTTP 404-statuskoder. 

## <a name="cause"></a>Orsak
Det finns flera möjliga orsaker, bland annat:

* Filens ursprung visas inte för CDN
* Slutpunkten är felaktigt konfigurerad orsakar CDN ska sökas i fel plats
* Värden tar inte emot värdadressen från CDN
* Slutpunkten har inte varit sprids i hela CDN

## <a name="troubleshooting-steps"></a>Felsökningssteg
> [!IMPORTANT]
> När du har skapat en CDN-slutpunkt blir den omedelbart inte tillgängliga för användning, som det tar tid för registreringen ska spridas via CDN.  För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis slutförs inom en minut.  För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.  Om du har slutfört stegen i det här dokumentet och du håller fortfarande på att 404 svar, Överväg att vänta några timmar för att kontrollera igen innan du öppnar ett supportärende.
> 
> 

### <a name="check-the-origin-file"></a>Kontrollera till originalfilen
Vi ska kontrollera först att den fil som vi vill cachelagrade finns på vår ursprung och är allmänt tillgänglig.  Det snabbaste sättet att göra det är att öppna en webbläsare i ett privat In eller Incognito-session och gå direkt till filen.  Bara ange eller klistra in Webbadressen i adressfältet och se om som resulterar i filen som du förväntar dig.  I det här exemplet ska vi använda en fil som jag har på ett Azure Storage-konto, tillgängliga `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Som du ser över har testet.

![Lyckades!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Detta är det snabbaste och enklaste sättet att verifiera din fil är offentligt tillgänglig, kan vissa nätverkskonfigurationer i din organisation ge du illusionen att filen inte är tillgängliga när det är faktum är bara synliga för användare i nätverket (även om Det finns i Azure).  Om du har en extern webbläsare som du kan testa, till exempel en mobil enhet som inte är ansluten till din organisations nätverk eller en virtuell dator i Azure, som skulle vara bäst.
> 
> 

### <a name="check-the-origin-settings"></a>Kontrollera inställningarna för ursprung
Nu när vi har bekräftat att filen är offentligt tillgänglig på internet, måste vi verifiera inställningarna för våra ursprung.  I den [Azure Portal](https://portal.azure.com), bläddra till CDN-profilen och klicka på den slutpunkt som du felsöker.  I den resulterande **Endpoint** bladet, klickar du på ursprung.  

![Slutpunkten bladet med ursprung markerat](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Den **ursprung** bladet visas. 

![Ursprung bladet](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ av ursprung och värdnamn
Kontrollera den **typ av ursprung** är korrekt och kontrollera den **ursprungsvärdnamn**.  I vårt exempel `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, hostname-delen av URL: en är `cdndocdemo.blob.core.windows.net`.  Detta är rätt som du ser i skärmbilden.  För Azure Storage, webbprogram och Molntjänsten ursprung i **ursprungsvärdnamn** fältet är listrutan, så vi inte behöver bry dig om stavning den korrekt.  Om du använder en anpassad ursprung, det är dock *absolut nödvändigt* värdnamnet är rätt stavat!

#### <a name="http-and-https-ports"></a>HTTP och HTTPS-portar
Klassificeringarna att kontrollera det här är din **HTTP** och **HTTPS-portar**.  I de flesta fall 80 och 443 är korrekta och du behöver inga ändringar.  Om den ursprungliga servern lyssnar på en annan port, som dock behöver representeras här.  Om du inte är säker på att bara titta på URL: en för din ursprungsfilen.  Specifikationer för HTTP och HTTPS ange portarna 80 och 443 som standard. I min URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, en port har angetts, så antas standardvärdet 443 och inställningarna är korrekta.  

Säg men URL-Adressen för din ursprung-fil som du tidigare har testat är `http://www.contoso.com:8080/file.txt`.  Observera den `:8080` i slutet av hostname-segment.  Som instruerar webbläsaren att använda port `8080` att ansluta till webbservern i `www.contoso.com`, så du behöver ange 8080 i den **HTTP-porten** fältet.  Det är viktigt att Observera att dessa portinställningar endast påverkar vilken port som slutpunkten används för att hämta information från ursprunget.

> [!NOTE]
> Slutpunkter av typen **Azure CDN från Akamai** tillåter inte det fullständiga TCP-portintervallet för ursprung.  En lista över ursprungsportar som inte tillåts finns i [Azure CDN från Akamai-tillåtna ursprungsportar](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Kontrollera inställningarna för slutpunkten
Tillbaka på den **Endpoint** bladet, klickar du på den **konfigurera** knappen.

![Slutpunkten bladet med Konfigurera knappen markerad](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Slutpunktens **konfigurera** bladet visas.

![Konfigurera bladet](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoll
För **protokoll**, kontrollera att det protokoll som används av klienterna har valts.  Samma protokoll som används av klienten kommer att den som används för att komma åt ursprung, så det är viktigt att ha ursprung portar som konfigurerats på rätt sätt i föregående avsnitt.  Slutpunkten lyssnar bara på HTTP och HTTPS standardportarna (80 och 443), oavsett ursprung-portar.

Nu ska vi gå tillbaka till vår hypotetiska exempel med `http://www.contoso.com:8080/file.txt`.  Som du kommer ihåg Contoso angetts `8080` sina HTTP-port, men vi förutsätter också de angivna `44300` som deras HTTPS-port.  Om de har skapat en slutpunkt med namnet `contoso`, deras CDN-slutpunktens värdnamn skulle vara `contoso.azureedge.net`.  En begäran om `http://contoso.azureedge.net/file.txt` är en HTTP-begäran så slutpunkten använder HTTP på port 8080 vill hämta från ursprunget.  En säker begäran via HTTPS, `https://contoso.azureedge.net/file.txt`, skulle orsaka slutpunkten att använda HTTPS på port 44300 när hämta filen från ursprunget.

#### <a name="origin-host-header"></a>Ursprungsvärdadress
Den **ursprungsvärdadress** är värden huvudets värde skickas till ursprunget med varje begäran.  I de flesta fall bör detta vara samma som den **ursprungsvärdnamn** vi verifierat tidigare.  Ett felaktigt värde i det här fältet normalt orsakar 404 status, men kan medföra andra 4xx statusar, beroende på vad som förväntas ursprung.

#### <a name="origin-path"></a>Sökväg
Till sist ska vi ska verifiera vår **ursprungssökväg**.  Detta är tomt som standard.  Du bör bara använda det här fältet om du vill begränsa omfånget för de ursprung finns resurser som du vill ska vara tillgängliga på CDN.  

Till exempel i min slutpunkt önskade alla resurser för storage-konto ska vara tillgängligt, så jag lämnas **ursprungssökväg** tomt.  Detta innebär att en begäran om att `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` resulterar i en anslutning från min slutpunkten till `cdndocdemo.core.windows.net` som begär `/publicblob/lorem.txt`.  På samma sätt kan en begäran om `https://cdndocdemo.azureedge.net/donotcache/status.png` resulterar i slutpunkten begär `/donotcache/status.png` från ursprunget.

Men vad händer om jag vill inte använda CDN för varje sökväg på min ursprung?  Säg I vill exponera den `publicblob` sökväg.  Om jag ange */publicblob* i min **ursprungssökväg** fält som gör att infoga slutpunkten */publicblob* innan alla begäranden som görs till ursprunget.  Detta innebär att begäran om `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nu faktiskt tar den begärt som del av URL-Adressen `/publicblob/lorem.txt`, och Lägg till `/publicblob` till början. Detta resulterar i en begäran om `/publicblob/publicblob/lorem.txt` från ursprunget.  Om sökvägen inte matcha en verklig fil, returnerar ursprung status 404.  Rätt URL för att hämta lorem.txt i det här exemplet skulle kunna vara `https://cdndocdemo.azureedge.net/lorem.txt`.  Observera att vi inte innehåller den */publicblob* sökväg, eftersom begäran-delen i Webbadressen är `/lorem.txt` och lägger till slutpunkten `/publicblob`, vilket resulterar i `/publicblob/lorem.txt` begäran skickas till ursprunget.

