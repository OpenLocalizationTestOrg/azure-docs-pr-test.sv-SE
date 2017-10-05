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
# <a name="sign-ins-from-multiple-geographies"></a><span data-ttu-id="5ae20-103">Inloggningar från flera geografiska områden</span><span class="sxs-lookup"><span data-stu-id="5ae20-103">Sign-ins from multiple geographies</span></span>
<span data-ttu-id="5ae20-104">Den här rapporten innehåller lyckade inloggningar från en användare där två inloggningar verkar har gjorts från olika regioner och tiden mellan inloggningarna gör det omöjligt att användare kunna ha förflyttat sig mellan de två regionerna.</span><span class="sxs-lookup"><span data-stu-id="5ae20-104">This report includes successful sign-ins from a user where two sign-ins appeared to originate from different regions and the time between the sign-ins makes it impossible for the user to have traveled between those regions.</span></span> <span data-ttu-id="5ae20-105">Möjliga orsaker är:</span><span class="sxs-lookup"><span data-stu-id="5ae20-105">Possible causes include:</span></span>

* <span data-ttu-id="5ae20-106">Användare delar sina lösenord med andra användare</span><span class="sxs-lookup"><span data-stu-id="5ae20-106">User is sharing their password with other users</span></span>
* <span data-ttu-id="5ae20-107">Användare använder ett fjärrskrivbord för att starta en webbläsare för inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae20-107">User is using a remote desktop to launch a web browser for sign-in</span></span>
* <span data-ttu-id="5ae20-108">En hackare har loggat in på kontot för en användare från ett annat land</span><span class="sxs-lookup"><span data-stu-id="5ae20-108">A hacker has signed in to the account of a user from a different country</span></span>
* <span data-ttu-id="5ae20-109">Användaren använder en VPN- eller proxy</span><span class="sxs-lookup"><span data-stu-id="5ae20-109">User is using a VPN or proxy</span></span>
* <span data-ttu-id="5ae20-110">Användaren är inloggad från flera enheter samtidigt som en stationär dator och en mobiltelefon och IP-adressen för mobiltelefonen är annorlunda.</span><span class="sxs-lookup"><span data-stu-id="5ae20-110">User is signed in from multiple devices at the same time, such as a desktop and a mobile phone, and the IP address of the mobile phone is unusual.</span></span>

<span data-ttu-id="5ae20-111">Resultaten från den här rapporten visar lyckad inloggning händelser, tillsammans med tiden mellan inloggningarna, de regioner där inloggningarna verkar har gjorts från och den uppskattade tid mellan de två regionerna.</span><span class="sxs-lookup"><span data-stu-id="5ae20-111">Results from this report will show you the successful sign-in events, together with the time between the sign-ins, the regions where the sign-ins appeared to originate from, and the estimated travel time between those regions.</span></span> <span data-ttu-id="5ae20-112">Den tid visas är endast en uppskattning och kan skilja sig från den faktiska tid mellan platser.</span><span class="sxs-lookup"><span data-stu-id="5ae20-112">The travel time shown is only an estimate and may be different from the actual travel time between the locations.</span></span>

![Inloggningar från flera geografiska områden](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

