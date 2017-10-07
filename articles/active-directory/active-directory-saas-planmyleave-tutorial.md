---
title: "Självstudier: Azure Active Directory-integrering med PlanMyLeave | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: 44a6782e44ef22fc957544960be1742f9eed6e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Självstudier: Azure Active Directory-integrering med PlanMyLeave

I kursen får du lära dig hur toointegrate PlanMyLeave med Azure Active Directory (AD Azure).

Integrera PlanMyLeave med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPlanMyLeave
- Du kan aktivera din användare tooautomatically get inloggade tooPlanMyLeave (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med PlanMyLeave, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En PlanMyLeave enkel inloggning på aktiverade prenumeration


> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till PlanMyLeave från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-planmyleave-from-hello-gallery"></a>Att lägga till PlanMyLeave från hello-galleriet
tooconfigure hello integrering av PlanMyLeave i Azure AD, behöver du tooadd PlanMyLeave hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd PlanMyLeave från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **PlanMyLeave**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. Markera hello resultat på panelen **PlanMyLeave**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PlanMyLeave baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PlanMyLeave är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PlanMyLeave toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i PlanMyLeave.

tooconfigure och testa Azure AD enkel inloggning med PlanMyLeave, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare PlanMyLeave](#creating-a-planmyleave-test-user)**  -toohave en motsvarighet för Britta Simon i PlanMyLeave som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt PlanMyLeave program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PlanMyLeave:**

1. I hello Azure Management portal på hello **PlanMyLeave** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan sida som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. På hello **PlanMyLeave domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    a. I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.planmyleave.com/Login.aspx`
    
    b. I hello **Identiferare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.planmyleave.com`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska inloggning URL och identifierare. Kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. På hello **SAML-signeringscertifikat** klickar du på **certifikat (base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. På hello **PlanMyLeave Configuration** klickar du på **konfigurera PlanMyLeave** tooopen **konfigurera inloggning** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. Logga in på din PlanMyLeave klient som en administratör i en annan webbläsarfönster.

11. Gå för**systeminställningarna**. På hello **säkerhetshantering** Klicka på avsnittet **företagets SAML inställningar** .

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. På hello **SAML inställningar** klickar du på ikonen för redigeraren.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. På hello **SAML uppdateringsinställningar** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    a.  I hello **inloggnings-URL** textruta placera hello värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.

    b.  Öppna filen hämtat certifikat i anteckningar, kopiera endast hello mellan hello---börjar certifikat--- och---slut certifikat---det till Urklipp, och klistra in den toohello **certifikat** textruta.

    c. Ange ”**har aktiverats**” för ”**Ja**”.

    d. Klicka på **Spara**.



### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 



### <a name="creating-a-planmyleave-test-user"></a>Skapa en testanvändare PlanMyLeave

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i PlanMyLeave. PlanMyLeave stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess PlanMyLeave om den inte finns.

> [!NOTE]
> Om du behöver toocreate en användare manuellt, måste toocontact [PlanMyLeave supportteamet](mailto:support@planmyleave.com).



### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooPlanMyLeave sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooPlanMyLeave utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **PlanMyLeave**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    


### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour PlanMyLeave programmet när du klickar på hello PlanMyLeave panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png