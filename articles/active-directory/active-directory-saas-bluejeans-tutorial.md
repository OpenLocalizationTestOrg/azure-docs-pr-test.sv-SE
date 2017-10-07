---
title: "Självstudier: Azure Active Directory-integrering med BlueJeans | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Självstudier: Azure Active Directory-integrering med BlueJeans

I kursen får du lära dig hur toointegrate BlueJeans med Azure Active Directory (AD Azure).

Integrera BlueJeans med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBlueJeans
- Du kan aktivera din användare tooautomatically get inloggade tooBlueJeans (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med BlueJeans, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En BlueJeans enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till BlueJeans från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-bluejeans-from-hello-gallery"></a>Att lägga till BlueJeans från hello-galleriet
tooconfigure hello integrering av BlueJeans i Azure AD, behöver du tooadd BlueJeans hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd BlueJeans från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **BlueJeans**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Markera hello resultat på panelen **BlueJeans**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BlueJeans baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BlueJeans är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BlueJeans toobe upprättas.

I BlueJeans, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med BlueJeans, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare BlueJeans](#creating-a-bluejeans-test-user)**  -toohave en motsvarighet för Britta Simon i BlueJeans som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BlueJeans program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BlueJeans:**

1. I hello Azure-portalen på hello **BlueJeans** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. På hello **BlueJeans domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.BlueJeans.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [BlueJeans klienten supportteamet](https://support.bluejeans.com/contact) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. På hello **BlueJeans Configuration** klickar du på **konfigurera BlueJeans** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Logga in tooyour i ett annat webbläsarfönster **BlueJeans** företagets webbplats som administratör.

8. Gå för**ADMIN \> gruppinställningar \> säkerhet**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

9. I hello **säkerhet** avsnittet, utföra hello följande steg:
   
   ![SAML för enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML för enkel inloggning")   
   
   a. Välj **SAML för enkel inloggning**.
  
   b. Välj **aktivera automatisk etablering**.

10. Gå vidare med hello följande steg:

    ![Certifikat sökvägen](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikat sökväg")
    
    a. Klicka på **Välj fil**, och sedan ladda upp hello hämtat certifikat.
   
    b. Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **inloggnings-URL** textruta.
   
    c. Klistra in **ändra lösenord URL** till hello **lösenord ändra URL** textruta.
   
    d. Klistra in **Sign-Out URL** till hello **logga ut URL** textruta.

11. Gå vidare med hello följande steg:
    
    ![Spara ändringarna](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "spara ändringar")
    
    a. I hello **användar-id** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    b. I hello **e-post** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    c. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-bluejeans-test-user"></a>Skapa en testanvändare BlueJeans

tooenable Azure AD-användare toolog i tooBlueJeans, måste de etableras i BlueJeans.  

Vid BlueJeans är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour **BlueJeans** företagets webbplats som administratör.

2. Gå för**ADMIN \> hantera användare \> Lägg till användare**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")
   
   >[!IMPORTANT]
   >Hej **Lägg till användare** fliken är endast tillgänglig om i hello **säkerhetsfliken**, **aktivera automatisk etablering** är avmarkerad. 
   
3. I hello **Lägg till användare** avsnittet, utföra hello följande steg:

    ![Lägg till användare](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "lägga till användare")
    
    a. Ange en **BlueJeans användarnamn**, en **e-postadress**, **BlueJeans möte ID**, **kontrollant lösenord**, en **fullständigt namn** , hello **företagets** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.
    
    b. Klicka på **lägga till användare**.

>[!NOTE]
>Du kan använda något annat BlueJeans användarens konto skapas verktyg eller API: er som tillhandahålls av BlueJeans tooprovision AAD-användarkonton. 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlueJeans.

![Tilldela användare][200] 

**tooassign Britta Simon tooBlueJeans utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **BlueJeans**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello BlueJeans panelen i hello åtkomstpanelen bör du hämta inloggningssidan för BlueJeans program.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

