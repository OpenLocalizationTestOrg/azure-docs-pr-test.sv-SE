---
title: "aaaRedirect översikt för Azure Programgateway | Microsoft Docs"
description: "Lär dig mer om hello omdirigerings-funktionen i Azure Programgateway"
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: 7149e3bd000d336c51995fb0e90f971b4d9ba2be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-redirect-overview"></a>Översikt över omdirigering Gateway

Ett vanligt scenario för många webbprogram är toosupport automatisk HTTP tooHTTPS omdirigering tooensure all kommunikation mellan program och dess användare sker via en krypterad sökväg. I tidigare hello använde kunder tekniker som skapar en dedikerad backend-adresspool vars syfte är tooredirect begäranden tas emot på http-tooHTTPS.  Programgateway stöder nu möjligheten tooredirect trafik på hello Application Gateway. Detta förenklar programkonfigurationen, optimerar hello Resursanvändning och har stöd för den nya omdirigering scenarier inklusive globala och sökväg-baserade omdirigering. Stöd för program Gateway omdirigering begränsas inte tooHTTP -> enbart HTTPS-omdirigering. Detta är en generisk omdirigering-mekanism som gör det möjligt för omdirigering av trafik som tas emot på en lyssnare tooanother lyssnare på en Programgateway. Det stöder också omdirigering tooan extern webbplats samt. Programstöd för Gateway-omdirigering innehåller hello följande funktioner:

1. Globala omdirigering från en lyssnare tooanother lyssnare på hello Gateway. Detta gör att HTTP-omdirigering på tooHTTPS på en plats.
2. Sökväg-baserade omdirigering. Den här typen av omdirigering aktiverar HTTP-omdirigering tooHTTPS endast på en viss plats, till exempel ett shopping kundvagn område betecknas med/kundvagn / *.
3. Omdirigera tooexternal plats.

![omdirigering](./media/application-gateway-redirect-overview/redirect.png)

Med den här ändringen kunder behöver toocreate ett nytt omdirigering konfigurationsobjekt som anger hello mål lyssnare eller extern webbplats toowhich omdirigering önskas. hello konfigurationselementet har också stöd för alternativ tooenable bifoga hello URI-sökväg och fråga sträng toohello omdirigeras URL. Kunder kan också välja om omdirigering är en tillfällig (HTTP-statuskod 302) eller en permanent omdirigering (HTTP-statuskod 301). När du skapat den här konfigurationen för omdirigering är anslutna toohello källa lyssnaren via en ny regel. När du använder en enkel regel hello omdirigerings-konfigurationen är associerad med en käll-lyssnare och är en global omdirigering. När en sökväg-baserade regel används hello omdirigerings-konfigurationen har definierats på hello URL-sökväg karta och gäller därför bara toohello angiven sökväg område för en plats.

### <a name="next-steps"></a>Nästa steg

[Konfigurera URL: en omdirigering på en Programgateway](application-gateway-configure-redirect-powershell.md)
