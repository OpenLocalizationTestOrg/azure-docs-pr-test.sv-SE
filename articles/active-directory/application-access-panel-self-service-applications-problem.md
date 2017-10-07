---
title: "aaaProblem med självbetjäning programåtkomst | Microsoft Docs"
description: "Felsöka problem relaterade tooself-tjänstprogrammet åtkomst"
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
ms.openlocfilehash: 2487be1df191a4e7fd0bcc0ebbe4ea62fae0fd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-using-self-service-application-access"></a>Problemet med hjälp av självbetjäning programåtkomst

Självbetjäning programåtkomst är ett bra sätt tooallow användare tooself-identifiera program, eventuellt tillåter hello business tooapprove toothose program. Du kan tillåta hello business grupp toomanage hello autentiseringsuppgifter tilldelade toothose användare för lösenord enkel inloggning på program direkt från deras åtkomst paneler.

Innan användarna kan själva identifiera program från deras åtkomstpanelen, behöver du tooenable **självbetjäning programåtkomst** tooany program som du vill tooallow användare tooself-identifiera och begära åtkomst till.

## <a name="general-issues-toocheck-first"></a>Allmänna problem toocheck först

-   Kontrollera att självbetjäning programåtkomst är korrekt konfigurerad. Se ”hur tooconfigure självbetjäning program åt”.

-   Kontrollera att hello användare eller grupp har aktiverat toorequest självbetjäning programåtkomst.

-   Kontrollera att hello användare besöker hello rätt plats för självbetjäning programåtkomst. användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat självbetjäning åtkomst.

-   Om självbetjäning programåtkomst konfigurerades har nyligen, försök toosign in och ut i hello användarens åtkomstpanelen efter några minuter toosee om hello självbetjäning ändringar har visats.

## <a name="how-tooconfigure-self-service-application-access"></a>Hur tooconfigure självbetjäning program åt

tooenable självbetjäning åtkomst tooan program, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooenable självbetjäning toofrom hello åtkomstlista.

7.  När programmet hello läses in klickar du på **självbetjäning** från hello programmet vänstra navigeringsmenyn.

8.  tooenable självbetjäning programåtkomst för det här programmet stänga hello **toorequest åtkomst toothis program för användarna?** växla för**Ja.**

9.  Klicka sedan tooselect hello grupp toowhich användare som begär åtkomst toothis program bör läggas på hello selector nästa toohello etikett **toowhich grupp ska tilldelas användare läggas?** och välja en grupp.

10. **Valfritt:** om du vill toorequire ett företag godkännande innan användarna får åtkomst måste du ange hello **kräver godkännande innan du beviljar åtkomst toothis program?** växla för**Ja**.

11. **Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill tooallow dessa företag godkännare toospecify hello lösenord som skickas toothis program för godkända användare måste ange hello **Tillåt godkännare tooset användarens lösenord för det här programmet?**  växla för**Ja**.

12. **Valfritt:** toospecify hello business godkännare som tooapprove åtkomst toothis program tillåts på hello selector nästa toohello etikett **vem som får tooapprove åtkomst toothis program?** tooselect in too10 enskilda företag godkännare.

 >[!NOTE]
 > Grupper stöds inte.
 >
 >

13. **Valfritt:** **för program som exponera roller**, om du inte vill tooassign godkända Självbetjäningsanvändare tooa roll, klicka på nästa hello selector-toohello **toowhich roll ska tilldelas användare i den här programmet?**  tooselect hello rollen toowhich dessa användare ska tilldelas.

14. Klicka på hello **spara** knappen hello överst i hello bladet toofinish.

När du har slutfört självbetjäning programkonfigurationen användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat Självbetjäning åtkomst. Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/). Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst tooan program som kräver godkännande. 

Dessa godkännanden stöd för godkännande endast arbetsflöden, vilket innebär att alla godkännare kan godkänna åtkomst toohello program om du anger flera godkännare.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Om felsökningen inte löser problemet hello 

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Konfigurera Azure Active Directory för grupphantering via självbetjäning](active-directory-accessmanagement-self-service-group-management.md)
