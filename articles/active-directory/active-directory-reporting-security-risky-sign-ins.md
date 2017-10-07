---
title: aaaRisky inloggningar rapporten i hello Azure Active Directory-portalen | Microsoft Docs
description: "Lär dig mer om hello riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a>Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen

Med hello säkerhetsrapporter i Azure Active Directory (Azure AD) kan du få insikter om hello sannolikheten för avslöjade användarkonton i din miljö. 

Azure AD identifierar misstänkt åtgärder som är relaterade tooyour användarkonton. För varje identifierad åtgärd skapas en post med namnet *riskhändelse*. Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md). 

hello upptäckt riskhändelser finns används toocalculate:

- **Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto. Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins). 

- **Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats. Mer information finns i avsnittet om [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk).  

I [hello Azure-portalen](https://portal.azure.com), du kan hitta hello säkerhet rapporter om hello **Azure Active Directory** bladet i hello **säkerhet** avsnitt. 

![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?  

Alla utgåvor av Azure Active Directory ger rapporter över riskfyllda inloggningar.  
Hello nivå av rapportens granularitet varierar dock mellan hello versioner: 

- I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över riskfyllda inloggningar. 

- Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport. 

- Hej **Azure Active Directory Premium 2** versionen ger dig med hello mest detaljerad information om alla underliggande riskhändelser och du kan också tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risknivåer.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory kostnadsfri och grundläggande utgåva

hello Azure Active Directory ledigt och Basic ger dig en lista över riskfyllda inloggningar som har identifierats för dina användare. Den här rapporten innehåller:

- **Användaren** - hello namn på hello-användaren som användes under hello logga in igen
- **IP** -hello IP-adressen för hello-enhet som tidigare har använt tooconnect tooAzure Active Directory
- **Plats** -hello plats används tooconnect tooAzure Active Directory
- **Inloggningstid** -hello tid när hello-inloggning har utförts
- **Status för** -hello status för hello-inloggning


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/01.png)

Du kan ge feedback tooAzure Active Directory i form av hello följande åtgärder baserat på din undersökning av hello riskfyllda inloggning:

- Lös
- Markera som falskt positivt resultat
- Ignorera
- Återaktivera

![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/21.png)

Mer information finns i [Stänga riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).

Rapporten tillhandahåller ett alternativ för att:

- Söka resurser
- Hämta hello rapportdata


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory Premium-versioner

hello riskfyllda inloggningar rapporten i hello Azure Active Directory premium-versioner ger dig:

- Information om hello samman [riskerar händelsetyper](active-directory-identity-protection-risk-events.md) som har identifierats

- En alternativ toodownload hello-rapport


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/456.png)


När du väljer en riskhändelse kan få en detaljerad rapportvy för den här riskhändelsen som gör att du kan göra följande:

- En alternativ tooconfigure en [användarprincip risk reparation](active-directory-identityprotection.md#user-risk-security-policy)  

- Granska hello identifiering tidslinje för hello risk händelse  

- Granska en lista över användare för vilka den här riskhändelsen har upptäckts

- [Stänga riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en manuellt stängd riskhändelse. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/457.png)

När du väljer en användare får du en detaljerad rapportvy för den här användaren som du kan använda för att göra följande:

- Öppna hello visa alla inloggningar

- Återställa hello användares lösenord

- Ignorera alla händelser

- Undersök rapporterade riskhändelser för hello användare. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/324.png)


tooinvestigate en risk-händelse, välja en hello lista.  
Då öppnas hello **information** bladet för den här händelsen för risk. På hello **information** bladet du har hello alternativet tooeither [stänga manuellt en risk händelse](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en stängd manuellt risk-händelse. 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a>Nästa steg

- Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

