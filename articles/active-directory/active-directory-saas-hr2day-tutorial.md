---
title: "Självstudier: Azure Active Directory-integrering med HR2day av Merces | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och HR2day av Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Självstudier: Azure Active Directory-integrering med HR2day av Merces

I kursen får du lära dig hur toointegrate HR2day av Merces med Azure Active Directory (AD Azure).

Integrera HR2day av Merces med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooHR2day av Merces.
- Du kan aktivera användarna tooautomatically hämta inloggad tooHR2day genom Merces med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats--hello Azure-portalen.

Mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med HR2day av Merces måste hello följande objekt:

- En Azure AD-prenumeration.
- En HR2day av Merces enkel inloggning aktiverad prenumeration.

> [!NOTE]
> Vi rekommenderar inte att du använder en produktion miljö tootest hello stegen i den här kursen.

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte din produktionsmiljö om det är nödvändigt.
- Hämta en [en månaders kostnadsfri utvärderingsversion av Azure AD](https://azure.microsoft.com/pricing/free-trial/) om det inte redan har.  

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs här består av två huvudsakliga byggblock:

1. Lägger till HR2day av Merces från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-hr2day-by-merces-from-hello-gallery"></a>Lägg till HR2day av Merces från hello-galleriet
tooconfigure hello integrering av HR2day av Merces i Azure AD och lägga till HR2day av Merces från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd HR2day av Merces från galleriet hello vidta hello följande steg:**

1. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd ett nytt program väljer hello **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **HR2day av Merces**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. Markera hello resultat på panelen **HR2day av Merces**, och välj sedan hello **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HR2day av Merces baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow som hello motsvarighet användaren i HR2day av Merces är tooa i Azure AD. Du behöver med andra ord tooestablish en länk mellan en Azure AD-användare och hello relaterade användare i HR2day av Merces.

Tilldela hello i HR2day av Merces **användarnamn** i Azure AD för **användarnamn** tooestablish hello relationen.

tooconfigure och testa Azure AD enkel inloggning med HR2day av Merces, behöver du toocomplete hello följande byggblock:

1. [Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on): aktivera din användare toouse den här funktionen.
2. [Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user): testa Azure AD enkel inloggning med Britta Simon.
3. [Skapa en HR2day av Merces testanvändare](#creating-an-hr2day-by-merces-test-user): skapa en motsvarighet för Britta Simon i HR2day av Merces som är länkade toohello Azure AD-representation av användaren.
4. [Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user): aktivera Britta Simon toouse Azure AD enkel inloggning.
5. [Testa enkel inloggning](#testing-single-sign-on): Kontrollera om hello konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din HR2day av Merces program.

**tooconfigure Azure AD enkel inloggning med HR2day av Merces, vidta hello följande steg:**

1. I hello Azure-portalen på hello **HR2day av Merces** programmet integration anger **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. tooenable enkel inloggning, i hello **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning**.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. I hello **HR2day Merces domänen och URL: er** avsnittet, vidta hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. I hello **inloggnings-URL** Skriv en URL med hjälp av hello följer mönstret: `https://<tenantname>.force.com/<instancename>`.

    b. I hello **identifierare** Skriv en URL med hjälp av hello följer mönstret: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) tooget dessa värden. 
 


4. På hello **SAML-signeringscertifikat** väljer **Certificate(Base64)**, och sedan spara hello certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. Det här avsnittet beskrivs hur tooenable användare tooauthenticate tooHR2day av Merces med sitt konto i Azure AD. De kan göra detta med hjälp av federation som baseras på hello SAML-protokoll.

    Din HR2day av Merces program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour SAML-token. hello följande skärmbild visar ett exempel på detta. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Innan du kan konfigurera hello SAML-kontroll måste du kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) och begära hello värdet för attributet för hello-Unik identifierare för din klient. Du behöver det här värdet toocomplete hello stegen i hello nästa avsnitt. 

6. I hello **enkel inloggning** i dialogrutan hello **användarattribut** avsnittet, konfigurera hello SAML-token attribut som visas i följande bild hello. Därefter börja hello följande steg.
    
      | Attributets namn    |   Attributvärdet |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | koppling ([e] ”102938475Z” ”, @” |
    
      a. tooopen hello **lägga till attributet** markerar **Lägg till attributet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    b. I hello **namn** skriver **ATTR_LOGINCLAIM**.

    c. Från hello **värdet** väljer **Join()**.

    d. Från hello **sträng1** väljer **user.mail**.

    e. För **sträng2**, ange hello Unik identifierare som tillhandahålls av din HR2day-teamet.

    f. I hello **avgränsare** skriver  **@** .
    
    g. Välj **Ok**.

7. Välj hello **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. I hello **HR2day Merces konfigurationen** väljer **konfigurera HR2day av Merces** tooopen hello **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens** avsnitt.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. tooconfigure enkel inloggning för ditt program bör du kontakta hello [HR2day av Merces klienten supportteamet](mailTo:servicedesk@merces.nl). Koppla hello hämtas **Certificate(Base64)** filen tooyour e-post. Ger också hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML inloggning tjänst-URL för enkel** så att de kan konfigureras för SSO-integrering.

    > [!NOTE]
    >Nämnt toohello Merces team som den här integreringen behöver hello enhets-ID toobe med hello mönster **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken. Sedan åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen i hello [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD ta hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper**, och välj sedan **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan **Lägg till** överst hello hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan vidta hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan, typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet**, och sedan skriva ned hello lösenord.

    d. Välj **Skapa**.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Skapa en HR2day av Merces testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i HR2day av Merces. tooadd hello hello HR2day kontot, arbeta med hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Om du behöver toocreate en användare manuellt kan kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl).

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooHR2day av Merces.

![Tilldela användare][200] 

**tooassign Britta Simon tooHR2day av Merces, vidta hello följande steg:**

1. I hello Azure portal, öppna hello program visa går toohello directory vy och sedan gå för**företagsprogram**. Välj därefter **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **HR2day av Merces**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. Välj hello menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Välj hello **Lägg till** knappen. Sedan hello **Lägg uppdrag** dialogrutan **användare och grupper**.

    ![Tilldela användare][203]

5. I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.

6. Klicka på hello **Välj** knappen.

7. I hello **Lägg uppdrag** dialogrutan **tilldela**.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.  

När du väljer hello HR2day av Merces panelen i hello åtkomstpanelen loggas du automatiskt hämta tooyour HR2day av Merces program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

