---
title: aaaSign i upplevelser med Azure AD Identity Protection | Microsoft Docs
description: "Ger en översikt över hello användarupplevelsen när identitetsskydd har minskas eller åtgärdad en användare eller när multifaktorautentisering krävs av en princip."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Logga in som inträffar med Azure AD Identity Protection
Med Azure Active Directory identitetsskydd kan du:

* Kräv användare tooregister för multifaktorautentisering
* Hantera riskfyllda inloggningar och avslöjade användare

hello svar hello system toothese problem påverkar på en användares inloggning eftersom bara direkt logga in genom att ange ett användarnamn och lösenord inte är möjligt längre. Ytterligare steg är nödvändiga tooget som en användare på ett säkert sätt tillbaka till företag.

Det här avsnittet ger en översikt över en användares inloggning i samtliga fall som kan uppstå.

**Multi-Factor Authentication**

* Registreringen för multifaktorautentisering

**Logga in i fara**

* Återställning för riskfyllda inloggning
* Riskfyllda inloggning blockeras
* Multifaktorautentisering registrering under en riskfyllda inloggning

**Användare i fara**

* Komprometterat kontoåterställning
* Komprometterat konto blockeras

## <a name="multi-factor-authentication-registration"></a>Registreringen för multifaktorautentisering
Hej bästa användarupplevelsen för både, hello komprometterat konto recovery flödet och hello flödet för riskfyllda inloggning, när hello användaren själv kan återställa. Om användarna har registrerats för multifaktorautentisering, har de redan ett telefonnummer som är associerad med ett konto som kan använda toopass säkerhetsutmaningar. Ingen hjälp supportavdelningen eller administratören inblandning är nödvändiga toorecover från kontot angrepp. Därför har hög rekommendationen tooget användarna registrerad för multifaktorautentisering. 

Administratörer kan:

* Ange en princip som kräver att användare tooset upp sina konton för ytterligare säkerhetsverifiering. 
* Tillåt hoppa över multifaktorautentisering registrering för in too30 dagar, om de vill toogive användare respitperiod innan du registrerar.

**Hej multifaktorautentisering registrering har tre steg:**

1. I första steget hello får hello användaren ett meddelande om hello krav tooset hello kontot för multifaktorautentisering. 
   
    ![Reparation](./media/active-directory-identityprotection-flows/140.png "reparation")
2. tooset multifaktorautentisering upp, behöver du toolet hello systemet vet du hur toobe kontaktas.
   
    ![Reparation](./media/active-directory-identityprotection-flows/141.png "reparation")
3. hello system skickar en utmaning tooyou och du behöver toorespond.
   
    ![Reparation](./media/active-directory-identityprotection-flows/142.png "reparation")

## <a name="risky-sign-in-recovery"></a>Återställning för riskfyllda inloggning
När en administratör har konfigurerat en princip för inloggning risker, meddelas hello påverkade användare när de försöker toosign i. 

**hello riskfyllda inloggning flödet har två steg:** 

1. hello användare informeras om att något annorlunda identifierades om deras inloggning, till exempel loggar in från en ny plats, enhet eller app. 
   
    ![Reparation](./media/active-directory-identityprotection-flows/120.png "reparation")
2. hello användaren är obligatoriska tooprove sin identitet genom att lösa en säkerhetskontrollen. Om hello användare är registrerad för multi-Factor authentication måste de tooround resa säkerhet kod tootheir telefonnummer. Eftersom det är bara en riskfyllda inloggning och komprometterat konto, hello användaren inte har toochange hello lösenord i det här flödet. 
   
    ![Reparation](./media/active-directory-identityprotection-flows/121.png "reparation")

## <a name="risky-sign-in-blocked"></a>Riskfyllda inloggning blockeras
Administratörer kan också välja tooset inloggning Risk princip tooblock användare vid inloggning beroende på hello risknivå. tooget avblockerad, slutanvändare måste kontakta en administratör eller supportavdelning eller de kan försöka logga in från en bekant plats eller en enhet. Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.

![Reparation](./media/active-directory-identityprotection-flows/200.png "reparation")

## <a name="compromised-account-recovery"></a>Komprometterat kontoåterställning
När en säkerhetsprincip för användaren risk har konfigurerats kan användare som uppfyller hello användaren risknivå anges i hello policy (och därför antas komprometteras) måste gå igenom hello användaren röjande recovery flödet innan de kan logga in. 

**hello användaren röjande recovery flödet har tre steg:**

1. hello användare informeras om att deras kontosäkerhet är i fara på grund av misstänkt aktivitet eller läcka ut autentiseringsuppgifter.
   
    ![Reparation](./media/active-directory-identityprotection-flows/101.png "reparation")
2. hello användaren är obligatoriska tooprove sin identitet genom att lösa en säkerhetskontrollen. Om hello användare är registrerad för multifaktorautentisering återställa de själva från komprometteras. De måste tooround resa säkerhet kod tootheir telefonnummer. 
   
   ![Reparation](./media/active-directory-identityprotection-flows/110.png "reparation")
3. Slutligen hello användaren är framtvingad toochange sitt lösenord eftersom någon annan kan ha haft åtkomst tootheir konto. 
   Skärmdumpar av det här upplevelsen är lägre än.
   
   ![Reparation](./media/active-directory-identityprotection-flows/111.png "reparation")

## <a name="compromised-account-blocked"></a>Komprometterat konto blockeras
tooget en användare som har blockerats av en användare risk säkerhetsprincip avblockerad hello användaren måste kontakta en administratör eller supportavdelningen. Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.

![Reparation](./media/active-directory-identityprotection-flows/104.png "reparation")

## <a name="reset-password"></a>Återställa lösenord
Om avslöjade användare blockeras från att logga in kan en administratör generera ett tillfälligt lösenord för dessa. hello användare har toochange sina lösenord under en nästa inloggning.

![Reparation](./media/active-directory-identityprotection-flows/160.png "reparation")

## <a name="see-also"></a>Se även
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md) 

