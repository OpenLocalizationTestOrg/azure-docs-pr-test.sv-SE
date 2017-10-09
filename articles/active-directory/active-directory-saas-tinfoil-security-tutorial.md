---
title: "Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY

I kursen får du lära dig hur toointegrate TINFOIL SECURITY med Azure Active Directory (AD Azure).

Integrera TINFOIL SECURITY med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooTINFOIL säkerhet
- Du kan aktivera din användare tooautomatically get inloggade tooTINFOIL säkerhet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TINFOIL SECURITY måste hello följande objekt:

- En Azure AD-prenumeration
- En TINFOIL SECURITY enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till TINFOIL SECURITY från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-tinfoil-security-from-hello-gallery"></a>Lägg till TINFOIL SECURITY från hello-galleriet
tooconfigure hello integrering av TINFOIL SECURITY i Azure AD, behöver du tooadd TINFOIL SECURITY hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TINFOIL SECURITY från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TINFOIL SECURITY**väljer **TINFOIL SECURITY** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![TINFOIL SECURITY från galleriet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TINFOIL SECURITY baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TINFOIL SECURITY är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TINFOIL SECURITY toobe upprättas.

Tilldela hello värdet för hello i TINFOIL SECURITY **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TINFOIL SECURITY, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  -toohave en motsvarighet för Britta Simon i TINFOIL SECURITY som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TINFOIL SECURITY-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TINFOIL SECURITY:**

1. I hello Azure-portalen på hello **TINFOIL SECURITY** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. På hello **TINFOIL SECURITY domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värde.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. mappningar av tooadd hello krävs, utför hello följande steg:
    
    ![Attribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "attribut")
    
    | Attributets namn    |   Attributvärdet |
    | ------------------- | -------------------- |
    | accountid | UXXXXXXXXXXXXX |
    
    a. Klicka på **lägga till användarattribut**.
    
    ![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "attribut")
    
    ![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "attribut")
    
    b. I hello **attributnamn** textruta typen **accountid**.
    
    c. I hello **attributvärdet** textruta klistra in hello konto-ID-värde som visas vid ett senare tillfälle hello kursen.
    
    d. Klicka på **OK**.    

6. Klicka på **spara** knappen.

    ![Knappen Spara](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. På hello **TINFOIL SECURITY Configuration** klickar du på **konfigurera TINFOIL SECURITY** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. Logga in på webbplatsen TINFOIL SECURITY företag som en administratör i en annan webbläsarfönster.

9. Klicka i hello verktygsfältet hello längst upp **mitt konto**.
   
    ![Instrumentpanelen](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "instrumentpanelen")

10. Klicka på **säkerhet**.
   
    ![Säkerhet](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "säkerhet")

11. På hello **enkel inloggning** konfigurationen utför hello följande steg:
   
    ![Enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "enkel inloggning")
   
    a. Välj **aktivera SAML**.
   
    b. Klicka på **manuell konfiguration**.
   
    c. I **SAML Post URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen
   
    d. I **SAML certifikat fingeravtryck** textruta klistra in hello värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt.
  
    e. Kopiera **ditt konto-ID** värdet och klistrar in hello värdet i **attributvärdet** textruta under **lägga till attributet** avsnitt i Azure-portalen.
   
    f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Användare och grupper -> alla användare ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Användare](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-tinfoil-security-test-user"></a>Skapa en testanvändare TINFOIL SECURITY

I ordning tooenable Azure AD-användare toolog till TINFOIL SECURITY, måste de etableras i TINFOIL SECURITY. TINFOIL SECURITY hello gäller är etablering en manuell aktivitet.

**tooget en användare som har etablerats, utför hello följande steg:**

1. Om hello användaren är en del av ett Enterprise-konto, behöver du för[Kontakta supportteamet för hello TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) tooget hello användarkonton som skapas.

2. Om hello användare är en vanlig TINFOIL SECURITY SaaS-användare, sedan hello användare kan lägga till en deltagare tooany hello användarens webbplatser. Den här utlösare en process toosend en inbjudan toohello angivna e-toocreate ett nytt användarkonto TINFOIL SECURITY.

> [!NOTE]
> Du kan använda något annat TINFOIL SECURITY användare konto skapas verktyg eller API: er som tillhandahålls av TINFOIL SECURITY tooprovision användarkonton i Azure AD.
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTINFOIL säkerhet.

![Tilldela användare][200] 

**tooassign Britta Simon tooTINFOIL säkerhet, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TINFOIL SECURITY**.

    ![Välj TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour TINFOIL SECURITY programmet när du klickar på hello TINFOIL SECURITY-panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

