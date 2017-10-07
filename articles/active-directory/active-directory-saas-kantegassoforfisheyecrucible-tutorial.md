---
title: "Självstudier: Azure Active Directory-integrering med Kantega SSO för FishEye/genom | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kantega SSO för FishEye/genom."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: fdd68b5e90c3e2893887650735429a33e21ffa68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Självstudier: Azure Active Directory-integrering med Kantega SSO för FishEye/genom

I kursen får du lära dig hur toointegrate Kantega SSO för FishEye/genom Azure Active Directory (AD Azure).

Integrera Kantega SSO för FishEye/genom med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooKantega enkel inloggning för FishEye/genom
- Du kan aktivera din användare tooautomatically get inloggade tooKantega enkel inloggning för FishEye/genom (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Kantega SSO för FishEye/genom måste hello följande objekt:

- En Azure AD-prenumeration
- En Kantega SSO för FishEye/genom enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Kantega SSO för FishEye/genom från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-kantega-sso-for-fisheyecrucible-from-hello-gallery"></a>Att lägga till Kantega SSO för FishEye/genom från hello-galleriet
tooconfigure hello-integrering av Kantega SSO för FishEye/genom Azure AD, behöver du tooadd Kantega SSO för FishEye/genom hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Kantega SSO för FishEye/genom från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Kantega SSO för FishEye/genom**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

5. Markera hello resultat på panelen **Kantega SSO för FishEye/genom**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kantega SSO för FishEye/genom baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Kantega SSO för FishEye/genom är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren Kantega SSO för FishEye/genom toobe upprättas.

I Kantega SSO för FishEye/genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Kantega SSO för FishEye/genom, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en Kantega SSO för FishEye/genom testanvändare](#creating-a-kantega-sso-for-fisheyecrucible-test-user)**  -toohave en motsvarighet för Britta Simon Kantega SSO för FishEye/genom som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Kantega SSO för FishEye/genom programmet.

**tooconfigure Azure AD enkel inloggning med Kantega SSO för FishEye/genom att utföra hello följande steg:**

1. I hello Azure-portalen på hello **Kantega SSO för FishEye/genom** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

3. I **IDP** initierade läge på hello **Kantega SSO FishEye/genom domän och URL: er** avsnittet utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. I **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Dessa värden tas emot under hello konfigurationen av FishEye/genom plugin-programmet som beskrivs senare i hello kursen.

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
7. Logga in tooyour FishEye/genom på lokal server som administratör i ett annat webbläsarfönster.

8. Hovra över kugge och klicka på hello **tillägg**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon1.png)

9. Klicka under systeminställningar avsnitt **söka efter nya tillägg**. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/add-on2.png)

10. Sök **Kantega SSO för genom** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon2.png)

11. hello plugin-installationen startar. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon33.png)

12. När hello installationen är klar. Klicka på **Stäng**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon34.png)

13. Klicka på **Hantera**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon35.png)

14. Klicka på **konfigurera** tooconfigure hello nytt plugin-program.  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon3.png)

15. I hello **SAML** avsnitt. Välj **Azure Active Directory (AD Azure)** från hello **Lägg till identitetsleverantör** listrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon4.png)

16. Välj prenumerationsnivån som **grundläggande**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon5.png)

17. På hello **appegenskaper** avsnittet, gör du följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Kopiera hello **App-ID URI** värde och använda det som **identifierare, Reply URL och inloggnings-URL** på hello **Kantega SSO FishEye/genom domän och URL: er** avsnitt i Azure-portalen.

    b. Klicka på **Nästa**.

18. På hello **Metadata importera** avsnittet, gör du följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Välj **metadatafil på datorn**, och överföra metadata-fil som du har hämtat från Azure-portalen.

    b. Klicka på **Nästa**.

19. På hello **och enkel inloggning** avsnittet, gör du följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Lägg till namnet på hello identitetsleverantör i **identitet providernamn** textruta (t.ex Azure AD).

    b. Klicka på **Nästa**.

20. Kontrollera hello signeringscertifikat och klicka på **nästa**.    

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon9.png)

21. På hello **FishEye användarkonton** avsnittet, gör du följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Välj **skapa användare i Fisheyes interna katalogen om det behövs** och ange hello rätt namn i hello gruppen för användare (kan vara flera Nej. av (grupper avgränsade med kommatecken).

    b. Klicka på **Nästa**.

22. Klicka på **Slutför**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon11.png)

23. På hello **kända domäner för Azure AD** avsnittet, gör du följande:   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Välj **kända domäner** hello vänstra panelen av hello-sidan.

    b. Ange domännamnet i hello **kända domäner** textruta.

    c. Klicka på **Spara**.  

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a>Skapa en Kantega SSO för FishEye/genom testanvändare

tooenable Azure AD-användare toolog i tooFishEye/genom de att etableras i FishEye/genom. I Kantega SSO för FishEye/genom är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour genom på lokal server som administratör.

2. Hovra över kugge och klicka på hello **användare**.

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user1.png) 

3. Under **användare** avsnittet klickar du på **Lägg till användare**.

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user2.png)

4. På hello **Lägg till nya användare** dialogrutan utför hello följande steg:

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user3.png) 

    a. I hello **användarnamn** textruta hello e-post för användare som Brittasimon@contoso.com.
    
    b. I hello **visningsnamn** textruta visningsnamn för hello användare som Britta Simon.
    
    c. I hello **e-postadress** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.

    d. I hello **lösenord** textruta typen hello lösenord för användare.  

    e. I hello **Bekräfta lösenord** textruta hello ange lösenordet för användaren.

    f. Klicka på **Lägg till**.   

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKantega enkel inloggning för FishEye/genom.

![Tilldela användare][200] 

**tooassign Britta Simon tooKantega SSO för FishEye/genom att utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Kantega SSO för FishEye/genom**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Kantega SSO för FishEye/genom panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Kantega SSO för FishEye/genom programmet.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

