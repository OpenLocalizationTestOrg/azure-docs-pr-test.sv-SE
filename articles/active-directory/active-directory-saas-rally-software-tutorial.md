---
title: "Självstudier: Azure Active Directory-integrering med Rally programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Rally programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: c75c8b98ce7fab19964c13de5ad7e19ef3ebd0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Självstudier: Azure Active Directory-integrering med Rally programvara

I kursen får du lära dig hur toointegrate Rally programvara med Azure Active Directory (AD Azure).

Integrera Rally programvara med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooRally programvara.
- Du kan låta dina användare tooautomatically get inloggade tooRally programvara (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Rally programvara, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Rally programvara enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Rally programvara från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-rally-software-from-hello-gallery"></a>Lägga till Rally programvara från hello-galleriet
tooconfigure hello integrering av Rally programvara i Azure AD, behöver du tooadd Rally programvara från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd Rally programvara från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Rally programvara**väljer **Rally programvara** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Rally programvara i hello resultatlistan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Rally programvara baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Rally programvaran är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Rally programvara toobe upprätta.

Tilldela hello värdet hello Rally programvaran **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Rally programvara, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Rally programvara](#create-a-rally-software-test-user)**  -toohave en motsvarighet för Britta Simon Rally programvara som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Rally programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Rally programvara:**

1. I hello Azure-portalen på hello **Rally programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. På hello **Rally programvara domän och URL: er** avsnittet, utföra hello följande steg:

    ![Rally programvara domän och URL: er enkel inloggning information](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.rally.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.rally.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Rally Programvaruklienten supportteamet](https://help.rallydev.com/) tooget dessa värden. 
 


4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. På hello **Rally programvarukonfiguration** klickar du på **konfigurera Rally programvara** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**

    ![Rally konfiguration av programvara](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. Logga in tooyour **Rally programvara** klient.

8. Klicka i hello verktygsfältet hello längst upp **installationsprogrammet**, och välj sedan **prenumeration**.
   
    ![Prenumerationen](./media/active-directory-saas-rally-software-tutorial/ic769531.png "prenumeration")

9. Klicka på hello **åtgärd** knappen. Välj **redigera prenumeration** på hello upp till höger i hello verktygsfält.

10. På hello **prenumeration** dialogrutan sida, utför följande steg hello och klicka sedan på **spara och Stäng**:
   
    ![Autentisering](./media/active-directory-saas-rally-software-tutorial/ic769542.png "autentisering")
   
    a. Välj **autentisering Rally eller SSO** autentisering listrutan.

    b. I hello **identitet providern URL** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen. 

    c. I hello **SSO logga ut** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-rally-software-test-user"></a>Skapa en testanvändare Rally programvara

Azure AD-användare toobe kan toosign i, måste de vara etablerade toohello Rally program med hjälp av deras användarnamn för Azure Active Directory.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour Rally programvara klient.

2. Gå för**installationsprogrammet \> användare**, och klicka sedan på **+ Lägg till ny**.
   
    ![Användare](./media/active-directory-saas-rally-software-tutorial/ic781039.png "användare")

3. Skriv hello namn i textrutan för hello ny användare och klicka sedan på **Lägg till med uppgifter**.

4. I hello **skapa användare** avsnittet, utföra hello följande steg:
   
    ![Skapa användare](./media/active-directory-saas-rally-software-tutorial/ic781040.png "skapa användare")

    a. I hello **användarnamn** textruta hello-typnamn för användaren som **Brittsimon**.
   
    b. I **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .

    c. I **Förnamn** text Ange hello första namn för användaren som **Britta**.

    d. I **efternamn** text Ange hello efternamn för användaren som **Simon**.

    e. Klicka på **spara och Stäng**.

   >[!NOTE]
   >Du kan använda alla andra Rally användaren konto skapas verktyg eller API: er som tillhandahålls av Rally programvara tooprovision användarkonton i Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRally programvara.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooRally programvara, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Rally programvara**.

    ![hello Rally programvarulänk i listan med program hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Rally program när du klickar på hello Rally programvara panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

