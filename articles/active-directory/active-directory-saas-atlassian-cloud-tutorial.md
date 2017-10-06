---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Atlassian | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Atlassian moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Självstudier: Azure Active Directory-integrering med Atlassian moln

I kursen får du lära dig hur toointegrate Atlassian moln med Azure Active Directory (AD Azure).

Integrera Atlassian moln med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAtlassian moln
- Du kan aktivera din användare tooautomatically get inloggade tooAtlassian molnet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Atlassian molnet, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Atlassian enkel inloggning aktiverad molnprenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Atlassian moln från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-atlassian-cloud-from-hello-gallery"></a>Att lägga till Atlassian moln från hello-galleriet
tooconfigure hello integrering av Atlassian moln i Azure AD, behöver du tooadd Atlassian moln hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Atlassian moln från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Atlassian moln**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. Markera hello resultat på panelen **Atlassian moln**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Atlassian molnet baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Atlassian molnet är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Atlassian molnet toobe upprätta.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Atlassian molnet.

tooconfigure och testa Azure AD enkel inloggning med Atlassian molnet, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Atlassian moln](#creating-an-atlassian-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Atlassian moln som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Atlassian moln.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Atlassian molnet:**

1. I hello Azure-portalen på hello **Atlassian moln** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. På hello **Atlassian moln domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.atlassian.net/admin/saml/edit`

    b. I hello **Reply URL** textruta Skriv en URL som:`https://id.atlassian.com/login/saml/acs`

4. Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och inloggnings-URL. Du kan hämta hello exakta värden från Atlassian Molnkonfigurationen för SAML-skärmen.
 
5. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. På hello **Atlassian Molnkonfigurationen** klickar du på **konfigurera Atlassian moln** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. tooget SSO konfigurerats för tillämpningsprogrammet inloggningen toohello Atlassian Portal med hello administratörsbehörighet.

8. Klicka på under hello autentisering i hello vänster navigeringsfält **domäner**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    a. Skriv ditt domännamn i hello textrutan och klicka på **Lägg till domän**.
        
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    b. tooverify hello domän, klickar du på **Kontrollera**. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    c. Hämta hello verifiering HTML-fil, ladda upp den toohello rotmappen för din domän webbplatsen och klicka sedan på **verifiera domän**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    d. När hello domän har verifierats hello värdet för hello **Status** fältet är **verifierad**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. I hello vänstra navigeringsfältet klickar du på **SAML**.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. Skapa en SAML-konfiguration och Lägg till hello identitet Providerkonfiguration.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. I hello **identitetsleverantör enhets-ID** klistra in hello värdet för textrutan **SAML enhets-ID** som du har kopierat från Azure-portalen.

    b. I hello **identitetsleverantör SSO URL** klistra in hello värdet för textrutan **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    c. Öppna hello hämtat certifikat från Azure portal och kopiera hello värden utan hello Begin och avsluta rader och klistra in den i hello **offentliga X509 certifikat** rutan.
    
    d. Klicka på **spara konfigurationen** tooSave hello inställningar.
     
11. Uppdatera hello Azure AD-inställningar toomake till att du har installationen hello korrigera Identifierare.
  
    a. Kopiera hello **SP identitet ID** från hello SAML skärmen och klistra in den i Azure AD som hello **identifierare** värde.

    b. Inloggning på URL: en är hello klient-URL för ditt Atlassian moln.     

     ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. I hello Azure-portalen klickar du på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-atlassian-cloud-test-user"></a>Skapa en testanvändare Atlassian moln

tooenable Azure AD-användare toolog i tooAtlassian moln, måste de etableras i Atlassian moln.  
Vid Atlassian moln är att etablera en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. I hello administrationsavsnittet för platsen, klickar du på hello **användare** knappen

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klicka på hello **skapa användare** knappen toocreate en användare i hello Atlassian moln

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Ange hello användare **e-postadress**, **användarnamn**, och **fullständiga namn** och tilldela hello programåtkomst. 

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klicka på **skapa användare** knappen, skickas hello inbjudan toohello användare och efter accepterar hello inbjudan hello användaren kommer att vara aktiv i hello system. 

>[!NOTE] 
>Du kan också skapa grupp användare för hello genom att klicka på hello **Bulk skapa** knapp i hello avsnittet användare.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAtlassian moln.

![Tilldela användare][200] 

**tooassign Britta Simon tooAtlassian moln, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Atlassian moln**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.

När du klickar på hello Atlassian moln panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Atlassian molnapp. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

