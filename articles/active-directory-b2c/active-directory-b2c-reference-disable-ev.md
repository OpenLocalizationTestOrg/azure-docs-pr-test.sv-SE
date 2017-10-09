---
title: 'Azure Active Directory B2C: Inaktivera e-Postverifiering under registreringen konsumenten | Microsoft Docs'
description: Ett avsnitt som visar hur toodisable e-verifiering under konsumenten registrering i Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Inaktivera e-Postverifiering under konsumenten registrering
När aktiverad hello Azure Active Directory (AD Azure) B2C ger en konsument möjlighet toosign för program genom att tillhandahålla en e-postadress och skapa ett lokalt konto. Azure AD B2C garanterar giltiga e-postadresser genom att kräva konsumenter tooverify dem under hello registreringsprocessen. Det förhindrar också en skadlig automatiserad process genererar falska konton för hello program.

Vissa programutvecklare föredrar tooskip e-Postverifiering under hello registreringsprocessen och i stället har konsumenter verifiera hello e-postadress senare. toosupport detta Azure AD B2C kan vara konfigurerade toodisable e-verifiering. Gör detta skapar en jämnare registreringsprocessen och ger utvecklare hello flexibilitet toodifferentiate hello konsumenter som sin e-postadress från dessa användare inte har verifierats.

Principer för registrering har e-Postverifiering aktiverad som standard. Använd hello följande steg tooturn av:

1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicka på **principer för registrering** eller **principer för registrering eller inloggning** beroende på vad du har konfigurerat för registrering.
3. Klicka på principen (till exempel ”B2C_1_SiUp”)-tooopen den. Klicka på **redigera** hello överst i hello-bladet.
4. Klicka på **sidan anpassningar**.
5. Klicka på **registreringssidan för lokalt konto**.
6. Klicka på **e-postadress** i hello **namn** kolumnen under hello **registreringsattribut** avsnitt.
7. Växla hello **Kräv verifiering** alternativet för**nr**.
8. Klicka på **OK** längst ned hello tills du når hello **Redigera princip** bladet.
9. Klicka på **spara** hello överst i hello-bladet. Du är klar!

> [!NOTE]
> Inaktivera e-Postverifiering i hello registreringsprocessen kan det leda till att toospam. Om du inaktiverar hello standardversionen rekommenderar vi att lägga till egna verifieringssystem.
> 
> 

Vi är alltid öppna toofeedback och förslag! Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant. För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
