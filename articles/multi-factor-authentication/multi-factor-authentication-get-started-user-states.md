---
title: "Microsoft Azure Multi-Factor Authentication användaren tillstånd"
description: "Mer information om användarens tillstånd i Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: ad8d531d633eb65fe90404fdab0499b8e5332db6
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Så här kräver tvåstegsverifiering för en användare eller grupp

Du kan göra något av två metoder för att kräva tvåstegsverifiering. Det första alternativet är att aktivera varje användare för Azure Multi-Factor Authentication (MFA). När användarna har aktiverats individuellt kan de utföra tvåstegsverifiering varje gång de loggar in (med vissa undantag, till exempel när de loggar in från tillförlitliga IP-adresser eller när den _sparas enheter_ funktionen är aktiverad). Det andra alternativet är att skapa en princip för villkorlig åtkomst som kräver tvåstegsverifiering vissa villkor.

>[!TIP] 
>Välj något av dessa metoder kräver tvåstegsverifiering, inte båda. Aktivera en användare för Azure Multi-Factor Authentication åsidosätter eventuella principer för villkorlig åtkomst.

## <a name="which-option-is-right-for-you"></a>Vilket alternativ som är rätt för dig?

**Aktivera Azure Multi-Factor Authentication genom att ändra användartillstånd** är den traditionella metoden för att kräva tvåstegsverifiering. Det fungerar för både Azure MFA i molnet och Azure MFA-Server. Alla användare som du aktiverar utföra tvåstegsverifiering varje gång de loggar in. Aktivera en användare åsidosätter eventuella principer för villkorlig åtkomst som kan påverka den användaren. 

