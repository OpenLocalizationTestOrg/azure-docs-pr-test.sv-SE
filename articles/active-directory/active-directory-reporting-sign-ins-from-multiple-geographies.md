---
title: "Inloggningar från flera geografiska områden"
description: "En rapport som visar användare där två inloggningar verkar har gjorts från olika regioner och tiden mellan de moduler gör det omöjligt för användaren att ha förflyttat sig mellan de två regionerna."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: 79259c8a-2388-4747-b41e-c07434ea9a02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 1de57f64692ade442f9ef8d1e3b587ffee35d7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-multiple-geographies"></a>Inloggningar från flera geografiska områden
Den här rapporten innehåller lyckade inloggningar från en användare där två inloggningar verkar har gjorts från olika regioner och tiden mellan inloggningarna gör det omöjligt att användare kunna ha förflyttat sig mellan de två regionerna. Möjliga orsaker är:

* Användare delar sina lösenord med andra användare
* Användare använder ett fjärrskrivbord för att starta en webbläsare för inloggning
* En hackare har loggat in på kontot för en användare från ett annat land
* Användaren använder en VPN- eller proxy
* Användaren är inloggad från flera enheter samtidigt som en stationär dator och en mobiltelefon och IP-adressen för mobiltelefonen är annorlunda.

Resultaten från den här rapporten visar lyckad inloggning händelser, tillsammans med tiden mellan inloggningarna, de regioner där inloggningarna verkar har gjorts från och den uppskattade tid mellan de två regionerna. Den tid visas är endast en uppskattning och kan skilja sig från den faktiska tid mellan platser.

![Inloggningar från flera geografiska områden](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

