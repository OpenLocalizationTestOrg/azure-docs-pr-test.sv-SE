---
title: "Självstudier: Azure Active Directory-integrering med StatusPage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och StatusPage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 7c6717017984241e9e459273ead4b5e062311120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Självstudier: Azure Active Directory-integrering med StatusPage

I kursen får du lära dig hur toointegrate StatusPage med Azure Active Directory (AD Azure).

Integrera StatusPage med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooStatusPage
- Du kan aktivera din användare tooautomatically get inloggade tooStatusPage (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med StatusPage, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En StatusPage enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till StatusPage från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-statuspage-from-hello-gallery"></a>Att lägga till StatusPage från hello-galleriet
tooconfigure hello integrering av StatusPage i Azure AD, behöver du tooadd StatusPage hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd StatusPage från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **StatusPage**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. Markera hello resultat på panelen **StatusPage**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med StatusPage baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i StatusPage är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i StatusPage toobe upprättas.

I StatusPage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med StatusPage, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare StatusPage](#creating-a-statuspage-test-user)**  -toohave en motsvarighet för Britta Simon i StatusPage som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt StatusPage program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med StatusPage:**

1. I hello Azure-portalen på hello **StatusPage** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. På hello **StatusPage domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret: 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > Kontakta supportteamet för hello StatusPage på [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)toorequest metadata nödvändiga tooconfigure enkel inloggning. 
    >
    >a. Kopiera hello utfärdaren värde från hello metadata, och klistra in den i hello **identifierare** textruta.
    >
    >b. Kopiera hello Reply URL från hello metadata, och klistra in den i hello **Reply URL** textruta.

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. På hello **StatusPage Configuration** klickar du på **konfigurera StatusPage** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. Logga in tooyour StatusPage företagets webbplats som en administratör i ett nytt webbläsarfönster.

8. I hello verktygsfältet klickar du på **Hantera konto**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. Klicka på hello **enkel inloggning** fliken. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. På installationssidan för hello SSO, utför hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    a. I hello **mål-URL för SSO** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    b. Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **certifikat** textruta. 

    c. Klicka på **Spara konfiguration**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-statuspage-test-user"></a>Skapa en testanvändare StatusPage

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i StatusPage.

StatusPage stöder just-in-time-etablering. Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).

**toocreate en användare som kallas Britta Simon i StatusPage, utför följande steg hello:**

1. Inloggning tooyour StatusPage företagets webbplats som administratör.

2. Hello-menyn överst hello **Hantera konto**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. Klicka på hello **gruppmedlemmar** fliken. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. Klicka på **Lägg till TEAMMEDLEM**. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. Typen hello **e-postadress**, **Förnamn**, och **Sur namn** av en giltig användare du vill använda tooprovision hello relaterade textrutor. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. Som **rollen**, Välj **administratör**.

7. Klicka på **skapa konto**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooStatusPage.

![Tilldela användare][200] 

**tooassign Britta Simon tooStatusPage utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **StatusPage**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour StatusPage programmet när du klickar på hello StatusPage panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

