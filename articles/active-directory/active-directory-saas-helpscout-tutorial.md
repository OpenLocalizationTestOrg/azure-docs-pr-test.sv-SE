---
title: "Självstudier: Azure Active Directory-integrering med hjälp Scout | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och hjälpa Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Självstudier: Azure Active Directory-integrering med hjälp Scout

I kursen får lära du att integrera hjälpa Scout med Azure Active Directory (AD Azure).

Du kan få följande fördelar från hjälp Scout integrera med Azure AD:

- Du kan styra vem som har åtkomst till hjälpen Scout i Azure AD.
- Du kan logga in automatiskt Scout hjälpa användarna med enkel inloggning och användarens Azure AD-kontot.
- Du kan hantera dina konton i en enda central plats och Azure-portalen.

Läs mer om programvara som en tjänst (SaaS) appintegrering med Azure AD i [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

Om du vill konfigurera Azure AD-integrering med hjälp Scout behöver du följande:

- En Azure AD-prenumeration
- En prenumeration som hjälper Scout med enkel inloggning aktiverad 

> [!NOTE]
> Om du testar stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.

Rekommendationer för att testa stegen i den här självstudiekursen:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. 

Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till hjälp Scout från galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-help-scout-from-the-gallery"></a>Lägg till hjälp Scout från galleriet
Lägg till hjälp Scout i listan över hanterade SaaS-appar för att ställa in att Scout med Azure AD-integrering i galleriet.

Lägg till hjälp Scout från galleriet:

1. I den [Azure-portalen](https://portal.azure.com), i den vänstra menyn, Välj **Azure Active Directory**. 

    ![Azure Active Directory-knappen][1]

2. Välj **företagsprogram**, och välj sedan **alla program**.

    ![Sidan Enterprise program][2]
    
3. Om du vill lägga till ett nytt program, Välj **nytt program**.

    ![Knappen Nytt program][3]

4. I sökrutan anger **hjälp Scout**. I sökresultaten väljer **hjälp Scout**, och välj sedan **Lägg till**.

    ![Hjälp Scout i resultatlistan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hjälp Scout baserat på en användare med namnet *Britta Simon*.

För enkel inloggning ska fungera, Azure AD som behöver veta Azure AD motsvarighet användaren i Hjälp Scout. En länk förhållandet mellan en Azure AD-användare och relaterade användaren i Hjälp Scout måste upprättas.

Etablera länk-relationen i Hjälp Scout för **användarnamn**, tilldela värdet för den **användarnamn** i Azure AD.

Om du vill konfigurera och testa Azure AD enkel inloggning med hjälp Scout, utför följande uppgifter:

1. [Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on). Ställer in en användare att använda den här funktionen.
2. [Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user). Testerna Azure AD enkel inloggning med användaren Britta Simon.
3. [Skapa en testanvändare hjälpa Scout](#create-a-help-scout-test-user). Skapar en motsvarighet för Britta Simon i Hjälp Scout som är kopplad till Azure AD-representation av användaren.
4. [Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user). Ställer in Britta Simon använda Azure AD för enkel inloggning.
5. [Testa enkel inloggning](#test-single-sign-on). Kontrollerar att konfigurationen fungerar.

### <a name="set-up-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet kan du ställa in Azure AD enkel inloggning i Azure-portalen. Sedan kan konfigurera du enkel inloggning i tillämpningsprogrammet att Scout.

Konfigurera Azure AD enkel inloggning med hjälp Scout:

1. I Azure-portalen på den **hjälp Scout** programmet integration anger **enkel inloggning**.
 
    ![Konfigurera enkel inloggning länk][4]

2. På den **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Under **hjälp Scout domän och URL: er**, om du vill installera programmet i IDP-initierad läge fullständig följande steg:

    1. I den **identifierare** ange en URL som har följande mönster:`urn:auth0:helpscout:<instancename>`

    2. I den **Reply URL** ange en URL som har följande mönster:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Om du vill installera programmet i SP-initierad läge, väljer du den **visa avancerade inställningar för URL: en** och gör sedan följande:

    * I den **inloggning URL** ange en URL som har följande format:`https://secure.helpscout.net/members/login/`

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > Värdena i dessa URL: er är bara exempel. Uppdatera värdena med faktiska Identifierare och reply-URL. För att få dessa värden kan kontakta [hjälp Scout supportteamet](mailto:help@helpscout.com). 

5. Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och spara sedan metadatafilen på datorn.

    ![Länken hämta certifikatet](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Välj **Spara**.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. Om du vill konfigurera enkel inloggning på sidan hjälp Scout, skicka hämtade metadata XML-filen till den [hjälp Scout supportteamet](mailto:help@helpscout.com). Support-teamet hjälper Scout gäller den här inställningen så att SAML enkel inloggning anslutningen är korrekt inställda på båda sidor.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen! När du har lagt till appen genom att välja **Active Directory** > **företagsprogram**, Välj den **enkel inloggning** fliken. Du kan komma åt inbäddade dokumentation i den **Configuration** avsnittet längst ned på sidan. Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

I det här avsnittet i Azure-portalen kan du skapa en testanvändare med namnet Britta Simon.

![Skapa en testanvändare i Azure AD][100]

Skapa en testanvändare i Azure AD:

1. Välj i Azure-portalen på den vänstra menyn **Azure Active Directory**.

    ![Azure Active Directory-knappen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.

    ![Välj användare och grupper och välj sedan alla användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Öppna den **användare** dialogrutan överst i den **alla användare** väljer **Lägg till**.

    ![Knappen Lägg till](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. I den **användaren** dialogrutan rutan, gör du följande:

    1. I den **namn** ange **BrittaSimon**.

    2. I den **användarnamn** ange användarens Britta Simon e-postadress.

    3. Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.

    4. Välj **Skapa**.

        ![Dialogrutan användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Skapa en hjälp Scout testanvändare

Syftet med det här avsnittet är att skapa en användare med namnet Britta Simon i Hjälp Scout. Hjälp Scout stöder just-in-time (JIT) etablering, som är aktiverad som standard.

I det här avsnittet finns det ingen åtgärd eller åtgärden har slutförts. Om en användare inte redan finns i Hjälp Scout, skapas en ny när du försöker komma åt Hjälp Scout.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet tillåter användaren Britta Simon att använda Azure AD enkel inloggning genom att bevilja användarkontoåtkomst till hjälpen Scout.

![Tilldela rollen][200] 

Tilldela Britta Simon hjälpa Scout:

1. Öppna vyn program i Azure-portalen och sedan gå till vyn directory. Välj **företagsprogram**, och välj sedan **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **hjälp Scout**.

    ![Länken Hjälp Scout i listan med program](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Välj i den vänstra menyn **användare och grupper**.

    ![Länka användare och grupper][202]

4. Välj **Lägg till**. Klicka sedan på den **Lägg uppdrag** väljer **användare och grupper**.

    ![Fönstret Lägg till tilldelning][203]

5. På den **användare och grupper** sida i listan över användare, Välj **Britta Simon**.

6. På den **användare och grupper** väljer **Välj**.

7. På den **Lägg uppdrag** väljer **tilldela**.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.

När du väljer att Scout panelen på åtkomstpanelen bör du vara automatiskt inloggad i tillämpningsprogrammet att Scout.

Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

