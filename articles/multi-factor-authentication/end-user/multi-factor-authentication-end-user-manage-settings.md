---
title: "aaaManage dina inställningar för tvåstegsverifiering | Microsoft Docs"
description: "Hantera hur du använder Azure Multi-Factor Authentication inklusive ändrar kontaktinformation eller konfigurera dina enheter."
services: multi-factor-authentication
keywords: flerfunktionsautentisering klienten, problem med autentisering, Korrelations-ID
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>Hantera inställningar för tvåstegsverifiering
Den här artikeln innehåller svar på frågor om hur tooupdate inställningar för tvåstegsverifiering verifieringen eller Multi-Factor authentication. Om du har problem med inloggning tooyour konto finns för[har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md) för hjälp med felsökning.

## <a name="where-toofind-hello-settings-page"></a>Där toofind hello inställningssidan
Beroende på hur företaget du konfigurerar Azure Multi-Factor Authentication, finns det några platser där du kan ändra inställningarna som ditt telefonnummer.

Om IT-administratören skickas en specifik URL eller steg toomanage tvåstegsverifiering, följer du dessa instruktioner. Annars bör hello följa anvisningar fungerar för alla andra. Om du gör så här men inte ser hello samma inställningar som innebär att ditt arbete eller skola anpassad sina egna portal. Be din administratör för hello länken tooyour flerfunktionsautentisering Azure portal.

1. Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Välj namnet på ditt konto i hello upp till höger och sedan **profil**.  
3. Välj **ytterligare säkerhetsverifiering**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. hello ytterligare säkerhet verifiering sidan läses in med dina inställningar.

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a>Jag vill toochange mitt telefonnummer eller lägga till en sekundär tal
Det är viktigt tooconfigure ett telefonnummer för sekundära autentiseringen.  Eftersom din primära telefonnummer nummer och din mobila app är förmodligen på hello samma telefon, hello sekundära telefonnumret är hello endast hur du ska kunna tooget tillbaka till ditt konto om telefonen tappas bort eller blir stulen.

> [!NOTE]
> Om du inte har åtkomst tooyour primära telefonnummer och behöver hjälp med att komma i tooyour konto, finns våra hjälpavsnitten i [har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md).  

**toochange din primära telefonnummer:**  

1. Hello ytterligare säkerhet kontroll på sidan Välj hello textruta med ditt aktuella telefonnummer och redigera den med ditt nya telefonnummer.  
2. Välj **Spara**.  
3. Om detta är hello-nummer som du använder för din verifieringsalternativet har du tooverify hello nytt nummer innan du kan spara den.  

**tooadd ett sekundärt telefonnummer:**  

1. Hello ytterligare säkerhet kontroll på sidan kryssrutan hello bredvid för**telefon för alternativa autentisering.**  
2. Ange ditt sekundära telefonnummer i hello textruta.  
3. Välj **spara** och ändringarna är klara.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Kräv tvåstegsverifiering igen på en enhet som du har markerat som betrodd

Beroende på inställningarna för din organisation kan du ha en kryssruta med texten ”fråga inte igen för **X** dagar” när du utför tvåstegsverifiering i webbläsaren. Om den här kryssrutan och sedan tappar bort din enhet eller anser att ditt konto komprometteras, bör du återställa tvåstegsverifiering verifiering tooall dina enheter. 

1. Hello ytterligare säkerhet kontroll på sidan Välj **Återställ multifaktorautentisering på tidigare betrodda enheter**.
2. hello kommer nästa gång du loggar in på alla enheter du att tillfrågas tooperform tvåstegsverifiering. 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a>Hur gör Rensa Microsoft Authenticator från min gamla enhet och flytta tooa ny?
När du avinstallerar hello app från din enhet eller Återställ hello enhet tas inte bort hello-aktivering på hello serverdel. Mer information finns i [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Nästa steg
* Hämta felsökningstips och hjälpa på [har problem med tvåstegsverifiering](multi-factor-authentication-end-user-troubleshoot.md)
* Ställ in [applösenord](multi-factor-authentication-end-user-app-passwords.md) för alla program som inte stöder tvåstegsverifiering.
