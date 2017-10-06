---
title: "Självstudier: Azure Active Directory-integrering med Marketo | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Självstudier: Azure Active Directory-integrering med Marketo

I kursen får du lära dig hur toointegrate Marketo med Azure Active Directory (AD Azure).

Integrera Marketo med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooMarketo
- Du kan aktivera din användare tooautomatically get inloggade tooMarketo (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Marketo, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Marketo enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Marketo från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-marketo-from-hello-gallery"></a>Att lägga till Marketo från hello-galleriet
tooconfigure hello integrering av Marketo i Azure AD, behöver du tooadd Marketo hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Marketo från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Marketo**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. Markera hello resultat på panelen **Marketo**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Marketo baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Marketo är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Marketo toobe upprättas.

I Marketo, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Marketo, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Marketo](#creating-a-marketo-test-user)**  -toohave en motsvarighet för Britta Simon i Marketo som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Marketo-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Marketo:**

1. I hello Azure-portalen på hello **Marketo** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. På hello **Marketo domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://saml.marketo.com/sp`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [Marketo supportteamet](http://investors.marketo.com/contactus.cfm) tooget dessa värden.
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. På hello **Marketo Configuration** klickar du på **konfigurera Marketo** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. tooget Munchkin Id för programmet, logga in med administratörsautentiseringsuppgifter tooMarketo och utföra följande åtgärder:
   
    a. Logga in tooMarketo app med administratörsautentiseringsuppgifter.
   
    b. Klicka på hello **Admin** knappen hello övre navigeringsfönstret.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigera toohello integrering-menyn och klicka på hello **Munchkin länk**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    d. Kopiera hello Munchkin-Id som visas på hello-skärmen och slutföra Reply-URL i guiden för konfiguration av hello Azure AD.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. tooconfigure hello SSO i hello program så hello nedanstående steg:
   
    a. Logga in tooMarketo app med administratörsautentiseringsuppgifter.
   
    b. Klicka på hello **Admin** knappen hello övre navigeringsfönstret.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigera toohello integrering-menyn och klicka på **enkel inloggning**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    d. tooenable hello SAML-inställningar, klickar du på **redigera** knappen.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Aktiverad** inställningar för enkel inloggning.
   
    f. Klistra in hello **SAML enhets-ID**, i hello **utfärdaren ID** textruta.
   
    g. I hello **enhets-ID** textruta ange hello URL: en som `http://saml.marketo.com/sp`.
   
    h. Välj hello plats-ID som **namnidentifierare elementet**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Om ditt användar-ID inte UPN-värde kan ändra hello värdet hello attribut på fliken.
   
    Jag. Överför hello-certifikatet som du har hämtat från Azure AD-konfigurationsguiden. **Spara** hello inställningar.
   
    j. Redigera inställningar för hello omdirigera felsidor.
   
    k. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** i hello **inloggnings-URL** textruta.
   
    l. Klistra in hello **Sign-Out URL** i hello **logga ut URL** textruta.
   
    m. I hello **fel URL**, kopiera ditt **Marketo instans-URL** och på **spara** knappen toosave inställningar.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. tooenable hello enkel inloggning för användare, fullständig hello följande åtgärder:
   
    a. Logga in tooMarketo app med administratörsautentiseringsuppgifter.
   
    b. Klicka på hello **Admin** knappen hello övre navigeringsfönstret.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigera toohello **säkerhet** -menyn och klicka på **inloggningsinställningar**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    d. Kontrollera hello **kräver SSO** alternativet och **spara** hello inställningar.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-marketo-test-user"></a>Skapa en testanvändare Marketo

I det här avsnittet skapar du en användare som kallas Britta Simon i Marketo. Följ dessa steg toocreate en användare i Marketo-plattformen.

1. Logga in tooMarketo app med administratörsautentiseringsuppgifter.

2. Klicka på hello **Admin** knappen hello övre navigeringsfönstret.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. Navigera toohello **säkerhet** -menyn och klicka på **användare och roller**
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. Klicka på hello **bjuda in nya användare** länk på fliken för hello-användare
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. I följande information hello bjuda in nya användare guiden fill hello
   
    a. Ange hello användare **e-post** adress i hello textruta
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    b. Ange hello **Förnamn** i hello textruta
   
    c. Ange hello **efternamn** i hello textruta
   
    d. Klicka på **Nästa**

6. I hello **behörigheter** fliken, väljer hello **userRoles** och på **nästa**
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. Klicka på hello **skicka** knappen toosend hello användarinbjudan
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Användaren får hello e-postmeddelande och har tooclick hello länka och ändra hello lösenord tooactivate hello konto. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMarketo.

![Tilldela användare][200] 

**tooassign Britta Simon tooMarketo utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Marketo**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Marketo programmet när du klickar på hello Marketo-panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

