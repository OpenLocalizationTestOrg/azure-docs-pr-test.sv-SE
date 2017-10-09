---
title: "Självstudier: Azure Active Directory-integrering med Samanage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Samanage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8edc29f113b8088438618a731e97c0f4f155b9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Självstudier: Azure Active Directory-integrering med Samanage

I kursen får du lära dig hur toointegrate Samanage med Azure Active Directory (AD Azure).

Integrera Samanage med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSamanage
- Du kan aktivera din användare tooautomatically get inloggade tooSamanage (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Samanage, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Samanage enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Samanage från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-samanage-from-hello-gallery"></a>Att lägga till Samanage från hello-galleriet
tooconfigure hello integrering av Samanage i Azure AD, behöver du tooadd Samanage hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Samanage från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Samanage**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. Markera hello resultat på panelen **Samanage**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Samanage baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Samanage är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Samanage toobe upprättas.

I Samanage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Samanage, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Samanage](#creating-a-samanage-test-user)**  -toohave en motsvarighet för Britta Simon i Samanage som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Samanage program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Samanage:**

1. I hello Azure-portalen på hello **Samanage** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. På hello **Samanage domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare som beskrivs senare i hello kursen. För mer information kontaktar [Samanage klienten supportteamet](https://www.samanage.com/support).    
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. På hello **Samanage Configuration** klickar du på **konfigurera Samanage** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. Logga in på webbplatsen Samanage företag som en administratör i en annan webbläsarfönster.

8. Klicka på **instrumentpanelen** och välj **installationsprogrammet** i vänstra navigeringsfönstret.
   
    ![Instrumentpanelen](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "instrumentpanelen")

9. Klicka på **enkel inloggning**.
   
    ![Enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "enkel inloggning")

10. Navigera för**inloggningen med SAML** avsnittet, utföra hello följande steg:
   
    ![Logga in med SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "inloggningen med SAML")
 
    a. Klicka på **aktivera enkel inloggning med SAML**.  
 
    b. I hello **identitet providern URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.    
 
    c. Bekräfta hello **inloggnings-URL** matchar hello **logga URL** av **Samanage domän och URL: er** avsnitt i Azure-portalen.
 
    d. I hello **logga ut URL** textruta ange hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
 
    e. I hello **SAML utfärdaren** textruta typen hello app-id URI som angetts i din identitetsleverantör.
 
    f. Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **klistra in din identitetsleverantör x.509-certifikatet nedan** textruta.
 
    g. Klicka på **skapa användare om de inte finns i Samanage**.
 
    h. Klicka på **uppdatering**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-samanage-test-user"></a>Skapa en testanvändare Samanage

tooenable Azure AD-användare toolog i tooSamanage, måste de etableras i Samanage.  
Hello gäller Samanage är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in på webbplatsen Samanage företag som administratör.

2. Klicka på **instrumentpanelen** och välj **installationsprogrammet** i navigeringen till vänster Panorera.
   
    ![Installationsprogrammet](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "installationen")

3. Klicka på hello **användare** fliken
   
    ![Användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "användare")

4. Klicka på **ny användare**.
   
    ![Ny användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "ny användare")

5. Typen hello **namn** och hello **e-postadress** för ett Azure Active Directory-konto du vill tooprovision och på **skapa användare**.
   
    ![Skapa användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "skapa användare")
   
   >[!NOTE]
   >hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras. Du kan använda något annat Samanage användarens konto skapas verktyg eller API: er som tillhandahålls av Samanage tooprovision Azure Active Directory användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSamanage.

![Tilldela användare][200] 

**tooassign Britta Simon tooSamanage utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Samanage**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Samanage programmet när du klickar på hello Samanage panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

