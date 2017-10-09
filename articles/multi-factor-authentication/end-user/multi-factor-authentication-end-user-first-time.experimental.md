---
title: "aaaSet tvåstegsverifiering för mitt konto för arbetet eller skolan | Microsoft Docs"
description: "När ditt företag konfigurerar Azure Multi-Factor Authentication, kommer du att ange toosign för tvåstegsverifiering. Lär dig hur tooset den. "
services: multi-factor-authentication
keywords: "hur toouse azure-katalogen, active directory i hello moln, active directory-självstudier"
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2d0348081eefa42c23de2047044688879dcb5966
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-my-account-for-two-step-verification"></a>Konfigurera mitt konto för tvåstegsverifiering
Tvåstegsverifiering är en ytterligare säkerhetssteg som hjälper dig att skydda ditt konto genom att göra det svårare för andra personer toobreak i. Om du läser den här artikeln har du förmodligen ett e-postmeddelande från administratören arbets- eller skolkonto om Multifaktorautentisering. Eller kanske du försökte toosign i och fick ett meddelande som ber tooset upp ytterligare säkerhetsverifiering. Om så är fallet hello **kan inte logga in förrän du har slutfört hello automatiska registreringen**.

Den här artikeln kan du ställa in din **arbets- eller skolkonto**. Om du vill tooenable tvåstegsverifiering för ditt eget, personligt Microsoft-konto finns [om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Konfigurera ditt konto

När IT-avdelningen kräver toostart genom tvåstegsverifiering, en skärm som säger **din administratör har begärt att du ställer in det här kontot för ytterligare säkerhetsverifiering** visas:

![Konfiguration](./media/multi-factor-authentication-end-user-first-time/first.png)

tooget igång, Välj **ställa in den nu.**

Om du inte ser en skärm som liknar detta när du loggar in, följer du anvisningarna för hello i [hantera inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) toofind hello inställningssidan där du kan hantera dina alternativ för verifiering. 

## <a name="decide-how-you-want-tooverify-your-sign-ins"></a>Bestämma hur du vill tooverify din inloggningar

hello första frågan i hello registreringen är hur du vill att vi toocontact du. Ta en titt på hello alternativ i hello tabell och Använd hello länkar toogo toohello konfigurationsstegen för varje metod.

| Kontaktmetod | Beskrivning |
| --- | --- |
| [Mobilapp](#use-a-mobile-app-as-the-contact-method) |- **Ta emot meddelanden för verifiering.** Det här alternativet skickas ett meddelande toohello authenticator-appen på din smartphone eller surfplatta. Visa hello-meddelande, och om det är tillförlitligt markerar **autentisera** i hello app. Ditt arbete eller skola kan kräva att du anger en PIN-kod innan du autentisera.<br>- **Använd verifieringskoden.** I det här läget genererar hello autentiseringsapp en Verifieringskod som uppdateras var 30: e sekund. Ange hello senaste Verifieringskod i hello i inloggningsgränssnittet.<br>hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| [Mobiltelefon samtal eller textmeddelande](#use-your-mobile-phone-as-the-contact-method) |- **Telefonsamtal** placerar en automatiserad röst anropet toohello telefonnummer du anger. Hello samtal och tryck på # i hello phone tangentbordet tooauthenticate.<br>- **Textmeddelande** slutar ett SMS med verifieringskoden. Följande hello efterfrågas i hello text, svara toohello SMS eller ange hello verifieringskoden som visas i hello i inloggningsgränssnittet. |
| [Telefonsamtal till arbete](#use-your-office-phone-as-the-contact-method) |Placerar en automatiserad röst anropet toohello telefonnummer du anger. Svaret hello anropa och trycka på # hello phone tangentbordet tooauthenticate. |

## <a name="use-a-mobile-app-as-hello-contact-method"></a>Använda en mobilapp som kontaktmetod hello
Med den här metoden kräver att du installerar en autentiseringsapp på din telefon eller surfplatta. hello stegen i den här artikeln är baserade på hello Microsoft Authenticator-appen som är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Välj **mobilappen** hello nedrullningsbara listan.
2. Välj antingen **ta emot meddelanden för verifiering** eller **Använd verifieringskoden**och välj **konfigurera**.

   ![Ytterligare säkerhet verifiering skärmen](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Öppna hello appen på din telefon eller surfplatta, och välj  **+**  tooadd ett konto. (För Android-enheter, Välj hello tre punkter sedan **Lägg till konto**.)
4. Ange att du vill tooadd ett arbets- eller skolkonto konto. hello QR-kod skanner på din telefon öppnas. Om din kamera inte fungerar korrekt, kan du välja tooenter företagets information manuellt. Mer information finns i [manuellt lägga till ett konto](#add-an-account-manually).  
5. Skanna hello QR-kod bild som visades med hello-skärmen för att konfigurera hello mobila appar.  Välj **klar** tooclose hello QR-kod skärmen.  

   ![Skärmen för QR-kod](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. När aktiveringen är klar på hello telefon, Välj **kontakta mig**.  Det här steget skickar ett meddelande eller verifiering kod tooyour telefon. Välj **Kontrollera**.  
7. Om ditt företag kräver en PIN-kod för att godkänna inloggningsverifiering, anger du den.

   ![Kryssrutan för att ange en PIN-kod](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. När PIN-koden är klar, Välj **Stäng**. Nu ska verifieringen lyckades.
9. Vi rekommenderar att du anger ditt mobiltelefonnummer ifall du skulle förlora åtkomst tooyour mobila appar. Välj land hello listrutan och ange ditt mobiltelefonnummer i hello rutan nästa toohello Landsnamn. Välj **nästa**.
10. Du kan nu ange tooset in applösenord för icke-webbläsarappar, till exempel Outlook 2010 eller äldre eller hello interna e-app på Apple-enheter. Den här åtgärden är eftersom vissa appar inte stöder tvåstegsverifiering. Om du inte använder de här apparna klickar du på **klar** och hoppa över hello resten av stegen.
11. Om du använder de här apparna kopiera hello applösenord tillhandahålls och klistra in den i ditt program i stället för vanliga lösenord. Du kan använda hello samma applösenord för flera appar. Mer information [hjälp med applösenord].
12. Klicka på **Klar**.

### <a name="add-an-account-manually"></a>Lägg till ett konto manuellt
Följ dessa steg om du vill tooadd en mobilapp för kontot toohello manuellt, i stället för hello QR-läsaren.

1. Välj hello **ange kontot manuellt** knappen.  
2. Ange hello kod och hello URL som finns angivna på hello samma sida som visar hello streckkod. Den här informationen är med i hello **kod** och **URL** rutor på hello mobila appar.

    ![Konfiguration](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. När hello aktiveringen är klar, Välj **kontakta mig**. Det här steget skickar ett meddelande eller verifiering kod tooyour telefon. Välj **Kontrollera**.

## <a name="use-your-mobile-phone-as-hello-contact-method"></a>Använda mobiltelefonen som kontaktmetod hello
1. Välj **telefon för autentisering** hello nedrullningsbara listan.  

    ![Konfiguration](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Välj ditt land hello listrutan och ange ditt mobiltelefonnummer.
3. Välj hello metod du föredrar toouse med din mobiltelefon - textmeddelande eller samtal.
4. Välj **kontakta mig** tooverify ditt telefonnummer. Beroende på hello-läge som du valde vi skickar ett SMS eller ringa dig. Följ anvisningarna på skärmen hello hello och välj sedan **Kontrollera**.
5. Du kan nu ange tooset in applösenord för icke-webbläsarappar, till exempel Outlook 2010 eller äldre eller hello interna e-app på Apple-enheter. Den här åtgärden är eftersom vissa appar inte stöder tvåstegsverifiering. Om du inte använder de här apparna klickar du på **klar** och hoppa över hello resten av hello steg.
6. Om du använder de här apparna kopiera hello applösenord tillhandahålls och klistra in den i ditt program i stället för vanliga lösenord. Du kan använda hello samma applösenord för flera appar. Mer information [hjälp med applösenord].
7. Klicka på **Klar**.

## <a name="use-your-office-phone-as-hello-contact-method"></a>Använd din Arbetstelefon som hello kontaktmetod
1. Välj **Arbetstelefon** hello listrutan  

    ![Konfiguration](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. hello telefonnummer, ruta fylls automatiskt med din kontaktinformation för företaget. Om hello tal är felaktig eller saknas kan du be din administratör toomake ändringar.
3. Välj **kontakta mig** tooverify ditt telefonnummer och vi ringer ditt nummer. Följ anvisningarna på skärmen hello hello och välj sedan **Kontrollera**.
4. Du kan nu ange tooset in applösenord för icke-webbläsarappar, till exempel Outlook 2010 eller äldre eller hello interna e-app på Apple-enheter. Den här åtgärden är eftersom vissa appar inte stöder tvåstegsverifiering. Om du inte använder de här apparna klickar du på **klar** och hoppa över hello resten av hello steg.
5. Om du använder de här apparna kopiera hello applösenord tillhandahålls och klistra in den i ditt program i stället för vanliga lösenord. Du kan använda hello samma applösenord för flera appar. Mer information finns i [vad är Applösenord](multi-factor-authentication-end-user-app-passwords.md).
6. Klicka på **Klar**.

## <a name="next-steps"></a>Nästa steg
* Ändra önskade alternativ och [hantera inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md)
* Ställ in [applösenord](multi-factor-authentication-end-user-app-passwords.md) för ursprunglig enhetsappar som inte stöder tvåstegsverifiering.
* Kolla in hello [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) säker autentisering för snabb, även om du inte har cell service.

