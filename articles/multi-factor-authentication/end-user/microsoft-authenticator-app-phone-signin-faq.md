---
title: aaaMicrosoft Authenticator phone inloggning - Azure och Microsoft-konton | Microsoft Docs
description: "Använd din telefon toosign i tooyour Microsoft-konto i stället för att ange lösenordet. Den här artikeln innehåller svar på vanliga frågor och svar om den här funktionen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: a4911ff580b3ffa078299ad706d099330b75a2e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Logga in med din telefon inte ditt lösenord

hello Microsoft Authenticator-appen hjälper dig att skydda dina konton genom att utföra tvåstegsverifiering när du anger ditt lösenord. Men visste du att den kan ersätta helt hello lösenordet för ditt personliga Microsoft-konto? 

Den här funktionen är tillgänglig på iOS och Android-enheter och fungerar med personliga Microsoft-konton. 

## <a name="how-it-works"></a>Hur det fungerar

Många av du använda hello Microsoft Authenticator-appen för tvåstegsverifiering när du loggar in tooyour Microsoft-konto. Du anger ditt lösenord och gå sedan toohello app tooeither godkänna ett meddelande eller hämta en Verifieringskod. Med phone inloggning, hoppa över hello lösenord och göra din identitetsverifiering på din telefon. Eftersom är en typ av tvåstegsverifiering inloggning telefon, måste du fortfarande tooprovide något du känner till och en sak som du har tooverify din identitet. hello telefon är fortfarande hello sak som du har och din telefon PIN-kod eller biometriska nyckel är hello sak du känner till. 

## <a name="how-tooget-started"></a>Hur tooget igång

toosign i tooyour personligt Microsoft-konto med din telefon så här: 

1. Aktivera phone inloggningsnamn för ditt konto. 

  - Om du inte hello Microsoft Authenticator-appen ännu, installerar och lägger till ditt personliga Microsoft-konto enligt toohello steg på hello [Microsoft Authenticator sidan](microsoft-authenticator-app-how-to.md). Konton som nyligen tillagda aktiveras automatiskt så att du är bra toogo.

  - Om du redan använder Microsoft Authenticator för tvåstegsverifiering, Välj ditt konto från startsidan för hello app och markera **aktivera inloggning phone** hello nedrullningsbara menyn.

  >[!NOTE] 
  >tooprotect ditt konto, vi kräver att en PIN-kod eller biometriska lås på enheten. Om du behåller din telefon olåst öppnas hello appen en begäran som ber dig tooset upp ett lås innan du aktiverar inloggning telefon. 

3. De flesta sidor där du normalt anger lösenordet för ditt Microsoft-konto har en länk som säger **använda en app i stället**. Välj den här länken toosign in med din telefon. 

4. Microsoft skickar en avisering tooyour telefon. Godkänna hello-meddelande toosign i tooyour-konto.   

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR 

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Hur loggar in med min telefon säkrare än att skriva ett lösenord?  

De flesta logga idag i tooweb platser och appar med ett användarnamn och lösenord.  Tyvärr är lösenord ofta förlorade, stulna eller gissa för hackare. När du ställer in hello Microsoft Authenticator app toosign i Skapa vi en nyckel på telefonen som kan låsa upp ditt konto. Vi skydda den här nyckeln med hello PIN-kod eller biometriska att du redan använder på din telefon.  När du loggar in med din telefon är den här nyckeln används tooprove din identitet på ett säkert sätt med två faktorer – hello phone sig själv och din möjlighet toounlock den. 

hello-nyckel som används är liknande toohello nycklarna som används i Windows Hello och hello FIDO Alliance UAF specifikationer. Din bio-data används tooprotect hello nyckel lokalt, och aldrig skickas till, eller lagras i hello molnet. 
 
### <a name="where-can-i-use-my-phone-tooreplace-my-password-and-where-would-i-still-need-hello-password"></a>Var kan jag använda min telefon tooreplace mitt lösenord och där behöver jag fortfarande hello lösenord?  

Idag fungerar hello inloggning telefonfunktionen bara med webbprogram och tjänster som tillhandahålls av personliga Microsoft-konton, iOS eller Android-appar som använder ett personligt microsoftkonto och appar på Windows 10 som använder ett personligt microsoftkonto. När du loggar in tooone på dessa webbplatser eller program på hello sida där du vanligtvis ange lösenordet för det finns en länk som säger **använda en app i stället**. 

Inloggning telefon måste använda toounlock en Windows-dator, XBOX eller fjärrskrivbord versioner av Microsoft-appar, till exempel Office-appar just nu. 
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Ersätter tvåstegsverifiering? Bör jag inaktivera den?   

Ibland. Vi arbetar med expanderande hello omfattning inloggning telefon, men nu finns fortfarande platser i hello Microsoft ekosystem som inte stöder den. På dessa platser använder vi fortfarande tvåstegsverifiering för säker inloggning. Därför är Nej, bör du inaktivera tvåstegsverifiering för ditt konto. 
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-tooapprove-two-notifications"></a>OK, om jag behåller tvåstegsverifiering aktiverat för mitt konto finns tooapprove två meddelanden?

Nej, inte. Logga in tooyour Microsoft-konto med telefonen räknas som tvåstegsverifiering. I stället för att skriva lösenordet, och sedan godkänna ett meddelande styrka din identitet genom att veta hur toounlock din telefon och sedan godkänna ett meddelande. Vi kommer inte att skicka en andra avisering tooapprove.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Vad händer om jag tappar bort min telefon eller inte har med mig, hur kan jag använda mitt konto?  

Du kan alltid Klicka **i stället använda ett lösenord** på hello-inloggningssida tooswitch tillbaka toousing ditt lösenord. Kom ihåg att om du använder tvåstegsverifiering kan du fortfarande behöver en andra metoden tooverify din inloggning. Det är därför starkt du gärna toomake till att du har extra, uppdaterad säkerhetsinformation för ditt konto. Du kan hantera din säkerhetsinformation på https://account.live.com/proofs/manage. 
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-tooentering-my-password"></a>Hur gör sluta använda den här funktionen och gå tillbaka tooentering mitt lösenord?

Klicka på **i stället använda ett lösenord** när du loggar in. Vi Kom ihåg din senaste valet och ger som standard hello genom att nästa gång du loggar in. Om du vill någonsin toogo tillbaka toosigning in med din telefon, klickar du på **använda en app i stället**. 
 
### <a name="can-i-use-hello-app-toosign-in-tooall-my-accounts-with-microsoft"></a>Kan jag använda hello app toosign i tooall Mina konton med Microsoft?   
Den här funktionen är endast tillgängligt för personliga Microsoft-konton. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Kan jag logga in på datorn med min telefon?  
För din dator bör du logga in med Windows Hello på Windows 10 använda din yta, fingeravtryck eller en PIN-kod.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Kan jag logga in med min Windows Phone?  
För närvarande kan utvecklar vi inte funktionen för hello Microsoft Authenticator på Windows Phone. 

## <a name="next-steps"></a>Nästa steg
Om du inte har hämtat hello Microsoft Authenticator-appen kan checka ut den. hello appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), och är tillgängligt på hello Microsoft Authenticator-appen för inloggning phone [Android](http://go.microsoft.com/fwlink/?Linkid=825072) och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Om du har frågor om hello app i allmänhet kan ta en titt på hello [Microsoft Authenticator vanliga frågor och svar](microsoft-authenticator-app-faq.md)
