---
title: "Självstudier: Azure Active Directory-integrering med Mozy Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mozy Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: bab0df4f3621b784cd8edfda3c8e10fe5a7ced9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Självstudier: Azure Active Directory-integrering med Mozy Enterprise

I kursen får du lära dig hur toointegrate Mozy Enterprise med Azure Active Directory (AD Azure).

Integrera Mozy Enterprise med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooMozy Enterprise
- Du kan aktivera din användare tooautomatically get inloggade tooMozy Enterprise (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Mozy Enterprise, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Mozy Enterprise enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Mozy Enterprise från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-mozy-enterprise-from-hello-gallery"></a>Att lägga till Mozy Enterprise från hello-galleriet
tooconfigure hello integrering av Mozy Enterprise i Azure AD, behöver du tooadd Mozy Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Mozy Enterprise från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Mozy Enterprise**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. Markera hello resultat på panelen **Mozy Enterprise**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med Mozy Enterprise baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mozy företag är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Mozy Enterprise toobe upprättas.

Tilldela hello värdet för hello i Mozy Enterprise **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Mozy Enterprise, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i Mozy företag som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Mozy Enterprise-programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Mozy Enterprise:**

1. I hello Azure-portalen på hello **Mozy Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. På hello **Mozy företagsdomänen och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.Mozyenterprise.com`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Mozy Enterprise-klienten supportteamet](http://support.mozy.com/) tooget det här värdet.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. På hello **Mozy företagskonfiguration** klickar du på **konfigurera Mozy Enterprise** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. Logga in på webbplatsen Mozy Enterprise företag som en administratör i en annan webbläsarfönster.

8. I hello **Configuration** klickar du på **autentiseringsprincip**.
   
   ![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "autentiseringsprincip")

9. På hello **autentiseringsprincip** avsnittet, utföra hello följande steg:
   
   ![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "autentiseringsprincip")
   
   a. Välj **katalogtjänsten** som **Provider**.
   
   b. Välj **använder LDAP Push**.
   
   c. Klicka på hello **SAML-autentisering** fliken.
   
   d. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **autentiserings-URL för** textruta.
   
   e. Klistra in **SAML enhets-ID**, som du har kopierat från hello Azure-portalen i hello **SAML Endpoint** textruta.
   
   f. Öppna din hämtade Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **SAML certifikat** textruta.
   
   g. Välj **aktivera enkel inloggning för administratörer toolog in med sina nätverksinloggningsuppgifter**.
   
   h. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Skapa en testanvändare Mozy Enterprise

I ordning tooenable Azure AD-användare toolog i Mozy Enterprise, måste de etableras i Mozy Enterprise. Hello gäller Mozy Enterprise är etablering en manuell aktivitet.

>[!NOTE]
>Du kan använda något annat Mozy Enterprise användarens konto skapas verktyg eller API: er som tillhandahålls av Mozy Enterprise tooprovision AAD användarkonton.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour **Mozy Enterprise** klient.

2. Klicka på **användare**, och klicka sedan på **Lägg till nya användare**.
   
   ![Användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "användare")
   
   >[!NOTE]
   >Hej **Lägg till nya användare** alternativet visas bara om **Mozy** är markerad som hello provider under **autentiseringsprincip**. Om SAML-autentisering är konfigurerad, sedan hello användare läggs till automatiskt på sin första inloggning via enkel inloggning på.
    
3. Utför följande hello på hello nya användardialogrutan:
   
   ![Lägg till användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "lägga till användare")
   
   a. Från hello **väljer du en grupp** väljer du en grupp.
   
   b. Från hello **vilken typ av användare** väljer du en typ.
   
   c. I hello **användarnamn** textruta hello-typnamn för hello Azure AD-användare.
   
   d. I hello **e-post** textruta typen hello användarens e-postadress hello Azure AD.
   
   e. Välj **skicka e-post för användaren instruktion**.
   
   f. Klicka på **lägga till användare**.

     >[!NOTE]
     > När du har skapat hello användaren skickas ett e-postmeddelande toohello Azure AD-användare som innehåller en länk tooconfirm hello-konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMozy Enterprise.

![Tilldela användare][200] 

**tooassign Britta Simon tooMozy Enterprise, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Mozy Enterprise**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Mozy Enterprise-panelen i hello åtkomstpanelen bör du hämta inloggningssidan Mozy affärsprogram.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