**Aktivera Azure Multi-Factor Authentication med en princip för villkorlig åtkomst** är ett mer flexibelt sätt för att kräva tvåstegsverifiering. Den fungerar bara för Azure MFA i molnet, men och _villkorlig åtkomst_ är en [betald funktion i Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Du kan skapa principer för villkorlig åtkomst som gäller för grupper som enskilda användare. Hög risk grupper kan få fler begränsningar än låg risk grupper eller tvåstegsverifiering kan krävs endast för hög risk molnappar och hoppades över för de låg risk. 

Båda alternativen be användarna att registrera dig för Azure Multi-Factor Authentication första gången de loggar in när kraven har aktiverat. Båda alternativen också fungera med den konfigurerbara [Azure Multi-Factor Authentication-inställningar](multi-factor-authentication-whats-next.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Aktivera Azure MFA genom att ändra användarstatus

Användarkonton i Azure Multi-Factor Authentication har följande tre skilda lägen:

| Status | Beskrivning | Icke-webbläsarappar som påverkas | Webbläsarbaserade appar som påverkas | Modern autentisering som påverkas |
|:---:|:---:|:---:|:--:|:--:|
| Disabled |Standardläget för en ny användare som inte har registrerats i Azure MFA. |Nej |Nej |Nej |
| Enabled |Användaren har registrerats i Azure MFA, men har inte registrerats. De får en uppmaning om att registrera sig nästa gång de loggar in. |Nej.  De fortsätter att fungera tills registreringen har slutförts. | Ja. När sessionen upphör att gälla krävs Azure MFA-registrering.| Ja. När den åtkomst-token upphör att gälla krävs Azure MFA-registrering. |
| Enforced |Användaren har registrerats och har slutfört registreringsprocessen för Azure MFA. |Ja.  Appar kräver applösenord. |Ja. Azure MFA krävs vid inloggning. | Ja. Azure MFA krävs vid inloggning. |

Tillstånd för en användare visar om en administratör har registrerats dem i Azure MFA och om de slutfört registreringsprocessen.

Alla användare först *inaktiverade*. När du registrerar användare i Azure MFA deras tillstånd ändras till *aktiverad*. När aktiverade användare logga in och slutföra registreringen, deras tillstånd ändras till *tvingande*.  

### <a name="view-the-status-for-a-user"></a>Visa status för en användare

Använd följande steg för att komma åt sidan där du kan visa och hantera användartillstånd:

1. Logga in på [Azure Portal](https://portal.azure.com) som administratör.
2. Gå till **Azure Active Directory** > **användare och grupper** > **alla användare**.
3. Välj **Multifaktorautentisering**.
   ![Välj Multifaktorautentisering](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. En ny sida som visar användartillståndsdata som öppnas.
   ![multifaktorautentisering användarstatus – skärmbild](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Ändra status för en användare

1. Använd ovanstående steg för att få med Azure Multi-Factor Authentication **användare** sidan.
2. Sök efter den användare som du vill aktivera för Azure MFA. Du kan behöva ändra på visningen längst upp. 
   ![Hitta användare – skärmbild](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Markera rutan bredvid användarens namn.
4. Till höger under **snabbsteg**, Välj **aktivera** eller **inaktivera**.
   ![Aktivera valda användare – skärmbild](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Aktiverad* användare automatiskt växlas till *tvingande* när de registrerar för Azure MFA. Gör inte manuellt ändra användarens tillstånd till *tvingande*. 

5. Bekräfta dina val i popup-fönstret som öppnas. 

När du aktiverar användare, meddela dem via e-post. Be dem att de kommer att behöva registrera sig nästa gång de loggar in. Om din organisation använder icke-webbläsarbaserade appar som inte stöder modern autentisering, måste de också skapa applösenord används. Du kan även inkludera en länk till den [Azure MFA slutanvändarens guiden](./end-user/multi-factor-authentication-end-user.md) hjälper dem att komma igång.

### <a name="use-powershell"></a>Använd PowerShell
Så här ändrar du användartillstånd med hjälp av [Azure AD PowerShell](/powershell/azure/overview), ändra `$st.State`. Det finns tre möjliga tillstånd:

* Enabled
* Enforced
* Disabled  

Flytta inte användare direkt till den *tvingande* tillstånd. Om du gör det, icke-webbläsarbaserade appar upphör att fungera eftersom användaren inte har gått igenom Azure MFA-registrering och hämta en [applösenord](multi-factor-authentication-whats-next.md#app-passwords). 

Med hjälp av PowerShell är ett bra alternativ när du behöver samtidigt som användarna. Skapa ett PowerShell-skript som körs i en slinga genom en lista över användare och gör det möjligt för dem:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Följande skript är ett exempel:

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

_Villkorlig åtkomst_ är en betald funktion i Azure Active Directory med många konfigurationsalternativ. Dessa steg igenom ett sätt att skapa en princip. Mer information finns i avsnittet om [villkorlig åtkomst i Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Logga in på [Azure Portal](https://portal.azure.com) som administratör.
2. Gå till **Azure Active Directory** > **villkorlig åtkomst**.
3. Välj **ny princip**.
4. Under **tilldelningar**väljer **användare och grupper**. Använd den **inkludera** och **undanta** flikar för att ange vilka användare och grupper principen hanterar.
5. Under **tilldelningar**väljer **Molnappar**. Välja att inkludera **alla molnappar**.
6. Under **åtkomstkontroller**väljer **bevilja**. Välj **kräver Multi-Factor authentication**.
7. Aktivera **aktiverar principen** till **på**, och välj sedan **spara**.

Andra alternativ i principen för villkorlig åtkomst ger dig möjligheten att ange exakt när tvåstegsverifiering krävs. Exempelvis kan du en princip, till exempel den här: när leverantörer försöker komma åt vår app inköp från ej betrodda nätverk på enheter som inte är ansluten till domänen, kräver tvåstegsverifiering. 

## <a name="next-steps"></a>Nästa steg

- Få tips den [bästa praxis för villkorlig åtkomst](../active-directory/active-directory-conditional-access-best-practices.md).

- Hantera Azure Multi-Factor Authentication-inställningar för [användarna och deras enheter](multi-factor-authentication-manage-users-and-devices.md).
