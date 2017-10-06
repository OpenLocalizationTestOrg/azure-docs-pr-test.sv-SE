---
title: toorequire aaaHow multifaktorautentisering | Microsoft Docs
description: "Lär dig hur toorequire multifaktorautentisering (MFA) för privilegierade identiteter med hello Azure Active Directory Privileged Identity Management-tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c08fad9dc80c09a14a4bd7497da4942fa227c3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-mfa-in-azure-ad-privileged-identity-management"></a>Hur toorequire MFA i Azure AD Privileged Identity Management
Vi rekommenderar att du kräver multifaktorautentisering (MFA) för alla dina administratörer. Detta minskar hello risken för angrepp på grund av tooa komprometteras lösenord.

Du kan kräva att användarna slutföra MFA-kontrollen när de loggar in. Hej blogginlägget [MFA för Office 365 och Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) jämför vad som ingår i Office och Azure-prenumerationer med hello-funktioner som ingår i erbjudandet för hello Microsoft Azure Multi-Factor Authentication.

Du kan också kräva att användarna slutföra MFA-kontrollen när de aktiverar en roll i Azure AD PIM. Det här sättet om hello användaren inte slutföra MFA-kontrollen när de loggat in, kommer de att ange toodo så av PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Kräver MFA i Azure AD Privileged Identity Management
När du hanterar identiteter i PIM som en administratör av Privilegierade roller kan du se aviseringar som rekommenderar MFA för privilegierade konton. Klicka på hello säkerhet avisering i hello PIM instrumentpanel och ett nytt blad öppnas med en lista över hello administratörskonton som ska kräva MFA.  Du kan kräva MFA genom att markera flera roller och sedan klicka på hello **åtgärda** knapp eller klicka på hello ellipserna nästa tooindividual roller och klicka sedan på hello **åtgärda** knappen.

> [!IMPORTANT]
> Just nu är Azure MFA fungerar bara med arbets- eller skolkonton, inte Microsoft-konton (vanligtvis ett personligt konto som har använt toosign i tooMicrosoft services Skype, Xbox, Outlook.com osv.). Därför kan inte alla som använder ett Microsoft-konto vara administratör berättigade eftersom de inte kan använda MFA tooactivate deras roller. Om dessa användare behöver toocontinue hantera arbetsbelastningar som använder ett Microsoft-konto kan du höja dem toopermanent administratörer för tillfället.
> 
> 

Du kan dessutom ändra hello MFA-kravet för en viss roll genom att klicka på den i hello roller avsnittet hello PIM-instrumentpanelen. Klicka på **inställningar** i hello rollen bladet och sedan välja **aktivera** under multifaktorautentisering.

## <a name="how-azure-ad-pim-validates-mfa"></a>Hur Azure AD PIM verifierar MFA
Det finns två alternativ för att verifiera MFA när en användare aktiverar rollen.

hello enklaste alternativet är toorely på Azure MFA för användare som aktiverar en privilegierad roll. toodo, första kontrollen som de användarna som är licensierad, om det behövs och har registrerat dig för Azure MFA. Mer information om hur toodo kan i [komma igång med Azure Multi-Factor Authentication i molnet hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Det rekommenderas, men inte krävs att du konfigurerar Azure AD tooenforce MFA för dessa användare när de loggar in. Det beror på att hello MFA kontroller ska göras av Azure AD PIM sig själv.

Om användare som autentiseras lokalt kan du ha identitetsprovider ansvara för MFA. Om du har konfigurerat AD federationstjänster toorequire smartkortbaserad autentisering innan du använder Azure AD, till exempel [skydda molnresurser med Azure Multi-Factor Authentication och AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) innehåller anvisningar för att konfigurera AD FS toosend anspråk tooAzure AD. När en användare försöker tooactivate en roll, accepterar Azure AD PIM att MFA har redan verifierats för hello användare när den tar emot hello lämpliga anspråk.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

