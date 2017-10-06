---
title: 'Azure AD: SSPR-registrering | Microsoft Docs'
description: "Registrera autentiseringsdata för Azure AD-lösenordet för självbetjäning återställa"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: end-user
ms.openlocfilehash: dfcd0106616218c84d23920b124bed5b202cdd6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="register-for-self-service-password-reset"></a>Registrera för återställning av lösenord med självbetjäning

> [!IMPORTANT]
> **Är du här eftersom du har problem med att logga in?** I så fall är det [här som du ser hur du kan ändra och återställa ditt eget lösenord](active-directory-passwords-update-your-own-password.md).

Du kan återställa ditt lösenord eller låsa upp kontot utan toospeak tooa person som använder lösenordsåterställning via självbetjäning (SSPR) som en slutanvändare. Innan du kan använda den här funktionen har tooregister autentiseringsmetoder eller bekräfta hello fördefinierade autentiseringsmetoder som administratören har fyllts i.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrera eller bekräfta autentiseringsdata med SSPR

1. Öppna hello webbläsaren på din enhet och gå toohello [registreringssidan för återställning av lösenord](http://aka.ms/ssprsetup)
2. Ange ditt användarnamn och lösenord som du har fått av administratören
3. Beroende på hur IT-personalen har konfigurerat saker, en eller flera av hello följande alternativ är tillgängliga för du tooconfigure och verifiera. Administratören kan fylla i vissa av dessa du om de har behörighet toouse hello informationen.
    * Arbetstelefon är bara kan toobe ställts in av administratören
    * Telefon för autentisering ska anges tooanother telefonnummer som du skulle ha åtkomst toolike en mobiltelefon som kan ta emot ett textmeddelande eller samtal.
    * Autentisering e-post ska anges tooan alternativa e-postadress som du kan komma åt utan hello lösenord behöver tooreset.
    * Säkerhetsfrågor ger dig en lista med frågor som administratören har godkänts för du tooanswer. Du kan inte använda hello samma fråga eller besvara mer än en gång.
4. Ange och kontrollera hello information som krävs av administratören. Om mer än ett alternativ är tillgängliga, föreslår vi att du registrerar flera metoder tooprovide flexibilitet när en annan metod är inte tillgänglig (exempel: resa och tooaccess din Arbetstelefon)

    ![Registrera autentiseringsmetoder och klicka på slutför][Register]

5. När du är klar med steg 4 väljer **Slutför** och du kan nu kan toouse Självbetjäning för återställning av lösenord när du behöver tooin hello framtiden.

Om du skriver data i hello telefon för autentisering eller autentisering e-post kan visas det inte i hello global katalog. hello endast personer som kan se dessa data är du och dina administratörer. Du kan se hello svar tooyour säkerhetsfrågor.

Administratörer kan kräva du tooconfirm din autentiseringsmetoder efter en viss tid toomake att du har fortfarande hello lämpliga metoder som är registrerad.

## <a name="common-problems-and-their-solutions"></a>Vanliga problem och lösningar

 Här följer tillfällen vanliga fel och lösningar:

| Fel-fall| Vilka fel ser du?| Lösning |
| --- | --- | --- |
| En ”Kontakta din administratör” sida kommer när du har angett mitt användar-ID | Kontakta administratören <br> <br> Vi har upptäckt att lösenordet till ditt användarkonto inte hanteras av Microsoft. Vi kan därför inte tooautomatically återställa ditt lösenord. <br> <br> Du behöver toocontact din IT-personal för att få hjälp. | Du ser det här meddelandet eftersom din IT-personal hanterar lösenordet i din lokala miljö och tillåter inte tooreset ditt lösenord från den kan inte komma åt ditt kontolänk. <br> <br> tooreset lösenordet, kontakta din IT-personal direkt för hjälp och låter dem vet du vill tooreset ditt lösenord så att de kan du aktivera funktionen för dig.|
| Jag felmeddelandet ”ditt konto har inte aktiverats för lösenordsåterställning” när du har angett mitt användar-ID | Kontot är inte aktiverat för återställning av lösenord <br> <br> Vi beklagar, men din IT-personal inte har lagt upp ditt konto för användning med den här tjänsten. <br> <br> Om du vill kontakta vi en administratör i din organisation tooreset lösenordet åt dig. | Du ser det här meddelandet eftersom din IT-personal inte har aktiverats för återställning av lösenord för din organisation från den kan inte komma åt ditt kontolänk eller har inte licensierats toouse hello-funktionen. <br> <br> tooreset ditt lösenord och klicka på kontakten en administratör länken toosend ett e-tooyour företag har IT-personal och meddela dem om du vill tooreset ditt lösenord så att de kan du aktivera funktionen för dig. |
| Jag felmeddelandet ”Det gick inte att verifiera ditt konto” när du har angett mitt användar-ID | Vi kunde inte verifiera ditt konto <br> <br> Om du vill kontakta vi en administratör i din organisation tooreset lösenordet åt dig. | Du ser det här meddelandet eftersom du är aktiverade för återställning av lösenord, men du inte har registrerat toouse hello-tjänsten. tooregister för lösenord återställa, gå toohttp://aka.ms/ssprsetup när du har återupprättats tooyour konto. <br> <br> tooreset ditt lösenord och klicka på kontakten en administratör länken toosend ett e-tooyour företags IT-personal. |

## <a name="next-steps"></a>Nästa steg

* [Hur toochange med självbetjäning lösenord för återställning av lösenord](active-directory-passwords-update-your-own-password.md)
* [Registreringssida för lösenordsåterställning](http://aka.ms/ssprsetup)
* [Portal för lösenordsåterställning](https://passwordreset.microsoftonline.com/)
* [Det går inte att logga in tooyour Microsoft-konto](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registreringssidan för lösenordsåterställning visar registrerade metoder och knappen Slutför"

