---
title: "Självstudier: Azure Active Directory-integrering med AnswerHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AnswerHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 90b530da31abe7e6f18bfa2c5409f8ff1d4f1063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Självstudier: Azure Active Directory-integrering med AnswerHub

I kursen får du lära dig hur toointegrate AnswerHub med Azure Active Directory (AD Azure).

Integrera AnswerHub med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooAnswerHub
- Du kan aktivera din användare tooautomatically get inloggade tooAnswerHub (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med AnswerHub, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En AnswerHub enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till AnswerHub från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-answerhub-from-hello-gallery"></a>Att lägga till AnswerHub från hello-galleriet
tooconfigure hello integrering av AnswerHub i Azure AD, behöver du tooadd AnswerHub hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd AnswerHub från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **AnswerHub**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. Markera hello resultat på panelen **AnswerHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AnswerHub baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AnswerHub är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AnswerHub toobe upprättas.

I AnswerHub, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med AnswerHub, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare AnswerHub](#creating-an-answerhub-test-user)**  -toohave en motsvarighet för Britta Simon i AnswerHub som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AnswerHub program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AnswerHub:**

1. I hello Azure-portalen på hello **AnswerHub** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. På hello **AnswerHub domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.answerhub.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.answerhub.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [AnswerHub klienten supportteamet](mailto:success@answerhub.com) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. På hello **AnswerHub Configuration** klickar du på **konfigurera AnswerHub** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. Logga in på webbplatsen AnswerHub företag som en administratör i en annan webbläsarfönster.
   
    >[!NOTE]
    >Om du behöver hjälp med att konfigurera AnswerHub kontaktar [Answerhubs supportteamet](mailto:success@answerhub.com.).
   
8. Gå för**Administration**.

9. Klicka på hello **användar- och** fliken.

10. I navigeringsfönstret för hello på vänster sida, i hello hello **sociala inställningar** klickar du på **SAML installationsprogrammet**.

11. Klicka på **IDP Config** fliken.

12. På hello **IDP Config** fliken, utföra hello följande steg:

     ![SAML-installationsprogrammet](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML-installationen")  
  
     a. I **IDP inloggnings-URL** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
  
     b. I **IDP logga ut URL** textruta klistra in **Sign-Out URL** värde som du har kopierat från Azure-portalen.
     
     c. I **IDP identifierare namnformat** textruta ange hello användare identifierare värdet samma som väljs i Azure-portalen i **användarattribut** avsnitt.
  
     d. Klicka på **nycklar och certifikat**.

13. Utför följande steg hello hello nycklar och certifikat på fliken:
    
     ![Nycklar och certifikat](./media/active-directory-saas-answerhub-tutorial/ic785173.png "nycklar och certifikat")  
 
     a. Öppna din Base64-kodade certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IDP offentlig nyckel (x 509-Format)** textruta.
  
     b. Klicka på **Spara**.

14. På hello **IDP Config** klickar du på **spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-answerhub-test-user"></a>Skapa en testanvändare AnswerHub

tooenable Azure AD-användare toolog i tooAnswerHub, måste de etableras i AnswerHub.  
Hello gäller AnswerHub är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **AnswerHub** företagets webbplats som administratör.

2. Gå för**Administration**.

3. Klicka på hello **användare och grupper** fliken.

4. I navigeringsfönstret för hello på vänster sida, i hello hello **hantera användare** klickar du på **skapa eller importera användare**.
   
   ![Användare och grupper](./media/active-directory-saas-answerhub-tutorial/ic785175.png "användare och grupper")

5. Typen hello **e-postadress**, **användarnamn** och **lösenord** av en giltig Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor och klicka sedan på  **Spara**.

>[!NOTE]
>Du kan använda något annat AnswerHub användarens konto skapas verktyg eller API: er som tillhandahålls av AnswerHub tooprovision AAD-användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAnswerHub.

![Tilldela användare][200] 

**tooassign Britta Simon tooAnswerHub utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **AnswerHub**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour AnswerHub programmet när du klickar på hello AnswerHub panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

