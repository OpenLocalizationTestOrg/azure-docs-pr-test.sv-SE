---
title: "Självstudier: Azure Active Directory-integrering med PurelyHR | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PurelyHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: bdc1748ed650cff36b1ef7d7330dd2a17b3bc760
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a>Självstudier: Azure Active Directory-integrering med PurelyHR

I kursen får du lära dig hur toointegrate PurelyHR med Azure Active Directory (AD Azure).

Integrera PurelyHR med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPurelyHR
- Du kan aktivera din användare tooautomatically get inloggade tooPurelyHR (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med PurelyHR, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En PurelyHR enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till PurelyHR från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-purelyhr-from-hello-gallery"></a>Att lägga till PurelyHR från hello-galleriet
tooconfigure hello integrering av PurelyHR i Azure AD, behöver du tooadd PurelyHR hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd PurelyHR från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **PurelyHR**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. Markera hello resultat på panelen **PurelyHR**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PurelyHR baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PurelyHR är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PurelyHR toobe upprättas.

I PurelyHR, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med PurelyHR, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare PurelyHR](#creating-a-purelyhr-test-user)**  -toohave en motsvarighet för Britta Simon i PurelyHR som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PurelyHR program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PurelyHR:**

1. I hello Azure-portalen på hello **PurelyHR** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. På hello **PurelyHR domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyID>.purelyhr.com/sso-consume`

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<companyID>.purelyhr.com/sso-initiate`
     
    > [!NOTE]
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL. Kontakta [PurelyHR klienten supportteamet](http://support.purelyhr.com/) tooget dessa värden. 

5. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. På hello **PurelyHR Configuration** klickar du på **konfigurera PurelyHR** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. tooconfigure enkel inloggning på **PurelyHR** , på webbplatsen för inloggning tootheir som administratör.

9. Öppna hello **instrumentpanelen** från hello alternativ i hello verktygsfältet och på **SSO-inställningarna**.

10. Klistra in hello värden i hello rutor som beskrivs nedan-

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    a. Öppna hello **Certificate(Bas64)** hämtas från hello Azure-portalen i anteckningar och kopiera hello Certifikatvärde. Klistra in hello kopieras värdet till hello **X.509-certifikat** rutan.

    b. I hello **Idp utfärdar-URL** klistra in hello **SAML enhets-ID** kopieras från hello Azure-portalen.

    c. I hello **Idp slutpunkts-URL** klistra in hello **SAML inloggning tjänst-URL för enkel** kopieras från hello Azure-portalen. 

    d. Kontrollera hello **skapa automatiskt användare** kryssrutan tooenable automatisk användaretablering i PurelyHR.

    e. Klicka på **spara ändringar** toosave hello inställningar.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-purelyhr-test-user"></a>Skapa en testanvändare PurelyHR

tooenable Azure AD-användare toolog i tooPurelyHR, måste de etableras i PurelyHR. Etablering är en automatisk uppgift i PurelyHR, och inga manuella steg krävs när automatisk användaretablering är aktiverad.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPurelyHR.

![Tilldela användare][200] 

**tooassign Britta Simon tooPurelyHR utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **PurelyHR**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Klicka på hello upptar LMS panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour upptar LMS program.

Mer information om hello åtkomstpanelen finns i. [Introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

