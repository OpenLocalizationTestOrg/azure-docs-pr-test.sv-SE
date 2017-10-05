---
title: "Förbättra prestanda genom att komprimera filerna i Azure CDN | Microsoft Docs"
description: "Lär dig att förbättra hastighet för överföring av filen och ökar belastningen prestanda genom att komprimera filerna i Azure CDN."
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
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Förbättra prestanda genom att komprimera filerna i Azure CDN
Komprimering är ett enkelt och effektivt sätt att förbättra hastighet för överföring av filen och öka belastningen prestanda genom att minska filstorleken innan den skickas från servern. Det minskar kostnader för bandbredd och ger en mer effektiv upplevelse för användarna.

Det finns två sätt att aktivera komprimering:

* Du kan aktivera komprimering på ursprungsservern, i vilket fall CDN passerar genom komprimerade filer och leverera komprimerade filer till klienter som begär dem.
* Du kan aktivera komprimering direkt på CDN edge-servrar, i vilket fall CDN komprimerar filerna och hantera dem till användare, även om de inte komprimeras i den ursprungliga servern.

> [!IMPORTANT]
> CDN-konfigurationsändringar ta lite tid att spridas via nätverket.  För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis har slutförts under en minut.  För <b>Azure CDN från Verizon</b> profiler, vanligtvis visas ändringarna gäller inom 90 minuter.  Om du har konfigurerat komprimering för CDN-slutpunkten bör du väntar på 1 – 2 timmar att komprimering inställningar har spridits till POP före felsökning
> 
> 

## <a name="enabling-compression"></a>Aktivera komprimering
> [!NOTE]
> Standard- och Premium CDN nivåerna ge samma funktioner för komprimering, men användargränssnittet skiljer sig åt.  Mer information om skillnaderna mellan Standard och Premium CDN nivåer finns [översikt över Azure CDN](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Standard-nivå
> [!NOTE]
> Det här avsnittet gäller **Azure CDN Standard från Verizon** och **Azure CDN Standard från Akamai** profiler.
> 
> 

1. Klicka på CDN-slutpunkt som du vill hantera från sidan CDN-profil.
   
    ![CDN-profilen sidan slutpunkter](./media/cdn-file-compression/cdn-endpoints.png)
   
    CDN-slutpunkten sidan öppnas.
2. Klicka på den **konfigurera** knappen.
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-config-btn.png)
   
    CDN-konfigurationssidan öppnas.
3. Aktivera **komprimering**.
   
    ![CDN-komprimeringsalternativ](./media/cdn-file-compression/cdn-compress-standard.png)
4. Använd standardtyperna eller ändra listan genom att ta bort eller lägga till filtyper.
   
   > [!TIP]
   > Även möjligt, rekommenderas inte att tillämpa komprimering komprimerat format, till exempel ZIP, MP3, MP4, JPG och så vidare.
   > 
   > 
5. När du har gjort ändringarna klickar du på den **spara** knappen.

### <a name="premium-tier"></a>Premiumnivå
> [!NOTE]
> Det här avsnittet gäller **Azure CDN Premium från Verizon** profiler.
> 
> 

1. Från sidan CDN-profil klickar du på den **hantera** knappen.
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-manage-btn.png)
   
    CDN-hanteringsportalen öppnas.
2. Hovra över den **HTTP stora** och klicka sedan hovra över den **cacheinställningarna** utfällbar.  Klicka på **komprimering**.

    ![Komprimering filval](./media/cdn-file-compression/cdn-compress-select.png)
   
    Komprimeringsalternativ visas.
   
    ![Komprimering](./media/cdn-file-compression/cdn-compress-files.png)
3. Aktivera komprimering genom att klicka på den **komprimering aktiverat** knappen.  Ange MIME-typer som du vill komprimera som en kommaavgränsad lista (inga blanksteg) i den **filtyper** textruta.
   
   > [!TIP]
   > Även möjligt, rekommenderas inte att tillämpa komprimering komprimerat format, till exempel ZIP, MP3, MP4, JPG och så vidare. 
   > 
   > 
4. När du har gjort ändringarna klickar du på den **uppdatering** knappen.

## <a name="compression-rules"></a>Regler för komprimering
Dessa tabeller beskrivs Azure CDN komprimering beteendet för varje scenario.

> [!IMPORTANT]
> För **Azure CDN från Verizon** (Standard och Premium) endast filer komprimeras.  En fil måste vara uppfyllda för komprimering:
> 
> * Är större än 128 byte.
> * Vara mindre än 1 MB.
> 
> För **Azure CDN från Akamai**, alla filer är tillgängliga för komprimering.
> 
> För alla Azure CDN-produkter, en fil måste vara en MIME-typ som har varit [konfigurerats för komprimering](#enabling-compression).
> 
> **Azure CDN från Verizon** profiler (Standard och Premium) stöd **gzip** (GNU zip) **deflate**, **bzip2**, eller **br**(Brotli) kodning. Komprimeringen görs endast i utkanten för Brotli kodning. Klientwebbläsaren måste skicka förfrågan Brotli kodning och komprimerade tillgången måste komprimerats på ursprung sida först. 
>
>**Azure CDN från Akamai** profiler stöder bara **gzip** kodning.
> 
> **Azure CDN från Akamai** slutpunkter begär alltid **gzip** kodade filer från ursprung, oavsett klientbegäran. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprimering inaktiveras eller filen är inte tillgänglig för komprimering
| Klienten begärde format (kopplingshuvud Accept-Encoding) | Cachelagrade filformat | CDN-svar till klienten | Anteckningar |
| --- | --- | --- | --- |
| Komprimerade |Komprimerade |Komprimerade | |
| Komprimerade |Okomprimerad |Okomprimerad | |
| Komprimerade |Inte cachelagras |Komprimerad eller okomprimerad |Beror på ursprung svar |
| Okomprimerad |Komprimerade |Okomprimerad | |
| Okomprimerad |Okomprimerad |Okomprimerad | |
| Okomprimerad |Inte cachelagras |Okomprimerad | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Aktiverad komprimering och filnamnet är berättigad för komprimering
| Klienten begärde format (kopplingshuvud Accept-Encoding) | Cachelagrade filformat | CDN-svar till klienten | Anteckningar |
| --- | --- | --- | --- |
| Komprimerade |Komprimerade |Komprimerade |CDN-transcodes mellan de format som stöds |
| Komprimerade |Okomprimerad |Komprimerade |CDN utför komprimering |
| Komprimerade |Inte cachelagras |Komprimerade |CDN utför komprimering om ursprung returnerar okomprimerade.  **Azure CDN från Verizon** skickar okomprimerad fil på den första begäranden och sedan komprimeras och cachelagrar filen för efterföljande förfrågningar.  Filer med `Cache-Control: no-cache` kommer aldrig att komprimera huvudet. |
| Okomprimerad |Komprimerade |Okomprimerad |CDN utför dekomprimering |
| Okomprimerad |Okomprimerad |Okomprimerad | |
| Okomprimerad |Inte cachelagras |Okomprimerad | |

## <a name="media-services-cdn-compression"></a>Media Services CDN komprimering
För Media Services CDN aktiverat strömningsslutpunkter komprimering är aktiverat som standard för följande typer av innehåll: application/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, program/f4m + xml. Du kan inte aktivera/inaktivera komprimering för nämnda typer med Azure-portalen.  

## <a name="see-also"></a>Se även
* [Felsöka CDN-filkomprimering](cdn-troubleshoot-compression.md)    

