---
title: "aaaProblems inloggning tooan Azure AD-galleriet program konfigurerade för federerad enkel inloggning | Microsoft Docs"
description: "Hur tootroubleshoot problem med Azure AD-galleriet program som har konfigurerats för lösenord för enkel inloggning"
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
ms.openlocfilehash: 7ba28765e1d1f13025d740790a2f87654ae0daed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a>Problem med att logga i tooan Galleri för Azure AD-program som konfigurerats för federerad enkel inloggning

Hej åtkomstpanelen är en webbaserad portal som aktiverar en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program att hello Azure AD-administratör har gett dem åtkomst till. En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen. Hej åtkomstpanelen skiljer sig från hello Azure-portalen och kräver inte användare toohave en Azure-prenumeration.

toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Möte Webbläsarkrav för hello åtkomstpanelen

Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS. toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

För lösenordsbaserad SSO kan hello användarens webbläsare vara:

-   Internet Explorer 8, 9, 10, 11 - på Windows 7 eller senare

-   Chrome - på Windows 7 eller senare, och i MacOS X eller senare

-   Firefox 26.0 eller senare - på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare

>[!NOTE]
>hello lösenordsbaserade SSO tillägget blir tillgängliga för kvalificerade i Windows 10 när webbläsartillägg blir stöd för kant.
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hur tooinstall hello åtkomst panelen webbläsartillägg

tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:

1.  Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.

2.  Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.

3.  I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.

4.  Baserat på din webbläsare vara du riktad toohello länken. **Lägg till** hello tillägget tooyour webbläsare.

5.  Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.

6.  När den har installerats, **starta om** webbläsarsessionen.

7.  Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program

Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:

-   [Tillägget för Chrome åtkomst panelen](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Tillägget för Firefox åtkomst panelen](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Konfigurera en grupprincip för Internet Explorer

Du kan konfigurera en grupprincip som gör att du tooremotely installera hello åtkomstpanelen tillägg för Internet Explorer på användarnas datorer.

omfattar hello krav:

-   Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer tooyour domän.

-   Du måste ha hello ”redigera inställningar för” behörighet tooedit hello grupprincipobjektet (GPO). Som standard medlemmar i hello följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip. [Läs mer](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Följ hello kursen [hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) steg-för-steg-instruktioner för hur tooconfigure hello Grupprincip och distribuera den toousers.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Felsöka hello åtkomstpanelen i Internet Explorer

Följ hello [Felsök hello Access-tillägg för Internet Explorer på panelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide för åtkomst till en diagnostik och steg-för-steg-instruktioner om hur du konfigurerar hello-tillägg för Internet Explorer.

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett program från hello Azure AD-galleriet](#_Add_an_application)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

-   [Tilldela användare toohello program](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Lägga till ett program från hello Azure AD-galleriet

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.

6.  I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.

7.  Välj hello-program som du vill använda tooconfigure för enkel inloggning.

8.  Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.

9.  Klicka på **Lägg till** knappen tooadd hello program.

Efter en kort period vara kan toosee hello programmets konfiguration bladet.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurera hello program för lösenord för enkel inloggning

tooconfigure enkel inloggning för ett program gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  [Tilldela användare toohello program](#_How_to_assign).

10. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

### <a name="assign-users-toohello-application"></a>Tilldela användare toohello program

tooassign en eller flera användare tooan programmet direkt, gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooassign en toofrom hello-användarlistan.

7.  När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.

9.  Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.

10. Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.

11. Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**. Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.

12. **Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.

13. När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.

14. **Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.

15. Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.

Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.

## <a name="if-these-troubleshoot-steps-dont-resolve-hello-issue"></a>Om felsökning av dessa åtgärder inte kan lösa hello problemet 
Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
