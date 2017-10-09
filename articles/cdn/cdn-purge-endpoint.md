---
title: aaaPurge en Azure CDN-slutpunkt | Microsoft Docs
description: "Lär dig hur toopurge alla cachelagrat innehåll från en Azure CDN-slutpunkt."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a>Rensa en Azure CDN-slutpunkt
## <a name="overview"></a>Översikt
Azure CDN edge noder cachelagras tillgångar tills hello tillgången time to live (TTL) upphör att gälla.  När hello tillgången TTL upphör att gälla, när en klient begär hello tillgångsinformation från hello kantnod, hämtar en ny uppdaterad kopia av hello tillgången tooserve hello klientbegäran hello kantnod och lagra hello cacheuppdatering.

hello bästa praxis toomake till användarna alltid få hello senaste kopian av dina tillgångar är tooversion dina tillgångar för varje uppdatera och publicera dem som nya URL: er.  CDN kommer omedelbart att hämta hello nya tillgångar för hello nästa klientbegäranden.  Ibland kanske du vill toopurge cachelagrat innehåll från alla noder i kant och tvinga dem alla tooretrieve nya uppdaterade tillgångar.  Detta kan bero på att tooupdates tooyour webbprogram eller tooquickly uppdatering tillgångar som innehåller felaktig information.

> [!TIP]
> Observera att rensa endast rensar hello cachelagrat innehåll på hello CDN edge-servrar.  Alla underordnade cacheminnen, till exempel proxyservrar och lokala browser cacheminnen kan fortfarande håller en cachelagrad kopia av hello-filen.  Det är viktigt tooremember detta när du ställer in en fil time-to-live.  Du kan tvinga en underordnad klienten toorequest hello senaste versionen av filen genom att ge den ett unikt namn varje gång du uppdaterar den eller genom att utnyttja [fråga cachelagring av frågesträngar](cdn-query-string.md).  
> 
> 

Den här självstudiekursen vägleder dig genom att rensa tillgångar från alla noder i kanten av en slutpunkt.

## <a name="walkthrough"></a>Genomgång
1. I hello [Azure Portal](https://portal.azure.com), bläddra toohello CDN-profil som innehåller hello-slutpunkt som du vill toopurge.
2. Från hello CDN-profilbladet Klicka hello Rensa.
   
    ![CDN-profilbladet](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    hello Rensa blad öppnas.
   
    ![CDN Rensa bladet](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Rensa bladet på hello, Välj hello-adress som du vill toopurge hello URL listrutan.
   
    ![Rensa formulär](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Du kan också få toohello Rensa bladet genom att klicka på hello **Rensa** hello CDN-slutpunkten bladet-knappen.  I så fall hello **URL** fältet kommer att vara förifyllda med hello-adress för den specifika slutpunkten.
   > 
   > 
4. Välj vilka resurser du vill toopurge från hello edge noder.  Om du inte vill tooclear alla tillgångar, klickar du på hello **Rensa alla** kryssrutan.  Annars ange hello sökväg för varje resurs du vill toopurge i hello **sökväg** textruta. Under format stöds i hello sökväg.
    1. **Den enda URL Rensa**: Rensa enskilda tillgångar genom att ange hello fullständiga URL-adress med eller utan hello filtillägget, t.ex.`/pictures/strasbourg.png`;`/pictures/strasbourg`
    2. **Jokertecken Rensa**: Asterisk (\*) kan användas som jokertecken. Rensa alla mappar, undermappar och filer på en slutpunkt med `/*` hello sökväg eller rensa alla undermappar och filer under en viss mapp genom att ange hello mappen följt av `/*`, t.ex.`/pictures/*`.  Observera att rensa jokertecken inte stöds av Azure CDN från Akamai för närvarande. 
    3. **Roten domän Rensa**: Rensa hello rot hello slutpunkt med ”/” i hello sökväg.
   
   > [!TIP]
   > Sökvägar måste anges för rensning och måste vara en relativ URL som passar hello följande [reguljärt uttryck](https://msdn.microsoft.com/library/az24scfc.aspx). **Rensa alla** och **jokertecken Rensa** stöds inte av **Azure CDN från Akamai** för närvarande.
   > > Enstaka URL-rensning`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Frågesträng`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Jokertecken Rensa `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Flera **sökväg** textrutor visas när du har angett texten tooallow toobuild en lista över flera resurser.  Du kan ta bort tillgångar hello listan genom att klicka på ellipsknappen för hello (...).
   > 
5. Klicka på hello **Rensa** knappen.
   
    ![Rensa knappen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Rensa begäranden tar cirka 2 – 3 minuter tooprocess med **Azure CDN från Verizon** (Standard och Premium) och cirka 7: e minut med **Azure CDN från Akamai**.  Azure CDN är begränsad till 50 samtidiga Rensa begäranden samtidigt. 
> 
> 

## <a name="see-also"></a>Se även
* [Läsa in tillgångar för en Azure CDN-slutpunkt i förväg](cdn-preload-endpoint.md)
* [Azure CDN REST API-referens - rensa eller i förväg läsa in en slutpunkt](https://msdn.microsoft.com/library/mt634451.aspx)

