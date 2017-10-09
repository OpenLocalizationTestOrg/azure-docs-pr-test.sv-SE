---
title: aaaImprove prestanda genom att komprimera filerna i Azure CDN | Microsoft Docs
description: "Lär dig hur tooimprove filöverföring hastighet och ökar belastningen prestanda genom att komprimera filerna i Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Förbättra prestanda genom att komprimera filerna i Azure CDN
Komprimering är ett enkelt och effektivt metoden tooimprove filen överföringshastighet och öka belastningen prestanda genom att minska filstorleken innan den skickas från hello-servern. Det minskar kostnader för bandbredd och ger en mer effektiv upplevelse för användarna.

Det finns två sätt tooenable komprimering:

* Du kan aktivera komprimering på ursprungsservern, i vilket fall hello CDN passerar genom hello komprimerade filer och leverera komprimerade filer tooclients som begär dem..
* Du kan aktivera komprimering direkt på CDN edge servrar, där case hello CDN komprimerar hello filer och hantera dem tooend användare, även om de inte komprimeras i hello ursprungsservern.

> [!IMPORTANT]
> CDN konfigurationsändringar ta viss tid toopropagate via hello nätverk.  För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis har slutförts under en minut.  För <b>Azure CDN från Verizon</b> profiler, vanligtvis visas ändringarna gäller inom 90 minuter.  Om detta är hello första gången som du har konfigurerat komprimering för CDN-slutpunkt, bör du vänta 1 – 2 timmar toobe att hello komprimeringsinställningar har spridits toohello POP före felsökning
> 
> 

## <a name="enabling-compression"></a>Aktivera komprimering
> [!NOTE]
> hello Standard och Premium CDN nivåer ange hello samma komprimering funktioner, men hello användargränssnittet skiljer sig åt.  Läs mer om hello skillnader mellan Standard och Premium CDN nivåer [översikt över Azure CDN](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Standard-nivå
> [!NOTE]
> Det här avsnittet gäller för**Azure CDN Standard från Verizon** och **Azure CDN Standard från Akamai** profiler.
> 
> 

1. Klicka på hello CDN-slutpunkt som du vill toomanage från hello CDN-profil.
   
    ![CDN-profilen sidan slutpunkter](./media/cdn-file-compression/cdn-endpoints.png)
   
    hello CDN-slutpunkten sidan öppnas.
2. Klicka på hello **konfigurera** knappen.
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-config-btn.png)
   
    hello CDN konfigurationssidan öppnas.
3. Aktivera **komprimering**.
   
    ![CDN-komprimeringsalternativ](./media/cdn-file-compression/cdn-compress-standard.png)
4. Använd standardtyperna hello eller ändra hello listan genom att ta bort eller lägga till filtyper.
   
   > [!TIP]
   > Även möjligt, inte rekommenderas tooapply komprimering toocompressed format, till exempel ZIP, MP3, MP4, JPG och så vidare.
   > 
   > 
5. När du har gjort ändringarna klickar du på hello **spara** knappen.

### <a name="premium-tier"></a>Premiumnivå
> [!NOTE]
> Det här avsnittet gäller för**Azure CDN Premium från Verizon** profiler.
> 
> 

1. Klicka på hello från hello CDN-profilen **hantera** knappen.
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Hovra över hello **HTTP stora** fliken och sedan hovra över hello **cacheinställningarna** utfällbar.  Klicka på **komprimering**.

    ![Komprimering filval](./media/cdn-file-compression/cdn-compress-select.png)
   
    Komprimeringsalternativ visas.
   
    ![Komprimering](./media/cdn-file-compression/cdn-compress-files.png)
3. Aktivera komprimering genom att klicka på hello **komprimering aktiverat** knappen.  Ange hello MIME-typer gärna toocompress som en kommaavgränsad lista (inga blanksteg) i hello **filtyper** textruta.
   
   > [!TIP]
   > Även möjligt, inte rekommenderas tooapply komprimering toocompressed format, till exempel ZIP, MP3, MP4, JPG och så vidare. 
   > 
   > 
4. När du har gjort ändringarna klickar du på hello **uppdatering** knappen.

## <a name="compression-rules"></a>Regler för komprimering
Dessa tabeller beskrivs Azure CDN komprimering beteendet för varje scenario.

> [!IMPORTANT]
> För **Azure CDN från Verizon** (Standard och Premium) endast filer komprimeras.  toobe berättigad för komprimering, en fil måste:
> 
> * Är större än 128 byte.
> * Vara mindre än 1 MB.
> 
> För **Azure CDN från Akamai**, alla filer är tillgängliga för komprimering.
> 
> För alla Azure CDN-produkter, en fil måste vara en MIME-typ som har varit [konfigurerats för komprimering](#enabling-compression).
> 
> **Azure CDN från Verizon** profiler (Standard och Premium) stöd **gzip** (GNU zip) **deflate**, **bzip2**, eller **br**(Brotli) kodning. För Brotli kodning görs hello komprimering endast vid hello kant. hello klientwebbläsaren skicka begäran om hello Brotli kodning och hello komprimerade tillgångsinformation måste komprimerats på hello ursprung sida först. 
>
>**Azure CDN från Akamai** profiler stöder bara **gzip** kodning.
> 
> **Azure CDN från Akamai** slutpunkter begär alltid **gzip** kodade filer från hello ursprung, oavsett hello klientbegäran. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprimering inaktiveras eller filen är inte tillgänglig för komprimering
| Klienten begärde format (kopplingshuvud Accept-Encoding) | Cachelagrade filformat | CDN-svar toohello klienten | Anteckningar |
| --- | --- | --- | --- |
| Komprimerade |Komprimerade |Komprimerade | |
| Komprimerade |Okomprimerad |Okomprimerad | |
| Komprimerade |Inte cachelagras |Komprimerad eller okomprimerad |Beror på ursprung svar |
| Okomprimerad |Komprimerade |Okomprimerad | |
| Okomprimerad |Okomprimerad |Okomprimerad | |
| Okomprimerad |Inte cachelagras |Okomprimerad | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Aktiverad komprimering och filnamnet är berättigad för komprimering
| Klienten begärde format (kopplingshuvud Accept-Encoding) | Cachelagrade filformat | CDN-svar toohello klienten | Anteckningar |
| --- | --- | --- | --- |
| Komprimerade |Komprimerade |Komprimerade |CDN-transcodes mellan de format som stöds |
| Komprimerade |Okomprimerad |Komprimerade |CDN utför komprimering |
| Komprimerade |Inte cachelagras |Komprimerade |CDN utför komprimering om ursprung returnerar okomprimerade.  **Azure CDN från Verizon** överför hello okomprimerad fil på hello första begäran och sedan komprimerar och cacheminnen hello-filen för efterföljande förfrågningar.  Filer med `Cache-Control: no-cache` kommer aldrig att komprimera huvudet. |
| Okomprimerad |Komprimerade |Okomprimerad |CDN utför dekomprimering |
| Okomprimerad |Okomprimerad |Okomprimerad | |
| Okomprimerad |Inte cachelagras |Okomprimerad | |

## <a name="media-services-cdn-compression"></a>Media Services CDN komprimering
För Media Services CDN aktiverat strömningsslutpunkter komprimering är aktiverat som standard för hello följande typer av innehåll: application/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, program/f4m + xml. Du kan inte aktivera/inaktivera komprimering för hello anges med hjälp av hello Azure-portalen.  

## <a name="see-also"></a>Se även
* [Felsöka CDN-filkomprimering](cdn-troubleshoot-compression.md)    

