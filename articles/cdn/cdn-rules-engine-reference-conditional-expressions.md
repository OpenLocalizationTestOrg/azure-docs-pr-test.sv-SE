---
title: aaaAzure CDN regler motorn villkorsuttryck | Microsoft Docs
description: "I referensdokumentationen för Azure CDN regler motorn matchar villkoren och funktioner."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 39d0754c34a577f77ca87b6fd92e2b6a9e4ff8fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Azure CDN regler motorn villkorsuttryck
Det här avsnittet innehåller detaljerade beskrivningar av hello villkorsuttryck för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).

hello första delen av en regel är hello villkorsuttryck.

Villkorsuttryck | Beskrivning
-----------------------|-------------
OM | Ett IF-uttryck är alltid en del av hello första satsen i en regel. Precis som andra villkorsuttryck måste om-satsen vara kopplad till en matchning. Om inga ytterligare villkorsuttryck definieras anger matchningen hello villkor som måste uppfyllas innan en uppsättning funktioner kanske tillämpade tooa begäran.
OCH OM | Ett uttryck och om kan endast läggas till efter hello följande typer av villkorliga uttryck: om, och om. Anger att det finns ytterligare ett villkor som måste uppfyllas för hello första om-satsen.
ANNARS OM| ANNARS om uttryck anger ett alternativt villkor som måste uppfyllas innan en uppsättning funktioner specifika toothis ANNARS om instruktionen äger rum. hello förekomsten av en annan om instruktionen anger hello satsslut hello tidigare. hello endast villkorsuttryck som kan vara placerad efter uttrycket ANNARS om är en annan ELSE IF-sats. Det innebär att ett ELSE IF-uttryck får endast används toospecify ett enda ytterligare villkor som har toobe uppfylls.

**Exempel**: ![CDN matchar villkoret](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > En efterföljande regel kan åsidosätta hello-åtgärder som angetts av en tidigare regel. Exempel: En fångar in alla regel skyddar alla begäranden via Token-baserad autentisering. En annan regel som kan skapas direkt under toomake ett undantag för vissa typer av begäranden.

### <a name="next-steps"></a>Nästa steg
* [Azure CDN-översikt](cdn-overview.md)
* [Regler modulreferens](cdn-rules-engine-reference.md)
* [Regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md)
* [Regler motorn funktioner](cdn-rules-engine-reference-features.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
