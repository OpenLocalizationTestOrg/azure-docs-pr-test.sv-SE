---
title: "Självstudier: Azure Active Directory-integrering med FreshDesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Självstudier: Azure Active Directory-integrering med FreshDesk

I kursen får du lära dig hur toointegrate FreshDesk med Azure Active Directory (AD Azure).

Integrera FreshDesk med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooFreshDesk
- Du kan aktivera din användare tooautomatically get inloggade tooFreshDesk (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med FreshDesk, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En FreshDesk enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till FreshDesk från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-freshdesk-from-hello-gallery"></a>Att lägga till FreshDesk från hello-galleriet
tooconfigure hello integrering av FreshDesk i Azure AD, behöver du tooadd FreshDesk hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd FreshDesk från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **FreshDesk**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. Markera hello resultat på panelen **FreshDesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FreshDesk baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FreshDesk är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FreshDesk toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i FreshDesk.

tooconfigure och testa Azure AD enkel inloggning med FreshDesk, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare FreshDesk](#creating-a-freshdesk-test-user)**  -toohave en motsvarighet för Britta Simon i FreshDesk som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt FreshDesk program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FreshDesk:**

1. I hello Azure Management portal på hello **FreshDesk** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. På hello **FreshDesk domän och URL: er** avsnittet genom att ange hello **inloggnings-URL** som: `https://<tenant-name>.freshdesk.com` eller något annat värde Freshdesk har förslag.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Observera att detta inte är det verkliga värdet. Du måste uppdatera värdet med det faktiska inloggnings-URL. Kontakta [FreshDesk klienten supportteamet](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) att hämta det här värdet.  

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. På hello **FreshDesk Configuration** klickar du på **konfigurera FreshDesk** tooopen inloggning konfigurera fönster. Kopiera hello SAML inloggning tjänst-URL för enkel och Sign-Out URL från hello **Snabbreferens** avsnitt.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. Logga in på webbplatsen Freshdesk företag som en administratör i en annan webbläsarfönster.

8. Hello-menyn överst hello **Admin**.
   
   ![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")

9. I hello **allmänna inställningar** klickar du på **säkerhet**.
   
   ![Säkerhet](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "säkerhet")

10. I hello **säkerhet** avsnittet, utföra hello följande steg:
   
    ![Enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "enkel inloggning")
   
    a. För **enkel inloggning (SSO)**väljer **på**.

    b. Välj **SAML SSO**.

    c. Typen hello **SAML enkel inloggning Tjänstwebbadress** du kopierade från Azure-portalen i hello **inloggnings-URL för SAML** textruta.

    d. Typen hello **Sign-Out URL** du kopierade från Azure-portalen i hello **logga ut URL** textruta.

    e. Kopiera hello **tumavtrycket** värde från hello hämtat certifikat från Azure-portalen och klistra in den i hello **säkerhet certifikat fingeravtryck** textruta.  
 
    >[!TIP]
    >Mer information finns i [hur tooretrieve en certifikatets tumavtrycksvärde](http://youtu.be/YKQF266SAxI). 
    
    f. Klicka på **Spara**.


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-freshdesk-test-user"></a>Skapa en testanvändare FreshDesk

I ordning tooenable Azure AD-användare toolog i FreshDesk, måste de etableras i FreshDesk.  
Hello gäller FreshDesk är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour **Freshdesk** klient.
2. Hello-menyn överst hello **Admin**.
   
   ![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")

3. I hello **allmänna inställningar** klickar du på **agenter**.
   
   ![Agenter](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agenter")

4. Klicka på **ny Agent**.
   
    ![Ny Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "ny Agent")

5. Utför följande hello på hello agentinformation dialogrutan:
   
   ![Agentinformation](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "agentinformation")
   
   a. I hello **fullständiga namn** textruta hello-typnamn för hello Azure AD-konto som du vill tooprovision.

   b. I hello **e-post** textruta typen hello Azure AD e-postadress på hello Azure AD-konto som du vill tooprovision.

   c. I hello **rubrik** textruta ange hello rubrik på hello Azure AD-konto som du vill tooprovision.

   d. Välj **agenter rollen**, och klicka sedan på **tilldela**.
       
   e. Klicka på **Spara**.     
   
    >[!NOTE]
    >hello Azure AD användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras. 
    > 
    
    >[!NOTE]
    >Du kan använda något annat Freshdesk användarens konto skapas verktyg eller API: er som tillhandahålls av Freshdesk tooprovision AAD-användarkonton. tooFreshDesk.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooBox sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooFreshDesk utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **FreshDesk**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få inloggningen sidan tooget inloggade tooyour FreshDesk programmet när du klickar på hello FreshDesk panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

