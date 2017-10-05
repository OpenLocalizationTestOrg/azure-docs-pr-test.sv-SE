---
title: "Självstudier: Azure Active Directory-integrering med Pingboard | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Självstudier: Azure Active Directory-integrering med Pingboard

I kursen får lära du att integrera Pingboard med Azure Active Directory (AD Azure).

Integrera Pingboard med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Pingboard
- Du kan aktivera användarna att automatiskt hämta loggat in på Pingboard (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Pingboard, behöver du följande:

- En Azure AD-prenumeration
- En Pingboard enkel inloggning på aktiverade prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Pingboard från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-pingboard-from-the-gallery"></a>Att lägga till Pingboard från galleriet
Du måste lägga till Pingboard från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Pingboard i Azure AD.

**Utför följande steg för att lägga till Pingboard från galleriet:**

1. I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Pingboard**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. Välj i resultatpanelen **Pingboard**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pingboard baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Pingboard motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Pingboard upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Pingboard.

Om du vill konfigurera och testa Azure AD enkel inloggning med Pingboard, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Pingboard](#creating-a-pingboard-test-user)**  – du har en motsvarighet för Britta Simon i Pingboard som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Pingboard program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Pingboard:**

1. I Azure-hanteringsportalen på den **Pingboard** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. På den **Pingboard domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. I den **identifierare** textruta Skriv värdet som:`http://<entity-id>.pingboard.com/sp`

    b. I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Observera att detta inte är verkliga värden. Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL. Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren. Kontakta [Pingboard klienten supportteamet](https://support.pingboard.com/) att hämta dessa värden. 

4. Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. I den **inloggnings-URL** textruta Skriv värdet som:`http://<sub-domain>.pingboard.com/sign_in`
     
5. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. Konfigurera enkel inloggning på Pingboard sida, öppna ett nytt webbläsarfönster och logga in på ditt konto för Pingboard. Du måste vara en Pingboard-administratör för att konfigurera enkel inloggning.

8. Välj den översta menyn **appar > integreringar**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  På den **integreringar** kan hitta den **”Azure Active Directory”** panelen och klicka på den.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. I modal som följer Klicka **”konfigurera”**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. På följande sida ser du att ”Azure SSO-integrering är aktiverad”.. Öppna den hämta filen i XML-Metadata i en anteckningar och klistra in innehållet i **IDP Metadata**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. Filen kommer att valideras och om allt är korrekt, enkel inloggning på nu aktiveras

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-pingboard-test-user"></a>Skapa en testanvändare Pingboard

För att aktivera Azure AD-användare att logga in på Pingboard etableras de i Pingboard.  
När det gäller Pingboard är etablering en manuell aktivitet.

**Utför följande steg för att etablera en användarkonton:**

1. Logga in på webbplatsen Pingboard företag som administratör.

2. Klicka på **”Lägg till medarbetare”** knappen på **Directory** sidan.

    ![Lägga till medarbetare](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. På den **”Lägg till medarbetare”** dialogrutan utför följande steg.

    ![Bjud in personer](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. I den **fullständiga namn** textruta skriver du det fullständiga namnet på Britta Simon.

    b. I den **e-post** textruta skriver Britta Simon konto e-postadress.

    c. I den **befattning** textruta skriver Britta Simon befattningen.

    d. I den **plats** listrutan, välj platsen för Britta Simon.
    
    e. Klicka på **Lägg till**.   

4. En bekräftelseskärm kommer nu att bekräfta att lägga till användaren.
    
    ![Bekräfta](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Pingboard.

![Tilldela användare][200] 

**Om du vill tilldela Pingboard Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Pingboard**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Pingboard på åtkomstpanelen du bör få automatiskt loggat in på ditt Pingboard program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
