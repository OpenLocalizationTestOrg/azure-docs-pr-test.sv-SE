---
title: 'Azure AD Connect-synkronisering: aktivera AD-Papperskorgen | Microsoft Docs'
description: "Det här avsnittet rekommenderar hello funktionen används för AD-Papperskorgen med Azure AD Connect."
services: active-directory
keywords: "AD-Papperskorgen, oavsiktlig borttagning, källfästpunkten"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect-synkronisering: aktivera AD-Papperskorgen
Det rekommenderas att du aktiverar hello AD Papperskorgen för din lokala Active kataloger, som är synkroniserade tooAzure AD. 

Om du av misstag tar bort en lokal AD användarobjektet och återställning med hello-funktionen, Azure AD återställs hello motsvarande Azure AD-användarobjekt.  Information om hello AD Papperskorgen finns tooarticle [översikt över scenarier för att återställa borttagna Active Directory-objekt](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a>Fördelarna med att aktivera hello AD-Papperskorgen
Den här funktionen hjälper med att återställa Azure AD-användarobjekt hello följande:

* Om du av misstag tar bort en lokal AD-användare objekt hello motsvarande Azure AD-användarobjektet kommer att tas bort i hello nästa synkronisering cykel. Som standard sparas Azure AD hello bort Azure AD-användarobjektet i ej permanent borttagna tillstånd i 30 dagar.

* Om du har lokala AD-Papperskorgen funktionen är aktiverad kan du återställa hello bort lokala AD-användarobjektet utan att ändra värdet Källfästpunkt. När hello återställts lokalt AD användarobjektet synkroniseras tooAzure AD Azure AD återställs hello motsvarande ej permanent borttagna objektet för Azure AD-användare. Information om Källfästpunkten attributet finns tooarticle [Azure AD Connect: Designbegreppen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Om du inte har lokala AD Återanvänd Bin funktionen är aktiverad kan du vara nödvändiga toocreate ett AD-objekt tooreplace hello borttagna användarobjekt. Om Azure AD Connect-synkroniseringstjänsten finns konfigurerade toouse systemgenererade AD attribut (till exempel ObjectGuid) för hello Källfästpunkten attribut, kommer hello nyskapade AD användarobjektet inte har hello samma Källfästpunkten värde eftersom hello bort användarobjekt i AD. När hello nyskapade AD användarobjektet är synkroniserade tooAzure AD, Azure AD skapar en ny Azure AD-användarobjektet i stället för att återställa hello ej permanent borttagna objektet för Azure AD-användare.

> [!NOTE]
> Som standard bort behåller Azure AD Azure AD-användarobjekt i ej permanent borttagna tillstånd i 30 dagar innan de tas bort permanent. Administratörer kan dock accelerera hello borttagning av dessa objekt. När hello objekt tas bort permanent de inte längre kan återställas-, även om lokala AD-Papperskorgen är aktiverad.



## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)

* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
