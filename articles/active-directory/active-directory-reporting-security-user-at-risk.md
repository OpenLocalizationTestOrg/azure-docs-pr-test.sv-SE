---
title: "aaaUsers flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen | Microsoft Docs"
description: "Lär dig mer om hello användare som flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a>Användare som har flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen

Med hello säkerhetsrapporter i hello Azure Active Directory (Azure AD), kan du få insikter om hello sannolikheten för avslöjade användarkonton i din miljö. 

Azure Active Directory identifierar misstänkt åtgärder som är relaterade tooyour användarkonton. För varje identifierad åtgärd skapas en post med namnet *riskhändelse*. Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md). 

hello upptäckt riskhändelser finns används toocalculate:

- **Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto. Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins). 

- **Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats. Mer information finns i avsnittet om [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk).  

I hello Azure-portalen, du kan hitta hello säkerhet rapporter om hello **Azure Active Directory** bladet i hello **säkerhet** avsnitt.  

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?  

Alla utgåvor av Azure Active Directory ger rapporter över användare som har flaggats för risk.  
Hello nivå av rapportens granularitet varierar dock mellan hello versioner: 

- I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över användare som har flaggats för risk. 

- Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport. 

- Hej **Azure Active Directory Premium 2** -versionen tillhandahåller hello mest detaljerad information om alla underliggande riskhändelser och gör att du tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risk nivåer.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory kostnadsfri och grundläggande utgåva

hello-användare som flaggats för risk rapporten i hello Azure Active Directory ledigt grundläggande utgåvor och ger dig en lista över användarkonton som komprometterats. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/03.png)

När du väljer en användare öppnar hello relaterade användaren data blad.
Du kan granska hello användarens inloggning historik och återställa hello lösenord om det behövs för användare som är i fara.

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/46.png)


Dialogrutan tillhandahåller ett alternativ för att:

- Hämta hello-rapport

- Söka efter användare

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory Premium-versioner

hello-användare som flaggats för risk rapporten i hello Azure Active Directory premium-versioner ger dig:

- En [lista över användarkonton](active-directory-identityprotection.md#users-flagged-for-risk) som kan ha drabbats 

- Information om hello samman [riskerar händelsetyper](active-directory-identity-protection-risk-events.md) som har identifierats

- En alternativ toodownload hello-rapport

- En alternativ tooconfigure en [användarprincip risk reparation](active-directory-identityprotection.md#user-risk-security-policy)  


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/71.png)

När du väljer en användare får du en detaljerad rapportvy för den här användaren som du kan använda för att göra följande:

- Öppna hello visa alla inloggningar

- Återställa hello användares lösenord

- Ignorera alla händelser

- Undersök rapporterade riskhändelser för hello användare. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/324.png)


tooinvestigate en risk-händelse, välja en hello listan tooopen hello **information** bladet för den här händelsen för risk. På hello **information** bladet du har hello alternativet tooeither [stänga manuellt en risk händelse](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en stängd manuellt risk-händelse. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Nästa steg

- Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

