---
title: aaaManage federationscertifikat i Azure AD | Microsoft Docs
description: "Lär dig hur toocustomize hello utgångsdatumet för federationscertifikat och hur toorenew certifikat som snart upphör att gälla."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: e17622d25c9babfa295cc0bb68c24e21b8c574d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Hantera certifikat för federerad enkel inloggning i Azure Active Directory
Den här artikeln innehåller vanliga frågor och information som rör toohello certifikat att Azure Active Directory (AD Azure) skapar tooestablish federerad enkel inloggning (SSO) tooyour SaaS-program. Lägg till program från hello Azure AD app-galleriet eller genom att använda en mall för icke-galleriet program. Konfigurera hello program genom att använda hello federerad enkel inloggning.

Den här artikeln är relevanta endast tooapps som är konfigurerade toouse Azure AD enkel inloggning via SAML federation som visas i följande exempel hello:

![Azure AD-Single Sign-On](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatiskt genererade certifikatet för galleriet och icke-galleriet program
När du lägger till ett nytt program från hello-galleriet och konfigurerar en SAML-baserade på genererar Azure AD ett certifikat för hello-program som är giltig i tre år. Du kan hämta certifikatet från hello **SAML-signeringscertifikat** avsnitt. Galleriet program, kan det här avsnittet visar ett alternativet toodownload hello certifikat eller metadata, beroende på hello behovet av programmet hello.

![Azure AD enkel inloggning](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-hello-expiration-date-for-your-federation-certificate-and-roll-it-over-tooa-new-certificate"></a>Anpassa hello förfallodatum för certifikatet för federationsserverproxyn och rulla över tooa nytt certifikat
Certifikat är som standard tooexpire efter tre år. Du kan välja ett annat utgångsdatum för ditt certifikat genom att slutföra hello följande steg.
hello skärmbilder använda Salesforce för hello saké exemplet kan, men de här stegen tooany federerad SaaS-app.

1. I hello [Azure-portalen](https://aad.portal.azure.com), klickar du på **företagsprogram** i hello till vänster och klicka sedan på **nytt program** på hello **översikt** sida:

   ![Öppna hello SSO-konfigurationsguiden](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. Sök efter hello galleriet programmet och välj sedan hello program som du vill tooadd. Om du inte hittar programmet hello krävs, lägga till hello program med hjälp av hello **icke-galleriet programmet** alternativet. Den här funktionen är endast tillgänglig i hello Azure AD Premium (P1 och P2) SKU.

    ![Azure AD enkel inloggning](./media/active-directory-sso-certs/add_gallery_application.png)

3. Klicka på hello **enkel inloggning** länken i vänster fönsterruta och ändra hello **läget för enkel inloggning** för**SAML-baserade inloggning**. Det här steget genererar ett tre års certifikat för programmet.

4. toocreate ett nytt certifikat klickar du på hello **Skapa nytt certifikat** länken i hello **SAML-signeringscertifikat** avsnitt.

    ![Generera ett nytt certifikat](./media/active-directory-sso-certs/create_new_certficate.png)

5. Hej **skapa ett nytt certifikat** länken öppnar hello kalenderkontroll. Du kan ange eventuella datum och tid toothree år för från hello aktuellt datum. hello valt datum och tid är hello nya datum och tid för det nya certifikatet. Klicka på **Spara**.

    ![Hämta och sedan ladda upp hello-certifikat](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. Hello nya certifikatet är nu tillgängliga toodownload. Klicka på hello **certifikat** länk toodownload den. Certifikatet är nu inte aktiv. När du vill ha tooroll över toothis certifikat väljer hello **aktivera nya certifikatet** kryssrutan och klicka på **spara**. Från den tidpunkten startar Azure AD med hello nytt certifikat för signering hello svar.

7.  toolearn hur tooupload hello certifikat tooyour visst SaaS-program klickar du på hello **visa configuration självstudien** länk.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Förnya ett certifikat som upphör snart att gälla
hello ska följande förnyelse leda till ingen betydande driftstopp för dina användare. hello skärmbilder i avsnittet funktionen Salesforce som ett exempel, men de här stegen kan tillämpa tooany federerad SaaS-app.

1. På hello **Azure Active Directory** programmet **enkel inloggning** kan generera hello nytt certifikat för ditt program. Du kan göra detta genom att klicka på hello **Skapa nytt certifikat** länken i hello **SAML-signeringscertifikat** avsnitt.

    ![Generera ett nytt certifikat](./media/active-directory-sso-certs/create_new_certficate.png)

2. Välj hello önskad utgångsdatum och utgångstid för din nya certifikatet och klickar på **spara**.

3. Hämta hello certifikatet i hello **SAML signeringscertifikat** alternativet. Överför hello nya certifikat toohello SaaS programmets konfiguration för enkel inloggning skärmen. toolearn hur toodo för din visst SaaS-program klickar du på hello **visa configuration självstudien** länk.
   
4. tooactivate hello nytt certifikat i Azure AD, Välj hello **aktivera nya certifikatet** kryssrutan och klicka på hello **spara** knappen hello överst på hello sidan. Detta samlar över hello nytt certifikat på hello Azure AD-sida. hello status för hello certifikat ändras från **ny** för**Active**. Från den tidpunkten startar Azure AD med hello nytt certifikat för signering hello svar. 
   
    ![Generera ett nytt certifikat](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a>Relaterade artiklar
* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Felsökning av SAML-baserade enkel inloggning](active-directory-saml-debugging.md)
