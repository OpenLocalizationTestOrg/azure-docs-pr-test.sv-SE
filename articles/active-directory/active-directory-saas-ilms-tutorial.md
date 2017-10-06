---
title: "Självstudier: Azure Active Directory-integrering med iLMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Självstudier: Azure Active Directory-integrering med iLMS

I kursen får du lära dig hur toointegrate iLMS med Azure Active Directory (AD Azure).

Integrera iLMS med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooiLMS
- Du kan aktivera din användare tooautomatically get inloggade tooiLMS (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med iLMS, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En iLMS enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till iLMS från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-ilms-from-hello-gallery"></a>Att lägga till iLMS från hello-galleriet
tooconfigure hello integrering av iLMS i Azure AD, behöver du tooadd iLMS hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd iLMS från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **iLMS**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Markera hello resultat på panelen **iLMS**, klicka på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med iLMS baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i iLMS är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i iLMS toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i iLMS.

tooconfigure och testa Azure AD enkel inloggning med iLMS, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare iLMS](#creating-an-ilms-test-user)**  -toohave en motsvarighet för Britta Simon i iLMS som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt iLMS program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med iLMS:**

1. I hello Azure-portalen på hello **iLMS** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. På hello **iLMS domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. I hello **identifierare** textruta klistra in hello **identifierare** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal.

    b. I hello **Reply URL** textruta klistra in hello **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal med hello följande mönstret`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Den här 123456 är en exempel-värdet för identifieraren.

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    I hello **inloggnings-URL** textruta klistra in hello **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML inställningarna iLMS administrationsportalen som`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

5. tooenable JIT-etablering, iLMS program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Skapa **avdelning, Region** och **Division** attribut och lägga till hello namn av dessa attribut i iLMS. Dessa attribut som visas ovan krävs.    

    > [!NOTE] 
    > Du har tooenable **skapa Un-recognized användarkonto** i iLMS toomap attributen. Följ instruktionerna för hello [här](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget en idé på hello attribut konfiguration.

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | ---------------| --------------- |    
    | Division | User.Department |
    | Region | User.state |
    | Avdelning | User.jobtitle |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**

7. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. Logga in tooyour i ett annat webbläsarfönster **iLMS administrationsportalen** som administratör.

10. Klicka på **SSO:SAML** under **inställningar** tooopen SAML-inställningar på fliken och utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Expandera hello **tjänstleverantör** avsnitt och kopiera hello **identifierare** och **slutpunkt (URL)** värde.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. Under **identitetsleverantör** klickar du på **importera Metadata**.
    
    c. Välj hello **Metadata** hämta filen från Azure-portalen från **SAML-signeringscertifikat** avsnitt.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Om du vill tooenable JIT etablering toocreate iLMS konton för un-känna igen användare, följa nedanstående steg:
        
       - Kontrollera **skapa icke godkänd användarkonto**.
       
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Mappa hello attribut i Azure AD med hello attribut i iLMS. Ange hello attribut namn eller hello standardvärdet i hello attributkolumnen.

    e. Gå för**affärsregler** fliken och utföra hello följande steg: 
        
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/5.png)

       - Kontrollera **skapa Un-recognized regioner, avdelningar och avdelningar** toocreate regioner, avdelningar och avdelningar som inte redan finns för närvarande hello av enkel inloggning.
        
       - Kontrollera **uppdatering användarens profil under inloggning** toospecify om hello användarens profil uppdateras med varje enkel inloggning. 
        
       - Om hello **”uppdatering tomma värden för ej obligatoriska fält i användarprofil”** alternativet är markerat, valfri profil fält som är tom vid inloggning kommer även att hello iLMS användarprofil toocontain tomma värden för fälten.
        
       - Kontrollera **skicka fel e-postmeddelande** och ange hello e-postadress för hello användare där du vill att tooreceive hello fel e-postmeddelandet.

11. Klicka på **spara** knappen toosave hello inställningar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-ilms-test-user"></a>Skapa en testanvändare iLMS

Programmet stöder bara i tid användaretablering och authentication-användare skapas i hello program automatiskt. JIT fungerar om du har klickat på hello **skapa Un-recognized användarkonto** kryssrutan under SAML Konfigurationsinställningen på administrationsportalen för iLMS.

Följ nedanstående steg om du behöver toocreate en användare manuellt:

1. Logga in tooyour iLMS företagets webbplats som administratör.

2. Klicka på **”registrera användare”** under **användare** fliken tooopen **registrera användaren** sidan. 
   
   ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/3.png)

3. På hello **”registrera användare”** utför hello följande steg.

    ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. I hello **Förnamn** textruta typnamn hello första Britta.
   
    b. I hello **efternamn** textruta typen hello efternamn Simon.

    c. I hello **e-post-ID** textruta typen hello e-postadress för Britta Simon konto.

    d. I hello **Region** listrutan, Välj hello värde för region.

    e. I hello **Division** listrutan, Välj hello värde för delning.

    f. I hello **avdelning** listrutan, Välj hello värde för avdelning.

    g. Klicka på **Spara**.

    > [!NOTE] 
    > Du kan skicka e-post toouser för registrering genom att välja **skicka e-post från registrering** kryssrutan.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooiLMS sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooiLMS utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **iLMS**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour iLMS programmet när du klickar på hello iLMS panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

