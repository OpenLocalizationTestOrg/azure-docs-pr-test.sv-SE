---
title: "Självstudier: Azure Active Directory-integrering med Voyance | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Voyance."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: ea3f8ff903c051ac2147408092e6f35421ed4011
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a>Självstudier: Azure Active Directory-integrering med Voyance

I kursen får lära du att integrera Voyance med Azure Active Directory (AD Azure).

Integrera Voyance med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Voyance
- Du kan aktivera användarna att automatiskt hämta loggat in på Voyance (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Voyance, behöver du följande:

- En Azure AD-prenumeration
- En Voyance enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Voyance från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-voyance-from-the-gallery"></a>Att lägga till Voyance från galleriet
Du måste lägga till Voyance från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Voyance i Azure AD.

**Utför följande steg för att lägga till Voyance från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet Enterprise program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **Voyance**väljer **Voyance** resultatet-panelen klickar **Lägg till** för att lägga till programmet.

    ![Voyance i resultatlistan](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Voyance baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Voyance motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Voyance upprättas.

I Voyance, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Voyance, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Voyance](#create-a-voyance-test-user)**  – du har en motsvarighet för Britta Simon i Voyance som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Voyance program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Voyance:**

1. I Azure-portalen på den **Voyance** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_samlbase.png)

3. På den **Voyance domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![Enkel inloggning information för IDP Voyance domän och URL: er](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url1.png)

    a. I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.nyansa.com`

    b. I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.nyansa.com/saml/create/`

4. Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![URL: er och Voyance domän med enkel inloggning information för SP](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url2.png)

    I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.nyansa.com/`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL. Kontakta [Voyance klienten supportteamet](mailto:support@nyansa.com) att hämta dessa värden. 

5. På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.

    ![Länken hämta certifikatet](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-voyance-tutorial/tutorial_general_400.png)
    
7. På den **Voyance Configuration** klickar du på **konfigurera Voyance** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**

    ![Voyance konfiguration](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_configure.png) 

8. I en annan webbläsarfönster inloggning till Voyance-klient som administratör.

9. Gå till det övre högra hörnet av navigeringsfältet och klicka på listrutan som säger ”**ABC University**”.
    
    ![Konfigurera enkel inloggning på App sida ABC University](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

10. Klicka på ”**administrationsinställningar**”.

    ![Konfigurera enkel inloggning på App-sida Admin-Inställningar](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

11. Klicka på ”**användaråtkomst**” fliken.

    ![Konfigurera enkel inloggning på App-sida-användaråtkomst](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

12. Klicka på den ”**SSO inaktiveras**” för att konfigurera Azure AD som en IdP använder SAML 2.0.

    ![Konfigurera enkel inloggning på App sida SSO inaktiveras knappen](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

13. Gå till **SAML v2** avsnittet och utföra nedanstående steg:

    ![Konfigurera enkel inloggning på App-sida SAML v2](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
    
    a. Välj **aktiverat**.
    
    b. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i den **IdP inloggnings-URL** textruta.

    c. Öppna din hämtade Base64-kodat certifikat i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **IdP Cert** textruta.
    
    d. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Azure Active Directory-knappen](./media/active-directory-saas-voyance-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-voyance-tutorial/create_aaduser_02.png) 

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Knappen Lägg till](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Dialogrutan användare](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-voyance-test-user"></a>Skapa en testanvändare Voyance

Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Voyance. Voyance stöder just-in-time-etablering, vilket är aktiverat som standard. Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök att komma åt Voyance om den inte finns.

>[!NOTE]
>Om du behöver skapa en användare manuellt, måste du kontakta [Voyance supportteamet](maiLto:support@nyansa.com).

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Voyance.

![Tilldela rollen][200]

**Om du vill tilldela Voyance Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Voyance**.

    ![Länken Voyance i listan med program](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Voyance på åtkomstpanelen du bör få automatiskt loggat in på ditt Voyance program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png

