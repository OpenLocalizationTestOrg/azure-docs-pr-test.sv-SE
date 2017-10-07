---
title: aaaTroubleshooting filkomprimering i Azure CDN | Microsoft Docs
description: "Felsöka problem med Azure CDN komprimering."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>Felsöka CDN-filkomprimering
Den här artikeln hjälper dig att felsöka problem med [CDN filkomprimering](cdn-improve-performance.md).

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och på **Get Support**.

## <a name="symptom"></a>Symtom
Komprimering för din slutpunkt har aktiverats men filer returneras okomprimerade.

> [!TIP]
> toocheck om filerna returneras komprimerade du behöver ett verktyg som toouse [Fiddler](http://www.telerik.com/fiddler) eller din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Kontrollera hello HTTP-svarshuvuden returneras med den cachelagrade CDN innehåll.  Om det finns en rubrik med namnet `Content-Encoding` med värdet **gzip**, **bzip2**, eller **deflate**, ditt innehåll komprimeras.
> 
> ![Innehållskodning sidhuvud](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Orsak
Det finns flera möjliga orsaker, bland annat:

* hello begärt innehåll inte är tillgänglig för komprimering.
* Komprimering är inte aktiverad för hello begärt filtyp.
* hello HTTP-begäran innehöll inte en rubrik som begär en giltig Komprimeringstypen.

## <a name="troubleshooting-steps"></a>Felsökningssteg
> [!TIP]
> Som distribuerar nya slutpunkter, tar CDN konfigurationsändringar vissa tid toopropagate via hello nätverk.  Vanligtvis tillämpas ändringar inom 90 minuter.  Om detta är hello första gången som du har konfigurerat komprimering för CDN-slutpunkt, bör du vänta 1 – 2 timmar toobe att hello komprimeringsinställningar har spridits toohello POP. 
> 
> 

### <a name="verify-hello-request"></a>Kontrollera hello begäran
Först ska vi göra en snabb förstånd kontroll på hello-begäran.  Du kan använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello begäranden.

* Kontrollera hello-begäran skickas tooyour slutpunkts-URL, `<endpointname>.azureedge.net`, och inte din ursprung.
* Kontrollera hello-begäran innehåller en **Accept-Encoding** sidhuvud och hello värde att huvudet innehåller **gzip**, **deflate**, eller **bzip2** .

> [!NOTE]
> **Azure CDN från Akamai** profiler endast stöder **gzip** kodning.
> 
> 

![CDN-huvuden för begäran](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Kontrollera inställningarna för komprimering (Standard CDN-profil)
> [!NOTE]
> Det här steget gäller endast om din CDN-profilen är en **Azure CDN Standard från Verizon** eller **Azure CDN Standard från Akamai** profil. 
> 
> 

Navigera tooyour slutpunkt i hello [Azure-portalen](https://portal.azure.com) och klicka på hello **konfigurera** knappen.

* Kontrollera komprimering har aktiverats.
* Kontrollera hello MIME-typ för hello innehåll komprimeras toobe ingår i hello lista över komprimerat format.

![CDN-inställningarna för komprimering](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Kontrollera inställningarna för komprimering (Premium CDN-profil)
> [!NOTE]
> Det här steget gäller endast om din CDN-profilen är en **Azure CDN Premium från Verizon** profil.
> 
> 

Navigera tooyour slutpunkt i hello [Azure-portalen](https://portal.azure.com) och klicka på hello **hantera** knappen.  hello kompletterande portalen öppnas.  Hovra över hello **HTTP stora** fliken och sedan hovra över hello **cacheinställningarna** utfällbar.  Klicka på **komprimering**. 

* Kontrollera komprimering har aktiverats.
* Kontrollera hello **filtyper** listan innehåller en kommaavgränsad lista (inga blanksteg) över MIME-typer.
* Kontrollera hello MIME-typ för hello innehåll komprimeras toobe ingår i hello lista över komprimerat format.

![CDN premium komprimeringsinställningar](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a>Kontrollera hello innehåll cachelagras
> [!NOTE]
> Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).
> 
> 

Kontrollera med din webbläsare utvecklingsverktyg hello huvuden tooensure hello svarsfilen cachelagras i hello region där den begärs.

* Kontrollera hello **Server** Svarsrubrik.  hello huvudet ska ha formatet hello **plattform (POP-Server-ID)**, som visas i följande exempel hello.
* Kontrollera hello **X-Cache** Svarsrubrik.  hello sidhuvud bör du läsa **NÅDDE**.  

![CDN-svarshuvuden](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a>Kontrollera hello filen uppfyller hello storlek
> [!NOTE]
> Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).
> 
> 

toobe berättigad för komprimering, en fil måste uppfylla följande storlekskraven hello:

* Större än 128 tecken.
* Mindre än 1 MB.

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a>Kontrollera hello begäran vid hello ursprungsservern för en **Via** sidhuvud
Hej **Via** HTTP-huvudet anger toohello webbserver som hello begäran skickas via en proxyserver.  Microsoft IIS-webbservrar som standard komprimera svar när hello-begäran innehåller en **Via** huvud.  toooverride problemet, utföra hello följande:

* **IIS 6**: [ange egenskaperna HcNoCompressionForProxies = ”FALSE” i egenskaperna för hello IIS-metabas](https://msdn.microsoft.com/library/ms525390.aspx)
* **IIS 7 och senare**: [anger både **noCompressionForHttp10** och **noCompressionForProxies** tooFalse i hello serverkonfiguration](http://www.iis.net/configreference/system.webserver/httpcompression)

