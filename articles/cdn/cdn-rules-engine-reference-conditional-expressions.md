---
title: Azure CDN regler motorn villkorsuttryck | Microsoft Docs
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
ms.openlocfilehash: 57e56c38e003cb83dcf44f455c4451d159db8a59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a><span data-ttu-id="b9c84-103">Azure CDN regler motorn villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="b9c84-103">Azure CDN rules engine conditional expressions</span></span>
<span data-ttu-id="b9c84-104">Det här avsnittet innehåller detaljerade beskrivningar av villkorliga uttryck för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="b9c84-104">This topic lists detailed descriptions of the Conditional Expressions for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="b9c84-105">Den första delen av en regel är villkorsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="b9c84-105">The first part of a rule is the Conditional Expression.</span></span>

<span data-ttu-id="b9c84-106">Villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="b9c84-106">Conditional Expression</span></span> | <span data-ttu-id="b9c84-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b9c84-107">Description</span></span>
-----------------------|-------------
<span data-ttu-id="b9c84-108">OM</span><span class="sxs-lookup"><span data-stu-id="b9c84-108">IF</span></span> | <span data-ttu-id="b9c84-109">Ett IF-uttryck är alltid en del av den första satsen i en regel.</span><span class="sxs-lookup"><span data-stu-id="b9c84-109">An IF expression is always a part of the first statement in a rule.</span></span> <span data-ttu-id="b9c84-110">Precis som andra villkorsuttryck måste om-satsen vara kopplad till en matchning.</span><span class="sxs-lookup"><span data-stu-id="b9c84-110">Like all other conditional expressions, this IF statement must be associated with a match.</span></span> <span data-ttu-id="b9c84-111">Om inga ytterligare villkorsuttryck definieras anger matchningen villkor som måste uppfyllas innan en uppsättning funktioner som kan tillämpas på en begäran.</span><span class="sxs-lookup"><span data-stu-id="b9c84-111">If no additional conditional expressions are defined, then this match determines the criterion that must be met before a set of features may be applied to a request.</span></span>
<span data-ttu-id="b9c84-112">OCH OM</span><span class="sxs-lookup"><span data-stu-id="b9c84-112">AND IF</span></span> | <span data-ttu-id="b9c84-113">Ett uttryck och om kan endast läggas till efter följande typer av villkorliga uttryck: om, och om.</span><span class="sxs-lookup"><span data-stu-id="b9c84-113">An AND IF expression may only be added after the following types of conditional expressions:IF,AND IF.</span></span> <span data-ttu-id="b9c84-114">Anger att det finns ytterligare ett villkor som måste uppfyllas för den första om-satsen.</span><span class="sxs-lookup"><span data-stu-id="b9c84-114">It indicates that there is another condition that must be met for the initial IF statement.</span></span>
<span data-ttu-id="b9c84-115">ANNARS OM</span><span class="sxs-lookup"><span data-stu-id="b9c84-115">ELSE IF</span></span>| <span data-ttu-id="b9c84-116">ANNARS om uttryck anger ett alternativt villkor som måste uppfyllas innan en uppsättning funktioner som är specifika för den här ANNARS om instruktionen äger rum.</span><span class="sxs-lookup"><span data-stu-id="b9c84-116">An ELSE IF expression specifies an alternative condition that must be met before a set of features specific to this ELSE IF statement takes place.</span></span> <span data-ttu-id="b9c84-117">Förekomsten av en annan om instruktionen anger slutet av den föregående instruktionen.</span><span class="sxs-lookup"><span data-stu-id="b9c84-117">The presence of an ELSE IF statement indicates the end of the previous statement.</span></span> <span data-ttu-id="b9c84-118">Endast villkorsuttrycket som kan vara placerad efter uttrycket ANNARS om är en annan ELSE IF-sats.</span><span class="sxs-lookup"><span data-stu-id="b9c84-118">The only conditional expression that may be placed after an ELSE IF statement is another ELSE IF statement.</span></span> <span data-ttu-id="b9c84-119">Det innebär ett ELSE IF-uttryck får endast användas för att ange ett enda ytterligare villkor som måste uppfyllas.</span><span class="sxs-lookup"><span data-stu-id="b9c84-119">This means that an ELSE IF statement may only be used to specify a single additional condition that has to be met.</span></span>

<span data-ttu-id="b9c84-120">**Exempel**: ![CDN matchar villkoret](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span><span class="sxs-lookup"><span data-stu-id="b9c84-120">**Example**: ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span></span>

 > [!TIP]
   > <span data-ttu-id="b9c84-121">En efterföljande regel kan åsidosätta de åtgärder som anges av en tidigare regel.</span><span class="sxs-lookup"><span data-stu-id="b9c84-121">A subsequent rule may override the actions specified by a previous rule.</span></span> <span data-ttu-id="b9c84-122">Exempel: En fångar in alla regel skyddar alla begäranden via Token-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="b9c84-122">Example: A catch-all rule secures all requests via Token-Based Authentication.</span></span> <span data-ttu-id="b9c84-123">En annan regel som kan skapas direkt under den så att ett undantag för vissa typer av begäranden.</span><span class="sxs-lookup"><span data-stu-id="b9c84-123">Another rule may be created directly below it to make an exception for certain types of requests.</span></span>

### <a name="next-steps"></a><span data-ttu-id="b9c84-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9c84-124">Next steps</span></span>
* [<span data-ttu-id="b9c84-125">Azure CDN-översikt</span><span class="sxs-lookup"><span data-stu-id="b9c84-125">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="b9c84-126">Regler modulreferens</span><span class="sxs-lookup"><span data-stu-id="b9c84-126">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="b9c84-127">Regler motorn matchar villkor</span><span class="sxs-lookup"><span data-stu-id="b9c84-127">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="b9c84-128">Regler motorn funktioner</span><span class="sxs-lookup"><span data-stu-id="b9c84-128">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="b9c84-129">Åsidosätta standardbeteendet i HTTP-motorn regler</span><span class="sxs-lookup"><span data-stu-id="b9c84-129">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
