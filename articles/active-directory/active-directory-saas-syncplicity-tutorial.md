---
title: "Självstudier: Azure Active Directory-integrering med Syncplicity | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Självstudier: Azure Active Directory-integrering med Syncplicity

I kursen får du lära dig hur toointegrate Syncplicity med Azure Active Directory (AD Azure).

Integrera Syncplicity med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSyncplicity
- Du kan aktivera din användare tooautomatically get inloggade tooSyncplicity (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Syncplicity, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Syncplicity enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Syncplicity från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-syncplicity-from-hello-gallery"></a>Att lägga till Syncplicity från hello-galleriet
tooconfigure hello integrering av Syncplicity i Azure AD, behöver du tooadd Syncplicity hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Syncplicity från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Syncplicity**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. Markera hello resultat på panelen **Syncplicity**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Syncplicity baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Syncplicity är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Syncplicity toobe upprättas.

I Syncplicity, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Syncplicity, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Syncplicity](#creating-a-syncplicity-test-user)**  -toohave en motsvarighet för Britta Simon i Syncplicity som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Syncplicity program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Syncplicity:**

1. I hello Azure-portalen på hello **Syncplicity** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. På hello **Syncplicity domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.syncplicity.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Syncplicity klienten supportteamet](https://www.syncplicity.com/contact-us) tooget dessa värden. 
 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. På hello **Syncplicity Configuration** klickar du på **konfigurera Syncplicity** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Logga in tooyour **Syncplicity** klient.

8. Hello-menyn överst hello **admin**väljer **inställningar**, och klicka sedan på **anpassad domän och enkel inloggning**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. På hello **enkel inloggning (SSO)** dialogrutan utför hello följande steg:
   
    ![Enkel inloggning \(enkel inloggning\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. I hello **anpassad domän** textruta hello-typnamn för din domän.
  
    b. Välj **aktiverat** som **enkel inloggning Status**.

    c. I hello **enhets-Id** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.

    d. I hello **inloggning Sidadress** textruta klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    e. I hello **logga ut Sidadress** textruta klistra in hello **Sign-Out URL** som du har kopierat från Azure-portalen.

    f. I **providern identitetscertifikat**, klickar du på **Välj fil**, och sedan ladda upp hello-certifikat som du har hämtat från hello Azure-portalen. 

    g. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-syncplicity-test-user"></a>Skapa en testanvändare Syncplicity
För AAD användare toobe kan toosign i, måste de vara etablerade tooSyncplicity program. Det här avsnittet beskrivs hur toocreate AAD användarkonton i Syncplicity.

**tooprovision konto tooSyncplicity för en användare utför hello följande steg:**

1. Logga in tooyour **Syncplicity** klient (till exempel: `https://company.Syncplicity.com`).

2. Klicka på **admin** och välj **användarkonton**.

3. Klicka på **lägga till en användare**.
   
    ![Hantera användare](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "hantera användare")

4. Typen hello **e-adresser** på ett AAD-konto som du vill tooprovision, Välj **användare** som **rollen**, och klicka sedan på **nästa**.
   
    ![Kontoinformation](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "kontoinformation")
   
    >[!NOTE]
    >hello AAD användare får ett e-postmeddelande inklusive en länk tooconfirm och aktivera hello-konto. 
    > 

5. Välj en grupp i företaget som de nya användarna ska bli medlem i och klicka sedan på **nästa**.
   
    ![Gruppmedlemskap](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "gruppmedlemskap")
   
    >[!NOTE]
    >Om det finns inga grupper visas, klickar du på **nästa**. 
    > 

6. Välj hello mappar du gillar tooplace under Syncplicitys kontroll på hello användares dator och klicka sedan på **nästa**.
   
    ![Syncplicity mappar](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappar")

>[!NOTE]
>Du kan använda något annat Syncplicity användarens konto skapas verktyg eller API: er som tillhandahålls av Syncplicity tooprovision AAD-användarkonton. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSyncplicity.

![Tilldela användare][200] 

**tooassign Britta Simon tooSyncplicity utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Syncplicity**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Syncplicity programmet när du klickar på hello Syncplicity panelen i hello åtkomstpanelen.
## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

