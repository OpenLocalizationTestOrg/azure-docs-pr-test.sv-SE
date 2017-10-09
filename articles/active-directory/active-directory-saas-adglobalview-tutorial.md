---
title: "Självstudier: Azure Active Directory-integrering med ADP Globalview | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ADP Globalview."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: aee2d56f05b486d12facbc41c9503455094604ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Självstudier: Azure Active Directory-integrering med ADP Globalview

I kursen får du lära dig hur toointegrate ADP Globalview med Azure Active Directory (AD Azure).

Integrera ADP Globalview med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooADP Globalview
- Du kan aktivera din användare tooautomatically get inloggade tooADP Globalview (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ADP Globalview måste hello följande objekt:

- En Azure AD-prenumeration
- En ADP Globalview enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ADP Globalview från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adp-globalview-from-hello-gallery"></a>Att lägga till ADP Globalview från hello-galleriet
tooconfigure hello integrering av ADP Globalview i Azure AD, behöver du tooadd ADP Globalview hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ADP Globalview från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **ADP Globalview**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. Markera hello resultat på panelen **ADP Globalview**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ADP Globalview baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ADP Globalview är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ADP Globalview toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ADP Globalview.

tooconfigure och testa Azure AD enkel inloggning med ADP Globalview, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ADP Globalview](#creating-an-adp-globalview-test-user)**  -toohave en motsvarighet för Britta Simon i ADP Globalview som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ADP Globalview.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ADP Globalview:**

1. I hello Azure-portalen på hello **ADP Globalview** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. På hello **ADP Globalview domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<subdomain>.globalview.adp.com/federate` eller`https://<subdomain>.globalview.adp.com/federate2`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Uppdatera hello värdet med hello faktiska identifierare. Kontakta [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) tooget hello värde.
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. hello ADP GlobalView program förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration. 

6. hello följande skärmbild visar ett exempel för den. hello anspråk namn alltid vara **”PersonImmutableID”** och hello-värde som vi har mappat tooExtensionAttribute2 som innehåller hello EmployeeID för hello användare. Här hello mappning från Azure AD tooADP GlobalView görs på hello EmployeeID, men du kan mappa den tooa annat värde också baserat på dina inställningar för programmet. Du kan arbeta med hello ADP GlobalView team första toouse hello rätt ID för en användare och mappa värdet med hello **”PersonImmutableID”** anspråk. Du kan också mappa hello e-post och UserID anspråk som hello bilden.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | ------------------- | -------------------- |    
    | personalimmutableid | User.extensionattribute2 |
    | E-post               | User.Mail |
    | användar-ID              | User.userPrincipalName|
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.

    > [!NOTE] 
    > Innan du kan konfigurera hello SAML-kontroll måste toocontact din [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) och begära hello värdet för attributet för hello-Unik identifierare för din klient. Du behöver det här värdet tooconfigure hello anpassat anspråk för ditt program. 

8. På hello **ADP Globalview Configuration** klickar du på **konfigurera ADP Globalview** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. tooconfigure enkel inloggning på **ADP Globalview** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-adp-globalview-test-user"></a>Skapa en testanvändare ADP Globalview

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i ADP GlobalView. Arbeta med [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) tooadd hello användare i hello ADP GlobalView konto. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooADP Globalview.

![Tilldela användare][200] 

**tooassign Britta Simon tooADP Globalview, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ADP Globalview**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.  

Du bör få automatiskt inloggade tooyour ADP GlobalView programmet när du klickar på hello ADP GlobalView panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

