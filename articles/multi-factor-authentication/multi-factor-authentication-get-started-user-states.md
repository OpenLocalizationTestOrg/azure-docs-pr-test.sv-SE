---
title: "aaaMicrosoft Azure Multi-Factor Authentication användaren tillstånd"
description: "Mer information om användarens tillstånd i Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: cf5b977b09d09330b7b3bc668abd79e602d62015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-two-step-verification-for-a-user-or-group"></a>Hur toorequire tvåstegsverifiering för en användare eller grupp

Det finns två tillvägagångssätt för att kräva tvåstegsverifiering. Hej första alternativet är tooenable varje enskild användare för Azure Multi-Factor Authentication (MFA). När användarna har aktiverats individuellt kan utföra de alltid tvåstegsverifiering (med vissa undantag, som när de loggar in från tillförlitliga IP-adresser eller om hello sparas enheter funktionen är aktiverad). hello andra alternativ är tooset in en princip för villkorlig åtkomst som kräver tvåstegsverifiering under vissa förhållanden.

>[!TIP] 
>Välj något av dessa metoder toorequire tvåstegsverifiering, inte båda. Aktivera en användare för Azure MFA åsidosätter eventuella principer för villkorlig åtkomst.

## <a name="which-option-is-right-for-you"></a>Vilket alternativ som är rätt för dig

**Aktivera Azure MFA genom att ändra användartillstånd** är hello traditionell metod för att kräva tvåstegsverifiering. Det fungerar för både Azure MFA i molnet hello och Azure MFA-Server. Alla hello-användare som du aktiverar har hello samma upplevelse, vilket är tooperform tvåstegsverifiering varje gång de loggar in. Aktivera en användare åsidosätter alla principer för villkorlig åtkomst som kan påverka den användaren. 

**Aktivera Azure MFA med en princip för villkorlig åtkomst** är ett mer flexibelt sätt för att kräva tvåstegsverifiering. Den fungerar bara för Azure MFA i hello moln, men och villkorlig åtkomst är en [betald funktion i Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Du kan skapa principer för villkorlig åtkomst som gäller toogroups samt enskilda användare. Hög risk grupper kan få fler begränsningar än låg risk grupper eller tvåstegsverifiering kan krävs endast för hög risk molnappar och hoppades över för de låg risk. 

Båda dessa alternativ uppmanar användare tooregister för Azure Multi-Factor Authentication hello första gången de loggar in när hello krav har aktiverat. Båda alternativen också fungera med hello konfigurerbara [Azure Multi-Factor Authentication-inställningar](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Aktivera Azure MFA genom att ändra användarstatus

Användarkonton i Azure Multi-Factor Authentication har hello följande tre skilda lägen:

| Status | Beskrivning | Icke-webbläsarappar som påverkas |
|:---:|:---:|:---:|
| Disabled |hello standardläget för en ny användare har inte registrerats för Azure Multi-Factor Authentication (MFA). |Nej |
| Enabled |hello användaren har registrerats i Azure MFA, men har inte registrerats. De kommer att tillfrågas tooregister hello nästa gång de loggar in. |Nej.  De fortsätta toowork tills hello registreringen har slutförts. |
| Enforced |hello användaren har registrerats och hello registreringen har slutförts för Azure MFA. |Ja.  Appar kräver applösenord. |

Tillstånd för en användare visar om en administratör har registrerats dem i Azure MFA och om de slutfört hello registreringsprocessen.

Alla användare först *inaktiveras*. När du registrerar användare i Azure MFA, deras tillståndsändringar *aktiverat*. När aktiverade användare logga in och slutföra registreringen för hello, deras tillstånd ändras för*tillämpas*.  

### <a name="view-hello-status-for-a-user"></a>Visa hello status för en användare

Använd hello följande steg tooaccess hello sida där du kan visa och hantera användartillstånd:

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Gå för**Azure Active Directory** > **användare och grupper** > **alla användare**.
3. Välj **Multifaktorautentisering**.
   ![Välj Multifaktorautentisering](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. En ny sida som visar hello användarens tillstånd, öppnas.
   ![multifaktorautentisering användarstatus – skärmbild](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-hello-status-for-a-user"></a>Ändra hello status för en användare

1. Använd hello föregående steg tooget toohello användare multifaktorautentiseringssidan.
2. Hitta hello användare som du vill tooenable för Azure MFA. Du kan behöva toochange hello visa hello överst. 
   ![Hitta användare – skärmbild](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Kontrollera hello rutan nästa tootheir namn.
4. Välj på höger under snabbsteg hello, **aktivera** eller **inaktivera**.
   ![Aktivera valda användare – skärmbild](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Aktiverad* användarna automatiskt växla för*tillämpas* när de registrerar för Azure MFA. Du bör inte ändra hello användaren tillstånd tooenforced manuellt. 

5. Bekräfta dina val i hello popup-fönster som öppnas. 

När du aktiverar användare, bör du meddela dem via e-post. Be dem att de uppmanade att ange tooregister hello nästa gång de loggar in. Även om din organisation använder icke-webbläsarbaserade appar som inte stöder modern autentisering, behöver de toocreate applösenord. Du kan även inkludera en länk tooour [Azure MFA slutanvändarens guiden](./end-user/multi-factor-authentication-end-user.md) toohelp dem komma igång.

### <a name="use-powershell"></a>Använd PowerShell
toochange hello användaren status tillstånd med [Azure AD PowerShell](/powershell/azure/overview), ändra `$st.State`. Det finns tre möjliga tillstånd:

* Enabled
* Enforced
* Disabled  

Flytta inte användare direkt toohello *tvingande* tillstånd. Icke-webbläsarbaserad appar slutar fungera eftersom hello användaren inte har gått igenom MFA-registrering och hämta en [applösenord](multi-factor-authentication-whats-next.md#app-passwords). 

Med hjälp av PowerShell är ett bra alternativ när du behöver toobulk användarna. Skapa ett PowerShell-skript som körs i en slinga genom en lista över användare och gör det möjligt för dem:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Här är ett exempel:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Aktivera Azure MFA med en princip för villkorlig åtkomst

Villkorlig åtkomst är en betald funktion i Azure Active Directory, där många möjliga konfigurationsalternativ. Dessa steg igenom enkelriktade toocreate en princip. Mer information finns i avsnittet om [villkorlig åtkomst i Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Gå för**Azure Active Directory** > **villkorlig åtkomst**.
3. Välj **ny princip**.
4. Under **tilldelningar**väljer **användare och grupper**. Använd hello **inkludera** och **undanta** flikarna toospecify vilka användare och grupper som ska hanteras av hello princip.
5. Under **tilldelningar**väljer **Molnappar**. Välj tooinclude **alla molnappar**.
6. Under **åtkomstkontroller**väljer **bevilja**. Välj **kräver Multi-Factor authentication**.
7. Aktivera **aktiverar principen** för**på** och välj sedan **spara**.

hello kan andra alternativ i hello principen för villkorlig åtkomst du toospecify exakt när tvåstegsverifiering ska krävas. Exempelvis kan du en princip som säger: när leverantörer tooaccess vår inköp app från ej betrodda nätverk på enheter som inte är ansluten till domänen, kräver tvåstegsverifiering. 

## <a name="next-steps"></a>Nästa steg

- Få tips på hello [bästa praxis för villkorlig åtkomst](../active-directory/active-directory-conditional-access-best-practices.md)

- Hantera Multi-Factor Authentication-inställningar för [användarna och deras enheter](multi-factor-authentication-manage-users-and-devices.md)