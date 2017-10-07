---
title: "Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Jitbit Helpdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f6c837bbb910c1e84f7ed9da676c5ab40f75338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk

I kursen får du lära dig hur toointegrate Jitbit Helpdesk med Azure Active Directory (AD Azure).

Integrera Jitbit Helpdesk med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooJitbit supportavdelningen
- Du kan aktivera din användare tooautomatically get inloggade tooJitbit supportavdelningen (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Jitbit Helpdesk måste hello följande objekt:

- En Azure AD-prenumeration
- En Jitbit Helpdesk enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Jitbit Helpdesk från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-jitbit-helpdesk-from-hello-gallery"></a>Att lägga till Jitbit Helpdesk från hello-galleriet
tooconfigure hello integrering av Jitbit Helpdesk i Azure AD, behöver du tooadd Jitbit Helpdesk hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Jitbit Helpdesk från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Jitbit Helpdesk**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. Markera hello resultat på panelen **Jitbit Helpdesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med Jitbit Helpdesk baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Jitbit Helpdesk är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Jitbit Helpdesk toobe upprättas.

Tilldela hello värdet för hello i Jitbit Helpdesk **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Jitbit Helpdesk, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  -toohave en motsvarighet för Britta Simon i Jitbit Helpdesk som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Jitbit Helpdesk.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Jitbit Helpdesk:**

1. I hello Azure-portalen på hello **Jitbit Helpdesk** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. På hello **Jitbit supportavdelningen domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Jitbit supportavdelningen klienten supportteamet](https://www.jitbit.com/support/) tooget det här värdet. 
    
    b.  I hello **identifierare** textruta Skriv en URL som följande:`https://www.jitbit.com/web-helpdesk/`

    
 


4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. På hello **Jitbit supportavdelningen Configuration** klickar du på **konfigurera Jitbit supportavdelningen** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. Logga in på webbplatsen Jitbit Helpdesk företag som en administratör i en annan webbläsarfönster.

8. Klicka i hello verktygsfältet hello längst upp **Administration**.
   
    ![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")

9. Klicka på **allmänna inställningar**.
   
    ![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "användare, företag och behörigheter")

10. I hello **autentiseringsinställningar** konfigurationen och utföra hello följande steg:
   
    ![Autentiseringsinställningar](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "autentiseringsinställningar")
    
    a. Välj **aktivera SAML 2.0 enkel inloggning**, toosign i med enkel inloggning (SSO), **OneLogin**.

    b. I hello **slutpunkts-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    c. Öppna din **Base64-** kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta

    d. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typnamn som **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Skapa en testanvändare Jitbit Helpdesk

I ordning tooenable Azure AD-användare toolog till Jitbit Helpdesk, måste de etableras i Jitbit Helpdesk.  Hello gäller Jitbit Helpdesk är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **Jitbit Helpdesk** klient.

2. Hello-menyn överst hello **Administration**.
   
    ![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")

3. Klicka på **användare och behörigheter**.
   
    ![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "användare, företag och behörigheter")

4. Klicka på **Lägg till användare**.
   
    ![Lägg till användare](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Lägg till användare")
   
5. I hello Skapa avsnittet, skriver du hello data på hello Azure AD-konto som du vill tooprovision på följande sätt:

    ![Skapa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "skapa")
   
   a. I hello **användarnamn** textruta typen **BrittaSimon**, hello användarnamn som hello Azure-portalen.

   b. I hello **e-post** textruta e-post för hello användare som  **BrittaSimon@contoso.com** .

   c. I hello **Förnamn** textruta Ange först namnet på hello användaren som **Britta**.

   d. I hello **efternamn** textruta Skriv Efternamn för hello användare som **Simon**.
   
   e. Klicka på **Skapa**.

>[!NOTE]
>Du kan använda något annat Jitbit Helpdesk användarens konto skapas verktyg eller API: er som tillhandahålls av Jitbit Helpdesk tooprovision användarkonton i Azure AD.
> 
        

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJitbit supportavdelningen.

![Tilldela användare][200] 

**tooassign Britta Simon tooJitbit supportavdelningen, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Jitbit Helpdesk**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få inloggningssidan för Jitbit Helpdesk program när du klickar på hello Jitbit Helpdesk panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

