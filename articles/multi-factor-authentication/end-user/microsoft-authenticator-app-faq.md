---
title: "aaaMicrosoft autentiseringsapp hjälp och support | Microsoft Docs"
description: "Visar en lista med vanliga frågor och svar och relaterade toohello Microsoft Authentication-appen Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: afba9b59ccaac60d022e8516fcf573dcfbf68af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft autentiseringsapp vanliga frågor och svar

Den här artikeln innehåller vanliga frågor som vi får om hello Microsoft Authenticator-appen. Om du inte ser en fråga tooyour gå toohello [Microsoft Authenticator app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Vi har också en annan vanliga frågor och svar om en specifik funktion på hello app [logga in med din telefon vanliga frågor och svar](microsoft-authenticator-app-phone-signin-faq.md).

hello Microsoft Authenticator-appen ersättas hello Azure Authenticator-appen och hello rekommenderas app när du använder Azure Multi-Factor Authentication. hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="what-are-hello-codes-in-hello-app-for-why-does-hello-number-keep-counting-down"></a>Vad är hello koder i hello-appen för? Varför hello nummer hålla räknat?

När du öppnar hello Microsoft Authenticator-appen, se hello-konton som du har lagt till och ett nummer sex eller åtta siffror som var och en av dem. Du kan se en 30 sekunder timer räknat.

Dessa koder används när du loggar in tooyour konto. När du anger ditt användarnamn och lösenord du bli ombedd tooenter en Verifieringskod. Öppna hello Microsoft Authenticator-appen och kopiera hello kod som visas just nu. Ange den kod i hello-inloggningssida toofinish.

hello orsak hello koder med 30 sekunders mellanrum ändras så att du aldrig använda hello samma kod två gånger. Det är inte som ett lösenord för att du borde tooremember. hello idé är att endast användare med åtkomst tooyour phone vet verifieringskoden.

hello-koder som kräver inte internet eller data, så du inte behöver tooworry om med phone service toosign. När du stänger hello app inte att köra i hello bakgrunden och det tömma inte batteri. Du kan stänga hello-app och ignorera tills hello nästa gång du loggar in.  

### <a name="i-only-get-notifications-when-i-have-hello-app-open-if-hello-app-isnt-open-i-dont-get-any-notifications"></a>Meddelanden visas endast när jag har hello appen öppnas. Jag får inga meddelanden om hello app inte är öppen.

Om du får meddelanden, men de inte gör brus eller vibrerar trots din ringer på, kontrollera först hello app-inställningar. Aktivera hello app toouse ljud eller vibrerar med dess meddelanden.

Om du inte får meddelanden alls, kontrollera hello följande fall:

- Är din telefon eller stör i tyst läge? Detta läge kan behålla appar från att skicka meddelanden.
- Kan du få meddelanden från andra appar? Om inte, kan det finnas ett problem med hello nätverksanslutningarna på din telefon eller hello meddelanden kanalen från Android- eller Apple. Du kan åtgärda hello första alternativet i dina telefoninställningar, men du kan behöva tootalk tooyour-leverantör för hjälp med hello andra alternativ.
- Kan du få meddelanden för vissa konton på hello app, men inte andra? Om Ja, ta bort hello problematiska kontot från din app och lägga till den igen tooenable push-meddelanden. 

Om du har försökt dessa felsökning förslag men fortfarande har problem med skicka dina loggar för diagnostik. Gå toohello app-inställningar och sedan välja **hjälp och feedback** och **skicka loggar**. Gå sedan toohello [Microsoft Authenticator app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) och berätta för oss vad problemet är och hur du har gjort hittills. 

### <a name="im-already-using-hello-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-tooone-click-push-notifications"></a>Jag använder redan hello Microsoft Authenticator-appen för verifieringskoder. Hur växlar tooone Klicka push-meddelanden?
Godkänna en inloggning via push-meddelanden är endast tillgängligt för personliga Microsoft-konton eller arbeta och skola Microsoft-konton, inte för konton från tredje part, till exempel Google eller Facebook. Om du har ett arbets- eller skolkonto Microsoft-konto kan kan din organisation välja toodisable det här alternativet.

Om du använder ett Microsoft-konto för ditt eget konto och vill ha tooswitch över toopush meddelanden, måste tooadd ditt konto igen. Registrera hello enheten med ditt konto och konfigurera push-meddelanden.  

Om du använder Microsoft Authenticator för ditt arbets- eller skolkonto konto och ditt företag avgör om tooallow enkelklickning meddelanden.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Fungerar enkelklickning push-meddelanden för icke-Microsoft konton?
Nej, push-meddelanden fungerar endast med Microsoft-konton och Azure Active Directory-konton. Om ditt arbete eller skola använder Azure AD-konton, kan de inaktivera den här funktionen.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Jag har återställt min enhet från en säkerhetskopia och koder för min saknas eller inte fungerar. Vad hände?
Vi återställa inte konton från appen säkerhetskopior av säkerhetsskäl.  När du återställer hello app kan ta bort dina konton och lägga till dem igen.

### <a name="i-got-a-new-device-how-do-i-remove-hello-microsoft-authenticator-app-from-my-old-device-and-move-toohello-new-one"></a>Jag har fått en ny enhet. Hur gör ta bort hello Microsoft Authenticator-appen från min gamla enhet och flytta toohello ny?
Att lägga till hello Microsoft Authenticator app tooa ny enhet tar inte bort automatiskt den från andra enheter. toomanage vilka enheter som har konfigurerats för ditt konto besöker hello samma webbplats som du använder toomanage tvåstegsverifiering och väljer tooremove gamla appar.

För personliga Microsoft-konton, den här webbplatsen är din [kontosäkerhet](https://account.microsoft.com/security) sidan. För arbetet eller skolan Microsoft-konton, den här webbplatsen kan antingen vara [MyApps](https://myapps.microsoft.com) eller en anpassad portal som din organisation har ställts in.

### <a name="how-do-i-remove-an-account-from-hello-app"></a>Hur tar jag bort ett konto från hello app?
* iOS: från huvudskärmen hello sveper du till vänster på ikonen för ett konto. Välj **Ta bort**.
* Windows Phone: Hello huvudskärmen, Välj hello menyn knappen sedan **redigera konton**. Tryck på hello **X** nästa toohello kontonamn.
* Android: Hello huvudskärmen, Välj hello menyn knappen sedan **redigera konton**. Tryck på hello **X** nästa toohello kontonamn.

Om du har en enhet som har registrerats hos din organisation kan behöva du toocomplete en extra steg tooremove ditt konto. På dessa enheter registreras hello Microsoft Authenticator-appen automatiskt som en enhetsadministratör för. Om du vill toocompletely avinstallera hello app måste toofirst avregistrera hello app i hello app-inställningar.

### <a name="why-does-hello-app-request-so-many-permissions"></a>Varför begär så många behörighet av hello app?
Här är en fullständig lista över behörigheter som vi kan fråga efter och hur de används i hello app. hello specifika behörigheter som visas beror på hello phone som du har.

* **Kameran**: Vi använder din kamera tooscan QR-koder när du lägger till en arbete, din skola eller icke-Microsoft-konto.
* **Kontakter och phone**: när du loggar in med ditt personliga Microsoft-konto vi försöka toosimplify hello processen genom att söka efter befintliga konton som du använder på din telefon.
* **SMS**: när du loggar in med ditt personliga Microsoft-konto för hello första gången, har vi toomake till att din telefon antalet matchningar hello något av posten. Vi skickar en text meddelandet toohello phone dit du hämtade hello app. hello-meddelande innehåller en Verifieringskod för 6 – 8 siffror. I stället för att be toofind den här koden och ange den i hello app vi dig att hitta det. i hello SMS.
* **Rita över andra appar**: när du får ett meddelande tooverify din identitet kan vi visas detta meddelande över alla andra appar som kan köras.
* **Ta emot data från hello internet**: den här behörigheten krävs för att skicka meddelanden.
* **Förhindra att phone viloläge**: Om du registrerar din enhet med din organisation, de kan ändra den här principen på din telefon.
* **Kontrollera vibration**: du kan välja om du vill att en vibration när du tar emot ett meddelande tooverify din identitet.
* **Använda fingeravtryck maskinvara**: vissa arbets- och skolkonton konton kräver en ytterligare PIN-kod när du verifierar din identitet. toomake hello processen enklare vi att du toouse ditt fingeravtryck istället för att ange hello PIN-kod.
* **Visa nätverksanslutningar**: när du lägger till ett Microsoft-konto hello appen kräver nätverk, internet-anslutning.
* **Läsa hello innehållet för lagringsutrymmet**: den här behörigheten används bara när du rapporterar via hello appinställningar tekniska problem. Viss information från ditt lager samlas toodiagnose hello problemet.
* **Fullständig nätverksåtkomst**: den här behörigheten krävs för att skicka meddelanden tooverify din identitet.
* **Kör vid start**: Om du startar om telefonen den här behörigheten garanterar att du fortsätter du att få meddelanden tooverify din identitet.

### <a name="why-does-hello-microsoft-authenticator-app-allow-you-tooapprove-a-request-without-unlocking-hello-device"></a>Varför hello Microsoft Autentiseringsapp kan du tooapprove en begäran utan att låsa upp hello-enhet?

Du har inte toounlock enhet tooapprove verifieringen begäranden eftersom allt du behöver tooprove är att du har telefonen med dig. Tvåstegsverifiering kräver bevisar två saker – en sak som du vet och en sak som du har. hello bör du vet är ditt lösenord. hello som du har är att din telefon (med hello Microsoft Authenticator-appen och registrerad som ett bevis MFA.) Därför har hello telefon och godkänna hello begäran uppfyller hello kriterier för hello tvåfaktorsautentisering för autentisering. 

### <a name="what-does-hello-lock-icon-in-hello-account-list-mean"></a>Vad innebär hello låsikonen i listan över hello?

hello hänglåsikon indikerar hello enheten är registrerad i Azure AD och registrerad toohello konto. Registrering av enheten för iOS äger rum under Microsoft Intune-registrering.

## <a name="next-steps"></a>Nästa steg

### <a name="contact-us"></a>Kontakta oss
Om din fråga inte besvaras här, vill vi toohear från dig. Gå toohello [Microsoft Authenticator app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) toopost din fråga och få hjälp från hello community, eller lämna en kommentar på den här sidan.


### <a name="related-topics"></a>Relaterade ämnen
* [Om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) för Microsoft-konton
* [Har du problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md) för ditt arbets- eller skolkonto konto?
* [Använd hello Microsoft Authenticator toosign i från din telefon](microsoft-authenticator-app-phone-signin-faq.md)

