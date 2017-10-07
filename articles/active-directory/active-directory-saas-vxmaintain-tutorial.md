---
title: "Självstudier: Integrera Azure Active Directory med vxMaintain | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och vxMaintain."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a>Självstudier: Integrera Azure Active Directory med vxMaintain

I kursen får du lära dig hur toointegrate vxMaintain med Azure Active Directory (AD Azure).

Den här integreringen ger flera viktiga fördelar. Du kan:

- Kontrollen i Azure AD som har åtkomst till toovxMaintain.
- Aktivera användare tooautomatically-inloggning toovxMaintain med enkel inloggning (SSO) med hjälp av sina Azure AD-konton.
- Hantera dina konton i en central plats: hello Azure-portalen.

toolearn mer om SaaS appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med vxMaintain, behöver du hello följande objekt:

- En Azure AD-prenumeration
- VxMaintain SSO-aktiverade prenumeration

> [!NOTE]
> När du testar hello stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. 

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

* Att lägga till vxMaintain från hello-galleriet
* Konfigurera och testa Azure AD enkel inloggning

## <a name="add-vxmaintain-from-hello-gallery"></a>Lägg till vxMaintain från hello-galleriet
tooconfigure hello integrering av vxMaintain med Azure AD, behöver du tooadd vxMaintain hello galleriet tooyour listan över hanterade SaaS-appar.

tooadd vxMaintain från galleriet hello hello följande:

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänstra rutan, Välj hello **Azure Active Directory** knappen. 

    ![hello Azure Active Directory-knappen][1]

2. Välj **företagsprogram** > **alla program**.

    ![Hej ”företagsprogram” fönstret][2]
    
3. tooadd ett program i hello **alla program** dialogrutan **nytt program**.

    ![Hej ”nya program” knappen][3]

4. Skriv i sökrutan hello **vxMaintain**.

    ![Hej ”enkel inloggning läget” listrutan](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Markera i hello resultat **vxMaintain**, och välj sedan **Lägg till**.

    ![Hej vxMaintain länk](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO med hjälp av vxMaintain, baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow hello vxMaintain motsvarighet toohello Azure AD-användare. Det vill säga måste du upprätta en länk relation mellan hello Azure AD-användare och hello motsvarande vxMaintain användare.

tooestablish hello länken relationen, tilldela hello vxMaintain **användarnamn** värde som hello Azure AD **användarnamn** värde.

tooconfigure och testa Azure AD enkel inloggning med hjälp av vxMaintain fullständig hello följande byggblock.

### <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD för enkel inloggning

I det här avsnittet kan du både aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vxMaintain hello följande:

1. I hello Azure-portalen på hello **vxMaintain** programmet integration anger **enkel inloggning**.

    ![Hej ”enkel inloggning” kommando][4]

2. tooenable SSO i hello **läget för enkel inloggning** listrutan, Välj **SAML-baserade inloggning**.
 
    ![Hej ”SAML-baserade inloggning” kommando](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. Under **vxMaintain domän och URL: er**, hello följande:

    ![Hej vxMaintain domän och URL: er](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. I hello **identifierare** Skriv en URL som har hello följande syntax:`https://<company name>.verisae.com`

    b. I hello **Reply URL** Skriv en URL som har hello följande syntax:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > hello föregående värden är inte verkliga. Uppdatera dem med hello faktiska identifierare och reply URL. tooobtain hello värden, kontakta hello [vxMaintain supportteamet](http://www.verisae.com/contact-us).
 
4. Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och sedan spara hello metadata filen tooyour dator.

    ![Hej ”SAML signeringscertifikat” avsnittet](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. Välj **Spara**.

    ![knappen Spara för hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. tooconfigure **vxMaintain** SSO, skicka hello hämtas **XML-Metadata för** filen toohello [vxMaintain supportteamet](http://www.verisae.com/contact-us).

> [!TIP]
> När du ställer in hello app kan du läsa en kortare version av hello före instruktioner i hello [Azure-portalen](https://portal.azure.com). När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentation från hello **Configuration** avsnitt. 
>
>toolearn mer om hello inbäddade dokumentationen funktionen finns [hantera enkel inloggning för företagsappar](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen hello följande:

![hello Azure AD-testanvändare][100]

1. I hello **Azure-portalen**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.

    ![Hej ”Azure Active Directory”-knappen](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. toodisplay en lista över användare, gå för**användare och grupper** > **alla användare**.
    
    ![Hej ”alla användare” länk](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)  
    Hej **alla användare** öppnas. 

3. tooopen hello **användare** dialogrutan **Lägg till**.
 
    ![hello webbinställningar](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan rutan, hello följande:
 
    ![hello användardialogrutan](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress test Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och Observera hello värdet som har genererats i hello **lösenord** rutan.

    d. Välj **Skapa**.
 
### <a name="create-a-vxmaintain-test-user"></a>Skapa en testanvändare vxMaintain

I det här avsnittet skapar du testanvändare Britta Simon i vxMaintain. tooadd i hello vxMaintain platform arbeta med den [vxMaintain supportteamet](http://www.verisae.com/contact-us). Innan du använder SSO, skapa och aktivera hello användare.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du testanvändare Britta Simon toouse Azure SSO genom att bevilja åtkomst toovxMaintain. toodo så hello följande:

![Testanvändare i hello visningsnamn lista][200] 

1. I hello Azure-portalen **program** visa finns för**Directory** Visa > **företagsprogram** > **alla program**.

    ![Hej ”alla program” länk][201] 

2. I hello **program** väljer **vxMaintain**.

    ![Hej vxMaintain länk](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. I hello vänster och välj **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Välj **Lägg till** och sedan, i hello **Lägg uppdrag** väljer **användare och grupper**.

    ![Hej ”användare och grupper” länk][203]

5. I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**, och välj sedan hello **Välj** knappen.

7. I hello **Lägg uppdrag** dialogrutan **tilldela**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Testa din Azure AD enkel inloggning

I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.

Du väljer hello **vxMaintain** panelen i hello åtkomstpanelen ska logga in dig på tooyour vxMaintain program automatiskt.

Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Nästa steg

* [Lista över självstudier om att integrera SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

