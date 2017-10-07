---
title: "aaaProblem installerar hello programmet åtkomst panelen webbläsartillägget | Microsoft Docs"
description: "Hur toofix vanliga fel påträffades när du installerar hello åtkomst panelen webbläsartillägg"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 5f750d12c5f9b405ec4f81596d5cc5e0a48f9a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-access-panel-browser-extension"></a>Problem med att installera hello programmet åtkomst panelen webbläsartillägg

Hej åtkomstpanelen är en webbaserad portal som aktiverar en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program att hello Azure AD-administratör har gett dem åtkomst till. En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen. Hej åtkomstpanelen skiljer sig från hello Azure-portalen och kräver inte användare toohave en Azure-prenumeration.

toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Möte Webbläsarkrav för hello åtkomstpanelen

Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS. toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

För lösenordsbaserad SSO kan hello användarens webbläsare vara:

-   Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare

-   Kanten på Windows 10 årsdagar Edition eller senare 

-   Chrome--På Windows 7 eller senare, och i MacOS X eller senare

-   Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hur tooinstall hello åtkomst panelen webbläsartillägg

tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:

1.  Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.

2.  Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.

3.  I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.

4.  Baserat på din webbläsare vara du riktad toohello länken. **Lägg till** hello tillägget tooyour webbläsare.

5.  Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.

6.  När den har installerats, **starta om** webbläsarsessionen.

7.  Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program

Du kan också hämta hello-tillägget för Chrome och kanten på hello Direktlänkar nedan:

-   [Tillägget för Chrome åtkomst panelen](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Tillägget för Microsoft Edge åtkomst panelen](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Konfigurera en grupprincip för Internet Explorer

Du kan konfigurera en grupprincip som gör att du tooremotely installera hello åtkomstpanelen tillägg för Internet Explorer på användarnas datorer.

omfattar hello krav:

-   Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer tooyour domän.

-   Du måste ha hello ”redigera inställningar för” behörighet tooedit hello grupprincipobjektet (GPO). Som standard medlemmar i hello följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip. [Läs mer](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Följ hello kursen [hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md) steg-för-steg-instruktioner för hur tooconfigure hello Grupprincip och distribuera den toousers.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Felsöka hello åtkomstpanelen i Internet Explorer

Följ hello [Felsök hello Access-tillägg för Internet Explorer på panelen](active-directory-saas-ie-troubleshooting.md) guide för åtkomst till en diagnostik och steg-för-steg-instruktioner om hur du konfigurerar hello-tillägg för Internet Explorer.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Om felsökningen inte löser problemet hello

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
