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
# <a name="sign-ins-from-possibly-infected-devices"></a>Inloggningar från eventuellt infekterade enheter
Den här rapporten försöker identifiera användarnas enheter som har infekteras och är nu en del av ett botnät. Vi korrelera IP-adresser för användarnas inloggningar mot IP-adresser som vi vet att har kontakt med botnät servrar.

Rekommendation: Den här rapporten flaggor IP-adresser, inte användarenheter. Vi rekommenderar att du kontakta användaren och söka igenom alla användares enheter att vara säker. Det är också möjligt att en användares personliga enhet har infekterats eller någon annan än den användare som med samma IP-adress som användaren, har en infekterad enhet.

Mer information om hur du adress infekteras av skadlig kod finns på [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).

![Inloggningar från eventuellt infekterade enheter](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

