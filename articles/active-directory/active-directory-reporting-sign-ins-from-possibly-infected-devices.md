---
title: "Inloggningar från eventuellt infekterade enheter"
description: "En rapport som innehåller tecknet inloggningsförsök som har utförts från enheter som vissa skadlig programvara (skadlig programvara) körs."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: d0361662-d970-4a66-8eb3-72e9f8824dcf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 3809e20937d8d9829675e20f893101cb849dcea2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-possibly-infected-devices"></a><span data-ttu-id="98dad-103">Inloggningar från eventuellt infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="98dad-103">Sign ins from possibly infected devices</span></span>
<span data-ttu-id="98dad-104">Den här rapporten försöker identifiera användarnas enheter som har infekteras och är nu en del av ett botnät.</span><span class="sxs-lookup"><span data-stu-id="98dad-104">This report attempts to identify your users' devices that that have become infected and are now part of a botnet.</span></span> <span data-ttu-id="98dad-105">Vi korrelera IP-adresser för användarnas inloggningar mot IP-adresser som vi vet att har kontakt med botnät servrar.</span><span class="sxs-lookup"><span data-stu-id="98dad-105">We correlate IP addresses of users' sign-ins against IP addresses that we know to be in contact with botnet servers.</span></span>

<span data-ttu-id="98dad-106">Rekommendation: Den här rapporten flaggor IP-adresser, inte användarenheter.</span><span class="sxs-lookup"><span data-stu-id="98dad-106">Recommendation: This report flags IP addresses, not user devices.</span></span> <span data-ttu-id="98dad-107">Vi rekommenderar att du kontakta användaren och söka igenom alla användares enheter att vara säker.</span><span class="sxs-lookup"><span data-stu-id="98dad-107">We recommend that you contact the user and scan all the user's devices to be certain.</span></span> <span data-ttu-id="98dad-108">Det är också möjligt att en användares personliga enhet har infekterats eller någon annan än den användare som med samma IP-adress som användaren, har en infekterad enhet.</span><span class="sxs-lookup"><span data-stu-id="98dad-108">It is also possible that a user's personal device is infected, or that someone other than the user, who was using the same IP address as the user, has an infected device.</span></span>

<span data-ttu-id="98dad-109">Mer information om hur du adress infekteras av skadlig kod finns på [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span><span class="sxs-lookup"><span data-stu-id="98dad-109">For more information about how to address malware infections, see the [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span></span>

![Inloggningar från eventuellt infekterade enheter](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

