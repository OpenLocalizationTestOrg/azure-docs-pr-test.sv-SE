---
title: "Självstudier: Azure Active Directory-integrering med Freshservice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Freshservice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Självstudier: Azure Active Directory-integrering med Freshservice

I kursen får du lära dig hur toointegrate Freshservice med Azure Active Directory (AD Azure).

Integrera Freshservice med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooFreshservice
- Du kan aktivera din användare tooautomatically get inloggade tooFreshservice (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Freshservice, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Freshservice enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Freshservice från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-freshservice-from-hello-gallery"></a>Att lägga till Freshservice från hello-galleriet
tooconfigure hello integrering av Freshservice i Azure AD, behöver du tooadd Freshservice hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Freshservice från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Freshservice**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. Markera hello resultat på panelen **Freshservice**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Freshservice baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Freshservice är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Freshservice toobe upprättas.

I Freshservice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Freshservice, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Freshservice](#creating-a-freshservice-test-user)**  -toohave en motsvarighet för Britta Simon i Freshservice som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Freshservice program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Freshservice:**

1. I hello Azure-portalen på hello **Freshservice** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. På hello **Freshservice domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<democompany>.freshservice.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Freshservice klienten supportteamet](https://support.freshservice.com/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnittet, kopiera **TUMAVTRYCKET** värdet för certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. På hello **Freshservice Configuration** klickar du på **konfigurera Freshservice** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. Logga in tooyour Freshservice företagets webbplats som en administratör i en annan webbläsarfönster.

8. Hello-menyn överst hello **Admin**.
   
    ![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")

9. I hello **Kundportal**, klickar du på **säkerhet**.
   
    ![Säkerhet](./media/active-directory-saas-freshservice-tutorial/ic790815.png "säkerhet")

10. I hello **säkerhet** avsnittet, utföra hello följande steg:
   
    ![Enkel inloggning](./media/active-directory-saas-freshservice-tutorial/ic790816.png "enkel inloggning")
   
    a. Växeln **enkel inloggning**.

    b. Välj **SAML SSO**.

    c. I hello **inloggnings-URL för SAML** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    d. I hello **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.

    e. I **säkerhet certifikat fingeravtryck** textruta klistra in hello **TUMAVTRYCKET** värdet för certifikat som du har kopierat från Azure-portalen.

    f. Klicka på **spara**
   
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-freshservice-test-user"></a>Skapa en testanvändare Freshservice

tooenable Azure AD-användare toolog i tooFreshService, måste de etableras i FreshService. Hello gäller FreshService är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **FreshService** företagets webbplats som administratör.

2. Hello-menyn överst hello **Admin**.
   
    ![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")

3. I hello **Användarhantering** klickar du på **beställare**.
   
    ![Beställare](./media/active-directory-saas-freshservice-tutorial/ic790818.png "beställare")

4. Klicka på **ny frågeställare**.
   
    ![Nya beställare](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nya beställare")

5. I hello **ny frågeställare** avsnittet, utföra hello följande steg:
   
    ![Ny frågeställare](./media/active-directory-saas-freshservice-tutorial/ic790820.png "ny frågeställare")   

    a. Ange hello **Förnamn** och **e-post** attribut för ett giltigt Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.

    b. Klicka på **Spara**.
   
    >[!NOTE]
    >hello Azure Active Directory användare får ett e-postmeddelande inklusive en länk tooconfirm hello kontot innan det blir aktiv
    >  

>[!NOTE]
>Du kan använda något annat FreshService användarens konto skapas verktyg eller API: er som tillhandahålls av FreshService tooprovision AAD-användarkonton.
>  

![Tilldela användare][200] 

**tooassign Britta Simon tooFreshservice utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Freshservice**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Freshservice programmet när du klickar på hello Freshservice panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

