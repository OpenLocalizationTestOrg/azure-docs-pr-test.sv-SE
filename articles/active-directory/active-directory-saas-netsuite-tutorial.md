---
title: "Självstudier: Azure Active Directory-integrering med Netsuite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Självstudier: Azure Active Directory-integrering med Netsuite

I kursen får du lära dig hur toointegrate Netsuite med Azure Active Directory (AD Azure).

Integrera Netsuite med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooNetsuite
- Du kan aktivera din användare tooautomatically get inloggade tooNetsuite (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Netsuite, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Netsuite enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Netsuite från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-netsuite-from-hello-gallery"></a>Att lägga till Netsuite från hello-galleriet
tooconfigure hello integrering av Netsuite i Azure AD, behöver du tooadd Netsuite hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Netsuite från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Netsuite**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. Markera hello resultat på panelen **Netsuite**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Netsuite baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Netsuite är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Netsuite toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Netsuite.

tooconfigure och testa Azure AD enkel inloggning med Netsuite, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Netsuite](#creating-a-netsuite-test-user)**  -toohave en motsvarighet för Britta Simon i Netsuite som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Netsuite program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Netsuite:**

1. I hello Azure-portalen på hello **Netsuite** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. På hello **Netsuite domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Det här värdet är inte verkliga värde. Hello uppdateringsvärde med hello faktiska Reply-URL. Kontakta [Netsuite supportteamet](http://www.netsuite.com/portal/services/support.shtml) tooget det här värdet.
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. På hello **Netsuite Configuration** klickar du på **konfigurera Netsuite** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. Öppna en ny flik i webbläsaren och logga in på webbplatsen Netsuite företag som administratör.

8. I hello verktygsfältet hello överst på hello sidan klickar du på **installationsprogrammet**, klicka på **installationsprogrammet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. Från hello **installationsaktiviteter** väljer **integrering**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. I hello **hantera autentisering** klickar du på **SAML enkel inloggning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. På hello **SAML installationsprogrammet** utför hello följande steg:
   
    a. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** värde från **Snabbreferens** avsnitt i **konfigurera inloggning** och klistra in den i hello **identitetsleverantören. Inloggningssidan** i Netsuite.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    b. Välj i Netsuite, **primära autentiseringsmetod**.

    c. För fältet hello **SAMLV2 identitet providern Metadata**väljer **överför IDP metadatafil**. Klicka på **Bläddra** tooupload hello metadatafilen som du hämtade från Azure-portalen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    d. Klicka på **skicka**.

12. I Azure AD, klickar du på **visa och redigera andra användarattribut** kryssrutan och Lägg till attribut.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. För hello **attributnamn** anger i `account`. För hello **attributvärdet** anger i din Netsuite konto-ID. Det här värdet är konstant och ändra med kontot. Instruktioner om hur toofind konto-ID ingår nedan:

      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    a. Klicka på Netsuite, **installationsprogrammet** hello övre navigeringsfältet-menyn.

    b. Klicka på under hello **installationsaktiviteter** hello vänstra navigeringsfönstret-menyn, Välj hello **integrering** avsnittet och klicka på **Web Services-inställningar**.

    c. Kopiera Netsuite konto-ID och klistra in den i hello **attributvärdet** i Azure AD.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. Innan användarna kan utföra enkel inloggning till Netsuite, måste de först tilldelas hello lämpliga behörigheter i Netsuite. Följ instruktionerna för hello nedan tooassign dessa behörigheter.

    a. På menyn övre navigeringsfältet hello **installationsprogrammet**, klicka på **installationsprogrammet**.
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    b. Välj på menyn vänstra navigeringsfönstret hello **användare och roller för**, klicka på **hantera roller**.
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    c. Klicka på **ny roll**.

    d. Ange en **namn** för nya rollen och välj hello **enkel inloggning endast** kryssrutan.
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    e. Klicka på **Spara**.

    f. Hello-menyn överst hello **behörigheter**. Klicka på **installationsprogrammet**.
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    g. Välj **ställa in SAM Single Sign-on**, och klicka sedan på **Lägg till**.

    h. Klicka på **Spara**.

    Jag. På menyn övre navigeringsfältet hello **installationsprogrammet**, klicka på **installationsprogrammet**.
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    j. Välj på menyn vänstra navigeringsfönstret hello **användare och roller för**, klicka på **hantera användare**.
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    k. Välj en testanvändare. Klicka på **redigera**.
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    l. Välj hello roll som du har skapat och klicka på hello roller dialogrutan **Lägg till**.
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    m. Klicka på **Spara**.
    
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-a-netsuite-test-user"></a>Skapa en testanvändare Netsuite

I det här avsnittet skapas en användare som kallas Britta Simon i Netsuite. Netsuite stöder just-in-time-allokering som är aktiverad som standard.
Det finns ingen åtgärd objekt i det här avsnittet. Om en användare inte redan finns i Netsuite, skapas en ny när du försöker tooaccess Netsuite.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNetsuite.

![Tilldela användare][200] 

**tooassign Britta Simon tooNetsuite utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Netsuite**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

tootest enkel inloggning inställningarna, öppna hello åtkomstpanelen på [https://myapps.microsoft.com](https://myapps.microsoft.com/), logga in på hello testkonto och på **Netsuite**.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

