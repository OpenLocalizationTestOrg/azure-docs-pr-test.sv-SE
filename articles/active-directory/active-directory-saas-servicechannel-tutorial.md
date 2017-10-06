---
title: "Självstudier: Azure Active Directory-integrering med ServiceChannel | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ServiceChannel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 956371a1e99dcba4137c271ecfe8a62b9ec64a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a>Självstudier: Azure Active Directory-integrering med ServiceChannel

I kursen får du lära dig hur toointegrate ServiceChannel med Azure Active Directory (AD Azure).

Integrera ServiceChannel med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooServiceChannel
- Du kan aktivera din användare tooautomatically get inloggade tooServiceChannel (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ServiceChannel, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En ServiceChannel enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ServiceChannel från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-servicechannel-from-hello-gallery"></a>Att lägga till ServiceChannel från hello-galleriet
tooconfigure hello integrering av ServiceChannel i Azure AD, behöver du tooadd ServiceChannel hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ServiceChannel från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **ServiceChannel**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. Markera hello resultat på panelen **ServiceChannel**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ServiceChannel baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ServiceChannel är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ServiceChannel toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ServiceChannel.

tooconfigure och testa Azure AD enkel inloggning med ServiceChannel, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ServiceChannel](#creating-a-servicechannel-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt ServiceChannel-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ServiceChannel:**

1. I hello Azure Management portal på hello **ServiceChannel** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. På hello **ServiceChannel domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    a. I hello **identifierare** textruta hello TYPVÄRDE som:`http://adfs.<domain>.com/adfs/service/trust`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<customer domain>.servicechannel.com/saml/acs`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL. Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare. Kontakta [ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us) tooget dessa värden.

4. Tillämpningsprogrammet ServiceChannel förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration. hello följande skärmbild visar ett exempel för det här. **NameIdentifier (användar-ID)** är hello endast obligatoriska anspråk och hello standardvärdet är **user.userprincipalname** men ServiceChannel förväntar sig den här toobe som mappats till **user.mail**. Om du planerar tooenable bara In Time användaretablering, bör du lägga till hello följande anspråk som visas nedan. **Rollen** anspråk måste mappas för toobe**user.assignedroles** som innehåller hello roll hello användare.  

    Du kan se ServiceChannel guide [här](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) för mer information om anspråk.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > Klicka på [här](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow hur tooconfigure **rollen** i Azure AD

5. I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.

    | Attributets namn | Attributvärdet |
    | --- | --- |    
    | Roll| User.assignedroles |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**
    
6. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. Klicka på **Spara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. På hello **ServiceChannel Configuration** klickar du på **konfigurera ServiceChannel** tooopen **konfigurera inloggning** fönster. Observera hello **SAML Enitity ID** från hello **Snabbreferens** avsnitt.

9. tooconfigure enkel inloggning på **ServiceChannel** sida, behöver du toosend hello hämtas **certifikat (Base64)** och **SAML enhets-ID** för[ ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us). De ska ställa in detta i ordning toohave hello SAML SSO anslutningen korrekt på båda sidor.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-a-servicechannel-test-user"></a>Skapa en testanvändare ServiceChannel

Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt. För fullständig användaretablering, kontakta [ServiceChannel-teamet](https://servicechannel.zendesk.com/hc/en-us)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooServiceChannel sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooServiceChannel utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ServiceChannel**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour ServiceChannel programmet när du klickar på hello ServiceChannel-panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png