---
title: "Azure AD Connect: Direkt autentisering - uppgradera förhandsversionen autentisering agenter | Microsoft Docs"
description: "Den här artikeln beskriver hur tooupgrade konfigurationen av Azure Active Directory (AD Azure) direkt-autentisering."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 1695326b182278944e9f1584da72b18862b6ef27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory direkt-autentisering: Uppgradera förhandsversionen autentisering agenter

## <a name="overview"></a>Översikt

Den här artikeln är för kunder som använder Azure AD direkt autentisering via preview. Vi nyligen uppgraderade (och byta namn) hello autentiseringsagent programvara. Du måste too_manually_ uppgradera förhandsversionen autentisering agenter som installerats på lokala servrar. Den här manuell uppgradering är en engångsåtgärd endast. Alla framtida uppdateringar tooAuthentication agenter är automatisk. hello orsaker tooupgrade är följande:

- hello förhandsversioner av autentisering agenter får inte några ytterligare säkerhet eller felkorrigeringar.
-   hello förhandsversioner av autentisering agenter kan inte installeras på flera servrar för hög tillgänglighet.

## <a name="check-versions-of-your-authentication-agents"></a>Kontrollera versioner av dina agenter för autentisering

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Steg 1: Kontrollera om autentisering-agenter är installerade

Följ dessa steg toocheck där autentisering-agenter är installerade:

1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med hello globala administratörsbehörigheter för din klient.
2. Välj **Azure Active Directory** på hello vänstra navigeringsfönstret.
3. Välj **Azure AD Connect**. 
4. Välj **direkt autentisering**. Det här bladet visar hello servrar där autentisering-agenter är installerade.

![Azure Active Directory Administrationscenter - bladet direkt-autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-hello-versions-of-your-authentication-agents"></a>Steg 2: Kontrollera hello versioner av dina agenter för autentisering

toocheck hello versioner av dina agenter för autentisering, på varje server som anges i föregående steg, hello följa instruktionerna nedan:

1. Gå för**Kontrollpanelen -> program -> program och funktioner** hello på lokal server.
2. Om det finns en post för ”**ansluta autentiseringsagent för Microsoft Azure AD**”, behöver du inte tootake alla åtgärder på den här servern.
3. Om det finns en post för ”**Microsoft Azure AD Application Proxy Connector**”, version 1.5.132.0 eller tidigare, så du behöver toomanually uppgraderingen på den här servern.

![Förhandsversionen av autentiseringsagent](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-toofollow-before-starting-hello-upgrade"></a>Bästa praxis toofollow innan du startar uppgraderingen hello

Se till att du har hello följande objekt på plats innan du uppgraderar:

1. **Skapa endast molnbaserad globala administratörskonto**: inte uppgraderar utan att behöva en endast molnbaserad Global administratör konto toouse i en nödsituation där direkt autentisering agenterna inte fungerar korrekt. Lär dig mer om [att lägga till ett globalt administratörskonto endast molnbaserad](../active-directory-users-create-azure-portal.md). Gör det här steget är mycket viktigt och garanterar att du inte blir utelåst från din klient.
2.  **Säkerställa hög tillgänglighet**: Om du inte har slutförts ännu, installera en andra fristående autentiseringsagent tooprovide hög tillgänglighet för inloggning begäranden med dessa [instruktioner](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-hello-authentication-agent-on-your-azure-ad-connect-server"></a>Uppgradera hello autentiseringsagent på Azure AD Connect-server

Du måste uppgradera Azure AD Connect innan du uppgraderar hello autentiseringsagent på hello samma server. Följ dessa steg på din primära och mellanlagring av Azure AD Connect-servrar:

1. **Uppgradera Azure AD Connect**: följer detta [artikel](./active-directory-aadconnect-upgrade-previous-version.md) och uppgradera toohello senaste Azure AD Connect-version.
2. **Avinstallera hello förhandsversionen av hello autentiseringsagent**: hämta [detta PowerShell-skript](https://aka.ms/rmpreviewagent) och kör som administratör på hello-servern.
3. **Hämta hello senaste versionen av hello autentiseringsagent (versioner 1.5.193.0 eller senare)**: Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med globala administratörsbehörigheter för din klient. Välj **Azure Active Directory -> Azure AD Connect -> direkt autentisering hämta agent ->**. Acceptera användarvillkoren hello och hämta hello senaste versionen av hello autentiseringsagent.
4. **Installera hello senaste versionen av hello autentiseringsagent**: kör hello körbara hämtade i steg3. Ange din klient Global administratör autentiseringsuppgifter när du tillfrågas.
5. **Kontrollera att senaste hello-versionen har installerats**: enligt innan du går för**Kontrollpanelen -> program -> program och funktioner** och kontrollera att det finns en post för ”**Microsoft Azure AD Connect Autentiseringsagent**”.

>[!NOTE]
>Om du markerar hello direkt autentisering bladet på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) när du har slutfört föregående steg hello ser du två autentiseringsagent poster per server – en post som visar hello Autentiseringsagent som **Active** och hello andra som **inaktiv**. Detta är _förväntade_. Hej **inaktiv** post automatiskt bort efter några dagar.

## <a name="upgrading-hello-authentication-agent-on-other-servers"></a>Uppgradera hello autentiseringsagent på andra servrar

Följ dessa steg tooupgrade autentisering agenter på andra servrar (där Azure AD Connect är inte installerat):

1. **Avinstallera hello förhandsversionen av hello autentiseringsagent**: hämta [detta PowerShell-skript](https://aka.ms/rmpreviewagent) och kör som administratör på hello-servern.
2. **Hämta hello senaste versionen av hello autentiseringsagent (versioner 1.5.193.0 eller senare)**: Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med globala administratörsbehörigheter för din klient. Välj **Azure Active Directory -> Azure AD Connect -> direkt autentisering hämta agent ->**. Acceptera användarvillkoren hello och hämta hello senaste versionen.
3. **Installera hello senaste versionen av hello autentiseringsagent**: kör hello körbara hämtade i steg 2. Ange din klient Global administratör autentiseringsuppgifter när du tillfrågas.
4. **Kontrollera att senaste hello-versionen har installerats**: enligt innan du går för**Kontrollpanelen -> program -> program och funktioner** och kontrollera att det finns en post med namnet **Microsoft Azure AD Connect Autentiseringsagent**.

>[!NOTE]
>Om du markerar hello direkt autentisering bladet på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) när du har slutfört föregående steg hello ser du två autentiseringsagent poster per server – en post som visar hello Autentiseringsagent som **Active** och hello andra som **inaktiv**. Detta är _förväntade_. Hej **inaktiv** post automatiskt bort efter några dagar.

## <a name="next-steps"></a>Nästa steg
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
