---
title: "aaaPre belastningen tillgångar på en Azure CDN-slutpunkt | Microsoft Docs"
description: "Lär dig hur toopre belastningen cachelagrat innehåll på en Azure CDN-slutpunkt."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Läsa in tillgångar för en Azure CDN-slutpunkt i förväg
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Som standard cachelagras först tillgångar när de begärs. Detta innebär att hello första begäran från varje region kan ta längre tid eftersom hello edge-servrar inte har hello innehåll cachelagras och behöver tooforward hello begäran toohello ursprungsservern. Den här första träffar latens undviks förinläsning innehåll.

Dessutom kan tooproviding en bättre kundupplevelse förinläsning dina cachelagrade tillgångar också minska nätverkstrafiken på hello ursprungsservern.

> [!NOTE]
> Förinläsning tillgångar är användbart för stora händelser eller innehåll som blir tillgängliga samtidigt tooa stort antal användare, till exempel en ny film version eller en programuppdatering.
> 
> 

Den här självstudiekursen vägleder dig genom förinläsning cachelagrat innehåll på alla noder i Azure CDN-kant.

## <a name="walkthrough"></a>Genomgång
1. I hello [Azure Portal](https://portal.azure.com), bläddra toohello CDN-profil som innehåller hello-slutpunkt som du vill toopre belastningen.  hello profil blad öppnas.
2. Klicka på hello slutpunkt i hello-listan.  hello endpoint blad öppnas.
3. Klicka på hello Läs in hello CDN-slutpunkten bladet.
   
    ![CDN-slutpunkten bladet](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    hello belastningen blad öppnas.
   
    ![CDN belastningen bladet](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Ange hello fullständig sökväg för varje resurs som du vill att tooload (t.ex. `/pictures/kitten.png`) i hello **sökväg** textruta.
   
   > [!TIP]
   > Flera **sökväg** textrutor visas när du har angett texten tooallow toobuild en lista över flera resurser.  Du kan ta bort tillgångar hello listan genom att klicka på ellipsknappen för hello (...).
   > 
   > Sökvägar måste vara en relativ URL som passar hello följande [reguljärt uttryck](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Läsa in sökväg för en enskild fil `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Läsa in en enstaka fil med frågesträng`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Varje tillgång måste ha sin egen sökväg.  Det finns inga jokertecken funktioner för före inläsning tillgångar.
   > 
   > 
   
    ![Läs in](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Klicka på hello **belastningen** knappen.
   
    ![Läs in](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Det finns en begränsning på 10 belastningsförfrågningar per minut per CDN-profilen. 50 sökvägar tillåts per begäran. Varje sökväg har en sökvägens längd på högst 1024 tecken.
> 
> 

## <a name="see-also"></a>Se även
* [Rensa en Azure CDN-slutpunkt](cdn-purge-endpoint.md)
* [Azure CDN REST API-referens - rensa eller i förväg läsa in en slutpunkt](https://msdn.microsoft.com/library/mt634451.aspx)

