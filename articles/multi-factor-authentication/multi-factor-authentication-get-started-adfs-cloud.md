---
title: aaaSecure molnresurser med Azure MFA och AD FS | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA och AD FS i hello molnet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Skydda molnresurser med Azure Multi-Factor Authentication och AD FS
Om din organisation är federerat med Azure Active Directory, använder du Azure Multi-Factor Authentication eller Active Directory Federation Services (AD FS) toosecure resurser som kan nås av Azure AD. Använd följande procedurer toosecure Azure Active Directory-resurser med Azure Multi-Factor Authentication eller Active Directory Federation Services hello.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Skydda Azure AD-resurser med hjälp av AD FS
toosecure din molnresursen, konfigurera en regel för anspråk så att Active Directory Federation Services avger hello multipleauthn anspråk när en användare utför tvåstegsverifiering har. Denna begäran har skickats på tooAzure AD. Följ den här proceduren toowalk hello stegen:


1. Öppna AD FS-hantering.
2. Hello vänster markerar **förtroende för förlitande part**.
3. Högerklicka på **Microsoft Office 365 Identity Platform** och välj **Redigera anspråksregler**.

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. På Hej guiden Lägg till transformera anspråk, Välj **släpp igenom eller filtrera ett inkommande anspråk** hello listrutan och klicka på **nästa**.

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Namnge din regel. 
7. Välj **autentiseringsmetodernas referenser** hello inkommande anspråkstyp.
8. Välj **Släpp igenom alla anspråksvärden**.
    ![Guiden Lägg till anspråksregel för transformering](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Klicka på **Slutför**. Stäng hello AD FS-hanteringskonsol.

## <a name="trusted-ips-for-federated-users"></a>Tillförlitliga IP-adresser för federerade användare
Tillförlitliga IP-adresser kan administratörer tooby pass tvåstegsverifiering för specifika IP-adresser eller externa användare som har begäranden från inom sin egen intranät. hello följande avsnitt beskrivs hur tooconfigure Azure Multi-Factor Authentication tillförlitliga IP-adresser med externa användare och hoppa över tvåstegsverifiering när en begäran kommer från inom en federerad användare intranät. Detta uppnås genom att konfigurera AD FS toouse en direktlagringsdisk eller filtrera ett inkommande anspråk mallen med hello inuti företagsnätverket anspråkstyp.

I det här exemplet används Office 365 för våra förlitande partsförtroenden.

### <a name="configure-hello-ad-fs-claims-rules"></a>Konfigurera regler för hello AD FS-anspråk
hello första vi behöver toodo är tooconfigure hello AD FS-anspråk. Skapa två anspråksregler, en för hello inuti företagsnätverket Anspråkstypen och ytterligare en för att hålla våra användare som har loggat in.

1. Öppna AD FS-hantering.
2. Hello vänster markerar **förtroende för förlitande part**.
3. Högerklicka på **Microsoft Office 365-identitetsplattform** och välj **Redigera anspråksregler...**
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. För Utfärdande av transformeringsregler klickar du på **Lägg till regel.**
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. På Hej guiden Lägg till transformera anspråk, Välj **släpp igenom eller filtrera ett inkommande anspråk** hello listrutan och klicka på **nästa**.
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Hello rutan nästa tooClaim Regelnamn, ge regeln ett namn. Exempel: InsideCorpNet.
7. Från hello nedrullningsbara nästa tooIncoming Anspråkstyp, Välj **inuti företagsnätverket**.
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klicka på **Slutför**.
9. För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.
10. På Hej guiden Lägg till transformera anspråk, Välj **skicka anspråk med en anpassad regel** hello listrutan och klicka på **nästa**.
11. I rutan hello under Regelnamn för anspråk: Ange *hålla användare inloggad i*.
12. Skriv följande i hello anpassad regel:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klicka på **Slutför**.
14. Klicka på **Använd**.
15. Klicka på **OK**.
16. Stäng AD FS-hantering.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurera tillförlitliga IP-adresser med federerade användare i Azure Multi-Factor Authentication
Nu när hello anspråk är på plats kan konfigurera vi tillförlitliga IP-adresser.

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på vänster hello **Active Directory**.
3. Välj hello katalog där du vill tooset in tillförlitliga IP-adresser under katalog.
4. Klicka på hello katalog som du har valt, **konfigurera**.
5. På hello multifaktorautentisering under **hantera tjänstinställningar**.
6. Hello-tjänsten på sidan Inställningar under tillförlitliga IP-adresser, Välj **hoppa över flera-factor-autentisering för förfrågningar från externa användare i intranätet**.  

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. Klicka på **Spara**.
8. När hello-uppdateringarna har tillämpats, klickar du på **Stäng**.

Klart! Federerade Office 365-användare ska nu endast ha toouse MFA när ett anspråk som kommer från utanför hello företagets intranät.
