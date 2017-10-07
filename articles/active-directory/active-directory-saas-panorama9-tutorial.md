---
title: "Självstudier: Azure Active Directory-integrering med Panorama9 | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Panorama9."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 548fb6434d920e076db98a0193f8dfdf8a958a91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Självstudier: Azure Active Directory-integrering med Panorama9

I kursen får du lära dig hur toointegrate Panorama9 med Azure Active Directory (AD Azure).

Integrera Panorama9 med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPanorama9
- Du kan aktivera din användare tooautomatically get inloggade tooPanorama9 (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Panorama9, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Panorama9 enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Panorama9 från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-panorama9-from-hello-gallery"></a>Att lägga till Panorama9 från hello-galleriet
tooconfigure hello integrering av Panorama9 i Azure AD, behöver du tooadd Panorama9 hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Panorama9 från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Panorama9**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. Markera hello resultat på panelen **Panorama9**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Panorama9 baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Panorama9 är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Panorama9 toobe upprättas.

I Panorama9, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Panorama9, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Panorama9](#creating-a-panorama9-test-user)**  -toohave en motsvarighet för Britta Simon i Panorama9 som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Panorama9 program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Panorama9:**

1. I hello Azure-portalen på hello **Panorama9** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. På hello **Panorama9 domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    a. I hello **inloggnings-URL** textruta Skriv en URL som:`https://dashboard.panorama9.com/saml/access/3262`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://www.panorama9.com/saml20/<tenant-name>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Panorama9 klienten supportteamet](https://support.panorama9.com) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. På hello **Panorama9 Configuration** klickar du på **konfigurera Panorama9** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. Logga in på webbplatsen Panorama9 företag som en administratör i en annan webbläsarfönster.

6. Klicka i hello verktygsfältet hello längst upp **hantera**, och klicka sedan på **tillägg**.
   
   ![Tillägg](./media/active-directory-saas-panorama9-tutorial/ic790023.png "tillägg")
7. På hello **tillägg** dialogrutan klickar du på **enkel inloggning**.
   
   ![Enkel inloggning](./media/active-directory-saas-panorama9-tutorial/ic790024.png "enkel inloggning")
8. I hello **inställningar** avsnittet, utföra hello följande steg:
   
   ![Inställningar för](./media/active-directory-saas-panorama9-tutorial/ic790025.png "inställningar")
   
    a. I **identitet providern URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
   
    b. I **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.    
         
9. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-panorama9-test-user"></a>Skapa en testanvändare Panorama9

I ordning tooenable Azure AD-användare toolog i Panorama9, måste de etableras i Panorama9.  

Hello gäller Panorama9 är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Panorama9** företagets webbplats som administratör.

2. Hello-menyn överst hello **hantera**, och klicka sedan på **användare**.
   
  ![Användare](./media/active-directory-saas-panorama9-tutorial/ic790027.png "användare")

3. I hello avsnittet användare, klickar du på  **+**  tooadd ny användare.

 ![Användare](./media/active-directory-saas-panorama9-tutorial/ic790028.png "användare")

4. Gå toohello användaren dataavsnittet typen hello e-postadressen till en giltig Azure Active Directory-användare som du vill tooprovision till hello **e-post** textruta.

5. Komma toohello avsnittet för användare, klicka på **spara**.
   
> [!NOTE]
    > hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPanorama9.

![Tilldela användare][200] 

**tooassign Britta Simon tooPanorama9 utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Panorama9**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooPanorama9 programmet när du klickar på hello Panorama9 panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

