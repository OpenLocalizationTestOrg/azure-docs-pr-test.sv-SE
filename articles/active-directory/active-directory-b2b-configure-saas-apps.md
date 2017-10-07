---
title: "aaaConfigure SaaS-appar för B2B-samarbete i Azure Active Directory | Microsoft Docs"
description: "Koden och PowerShell-exempel för Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>Konfigurera SaaS-appar för B2B-samarbete

Azure Active Directory (AD Azure) B2B-samarbete fungerar med de flesta appar som integreras med Azure AD. I det här avsnittet går vi igenom instruktioner för att konfigurera vissa populära SaaS-appar för användning med Azure AD B2B.

Innan du tittar på app-specifik anvisningarna är här några tumregel:

* För de flesta hello appar måste användarinställningar toohappen manuellt. Det vill säga skapas användare manuellt i hello-app.

* För appar som har stöd för automatisk installation, till exempel Dropbox, skapas separat inbjudningar från hello appar. Användare måste vara säker på att tooaccept varje inbjudan.

* Ange alltid i hello användarattribut, toomitigate eventuella problem med felaktig användarprofil-disk (UPD) i gästanvändare, **användar-ID** för**user.mail**.


## <a name="dropbox-business"></a>Dropbox företag

tooenable användare toosign in med sina organisation, måste du manuellt konfigurera Dropbox Business toouse Azure AD som en identitetsleverantör Security Assertion Markup Language (SAML). Fråga om Dropbox företag inte har konfigurerat toodo så kan inte eller annars tillåter användare toosign i med hjälp av Azure AD.

1. tooadd hello Dropbox Business appen till Azure AD, Välj **företagsprogram** i hello till vänster och klicka sedan på **Lägg till**.

  ![Hej ”Lägg till”-knappen hello Enterprise program sida](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. I hello **lägga till ett program** fönstret Ange **dropbox** i hello sökrutan och väljer sedan **Dropbox för företag** i hello resultatlistan.

  ![Sök efter ”dropbox” på hello Lägg till ett program på sidan](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. På hello **enkel inloggning** väljer **enkel inloggning** i hello till vänster och sedan ange **user.mail** i hello **användar-ID** ruta. (Det är angivet som UPN som standard.)

  ![Konfigurera enkel inloggning för hello app](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. toodownload hello certifikat toouse för Dropbox-konfiguration, markera **konfigurera DropBox**, och välj sedan **SAML logga på tjänst-URL för enkel** i hello-listan.

  ![Hämta hello certifikat för Dropbox-konfiguration](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Logga in tooDropbox med hello inloggning URL från hello **enkel inloggning** sidan.

  ![sidan hello Dropbox-inloggning](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. Välj på menyn hello **administratörskonsolen**.

  ![Hej ”Admin Console” länk på hello Dropbox-menyn](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. I hello **autentisering** dialogrutan **mer**, överför hello certifikat och sedan, i hello **logga in URL: en** anger Webbadressen för hello SAML enkel inloggning.

  ![Hej länken ”Mer” hello komprimerad dialogrutan för autentisering](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![Hej ”tecken i URL” i hello expanderas dialogrutan autentisering](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. Välj tooconfigure automatisk användarinställningar i hello Azure-portalen **etablering** i hello vänster och välj **automatisk** i hello **etablering läge** och välj sedan **Auktorisera**.

  ![Konfigurera automatisk användaretablering i hello Azure-portalen](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

När gästen eller medlem användare har ställts in i hello Dropbox app, kan de få en separat inbjudan från Dropbox. toouse Dropbox enkel inloggning inbjudna måste acceptera inbjudan hello genom att klicka på en länk i den.

## <a name="box"></a>Box
Du kan aktivera användare tooauthenticate rutan gästanvändare med sina Azure AD-kontot med hjälp av federation som baseras på hello SAML-protokoll. I den här proceduren kan du överföra metadata tooBox.com.

1. Lägg till hello Box-app från hello företagsappar.

2. Konfigurera enkel inloggning i hello följande ordning:

  ![Konfigurera rutan enkel inloggning](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 a. I hello **inloggning URL** kontrollerar du att hello inloggnings-URL är inställd på rätt sätt för rutan i hello Azure-portalen. Denna URL är hello URL för din Box.com-klient. Det bör följa hello namngivningskonvention *https://.box.com*.  
 Hej **identifierare** gäller inte toothis appen, men som fortfarande visas som ett obligatoriskt fält.

 b. I hello **användar-ID** ange **user.mail** (för enkel inloggning för gästkonton).

 c. Under **SAML-signeringscertifikat**, klickar du på **Skapa nytt certifikat**.

 d. Konfigurera din Box.com klient toouse Azure AD som en identitetsleverantör toobegin hämta hello metadatafil och spara sedan det tooyour lokal enhet.

 e. Vidarebefordra hello metadata filen toohello rutan supportteamet, som konfigurerar enkel inloggning för dig.

3. Azure AD automatisk användarinställningar, i hello vänster och välj **etablering**, och välj sedan **auktorisera**.

  ![Verifiera Azure AD tooconnect tooBox](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Som Dropbox inbjudna lösa rutan inbjudna vill sina inbjudan från hello Box-app.

## <a name="next-steps"></a>Nästa steg

Se följande artiklar om Azure AD B2B-samarbete hello:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2B-samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
