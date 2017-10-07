---
title: "aaaAdmins hantera användare och enheter – Azure MFA | Microsoft Docs"
description: "Här beskrivs hur toochange användarinställningar, till exempel att tvinga hello användare toodo hello bevis startprocessen igen."
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8c35435e4f6504014d9a4f6f1b837639c3b53481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-hello-cloud"></a>Hantera användarinställningar med Azure Multi-Factor Authentication i molnet hello
Som administratör kan hantera du hello följande användar- och inställningar:

* Kräv användare tooprovide kontaktmetoder igen
* Ta bort applösenord
* Kräva MFA på alla betrodda enheter 

## <a name="require-users-tooprovide-contact-methods-again"></a>Kräv användare tooprovide kontaktmetoder igen
Den här inställningen tvingar hello användaren toocomplete hello registreringsprocessen igen. Icke-webbläsarappar fortsätta toowork om hello användaren har applösenord för dessa.  Du kan ta bort hello applösenord för användare genom att markera **ta bort alla befintliga applösenord som genererats av hello valda användare**.

### <a name="how-toorequire-users-tooprovide-contact-methods-again"></a>Hur toorequire användare tooprovide kontaktmetoder igen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänster markerar **Azure Active Directory** > **användare och grupper** > **alla användare**.
3. Välj **Multifaktorautentisering**. Hej multifaktorautentiseringssidan öppnas. 
4. Kontrollera hello rutan nästa toohello användare eller användare som du vill toomanage. En lista över snabbsteg alternativ visas på hello rätt. 
5. Välj **hantera användarinställningar**.
6. Kryssrutan för hello **Kräv att valda användare tooprovide kontaktmetoder igen**.
   ![Uppge kontaktmetoder](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. Klicka på **Spara**.
8. Klicka på **Stäng**.

## <a name="delete-users-existing-app-passwords"></a>Ta bort användare som befintliga applösenord
Den här inställningen tar bort alla hello applösenord som en användare har skapat. Icke-webbläsarbaserade appar som har kopplats till dessa applösenord upphör att fungera förrän ett nytt applösenord har skapats.

### <a name="how-toodelete-users-existing-app-passwords"></a>Hur toodelete användare befintliga applösenord
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänster markerar **Azure Active Directory** > **användare och grupper** > **alla användare**.
3. Välj **Multifaktorautentisering**. Hej multifaktorautentiseringssidan öppnas. 
6. Kontrollera hello rutan nästa toohello användare eller användare som du vill toomanage. En lista över snabbsteg alternativ visas på hello rätt. 
7. Välj **hantera användarinställningar**.
8. Kryssrutan för hello **ta bort alla befintliga applösenord som genererats av hello valda användare**.
   ![Ta bort applösenord](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klicka på **Spara**.
10. Klicka på **Stäng**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Återställ Multifaktorautentisering på alla sparade enheter för en användare
En av hello konfigurerbara funktionerna i Azure Multi-Factor Authentication ge användarna hello alternativet toomark enheter som betrodd. Mer information finns i [konfigurerar Azure Multi-Factor Authentication-inställningar](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Användare kan välja bort tvåstegsverifiering för antalet dagar på sina enheter för vanliga. Om ett konto komprometteras eller en betrodd enhet tappas bort, du behöver toobe kan tooremove hello betrodda status och behöver tvåstegsverifiering igen.

Hej **Återställ multifaktorautentisering på alla sparade enheter** inställningen innebär att användaren hello är handlingar tooperform tvåstegsverifiering verifiering hello nästa gång de loggar in, oavsett om de har valt toomark sina enheten som betrodd. 

### <a name="how-toorestore-mfa-on-all-suspended-devices-for-a-user"></a>Hur toorestore MFA på alla avbrutna enheter för en användare
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänster markerar **Azure Active Directory** > **användare och grupper** > **alla användare**.
3. Välj **Multifaktorautentisering**. Hej multifaktorautentiseringssidan öppnas. 
6. Kontrollera hello rutan nästa toohello användare eller användare som du vill toomanage. En lista över snabbsteg alternativ visas på hello rätt. 
7. Välj **hantera användarinställningar**.
8. Kryssrutan för hello **Återställ multifaktorautentisering på alla sparade enheter**
   ![ta bort applösenord](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klicka på **Spara**.
10. Klicka på **Stäng**.

## <a name="next-steps"></a>Nästa steg

- Hämta mer information om hur för[konfigurerar Azure Multi-Factor Authentication-inställningar](multi-factor-authentication-whats-next.md)

- Om dina användare behöver hjälp peka dem mot hello [användarhandboken för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user.md)
