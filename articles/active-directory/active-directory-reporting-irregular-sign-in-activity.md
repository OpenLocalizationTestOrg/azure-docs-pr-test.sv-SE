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
# <a name="irregular-sign-in-activity"></a>Oregelbunden inloggningsaktivitet
Oregelbundna inloggningar är de som har identifierats av våra maskininlärningsalgoritmer på grundval av ett ”omöjligt att resa” villkor som kombineras med avvikande inloggning plats och enheten. Detta kan tyda på att en hackare har loggat in med det här kontot.
Vi skickar ett e-postmeddelande till globala administratörer om vi får 10 eller fler avvikande inloggning händelser inom ett intervall på 30 dagar eller mindre. Kontrollera att det att inkludera aad-alerts-noreply@mail.windowsazure.com i listan med betrodda avsändare.

