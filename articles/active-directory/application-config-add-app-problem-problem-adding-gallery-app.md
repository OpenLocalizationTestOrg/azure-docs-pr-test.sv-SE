---
title: "lägga till ett program för Azure AD-galleriet aaaProblem | Microsoft Docs"
description: "Förstå hello vanliga problem personer yta när du lägger till Azure AD-galleriet program och vad du kan göra tooresolve dem."
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
ms.openlocfilehash: 654f98116176d5590563c0471b92809f8763fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Problem att lägga till ett program för Azure AD-galleriet

Den här artikeln hjälper dig toounderstand hello vanliga problem personer står inför när du lägger till Azure AD-galleriet program och vad du kan göra tooresolve dem.

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a>Klickade hello ”Lägg till”-knappen och mitt program tog en lång tid tooappear

I vissa fall kan det ta 1 till 2 minuter (och ibland längre) för ett program tooappear har lagt till tooyour directory. Även om detta inte är hello normal prestanda ser hello programmet tillägg pågår genom att klicka på hello **meddelanden** ikonen (hello bell) i hello övre högra hörnet på hello [Azure Portal](https://portal.azure.com/)och letar efter en **pågår** eller **slutförd** meddelande med etiketten **skapa program.**

Om programmet aldrig har lagts till, eller om det uppstår ett fel när du klickar på hello **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd. Om du vill veta mer om hello fel toolearn mer tooor dela med en stöd engingeer du kan se mer information om hello felet genom att följa stegen hello i hello [hur toosee hello information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a>Klickade hello ”Lägg till”-knappen och mitt program visas inte

Ibland på grund av problem med tootransient nätverksproblem eller ett programfel kan lägga till ett program misslyckas. Du kan se detta händer när du klickar på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure Portal och du ser ett rött (!) ikonen nästa tooyour **skapa program** meddelande. Detta anger att ett fel uppstod när du skapar hello program.

Om det uppstår ett fel när du klickar på hello **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd. Om du vill veta mer om hello fel toolearn mer tooor dela med en stöd engingeer du kan se mer information om hello felet genom att följa stegen hello i hello [hur toosee hello information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.

 ## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a>Jag vet inte hur tooset in Mina program när jag har lagt till den

Om du behöver hjälp med utbildning om program hello [lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) artikeln är en bra toostart.

I tillägg toothis hello [dokumentbibliotek för Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) toolearn mer om enkel inloggning med Azure AD och hur det fungerar.

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Hur toosee hello information om ett meddelande om portal

Du kan se hello information om eventuella portalmeddelandet genom att följa hello stegen nedan:

1.  Klicka på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure-portalen

2.  Markera alla meddelanden i en **fel** tillstånd (de med en röd (!) nästa toothem).

    >[!NOTE]
    >Du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.
    >
    >

3.  Den här öppna hello **detaljer** bladet.

4.  Använd den här informationen själv toounderstand mer information om hello problem.

5.  Om du fortfarande behöver hjälp kan också dela informationen med en stöd tekniker eller hello grupp tooget produkthjälp med ditt problem.

6.  Klicka på hello **kopia** **ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Hur tooget genom att skicka meddelande information tooa supportteknikern

Det är mycket viktigt att du dela **alla hello information som visas nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt. Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på hello **kopiera felikonen**, hitta toohello höger om hello **Kopieringsfel** textruta.

## <a name="notification-details-explained"></a>Meddelandeinformation förklaras

hello nedan förklaras mer vad varje hello avisering artiklar innebär och ger exempel på var och en av dem.

### <a name="essential-notification-items"></a>Viktigt meddelande objekt

-   **Rubrik** – hello beskrivande rubrik av hello-meddelande

  * Exempel – **Application proxy-inställningar**

-   **Beskrivning** – hello beskrivning av vad som hänt på grund av hello åtgärden

    -   Exempel – **intern url har angett används redan av ett annat program**

-   **Meddelande-Id** – hello unikt id för hello-meddelande

    -   Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Id för klientbegäran** – hello specifik begäran-id som din webbläsare

    -   Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Tid UTC stämpel** – hello tidsstämpeln då uppstod hello-meddelande i UTC

    -   Exempel – **2017-03-23T19:50:43.7583681Z**

-   **Internt transaktions-Id** – hello internt ID vi kan använda toolook hello fel i vårt system

    -   Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – hello-användaren som utförde åtgärden hello

    -   Exempel –**tperkins@f128.info**

-   **Klient-Id** – hello unikt ID för hello-klient som hello användaren som utförde åtgärden hello var medlem av

    -   Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Användarobjektet Id** – hello unikt ID för hello-användaren som utförde åtgärden hello

    -   Exempel – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Detaljerat meddelande objekt

-   **Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn för hello-fel

    -   Exempel – **Application proxy-inställningar**

-   **Status för** – hello viss status av hello-meddelande

    -   Exempel – **misslyckades**

-   **Objekt-Id** – **(kan vara tom)** hello mot vilken hello åtgärden utfördes objekt-ID

    -   Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Information om** – hello detaljerad beskrivning av vad som hänt på grund av hello åtgärden

    -   Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**

-   **Kopiera fel** – Klicka på hello **kopiera-ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker

    -   Exempel```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
