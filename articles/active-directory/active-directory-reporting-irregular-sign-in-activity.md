---
title: Oregelbunden inloggningsaktivitet
description: "En rapport som innehåller inloggningar som har identifierats som avvikande av våra maskininlärningsalgoritmer."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: a5a270a9-a0eb-4a99-b04c-ccde3829ffe4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 565321b6f3ad5988f383a701cb9d8bd5c9937795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="irregular-sign-in-activity"></a><span data-ttu-id="a79a0-103">Oregelbunden inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="a79a0-103">Irregular sign-in activity</span></span>
<span data-ttu-id="a79a0-104">Oregelbundna inloggningar är de som har identifierats av våra maskininlärningsalgoritmer på grundval av ett ”omöjligt att resa” villkor som kombineras med avvikande inloggning plats och enheten.</span><span class="sxs-lookup"><span data-stu-id="a79a0-104">Irregular sign-ins are those that have been identified by our machine learning algorithms, on the basis of an "impossible travel" condition combined with an anomalous sign in location and device.</span></span> <span data-ttu-id="a79a0-105">Detta kan tyda på att en hackare har loggat in med det här kontot.</span><span class="sxs-lookup"><span data-stu-id="a79a0-105">This may indicate that a hacker has successfully signed in using this account.</span></span>
<span data-ttu-id="a79a0-106">Vi skickar ett e-postmeddelande till globala administratörer om vi får 10 eller fler avvikande inloggning händelser inom ett intervall på 30 dagar eller mindre.</span><span class="sxs-lookup"><span data-stu-id="a79a0-106">We will send an email notification to the global admins if we encounter 10 or more anomalous sign-in events within a span of 30 days or less.</span></span> <span data-ttu-id="a79a0-107">Kontrollera att det att inkludera aad-alerts-noreply@mail.windowsazure.com i listan med betrodda avsändare.</span><span class="sxs-lookup"><span data-stu-id="a79a0-107">Please be sure to include aad-alerts-noreply@mail.windowsazure.com in your safe senders list.</span></span>

