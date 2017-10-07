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
# <a name="azure-cdn-rules-engine-conditional-expressions"></a><span data-ttu-id="f1841-103">Azure CDN regler motorn villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="f1841-103">Azure CDN rules engine conditional expressions</span></span>
<span data-ttu-id="f1841-104">Det här avsnittet innehåller detaljerade beskrivningar av hello villkorsuttryck för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="f1841-104">This topic lists detailed descriptions of hello Conditional Expressions for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="f1841-105">hello första delen av en regel är hello villkorsuttryck.</span><span class="sxs-lookup"><span data-stu-id="f1841-105">hello first part of a rule is hello Conditional Expression.</span></span>

<span data-ttu-id="f1841-106">Villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="f1841-106">Conditional Expression</span></span> | <span data-ttu-id="f1841-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1841-107">Description</span></span>
-----------------------|-------------
<span data-ttu-id="f1841-108">OM</span><span class="sxs-lookup"><span data-stu-id="f1841-108">IF</span></span> | <span data-ttu-id="f1841-109">Ett IF-uttryck är alltid en del av hello första satsen i en regel.</span><span class="sxs-lookup"><span data-stu-id="f1841-109">An IF expression is always a part of hello first statement in a rule.</span></span> <span data-ttu-id="f1841-110">Precis som andra villkorsuttryck måste om-satsen vara kopplad till en matchning.</span><span class="sxs-lookup"><span data-stu-id="f1841-110">Like all other conditional expressions, this IF statement must be associated with a match.</span></span> <span data-ttu-id="f1841-111">Om inga ytterligare villkorsuttryck definieras anger matchningen hello villkor som måste uppfyllas innan en uppsättning funktioner kanske tillämpade tooa begäran.</span><span class="sxs-lookup"><span data-stu-id="f1841-111">If no additional conditional expressions are defined, then this match determines hello criterion that must be met before a set of features may be applied tooa request.</span></span>
<span data-ttu-id="f1841-112">OCH OM</span><span class="sxs-lookup"><span data-stu-id="f1841-112">AND IF</span></span> | <span data-ttu-id="f1841-113">Ett uttryck och om kan endast läggas till efter hello följande typer av villkorliga uttryck: om, och om.</span><span class="sxs-lookup"><span data-stu-id="f1841-113">An AND IF expression may only be added after hello following types of conditional expressions:IF,AND IF.</span></span> <span data-ttu-id="f1841-114">Anger att det finns ytterligare ett villkor som måste uppfyllas för hello första om-satsen.</span><span class="sxs-lookup"><span data-stu-id="f1841-114">It indicates that there is another condition that must be met for hello initial IF statement.</span></span>
<span data-ttu-id="f1841-115">ANNARS OM</span><span class="sxs-lookup"><span data-stu-id="f1841-115">ELSE IF</span></span>| <span data-ttu-id="f1841-116">ANNARS om uttryck anger ett alternativt villkor som måste uppfyllas innan en uppsättning funktioner specifika toothis ANNARS om instruktionen äger rum.</span><span class="sxs-lookup"><span data-stu-id="f1841-116">An ELSE IF expression specifies an alternative condition that must be met before a set of features specific toothis ELSE IF statement takes place.</span></span> <span data-ttu-id="f1841-117">hello förekomsten av en annan om instruktionen anger hello satsslut hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="f1841-117">hello presence of an ELSE IF statement indicates hello end of hello previous statement.</span></span> <span data-ttu-id="f1841-118">hello endast villkorsuttryck som kan vara placerad efter uttrycket ANNARS om är en annan ELSE IF-sats.</span><span class="sxs-lookup"><span data-stu-id="f1841-118">hello only conditional expression that may be placed after an ELSE IF statement is another ELSE IF statement.</span></span> <span data-ttu-id="f1841-119">Det innebär att ett ELSE IF-uttryck får endast används toospecify ett enda ytterligare villkor som har toobe uppfylls.</span><span class="sxs-lookup"><span data-stu-id="f1841-119">This means that an ELSE IF statement may only be used toospecify a single additional condition that has toobe met.</span></span>

<span data-ttu-id="f1841-120">**Exempel**: ![CDN matchar villkoret](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span><span class="sxs-lookup"><span data-stu-id="f1841-120">**Example**: ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span></span>

 > [!TIP]
   > <span data-ttu-id="f1841-121">En efterföljande regel kan åsidosätta hello-åtgärder som angetts av en tidigare regel.</span><span class="sxs-lookup"><span data-stu-id="f1841-121">A subsequent rule may override hello actions specified by a previous rule.</span></span> <span data-ttu-id="f1841-122">Exempel: En fångar in alla regel skyddar alla begäranden via Token-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="f1841-122">Example: A catch-all rule secures all requests via Token-Based Authentication.</span></span> <span data-ttu-id="f1841-123">En annan regel som kan skapas direkt under toomake ett undantag för vissa typer av begäranden.</span><span class="sxs-lookup"><span data-stu-id="f1841-123">Another rule may be created directly below it toomake an exception for certain types of requests.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f1841-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1841-124">Next steps</span></span>
* [<span data-ttu-id="f1841-125">Azure CDN-översikt</span><span class="sxs-lookup"><span data-stu-id="f1841-125">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="f1841-126">Regler modulreferens</span><span class="sxs-lookup"><span data-stu-id="f1841-126">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="f1841-127">Regler motorn matchar villkor</span><span class="sxs-lookup"><span data-stu-id="f1841-127">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="f1841-128">Regler motorn funktioner</span><span class="sxs-lookup"><span data-stu-id="f1841-128">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="f1841-129">Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor</span><span class="sxs-lookup"><span data-stu-id="f1841-129">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
