---
title: "Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler privat åtkomst (ZPA)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 0370cff60c8ac15bd1919acccc924da1e50dc45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Självstudier: Azure Active Directory-integrering med Zscaler privat åtkomst (ZPA)

I kursen får du lära dig hur toointegrate Zscaler privat åtkomst (ZPA) med Azure Active Directory (AD Azure).

Integrera Zscaler privat åtkomst (ZPA) med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooZscaler privat åtkomst (ZPA)
- Du kan aktivera din användare tooautomatically get inloggade tooZscaler privat åtkomst (ZPA) (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Zscaler privat åtkomst (ZPA), behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Zscaler privat åtkomst (ZPA) enkel inloggning på aktiverade prenumeration


> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zscaler privat åtkomst (ZPA) från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-zscaler-private-access-zpa-from-hello-gallery"></a>Att lägga till Zscaler privat åtkomst (ZPA) från hello-galleriet
tooconfigure hello integrering av Zscaler privat åtkomst (ZPA) i Azure AD, behöver du tooadd Zscaler privat åtkomst (ZPA) från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd Zscaler privat åtkomst (ZPA) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Zscaler privat åtkomst (ZPA)**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. Markera hello resultat på panelen **Zscaler privat åtkomst (ZPA)**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zscaler privat åtkomst (ZPA) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Zscaler privat åtkomst (ZPA) toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zscaler privat åtkomst (ZPA).

tooconfigure och testa Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Zscaler privat åtkomst (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler privat åtkomst (ZPA) som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler privat åtkomst (ZPA).

**tooconfigure Azure AD enkel inloggning med Zscaler privat åtkomst (ZPA), utför följande steg hello:**

1. I hello Azure Management portal på hello **Zscaler privat åtkomst (ZPA)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. På hello **Zscaler privat åtkomst (ZPA)-domän och URL: er** avsnittet, utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. I hello **identifierare** textruta typ:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska inloggning URL och identifierare. Vi rekommenderar här du toouse hello unikt värde för URL: en i hello identifierare. Kontakta [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. Logga in på webbplatsen för företagets Zscaler privat åtkomst (ZPA) som en administratör i en annan webbläsarfönster.

10. Navigera för**administratör** och klicka sedan på **Idp Configuration**.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. I hello **Idp Configuration** klickar du på **lägga till nya IDP konfigurationen**.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. I hello **nya IDP konfigurationen** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Klicka på **Välj fil** och ladda upp din hämtade metadatafil.

    b. Klicka på **spara** knappen.
    


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Skapa en testanvändare Zscaler privat åtkomst (ZPA)

I det här avsnittet skapar du en användare som kallas Britta Simon i Zscaler privat åtkomst (ZPA). Se tillsammans med [Zscaler privat åtkomst (ZPA) supportteamet](https://help.zscaler.com/zpa-submit-ticket) tooadd hello användare i hello Zscaler privat åtkomst (ZPA)-plattformen.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooZscaler privat åtkomst (ZPA).

![Tilldela användare][200] 

**tooassign Britta Simon tooZscaler privat åtkomst (ZPA), utför följande steg hello:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zscaler privat åtkomst (ZPA)**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    


### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Zscaler privat åtkomst (ZPA)-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Zscaler privat åtkomst (ZPA) program.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png