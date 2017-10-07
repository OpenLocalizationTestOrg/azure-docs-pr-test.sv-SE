---
title: "aaaHTTP/2-stöd i Azure CDN | Microsoft Docs"
description: "Läs mer om stöd för HTTP-2 och CDN."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a>HTTP-2-stöd i Azure CDN

HTTP-2 är en större ändring tooHTTP/1.1\. Det ger snabbare webbprestanda, minskade svarstid och ger en förbättrad upplevelse samtidigt hello bekant HTTP-metoderna och statuskoder semantik. Även om HTTP-2 är utformad toowork med HTTP och HTTPS, stöder endast HTTP/2 via TLS många klientens webbläsare.

###<a name="http2-benefits"></a>Fördelar med HTTP-2

hello fördelar HTTP/2:

*   **Multiplexering och samtidighet**

    Med HTTP 1.1 flera gör flera förfrågningar för resursen kräver flera TCP-anslutningar och varje anslutning har prestanda försämras som är kopplade till den. HTTP-2 kan flera resurser toobe efterfrågas på en enda TCP-anslutning.

*   **Komprimering av huvud**

    Genom att komprimera hello HTTP-huvuden för hanteras resurser minskas tid hello överföring avsevärt.

*   **Stream-beroenden**

    Dataströmmen beroenden kan hello klienten tooindicate toohello server där resurser som har prioritet.


##<a name="http2-browser-support"></a>Stöd för HTTP-2-webbläsare

Alla större webbläsare för hello har implementerat HTTP/2-stöd i aktuell version. Webbläsare som inte stöds kommer automatiskt återställningsplats tooHTTP/1.1.

|Webbläsare|Lägsta Version|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

##<a name="enabling-http2-support-in-azure-cdn"></a>Aktivera HTTP-2-stöd i Azure CDN

För närvarande stöd för HTTP-2 är aktiv för **Azure CDN från Akamai** och **Azure CDN från Verizon** profiler. Ingen ytterligare åtgärd krävs från kunder.

##<a name="next-steps"></a>Nästa steg

toosee hello fördelarna med HTTP/2 i åtgärden, se [i den här demon från Akamai](https://http2.akamai.com/demo).

toolearn mer information om HTTP-2 finns hello följande resurser:

*   [HTTP-2-specifikationen webbsida](https://http2.github.io/)
*   [Officiell HTTP/2 vanliga frågor och svar](https://http2.github.io/faq/)
*   [Akamai HTTP/2-information](https://http2.akamai.com/)

toolearn mer om Azure CDN tillgängliga funktioner, se hello [översikt över Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).
