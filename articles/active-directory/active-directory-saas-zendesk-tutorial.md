---
title: "Självstudier: Azure Active Directory-integrering med Zendesk | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: 095fdd68cafbb1bf7d753dd82821bdc2fd089ef0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Självstudier: Azure Active Directory-integrering med Zendesk

I kursen får lära du att integrera Zendesk med Azure Active Directory (AD Azure).

Integrera Zendesk med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Zendesk.
- Du kan aktivera användarna att automatiskt hämta loggat in på Zendesk (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - Azure-portalen.

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Zendesk, behöver du följande:

- En Azure AD-prenumeration
- En Zendesk enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zendesk från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zendesk-from-the-gallery"></a>Att lägga till Zendesk från galleriet
Du måste lägga till Zendesk från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zendesk i Azure AD.

**Utför följande steg för att lägga till Zendesk från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet Enterprise program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **Zendesk**väljer **Zendesk** resultatet-panelen klickar **Lägg till** för att lägga till programmet.

    ![Zendesk i resultatlistan](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zendesk baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Zendesk motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zendesk upprättas.

I Zendesk, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Zendesk, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Zendesk](#create-a-zendesk-test-user)**  – du har en motsvarighet för Britta Simon i Zendesk som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zendesk-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zendesk:**

1. I Azure-portalen på den **Zendesk** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. På den **Zendesk domän och URL: er** avsnittet, utför följande steg:

    ![URL: er och Zendesk-domän med enkel inloggning information](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.zendesk.com`

    b. I den **identifierare** textruta Skriv det värde som använder följande mönster:`<subdomain>.zendesk.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare. Kontakta [Zendesk klienten supportteamet](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) att hämta dessa värden. 
 
4. På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.

    ![Länken hämta certifikatet](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png)

5. Zendesk förväntar SAML-intyg i ett specifikt format. Det finns inga obligatoriska SAML-attribut men om du vill kan du lägga till ett attribut från **användarattribut** avsnittet genom att följa de nedanstående steg: 

     ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning Lägg till](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning addattb](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    b. I den **namn** textruta ange attributets namn visas för den raden.

    c. Från den **värdet** listan, ange det attributvärde som visas för den raden.
    
    d. Klicka på **OK**.
 
    > [!NOTE] 
    > Du kan använda Tilläggsattribut för att lägga till attribut som inte ingår i Azure AD som standard. Klicka på [användarattribut som kan anges i SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) för att hämta den fullständiga listan med SAML attribut som **Zendesk** accepterar.  

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zendesk-tutorial/tutorial_general_400.png)

7. På den **Zendesk Configuration** klickar du på **konfigurera Zendesk** att öppna **konfigurera inloggning** fönster. Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**

    ![Zendesk-konfiguration](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

8. Logga in på webbplatsen Zendesk företag som en administratör i en annan webbläsarfönster.

9. Klicka på **Admin**.

10. I det vänstra navigeringsfönstret klickar du på **inställningar**, och klicka sedan på **säkerhet**.

11. På den **säkerhet** utför följande steg: 
   
     ![Säkerhet](./media/active-directory-saas-zendesk-tutorial/ic773089.png "säkerhet")

    ![Enkel inloggning](./media/active-directory-saas-zendesk-tutorial/ic773090.png "enkel inloggning")

     a. Klicka på den **Admin & agenter** fliken.

     b. Välj **enkel inloggning (SSO) och SAML**, och välj sedan **SAML**.

     c. I **SAML SSO URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen. 

     d. I **Remote logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
        
     e. I **certifikat fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.
     
     f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png)

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png)

4. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** Skriv användarens Britta Simon e-postadress.

    c. Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-zendesk-test-user"></a>Skapa en testanvändare Zendesk

Aktivera Azure AD-användare att logga in på **Zendesk**, de måste etableras i **Zendesk**.  
Beroende på vilken roll som tilldelats i apparna, är det förväntat beteende:

 1. **Slutanvändarens** konton tillhandahålls automatiskt när du loggar in.
 2. **Agenten** och **Admin** konton måste tillhandahållas manuellt i **Zendesk** innan du loggar in.
 
**Utför följande steg om du vill konfigurera ett användarkonto:**

1. Logga in på ditt **Zendesk** klient.

2. Välj den **lista över kunder** fliken.

3. Välj den **användare** och på **Lägg till**.
   
    ![Lägg till användare](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Lägg till användare")
4. Typ av **namn** och **e-post** för ett befintligt Azure AD-konto du vill etablera och klicka sedan på **spara**.
   
    ![Ny användare](./media/active-directory-saas-zendesk-tutorial/ic773633.png "ny användare")

> [!NOTE]
> Du kan använda något annat Zendesk användarens konto skapas verktyg eller API: er fås från Zendesk till etablera AAD-användarkonton.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zendesk.

![Tilldela rollen][200] 

**Om du vill tilldela Zendesk Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Zendesk**.

    ![Länken Zendesk i listan med program](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png)  

3. Klicka på menyn till vänster **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Zendesk på åtkomstpanelen du bör få automatiskt loggat in på ditt Zendesk-program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png

