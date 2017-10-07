---
title: "aaaControl Azure CDN cachelagringsbeteendet med frågesträngar | Microsoft Docs"
description: "Azure CDN cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Kontrollen Azure CDN cachelagring av frågesträngar med frågesträngar
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium från Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Översikt
Cachelagring av frågesträng styr hur filer är toobe cachelagras när de innehåller frågesträngar.

> [!IMPORTANT]
> hello Standard och Premium CDN produkter har hello samma fråga sträng cachelagring funktioner, men hello användargränssnittet skiljer sig åt.  Det här dokumentet beskriver hello gränssnitt för **Azure CDN Standard från Akamai** och **Azure CDN Standard från Verizon**.  För att fråga sträng cachelagring med **Azure CDN Premium från Verizon**, se [styr cachelagring beteendet för CDN-begäranden med frågesträngar - Premium](cdn-query-string-premium.md).
> 
> 

Det finns tre lägen:

* **Ignorera frågesträngar**: det här är standardläget för hello.  hello CDN kantnod skickar hello frågesträng från hello begärande toohello ursprung på hello första begäran och cache hello tillgången.  Alla efterföljande förfrågningar för den tillgången som hämtas från hello kantnod ignoreras hello frågesträngen tills hello cachelagrade tillgången upphör att gälla.
* **Kringgå cachelagring för URL med frågesträngar**: I det här läget begäranden med frågesträngar inte cachelagras på hello CDN kantnod.  hello kantnod hämtar hello tillgången direkt från hello ursprung och skickar den begärande toohello med varje begäran.
* **Cachelagra varje unik URL**: det här läget behandlar varje begäran med en frågesträng som en unik tillgång med sin egen cache.  Till exempel hello svar från hello ursprung för en begäran om *foo.ashx?q=bar* skulle cachelagras på hello kantnod och returneras för efterföljande med samma fråga strängen.  En begäran om *foo.ashx?q=somethingelse* skulle cachelagras som en separat tillgång med sin egen toolive tid.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Ändra inställningar för standard CDN-profiler för cachelagring av frågesträng
1. Klicka på hello CDN-slutpunkt som du vill toomanage hello CDN-profilbladet.
   
    ![CDN-profilen bladet-slutpunkter](./media/cdn-query-string/cdn-endpoints.png)
   
    hello CDN-slutpunkten blad öppnas.
2. Klicka på hello **konfigurera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-query-string/cdn-config-btn.png)
   
    hello CDN Configuration blad öppnas.
3. Välj en inställning i hello **cachelagring av frågesträngar** listrutan.
   
    ![CDN-frågesträng cachelagringsalternativ](./media/cdn-query-string/cdn-query-string.png)
4. När du har gjort ditt val klickar du på hello **spara** knappen.

> [!IMPORTANT]
> hello ändringarna kan inte visas omedelbart, som det tar tid för hello registrering toopropagate via hello CDN.  För profiler av typen <b>Azure CDN från Akamai</b> slutförs spridningen vanligtvis inom en minut.  För profiler av typen <b>Azure CDN från Verizon</b> slutförs spridningen vanligtvis inom 90 minuter, men i vissa fall kan det ta längre tid.
> 
> 

