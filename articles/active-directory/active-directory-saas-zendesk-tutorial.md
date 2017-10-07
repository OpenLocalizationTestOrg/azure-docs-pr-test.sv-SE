---
title: "Självstudier: Azure Active Directory-integrering med Zendesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Självstudier: Azure Active Directory-integrering med Zendesk

I kursen får du lära dig hur toointegrate Zendesk med Azure Active Directory (AD Azure).

Integrera Zendesk med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooZendesk
- Du kan aktivera din användare tooautomatically get inloggade tooZendesk (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Zendesk, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Zendesk enkel inloggning aktiverad prenumeration


> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zendesk från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-zendesk-from-hello-gallery"></a>Att lägga till Zendesk från hello-galleriet
tooconfigure hello integrering av Zendesk i Azure AD, behöver du tooadd Zendesk hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Zendesk från galleriet hello utför hello följande steg:**

1. I hello ** [Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Zendesk**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Markera hello resultat på panelen **Zendesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zendesk baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zendesk är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zendesk toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zendesk.

tooconfigure och testa Azure AD enkel inloggning med Zendesk, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Zendesk](#creating-a-zendesk-test-user) ** -toohave en motsvarighet för Britta Simon i Zendesk som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zendesk-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zendesk:**

1. I hello Azure-portalen på hello **Zendesk** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. På hello **Zendesk domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.zendesk.com`

    b. I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och Identifierare. Kontakta [Zendesk supportteamet](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget dessa värden. 

4. Zendesk förväntar hello SAML intyg i ett specifikt format. Det finns inga obligatoriska SAML-attribut men om du vill kan du lägga till ett attribut från **användarattribut** avsnitt genom följande hello nedanstående steg: 

     ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klicka på hello **visa och redigera alla hello andra attribut** kryssrutan.
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Klicka på hello **lägga till attributet** tooopen **Lägg till attributet** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. I hello **namn** textruta typen hello attributets namn (till exempel **e-postadress**).
    
    d. Från hello **värdet** listan, Välj hello attributvärde (som **user.mail**).
    
    e. Klicka på **Ok**
 
    > [!NOTE] 
    > Du kan använda attribut tooadd tilläggsattribut som inte ingår i Azure AD som standard. Klicka på [användarattribut som kan anges i SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello fullständig lista över SAML attribut som **Zendesk** accepterar. 

5. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. På hello **Zendesk Configuration** klickar du på **konfigurera Zendesk** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. Logga in på webbplatsen Zendesk företag som en administratör i en annan webbläsarfönster.

8. Klicka på **Admin**.

9. Hello vänstra navigeringsfönstret, klicka på **inställningar**, och klicka sedan på **säkerhet**.

10. På hello **säkerhet** utför hello följande steg: 
   
     ![Säkerhet](./media/active-directory-saas-zendesk-tutorial/ic773089.png "säkerhet")

    ![Enkel inloggning](./media/active-directory-saas-zendesk-tutorial/ic773090.png "enkel inloggning")

     a. Klicka på hello **Admin & agenter** fliken.

     b. Välj **enkel inloggning (SSO) och SAML**, och välj sedan **SAML**.

     c. I **SAML SSO URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen. 

     d. I **Remote logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
        
     e. I **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.
     
     f. Klicka på **Spara**.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-a-zendesk-test-user"></a>Skapa en testanvändare Zendesk

tooenable Azure AD-användare toolog till **Zendesk**, de måste etableras i **Zendesk**.  
Beroende på hello roll som tilldelats i hello appar, är dess hello förväntat:

 1. **Slutanvändarens** konton tillhandahålls automatiskt när du loggar in.
 2. **Agenten** och **Admin** konton måste toobe manuellt etableras i **Zendesk** innan du loggar in.
 
**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **Zendesk** klient.

2. Välj hello **lista över kunder** fliken.

3. Välj hello **användare** och på **Lägg till**.
   
    ![Lägg till användare](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Lägg till användare")
4. Skriv hello e-postadress för ett befintligt Azure AD-konto du vill tooprovision och klicka sedan på **spara**.
   
    ![Ny användare](./media/active-directory-saas-zendesk-tutorial/ic773633.png "ny användare")

> [!NOTE]
> Du kan använda något annat Zendesk användarens konto skapas verktyg eller API: er som tillhandahålls av Zendesk tooprovision AAD-användarkonton.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZendesk.

![Tilldela användare][200] 

**tooassign Britta Simon tooZendesk utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zendesk**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Zendesk programmet när du klickar på hello Zendesk-panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
