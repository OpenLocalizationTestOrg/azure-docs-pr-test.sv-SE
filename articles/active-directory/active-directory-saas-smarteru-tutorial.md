---
title: "Självstudier: Azure Active Directory-integrering med SmarterU | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SmarterU."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a3b81b557c47e31f09e61bcf75dd23f370e642e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Självstudier: Azure Active Directory-integrering med SmarterU

I kursen får du lära dig hur toointegrate SmarterU med Azure Active Directory (AD Azure).

Integrera SmarterU med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSmarterU
- Du kan aktivera din användare tooautomatically get inloggade tooSmarterU (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SmarterU, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En SmarterU enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SmarterU från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-smarteru-from-hello-gallery"></a>Att lägga till SmarterU från hello-galleriet
tooconfigure hello integrering av SmarterU i Azure AD, behöver du tooadd SmarterU hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SmarterU från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SmarterU**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. Markera hello resultat på panelen **SmarterU**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SmarterU baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SmarterU är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SmarterU toobe upprättas.

I SmarterU, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SmarterU, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SmarterU](#creating-a-smarteru-test-user)**  -toohave en motsvarighet för Britta Simon i SmarterU som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt SmarterU program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SmarterU:**

1. I hello Azure-portalen på hello **SmarterU** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. På hello **SmarterU domän och URL: er** avsnittet, utföra hello följande steg: 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    I hello **identifierare** textruta typen hello URL:`https://www.smarteru.com/`

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. Logga in tooyour SmarterU företagets webbplats som en administratör i en annan webbläsarfönster.

7. Klicka i hello verktygsfältet hello längst upp **kontoinställningar**.
   
    ![Kontoinställningar](./media/active-directory-saas-smarteru-tutorial/IC777326.png "kontoinställningar")

8. På konfigurationssidan för hello konto utför hello följande steg:
   
    ![Externa auktorisering](./media/active-directory-saas-smarteru-tutorial/IC777327.png "externa auktorisering") 
 
      a. Välj **Aktivera externa auktorisering**.
  
      b. I hello **Master inloggningen kontrollen** avsnitt, Välj hello **SmarterU** fliken.
  
      c. I hello **standard användarinloggning** avsnitt, Välj hello **SmarterU** fliken.
  
      d. Välj **aktivera Okta**.
  
      e. Kopiera hello innehållet för hello hämtade metadatafil och klistra in den i hello **Okta Metadata** textruta.
  
      f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-smarteru-test-user"></a>Skapa en testanvändare SmarterU

tooenable Azure AD-användare toolog i tooSmarterU, måste de etableras i SmarterU.

När SmarterU, etablering är en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **SmarterU** klient.

2. Gå för**användare**.

3. Utför följande hello under hello användare:
   
    ![Ny användare](./media/active-directory-saas-smarteru-tutorial/IC777329.png "ny användare")  

    a. Klicka på **+ användaren**.
    
    b. Typen hello relaterade attributvärden för hello Azure AD-användarkonto i hello följande textrutor: **primära e-post**, **anställnings-ID**, **lösenord**,  **Bekräfta lösenord**, **Förnamn**, **efternamn**.
    
    c. Klicka på **Active**. 
    
    d. Klicka på **Spara**.

>[!NOTE]
>Du kan använda något annat SmarterU användarens konto skapas verktyg eller API: er som tillhandahålls av SmarterU tooprovision AAD-användarkonton.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSmarterU.

![Tilldela användare][200] 

**tooassign Britta Simon tooSmarterU utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SmarterU**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.
 
Du bör få automatiskt inloggade tooyour SmarterU programmet när du klickar på hello SmarterU panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

