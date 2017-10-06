---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - ange krav för multifaktorautentisering"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Ange krav för multifaktorautentisering för dina hybrididentitetslösning
I den här värld mobilitet, med användare som ansluter till data och program i molnet hello och från valfri enhet, blivit skydda den här informationen ytterst viktigt.  Varje dag är det en ny rubrik om ett intrång.  Även om det är inte säkert mot sådana överträdelser, multifaktorautentisering, ger ett ytterligare lager av säkerhet toohelp förhindra dessa överträdelser.
Börja med att utvärdera hello organisationer krav för multifaktorautentisering. Vad är hello organisation försök toosecure.  Denna utvärdering är viktiga toodefine hello tekniska krav för att installera och aktivera hello organisationer användare för multifaktorautentisering.

> [!NOTE]
> Om du inte är bekant med MFA och syfte rekommenderas starkt att du läser hello artikel [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) tidigare toocontinue läser det här avsnittet.
> 
> 

Se till att tooanswer hello följande:

* Försöker toosecure Microsoft-appar i företaget? 
* Hur dessa appar är publicerade?
* Företaget kan ge fjärråtkomst tooallow anställda tooaccess lokala appar?

Om Ja, vilken typ av fjärråtkomst? Du måste också tooevaluate där hello användare som kommer åt dessa program kommer att finnas. Denna utvärdering är ett annat viktigt steg toodefine hello rätt multifaktorautentisering strategi. Se till att tooanswer hello följande frågor:

* Var finns hello användare ska toobe?
* Kan de finnas var som helst?
* Vill ditt företag tooestablish begränsningar enligt toohello användarens plats?

När du förstår dessa krav är det viktigt tooalso utvärdera hello användarens krav för multifaktorautentisering. Denna utvärdering är viktig eftersom den definierar hello krav för lansera multifaktorautentisering. Se till att tooanswer hello följande frågor:

* Känner hello användare med Multi-Factor authentication?
* Vissa använder kommer vara nödvändiga tooprovide ytterligare autentisering?  
  * Om Ja, alla hello gången när de kommer från externa nätverk eller vid åtkomst till specifika program eller andra villkor?
* Hello användare kräver utbildning om hur toosetup och implementera multifaktorautentisering?
* Vad är hello viktiga scenarier som företaget vill tooenable Multi-Factor authentication för användarna?

När besvara hello föregående frågor, kommer du att kunna toounderstand om multifaktorautentisering redan implementerats lokala. Denna utvärdering är viktiga toodefine hello tekniska krav för att installera och aktivera hello organisationer användare för multifaktorautentisering. Se till att tooanswer hello följande frågor:

* Behöver företaget tooprotect Privilegierade konton med MFA?
* Behöver företaget tooenable MFA för vissa program på grund av kompatibilitetsskäl?
* Behöver företaget tooenable MFA för alla berättigade användare av dessa program eller endast administratörer?
* Du behöver har alltid aktiverat MFA eller bara när hello användare är inloggade utanför företagets nätverk?

## <a name="next-steps"></a>Nästa steg
[Definiera en strategi för införandet en hybrid identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

