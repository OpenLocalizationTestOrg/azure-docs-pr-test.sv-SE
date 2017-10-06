---
title: "Självstudier: Azure Active Directory-integrering med UNIFI | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UNIFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af7cc1167b5c0cff2a1f4cdaa8a2b93f5a718f81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Självstudier: Azure Active Directory-integrering med UNIFI

I kursen får du lära dig hur toointegrate UNIFI med Azure Active Directory (AD Azure).

Integrera UNIFI med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooUNIFI
- Du kan aktivera din användare tooautomatically get inloggade tooUNIFI (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med UNIFI, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En UNIFI enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till UNIFI från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-unifi-from-hello-gallery"></a>Att lägga till UNIFI från hello-galleriet
tooconfigure hello integrering av UNIFI i Azure AD, behöver du tooadd UNIFI hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd UNIFI från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **UNIFI**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. Markera hello resultat på panelen **UNIFI**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UNIFI baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UNIFI är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UNIFI toobe upprättas.

I UNIFI, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med UNIFI, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en UNIFI testanvändare](#creating-a-unifi-test-user)**  -toohave en motsvarighet för Britta Simon i UNIFI som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UNIFI program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UNIFI:**

1. I hello Azure-portalen på hello **UNIFI** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. På hello **UNIFI domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    I hello **identifierare** textruta hello TYPVÄRDE:`INVIEWlabs` 

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    I hello **inloggnings-URL** textruta typen hello URL:`https://app.discoverunifi.com/login`

5. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. På hello **UNIFI Configuration** klickar du på **konfigurera UNIFI** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. Inloggning i en annan webbläsarfönster tooyour **UNIFI** företagets webbplats som administratör.

9. Klicka på hello **användare**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. Klicka på hello **lägga till nya identitetsleverantör**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app2.png)

11. I hello **lägga till identitetsleverantör** avsnittet, utföra hello följande steg:   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app3.png) 

    a. I hello **providernamn** textruta hello-typnamn för hello identitetsleverantör...

    b. I hello hello **providern URL** textruta klistra in hello **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen.

    c. Öppna hello certifikat som du har hämtat från hello Azure-portalen i anteckningar, ta bort hello **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---** tagga och klistra in hello återstående innehåll i Hej **certifikat** textruta.

    d. Välj hello **är som standard Provider** kryssrutan.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-unifi-test-user"></a>Skapa en testanvändare UNIFI

I det här avsnittet skapar du en användare som kallas Britta Simon. **UNIFI** stöder automatisk användaretablering, så inga manuella steg krävs. Användare skapas automatiskt efter en lyckad autentisering från hello Azure AD.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUNIFI.

![Tilldela användare][200] 

**tooassign Britta Simon tooUNIFI utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **UNIFI**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour UNIFI programmet när du klickar på hello UNIFI panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

