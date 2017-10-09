---
title: 'Azure Active Directory B2C: Multifaktorautentisering | Microsoft Docs'
description: Hur tooenable Multifaktorautentisering i konsumentinriktade program skyddas av Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 29beb1e6ab5d8ab5a65f9c5c068d9e71c60418dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Aktivera Multi-Factor Authentication i dina konsumentinriktade program
Azure Active Directory (AD Azure) B2C integreras direkt med [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) så att du kan lägga till ett andra lager av säkerhet toosign upp och logga in i dina konsumentinriktade program. Och du kan göra detta utan att skriva en enda rad kod. Vi stöder för närvarande telefonsamtal och SMS-verifiering. Du kan aktivera Multifaktorautentisering även om du redan har skapat principer för registrering och inloggning.

> [!NOTE]
> Multifaktorautentisering kan även aktiveras när du skapar principer för registrering och inloggning, inte bara genom att redigera befintliga principer.
> 
> 

Den här funktionen kan hantera scenarier, till exempel hello följande program:

* Du behöver Multifaktorautentisering tooaccess ett program, men du behöver den tooaccess en annan. Till exempel hello konsumenten kan logga in på ett insurance program automatiskt med ett sociala eller lokala konto men måste kontrollera hello telefonnummer innan åtkomst till hello hem insurance program registreras i hello samma katalog.
* Du behöver Multifaktorautentisering tooaccess ett program i allmänhet, men du behöver den tooaccess hello känsliga delar i den. Hello konsumenten kan logga in tooa bankwebbplatser program med ett sociala eller lokala konto och kontrollera kontosaldo men hello telefonnummer måste kontrollera innan du försöker utföra en överföring-överföring.

## <a name="modify-your-sign-up-policy-tooenable-multi-factor-authentication"></a>Ändra din registreringsprincipen tooenable Multifaktorautentisering
1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicka på **Registreringsprinciper**.
3. Klicka på principen för registrering (till exempel ”B2C_1_SiUp”)-tooopen den.
4. Klicka på **multifaktorautentisering** och aktivera hello **tillstånd** för**på**. Klicka på **OK**.
5. Klicka på **spara** hello överst i hello-bladet.

Du kan använda funktionen för hello ”kör nu” på hello princip tooverify hello användarfunktioner. Bekräfta hello följande:

Ett konsumentkonto skapas i katalogen innan hello Multifaktorautentisering steg sker. Hello steget uppmanas hello konsumenten tooprovide sina telefonnummer och verifiera den. Om verifieringen lyckas hello telefonnummer är ansluten toohello konsumentkonto för senare användning. Även om hello konsumenten avbryter eller minskar, kan han eller hon bli ombedd tooverify ett telefonnummer igen under hello nästa inloggning (med Multi-Factor Authentication har aktiverats).

## <a name="modify-your-sign-in-policy-tooenable-multi-factor-authentication"></a>Ändra din inloggningsprincip tooenable Multifaktorautentisering
1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicka på **inloggning principer**.
3. Klicka på din inloggningsprincip (till exempel ”B2C_1_SiIn”) tooopen den. Klicka på **redigera** hello överst i hello-bladet.
4. Klicka på **multifaktorautentisering** och aktivera hello **tillstånd** för**på**. Klicka på **OK**.
5. Klicka på **spara** hello överst i hello-bladet.

Du kan använda funktionen för hello ”kör nu” på hello princip tooverify hello användarfunktioner. Bekräfta hello följande:

När hello konsumenten loggar in (med ett sociala eller lokala konto) om en verifierad telefonnummer bifogas toohello konsumentkonto kan han eller hon uppmanas tooverify den. Om utan telefonnummer är ansluten blir ombedd tooprovide en hello konsumenten och verifiera den. På lyckad verifiering hello telefonnummer är ansluten toohello konsumentkonto för senare användning.

## <a name="multi-factor-authentication-on-other-policies"></a>Multifaktorautentisering på andra principer
Enligt beskrivningen för registrering och inloggning i principer som ovan, är det också möjligt tooenable multifaktorautentisering för registrering eller inloggning principer och lösenord för återställning av principer. Den blir tillgänglig snart på profilen ändra principer.

