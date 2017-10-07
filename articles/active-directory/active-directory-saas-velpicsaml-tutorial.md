---
title: "Självstudier: Azure Active Directory-integrering med Velpic SAML | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Velpic SAML."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Självstudier: Azure Active Directory-integrering med Velpic SAML

I kursen får du lära dig hur toointegrate Velpic SAML med Azure Active Directory (AD Azure).

Integrera Velpic SAML med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooVelpic SAML
- Du kan aktivera din användare tooautomatically get inloggade tooVelpic SAML (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Velpic SAML måste hello följande objekt:

- En Azure AD-prenumeration
- En Velpic SAML enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Velpic SAML från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-velpic-saml-from-hello-gallery"></a>Att lägga till Velpic SAML från hello-galleriet
tooconfigure hello integrering av Velpic SAML i Azure AD, behöver du tooadd Velpic SAML hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Velpic SAML från galleriet hello utför hello följande steg:**

1. I hello ** [Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Velpic SAML**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. Markera hello resultat på panelen **Velpic SAML**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med Velpic SAML baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Velpic SAML är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Velpic SAML toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Velpic SAML.

tooconfigure och testa Azure AD enkel inloggning med Velpic SAML, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Velpic SAML](#creating-a-velpic-saml-test-user) ** -toohave en motsvarighet för Britta Simon i Velpic SAML som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Velpic SAML-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Velpic SAML:**

1. I hello Azure Management portal på hello **Velpic SAML** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Ange hello detaljer i hello **Velpic SAML domän och URL: er** avsnittet -

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://<sub-domain>.velpicsaml.net`

    b. I hello **identifierare** textruta klistra in hello **'Enkel inloggning på URL'** värde`https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Observera att hello URL: en inloggning kommer att tillhandahållas av hello Velpic SAML-teamet och identifierarvärde blir tillgänglig när du konfigurerar hello SSO-plugin-programmet på Velpic SAML-sida. Du måste toocopy som värdet från Velpic SAML appen på sidan och klistra in den här.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Öppna hello Velpic SAML-konfigurationsavsnittet, klicka på Konfigurera Velpic SAML tooopen konfigurera inloggning. Kopiera hello SAML enhets-ID från hello Snabbreferens avsnitt.

7. Logga in på webbplatsen Velpic SAML företag som en administratör i en annan webbläsarfönster.

8. Klicka på **hantera** fliken och gå för**integrering** avsnitt där du behöver tooclick på **plugin-program** knappen toocreate nytt plugin-program för inloggning.

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Klicka på hello **'Lägga till plugin-programmet'** knappen.
    
    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Klicka på hello **SAML** panelen i hello lägga till Plugin-sida.
    
    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Ange hello av hello nytt SAML plugin-program och klicka på hello **'Add-** knappen.

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Ange hello information på följande sätt:

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. I hello **namn** textruta hello-typnamn för SAML-plugin-programmet.

    b. I hello **utfärdar-URL** textruta klistra in hello **SAML enhets-ID** du kopierade från hello **konfigurera inloggning** fönster för hello Azure-portalen.

    c. I hello **providern Metadata Config** överför hello Metadata XML-fil som du hämtade från Azure-portalen.

    d. Du kan också välja tooenable SAML precis i tid etablering genom att aktivera hello **'Automatiskt skapa nya användare'** kryssrutan. Om en användare finns inte i Velpic och den här flaggan är inte aktiverad, misslyckas hello inloggningen från Azure. Om hello flaggan är aktiverad hello användaren kommer automatiskt att etableras i Velpic när hello inloggningen. 

    e. Kopiera hello **enkel inloggning på URL: en** från hello textrutan och klistra in den i hello Azure-portalen.
    
    f. Klicka på **Spara**.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Skapa en testanvändare Velpic SAML

Det här steget krävs vanligtvis inte eftersom programmet hello stöder bara i tid användaretablering. Om hello automatisk användaretablering inte är aktiverat kan sedan skapa manuella användare göras som beskrivs nedan.

Logga in på webbplatsen Velpic SAML företag som administratör och utför följande steg:
    
1. Klickar du på Hantera och gå tooUsers avsnittet, och klicka sedan på nya knappen tooadd användare.

    ![Lägg till användare](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. På hello **”skapa nya användare”** dialogrutan utför hello följande steg.

    ![Användaren](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. I hello **Förnamn** textruta typen hello förnamn Britta Simon.

    b. I hello **efternamn** textruta Skriv hello efternamn Britta Simon.

    c. I hello **användarnamn** textruta typen hello användarnamn i Britta Simon.

    d. I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.

    e. Resten av hello information är valfritt, kan du fylla i den vid behov.
    
    f. Klicka på **SPARA**.  

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooVelpic SAML.

![Tilldela användare][200] 

**tooassign Britta Simon tooVelpic SAML utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Velpic SAML**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

1. När du klickar på hello Velpic SAML-panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Velpic SAML-program. Du bör se hello **”logga in med Azure AD-** hello inloggningssidan-knappen.

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Klicka på hello **”logga in med Azure AD-** knappen toolog i tooVelpic med hjälp av Azure AD-kontot.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

