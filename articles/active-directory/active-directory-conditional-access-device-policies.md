---
title: "aaaAzure Active Directory device principer för villkorlig åtkomst för Office 365-tjänster | Microsoft Docs"
description: "Lär dig mer om hur tooprovision villkorlig åtkomst enheten principer toohelp ser företagsresurser säkrare, samtidigt som användaren tooservices för efterlevnad och åtkomst."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 38229a8d9766efbf0e6b17875b3018011c6b4ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Active Directory device principer för villkorlig åtkomst för Office 365-tjänster

Villkorlig åtkomst kräver flera delar toowork. Det innebär att en Multi-Factor autentiserade användare, en autentiserad enhet och en kompatibel enhet, bland annat. I den här artikeln fokuserar vi i första hand på enhetsbaserad villkor som din organisation kan använda toohelp du styr åtkomst till tooOffice 365-tjänster. 

Företagsanvändare vill tooaccess Office 365-tjänster som Exchange och SharePoint Online på arbetet eller skolan från sina personliga enheter. Vill du hello åtkomst toobe säker. Du kan etablera villkorlig åtkomst principer toohelp kontrollera företagets enhetsresurser säkrare när bevilja åtkomst tooservices för användare som använder kompatibla enheter. Du kan ange villkorlig åtkomst principer tooOffice 365 i hello Microsoft Intune-portalen för villkorlig åtkomst.

Azure Active Directory (AD Azure) tillämpar villkorlig åtkomst principer toohelp säker åtkomst tooOffice 365-tjänster. Du kan skapa en princip för villkorlig åtkomst som blockerar en användare som använder en icke-kompatibla enheter från att komma åt en Office 365-tjänst. hello användare måste följa toohello företagets principer för enheten innan tjänsten för dataåtkomst toohello beviljas. Alternativt kan skapa du en princip som kräver att användare tooenroll sina enheter toogain åtkomst tooan Office 365-tjänsten. Principer kan vara tillämpade tooall användare i en organisation eller begränsad tooa några målgrupper. Du kan lägga till flera mål grupper tooa princip över tid.

En förutsättning för att genomdriva principer för enheter är att användare registrerar sina enheter med hello Azure AD device registration service. Du kan välja tooturn på Multi-Factor authentication för enheter som registreras med hello enhetsregistreringstjänsten för Azure AD. Multifaktorautentisering rekommenderas för hello enhetsregistreringstjänsten för Azure Active Directory. När multifaktorautentisering aktiveras kan användare registrerar sina enheter med hello enhetsregistreringstjänsten för Azure AD att dirigeras till andra faktor-autentisering.

## <a name="how-does-a-conditional-access-policy-work"></a>Hur fungerar en princip för villkorlig åtkomst?

När en användare begär åtkomst tooan Office 365-tjänst från en stödd enhetsplattform, autentiserar Azure AD hello användar- och hello. Azure AD beviljar åtkomst toohello service endast om användaren hello överensstämmer toohello princip för hello-tjänsten. På enheter som inte är registrerade instruktioner om hur tooenroll och blir kompatibla tooaccess företagets Office 365-tjänster. Användare på iOS och Android-enheter är nödvändiga tooenroll sina enheter med hjälp av hello företagsportal program. När en användare registrerar en enhet, hello enheten är registrerad med Azure AD och den har registrerats för hantering av enheter och efterlevnad. Du måste använda registreringstjänsten för hello Azure AD-enheter med Microsoft Intune för hantering av mobila enheter för Office 365-tjänster. Registrering av enheter krävs för användare tooaccess Office 365-tjänster när enhetsprinciper tillämpas.

När en användare har registrerar en enhet, hello enheten blir betrodd. Azure AD ger hello autentiserade användare åtkomst för enkel inloggning toocompany program. Azure AD tillämpar en villkorlig åtkomst princip toogrant tooa tjänsten för dataåtkomst inte bara hello hello användare begär åtkomst, men varje gång hello användare förnyar en begäran om åtkomst. hello användare nekas åtkomst tooservices när inloggningsuppgifter ändras, hello enhet tappas bort eller blir stulen eller hello villkoren i hello princip är inte uppfyllda på hello tid för begäran om förnyelse.

## <a name="deployment-considerations"></a>Distributionsöverväganden

Du måste använda hello Azure AD device registrering service tooregister enheter.

När lokala användare är om toobe autentiseras, Active Directory Federation Services (AD FS) (version 1.0 och senare) krävs. Multifaktorautentisering för anslutning till arbetsplatsen misslyckas när hello identitetsleverantör inte kan utföra multifaktorautentisering. Du kan till exempel använda multifaktorautentisering med AD FS 2.0. Se till att hello lokala AD FS fungerar med multifaktorautentisering och att en giltig multifaktorautentisering metod är på plats innan du aktiverar multifaktorautentisering hello Azure AD för registreringstjänsten för enheten. Till exempel har AD FS i Windows Server 2012 R2 funktioner för multifaktorautentisering. Du måste ställa en ytterligare giltig autentisering (multifaktorautentisering)-metod på hello AD FS-servern innan du aktiverar multifaktorautentisering hello Azure AD för registreringstjänsten för enheten. Mer information om metoder stöds multifaktorautentisering i AD FS finns [konfigurera ytterligare autentiseringsmetoder för AD FS](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Nästa steg

*   Svaren toocommon frågor finns i [villkorlig åtkomst för Azure Active Directory vanliga frågor och svar](active-directory-conditional-faqs.md).
