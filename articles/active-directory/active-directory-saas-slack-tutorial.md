---
title: "Självstudier: Azure Active Directory-integrering med Slack | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Slack."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 7f0151401af4dc63d2f714d4b4f66380c4b51e0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Självstudier: Azure Active Directory-integrering med Slack

I kursen får du lära dig hur toointegrate Slack med Azure Active Directory (AD Azure).

Integrera Slack med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSlack
- Du kan aktivera din användare tooautomatically get inloggade tooSlack (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Slack måste hello följande objekt:

- En Azure AD-prenumeration
- En Slack enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Slack från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-slack-from-hello-gallery"></a>Att lägga till Slack från hello-galleriet
tooconfigure hello integration slack till Azure AD, behöver du tooadd Slack hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Slack från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Slack**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. Markera hello resultat på panelen **Slack**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Slack baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Slack är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Slack toobe upprättas.

I Slack, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Slack, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en Slack testanvändare](#creating-a-slack-test-user)**  -toohave en motsvarighet för Britta Simon i Slack som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Slack.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Slack:**

1. I hello Azure-portalen på hello **Slack** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. På hello **Slack domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.slack.com`

    b. I hello **identifierare** textruta typen hello URL:`https://slack.com`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Du har tooupdate hello-värde med hello faktiska logga URL. Kontakta [Slack supportteamet](https://slack.com/help/contact) tooget hello värde
     
4. Slack program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. I hello **användarattribut** avsnittet hello **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i hello tabellen nedan, utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | --- | --- |
    | Förnamn | User.givenName |
    | Efternamn | User.surname |
    | User.Email | User.Mail |  
    | User.Username | User.userPrincipalName |

    a. Klicka på **attributet** tooopen **Redigera attribut** dialogrutan rutan och utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    a. I hello **namn** textruta hello attributnamn visas för den raden.
    
    b. Från hello **värdet** listan, Välj hello-attributvärde som visas för den raden.
    
    c. Klicka på **OK**

6. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. På hello **Slack Configuration** klickar du på **konfigurera Slack** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  Logga in tooyour Slack företagets webbplats som en administratör i en annan webbläsarfönster.

10.  Navigera för**Microsoft Azure AD** gå för**inställningar för Team**.

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  I hello **inställningar för Team** klickar du på hello **autentisering** fliken och klicka sedan på **ändra inställningar för**.

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. På hello **SAML autentiseringsinställningar** dialogrutan utföra hello följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  I hello **SAML 2.0 slutpunkt (HTTP)** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    b.  I hello **identitet providern utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.

    c.  Öppna filen hämtat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat med offentlig** textruta.

    d. Konfigurera hello tre inställningarna ovan som passar din Slack-teamet. Mer information om inställningar för hello hitta hello **Slacks guiden för konfiguration av SSO** här. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Klicka på **spara konfigurationen**.
     
    <!-- Deselect **Allow users toochange their email address**.

    e.  Select **Allow users toochoose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-slack-test-user"></a>Skapa en Slack testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Slack. Slack stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess Slack om den inte finns.

> [!NOTE]
> Om du behöver toocreate en användare manuellt, måste tooContact [Slack supportteamet](https://slack.com/help/contact).

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSlack.

![Tilldela användare][200] 

**tooassign Britta Simon tooSlack utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Slack**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Slack-panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour Slack program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

