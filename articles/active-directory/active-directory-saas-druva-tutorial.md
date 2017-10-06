---
title: "Självstudier: Azure Active Directory-integrering med Druva | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Druva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Självstudier: Azure Active Directory-integrering med Druva

I kursen får du lära dig hur toointegrate Druva med Azure Active Directory (AD Azure).

Integrera Druva med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooDruva.
- Du kan låta dina användare tooautomatically get inloggade tooDruva (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Druva, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Druva enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Druva från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-druva-from-hello-gallery"></a>Att lägga till Druva från hello-galleriet
tooconfigure hello integrering av Druva i Azure AD, behöver du tooadd Druva hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Druva från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Druva**väljer **Druva** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Druva i hello resultatlistan](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Druva baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Druva är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Druva toobe upprättas.

I Druva, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Druva, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Druva](#create-a-druva-test-user)**  -toohave en motsvarighet för Britta Simon i Druva som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Druva program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Druva:**

1. I hello Azure-portalen på hello **Druva** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. På hello **Druva domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    I hello **inloggnings-URL** textruta typen hello URL:`https://cloud.druva.com/home`

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. Tillämpningsprogrammet Druva förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** konfiguration. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i föregående bild hello och utföra hello följande steg:

    | Attributets namn      | Attributvärdet      |
    | ------------------- | -------------------- |
    | synkroniserad\_auth\_token |Ange hello token genererade värde |
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden. hello token genererat värde beskrivs senare i självstudiekursen.
    
    d. Klicka på **OK**.    

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. På hello **Druva Configuration** klickar du på **konfigurera Druva** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. Logga in tooyour Druva företagets webbplats som en administratör i en annan webbläsarfönster.

10. Gå för**hantera \> inställningar**.

    ![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795091.png "inställningar")

11. Dialogrutan Inställningar för enkel inloggning hello utför hello följande steg:

    ![Enkel inloggning inställningar](./media/active-directory-saas-druva-tutorial/ic795092.png "enkel inloggning inställningar")
    
    a. Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **ID providern inloggnings-URL** textruta.
    
    b. Klistra in **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **ID providern logga ut URL** textruta.
    
     c. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **ID providern certifikat** textruta
     
     d. tooopen hello **inställningar** klickar du på **spara**.

12. På hello **inställningar** klickar du på **generera SSO Token**.

    ![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795093.png "inställningar")

13. På hello **enkel inloggning autentiseringstoken** dialogrutan utföra hello följande steg:

    ![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO-Token")
    
    a. Klicka på **kopiera**, klistra in kopierade värdet i hello **värdet** TextBox-kontroll i hello **lägga till attributet** avsnitt.
    
    b. Klicka på **Stäng**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-druva-test-user"></a>Skapa en testanvändare Druva

I ordning tooenable Azure AD-användare toolog i tooDruva, måste de etableras i Druva. Hello gäller Druva är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Druva** företagets webbplats som administratör.

2. Gå för**hantera \> användare**.
   
   ![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795097.png "hantera användare")

3. Klicka på **skapa nya**.
   
   ![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795098.png "hantera användare")

4. Dialogrutan Skapa ny användare hello utför hello följande steg:
   
   ![Skapa ny användare](./media/active-directory-saas-druva-tutorial/ic795099.png "Skapa ny användare")
   
   a. I hello **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .
   
   b. I hello **namn** textruta anger hello namnet på användaren som **BrittaSimon**.
   
   c. Klicka på **skapa användare**.

>[!NOTE]
>Du kan använda något annat Druva användarens konto skapas verktyg eller API: er som tillhandahålls av Druva tooprovision användarkonton i Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDruva.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooDruva utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Druva**.

    ![Hej Druva länken i listan med program hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Druva programmet när du klickar på hello Druva panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

