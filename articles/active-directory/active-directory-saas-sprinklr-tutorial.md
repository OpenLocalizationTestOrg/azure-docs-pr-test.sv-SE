---
title: "Självstudier: Azure Active Directory-integrering med Sprinklr | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 6e1622cd55e3b0e8063604ac9dc0cb0673fa9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Självstudier: Azure Active Directory-integrering med Sprinklr

I kursen får lära du att integrera Sprinklr med Azure Active Directory (AD Azure).

Integrera Sprinklr med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Sprinklr
- Du kan aktivera användarna att automatiskt hämta loggat in på Sprinklr (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Sprinklr, behöver du följande:

- En Azure AD-prenumeration
- En Sprinklr enkel inloggning på aktiverade prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Sprinklr från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sprinklr-from-the-gallery"></a>Att lägga till Sprinklr från galleriet
Du måste lägga till Sprinklr från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Sprinklr i Azure AD.

**Utför följande steg för att lägga till Sprinklr från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Sprinklr**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Välj i resultatpanelen **Sprinklr**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sprinklr baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Sprinklr motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Sprinklr upprättas.

I Sprinklr, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Sprinklr, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Sprinklr](#creating-a-sprinklr-test-user)**  – du har en motsvarighet för Britta Simon i Sprinklr som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Sprinklr program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Sprinklr:**

1. I Azure-portalen på den **Sprinklr** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. På den **Sprinklr domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.sprinklr.com`

    b. I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera värdet med det faktiska inloggnings-URL och identifierare. Kontakta [Sprinklr klienten supportteamet](https://www.sprinklr.com/contact-us/) att hämta dessa värden. 
 
4. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. På den **Sprinklr Configuration** klickar du på **konfigurera Sprinklr** att öppna **konfigurera inloggning** fönster. Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**

7. I en annan webbläsarfönster loggar du in på webbplatsen Sprinklr företag som administratör.

8. Gå till **Administration \> inställningar**.
   
    ![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")

9. Gå till **hantera Partner \> för enkel inloggning** på i den vänstra rutan.
   
    ![Hantera Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "hantera Partner")

10. Klicka på **+ Lägg till en enda inloggningar**.
   
    ![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "enkel inloggning")

11. På den **för enkelinloggning** utför följande steg:
   
    ![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "enkel inloggning")

    a. I den **namn** textruta, ange ett namn för konfigurationen (till exempel: *WAADSSOTest*).

    b. Välj **aktiverat**.

    c. Välj **nya SSO certifikatet**.
             
    e. Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **providern identitetscertifikat** textruta.

    f. Klistra in den **SAML enhets-ID** värde som du har kopierat från Azure Portal till den **enhets-Id** textruta.

    g. Klistra in den **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från Azure Portal till den **identitet providern inloggnings-URL** textruta.

    h. Klistra in den **Sign-Out URL** värde som du har kopierat från Azure Portal till den **identitet providern logga ut URL** textruta.
     
    Jag. Som **SAML-ID typ**väljer **Assertion innehåller användare ”s sprinklr.com användarnamn**.

    j. Som **SAML Användarplats ID**väljer **användar-ID är i elementet namnidentifierare i instruktionen ämne**.

    k. Klicka på **Spara**.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sprinklr-test-user"></a>Skapa en testanvändare Sprinklr

1. Logga in på webbplatsen Sprinklr företag som administratör.

2. Gå till **Administration \> inställningar**.
   
    ![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")

3. Gå till **hantera klienten \> användare** i den vänstra rutan.
   
    ![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "inställningar")

4. Klicka på **lägga till användare**.
   
    ![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "inställningar")

5. På den **Redigera användare** dialogrutan, utför följande steg:
   
    ![Redigera användare](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Redigera användare") 

    a. I den **e-post**, **Förnamn** och **efternamn** textrutor, ange information för en Azure AD-användarkonto som du vill etablera.

    b. Välj **lösenord inaktiveras**.

    c. Välj **språk**.

    d. Välj **användartyp**.

    e. Klicka på **uppdatering**.
   
     >[!IMPORTANT]
     >**Lösenord inaktiveras** måste väljas för att aktivera en användare loggar in via en identitetsleverantör. 
     
6. Gå till **rollen**, och utför sedan följande steg:
   
    ![Samarbeta roller](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner roller")

    a. Från den **Global** väljer **alla\_behörigheter**.  

    b. Klicka på **uppdatering**.

>[!NOTE]
>Du kan använda något annat Sprinklr användarens konto skapas verktyg eller API: er som tillhandahålls av Sprinklr att etablera Azure AD-användarkonton. 

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Sprinklr.

![Tilldela användare][200] 

**Om du vill tilldela Sprinklr Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Sprinklr**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Sprinklr på åtkomstpanelen du bör få automatiskt loggat in på ditt program Sprinklr för mer information om panelen åtkomst finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

