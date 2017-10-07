---
title: "Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SensoScientific trådlösa temperatur övervakning av systemet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 4eabf7fc6457c217fd5c0c2539ab88c8110055e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem

I kursen får du lära dig hur toointegrate SensoScientific trådlösa temperatur övervakningssystem med Azure Active Directory (AD Azure).

Integrera SensoScientific trådlösa temperatur övervakningssystem med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSensoScientific övervakningssystem för trådlösa temperatur
- Du kan aktivera din användare tooautomatically get inloggade tooSensoScientific trådlösa temperatur övervakningssystem (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SensoScientific trådlösa temperatur övervakningssystem måste hello följande objekt:

- En Azure AD-prenumeration
- En SensoScientific trådlösa temperatur övervakningssystem enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SensoScientific trådlösa temperatur övervakningssystem från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-hello-gallery"></a>Att lägga till SensoScientific trådlösa temperatur övervakningssystem från hello-galleriet
tooconfigure hello integrering av SensoScientific trådlösa temperatur övervakningssystem i Azure AD, behöver du tooadd SensoScientific trådlösa temperatur övervakningssystem hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SensoScientific trådlösa temperatur övervakningssystem från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SensoScientific trådlösa temperatur övervakningssystem**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. Markera hello resultat på panelen **SensoScientific trådlösa temperatur övervakningssystem**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SensoScientific trådlösa temperatur övervakningssystem är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i SensoScientific trådlösa temperatur övervakningssystem toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SensoScientific trådlösa temperatur övervakningssystem.

tooconfigure och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SensoScientific trådlösa temperatur övervakningssystem](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  -toohave en motsvarighet för Britta Simon i SensoScientific trådlösa temperatur övervakningssystem som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt övervakningssystem för SensoScientific trådlösa temperatur-program.

**tooconfigure Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakning, utför följande steg hello:**

1. I hello Azure-portalen på hello **SensoScientific trådlösa temperatur övervakningssystem** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. På hello **SensoScientific trådlösa temperatur övervakning domän och URL: er** avsnitt, utan behov av tooperform eventuella åtgärder som hello app redan redan är integrerade med Azure:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. På hello **övervakning systemkonfigurationen för SensoScientific trådlösa temperatur** klickar du på **konfigurera SensoScientific trådlösa temperatur övervakningssystem** tooopen  **Konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. Logga in tooyour SensoScientific trådlösa temperatur övervakningssystem programmet som administratör.

8. Klicka i hello navigeringsmenyn överst hello **Configuration** och gå till **konfigurera** under **enkel inloggning** tooopen hello enkel inloggning på inställningar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. I **enkel inloggning på inställningar** formuläret utföra hello följande steg:
 
    a. Välj **utfärdarnamnet** som Azure AD.
    
    b. Klistra in hello **SAML enhets-ID** som du har kopierat från Azure-portalen i utfärdar-URL-textrutan.
    
    c. Klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i textrutan för enkel inloggning tjänst-URL.

    d. Klistra in hello **Sign-Out URL** som du har kopierat från Azure-portalen i tjänst-URL för enkel Sign-Out textruta.

    e. Bläddra hello-certifikat som du har hämtat från Azure-portalen och överföra här.
    
    f. Klicka på **Spara**.
  
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Skapa en SensoScientific trådlösa temperatur övervakningssystem testanvändare

tooenable Azure AD-användare toolog i tooSensoScientific övervakningssystem för trådlösa temperatur de att etableras i SensoScientific trådlösa temperatur övervakningssystem. Arbeta med [SensoScientific trådlösa temperatur övervakningssystem supportteamet](https://www.sensoscientific.com/contact-us/) att lägga till hello användare i hello SensoScientific trådlösa temperatur övervakning plattform. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSensoScientific övervakningssystem för trådlösa temperatur.

![Tilldela användare][200] 

**tooassign Britta Simon tooSensoScientific trådlösa temperatur övervakningssystem, utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SensoScientific trådlösa temperatur övervakningssystem**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen. Klicka på hello SensoScientific trådlösa temperatur övervakningssystem panelen i hello åtkomstpanelen, kommer du att automatiskt inloggade tooyour SensoScientific trådlösa temperatur övervakning systemprogram. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

