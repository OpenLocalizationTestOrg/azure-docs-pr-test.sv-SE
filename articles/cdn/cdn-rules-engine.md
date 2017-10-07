---
title: "aaaOverride HTTP beteende med hjälp av hello Azure CDN regelmotor | Microsoft Docs"
description: "hello regelmotor kan du toocustomize hur Azure CDN hanteras HTTP-begäranden som blockerar hello leverans av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a>Åsidosätta HTTP beteende med hjälp av hello Azure CDN regelmotor
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Översikt
hello regelmotor kan toocustomize hur HTTP-begäranden hanteras som blockerar hello leverans av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden.  Den här kursen visar att skapa en regel som ändrar hello cachelagring av frågesträngar CDN tillgångar.  Det finns också videoinnehåll i hello ”[finns också](#see-also)” avsnittet.

   > [!TIP] 
   > En referens toohello syntax i detalj finns [regler modulreferens](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Självstudier
1. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Klicka på hello **HTTP stora** TABB, följt av **regelmotor**.
   
    Alternativ för en ny regel visas.
   
    ![CDN nya alternativ för regel](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > hello ordning i vilken flera regler påverkar hur de ska hanteras. En efterföljande regel kan åsidosätta hello-åtgärder som angetts av en tidigare regel.
   > 
   > 
3. Ange ett namn i hello **namn / beskrivning** textruta.
4. Identifiera hello typ av förfrågningar hello regeln gäller för.  Som standard hello **alltid** matchar villkoret är markerad.  Du kommer att använda **alltid** för den här självstudiekursen, så klicka på OK.
   
   ![CDN matchar villkoret](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Det finns många typer av matchar villkoren som är tillgängliga i listrutan hello.  Klicka på hello blå informationsmeddelande ikonen toohello förklarar till vänster i hello matchar villkoret hello valt villkor i detalj.
   > 
   >  Hello fullständig lista över villkorsuttryck i detalj, se [regler motorn villkorsuttryck](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Hello fullständig lista över matchar villkoren i detalj, se [regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Klicka på hello  **+**  knappen för nästa**funktioner** tooadd en ny funktion.  Välj i listrutan hello hello vänster **kraft interna Max-ålder**.  Ange i hello textrutan som visas, **300**.  Lämna hello återstående standardvärdena.
   
   ![CDN-funktion](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Som matchar villkoren klickar du på hello blå informationsmeddelande ikonen toohello vänsterkant hello nya visar funktionen information om den här funktionen.  Hello gäller **kraft interna Max-ålder**, vi åsidosätter hello tillgången **Cache-Control** och **Expires** huvuden toocontrol när hello CDN kantnod uppdateras hello tillgångsinformation från hello ursprunget.  Vårt exempel 300 sekunder innebär hello CDN kantnod cachelagras hello tillgången i 5 minuter innan du uppdaterar hello tillgångsinformation från sin ursprungsplats.
   > 
   > Hello fullständig lista över funktioner i detalj finns [regler motorn funktionen information](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Klicka på hello **Lägg till** knappen toosave hello ny regel.  hello ny regel nu väntar på godkännande. När den har godkänts hello status ändras från **väntande XML** för**Active XML**.
   
   > [!IMPORTANT]
   > Regler ändringarna kan ta too90 minuter toopropagate via hello CDN.
   > 
   > 

## <a name="see-also"></a>Se även
* [Azure CDN-översikt](cdn-overview.md)
* [Regler modulreferens](cdn-rules-engine-reference.md)
* [Regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md)
* [Regler motorn villkorsuttryck](cdn-rules-engine-reference-conditional-expressions.md)
* [Regler motorn funktioner](cdn-rules-engine-reference-features.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
* [Azure fredagar: Azure CDN kraftfulla nya Premiumfunktioner](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)