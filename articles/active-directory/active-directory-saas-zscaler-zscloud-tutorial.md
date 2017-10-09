---
title: "Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler ZSCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: af6d5c1994e715cccf959cc9fd3ba998e5b9effa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud

I kursen får du lära dig hur toointegrate Zscaler ZSCloud med Azure Active Directory (AD Azure).

Integrera Zscaler ZSCloud med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooZscaler ZSCloud
- Du kan aktivera din användare tooautomatically get inloggade tooZscaler ZSCloud (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Zscaler ZSCloud, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Zscaler ZSCloud enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zscaler ZSCloud från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zscaler-zscloud-from-hello-gallery"></a>Att lägga till Zscaler ZSCloud från hello-galleriet
tooconfigure hello integrering av Zscaler ZSCloud i Azure AD, behöver du tooadd Zscaler ZSCloud hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Zscaler ZSCloud från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Zscaler ZSCloud**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. Markera hello resultat på panelen **Zscaler ZSCloud**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler ZSCloud baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zscaler ZSCloud är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zscaler ZSCloud toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zscaler ZSCloud.

tooconfigure och testa Azure AD enkel inloggning med Zscaler ZSCloud, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler ZSCloud som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler ZSCloud.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zscaler ZSCloud:**

1. I hello Azure-portalen på hello **Zscaler ZSCloud** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. På hello **Zscaler ZSCloud domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     I hello **inloggnings-URL** textruta typen hello URL som används av dina användare toosign på tooyour ZScaler ZSCloud program.
    
    > [!NOTE] 
    > Du har tooupdate värdet med hello faktiska inloggnings-URL. Kontakta [Zscaler ZSCloud klienten supportteamet](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. På hello **Zscaler ZSCloud Configuration** klickar du på **konfigurera Zscaler ZSCloud** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. I en annan webbläsarfönster logga in tooyour ZScaler ZSCloud företagets webbplats som administratör.

8. Hello-menyn överst hello **Administration**.
   
    ![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")

9. Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.   
            
    ![Hantera användare och autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "hantera användare och autentisering")

10. I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:   
                
    ![Autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "autentisering")
   
    a. Välj **autentisera med SAML enkel inloggning**.

    b. Klicka på **konfigureras SAML enkel inloggning**.

11. På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**

    ![Enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "enkel inloggning")
    
    a. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värdet till hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.
    
    b. I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.
    
    c. tooupload hämtade certifikatet, klicka på **Zscaler pem**.
    
    d. Välj **aktivera etablering av SAML-automatiskt**.

12. På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:

    ![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")
    
    a. Klicka på **Spara**.

    b. Klicka på **aktivera nu**.

## <a name="configuring-proxy-settings"></a>Konfigurera proxyinställningar
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a>tooconfigure hello proxyinställningarna i Internet Explorer

1. Starta **Internet Explorer**.

2. Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.   
    
     ![Internet-alternativ](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet-alternativ")

3. Klicka på hello **anslutningar** fliken.   
  
     ![Anslutningar](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "anslutningar")

4. Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.

5. I hello avsnittet för Proxy-server, utför hello följande steg:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxyserver")

    a. Välj **använder en proxyserver för nätverket**.

    b. Skriv i hello adress textruta **gateway.zscalerone.net**.

    c. Skriv i hello Port textruta **80**.

    d. Välj **Använd ingen proxyserver för lokala adresser**.

    e. Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.

6. Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Skapa en testanvändare Zscaler ZSCloud

tooenable Azure AD-användare toolog i tooZScaler ZSCloud, måste de vara etablerade tooZScaler ZSCloud.  
Hello gäller ZScaler ZSCloud är etablering en manuell aktivitet.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:

1. Logga in tooyour **Zscaler** klient.

2. Klicka på **Administration**.   
   
    ![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")

3. Klicka på **Användarhantering**.   
        
     ![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")

4. I hello **användare** klickar du på **Lägg till**.
      
    ![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")

5. I hello lägga till användare, utföra hello följande steg:
        
    ![Lägg till användare](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "lägga till användare")
   
    a. Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** på en giltig AAD-konto som du vill tooprovision.

    b. Klicka på **Spara**.

> [!NOTE]
> Du kan använda något annat ZScaler ZSCloud användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler ZSCloud tooprovision AAD-användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler ZSCloud.

![Tilldela användare][200] 

**tooassign Britta Simon tooZscaler ZSCloud, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zscaler ZSCloud**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Zscaler ZSCloud programmet när du klickar på hello Zscaler ZSCloud panelen i hello åtkomstpanelen.

Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

