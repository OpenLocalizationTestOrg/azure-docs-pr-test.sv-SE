---
title: "Självstudier: Azure Active Directory-integrering med Zscaler Beta | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler Beta."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1471c2b51ca5684a11acd40f4e450521605bb786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Självstudier: Azure Active Directory-integrering med Zscaler Beta

I kursen får du lära dig hur toointegrate Zscaler Beta med Azure Active Directory (AD Azure).

Integrera Zscaler Beta med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooZscaler Beta
- Du kan aktivera din användare tooautomatically get inloggade tooZscaler Beta (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Zscaler Beta, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Zscaler Beta enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zscaler Beta från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zscaler-beta-from-hello-gallery"></a>Att lägga till Zscaler Beta från hello-galleriet
tooconfigure hello integrering av Zscaler Beta i Azure AD, behöver du tooadd Zscaler Beta hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Zscaler Beta från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Zscaler Beta**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. Markera hello resultat på panelen **Zscaler Beta**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med Zscaler Beta baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i betaversionen av Zscaler är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i betaversionen av Zscaler toobe upprättas.

Tilldela hello värdet hello i betaversionen av Zscaler **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Zscaler Beta, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer
3. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
4. **[Skapa en testanvändare Zscaler Beta](#creating-a-zscaler-beta-test-user)**  -toohave en motsvarighet för Britta Simon i betaversionen av Zscaler som är länkade toohello Azure AD-representation av användaren.
5. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
6. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zscaler Beta-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zscaler Beta:**

1. I hello Azure-portalen på hello **Zscaler Beta** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. På hello **Zscaler Beta domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Ange hello-URL som används av dina användare toosign på tooyour Zscaler Beta-programmet i hello inloggning URL: en textruta.

    > [!NOTE] 
    > Du har tooupdate värdet med hello faktiska inloggnings-URL. Kontakta [Zscaler Betaklienten supportteamet](https://www.zscaler.com/company/contact) tooget det här värdet. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. På hello **Zscaler Beta Configuration** klickar du på **konfigurera Zscaler Beta** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. I en annan webbläsarfönster logga in tooyour Zscaler Beta företagets webbplats som administratör.

8. Hello-menyn överst hello **Administration**.
   
    ![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "Administration")

9. Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.   
            
    ![Hantera användare och autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "hantera användare och autentisering")

10. I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:   
                
    ![Autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "autentisering")
   
    a. Välj **autentisera med SAML enkel inloggning**.

    b. Klicka på **konfigureras SAML enkel inloggning**.

11. På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**

    ![Enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "enkel inloggning")
    
    a. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.
    
    b. I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.
    
    c. tooupload hämtade certifikatet, klicka på **Zscaler pem**.
    
    d. Välj **aktivera etablering av SAML-automatiskt**.

12. På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:

    ![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "Administration")
    
    a. Klicka på **Spara**.

    b. Klicka på **aktivera nu**.

## <a name="configuring-proxy-settings"></a>Konfigurera proxyinställningar
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a>tooconfigure hello proxyinställningarna i Internet Explorer

1. Starta **Internet Explorer**.

2. Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.   
    
     ![Internet-alternativ](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internet-alternativ")

3. Klicka på hello **anslutningar** fliken.   
  
     ![Anslutningar](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "anslutningar")

4. Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.

5. I hello avsnittet för Proxy-server, utför hello följande steg:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "proxyserver")

    a. Välj **använder en proxyserver för nätverket**.

    b. Skriv i hello adress textruta **gateway.zscalerbeta.net**.

    c. Skriv i hello Port textruta **80**.

    d. Välj **Använd ingen proxyserver för lokala adresser**.

    e. Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.

6. Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Skapa en testanvändare Zscaler Beta

tooenable Azure AD-användare toolog i tooZscaler Beta, måste de vara etablerade tooZscaler Beta. Hello gäller Zscaler Beta är etablering en manuell aktivitet.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:

1. Logga in tooyour **Zscaler Beta** klient.

2. Klicka på **Administration**.   
   
    ![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "Administration")

3. Klicka på **Användarhantering**.   
        
     ![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "Lägg till")

4. I hello **användare** klickar du på **Lägg till**.
      
    ![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "Lägg till")

5. I hello lägga till användare, utföra hello följande steg:
        
    ![Lägg till användare](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "lägga till användare")
   
    a. Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** av ett giltigt Azure AD-kontot som du vill tooprovision.

    b. Klicka på **Spara**.

> [!NOTE]
> Du kan använda något annat Zscaler Beta användarens konto skapas verktyg eller API: er som tillhandahålls av Zscaler Beta tooprovision användarkonton i Azure AD.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler Beta.

![Tilldela användare][200] 

**tooassign Britta Simon tooZscaler Beta, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zscaler Beta**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Zscaler Beta-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Zscaler Beta-programmet.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

