---
title: "Självstudier: Azure Active Directory-integrering med Igloo programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Igloo programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 406405d4faa6e56f1005a61e69a29ef2ef2eb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Självstudier: Azure Active Directory-integrering med Igloo programvara

I kursen får du lära dig hur toointegrate Igloo programvara med Azure Active Directory (AD Azure).

Integrera Igloo programvara med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooIgloo programvara
- Du kan aktivera din användare tooautomatically get inloggade tooIgloo programvara (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Igloo programvara, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Igloo programvara enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Igloo programvara från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-igloo-software-from-hello-gallery"></a>Lägga till Igloo programvara från hello-galleriet
tooconfigure hello integrering av Igloo programvara i Azure AD, behöver du tooadd Igloo programvara från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd Igloo programvara från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Igloo programvara**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. Markera hello resultat på panelen **Igloo programvara**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Igloo programvara baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Igloo programvara är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Igloo programvara toobe upprätta.

I Igloo programvara, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Igloo programvara, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Igloo programvara](#creating-an-igloo-software-test-user)**  -toohave en motsvarighet för Britta Simon Igloo programvara som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Igloo programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Igloo programvara:**

1. I hello Azure-portalen på hello **Igloo programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. På hello **Igloo programvara domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com/saml.digest`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [Igloo Programvaruklienten supportteamet](https://www.igloosoftware.com/services/support) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. På hello **Igloo programvarukonfiguration** klickar du på **konfigurera Igloo programvara** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. Logga in tooyour Igloo programvara företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå toohello **Kontrollpanelen**.
   
     ![Kontrollpanelen](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Kontrollpanelen")

9. I hello **medlemskap** klickar du på **logga inställningarna**.
   
    ![Logga in inställningar](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "logga in inställningarna")

10. I hello SAML-konfigurationsavsnittet, klickar du på **konfigurera SAML-autentisering**.
   
    ![SAML-konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-konfiguration")
   
11. I hello **Allmän konfiguration** avsnittet, utföra hello följande steg:
   
    ![Allmän konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "Allmän konfiguration")

    a. I hello **anslutningsnamn** textruta skriver du ett eget namn för din konfiguration.
   
    b. I hello **IdP inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    c. I hello **IdP logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
    
    d. Välj **logga ut svar och typ av begäran HTTP** som **efter**.
   
    e. Öppna din **Base64-** kodat certifikat i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat med offentlig** textruta.
    
12. I hello **svar och Autentiseringskonfiguration**, utföra hello följande steg:
    
    ![Svar och Autentiseringskonfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "svar och Autentiseringskonfiguration")
  
      a. Som **identitetsleverantör**väljer **Microsoft ADFS**.
      
      b. Som **Ämnesidentifierarens typ**väljer **e-postadress**. 

      c. I hello **e-attributet** textruta typen **e-postadress**.

      d. I hello **förnamn attributet** textruta typen **givenname**.

      e. I hello **senaste namnattributet** textruta typen **efternamn**.

13. Utför hello följande steg toocomplete hello konfiguration:
    
    ![Skapa användare på inloggning](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "skapa användare på Logga in") 

     a. Som **skapa användare på inloggning**väljer **skapa en ny användare på din plats när de loggar in**.

     b. Som **inloggning inställningar**väljer **Använd SAML-knappen ”Logga in” skärmen**.

     c. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-igloo-software-test-user"></a>Skapa en testanvändare Igloo programvara

Det finns inga uppgiften för du tooconfigure användaretablering tooIgloo programvara.  

När en tilldelad användare försöker toolog i tooIgloo programvara med hjälp av hello åtkomstpanelen, Igloo programvara kontrollerar om hello användaren finns.  Om det finns inget användarkonto ännu, skapas den automatiskt med Igloo program.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIgloo programvara.

![Tilldela användare][200] 

**tooassign Britta Simon tooIgloo programvara, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Igloo programvara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Igloo programvara panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Igloo program.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

