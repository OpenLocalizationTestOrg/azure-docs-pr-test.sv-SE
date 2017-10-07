---
title: "Självstudier: Azure Active Directory-integrering med LearnUpon | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Självstudier: Azure Active Directory-integrering med LearnUpon

I kursen får du lära dig hur toointegrate LearnUpon med Azure Active Directory (AD Azure).

Integrera LearnUpon med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooLearnUpon
- Du kan aktivera din användare tooautomatically get inloggade tooLearnUpon (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LearnUpon, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En LearnUpon enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till LearnUpon från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-learnupon-from-hello-gallery"></a>Att lägga till LearnUpon från hello-galleriet
tooconfigure hello integrering av LearnUpon i Azure AD, behöver du tooadd LearnUpon hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LearnUpon från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LearnUpon**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. Markera hello resultat på panelen **LearnUpon**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LearnUpon baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LearnUpon är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LearnUpon toobe upprättas.

I LearnUpon, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med LearnUpon, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LearnUpon](#creating-a-learnupon-test-user)**  -toohave en motsvarighet för Britta Simon i LearnUpon som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LearnUpon program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LearnUpon:**

1. I hello Azure-portalen på hello **LearnUpon** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. På hello **LearnUpon domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värdet. du har tooupdate värdet med hello faktiska Reply-URL. tooget värdet Kontakta [LearnUpon supportteamet](https://www.learnupon.com/features/support/).



4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. På hello **LearnUpon Configuration** klickar du på **konfigurera LearnUpon** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Öppna en annan instans för webbläsaren och logga in till LearnUpon med ett administratörskonto. 

8. Klicka på hello **inställningar** fliken.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Klicka på **Single Sign On - SAML**, och klicka sedan på **allmänna inställningar** tooconfigure SAML-inställningar.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. I hello **allmänna inställningar** avsnittet, utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Välj **aktiverat**.

    b. Välj **Version** som **2.0**.

    c. Välj **hoppa över villkor** som **nr**.

    d. I hello **SAML-Token efter param namnet** textruta hello-typnamn för begäran post parametern toohello SAML konsument-URL som nämns ovan som innehåller hello SAML Assertion toobe verifieras och autentiserad – till exempel  **SAMLResponse**.

    e. I hello **identifierare namnformat** textruta typen hello-värde som anger där användarna SAML Assertion hello identifierare (e-postadress) finns – till exempel **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.
  
    f. I hello **identifiera leverantörsplatsen** textruta, typen hello-värde som anger om hello användare skickas tooif de Klicka på din överförda ikon från Azure portal inloggningsskärmen.
  
    g. I hello **logga ut URL** textruta klistra in hello **Sign-Out URL** som du har kopierat från hello Azure-portalen.
    
    h. Klicka på **hantera fingeravtrycksläsare utskrifter**, och sedan ladda upp hello fingeravtryck av hämtade certifikatet.

11. Klicka på **användarinställningar**, och utför sedan hello följande steg:
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. I hello **förnamn identifierare Format** textruta hello TYPVÄRDE som talar om för oss var i din SAML Assertion hello användare firstname finns – till exempel: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. I hello **senaste namnformat identifierare** textruta hello TYPVÄRDE som talar om för oss var i din SAML Assertion hello användare efternamn finns – till exempel: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-learnupon-test-user"></a>Skapa en testanvändare LearnUpon

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i LearnUpon. LearnUpon stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess LearnUpon om den inte finns. [Konfigurera Azure AD-Single Sign-On](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact [LearnUpon supportteamet](https://www.learnupon.com/features/support/). 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLearnUpon.

![Tilldela användare][200] 

**tooassign Britta Simon tooLearnUpon utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LearnUpon**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour LearnUpon programmet när du klickar på hello LearnUpon panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

